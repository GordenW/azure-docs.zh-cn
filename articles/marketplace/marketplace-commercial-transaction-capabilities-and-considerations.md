---
title: Microsoft 商业市场交易功能
description: 本文介绍商业市场交易选项的定价、计费、开票和支出注意事项。
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 07/22/2020
ms.author: mingshen
author: mingshen-ms
ms.openlocfilehash: e2ce35158e8f9a3a2ad9da2b3d67a3f35d5a8c80
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/31/2020
ms.locfileid: "87503559"
---
# <a name="commercial-marketplace-transact-capabilities"></a>商业市场交易功能

## <a name="transactions-by-publishing-option"></a>按发布选项统计的交易量

发布者或 Microsoft 负责管理商业市场中的产品/服务的软件许可证交易。 你为产品/服务选择的发布选项将决定交易的管理方。 请参阅[确定发布选项](./determine-your-listing-type.md#choose-a-call-to-action)，了解每个发布选项的可用性和说明。

### <a name="list-trial-and-byol-publishing-options"></a>列表、试用和 BYOL 发布选项

具有现有商业功能的发布者可以选择列表、试用和自带许可 (BYOL) 发布选项来实现促销目的并获得用户。 使用这些选项，Microsoft 不直接参与发布者的软件许可证交易，因此没有相关的交易费。 发布者负责支持软件许可证交易的各个方面，包括但不限于：订单、履行、计量、计费、开具发票、付款和费用收集。 借助列表和试用发布选项，发布者保留 100% 的收集自客户的发布者软件许可费。

### <a name="transact-publishing-option"></a>交易发布选项

交易发布选项利用 Microsoft 商业功能，并提供从发现和评估到购买和实施的端到端体验。 针对现有的 Microsoft 订阅或信用卡对交易产品/服务进行计费，从而使 Microsoft 可以代表发布者来主持云市场交易。

在合作伙伴中心创建新产品/服务时，将要选择交易选项。 在“设置详细信息”下的“产品/服务设置”页上，选择“是，我想通过 Microsoft 进行销售并让 Microsoft 代表我主持交易。 ” 仅在你的产品/服务类型适合交易时，才会显示此选项。

## <a name="transact-overview"></a>交易概述

使用交易发布选项时，Microsoft 支持向客户的 Azure 订阅销售第三方软件并部署部分类型的产品/服务。 在选择计费模型和产品/服务类型时，发布者必须考虑基础结构费用，以及发布者自己的软件许可费用。

目前以下产品/服务类型支持交易发布选项：

- 虚拟机
- Azure 应用程序
- SaaS 应用程序

### <a name="billing-infrastructure-costs"></a>计费基础结构成本

对于**虚拟机**和**azure 应用程序**，azure 基础结构使用费用会按客户的 azure 订阅计费。 在客户的发票上，基础结构使用费与软件提供商的许可费分开计价和显示。

对于 SaaS 应用，发布者必须将 Azure 基础结构使用费和软件许可费视为单一费用项。  它表示为客户的固定费用。 Azure 基础结构使用情况由合作伙伴直接管理并对其计费。 客户将无法看到实际的基础结构使用费。 发布者通常选择将 Azure 基础结构使用费捆绑到其软件许可证定价中。 不计量软件许可费，它也不基于消耗量。

## <a name="transact-billing-models"></a>交易计费模式

根据使用的交易选项，软件许可证费用如下所示：

- **免费** - 不收取软件许可证费用。
- **自带许可 (BYOL)** - 直接在发布者和客户之间管理所有适用的软件许可证费用。 Microsoft 仅收取 Azure 基础结构使用费。 这仅适用于虚拟机和 Azure 应用程序。
- **即用即付** - 软件许可证费用将根据所用的 Azure 基础结构显示为每小时每个核心 (vCPU) 定价费率。 这仅适用于虚拟机和 Azure 应用程序。
- **订阅定价** - 软件许可证费用显示为每月或每年的周期性费用，采用固定费率或按席位收取。 这适用于 SaaS 应用（每月或每年）和 Azure 应用程序-托管应用（每月）。
- **免费软件试用版** - 免费使用软件许可证 30 天或 90 天。

### <a name="free-and-bring-your-own-license-byol-pricing"></a>免费和自带许可 (BYOL) 定价

发布免费或自带许可的交易产品/服务时，Microsoft 在促成有关软件许可证费用的销售交易中不扮演任何角色。 如列表和试用发布选项一样，发布者保留 100% 的软件许可证费用。

### <a name="pay-as-you-go-and-subscription-site-based-pricing"></a>即用即付和订阅（基于站点）定价

发布即用即付或订阅型交易产品/服务时，Microsoft 提供技术和服务用于处理软件许可证的购买、退货和退款。 在这种情况下，发布者出于上述目的授权 Microsoft 充当代理人。 发布者允许 Microsoft 促成软件许可交易，同时保留自身的卖家、提供商、分销商和许可方的称号。

Microsoft 使客户能在 Microsoft 商业市场和你的最终用户许可协议的条款和条件约束下订购、许可和使用你的软件。 创建产品/服务时，必须提供自己的最终用户许可协议或选择[标准协定](./standard-contract.md)。

### <a name="free-software-trials"></a>免费软件试用

在交易发布方案中，可免费提供软件许可证 30 天或 90 天。 此折扣功能不包括使用合作伙伴解决方案引致的 Azure 基础结构使用费用。

### <a name="private-offers"></a>专用产品/服务

除了使用产品/服务类型和计费模型将产品/服务转化为收益外，还可以交易专用产品/服务，采用经过协商的、特定于交易的定价或者自定义配置。 专用产品/服务受全部三个交易发布选项的支持。

此选项允许采用比公开提供的产品/服务更高或更低的定价。 专用产品/服务可用于提供产品/服务折扣或为其添加奖励金。 可在产品/服务级别将其 Azure 订阅添加到允许列表，向一个或多个客户提供专用产品/服务。

### <a name="examples"></a>示例

**即用即付** 

即用即付的成本构成如下：

|你的许可证费用  | 每小时 $1.00   |
|---------|---------|
|Azure 使用费用（D1/1 核）    |   每小时 $0.14     |
|*由 Microsoft 向客户收费*    |  *每小时 $1.14*       |
||

在此场景中，Microsoft 按每小时 $1.14 收取所发布的 VM 映像的使用费用。

|Microsoft 收费  | 每小时 $1.14  |
|---------|---------|
|Microsoft 将许可证费用的 80% 支付给你|   每小时 $0.80     |
|Microsoft 保留许可证费用的 20%  |  每小时 $0.20       |
|Microsoft 保留 100% 的 Azure 使用费 | 每小时 $0.14 |
||

**自带许可 (BYOL)**

BYOL 的成本构成如下：

|你的许可证费用  | 许可证费用由你协商确定和收取  |
|---------|---------|
|Azure 使用费用（D1/1 核）    |   每小时 $0.14     |
|*由 Microsoft 向客户收费*    |  *每小时 $0.14*       |
||

在此场景中，Microsoft 按每小时 $0.14 收取所发布的 VM 映像的使用费用。

|Microsoft 收费  | 每小时 $0.14  |
|---------|---------|
|Microsoft 保留 Azure 使用费用    |   每小时 $0.14     |
|Microsoft 保留许可证费用的 0%   |  每小时 $0.00       |
||

**SaaS 应用订阅**

此选项必须配置为通过 Microsoft 进行销售，并且可以在每月或每年的基础上收取固定费用或按用户收费。 如果为 SaaS 产品/服务启用“通过 Microsoft 销售”选项，则成本构成如下：

| 你的许可证费用       | 100.00 美元/月  |
|--------------|---------|
| Azure 使用费用（D1/1 核）    | 直接向发布者而不是客户收费 |
| *由 Microsoft 向客户收费*    |  100.00 美元/月（发布者必须将所有产生的或者转嫁的基础结构费用纳入许可证费用中）  |
||

在这种情况下，Microsoft 向你收取 100.00 美元的软件许可证费用，并向发布者支付 80.00 美元。

2019 年 5 月到 2020 年 6 月，符合减少市场服务费资格的合作伙伴所需支付的 SaaS 产品/服务交易费将减少。

在此方案中，Microsoft 收取 100.00 美元的软件许可证费用，并向发布者支付 90.00 美元：

|Microsoft 收费  | 100.00 美元/月  |
|---------|---------|
|Microsoft 将许可证费用的 80% 支付给你 <br> \* 对于所有符合资格的 SaaS 应用，Microsoft 将为你垫付许可证费用的 90%   |   80.00 美元/月 <br> \* 90.00 美元/月    |
|Microsoft 保留许可证费用的 20% <br> \* 对于所有符合资格的 SaaS 应用，Microsoft 保留许可证费用的 10%。  |  20.00 美元/月 <br> \* 10.00 美元     |

对于在商业市场上发布的特定产品/服务，Microsoft 会将其 Marketplace 服务费用从20% 降低到10% （如 Microsoft 发布者协议中所述）。 对于你的产品/服务，你的产品/服务必须已由 Microsoft 指定为 Azure IP 共同销售 incentivized。 在每个日历月结束之前，资格必须至少满足五（5）个工作日，才能获得该月降低的 Marketplace 服务费用。 降低的 Marketplace 服务费用适用于 Azure IP 共同销售 incentivized SaaS、Vm、托管应用以及通过商业市场提供的任何其他合格事务 IaaS 产品/服务。

### <a name="customer-invoicing-payment-billing-and-collections"></a>客户开具发票、付款、计费和费用收集

**开具发票与付款** - 可以使用客户的首选发票开具方法来递送订阅或 PAYGO 软件许可证费用。

**企业协议** - 如果客户的首选发票开具方法是 Microsoft 企业协议，将使用此方法以逐项列出费用的形式对软件许可证进行计费，与任何特定于 Azure 的使用费分开。

**信用卡和每月发票** - 客户还可以使用信用卡和每月发票进行付款。 在这种情况下，将像企业协议方案一样对软件许可证进行计费：采用逐项列出费用的形式并与任何特定于 Azure 的使用费分开。

**免费信用额度和货币承诺** - 某些客户选择通过企业协议中的货币承诺向 Azure 预付款，或者已向他们提供免费信用额度用于 Azure。 尽管可以这些信用额度可用于支付 Azure 使用情况，但它们不能用于支付发布者软件许可证费用。

**计费和收款** - 发布者软件许可证计费使用客户所选的发票开票方法进行显示，并遵循开票时间线。 将对没有企业协议的客户就市场软件许可证按月计费。 通过每季度显示的发票对具有企业协议的客户按月计费。

选择订阅或即用即付定价模型时，Microsoft 将充当发布者的代理，并且负责计费、付款和费用收集的所有方面。

### <a name="publisher-payout-and-reporting"></a>发布者付款和报表

Microsoft 作为代理收集的所有软件许可费用将收取 20% 的手续费（另行指定的除外），且会在发布者付款时扣除。

客户通常使用企业协议或支持信用卡的即用即付协议进行购买。 协议类型确定计费、发票、费用收集和付款时间。

>[!NOTE]
>在合作伙伴中心的“分析”部分可以找到交易发布选项的所有报告和见解。

#### <a name="billing-questions-and-support"></a>计费问题和支持

有关详细信息和法律策略，请参阅[发布者协议](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4xqkx)（请参见合作伙伴中心）。

若在计费方面遇到问题且需要帮助，请联系[商业市场发布者支持](https://aka.ms/marketplacepublishersupport)。

## <a name="transact-requirements"></a>交易要求

本部分介绍不同产品/服务类型的交易要求。

### <a name="requirements-for-all-offer-types"></a>所有产品/服务类型的要求

- 无论产品/服务采用怎样的定价模型，交易发布选项都需要使用 Microsoft 帐户和财务信息。
- 必需的财务信息包括支出帐户和计税配置文件。

有关设置这些帐户的详细信息，请参阅[在合作伙伴中心管理商业 marketplace 帐户](partner-center-portal/manage-account.md)。

### <a name="requirements-for-specific-offer-types"></a>特定产品/服务类型的要求

交易发布选项仅适用于以下市场产品/服务类型：

- **虚拟机**–从免费、自带许可证或即用即付定价模型中进行选择，并以服务级别定义的计划形式提供。 在客户的 Azure 帐单上，Microsoft 将发布者软件许可证费用与隐含的 Azure 基础结构费用分开显示。 Azure 基础结构费用取决于发布者软件使用情况。

- **Azure 应用程序：解决方案模板或托管应用**–必须预配一个或多个虚拟机，并提取虚拟机定价的总和。 对于单个计划的托管应用，可以选择固定费率每月订阅作为定价模型，而不是虚拟机定价。 在部分情况下，Azure 基础结构使用费将与软件许可证费用分开，单独传递给客户，但位于相同的账单上。 但是，如果你配置了托管应用产品/服务以实现 ISV 基础结构费用，则会向发布者计费 Azure 资源，并且客户会收到包含基础结构、软件许可证和管理服务成本的固定费用。

- **Saas 应用程序**-必须是多租户解决方案，使用[Azure Active Directory](https://azure.microsoft.com/services/active-directory/)进行身份验证，并与[SaaS 履单 api](partner-center-portal/pc-saas-fulfillment-api-v2.md)集成。 Azure 基础结构的使用情况是直接对你（合作伙伴）进行管理和计费的，因此，你必须将 Azure 基础结构使用费用和软件许可费用视为单个成本项。 有关详细指南，请参阅[在商业应用商店中创建新的 SaaS 产品/服务](partner-center-portal/create-new-saas-offer.md)。

## <a name="next-steps"></a>后续步骤

- 查看发布选项中按套餐类型划分的资格要求，以便最终确定套餐的选择和配置。
- 查看按店面划分的发布模式，例如，解决方案如何映射到套餐类型和配置。
