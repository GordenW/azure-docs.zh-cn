---
title: 处理 Azure Blob 存储中的更改源（预览版）| Microsoft Docs
description: 了解如何在 .NET 客户端应用程序中处理更改源日志
author: normesta
ms.author: normesta
ms.date: 11/04/2019
ms.topic: article
ms.service: storage
ms.subservice: blobs
ms.reviewer: sadodd
ms.openlocfilehash: 75995eeb3f8255cb4c60d5be267f9c343edfea89
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "74111855"
---
# <a name="process-change-feed-in-azure-blob-storage-preview"></a>处理 Azure Blob 存储中的更改源（预览版）

更改源提供存储帐户中 Blob 和 Blob 元数据发生的所有更改的事务日志。 本文介绍如何使用 Blob 更改源处理器库读取更改源记录。

若要详细了解更改源，请参阅 [Azure Blob 存储中的更改源（预览版）](storage-blob-change-feed.md)。

> [!NOTE]
> 更改源以公共预览版提供，并在**westcentralus**和**westus2**区域中提供。 若要了解有关此功能以及已知问题和限制的详细信息，请参阅[Azure Blob 存储中的更改源支持](storage-blob-change-feed.md)。 更改源处理器库将在现在和此库公开上市时进行更改。

## <a name="get-the-blob-change-feed-processor-library"></a>获取 Blob 更改源处理器库

