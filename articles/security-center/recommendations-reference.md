---
title: 所有 Azure 安全中心建议的参考表
description: 本文列出了 Azure 安全中心的安全建议，这些建议可帮助保护你的资源。
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/05/2020
ms.author: memildin
ms.openlocfilehash: 9d03866858c9f8af521b27c5e36f882d9e0e177d
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/29/2020
ms.locfileid: "87404967"
---
# <a name="security-recommendations---a-reference-guide"></a>安全建议 - 参考指南

本文列出了 Azure 安全中心可能会显示的建议。 环境中显示的建议取决于要保护的资源和自定义的配置。

安全中心的建议基于最佳实践。 其中一些与基于公共符合性框架的安全性和符合性最佳做法的**Azure 安全性基准**和 Microsoft 创作的特定于 azure 的准则相一致。 [了解有关 Azure 安全性基准的详细信息](https://docs.microsoft.com/azure/security/benchmarks/introduction)。

若要了解如何响应这些建议，请参阅 [Azure 安全中心的修正建议](security-center-remediate-recommendations.md)。

安全分数基于已完成的安全中心建议的数量。 若要确定首先要解决的建议，请查看每个建议的严重级别，及其对安全分数的潜在影响。

>[!TIP]
> 如果建议的描述中显示“无相关策略”，通常是因为该建议依赖于另一个建议及其策略。 例如，建议“应修正 Endpoint Protection 运行状况失败...”依赖于建议“应安装 Endpoint Protection 解决方案...”，后者检查 Endpoint Protection 解决方案是否已安装。 基础建议*确实*具有一个策略。 将策略限制为仅限基础建议可简化策略管理。

## <a name="network-recommendations"></a><a name="recs-network"></a>网络建议

|建议|说明及相关策略|严重性|已启用快速修复？（[了解详细信息](https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations#recommendations-with-quick-fix-remediation)）|资源类型|
|----|----|----|----|----|
|**应在面向 Internet 的虚拟机上应用自适应网络强化建议**|当自适应网络强化功能发现了过于宽松的 NSG 规则时，标准定价层上的客户会看到此建议。<br>（相关策略：应在面向 Internet 的虚拟机上应用自适应网络强化建议）|高|N|虚拟机|
|应在与 VM 关联的 NSG 上限制所有网络端口|通过限制现有允许规则的访问来增强面向 Internet 的 VM 的网络安全组。<br>当向*所有*源开放任何端口（端口 22、3389、5985、5986、80 和 1443 除外）时，将触发此建议。<br>（相关策略：应该限制通过面向 Internet 的终结点进行访问）|高|N|虚拟机|
|**应启用 DDoS 防护标准版**|通过启用 DDoS 防护服务标准，保护包含具有公共 IP 的应用程序的虚拟网络。 DDoS 防护可缓解网络容量和协议攻击。<br>（相关策略：应启用 DDoS 防护标准）|高|N|虚拟网络|
|**应该只能通过 HTTPS 访问函数应用**|为函数应用启用“仅 HTTPS”访问权限。 使用 HTTPS 可确保执行服务器/服务身份验证服务，并保护传输中的数据不受网络层窃听攻击威胁。<br>（相关策略：应只能通过 HTTPS 访问函数应用）|中型|**是**|函数应用|
|**面向 Internet 的虚拟机应使用网络安全组进行保护**|启用网络安全组以控制虚拟机的网络访问。<br>（相关策略：面向 Internet 的虚拟机应使用网络安全组进行保护）|高/中|N|虚拟机|
|**应使用网络安全组来保护非面向 Internet 的虚拟机**|    通过使用网络安全组（NSG）限制对非面向 internet 的虚拟机进行限制，使其免受潜在威胁的侵害。<br>Nsg 包含访问控制列表（ACL），可以分配给 VM 的 NIC 或子网。 ACL 规则允许或拒绝到分配的资源的网络流量。<br>（相关策略：应通过网络安全组保护非面向 internet 的虚拟机）|低|N|虚拟机|
|应禁用虚拟机上的 IP 转发|禁用 IP 转发。 当在虚拟机的 NIC 上启用 IP 转发后，该计算机可接收发往其他目标的流量。 极少需要启用 IP 转发（例如，将 VM 用作网络虚拟设备时），因此，此策略应由网络安全团队评审。<br>（相关策略：[预览]：应禁用虚拟机上的 IP 转发）|中型|N|虚拟机|
|**应通过即时网络访问控制来保护虚拟机的管理端口**|应用恰时 (JIT) 虚拟机 (VM) 访问控制来永久锁定对所选端口的访问，并使经授权的用户能够通过 JIT 将其打开有限的时间量。<br>（相关策略：应通过即时网络访问控制来保护虚拟机的管理端口）|高|N|虚拟机|
|**应关闭虚拟机上的管理端口**|强化虚拟机的网络安全组，以限制对管理端口的访问。<br>（相关策略：应关闭虚拟机上的管理端口）|高|N|虚拟机|
|**应启用安全传输到存储帐户**|启用到存储帐户的安全传输。 安全传输选项会强制存储帐户仅接受来自安全连接 (HTTPS) 的请求。 使用 HTTPS 可确保服务器和服务之间的身份验证并保护传输中的数据免受中间人攻击、窃听和会话劫持等网络层攻击。<br>（相关策略：应启用到存储帐户的安全传输）|高|**是**|存储帐户|
|**子网应与网络安全组关联**|启用网络安全组以控制子网中部署的资源的网络访问。<br>（相关策略：子网应与网络安全组关联。<br>默认情况下禁用此策略）|高/中|N|子网|
|**只能通过 HTTPS 访问 Web 应用程序**|为 Web 应用启用“仅 HTTPS”访问权限。 使用 HTTPS 可确保执行服务器/服务身份验证服务，并保护传输中的数据不受网络层窃听攻击威胁。<br>（相关策略：应只能通过 HTTPS 访问 Web 应用程序）|中型|**是**|Web 应用程序|
||||||


## <a name="container-recommendations"></a><a name="recs-containers"></a>容器建议

|建议|说明及相关策略|严重性|已启用快速修复？（[了解详细信息](https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations#recommendations-with-quick-fix-remediation)）|资源类型|
|----|----|----|----|----|
|**应对 Azure Kubernetes 服务的群集启用高级威胁防护**|安全中心为容器化环境提供实时威胁防护，并针对可疑活动生成警报。 可以使用此信息快速补救安全问题，并提高容器的安全性。<br>重要提示：修正此建议将导致保护 AKS 群集。 如果此订阅中没有任何 AKS 群集，则不会产生任何费用。 如果以后在此订阅上创建任何 AKS 群集，这些群集将自动受到保护，并将在该时间开始收费。<br>（相关策略：[应在 Azure Kubernetes Service 群集上启用高级威胁防护](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f523b5cd1-3e23-492f-a539-13118b6d1e3a)）|高|**是**|订阅|
|**应在 Kubernetes 服务上定义经授权的 IP 范围**|通过仅向特定范围内的 IP 地址授予 API 访问权限，来限制对 Kubernetes 服务管理 API 的访问。 建议配置已获授权的 IP 范围，以便只有受允许网络中的应用程序可以访问群集。<br>（相关策略：[预览]：应在 Kubernetes 服务上定义经授权的 IP 范围）|高|N|计算资源（容器）|
|应定义 Pod 安全策略，通过删除不必要的应用程序特权来减少攻击途径。|通过删除不必要的应用程序特权，来定义 Pod 安全策略以减少攻击途径。 建议配置 pod 安全策略，以便 pod 只能访问其有权访问的资源。<br>（相关策略：[预览]：应在 Kubernetes 服务上定义 Pod 安全策略）|中型|N|计算资源（容器）|
|**应使用基于角色的访问控制来限制对 Kubernetes 服务群集的访问权限**|若要提供对用户可以执行的操作的粒度筛选，请使用基于角色的访问控制 (RBAC) 来管理 Kubernetes 服务群集中的权限并配置相关授权策略。 有关详细信息，请参阅 [Azure 基于角色的访问控制](https://docs.microsoft.com/azure/aks/concepts-identity#role-based-access-controls-rbac)。<br>（相关策略：[预览]：应在 Kubernetes 服务中使用基于角色的访问控制 (RBAC)）|中型|N|计算资源（容器）|
|**Kubernetes 服务应升级到最新的 Kubernetes 版本**|将 Azure Kubernetes Service 群集升级到最新的 Kubernetes 版本，以便从最新的漏洞修补程序中获益。 有关特定 Kubernetes 漏洞的详细信息，请参阅 [Kubernetes CVE](https://cve.mitre.org/cgi-bin/cvekey.cgi?keyword=kubernetes)。<br>（相关策略：[预览]：Kubernetes 服务应升级到不易受攻击的 Kubernetes 版本）|高|N|计算资源（容器）|
|**应对 Azure 容器注册表的注册表启用高级威胁防护**|若要构建安全的容器化工作负荷，请确保它们所基于的映像没有已知的漏洞。 安全中心会扫描注册表中每个推送的容器映像的安全漏洞，并显示每个映像的详细发现。<br>重要提示：修正此建议将导致为你的 ACR 注册表提供保护费用。 如果此订阅中没有任何 ACR 注册表，则不会产生任何费用。 如果以后在此订阅上创建任何 ACR 注册表，则这些注册表会自动受到保护，并将从该时间开始收费。<br>（相关策略：[应在 Azure 容器注册表注册表中启用高级威胁防护](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2fc25d9a16-bc35-4e15-a7e5-9db606bf9ed4)）|高|**是**|订阅|
|应修正 Azure 容器注册表映像中的漏洞（由 Qualys 提供技术支持）|容器映像漏洞评估功能会扫描注册表中每个推送的容器映像上的安全漏洞，并按映像显示详细的发现结果。 修复这些漏洞可以极大改善容器的安全状况，并保护其不受攻击影响。<br>（无相关策略）|高|N|计算资源（容器）|
||||||


## <a name="app-service-recommendations"></a><a name="recs-appservice"></a>应用服务建议

|建议|说明及相关策略|严重性|已启用快速修复？（[了解详细信息](https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations#recommendations-with-quick-fix-remediation)）|资源类型|
|----|----|----|----|----|
|**应在 Azure 应用服务计划上启用高级威胁防护**|安全中心利用云的规模和 Azure 作为云提供商拥有的可见性来监视常见的 Web 应用攻击。<br>重要提示：修正此建议将导致保护应用服务计划的费用。 如果此订阅中没有任何应用服务计划，则不会产生任何费用。 如果以后在此订阅上创建任何应用服务计划，则这些计划会自动受到保护，并将在该时间开始收费。<br>（相关策略：[应对 Azure App Service 计划启用高级威胁防护](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f2913021d-f2fd-4f3d-b958-22354e2bdbcb)）|高|**是**|订阅|
|**只能通过 HTTPS 访问 Web 应用程序**|为 Web 应用启用“仅 HTTPS”访问权限。 使用 HTTPS 可确保执行服务器/服务身份验证服务，并保护传输中的数据不受网络层窃听攻击威胁。<br>（相关策略：应只能通过 HTTPS 访问 Web 应用程序）|中型|**是**|应用服务|
|**应该只能通过 HTTPS 访问函数应用**|为函数应用启用“仅 HTTPS”访问权限。 使用 HTTPS 可确保执行服务器/服务身份验证服务，并保护传输中的数据不受网络层窃听攻击威胁。<br>（相关策略：应只能通过 HTTPS 访问函数应用）|中型|**是**|应用服务|
|**只能通过 HTTPS 访问 API 应用**|仅限通过 HTTPS 访问 API 应用。<br>（相关策略：应只能通过 HTTPS 访问 API 应用）|中型|N|应用服务|
|**应禁用 Web 应用程序的远程调试**|如果不再需要使用 Web 应用程序的调试，请将其禁用。 远程调试需要在 Web 应用上打开入站端口。<br>（相关策略：应为 Web 应用程序禁用远程调试）|低|**是**|应用服务|
|应为函数应用禁用远程调试|如果不再需要使用函数应用的调试，请将其禁用。 远程调试需要在函数应用上打开入站端口。<br>（相关策略：应为函数应用禁用远程调试）|低|**是**|应用服务|
|应为 API 应用禁用远程调试|如果不再需要针对 API 应用的调试，请将其禁用。 远程调试需要在 API 应用上打开入站端口。<br>（相关策略：应为 API 应用禁用远程调试）|低|**是**|应用服务|
|**CORS 不应允许所有资源都能访问你的 Web 应用程序**|仅允许所需的域与 Web 应用程序交互。 跨源资源共享 (CORS) 不应允许所有域都能访问你的 Web 应用程序。<br>（相关策略：CORS 不应允许所有资源都能访问 Web 应用程序）|低|**是**|应用服务|
|CORS 不应允许所有资源都能访问函数应用|仅允许所需的域与函数应用程序交互。 跨源资源共享 (CORS) 不应允许所有域都能访问你的函数应用程序。<br>（相关策略：CORS 不应允许所有资源都能访问函数应用）|低|**是**|应用服务|
|**CORS 不应允许所有资源都能访问 API 应用**|仅允许所需的域与 API 应用程序交互。 跨源资源共享 (CORS) 不应允许所有域都能访问你的 API 应用程序。<br>（相关策略：CORS 不应允许所有资源都能访问 API 应用）|低|**是**|应用服务|
|**应启用应用程序服务中的诊断日志**|启用日志并将其保留长达一年。 这样便可以在发生安全事件或网络遭泄露时，重新创建活动线索用于调查目的。<br>（相关策略：应启用应用服务的诊断日志）</span>|低|N|应用服务|
||||||


## <a name="compute-and-app-recommendations"></a><a name="recs-computeapp"></a>计算和应用建议

|建议|说明及相关策略|严重性|已启用快速修复？（[了解详细信息](https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations#recommendations-with-quick-fix-remediation)）|资源类型|
|----|----|----|----|----|
|**应在虚拟机上启用高级威胁防护**|安全中心为虚拟机工作负荷提供实时威胁保护，并生成强化 recommmendations 以及可疑活动的警报。<br>你可以使用此信息快速纠正安全问题，并提高虚拟机的安全性。<br>重要提示：修正此建议将导致保护虚拟机的费用。 如果此订阅中没有任何虚拟机，将不会产生任何费用。 如果以后在此订阅上创建任何虚拟机，则这些虚拟机将自动受到保护，并将在该时间开始收费。<br>（相关策略：[应在虚拟机上启用高级威胁防护](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f4da35fc9-c9e7-4960-aec9-797fe7d9051d)）|高|**是**|订阅|
|**应启用 Azure 流分析的诊断日志**|启用日志并将其保留长达一年。 这样便可以在发生安全事件或网络遭泄露时，重新创建活动线索用于调查目的。<br>（相关策略：应启用 Azure 流分析的诊断日志）|低|**是**|计算资源（流分析）|
|**应启用 Batch 帐户的诊断日志**|启用日志并将其保留长达一年。 这样便可以在发生安全事件或网络遭泄露时，重新创建活动线索用于调查目的。<br>（相关策略：应启用 Batch 帐户的诊断日志）|低|**是**|计算资源 (Batch)|
|**应启用事件中心的诊断日志**|启用日志并将其保留长达一年。 这样便可以在发生安全事件或网络遭泄露时，重新创建活动线索用于调查目的。<br>（相关策略：应启用事件中心的诊断日志）|低|**是**|计算资源（事件中心）|
|**应启用逻辑应用的诊断日志**|启用日志并将其保留长达一年。 这样便可以在发生安全事件或网络遭泄露时，重新创建活动线索用于调查目的。<br>（相关策略：应启用逻辑应用的诊断日志）|低|**是**|计算资源（逻辑应用）|
|**应启用搜索服务的诊断日志**|启用日志并将其保留长达一年。 这样便可以在发生安全事件或网络遭泄露时，重新创建活动线索用于调查目的。<br>（相关策略：应启用搜索服务的诊断日志）|低|**是**|计算资源（搜索）|
|**应启用服务总线的诊断日志**|启用日志并将其保留长达一年。 这样便可以在发生安全事件或网络遭泄露时，重新创建活动线索用于调查目的。<br>（相关策略：应启用服务总线的诊断日志）|低|**是**|计算资源（服务总线）|
|**Service Fabric 群集应仅使用 Azure Active Directory 进行客户端身份验证**|在 Service Fabric 中仅通过 Azure Active Directory 执行客户端身份验证。<br>（相关策略：Service Fabric 群集应仅使用 Azure Active Directory 进行客户端身份验证）|高|N|计算资源 (Service Fabric)|
|**Service Fabric 群集应将 ClusterProtectionLevel 属性设置为 EncryptAndSign**|Service Fabric 使用主要群集证书为节点之间的通信提供三个保护级别（None、Sign 和 EncryptAndSign）。 请设置保护级别，确保节点到节点的所有消息经过加密和数字签名。<br>（相关策略：应将 Service Fabric 中的 ClusterProtectionLevel 属性设置为 EncryptAndSign）|高|N|计算资源 (Service Fabric)|
|**应从服务总线命名空间中删除 RootManageSharedAccessKey 以外的所有授权规则**|服务总线客户端不应使用提供对命名空间中所有队列和主题的访问的命名空间级访问策略。 若要符合最低特权安全模型，应在实体级别针对队列和主题创建访问策略，以便仅提供对特定实体的访问。<br>（相关策略：应从服务总线命名空间中删除 RootManageSharedAccessKey 以外的所有授权规则）|低|N|计算资源（服务总线）|
|**应从事件中心命名空间中删除 RootManageSharedAccessKey 以外的所有授权规则**|服务中心客户端不应使用提供对命名空间中所有队列和主题的访问的命名空间级访问策略。 若要符合最低特权安全模型，应在实体级别针对队列和主题创建访问策略，以便仅提供对特定实体的访问。<br>（相关策略：应从事件中心命名空间中删除 RootManageSharedAccessKey 以外的所有授权规则）|低|N|计算资源（事件中心）|
|应定义事件中心实体上的授权规则|审核事件中心实体的授权规则以授予最低访问权限。<br>（相关策略：应定义事件中心实体上的授权规则）|低|N|计算资源（事件中心）|
|在虚拟机上安装监视代理|安装监视代理以对每台计算机启用数据收集、更新扫描、基线扫描和终结点保护。<br>（无相关策略）|高|**是**|计算机|
|**Log Analytics 代理应安装在基于 Windows 的 Azure Arc 计算机上（预览）**|安全中心使用[Log Analytics 代理](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent)（也称为 MMA）从 Azure Arc 计算机收集安全事件。<br>（相关策略： [预览版]：应在 Windows Azure Arc 计算机上安装 Log Analytics 代理）|高|**是**|Azure Arc 计算机|
|**Log Analytics 代理应安装在基于 Linux 的 Azure Arc 计算机上（预览版）**|安全中心使用[Log Analytics 代理](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent)（也称为 MMA）从 Azure Arc 计算机收集安全事件。<br>（相关策略： [预览版]：应在 Linux Azure Arc 计算机上安装 Log Analytics 代理）|高|**是**|Azure Arc 计算机|
|**应在 Windows 虚拟机上安装来宾配置扩展（预览版）**|安装来宾配置代理以在计算机内启用审核设置，例如：操作系统配置、应用程序配置或状态显示、环境设置。 安装后，来宾内策略将可用，如 "应启用 Windows Exploit guard"。<br>（相关策略：审核在 Windows Vm 上启用来宾配置策略的先决条件）|高|**是**|计算机|
|**应在你的计算机上启用 Windows Defender 攻击防护（预览版）**|Windows Defender 攻击防护利用 Azure 策略来宾配置代理。 利用防护具有四个组件，这些组件专门用于锁定设备，使其免受恶意软件攻击的各种攻击媒介和阻止行为的威胁，同时使企业能够平衡其安全风险和生产力要求（仅限 Windows）。<br>（相关策略：审核未启用 Windows Defender 攻击防护的 Windows Vm）|中型|N|计算机|
|应在计算机上解决监视代理运行状况问题|若要实现全面的安全中心保护，请遵照故障排除指南中的说明，解决计算机上的监视代理问题<br>（无相关策略 - 依赖于“在虚拟机上安装监视代理”）|中型|N|计算机|
|**应在虚拟机上启用自适应应用程序控制**|启用应用程序控制，以控制哪些应用程序可在 Azure 中的 VM 上运行。 这有助于强化 VM 防范恶意软件的能力。 安全中心使用机器学习来分析每个 VM 上运行的应用程序，帮助你运用此智能来应用允许规则。 此功能简化了配置和维护应用程序允许规则的过程。<br>（相关策略：应对虚拟机启用自适应应用程序控制）|高|N|计算机|
|在计算机上安装 Endpoint Protection 解决方案|在 Windows 和 Linux 计算机上安装 Endpoint Protection 解决方案，以保护其免受威胁和漏洞的侵害。<br>（相关策略：监视 Azure 安全中心 Endpoint Protection 的缺失情况）|中型|N|计算机|
|在虚拟机上安装 Endpoint Protection 解决方案|在虚拟机上安装终结点保护解决方案，以保护其免受威胁和漏洞的侵害。<br>（无相关策略）|中型|N|计算机|
|应为云服务角色更新 OS 版本|将云服务角色的操作系统(OS)版本更新为适用于 OS 系列的最新版本。<br>（无相关策略）|高|N|计算机|
|**应在计算机上安装系统更新**|安装缺少的系统安全和关键更新，以保护 Windows 和 Linux 虚拟机与计算机<br>（相关策略：应在计算机上安装系统更新）|高|N|计算机|
|应在 Linux 虚拟机上安装网络流量数据收集代理（预览版）|安全中心使用 Microsoft Monitoring Dependency Agent 从 Azure 虚拟机收集网络流量数据，以启用高级网络保护功能，例如网络映射上的流量可视化效果、网络强化建议和特定网络威胁。<br>（相关策略：[预览]：应在 Linux 虚拟机上安装网络流量数据收集代理）|中型|**是**|计算机|
|应在 Windows 虚拟机上安装网络流量数据收集代理（预览版）|安全中心使用 Microsoft Monitoring Dependency Agent 从 Azure 虚拟机收集网络流量数据，以启用高级网络保护功能，例如网络映射上的流量可视化效果、网络强化建议和特定网络威胁。<br>（相关策略：[预览]：应在 Windows 虚拟机上安装网络流量数据收集代理）|中型|**是**|计算机|
|在虚拟机上启用内置漏洞评估解决方案|安装 Qualys 代理(内置 Azure 安全中心标准层产品)，以在虚拟机上启用同类最佳的漏洞评估解决方案。<br>（相关策略：[应在虚拟机上启用漏洞评估](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F501541f7-f7e7-4cd6-868c-4190fdad3ac9)）|中型|**是**|计算机|
|修正虚拟机上发现的漏洞（由 Qualys 提供支持）|监视由 Azure 安全中心的内置漏洞评估解决方案(由 Qualys 提供支持)所发现的虚拟机上的漏洞发现。<br>（相关策略：应在虚拟机上启用漏洞评估）|低|N|计算机|
|应重启计算机来应用系统更新|重启计算机以应用系统更新并保护计算机免受漏洞攻击。<br>（无相关策略 - 依赖于“应在计算机上安装系统更新”）|中型|N|计算机|
|**自动化帐户变量应加密**|存储敏感数据时，请启用自动化帐户变量资产加密。<br>（相关策略：应对自动化帐户变量启用加密）|高|N|计算资源（自动化帐户）|
|**应在虚拟机上应用磁盘加密**|使用适用于 Windows 和 Linux 虚拟机的 Azure 磁盘加密来加密虚拟机磁盘。 Azure 磁盘加密 (ADE) 利用行业标准 Windows 的 BitLocker 功能和 Linux 的 DM-Crypt 功能提供 OS 磁盘和数据磁盘加密，以帮助保护数据，并实现组织在客户 Azure Key Vault 方面作出的安全性与合规性承诺。 当合规性与安全性政策要求使用加密密钥对数据进行端到端加密（包括加密临时磁盘，即本地附加的临时磁盘）时，请使用 Azure 磁盘加密。 或者，系统默认会使用 Azure 存储服务加密对托管磁盘进行静态加密，其中，加密密钥是 Azure 中的 Microsoft 托管密钥。 如果这符合你的合规性与安全性要求，则可以利用默认托管磁盘加密来满足要求。<br>（相关策略：应对虚拟机应用磁盘加密）|高|N|计算机|
|**应将虚拟机迁移到新的 Azure 资源管理器资源**|对虚拟机使用 Azure 资源管理器以提供安全增强功能，例如：更强的访问控制 (RBAC)、更好的审核、基于资源管理器的部署和治理、托管标识访问权限、用于存储机密的密钥保管库的访问权限、基于 Azure AD 的身份验证以及可实现更轻松安全管理的标记和资源组支持。<br>（相关策略：应将虚拟机迁移到新的 Azure 资源管理器资源）|低|N|计算机|
|应在虚拟机上安装漏洞评估解决方案|在虚拟机上安装漏洞评估解决方案<br>（相关策略：应通过漏洞评估解决方案修复漏洞）|中型|N|计算机|
|**应通过漏洞评估解决方案修复漏洞**|会持续评估为其部署了漏洞评估第三方解决方案的虚拟机的应用程序和 OS 漏洞。 只要发现此类漏洞，就会在建议中提供详细信息。<br>（相关策略：应通过漏洞评估解决方案修正漏洞）|高|N|计算机|
|**应修复计算机上安全配置中的漏洞**|修复计算机上安全配置的漏洞，以保护其免受攻击。<br>（相关策略：应修正计算机上安全配置中的漏洞）|低|N|计算机|
|**应修正容器安全配置中的漏洞**|修复安装了 Docker 的计算机上安全配置中的漏洞，使它们免受攻击。<br>（相关策略：应修正容器安全配置中的漏洞）|高|N|计算机|
|应在计算机上解决 Endpoint Protection 运行状况问题|若要实现全面的安全中心保护，请遵照故障排除指南中的说明，解决计算机上的监视代理问题。<br>（此建议依赖于建议“在计算机上安装 Endpoint Protection 解决方案”和其策略）|中型|N|计算机|
||||||


## <a name="virtual-machine-scale-set-recommendations"></a><a name="recs-vmscalesets"></a>虚拟机规模集建议

|建议|说明及相关策略|严重性|已启用快速修复？（[了解详细信息](https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations#recommendations-with-quick-fix-remediation)）|资源类型|
|----|----|----|----|----|
|**应当启用虚拟机规模集中的诊断日志**|启用日志并将其保留长达一年的时间。 这样可以重新创建用于调查的活动线索。 这适用于发生安全事件或网络受到危害的情况。<br>（相关策略：应启用虚拟机规模集的诊断日志）|低|N|虚拟机规模集|
|应在虚拟机规模集上修正 Endpoint Protection 运行状况故障|修复虚拟机规模集上的 Endpoint Protection 运行状况故障，使其免受威胁和漏洞的侵害。<br>（无相关策略 - 依赖于“应在虚拟机规模集上安装 Endpoint Protection 解决方案”）|低|N|虚拟机规模集|
|**应在虚拟机规模集上安装终结点保护解决方案**|在虚拟机规模集上安装 Endpoint Protection 解决方案，使其免受威胁和漏洞的侵害。<br>（相关策略：应在虚拟机规模集上安装 Endpoint Protection 解决方案）|高|N|虚拟机规模集|
|应在虚拟机规模集上安装监视代理|安全中心使用 Microsoft Monitoring Agent (MMA) 从 Azure 虚拟机规模集中收集安全事件。 无法为 Azure 虚拟机规模集配置 MMA 自动预配。 若要在虚拟机规模集上（包括 Azure Kubernetes 服务和 Azure Service Fabric 等 Azure 托管服务所使用的规模集）部署 MMA，请按照修正步骤操作。|高|**是**|虚拟机规模集|
|**应在虚拟机规模集上安装系统更新**|安装缺少的系统安全更新和关键更新，保护 Windows 和 Linux 虚拟机规模集。<br>（相关策略：应在虚拟机规模集上安装系统更新）|高|N|虚拟机规模集|
|**应修复虚拟机规模集上安全配置中的漏洞**|修复虚拟机规模集上安全配置中的漏洞，使其免受攻击。 <br>（相关策略：应修正虚拟机规模集上安全配置中的漏洞）|高|N|虚拟机规模集|
||||||


## <a name="data-and-storage-recommendations"></a><a name="recs-datastorage"></a>数据和存储建议

|建议|说明及相关策略|严重性|已启用快速修复？（[了解详细信息](https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations#recommendations-with-quick-fix-remediation)）|资源类型|
|----|----|----|----|----|
|**应在 Azure SQL 数据库服务器上启用高级数据安全**|高级数据安全是一个提供高级 SQL 安全功能的统一包。 它包括用于呈现和缓解潜在的数据库漏洞的功能，检测可能指示数据库威胁的异常活动，并发现和分类敏感数据。 <br>重要提示：修正此建议将导致保护 Azure SQL 数据库服务器的费用。 如果此订阅中没有任何 Azure SQL 数据库服务器，则不会产生任何费用。 如果以后在此订阅上创建任何 Azure SQL 数据库服务器，则这些服务器会自动受到保护，并将在该时间开始收费。<br>（相关策略：[应在 AZURE SQL 数据库服务器上启用高级数据安全性](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f7fe3b40f-802b-4cdd-8bd4-fd799c948cc2)）|高|**是**|订阅|
|**应在计算机的 SQL 服务器上启用高级数据安全**|高级数据安全是一个提供高级 SQL 安全功能的统一包。 它包括用于呈现和缓解潜在的数据库漏洞的功能，检测可能指示数据库威胁的异常活动，并发现和分类敏感数据。 <br>重要提示：修正此建议将导致保护计算机上的 SQL server 的费用。 如果此订阅中的计算机上没有 SQL server，则不会产生任何费用。 如果以后在此订阅的计算机上创建任何 SQL server，则这些服务器会自动受到保护，并将在该时间开始收费。<br>（相关策略：[应在计算机上的 SQL server 上启用高级数据安全性](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f6581d072-105e-4418-827f-bd446d56421b)）|高|**是**|订阅|
|**应对 Azure 存储帐户启用高级威胁防护**|针对存储的高级威胁防护检测到异常和可能有害的访问或利用存储帐户的尝试。<br>重要提示：修正此建议将导致保护 Azure 存储帐户。 如果此订阅中没有任何 Azure 存储帐户，则不会产生任何费用。 如果以后在此订阅上创建任何 Azure 存储帐户，这些帐户将自动受到保护，并将在该时间开始收费。<br>（相关策略：[应在 Azure 存储帐户上启用高级威胁防护](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f308fbb08-4ab8-4e67-9b29-592e93fb94fa)）|高|**是**|订阅|
|应限制对具有防火墙和虚拟网络配置的存储帐户的访问|在存储帐户防火墙设置中审核无限制的网络访问权限。 应该配置网络规则，以便只有来自许可网络的应用程序才能访问存储帐户。 若要允许来自特定 Internet 或本地客户端的连接，可以向来自特定 Azure 虚拟网络或到公共 Internet IP 地址范围的流量授予访问权限。<br>（相关策略：审核对存储帐户的无限制网络访问）|低|N|存储帐户|
|应对托管实例启用高级数据安全|高级数据安全 (ADS) 是一个提供高级 SQL 安全功能的统一包。 它可发现和分类敏感数据、呈现和减少潜在数据库漏洞，以及检测可能表明数据库有威胁的异常活动。 每个托管实例的广告收费为 $15。<br>（相关策略：应在 SQL 托管实例上启用高级数据安全性）|高|**是**|SQL|
|**应在 SQL 服务器上启用高级数据安全性**|高级数据安全 (ADS) 是一个提供高级 SQL 安全功能的统一包。 它可发现和分类敏感数据、呈现和减少潜在数据库漏洞，以及检测可能表明数据库有威胁的异常活动。 ADS 费用为每个 SQL server 15 美元。<br>（相关策略：应对 SQL Server 启用高级数据安全）|高|**是**|SQL|
|**应为 SQL 数据库设置 Azure Active Directory 管理员**|为 SQL 数据库预配 Azure AD 管理员，以启用 Azure AD 身份验证。 使用 Azure AD 身份验证可以简化权限管理，以及集中化数据库用户和其他 Microsoft 服务的标识管理。<br>（相关策略：审核确认已为 SQL Server 预配了 Azure Active Directory 管理员）|高|N|SQL|
|**应启用对 SQL 数据库的审核**|启用 SQL 数据库审核。 <br>（相关策略：应为服务器的高级数据安全设置上的 SQL 数据库启用审核）|低|**是**|SQL|
|**应启用 Azure Data Lake Store 的诊断日志**|启用日志并将其保留长达一年。 这样便可以在发生安全事件或网络遭泄露时，重新创建活动线索用于调查目的。<br>（相关策略：应启用 Azure Data Lake Store 的诊断日志）|低|**是**|Data Lake Store|
|**应启用 Data Lake Analytics 的诊断日志**|启用日志并将其保留长达一年。 这样便可以在发生安全事件或网络遭泄露时，重新创建活动线索用于调查目的。<br>（相关策略：应启用 Data Lake Analytics 的诊断日志）|低|**是**|Data Lake Analytics|
|**只应启用与 Redis 缓存的安全连接**|仅启用通过 SSL 来与 Azure Redis 缓存建立连接。 使用安全连接可确保服务器和服务之间的身份验证并保护传输中的数据免受中间人攻击、窃听攻击和会话劫持等网络层攻击。<br>（相关策略：应仅启用与 Redis 缓存的安全连接）|高|N|Redis|
|**应启用安全传输到存储帐户**|安全传输选项会强制存储帐户仅接受来自安全连接 (HTTPS) 的请求。 HTTPS 可确保服务器和服务之间的身份验证并保护传输中的数据免受中间人攻击、窃听和会话劫持等网络层攻击。<br>（相关策略：应启用到存储帐户的安全传输）|高|N|存储帐户|
|**应对 SQL 数据库中的敏感数据进行分类**|Azure SQL 数据库数据发现 & 分类提供用于发现、分类、标记和保护数据库中的敏感数据的功能。 数据分类后，可以使用 Azure SQL 数据库审核来审核访问和监视敏感数据。 Azure SQL 数据库还启用了高级威胁防护功能，这些功能基于访问模式对敏感数据的更改创建智能警报。<br>（相关策略：[预览]：应对 SQL 数据库中的敏感数据进行分类）|高|N|SQL|
|**存储帐户应迁移到新的 Azure 资源管理器资源**|为存储帐户使用新的 Azure 资源管理器以提供安全增强功能，例如：更强的访问控制(RBAC)、更好地审核、基于资源管理器的部署和治理、托管标识访问权限、用于提供机密的 Key Vault 的访问权限、基于 Azure AD 的身份验证以及可实现更轻松安全管理的标记和资源组支持。<br>（相关策略：应将存储帐户迁移到新的 Azure 资源管理器资源）|低|N|存储帐户|
|**应在 SQL 数据库上启用透明数据加密**|启用透明数据加密以保护静态数据并满足合规性要求。<br>（相关策略：应对 SQL 数据库启用透明数据加密）|低|**是**|SQL|
|**应在 SQL 数据库上启用漏洞评估**|漏洞评估可发现、跟踪和帮助你修正潜在数据库漏洞。<br>（相关策略：应对 SQL Server 启用漏洞评估）|高|**是**|SQL|
|**应在 SQL 托管实例上启用漏洞评估**|漏洞评估可发现、跟踪和帮助你修正潜在数据库漏洞。<br>（相关策略：应在 SQL 托管实例上启用漏洞评估）|高|**是**|SQL|
|**应修正计算机上的 SQL server 上的漏洞**|SQL 漏洞评估会扫描数据库中的安全漏洞，并显示与最佳实践之间的任何偏差，如配置错误、权限过多和敏感数据未受保护。 解决发现的漏洞可以极大地改善数据库安全态势。|高|N|SQL|
|**应修复 SQL 数据库中的漏洞**|SQL 漏洞评估会扫描数据库中的安全漏洞，并显示与最佳实践之间的任何偏差，如配置错误、权限过多和敏感数据未受保护。 解决发现的漏洞可以极大地改善数据库安全态势。<br>（相关策略：应修正 SQL 数据库中的漏洞）|高|N|SQL|
||||||


## <a name="identity-and-access-recommendations"></a><a name="recs-identity"></a>标识和访问建议

|建议|说明及相关策略|严重性|已启用快速修复？（[了解详细信息](https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations#recommendations-with-quick-fix-remediation)）|资源类型|
|----|----|----|----|----|
|**应在对订阅拥有读取权限的帐户上启用 MFA**|为具有读取特权的所有订阅帐户启用多重身份验证 (MFA)，以防止破坏帐户或资源。<br>（相关策略：应在对订阅拥有读取权限的帐户上启用 MFA）|高|N|订阅|
|应在对订阅拥有写入权限的帐户上启用 MFA|为具有写入特权的所有订阅帐户启用多重身份验证 (MFA)，以防止破坏帐户或资源。<br>（相关策略：应在对订阅拥有写入权限的帐户上启用 MFA）|高|N|订阅|
|**应在对订阅拥有所有者权限的帐户上启用 MFA**|为具有所有者特权的所有订阅帐户启用多重身份验证 (MFA)，以防止破坏帐户或资源。<br>（相关策略：应在对订阅拥有所有者权限的帐户上启用 MFA）|高|N|订阅|
|**应从订阅中删除拥有读取权限的外部帐户**|从订阅中删除具有读取特权的外部帐户，以防止发生未受监视的访问。<br>（相关策略：应从订阅中删除拥有读取权限的外部帐户）|高|N|订阅|
|**应从订阅中删除具有写入权限的外部帐户**|从订阅中删除具有写入特权的外部帐户，以防止发生未受监视的访问。<br>（相关策略：应从订阅中删除具有写入权限的外部帐户）|高|N|订阅|
|**应从订阅中删除拥有所有者权限的外部帐户**|从订阅中删除具有所有者特权的外部帐户，以防止发生未受监视的访问。<br>（相关策略：应从订阅中删除拥有所有者权限的外部帐户）|高|N|订阅|
|**应从订阅中删除拥有所有者权限的已弃用帐户**|从订阅中删除拥有所有者权限的已弃用帐户。<br>（相关策略：应从订阅中删除拥有所有者权限的已弃用帐户）|高|N|订阅|
|**应从订阅中删除弃用的帐户**|从订阅中删除已弃用的帐户，以便只允许当前用户访问。<br>（相关策略：应从订阅中删除弃用的帐户）|高|N|订阅|
|**应为订阅分配了多个所有者**|指定多个订阅所有者，以实现管理员访问权限冗余。<br>（相关策略：应向订阅分配多个所有者）|高|N|订阅|
|**只多只为订阅指定 3 个所有者**|指定少于 3 个订阅所有者，以减少已遭入侵的所有者做出违规行为的可能性。<br>（相关策略：最多只能为订阅指定 3 个所有者）|高|N|订阅|
|**应对 Azure Key Vault 的保管库启用高级威胁防护**|Azure 安全中心包含针对 Azure Key Vault 的 Azure 原生高级威胁防护，提供额外的安全情报层。<br>重要提示：修正此建议将导致保护 AKV 保管库的费用。 如果此订阅中没有任何 AKV 保管库，则不会产生任何费用。 如果以后在此订阅上创建任何 AKV 保管库，则这些保管库将自动受到保护，并将从该时间开始收费。<br>（相关策略：[应对 Azure Key Vault 保管库启用高级威胁防护](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f0e6763cc-5078-4e64-889d-ff4d9a839047)）|高|**是**|订阅|
|**应启用 Key Vault 的诊断日志**|启用日志并将其保留长达一年。 这样便可以在发生安全事件或网络遭泄露时，重新创建活动线索用于调查目的。<br>（相关策略：应启用密钥保管库的诊断日志）|低|**是**|Key Vault|
||||||


## <a name="deprecated-recommendations"></a>弃用的建议

|建议|说明及相关策略|严重性|已启用快速修复？（[了解详细信息](https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations#recommendations-with-quick-fix-remediation)）|资源类型|
|----|----|----|----|----|
|应限制对应用服务的访问|通过更改网络配置来限制对应用服务的访问，以拒绝来自过大范围的入站流量。<br>（相关策略：[预览]：应限制对应用服务的访问）|高|N|应用服务|
|应强化 IaaS NSG 上 Web 应用的规则|如果运行 web 应用程序的虚拟机的网络安全组 (NSG) 所包含的 NSG 规则对于 web 应用程序端口而言过于宽松，应强化这些安全组。<br>（相关策略：应该强化 IaaS 上 Web 应用程序的 NSG 规则）|高|N|虚拟机|



## <a name="next-steps"></a>后续步骤
若要详细了解建议，请参阅以下内容：

* [有关如何分析安全中心所提建议的 Microsoft Learn 模块](https://docs.microsoft.com/learn/modules/identify-threats-with-azure-security-center/)
* [Azure 安全中心的安全建议](security-center-recommendations.md)
* [保护计算机和应用程序](security-center-virtual-machine-protection.md)
* [保护 Azure 安全中心中的网络](security-center-network-recommendations.md)