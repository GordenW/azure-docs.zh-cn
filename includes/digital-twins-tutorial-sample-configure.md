---
author: baanders
description: Azure 数字孪生教程的包含文件 - 配置示例项目
ms.service: digital-twins
ms.topic: include
ms.date: 5/25/2020
ms.author: baanders
ms.openlocfilehash: ad9eb95c9e3a21cd33a367cc864a4d791888afb9
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/24/2020
ms.locfileid: "87124540"
---
## <a name="configure-the-sample-project"></a>配置示例项目

接下来，设置将与 Azure 数字孪生实例交互的示例客户端应用程序。 如果尚未下载示例项目，请选择标题下面的“下载 ZIP”按钮，立即从 [Azure 数字孪生示例](https://docs.microsoft.com/samples/azure-samples/digital-twins-samples/digital-twins-samples)登陆页面中获取。

导航到已在计算机上下载的文件，然后将其解压缩。

打开解压缩的文件夹后，导航到 AdtSampleApp/。 在 Visual Studio 2019 中打开 AdtE2ESample.sln。 

在 Visual Studio 中，使用“解决方案资源管理器”窗格创建 SampleClientApp > serviceConfig.json.TEMPLATE 文件的副本（可使用右键菜单进行复制和粘贴）。 重命名副本 serviceConfig.json。 该副本将充当预设置的 JSON 文件，其中包含运行项目所需的配置变量。

选择 serviceConfig.json 文件，在编辑窗口中将其打开。 将 `tenantId` 更改为目录 ID，将 `clientId` 更改为应用程序 ID，并将 `instanceUrl` 更改为 Azure 数字孪生实例 hostName URL（以 https:// 开头，如下所示）   。

```json
{
  "tenantId": "<your-directory-ID>",
  "clientId": "<your-application-ID>",
  "instanceUrl": "https://<your-Azure-Digital-Twins-instance-hostName>"
}
```

保存并关闭该文件。 

接下来，配置 serviceConfig 文件，在生成 SampleClientApp 时将其复制到输出目录 。 为此，请右键单击 serviceConfig.json 文件，然后选择“属性” 。 在“属性”检查器中，将“复制到输出目录”属性的值更改为“如果较新则复制”  。

:::image type="content" source="../articles/digital-twins/media/includes/copy-config.png" alt-text="Visual Studio 窗口的摘录，其中显示了“解决方案资源管理器”窗格（该窗格中突出显示了 serviceConfig.json）和“属性”窗格（该窗格中将“复制到输出目录”属性设置为“如果较新则复制”）" border="false":::

在 Visual Studio 中将 AdtE2ESample 项目保持在打开状态，以便在本教程中继续使用该项目。

