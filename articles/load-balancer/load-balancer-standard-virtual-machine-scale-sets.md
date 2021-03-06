---
title: Azure 标准负载均衡器和虚拟机规模集
titleSuffix: Azure Standard Load Balancer and Virtual Machine Scale Sets
description: 了解此学习途径后，开始 Azure 标准负载均衡器和虚拟机规模集。
services: load-balancer
documentationcenter: na
author: irenehua
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/17/2020
ms.author: irenehua
ms.openlocfilehash: f5a8453f84854108facb4da4616a7d362e0fb38c
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86531097"
---
# <a name="azure-load-balancer-with-azure-virtual-machine-scale-sets"></a>Azure 负载均衡器与 Azure 虚拟机规模集

使用虚拟机规模集和负载均衡器时，应考虑以下准则：

## <a name="multiple-virtual-machine-scale-sets-cant-use-the-same-load-balancer"></a>多个虚拟机规模集不能使用相同的负载均衡器
## <a name="port-forwarding-and-inbound-nat-rules"></a>端口转发和入站 NAT 规则：
  * 创建规模集后，无法为负载均衡器的运行状况探测所用的负载均衡规则修改后端端口。 为了更改端口，可以通过更新 Azure 虚拟机规模集来删除运行状况探测，更新端口，然后重新配置运行状况探测。
  * 使用负载均衡器后端池中的虚拟机规模集时，将自动创建默认的入站 NAT 规则。
## <a name="inbound-nat-pool"></a>入站 NAT 池：
  * 每个虚拟机规模集必须至少有一个入站 NAT 池。 
  * 入站 NAT 池是入站 NAT 规则的集合。 一个入站 NAT 池不能支持多个虚拟机规模集。
  
## <a name="load-balancing-rules"></a>负载均衡规则：
  * 使用负载均衡器后端池中的虚拟机规模集时，将自动创建默认负载均衡规则。
## <a name="outbound-rules"></a>出站规则：
  *  若要为已被负载平衡规则引用的后端池创建出站规则，则在创建入站负载均衡规则时，需要先将 **"创建隐式出站规则"** 标记为门户中的 "**否**"。

  :::image type="content" source="./media/vm-scale-sets/load-balancer-and-vm-scale-sets.png" alt-text="创建负载均衡规则" border="true":::

可以使用以下方法部署一个包含现有 Azure 负载均衡器的虚拟机规模集。

* [使用 Azure 门户配置包含现有 Azure 负载均衡器的虚拟机规模集](https://docs.microsoft.com/azure/load-balancer/configure-vm-scale-set-portal)。
* [使用 Azure PowerShell 配置包含现有 Azure 负载均衡器的虚拟机规模集](https://docs.microsoft.com/azure/load-balancer/configure-vm-scale-set-powershell)。
* [使用 Azure CLI 配置包含现有 Azure 负载均衡器的虚拟机规模集](https://docs.microsoft.com/azure/load-balancer/configure-vm-scale-set-cli)。
