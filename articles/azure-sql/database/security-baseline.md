---
title: Azure 安全基线
description: 适用于 Azure SQL 数据库和 Azure SQL 的 azure 安全基线托管实例
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 04/09/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 3c890dc48c97580fd8de5c817c96b62d4eb9c009
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/07/2020
ms.locfileid: "86045937"
---
# <a name="azure-security-baseline-for-azure-sql-database--sql-managed-instance"></a>适用于 Azure SQL 数据库的 azure 安全基线 & SQL 托管实例
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

适用于 Azure SQL 数据库的 Azure 安全基准包含有助于改进部署安全状况的建议。

此服务的基线摘自 [Azure 安全基准版本 1.0](https://docs.microsoft.com/azure/security/benchmarks/overview)，其中提供了有关如何根据我们的最佳做法指导保护 Azure 上的云解决方案的建议。

有关详细信息，请参阅 [Azure 安全基线概述](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview)。

## <a name="network-security"></a>网络安全

有关详细信息，请参阅[安全控制：网络安全性](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security)。

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1.1：在虚拟网络中使用网络安全组或 Azure 防火墙保护资源

**指南**：可以启用 Azure 专用链接，以允许通过虚拟网络中的专用终结点访问 Azure PaaS 服务（例如 SQL 数据库）和 azure 托管的客户/合作伙伴服务。 虚拟网络与服务之间的流量将通过 Microsoft 主干网络，因此不会从公共 Internet 泄露。

若要允许流量到达 Azure SQL 数据库，请使用 SQL 服务标记，以允许出站流量通过网络安全组。

虚拟网络规则使 Azure SQL 数据库仅接受从虚拟网络中的所选子网发送的通信。

如何设置 Azure SQL 数据库的专用链接：

https://docs.microsoft.com/azure/sql-database/sql-database-private-endpoint-overview#how-to-set-up-private-link-for-azure-sql-database

如何使用虚拟网络服务终结点和服务器规则：

https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview

**Azure 安全中心监视**：是

**责任**：客户

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1.2：监视和记录 VNet、子网和 NIC 的配置与流量

**指南**：对于 Azure SQL 数据库部署到的子网，使用 Azure 安全中心并修正网络保护建议。

对于将连接到 Azure SQL 数据库的 Azure 虚拟机（VM），启用用于保护这些 Vm 的网络安全组（NSG）流日志，并将日志发送到 Azure 存储帐户以进行流量审核。

还可以将 NSG 流日志发送到 Log Analytics 工作区，并使用流量分析来提供对 Azure 云中的流量流的见解。 流量分析的优势包括能够可视化网络活动、识别热点、识别安全威胁、了解流量流模式，以及查明网络不当配置。

如何启用 NSG 流日志：

https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

了解 Azure 安全中心提供的网络安全性：

https://docs.microsoft.com/azure/security-center/security-center-network-recommendations

如何启用和使用流量分析： 

https://docs.microsoft.com/azure/network-watcher/traffic-analytics

了解 Azure 安全中心提供的网络安全性：

https://docs.microsoft.com/azure/security-center/security-center-network-recommendations

**Azure 安全中心监视**：是

**责任**：客户

### <a name="13-protect-critical-web-applications"></a>1.3：保护关键 Web 应用程序

**指南**：不适用;此建议适用于 Azure 应用服务或托管 web 应用程序的计算资源。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4：拒绝与已知恶意的 IP 地址进行通信

**指南**：在与 Azure SQL 数据库关联的虚拟网络上启用 DDoS 保护标准，以防范分布式拒绝服务攻击。 根据 Azure 安全中心集成的威胁情报进行判断，拒绝与已知恶意的或未使用过的 Internet IP 地址通信。

如何配置 DDoS 防护：

https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection

了解 Azure 安全中心集成的威胁情报：

https://docs.microsoft.com/azure/security-center/security-center-alerts-data-services

**Azure 安全中心监视**：是

**责任**：客户

### <a name="15-record-network-packets-and-flow-logs"></a>1.5：记录网络数据包和流日志

**指南**：对于将连接到 Azure SQL 数据库实例的 Azure 虚拟机（vm），启用 nsg 的网络安全组（NSG）流日志来保护这些 vm，并将日志发送到 Azure 存储帐户以进行流量审核。 如果需要调查异常活动，请启用网络观察程序数据包捕获。

如何启用 NSG 流日志：

https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

如何启用网络观察程序：

https://docs.microsoft.com/azure/network-watcher/network-watcher-create

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6：部署基于网络的入侵检测/入侵防护系统 (IDS/IPS)

**指南**：为 Azure SQL 数据库启用高级威胁防护（ATP）。  出现可疑数据库活动、潜在漏洞、SQL 注入攻击和异常数据库访问和查询模式时，用户将收到警报。 高级威胁防护还将警报与 Azure 安全中心集成。

了解并使用适用于 Azure SQL 数据库的高级威胁防护：https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview

**Azure 安全中心监视**：是

**责任**：客户

### <a name="17-manage-traffic-to-web-applications"></a>1.7：管理发往 Web 应用程序的流量

**指南**：不适用;此建议适用于 Azure 应用服务或托管 web 应用程序的计算资源。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8：最大程度地降低网络安全规则的复杂性和管理开销

**指南**：使用虚拟网络服务标记定义网络安全组或 Azure 防火墙上的网络访问控制。 创建安全规则时，可以使用服务标记代替特定的 IP 地址。 在规则的相应源或目标字段中指定服务标记名称（例如 ApiManagement），可以允许或拒绝相应服务的流量。 Microsoft 会管理服务标记包含的地址前缀，并会在地址发生更改时自动更新服务标记。

使用 Azure SQL 数据库的服务终结点时，需要出站到 Azure SQL 数据库的公共 IP 地址：必须打开网络安全组（Nsg），以允许连接到 Azure SQL 数据库 Ip。 为此，可以使用适用于 Azure SQL 数据库的 NSG 服务标记。

了解 Azure SQL 数据库的服务终结点的服务标记：

https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview#limitations

了解并使用服务标记：

https://docs.microsoft.com/azure/virtual-network/service-tags-overview

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9：维护网络设备的标准安全配置

**指南**：通过 azure 策略为 Azure SQL 数据库定义和实施网络安全配置。 你可以使用 "Sql-dmo" 命名空间来定义自定义策略定义，或者使用为服务器网络保护设计的任何内置策略定义。 适用于服务器的内置网络安全策略的一个示例是： "SQL 数据库应使用虚拟网络服务终结点"。

使用 Azure 蓝图可以通过在单个蓝图定义中打包关键环境项目（例如 Azure 资源管理模板、基于角色的访问控制（RBAC）和策略）来简化大规模的 Azure 部署。 轻松将蓝图应用到新的订阅和环境，并通过版本控制来微调控制措施和管理。

如何配置和管理 Azure Policy： https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

如何创建 Azure 蓝图： https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal

**Azure 安全中心监视**：是

**责任**：客户

### <a name="110-document-traffic-configuration-rules"></a>1.10：阐述流量配置规则

**指南**：对网络安全组（NSG）和与网络安全和通信流相关的其他资源使用标记。 对于单个 NSG 规则，请使用“说明”字段针对允许流量传入/传出网络的任何规则指定业务需求和/或持续时间等。

使用与标记相关的任何内置 Azure 策略定义（如 "需要标记和值"）来确保使用标记创建所有资源并通知现有未标记资源。

可以使用 Azure PowerShell 或 Azure CLI 基于其标记对资源进行查找或执行操作。

如何创建和使用标记：

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure 安全中心监视**：不适用

责任：客户

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11：使用自动化工具来监视网络资源配置和检测更改

**指南**：使用 Azure 活动日志监视网络资源配置，并检测与服务器相关的网络资源的更改。 在 Azure Monitor 中创建当关键网络资源发生更改时触发的警报。

如何查看和检索 Azure 活动日志事件：

https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view

如何在 Azure Monitor 中创建警报： 

https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

**Azure 安全中心监视**：不适用

责任：客户

## <a name="logging-and-monitoring"></a>日志记录和监视

有关详细信息，请参阅[安全控制：日志记录和监视](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring)。

### <a name="21-use-approved-time-synchronization-sources"></a>2.1：使用批准的时间同步源

**指南**： Microsoft 维护 Azure 资源的时间源。 你可以为计算部署更新时间同步。

如何为 Azure 计算资源配置时间同步：

https://docs.microsoft.com/azure/virtual-machines/windows/time-sync

**Azure 安全中心监视**：不适用

**责任**：Microsoft

### <a name="22-configure-central-security-log-management"></a>2.2：配置中心安全日志管理

**指南**：为 Azure SQL 数据库启用审核以跟踪数据库事件，并将其写入 Azure 存储帐户、Log Analytics 工作区或事件中心中的审核日志。

此外，你还可以将 Azure SQL 诊断遥测流式传输到 Azure SQL Analytics，这是一种云解决方案，可跨多个订阅大规模监视 Azure SQL 数据库和 Azure SQL 托管实例的性能。 它可以帮助你收集和可视化 Azure SQL 数据库性能指标，并提供内置智能进行性能故障排除。

如何设置 Azure SQL 数据库的审核：

https://docs.microsoft.com/azure/sql-database/sql-database-auditing

如何使用 Azure Monitor 收集平台日志和指标： 

https://docs.microsoft.com/azure/sql-database/sql-database-metrics-diag-logging

如何将诊断流到 Azure SQL Analytics：

https://docs.microsoft.com/azure/sql-database/sql-database-metrics-diag-logging#stream-into-azure-sql-analytics

**Azure 安全中心监视**：是

**责任**：客户

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3：为 Azure 资源启用审核日志记录

**指南**：在服务器上启用审核并为审核日志选择存储位置（Azure 存储、Log Analytics 或事件中心）。

如何启用对 Azure SQL 数据库的审核：

https://docs.microsoft.com/azure/sql-database/sql-database-auditing

**Azure 安全中心监视**：是

**责任**：客户

### <a name="24-collect-security-logs-from-operating-systems"></a>2.4：从操作系统收集安全日志

**指南**：不适用;此基准用于计算资源。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="25-configure-security-log-storage-retention"></a>2.5：配置安全日志存储保留期

**指南**：将 Azure SQL 数据库日志存储在 Log Analytics 工作区中时，请根据组织的符合性法规设置日志保持期。

如何设置日志保留参数： 

https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period

**Azure 安全中心监视**：不适用

责任：客户

### <a name="26-monitor-and-review-logs"></a>2.6：监视和审查日志

**指导**：分析和监视日志中的异常行为，并定期审查结果。 使用 Azure 安全中心的高级威胁防护来对与 Azure SQL 数据库实例相关的异常活动发出警报。 或者，根据与 Azure SQL 数据库实例相关的指标值或 Azure 活动日志条目来配置警报。

了解 Azure SQL 数据库的高级威胁防护和警报：

https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview

如何配置 Azure SQL 数据库的自定义警报：

https://docs.microsoft.com/azure/sql-database/sql-database-insights-alerts-portal?view=azps-1.4.0

**Azure 安全中心监视**：是

**责任**：客户

### <a name="27-enable-alerts-for-anomalous-activity"></a>2.7：针对异常活动启用警报

**指南**：对 Azure SQL 数据库使用 Azure 安全中心高级威胁防护，以便在异常活动上进行监视和警报。 为 SQL 数据库启用高级数据安全性。 高级数据安全性包括用于发现和分类敏感数据、呈现和缓解潜在的数据库漏洞以及检测可能指示数据库威胁的异常活动的功能。

了解 Azure SQL 数据库的高级威胁防护和警报：

https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview

如何为 Azure SQL Database 启用高级数据安全：

https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security

如何在 Azure 安全中心管理警报：

https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts

**Azure 安全中心监视**：是

**责任**：客户

### <a name="28-centralize-anti-malware-logging"></a>2.8：集中管理反恶意软件日志记录

**指南**：不适用;对于 Azure SQL 数据库，反恶意软件解决方案由 Microsoft 在底层平台上进行管理。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="29-enable-dns-query-logging"></a>2.9：启用 DNS 查询日志记录

**指南**：不适用;DNS 日志记录不适用于 Azure SQL 数据库。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="210-enable-command-line-audit-logging"></a>2.10：启用命令行审核日志记录

**指南**：不适用;命令行审核不适用于 Azure SQL 数据库。

**Azure 安全中心监视**：不适用

**责任**：不适用

## <a name="identity-and-access-control"></a>标识和访问控制

有关详细信息，请参阅[安全控制：标识和访问控制](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control)。

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1：维护管理帐户的清单

**指南**： AZURE ACTIVE DIRECTORY （AAD）的内置角色必须显式分配并可查询。 使用 AAD PowerShell 模块执行即席查询，以发现属于管理组成员的帐户。

如何使用 PowerShell 获取 Azure AD 中的目录角色：

https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0

如何使用 PowerShell 获取 Azure AD 中目录角色的成员：

https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0

**Azure 安全中心监视**：不适用

责任：客户

### <a name="32-change-default-passwords-where-applicable"></a>3.2：在适用的情况下更改默认密码

**指南**： Azure Active Directory 没有默认密码的概念。 预配 Azure SQL 数据库实例时，建议选择将身份验证与 Azure Active Directory 集成。

如何配置和管理 Azure SQL Azure Active Directory 身份验证：

https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-configure

**Azure 安全中心监视**：是

**责任**：客户

### <a name="33-use-dedicated-administrative-accounts"></a>3.3：使用专用管理帐户

**指南**：围绕使用专用管理帐户创建策略和过程。 使用 Azure 安全中心标识和访问管理来监视管理帐户的数量。

了解 Azure 安全中心标识和访问： 

https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure 安全中心监视**：是

**责任**：客户

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3.4：将单一登录 (SSO) 与 Azure Active Directory 配合使用

**指南**：不适用;虽然你可以配置 Azure Active Directory 身份验证，以便与 Azure SQL 数据库集成，但不支持单一登录。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5：对所有基于 Azure Active Directory 的访问使用多重身份验证

**指南**：启用 AZURE ACTIVE DIRECTORY （AAD）多重身份验证（MFA）并遵循 Azure 安全中心的标识和访问管理建议。

如何在 Azure 中启用 MFA：

https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

如何在 Azure 安全中心监视标识和访问：

https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure 安全中心监视**：是

**责任**：客户

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6：对所有管理任务使用专用计算机（特权访问工作站）

**指南**：将特权访问工作站（PAW）与配置为登录和配置 Azure 资源的多重身份验证 MFA 结合使用。

了解特权访问工作站：

https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations

如何在 Azure 中启用 MFA：

https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

**Azure 安全中心监视**：不适用

责任：客户

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3.7：记录来自管理帐户的可疑活动并对其发出警报

**指南**：在环境中发生可疑活动或不安全活动时，使用 Azure Active Directory 安全报告生成日志和警报。

使用适用于 Azure SQL 数据库的高级威胁防护来检测异常活动，这些活动表明访问或利用数据库的异常和潜在有害尝试。

如何确定标记为有风险活动 Azure AD 用户：

https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-user-at-risk

如何在 Azure 安全中心监视用户标识和访问活动：

https://docs.microsoft.com/azure/security-center/security-center-identity-access

查看高级威胁防护和潜在警报：

https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview#advanced-threat-protection-alerts

**Azure 安全中心监视**：是

**责任**：客户

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8：仅从批准的位置管理 Azure 资源

**指南**：使用条件访问命名位置，仅允许从 IP 地址范围或国家/地区的特定逻辑分组访问门户和 Azure 资源管理。

如何在 Azure 中配置命名位置： https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="39-use-azure-active-directory"></a>3.9：使用 Azure Active Directory

**指南**：为服务器创建 AZURE ACTIVE DIRECTORY （AAD）管理员。

如何配置和管理 Azure SQL Azure Active Directory 身份验证：

https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-configure

如何创建和配置 AAD 实例：

https://docs.microsoft.com/azure/active-directory-domain-services/tutorial-create-instance

**Azure 安全中心监视**：是

责任：客户

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10：定期审查和协调用户访问

**指南**： AZURE ACTIVE DIRECTORY （AAD）提供日志以帮助发现过时帐户。 此外，使用 Azure 标识访问评审来有效地管理组成员身份、访问企业应用程序和角色分配。 可以定期查看用户的访问权限，以确保只有正确的用户才能继续访问。

如何使用 Azure 标识访问评审：

https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview

**Azure 安全中心监视**：是

**责任**：客户

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3.11：监视访问已停用帐户的企图

**指南**：使用 Azure SQL 配置 AZURE ACTIVE DIRECTORY （AAD）身份验证，并为 Azure Active Directory 用户帐户创建诊断设置，并将审核日志和登录日志发送到 Log Analytics 工作区。 在 Log Analytics 工作区中配置所需的警报。

如何配置和管理 Azure SQL Azure Active Directory 身份验证：

https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-configure

如何将 Azure 活动日志集成到 Azure Monitor 中：

https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="312-alert-on-account-login-behavior-deviation"></a>3.12：针对帐户登录行为偏差发出警报

**指南**：使用 AZURE ACTIVE DIRECTORY （AAD）标识保护和风险检测来配置对检测到的与用户标识相关的可疑操作的自动响应。 此外，可将数据引入 Azure Sentinel 以做进一步调查。

如何查看 Azure AD 风险登录：

https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins

如何配置和启用标识保护风险策略：

https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13：在支持场合下为 Microsoft 提供对相关客户数据的访问权限

**指南**：在 Microsoft 需要访问客户数据的支持方案中，Azure 客户密码箱提供了一个界面，供客户查看和批准或拒绝客户数据访问请求。

了解客户密码箱：

https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview

**Azure 安全中心监视**：不适用

责任：客户

## <a name="data-protection"></a>数据保护

有关详细信息，请参阅[安全控制：数据保护](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection)。

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1：维护敏感信息的清单

**指导**：使用标记可以帮助跟踪存储或处理敏感信息的 Azure 资源。

如何创建和使用标记：

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2：隔离存储或处理敏感信息的系统

**指导**：为开发、测试和生产实施单独的订阅和/或管理组。 资源应由 Vnet/子网进行分隔，对其进行适当标记，并在 NSG 或 Azure 防火墙内保护。 应该隔离存储或处理敏感数据的资源。 使用专用链接;在 Vnet 中部署 Azure SQL 数据库，并使用专用终结点进行私下连接。

如何创建其他 Azure 订阅：

https://docs.microsoft.com/azure/billing/billing-create-subscription

如何创建管理组：

https://docs.microsoft.com/azure/governance/management-groups/create

如何创建和使用标记：

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

如何设置 Azure SQL 数据库的专用链接：

https://docs.microsoft.com/azure/sql-database/sql-database-private-endpoint-overview#how-to-set-up-private-link-for-azure-sql-database

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3：监视和阻止未经授权的敏感信息传输

**指南**：对于 Azure SQL 数据库中的数据库，存储或处理敏感信息，使用标记将数据库和相关资源标记为敏感。 在 Azure SQL 数据库实例上，将专用链接配置为结合网络安全组服务标记，以防止渗透敏感信息。

对于 Microsoft 管理的底层平台，Microsoft 会将所有客户内容视为敏感数据，并会全方位地防范客户数据丢失和遭到透露。 为了确保 Azure 中的客户数据保持安全，Microsoft 已实施并维护一套可靠的数据保护控制机制和功能。

如何配置 Private Link 和 Nsg 以防止 Azure SQL 数据库实例上的数据渗透：

https://docs.microsoft.com/azure/sql-database/sql-database-private-endpoint-overview

了解 Azure 中的客户数据保护：

https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4：加密传输中的所有敏感信息

**指南**： Azure SQL 数据库通过使用传输层安全性对活动中的数据进行加密来保护数据。 对于所有连接，SQL 数据库始终强制执行加密（SSL/TLS）。 这样可以确保在客户端与服务器之间传输的所有数据经过加密，而不管连接字符串中的 Encrypt 或 TrustServerCertificate 设置如何。

了解传输过程中的 Azure SQL 加密：

https://docs.microsoft.com/azure/sql-database/sql-database-security-overview#information-protection-and-encryption

**Azure 安全中心监视**：不适用

**责任**：Microsoft

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5：使用有效的发现工具识别敏感数据

**指南**：使用 Azure SQL 数据库数据发现和分类功能。 数据发现和分类提供了内置于 Azure SQL 数据库的高级功能，用于发现、分类、 &amp; 保护数据库中的敏感数据。

如何使用 Azure SQL 数据库的数据发现和分类：

https://docs.microsoft.com/azure/sql-database/sql-database-data-discovery-and-classification

**Azure 安全中心监视**：是

**责任**：客户

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6：使用 Azure RBAC 控制对资源的访问

**指南**：使用 AZURE ACTIVE DIRECTORY （AAD）对 Azure SQL 数据库实例进行身份验证和控制访问权限。

如何将 Azure SQL 数据库与用于身份验证的 Azure Active Directory 集成：

https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication

如何在 Azure SQL 数据库中控制访问权限：

https://docs.microsoft.com/azure/sql-database/sql-database-control-access

**Azure 安全中心监视**：是

**责任**：客户

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7：使用基于主机的数据丢失防护来强制实施访问控制

**指南**： Microsoft 管理 Azure SQL 数据库的底层基础结构，并已实现严格控制，以防止客户数据丢失或泄露。

了解 Azure 中的客户数据保护：

https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8：静态加密敏感信息

**指南**：透明数据加密（TDE）可帮助保护 Azure sql 数据库、azure sql 托管实例和 Azure 数据仓库，防止静态静态数据的恶意脱机活动的威胁。 它可执行静态数据库、关联备份和事务日志文件的实时加密和解密，无需更改应用程序。 默认情况下，为 SQL 数据库和 SQL 托管实例中的所有新部署的数据库启用了 TDE。 Microsoft 或客户可以管理 TDE 加密密钥。

如何管理透明数据加密和使用自己的加密密钥：

https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql?tabs=azure-portal#manage-transparent-data-encryption

**Azure 安全中心监视**：是

**责任**：客户

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9：记录对关键 Azure 资源的更改并对此类更改发出警报

**指南**：将 Azure Monitor 与 Azure 活动日志结合使用，为 Azure SQL 数据库的生产实例以及其他关键或相关资源的发生更改创建警报。

如何针对 Azure 活动日志事件创建警报： 

https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

**Azure 安全中心监视**：是

**责任**：客户

## <a name="vulnerability-management"></a>漏洞管理

有关详细信息，请参阅[安全控制：漏洞管理。](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management)

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1：运行自动漏洞扫描工具

**指南**：为 Azure SQL 数据库启用高级数据安全，并根据 Azure 安全中心的建议，在服务器上执行漏洞评估。

如何对 Azure SQL 数据库运行漏洞评估：

https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment

如何启用高级数据安全：

https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security

如何实施 Azure 安全中心漏洞评估建议： 

https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations

**Azure 安全中心监视**：是

责任：客户

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5.2：部署自动操作系统修补管理解决方案

**指南**：不适用；此建议适用于计算资源。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5.3：部署第三方自动软件修补管理解决方案

**指南**：不适用;此基准用于计算资源。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4：比较连续进行的漏洞扫描

**指南**：对 Azure SQL 数据库实例启用定期定期扫描;这会将漏洞评估配置为每周自动运行一次数据库扫描。 扫描结果摘要会发送到你提供的电子邮件地址。 比较结果以验证是否已修正了漏洞。

如何在 Azure 安全中心导出漏洞评估报告：

https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment#implementing-vulnerability-assessment

**Azure 安全中心监视**：不适用

责任：客户

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5：使用风险评级过程来确定已发现漏洞的修正措施的优先级

**指导**：使用 Azure 安全中心提供的默认风险评级（安全功能分数）。

了解 Azure 安全中心安全功能分数： 

https://docs.microsoft.com/azure/security-center/security-center-secure-score

**Azure 安全中心监视**：是

**责任**：客户

## <a name="inventory-and-asset-management"></a>清单和资产管理

有关详细信息，请参阅[安全控制：清单和资产管理](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management)。

### <a name="61-use-azure-asset-discovery"></a>6.1：使用 Azure 资产发现

**指南**：使用 Azure 资源关系图查询并发现订阅中的所有资源（包括 Azure SQL 数据库）。  确保你在租户中拥有适当的（读取）权限，并且可以枚举所有 Azure 订阅，以及订阅中的资源。

尽管可以通过 Resource Graph 发现经典 Azure 资源，但我们强烈建议你今后还是创建并使用 Azure 资源管理器资源。

如何使用 Azure Resource Graph 创建查询： https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

如何查看 Azure 订阅： https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0

了解 Azure RBAC： https://docs.microsoft.com/azure/role-based-access-control/overview

**Azure 安全中心监视**：不适用

责任：客户

### <a name="62-maintain-asset-metadata"></a>6.2：维护资产元数据

**指导**：将标记应用到 Azure资源，以便有条理地将元数据组织成某种分类。

如何创建和使用标记：

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="63-delete-unauthorized-azure-resources"></a>6.3：删除未经授权的 Azure 资源

**指导**：在适用的情况下，请使用标记、管理组和单独的订阅来组织和跟踪资产。 定期核对清单，确保及时地从订阅中删除未经授权的资源。

如何创建其他 Azure 订阅：

https://docs.microsoft.com/azure/billing/billing-create-subscription

如何创建管理组：

https://docs.microsoft.com/azure/governance/management-groups/create

如何创建和使用标记：

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="64-maintain-an-inventory-of-approved-azure-resources-and-software-titles"></a>6.4：维护已批准的 Azure 资源和软件标题的清单

**指导**：定义已批准的 Azure 资源以及计算资源的已批准软件的列表

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5：监视未批准的 Azure 资源

**指南**：在 Azure Policy 中使用以下内置策略定义，对可以在客户订阅中创建的资源类型施加限制：

- 不允许的资源类型

- 允许的资源类型

使用 Azure Resource Graph 查询/发现订阅中的资源。 确保环境中存在的所有 Azure 资源已获得批准。

如何配置和管理 Azure Policy： https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

如何使用 Azure Graph 创建查询： https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

Azure 安全中心监视：不适用

责任：客户

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6：监视计算资源中未批准的软件应用程序

**指南**：不适用；此建议适用于计算资源。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7：删除未批准的 Azure 资源和软件应用程序

**指南**：不适用；此建议适用于计算资源。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="68-use-only-approved-applications"></a>6.8：仅使用已批准的应用程序

**指南**：不适用；此建议适用于计算资源。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="69-use-only-approved-azure-services"></a>6.9：仅使用已批准的 Azure 服务

**指南**：使用 Azure 策略对可使用以下内置策略定义在客户订阅中创建的资源类型进行限制：

- 不允许的资源类型

- 允许的资源类型

使用 Azure Resource Graph 查询/发现订阅中的资源。 确保环境中存在的所有 Azure 资源已获得批准。

如何配置和管理 Azure Policy： https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

如何使用 Azure Policy 拒绝特定的资源类型： https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="610-implement-approved-application-list"></a>6.10：实施已批准的应用程序列表

**指南**：不适用;此建议适用于在计算资源上运行的应用程序。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="611-limit-users-ability-to-interact-with-azure-resources-manager-via-scripts"></a>6.11：限制用户通过脚本来与 Azure 资源管理器交互的功能

**指导**：通过对“Microsoft Azure 管理”应用配置“阻止访问”，使用 Azure 条件访问来限制用户与 Azure 资源管理器交互的功能。

如何配置条件访问以阻止访问 Azure 资源管理器： https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6.12：限制用户在计算资源中执行脚本的功能

**指南**：不适用；此建议适用于计算资源。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13：以物理或逻辑方式隔离高风险应用程序

**指南**：不适用;此建议适用于承载桌面或 web 应用程序的应用服务或计算资源。

**Azure 安全中心监视**：不适用

**责任**：不适用

## <a name="secure-configuration"></a>安全配置

有关详细信息，请参阅[安全控制：安全配置](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration)。

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1：为所有 Azure 资源建立安全配置

**指南**：对 Azure SQL 数据库使用 azure 策略或 Azure 安全中心建议来维护所有 azure 资源的安全配置。

如何配置和管理 Azure Policy：

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure 安全中心监视**：是

**责任**：客户

### <a name="72-establish-secure-operating-system-configurations"></a>7.2：建立安全的操作系统配置

**指南**：不适用;此建议用于计算资源。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3：维护安全的 Azure 资源配置

**指南**：使用 Azure Policy [拒绝] 和 [不存在时部署] 在 Azure 资源中强制实施安全设置。

如何配置和管理 Azure Policy：

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

了解 Azure Policy 效应：

https://docs.microsoft.com/azure/governance/policy/concepts/effects

**Azure 安全中心监视**：不适用

责任：客户

### <a name="74-maintain-secure-operating-system-configurations"></a>7.4：维护安全的操作系统配置

**指南**：不适用；此建议适用于计算资源。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5：安全存储 Azure 资源的配置

**指导**：如果使用自定义的 Azure Policy 定义，请使用 Azure DevOps 或 Azure Repos 安全地存储和管理代码。

如何在 Azure DevOps 中存储代码：

https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops

Azure Repos 文档：

https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="76-securely-store-custom-operating-system-images"></a>7.6：安全存储自定义操作系统映像

**指南**：不适用；此建议适用于计算资源。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="77-deploy-system-configuration-management-tools"></a>7.7：部署系统配置管理工具

**指南**：使用 "sql-dmo" 命名空间中的 Azure 策略别名创建自定义策略，以对系统配置进行警报、审核和强制执行。 另外，开发一个用于管理策略例外的流程和管道。

如何配置和管理 Azure Policy：

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure 安全中心监视**：不适用

责任：客户

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7.8：为操作系统部署系统配置管理工具

**指南**：不适用；此建议适用于计算资源。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7.9：为 Azure 服务实施自动配置监视

**指南**：利用 Azure 安全中心对 Azure SQL 数据库执行基线扫描。

如何在 Azure 安全中心修正建议： 

https://docs.microsoft.com/azure/security-center/security-center-sql-service-recommendations

**Azure 安全中心监视**：是

**责任**：客户

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10：为操作系统实施自动配置监视

**指南**：不适用；此建议适用于计算资源。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="711-manage-azure-secrets-securely"></a>7.11：安全管理 Azure 机密

**指南**：使用 Azure Key Vault 存储 Azure SQL Database 透明数据加密的加密密钥（TDE）。

如何保护 Azure SQL 数据库中存储的敏感数据并将加密密钥存储在 Azure Key Vault 中：

https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="712-manage-identities-securely-and-automatically"></a>7.12：安全自动管理标识

**指南**：使用托管标识为 Azure 服务提供 AZURE ACTIVE DIRECTORY （AAD）中的自动托管标识。 通过托管标识，你可以向支持 AAD 身份验证的任何服务（包括 Azure Key Vault）进行身份验证，而无需在代码中包含任何凭据。

教程：使用 Windows VM 系统分配的托管标识访问 Azure SQL：

https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-sql

如何配置托管标识： 

https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm

**Azure 安全中心监视**：不适用

责任：客户

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13：消除意外的凭据透露

**指南**：通过实现凭据扫描器来识别代码中的凭据。 凭据扫描程序还会建议将发现的凭据转移到更安全的位置，例如 Azure Key Vault。

如何设置凭据扫描器： https://secdevtools.azurewebsites.net/helpcredscan.html

**Azure 安全中心监视**：不适用

责任：客户

## <a name="malware-defense"></a>恶意软件防护

有关详细信息，请参阅[安全控制：恶意软件防护](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense)。

### <a name="81-use-centrally-managed-anti-malware-software"></a>8.1：使用集中管理的反恶意软件

**指导**：不适用；此建议适用于计算资源。 Microsoft 处理底层平台的反恶意软件。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2：预先扫描要上传到非计算 Azure 资源的文件

**指南**：在支持 Azure 服务的基础主机（例如 Azure App Service）上启用了 Microsoft 反恶意软件，但它不会在客户内容上运行。

预先扫描正在上载到非计算 Azure 资源的任何内容，例如应用服务、Data Lake Storage、Blob 存储、Azure SQL 数据库等。Microsoft 无法访问这些实例中的数据。

了解适用于 Azure 云服务和虚拟机的 Microsoft 反恶意软件：https://docs.microsoft.com/azure/security/fundamentals/antimalware

**Azure 安全中心监视**：不适用

责任：客户

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>步骤 8.3：确保反恶意软件和签名已更新

**指南**：不适用；此建议适用于计算资源。 Microsoft 处理底层平台的反恶意软件。

**Azure 安全中心监视**：不适用

**责任**：不适用

## <a name="data-recovery"></a>数据恢复

有关详细信息，请参阅[安全控制：数据恢复](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery)。

### <a name="91-ensure-regular-automated-back-ups"></a>9.1：确保定期执行自动备份

**指南**：为了防止业务丢失数据，Azure SQL 数据库每周自动创建完整数据库备份，每隔12小时自动创建一次差异数据库备份，每 5-10 分钟自动创建一次事务日志备份。 所有服务层级的备份在 RA-GRS 存储中存储至少 7 天。 对于时间点还原，所有服务层级（“基本”除外）都支持可配置的备份保留期，最长为 35 天。

为了满足不同的符合性要求，你可以为每周、每月和/或每年备份选择不同的保留期。 存储消耗量取决于所选的备份频率和保留期。

了解 Azure SQL 数据库的备份和业务连续性：

https://docs.microsoft.com/azure/sql-database/sql-database-business-continuity

**Azure 安全中心监视**：是

**责任**：共享

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2：执行完整的系统备份并备份所有客户托管密钥

**指南**： Azure SQL 数据库自动创建保留7到35天之间的数据库备份，并使用 Azure 读取访问异地冗余存储（GRS）来确保即使数据中心不可用，也会保留这些备份。 这些备份是自动创建的。 如果需要，为 Azure SQL 数据库启用长期冗余备份。

如果使用客户托管密钥进行透明数据加密，请确保密钥正在备份。

了解 Azure SQL 数据库中的备份：

https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups?tabs=single-database

如何在 Azure 中备份 key vault 密钥：

https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0

**Azure 安全中心监视**：是

**责任**：客户

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3：验证所有备份，包括客户管理的密钥

**指南**：确保能够定期执行 Azure 备份中的内容数据还原。 如有必要，请测试将内容还原到隔离的 VLAN 中。 测试已备份客户托管密钥的还原。

如何在 Azure 中还原 Key Vault 密钥：

https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0

如何使用时间点还原恢复 Azure SQL 数据库备份：

https://docs.microsoft.com/azure/sql-database/sql-database-recovery-using-backups#point-in-time-restore

**Azure 安全中心监视**：不适用

责任：客户

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4：确保保护备份和客户管理的密钥

**指南**：在 Azure Key Vault 中启用软删除，以防止意外或恶意删除密钥。

如何在 Key Vault 中启用软删除：

https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal

**Azure 安全中心监视**：是

责任：客户

## <a name="incident-response"></a>事件响应

有关详细信息，请参阅[安全控制：事件响应](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response)。

### <a name="101-create-an-incident-response-guide"></a>10.1：创建事件响应指导

**指导**：确保在书面的事件响应计划中定义人员职责，以及事件处理/管理的各个阶段。

如何在 Azure 安全中心配置工作流自动化：

https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide

**Azure 安全中心监视**：不适用

责任：客户

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2：创建事件评分和优先级设定过程

**指导**：安全中心将为警报分配严重性来帮助你确定每条警报的处理优先顺序，以便在资源泄密时可以立即采取措施。 严重性取决于安全中心在发出警报时所依据的检测结果和分析结果的置信度，以及导致发出警报的活动的恶意企图的置信度。

Azure 安全中心中的安全警报：https://docs.microsoft.com/azure/security-center/security-center-alerts-overview

**Azure 安全中心监视**：是

**责任**：客户

### <a name="103-test-security-response-procedures"></a>10.3：测试安全响应过程

**指导**：定期执行演练来测试系统的事件响应功能。 识别弱点和差距，并根据需要修改计划。

可以参考 NIST 发布：针对 IT 计划和功能的测试、培训和试验计划指南：

https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf

**Azure 安全中心监视**：不适用

责任：客户

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4：提供安全事件联系人详细信息，并针对安全事件配置警报通知

**指南**：如果 Microsoft 安全响应中心（MSRC）发现你的数据被非法或未授权的一方访问，microsoft 将使用安全事件联系人信息与你联系。

如何设置 Azure 安全中心安全联系人：

https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details

**Azure 安全中心监视**：是

**责任**：客户

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5：将安全警报整合到事件响应系统中

**指导**：使用连续导出功能导出 Azure 安全中心警报和建议。 使用连续导出可以手动导出或者持续导出警报和建议。 可以使用 Azure 安全中心数据连接器将警报流式传输到 Sentinel。

如何配置连续导出：

https://docs.microsoft.com/azure/security-center/continuous-export

如何将警报流式传输到 Azure Sentinel：

https://docs.microsoft.com/azure/sentinel/connect-azure-security-center

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="106-automate-the-response-to-security-alerts"></a>10.6：自动响应安全警报

**指导**：使用 Azure 安全中心内的工作流自动化功能可以通过“逻辑应用”针对安全警报和建议自动触发响应。

如何配置工作流自动化和逻辑应用：

https://docs.microsoft.com/azure/security-center/workflow-automation

**Azure 安全中心监视**：目前不可用

**责任**：客户

## <a name="penetration-tests-and-red-team-exercises"></a>渗透测试和红队练习

有关详细信息，请参阅[安全控制：渗透测试和红队演练](https://docs.microsoft.com/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises)。

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings-within-60-days"></a>11.1：定期对 Azure 资源执行渗透测试，确保在 60 天内修正所有发现的关键安全问题

**指导**：请遵循 Microsoft 互动规则，确保你的渗透测试不违反 Microsoft 政策：

https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1.

在以下网页中可以找到有关 Microsoft 红队演练策略和执行，以及针对 Microsoft 托管云基础结构、服务和应用程序执行现场渗透测试的详细信息： https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e

**Azure 安全中心监视**：不适用

**责任**：共享

## <a name="next-steps"></a>后续步骤

- 参阅 [Azure 安全基准](https://docs.microsoft.com/azure/security/benchmarks/overview)
- 详细了解 [Azure 安全基线](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview)