1. 在 Visual Studio 中，将 URL `https://azuresdkartifacts.blob.core.windows.net/azuresdkpartnerdrops/index.json` 添加到 NuGet 包源。 

   若要了解如何操作，请参阅[包源](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-visual-studio#package-sources)。

2. 在 NuGet 包管理器中找到“Microsoft.Azure.Storage.Changefeed”包，并将其安装到项目。  

   若要了解如何操作，请参阅[查找和安装包](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-visual-studio#find-and-install-a-package)。

## <a name="connect-to-the-storage-account"></a>连接到存储帐户

调用 [CloudStorageAccount.TryParse](/dotnet/api/microsoft.azure.storage.cloudstorageaccount.tryparse) 方法来分析连接字符串。 

然后调用 [CloudStorageAccount.CreateCloudBlobClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.blob.blobaccountextensions.createcloudblobclient) 方法，在存储帐户中创建一个表示 Blob 存储的对象。

```cs
public bool GetBlobClient(ref CloudBlobClient cloudBlobClient, string storageConnectionString)
{
    if (CloudStorageAccount.TryParse
        (storageConnectionString, out CloudStorageAccount storageAccount))
        {
            cloudBlobClient = storageAccount.CreateCloudBlobClient();

            return true;
        }
        else
        {
            return false;
        }
    }
}
```

## <a name="initialize-the-change-feed"></a>初始化更改源

将以下 using 语句添加到代码文件的顶部。 

```csharp
using Avro.Generic;
using ChangeFeedClient;
```

然后，调用 GetContainerReference 方法创建 ChangeFeed 类的实例。   传入更改源容器的名称。

```csharp
public async Task<ChangeFeed> GetChangeFeed(CloudBlobClient cloudBlobClient)
{
    CloudBlobContainer changeFeedContainer =
        cloudBlobClient.GetContainerReference("$blobchangefeed");

    ChangeFeed changeFeed = new ChangeFeed(changeFeedContainer);
    await changeFeed.InitializeAsync();

    return changeFeed;
}
```

## <a name="reading-records"></a>读取记录

> [!NOTE]
> 更改源是存储帐户中的不可变只读实体。 任意数量的应用程序都可以同时独立读取和处理更改源，具体取决于应用程序自身的需求。 当应用程序读取记录时，不会从更改源中删除这些记录。 每个使用方读取器的读取或迭代状态是独立的，仅由应用程序维护。

读取记录的最简单方法是创建 ChangeFeedReader 类的实例。  

此示例将循环访问更改源中的所有记录，然后在控制台上输出每条记录中的几个值。 
 
```csharp
public async Task ProcessRecords(ChangeFeed changeFeed)
{
    ChangeFeedReader processor = await changeFeed.CreateChangeFeedReaderAsync();

    ChangeFeedRecord currentRecord = null;
    do
    {
        currentRecord = await processor.GetNextItemAsync();

        if (currentRecord != null)
        {
            string subject = currentRecord.record["subject"].ToString();
            string eventType = ((GenericEnum)currentRecord.record["eventType"]).Value;
            string api = ((GenericEnum)((GenericRecord)currentRecord.record["data"])["api"]).Value;

            Console.WriteLine("Subject: " + subject + "\n" +
                "Event Type: " + eventType + "\n" +
                "Api: " + api);
        }

    } while (currentRecord != null);
}
```

## <a name="resuming-reading-records-from-a-saved-position"></a>从保存的位置继续读取记录

可以选择在更改源中保存读取位置，将来可以继续迭代记录。 可以使用 ChangeFeedReader.SerializeState() 方法随时保存更改源的迭代状态。  该状态是一个字符串，应用程序可以根据自身的设计保存该状态（例如，保存到数据库或文件中）。 

```csharp
    string currentReadState = processor.SerializeState();
```

可以通过使用 CreateChangeFeedReaderFromPointerAsync 方法创建 ChangeFeedReader，继续从最后一个状态循环访问记录。  

```csharp
public async Task ProcessRecordsFromLastPosition(ChangeFeed changeFeed, string lastReadState)
{
    ChangeFeedReader processor = await changeFeed.CreateChangeFeedReaderFromPointerAsync(lastReadState);

    ChangeFeedRecord currentRecord = null;
    do
    {
        currentRecord = await processor.GetNextItemAsync();

        if (currentRecord != null)
        {
            string subject = currentRecord.record["subject"].ToString();
            string eventType = ((GenericEnum)currentRecord.record["eventType"]).Value;
            string api = ((GenericEnum)((GenericRecord)currentRecord.record["data"])["api"]).Value;

            Console.WriteLine("Subject: " + subject + "\n" +
                "Event Type: " + eventType + "\n" +
                "Api: " + api);
        }

    } while (currentRecord != null);
}

```

## <a name="stream-processing-of-records"></a>记录的流式处理

可以选择在更改源记录到达时对其进行处理。 请参阅[规范](storage-blob-change-feed.md#specifications)。

```csharp
public async Task ProcessRecordsStream(ChangeFeed changeFeed, int waitTimeMs)
{
    ChangeFeedReader processor = await changeFeed.CreateChangeFeedReaderAsync();

    ChangeFeedRecord currentRecord = null;
    while (true)
    {
        do
        {
            currentRecord = await processor.GetNextItemAsync();

            if (currentRecord != null)
            {
                string subject = currentRecord.record["subject"].ToString();
                string eventType = ((GenericEnum)currentRecord.record["eventType"]).Value;
                string api = ((GenericEnum)((GenericRecord)currentRecord.record["data"])["api"]).Value;

                Console.WriteLine("Subject: " + subject + "\n" +
                    "Event Type: " + eventType + "\n" +
                    "Api: " + api);
            }

        } while (currentRecord != null);

        await Task.Delay(waitTimeMs);
    }
}
```

## <a name="reading-records-within-a-time-range"></a>读取时间范围内的记录

更改源根据更改事件时间组织到每小时的段中。 请参阅[规范](storage-blob-change-feed.md#specifications)。 可以从更改源段中读取处于特定时间范围内的记录。

此示例获取所有段的开始时间。 然后，它将循环访问该列表，直到开始时间超出了最后一个可使用段的时间，或超出了所需范围的结束时间。 

### <a name="selecting-segments-for-a-time-range"></a>根据时间范围选择段

```csharp
public async Task<List<DateTimeOffset>> GetChangeFeedSegmentRefsForTimeRange
    (ChangeFeed changeFeed, DateTimeOffset startTime, DateTimeOffset endTime)
{
    List<DateTimeOffset> result = new List<DateTimeOffset>();

    DateTimeOffset stAdj = startTime.AddMinutes(-15);
    DateTimeOffset enAdj = endTime.AddMinutes(15);

    DateTimeOffset lastConsumable = (DateTimeOffset)changeFeed.LastConsumable;

    List<DateTimeOffset> segments = 
        (await changeFeed.ListAvailableSegmentTimesAsync()).ToList();

    foreach (var segmentStart in segments)
    {
        if (lastConsumable.CompareTo(segmentStart) < 0)
        {
            break;
        }

        if (enAdj.CompareTo(segmentStart) < 0)
        {
            break;
        }

        DateTimeOffset segmentEnd = segmentStart.AddMinutes(60);

        bool overlaps = stAdj.CompareTo(segmentEnd) < 0 && 
            segmentStart.CompareTo(enAdj) < 0;

        if (overlaps)
        {
            result.Add(segmentStart);
        }
    }

    return result;
}
```

### <a name="reading-records-in-a-segment"></a>读取段中的记录

可以读取单个段或段范围内的记录。

```csharp
public async Task ProcessRecordsInSegment(ChangeFeed changeFeed, DateTimeOffset segmentOffset)
{
    ChangeFeedSegment segment = new ChangeFeedSegment(segmentOffset, changeFeed);
    await segment.InitializeAsync();

    ChangeFeedSegmentReader processor = await segment.CreateChangeFeedSegmentReaderAsync();

    ChangeFeedRecord currentRecord = null;
    do
    {
        currentRecord = await processor.GetNextItemAsync();

        if (currentRecord != null)
        {
            string subject = currentRecord.record["subject"].ToString();
            string eventType = ((GenericEnum)currentRecord.record["eventType"]).Value;
            string api = ((GenericEnum)((GenericRecord)currentRecord.record["data"])["api"]).Value;

            Console.WriteLine("Subject: " + subject + "\n" +
                "Event Type: " + eventType + "\n" +
                "Api: " + api);
        }

    } while (currentRecord != null);
}
```

## <a name="read-records-starting-from-a-time"></a>读取从某个时间开始的记录

可以从起始段到末尾读取更改源的记录。 与读取某个时间范围内的记录类似，可以列出段，并选择要从其开始迭代的段。

此示例获取第一个要处理的段的 [DateTimeOffset](https://docs.microsoft.com/dotnet/api/system.datetimeoffset?view=netframework-4.8)。

```csharp
public async Task<DateTimeOffset> GetChangeFeedSegmentRefAfterTime
    (ChangeFeed changeFeed, DateTimeOffset timestamp)
{
    DateTimeOffset result = new DateTimeOffset();

    DateTimeOffset lastConsumable = (DateTimeOffset)changeFeed.LastConsumable;
    DateTimeOffset lastConsumableEnd = lastConsumable.AddMinutes(60);

    DateTimeOffset timestampAdj = timestamp.AddMinutes(-15);

    if (lastConsumableEnd.CompareTo(timestampAdj) < 0)
    {
        return result;
    }

    List<DateTimeOffset> segments = (await changeFeed.ListAvailableSegmentTimesAsync()).ToList();
    foreach (var segmentStart in segments)
    {
        DateTimeOffset segmentEnd = segmentStart.AddMinutes(60);
        if (timestampAdj.CompareTo(segmentEnd) <= 0)
        {
            result = segmentStart;
            break;
        }
    }

    return result;
}
```

此示例从起始段的 [DateTimeOffset](https://docs.microsoft.com/dotnet/api/system.datetimeoffset?view=netframework-4.8) 开始处理更改源记录。

```csharp
public async Task ProcessRecordsStartingFromSegment(ChangeFeed changeFeed, DateTimeOffset segmentStart)
{
    TimeSpan waitTime = new TimeSpan(60 * 1000);

    ChangeFeedSegment segment = new ChangeFeedSegment(segmentStart, changeFeed);

    await segment.InitializeAsync();

    while (true)
    {
        while (!await IsSegmentConsumableAsync(changeFeed, segment))
        {
            await Task.Delay(waitTime);
        }

        ChangeFeedSegmentReader reader = await segment.CreateChangeFeedSegmentReaderAsync();

        do
        {
            await reader.CheckForFinalizationAsync();

            ChangeFeedRecord currentItem = null;
            do
            {
                currentItem = await reader.GetNextItemAsync();
                if (currentItem != null)
                {
                    string subject = currentItem.record["subject"].ToString();
                    string eventType = ((GenericEnum)currentItem.record["eventType"]).Value;
                    string api = ((GenericEnum)((GenericRecord)currentItem.record["data"])["api"]).Value;

                    Console.WriteLine("Subject: " + subject + "\n" +
                        "Event Type: " + eventType + "\n" +
                        "Api: " + api);
                }
            } while (currentItem != null);

            if (segment.timeWindowStatus != ChangefeedSegmentStatus.Finalized)
            {
                await Task.Delay(waitTime);
            }
        } while (segment.timeWindowStatus != ChangefeedSegmentStatus.Finalized);

        segment = await segment.GetNextSegmentAsync(); // TODO: What if next window doesn't yet exist?
        await segment.InitializeAsync(); // Should update status, shard list.
    }
}

private async Task<bool> IsSegmentConsumableAsync(ChangeFeed changeFeed, ChangeFeedSegment segment)
{
    if (changeFeed.LastConsumable >= segment.startTime)
    {
        return true;
    }
    await changeFeed.InitializeAsync();
    return changeFeed.LastConsumable >= segment.startTime;
}
```

>[!TIP]
> 段可以在一个或多个 chunkFilePath 中包含更改源日志。  如果存在多个 chunkFilePath，则系统已在内部将记录分区成多个分片，以管理发布吞吐量。  系统会确保段的每个分区包含互斥 Blob 的更改并且可以单独处理，而不妨碍排序。 可以使用 ChangeFeedSegmentShardReader 类在分片级别循环访问记录（如果这在你的方案中是最有效的做法）。 

## <a name="next-steps"></a>后续步骤

详细了解更改源日志。 请参阅 [Azure Blob 存储中的更改源（预览版）](storage-blob-change-feed.md)
