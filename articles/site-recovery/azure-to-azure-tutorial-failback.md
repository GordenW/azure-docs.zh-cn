---
title: 使用 Azure Site Recovery 服务将 Azure VM 故障回复到主区域。
description: 介绍如何使用 Azure Site Recovery 服务将 Azure VM 故障回复到主要区域。
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 11/14/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: f73d20c19e8fc26c553490772f5374e8a88a77b2
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2020
ms.locfileid: "87289297"
---
# <a name="fail-back-an-azure-vm-between-azure-regions"></a>在 Azure 区域之间对 Azure VM 进行故障回复

[Azure Site Recovery](site-recovery-overview.md) 服务可管理和协调本地计算机和 Azure 虚拟机 (VM) 的复制、故障转移和故障回复，因而有利于灾难恢复策略。

本教程介绍如何故障回复单个 Azure VM。 故障转移后，须在主要区域可用时故障回复到主要区域。 在本教程中，你将了解如何执行以下操作：

> [!div class="checklist"]
> 
> * 对次要区域中的 VM 进行故障回复。
> * 重新保护主 VM，以便它复制回次要区域。
> 
> [!NOTE]
> 
> 本教程可帮助你在只需进行极少量的自定义操作的情况下，将多个 VM 故障转移到目标区域，然后故障回复到源区域。 有关更深入的说明，请查看 [Azure VM 的操作指南](../virtual-machines/windows/index.yml)。

## <a name="before-you-start"></a>开始之前

* 确保 VM 的状态为“故障转移已提交”  。
* 检查主要区域是否可用，并且你是否能够在其中创建和访问新资源。
* 请确保启用重新保护。

## <a name="fail-back-to-the-primary-region"></a>故障回复到主要区域

重新保护 VM 后，可根据需要故障回复到主要区域。

1. 在保管库中选择“复制的项”，然后选择已重新保护的 VM  。

    ![故障回复到主要区域](./media/site-recovery-azure-to-azure-failback/azure-to-azure-failback.png)

2. 在“复制的项”中选择 VM，然后选择“故障转移”   。
3. 在“故障转移”中，选择要故障转移到的恢复点  ：
    - **最新(默认设置)** ：处理 Site Recovery 服务中的所有数据，并提供最低的恢复点目标 (RPO)。
    - **最新处理**：将 VM 还原到由 Site Recovery 处理过的最新恢复点。
    - **自定义**：故障转移到特定的恢复点。 此选项可用于执行测试故障转移。
4. 如果希望 Site Recovery 在触发故障转移之前在 DR 区域尝试关闭 VM，请选择“在开始故障转移前关闭计算机”  。 即使关机失败，故障转移也仍会继续。 
5. 在“作业”页上跟踪故障转移进度。
6. 故障转移完成后，请登录到 VM 来对它进行验证。 可根据需要更改恢复点。
7. 验证故障转移后，选择“提交故障转移”  。 提交操作会删除所有可用的恢复点。 “更改恢复点”选项不再可用。
8. VM 应显示为已故障转移并已故障回复。

    ![主要和次要区域的 VM](./media/site-recovery-azure-to-azure-failback/azure-to-azure-failback-vm-view.png)

> [!NOTE]
> 对于使用托管磁盘以及运行 Site Recovery 扩展版本 9.28.x.x 及更高版本[更新汇总 40](https://support.microsoft.com/help/4521530/update-rollup-40-for-azure-site-recovery) 的计算机，在故障回复完成并重新保护 VM 后，Site Recovery 将清理次要灾难恢复区域中的计算机。 无需手动删除次要区域中的 VM 和 NIC。 请注意，不会清理使用非托管磁盘的 VM。 如果在故障回复后完全禁用复制，则除了 VM 和 NIC 之外，Site Recovery 还会清理灾难恢复区域中的磁盘。

## <a name="next-steps"></a>后续步骤

[详细了解](azure-to-azure-how-to-reprotect.md#what-happens-during-reprotection)重新保护工作流。
