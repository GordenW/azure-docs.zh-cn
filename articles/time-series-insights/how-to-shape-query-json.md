---
title: 塑造 JSON 的最佳做法 - Azure 时序见解查询 | Microsoft Docs
description: 了解如何通过塑造 JSON 提高 Azure 时序见解查询的效率。
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: diviso
ms.service: time-series-insights
ms.topic: article
ms.date: 06/30/2020
ms.custom: seodec18
ms.openlocfilehash: cc24c1f49a48e81509961d5d7d01dba60dc50475
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/23/2020
ms.locfileid: "87077656"
---
# <a name="shape-json-to-maximize-query-performance-in-your-gen1-environment"></a>在 Gen1 环境中最大化查询性能的形状 JSON

本文提供有关如何塑造 JSON 以最大化 Azure 时序见解查询效率的指导。

## <a name="video"></a>视频

### <a name="learn-best-practices-for-shaping-json-to-meet-your-storage-needsbr"></a>了解整形 JSON 以满足你的存储需求的最佳实践。</br>

> [!VIDEO https://www.youtube.com/embed/b2BD5hwbg5I]

## <a name="best-practices"></a>最佳实践

考虑如何将事件发送到 Azure 时序见解。 即，始终：

1. 尽量高效地通过网络发送数据。
1. 确保数据的存储方式可让你执行适合方案的聚合。
1. 请确保不会达到 Azure 时序见解的最大属性限制：
   - 对于 S1 环境，属性（列）数目不能超过 600 个。
   - 对于 S2 环境，属性（列）数目不能超过 800 个。

以下指导可帮助实现最佳查询性能：

1. 不要使用动态属性（例如标记 ID）作为属性名称。 这种用法会导致达到最大属性限制。
1. 不要发送不必要的属性。 如果不需要查询属性，最好不要发送它。 这可以避免达到存储限制。
1. 使用[引用数据](time-series-insights-add-reference-data-set.md)来避免通过网络发送静态数据。
1. 在多个事件之间共享维度属性，以更有效地通过网络发送数据。
1. 不要使用深层数组嵌套。 Azure 时序见解最多支持两个包含对象的嵌套数组。 Azure 时序见解将消息中的数组平展为多个具有属性值对的事件。
1. 如果所有或大多数事件只存在几个度量，最好是在同一个对象中将这些度量作为单独的属性发送。 单独发送度量可以减少事件数目，并可能会提高查询的效率，因为要处理的事件更少。 如果存在多个度量，在单个属性中将它们作为值发送可以最大程度地减少达到最大属性数限制的可能性。

## <a name="example-overview"></a>示例概述

以下两个示例演示如何发送事件，并强调前面所述的建议。 在每个示例后，你可以查看建议是如何应用的。

这些示例基于多个设备发送度量值或信号的场景。 度量值或信号可能包括“流速”、“引擎油压”、“温度”和“湿度”。 在第一个示例中，所有设备提供了一些度量值。 第二个示例涉及许多的设备，每个设备发送多个唯一的度量值。

## <a name="scenario-one-only-a-few-measurements-exist"></a>方案一：仅存在少数度量值

> [!TIP]
> 我们建议将每个度量值或信号作为单独的属性或列发送。

以下示例涉及到单个 Azure IoT 中心消息，其中的外部数组包含通用维度值的共享节。 该外部数组使用参考数据来提高消息的效率。 参考数据包含设备元数据，这些数据不会根据每个事件变化，但提供有用的属性用于数据分析。 批处理通用维度值和采用参考数据都能减少通过网络发送的字节数，这可以提高消息的效率。

使用在发送到 Azure 云时序列化为 JSON 的[IoT 设备消息对象](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.message?view=azure-dotnet)，将以下 JSON 有效负载发送到 Azure 时序见解 GA 环境：


```JSON
[
    {
        "deviceId": "FXXX",
        "timestamp": "2018-01-17T01:17:00Z",
        "series": [
            {
                "Flow Rate ft3/s": 1.0172575712203979,
                "Engine Oil Pressure psi ": 34.7
            },
            {
                "Flow Rate ft3/s": 2.445906400680542,
                "Engine Oil Pressure psi ": 49.2
            }
        ]
    },
    {
        "deviceId": "FYYY",
        "timestamp": "2018-01-17T01:18:00Z",
        "series": [
            {
                "Flow Rate ft3/s": 0.58015072345733643,
                "Engine Oil Pressure psi ": 22.2
            }
        ]
    }
]
```

* 包含键属性 **deviceId** 的参考数据表：

   | deviceId | messageId | deviceLocation |
   | --- | --- | --- |
   | FXXX | LINE\_DATA | EU |
   | FYYY | LINE\_DATA | 美国 |

* 平展后的 Azure 时序见解事件表：

   | deviceId | messageId | deviceLocation | timestamp | series.Flow Rate ft3/s | series.Engine Oil Pressure psi |
   | --- | --- | --- | --- | --- | --- |
   | FXXX | LINE\_DATA | EU | 2018-01-17T01:17:00Z | 1.0172575712203979 | 34.7 |
   | FXXX | LINE\_DATA | EU | 2018-01-17T01:17:00Z | 2.445906400680542 | 49.2 |
   | FYYY | LINE\_DATA | 美国 | 2018-01-17T01:18:00Z | 0.58015072345733643 | 22.2 |

> [!NOTE]
> - **deviceId** 列充当机群中各种设备的列标题。 对于其他五个列，使 **deviceId** 值成为其自身的属性名称会将设备总数限制为 595（对于 S1 环境）或 795（对于 S2 环境）。
> - 避免不必要的属性（例如制造商和型号信息）。 由于将来不会查询这些属性，消除它们对网络和存储效率有利。
> - 参考数据用于减少通过网络传输的字节数。 已使用键属性 **deviceId** 联接 **messageId** 和 **deviceLocation** 这两个特性。 此数据将在传入时与遥测数据联接，然后存储在 Azure 时序见解中以供查询。
> - 使用两层嵌套，这是 Azure 时序见解支持的最大嵌套量。 必须避免深层嵌套的数组。
> - 由于度量很少，因此将其作为单独的属性在同一对象中发送。 此处的 **series.Flow Rate psi** 和 **series.Engine Oil Pressure ft3/s** 是唯一的列。

## <a name="scenario-two-several-measures-exist"></a>方案二：存在多个度量值

> [!TIP]
> 我们建议将度量值作为“type”、“unit”、“value”元组发送。

示例 JSON 有效负载：

```JSON
[
    {
        "deviceId": "FXXX",
        "timestamp": "2018-01-17T01:17:00Z",
        "series": [
            {
                "tagId": "pumpRate",
                "value": 1.0172575712203979
            },
            {
                "tagId": "oilPressure",
                "value": 49.2
            },
            {
                "tagId": "pumpRate",
                "value": 2.445906400680542
            },
            {
                "tagId": "oilPressure",
                "value": 34.7
            }
        ]
    },
    {
        "deviceId": "FYYY",
        "timestamp": "2018-01-17T01:18:00Z",
        "series": [
            {
                "tagId": "pumpRate",
                "value": 0.58015072345733643
            },
            {
                "tagId": "oilPressure",
                "value": 22.2
            }
        ]
    }
]
```

* 包含键属性 **deviceId** 和 **series.tagId** 的参考数据表：

   | deviceId | series.tagId | messageId | deviceLocation | 类型 | unit |
   | --- | --- | --- | --- | --- | --- |
   | FXXX | pumpRate | LINE\_DATA | EU | 流速 | ft3/s |
   | FXXX | oilPressure | LINE\_DATA | EU | 引擎油压 | psi |
   | FYYY | pumpRate | LINE\_DATA | 美国 | 流速 | ft3/s |
   | FYYY | oilPressure | LINE\_DATA | 美国 | 引擎油压 | psi |

* 平展后的 Azure 时序见解事件表：

   | deviceId | series.tagId | messageId | deviceLocation | 类型 | unit | timestamp | series.value |
   | --- | --- | --- | --- | --- | --- | --- | --- |
   | FXXX | pumpRate | LINE\_DATA | EU | 流速 | ft3/s | 2018-01-17T01:17:00Z | 1.0172575712203979 | 
   | FXXX | oilPressure | LINE\_DATA | EU | 引擎油压 | psi | 2018-01-17T01:17:00Z | 34.7 |
   | FXXX | pumpRate | LINE\_DATA | EU | 流速 | ft3/s | 2018-01-17T01:17:00Z | 2.445906400680542 | 
   | FXXX | oilPressure | LINE\_DATA | EU | 引擎油压 | psi | 2018-01-17T01:17:00Z | 49.2 |
   | FYYY | pumpRate | LINE\_DATA | 美国 | 流速 | ft3/s | 2018-01-17T01:18:00Z | 0.58015072345733643 |
   | FYYY | oilPressure | LINE\_DATA | 美国 | 引擎油压 | psi | 2018-01-17T01:18:00Z | 22.2 |

> [!NOTE]
> - 列 **deviceId** 和 **series.tagId** 充当机群中各个设备和标记的列标题。 对于其他六个列，将每个值用作其自身的特性会将设备总数查询限制为 594（对于 S1 环境）或 794（对于 S2 环境）。
> - 出于第一个示例中提到的原因，请避免不必要的属性。
> - 参考数据用于减少通过网络传输的字节数，因为针对 **messageId** 和 **deviceLocation** 的唯一对引入了 **deviceId**。 针对 **type** 和 **unit** 的唯一对使用了组合键 **series.tagId**。 组合键允许使用**deviceId**和**tagId**对来表示四个值： **messageId、msds-devicelocation、type**和**unit**。 在流入时，此数据将联接到遥测数据。 然后，将其存储在 Azure 时序见解中用于查询。
> - 出于第一个示例中提到的原因，使用了两个嵌套层。

### <a name="for-both-scenarios"></a>对于两种场景

对于包含大量可能值的属性，最好是在单个列中将这些值作为非重复值发送，而不要为每个值创建一个新列。 对于前面的两个示例：

  - 在第一个示例中，少量的属性包含多个值，因此，适合将每个属性设为单独的属性。
  - 在第二个示例中，度量未指定为单独的属性， 而是通用系列属性下的值或度量数组。 发送了新键 **tagId**，该键在平展表中创建新列 **series.tagId**。 新属性 **type** 和 **unit** 是使用参考数据创建的，因此不会达到属性限制。

## <a name="next-steps"></a>后续步骤

- 阅读有关[将 IoT 中心设备消息发送到云](../iot-hub/iot-hub-devguide-messages-construct.md)的详细信息。

- 有关详细信息，请参阅[azure 时序见解查询语法](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-syntax)，详细了解 Azure 时序见解数据访问 REST API 的查询语法。

- 了解[如何调整事件](./time-series-insights-send-events.md)。
