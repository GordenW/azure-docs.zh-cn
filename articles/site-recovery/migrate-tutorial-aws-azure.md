---
title: 使用 Azure Site Recovery 服务将 AWS VM 迁移到 Azure | Microsoft Docs
description: 本文介绍如何使用 Azure Site Recovery 将 Amazon Web Services (AWS) 中运行的 Windows VM 迁移到 Azure。
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 07/27/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: dd91e99b45405cca10b9ddc2982674e72ad6bf86
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2020
ms.locfileid: "87281287"
---
# <a name="migrate-amazon-web-services-aws-vms-to-azure"></a>将 Amazon Web Services (AWS) VM 迁移到 Azure

本文介绍用于将 Amazon Web Services (AWS) 实例迁移到 Azure 的选项。

## <a name="migrate-with-azure-migrate"></a>使用 Azure Migrate 进行迁移

建议使用 [Azure Migrate](../migrate/migrate-services-overview.md) 服务将 AWS 实例迁移到 Azure。 Azure Migrate 提供一个集中式中心，用于使用 Azure Migrate、其他 Azure 服务和第三方工具评估本地计算机并将其迁移到 Azure。

[了解如何](../migrate/tutorial-migrate-aws-virtual-machines.md)使用 Azure Migrate 迁移 AWS 实例。 


## <a name="migrate-with-site-recovery"></a>使用 Site Recovery 进行迁移

Site Recovery 应仅用于灾难恢复，而不用于迁移。

如果已在使用 Azure Site Recovery，并且要继续使用它进行 AWS 迁移，请按照用于设置[物理计算机灾难恢复](physical-azure-disaster-recovery.md)的相同步骤进行操作。


> [!NOTE]
> 运行故障转移进行灾难恢复时，最后一步是提交故障转移。 迁移 AWS 实例时，“提交”选项不相关。 而是选择“完成迁移”选项。 

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> 查看有关 Azure Migrate 的[常见问题解答](../migrate/resources-faq.md)。
