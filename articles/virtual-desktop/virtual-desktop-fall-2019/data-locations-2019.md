---
title: Windows 虚拟桌面的数据位置（经典）-Azure
description: Windows 虚拟桌面（经典）数据和元数据的存储位置的简要概述。
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 0e12e17914374a64ecd2d17a8e73c6ebf20328d9
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2020
ms.locfileid: "87270033"
---
# <a name="data-locations-for-windows-virtual-desktop-classic"></a>Windows 虚拟桌面的数据位置（经典）

>[!IMPORTANT]
>此内容适用于不支持 Azure 资源管理器 Windows 虚拟桌面对象的 Windows 虚拟桌面（经典）。 如果尝试管理 Azure 资源管理器 Windows 虚拟桌面对象，请参阅[此文](../data-locations.md)。

Windows 虚拟桌面目前可用于所有地理位置。 最初，只能将服务元数据存储在美国（US）地理位置。 管理员可以在创建主机池虚拟机和相关服务（如文件服务器）时选择存储用户数据的位置。 在[azure 数据中心地图](https://azuredatacentermap.azurewebsites.net/)上详细了解 azure 地理位置。

>[!NOTE]
>Microsoft 不会控制或限制你或你的用户可以访问你的用户和特定于应用程序的数据的区域。

>[!IMPORTANT]
>Windows 虚拟桌面将全局元数据信息存储在美国中的数据中心，例如租户名称、主机池名称、应用组名称和用户主体名称。 存储的元数据是静态加密的，并且异地冗余镜像在美国中进行维护。 所有客户数据（如应用设置和用户数据）驻留在客户选择的位置，并且不受服务管理。

出于灾难恢复目的，在美国中复制服务元数据。