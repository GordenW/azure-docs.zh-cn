---
title: Azure 导入/导出服务的要求 | Microsoft Docs
description: 了解 Azure 导入/导出服务的软件和硬件要求。
author: alkohli
services: storage
ms.service: storage
ms.topic: conceptual
ms.date: 08/12/2019
ms.author: alkohli
ms.subservice: common
ms.openlocfilehash: 7b7915e1b7779c4d0359b9ac190574c43a624cbe
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2020
ms.locfileid: "87276391"
---
# <a name="azure-importexport-system-requirements"></a>Azure 导入/导出系统要求

本文介绍了 Azure 导入/导出服务的重要要求。 建议在使用导入/导出服务之前仔细查看这些信息，并且在操作过程中根据需要重新参阅。

## <a name="supported-operating-systems"></a>支持的操作系统

为了使用 WAImportExport 工具准备硬盘驱动器，以下**支持 BitLocker 驱动器加密的 64 位操作系统**受支持。


|平台 |版本 |
|---------|---------|
|Windows     | Windows 7 企业版、Windows 7 旗舰版 <br> Windows 8 专业版、Windows 8 企业版、Windows 8.1 专业版、Windows 8.1 企业版 <br> Windows 10        |
|Windows Server     |Windows Server 2008 R2 <br> Windows Server 2012、Windows Server 2012 R2         |

## <a name="other-required-software-for-windows-client"></a>Windows 客户端所需的其他软件

|平台 |版本 |
|---------|---------|
|.NET framework    | 4.5.1       |
| BitLocker        |  _          |


## <a name="supported-storage-accounts"></a>支持的存储帐户

Azure 导入/导出服务支持以下类型的存储帐户：

- 标准常规用途 v2 存储帐户（建议用于大多数方案）
- Blob 存储帐户
- 常规用途 v1 存储帐户（经典部署或 Azure 资源管理器部署）

有关存储帐户的详细信息，请参阅 [Azure 存储帐户概述](storage-account-overview.md)。

每个作业只能用于将数据传入/传出一个存储帐户。 换言之，一个导入/导出作业不能跨多个存储帐户。 有关创建新存储帐户的信息，请参阅[如何创建存储帐户](storage-account-create.md)。

> [!IMPORTANT]
> 对于已启用[虚拟网络服务终结点](../../virtual-network/virtual-network-service-endpoints-overview.md)功能的存储帐户   ，请使用 "**允许受信任的 Microsoft 服务 ...** " 设置，[使导入/导出](/azure/storage/common/storage-network-security)服务能够执行从 Azure 导入/导出数据。

## <a name="supported-storage-types"></a>受支持的存储类型

下面列出的存储类型都受 Azure 导入/导出服务支持。


|作业  |存储服务 |支持  |不支持  |
|---------|---------|---------|---------|
|导入     |  Azure Blob 存储 <br><br> Azure 文件存储       | 支持块 Blob 和页 Blob <br><br> 支持文件          |
|导出     |   Azure Blob 存储       | 支持块 Blob、页 Blob 和追加 Blob         | 不支持 Azure 文件


## <a name="supported-hardware"></a>支持的硬件

对于 Azure 导入/导出服务，需要使用支持的磁盘复制数据。

### <a name="supported-disks"></a>支持的磁盘

下面列出的存储类型均受支持，可与 Azure 导入/导出服务配合使用。


|磁盘类型  |大小  |支持 |
|---------|---------|---------|
|SSD    |   2.5"      |SATA III          |
|HDD     |  2.5"<br>3.5"       |SATA II、SATA III         |

不支持以下磁盘类型：

- USB。
- 带有内置 USB 适配器的外部 HDD。
- 外部 HDD 外壳内的磁盘。

单个导入/导出作业可以有：

- 最多 10 个 HDD/SSD。
- 任意大小的 HDD/SSD 的组合。

大量驱动器可在多个作业中分布，且对可创建的作业数量没有限制。 对于导入作业，仅处理驱动器上的第一个数据卷。 该数据卷必须使用 NTFS 进行格式化。

当使用 WAImportExport 工具准备硬盘驱动器并复制数据时，可以使用外部 USB 适配器。 大多数现成的 USB 3.0 或更高版本的适配器应可以工作。

## <a name="next-steps"></a>后续步骤

* [使用 AzCopy 命令行实用程序传输数据](storage-use-azcopy.md)
