---
title: 限制和配置
description: Azure 逻辑应用的服务限制（例如持续时间、吞吐量和容量）以及配置值（例如允许的 IP 地址）
services: logic-apps
ms.suite: integration
ms.reviewer: jonfan, logicappspm
ms.topic: article
ms.date: 08/03/2020
ms.openlocfilehash: 03bd97e487e28695133d7d69a71c0dbc90d5d605
ms.sourcegitcommit: 97a0d868b9d36072ec5e872b3c77fa33b9ce7194
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/04/2020
ms.locfileid: "87563970"
---
# <a name="limits-and-configuration-information-for-azure-logic-apps"></a>Azure 逻辑应用的限制和配置信息

本文介绍使用 Azure 逻辑应用创建和运行自动化工作流的限制和配置的详细信息。 对于 Power Automate，请参阅 [Power Automate 中的限制和配置](/flow/limits-and-config)。

<a name="definition-limits"></a>

## <a name="definition-limits"></a>定义限制

下面是针对单个逻辑应用定义的限制：

| 名称 | 限制 | 注释 |
| ---- | ----- | ----- |
| 每个工作流的操作数 | 500 | 要对此限制进行扩展，可根据需要添加嵌套工作流。 |
| 操作的允许嵌套深度 | 8 | 要对此限制进行扩展，可根据需要添加嵌套工作流。 |
| 每个订阅每个区域的工作流数 | 1,000 | |
| 每个工作流的触发数 | 10 个 | 在代码视图中工作时，而不是在设计器中工作时 |
| Switch 作用域事例限制 | 25 | |
| 每个工作流的变量数 | 250 | |
| 每个表达式的字符数 | 8,192 | |
| `trackedProperties` 的最大大小 | 16,000 个字符 |
| `action` 或 `trigger` 的名称 | 80 个字符 | |
| `description` 的长度 | 256 个字符 | |
| `parameters` 最大值 | 50 | |
| `outputs` 最大值 | 10 个 | |

<a name="run-duration-retention-limits"></a>

## <a name="run-duration-and-retention-limits"></a>运行持续时间和保留期限制

下面是针对单个逻辑应用运行的限制：

| 名称 | 多租户限制 | 集成服务环境限制 | 注释 |
|------|--------------------|---------------------------------------|-------|
| 运行持续时间 | 90 天 | 366 天 | 运行持续时间通过两部分计算得到：运行的开始时间，以及[运行历史记录保留天数](#change-duration)工作流设置在开始时间设置的限制。 <p><p>若要更改默认限制（90 天），请参阅[更改运行持续时间](#change-duration)。 |
| 存储中的运行保留期 | 90 天 | 366 天 | 运行保留期通过两部分计算得到：运行的开始时间，以及[运行历史记录保留天数](#change-retention)工作流设置在当前时间设置的限制。 无论运行是完成还是超时，保留期计算始终使用运行开始时间。 当运行的持续时间超过当前保留期限制时，将从运行历史记录中删除该运行。 <p><p>如果更改此设置，将始终使用当前限制来计算保留期，而不考虑以前的限制。 例如，如果将保留期限制从 90 天减至 30 天，则会从运行历史记录中删除保留了 60 天的运行。 如果将保留期从 30 天增至 60 天，则已经保留了 20 天之久的运行将在运行历史记录中继续保留 40 天。 <p><p>若要更改默认限制（90 天），请参阅[更改存储中的运行保留期](#change-retention)。 |
| 最小重复间隔 | 1 秒 | 1 秒 ||
| 最大重复间隔 | 500 天 | 500 天 ||
|||||

<a name="change-duration"></a>
<a name="change-retention"></a>

### <a name="change-run-duration-and-run-retention-in-storage"></a>更改存储中的运行持续时间和运行保留期

若要更改存储中的运行持续时间和运行保留期的默认限制，请执行以下步骤。 若要提高最大限制，[请联系逻辑应用团队](mailto://logicappsemail@microsoft.com)，就你的要求获取帮助。

> [!NOTE]
> 对于多租户 Azure 中的逻辑应用，90 天默认限制与最大限制相同。 只能减小此值。
> 对于集成服务环境中的逻辑应用，可以降低或提高 90 天默认限制。

1. 转到 [Azure 门户](https://portal.azure.com)。 在门户搜索框中，找到并选择“逻辑应用”。

1. 在逻辑应用设计器中选择并打开你的逻辑应用。

1. 在逻辑应用的菜单中选择“工作流设置”。

1. 在“运行时选项”下，从“运行历史记录保留天数”列表中选择“自定义”  。

1. 拖动滑块更改所需的天数。

1. 完成后，在“工作流设置”工具栏上选择“保存”。 

<a name="looping-debatching-limits"></a>

## <a name="concurrency-looping-and-debatching-limits"></a>并发、循环和拆分限制

下面是针对单个逻辑应用运行的限制：

| 名称 | 限制 | 注释 |
| ---- | ----- | ----- |
| 触发器并发 | - 若并发控制已关闭，则无限制 <p><p>- 在启用并发控制时，默认限制为 25（启用并发后无法撤消）。 可以将默认值更改为介于 1 与 50（含）之间的值。 | 此限制描述可以在同一时间或并行运行的逻辑应用实例的最大数。 <p><p>**注意**：启用并发后，[解除数组批处理](../logic-apps/logic-apps-workflow-actions-triggers.md#split-on-debatch)的 SplitOn 限制会降低到 100 个项。 <p><p>若要将默认限制更改为介于 1 到 50 之间（含）的值，请参阅[更改触发器并发限制](../logic-apps/logic-apps-workflow-actions-triggers.md#change-trigger-concurrency)或[按顺序触发实例](../logic-apps/logic-apps-workflow-actions-triggers.md#sequential-trigger)。 |
| 最大等待运行数 | - 未启用并发时，最小等待运行数是 1，最大数目是 50。 <p><p>- 启用并发时，最小等待运行数是 10 加上并发运行（触发器并发）数。 可以将最大数更改为多达 100 个（含）。 | 此限制描述当逻辑应用已在运行最大数量并发实例时，可等待运行的最大逻辑应用实例数。 <p><p>若要更改此默认限制，请参阅[更改等待的运行限制](../logic-apps/logic-apps-workflow-actions-triggers.md#change-waiting-runs)。 |
| Foreach 数组项 | 100,000 | 此限制描述“for each”循环可以处理的最大数组项数。 <p><p>可以使用[查询操作](logic-apps-perform-data-operations.md#filter-array-action)筛选更大数组。 |
| Foreach 并发 | 20 是并发控制关闭时的默认限制。 可以将默认值更改为介于 1 与 50（含）之间的值。 | 此限制是可同时或并行运行的最大“for each”循环迭代数。 <p><p>若要将默认限制更改为介于 1 到 50 之间（含）的值，请参阅[更改“for each”并发限制](../logic-apps/logic-apps-workflow-actions-triggers.md#change-for-each-concurrency)或[按顺序运行“for each”循环](../logic-apps/logic-apps-workflow-actions-triggers.md#sequential-for-each)。 |
| SplitOn 项 | - 未启用触发器并发时为 100,000 <p><p>- 启用触发器并发时为 100 | 对于返回数组的触发器，可指定一个表达式，它使用[将数组项拆分或解除批处理到多个工作流实例](../logic-apps/logic-apps-workflow-actions-triggers.md#split-on-debatch)进行处理的“SplitOn”属性，而不是使用“Foreach”循环。 此表达式引用要用于为每个数组项创建和运行工作流实例的数组。 <p><p>**注意**：启用并发后，SplitOn 限制会降低到 100 个项。 |
| Until 迭代 | - 默认值：60 <p><p>- 最大值：5,000 | |
||||

<a name="throughput-limits"></a>

## <a name="throughput-limits"></a>吞吐量限制

下面是针对单个逻辑应用定义的限制：

### <a name="multi-tenant-logic-apps-service"></a>多租户逻辑应用服务

| 名称 | 限制 | 注释 |
| ---- | ----- | ----- |
| 操作：每 5 分钟执行的次数 | 默认限制为 100,000，最大限制为 300,000。 | 若要更改此默认限制，请参阅处于预览阶段的[在“高吞吐量”模式下运行逻辑应用](../logic-apps/logic-apps-workflow-actions-triggers.md#run-high-throughput-mode)。 或者，你可根据需要在多个逻辑应用之间分配工作负荷。 |
| 操作：并发出站调用 | ~2,500 | 你可减少并发请求数，或根据需要减少持续时间。 |
| 运行时终结点：并发入站调用 | ~1,000 | 你可减少并发请求数，或根据需要减少持续时间。 |
| 运行时终结点：每 5 分钟读取调用  | 60,000 | 此限制适用于从逻辑应用的运行历史记录获取原始输入和输出的调用。 可根据需要在多个应用中分发工作负荷。 |
| 运行时终结点：每 5 分钟调用调用 | 45,000 | 可根据需要在多个应用中分发工作负荷。 |
| 每 5 分钟的内容吞吐量 | 600 MB | 可根据需要在多个应用中分发工作负荷。 |
||||

### <a name="integration-service-environment-ise"></a>集成服务环境 (ISE)

下面是[高级 ISE SKU](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level) 的吞吐量限制：

| “属性” | 限制 | 注释 |
|------|-------|-------|
| 基础单位执行限制 | 当基础结构容量达到 80% 时，系统受到限制 | 每分钟提供约 4,000 次操作执行，每月大约 1.6 亿次操作执行 | |
| 缩放单元执行限制 | 当基础结构容量达到 80% 时，系统受到限制 | 每个缩放单元每分钟可提供约 2,000 次额外的操作执行，每月约 8000 万次额外的操作执行 | |
| 可添加的缩放单元数上限 | 10 | |
||||

若要在正常处理中超过这些限制，或要运行可能超过这些限制的负载测试，请[与逻辑应用团队联系](mailto://logicappsemail@microsoft.com)，获取满足要求的帮助。

> [!NOTE]
> 尚未发布对[开发人员 ISE SKU](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level) 的限制，它没有任何纵向扩展功能，也没有服务级别协议 (SLA)。 此 SKU 仅可用于试验、开发和测试，不可用于生产和性能测试。

<a name="gateway-limits"></a>

## <a name="gateway-limits"></a>网关限制

Azure 逻辑应用支持通过网关执行写入操作（包括插入和更新）。 但是，这些操作存在[有效负载大小限制](/data-integration/gateway/service-gateway-onprem#considerations)。

<a name="request-limits"></a>

## <a name="http-limits"></a>HTTP 限制

下面是单个传出或传入 HTTP 调用的限制：

#### <a name="timeout"></a>超时

某些连接器操作会进行异步调用或侦听 Webhook 请求，因此，这些操作的超时时间可能会长于以下限制。 有关详细信息，请参阅特定连接器的技术详细信息以及[工作流触发器和操作](../logic-apps/logic-apps-workflow-actions-triggers.md#http-action)。

| 名称 | 多租户限制 | 集成服务环境限制 | 注释 |
|------|--------------------|---------------------------------------|-------|
| 出站请求 | 120 秒 <br>（2 分钟） | 240 秒 <br>（4 分钟） | HTTP 触发器发出的调用就是出站请求。 <p><p>**提示**：对于运行时间较长的操作，请使用[异步轮询模式](../logic-apps/logic-apps-create-api-app.md#async-pattern)或 [until 循环](../logic-apps/logic-apps-workflow-actions-triggers.md#until-action)。 在调用其他具有[可调用终结点](logic-apps-http-endpoint.md)的逻辑应用时，若要绕过超时限制，可改用内置的 Azure 逻辑应用操作（可在“内置”下的连接器连接器中找到）。 |
| 入站请求 | 120 秒 <br>（2 分钟） | 240 秒 <br>（4 分钟） | 请求触发器和 Webhook 触发器接收的调用就是入站请求。 <p><p>**注意**：要使原始调用方能够获得响应，则除非以嵌套工作流的形式调用其他逻辑应用，否则必须在限制内完成响应的所有步骤。 有关详细信息，请参阅[调用、触发器或嵌套逻辑应用](../logic-apps/logic-apps-http-endpoint.md)。 |
|||||

<a name="message-size-limits"></a>

#### <a name="message-size"></a>消息大小

| 名称 | 多租户限制 | 集成服务环境限制 | 注释 |
|------|--------------------|---------------------------------------|-------|
| 消息大小 | 100 MB | 200 MB | 若要解决此限制问题，请参阅[使用分块处理大型消息](../logic-apps/logic-apps-handle-large-messages.md)。 但是，某些连接器和 API 可能不支持分块，甚至不支持默认限制。 <p><p>- 连接器（如 AS2、X12 和 EDIFACT）具有自己的 [B2B 消息限制](#b2b-protocol-limits)。 <br>- ISE 连接器使用 ISE 限制，而不是非 ISE 连接器限制。 |
| 使用分块的消息大小 | 1 GB | 5 GB | 此限制适用于本机支持分块或可在其运行时配置中启用分块的操作。 <p><p>如果你使用的是 ISE，则逻辑应用引擎支持此限制，但连接器具有自己的分块限制（不超过引擎限制），例如请参阅 [Azure Blob 存储连接器的 API 参考](/connectors/azureblob/)。 有关分块的详细信息，请参阅[使用分块处理大型消息](../logic-apps/logic-apps-handle-large-messages.md)。 |
|||||

#### <a name="character-limits"></a>字符限制

| 名称 | 注释 |
|------|-------|
| 表达式计算限制 | 131,072 个字符 | `@concat()`、`@base64()`、`@string()` 表达式的长度不能超过此限制。 |
| 请求 URL 字符限制 | 16,384 个字符 |
|||

<a name="retry-policy-limits"></a>

#### <a name="retry-policy"></a>重试策略

| 名称 | 限制 | 注释 |
| ---- | ----- | ----- |
| 重试次数 | 90 | 默认值为 4。 若要更改默认值，请使用[重试策略参数](../logic-apps/logic-apps-workflow-actions-triggers.md)。 |
| 重试最大延迟 | 1 天 | 若要更改默认值，请使用[重试策略参数](../logic-apps/logic-apps-workflow-actions-triggers.md)。 |
| 重试最小延迟 | 5 秒 | 若要更改默认值，请使用[重试策略参数](../logic-apps/logic-apps-workflow-actions-triggers.md)。 |
||||

<a name="authentication-limits"></a>

### <a name="authentication-limits"></a>身份验证限制

如果逻辑应用从使用请求触发器开始，并启用 [Azure Active Directory 开放式身份验证](../active-directory/develop/index.yml) (Azure AD OAuth) 来授权对请求触发器的入站调用，则应遵循以下限制：

| “属性” | 限制 | 注释 |
| ---- | ----- | ----- |
| Azure AD 授权策略 | 5 | |
| 每个授权策略的声明 | 10 | |
||||

<a name="custom-connector-limits"></a>

## <a name="custom-connector-limits"></a>自定义连接器限制

下面介绍对可通过 Web API 创建的自定义连接器的限制。

| 名称 | 多租户限制 | 集成服务环境限制 | 注释 |
|------|--------------------|---------------------------------------|-------|
| 自定义连接器数 | 每个 Azure 订阅 1,000 | 每个 Azure 订阅 1,000 ||
| 自定义连接器的每分钟请求数 | 每分钟每个连接 500 个请求 | 每分钟每个自定义连接器 2,000 个请求** ||
|||

<a name="managed-identity"></a>

## <a name="managed-identities"></a>托管标识

| 名称 | 限制 |
|------|-------|
| 每个逻辑应用的托管标识 | 系统分配的标识或 1 个用户分配的标识 |
| 每个区域的每个 Azure 订阅中具有托管标识的逻辑应用数量 | 1,000 |
|||

<a name="integration-account-limits"></a>

## <a name="integration-account-limits"></a>集成帐户限制

每个 Azure 订阅具有以下集成帐户限制：

* 每个 Azure 区域有一个[免费层](../logic-apps/logic-apps-pricing.md#integration-accounts)集成帐户。 此级别仅适用于 Azure 中的公共区域，例如 "美国西部" 或 "东南亚"，但不适用于[Azure 中国世纪互联](/azure/china/overview-operations)或[azure 政府](../azure-government/documentation-government-welcome.md)版。

* 共 1,000 个集成帐户，包括[开发人员和高级 SKU](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level)中任何[集成服务环境 (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) 内的集成帐户。

* 无论是[开发人员还是高级](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level)层级，每个 ISE 总共只能有 5 个集成帐户：

  | ISE SKU | 集成帐户限制 |
  |---------|----------------------------|
  | **高级** | 共 5 个 - 仅限[标准](../logic-apps/logic-apps-pricing.md#integration-accounts)帐户，包括一个免费的标准帐户。 不允许使用免费帐户或基本帐户。 |
  | **开发人员** | 共 5 个 - [免费](../logic-apps/logic-apps-pricing.md#integration-accounts)帐户（仅限 1 个）与[标准](../logic-apps/logic-apps-pricing.md#integration-accounts)帐户之和，或全部是标准帐户。 不允许使用基本帐户。 [开发人员 SKU](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level) 仅可用于试验、开发和测试，不可用于生产和性能测试。 |
  |||

超出 ISE 附带的限额添加的集成帐户需支付额外费用。 要了解 ISE 的定价和计费原理，请参阅[逻辑应用定价模型](../logic-apps/logic-apps-pricing.md#fixed-pricing)。 有关定价费率，请参阅[逻辑应用定价](https://azure.microsoft.com/pricing/details/logic-apps/)。

<a name="artifact-number-limits"></a>

### <a name="artifact-limits-per-integration-account"></a>每个集成帐户的项目限制

下面介绍对每个集成帐户层的项目数量限制。
有关定价费率，请参阅[逻辑应用定价](https://azure.microsoft.com/pricing/details/logic-apps/)。 若要了解集成帐户的定价和计费工作原理，请参阅[逻辑应用定价模型](../logic-apps/logic-apps-pricing.md#integration-accounts)。

> [!NOTE]
> 免费层仅用于探索场景，不用于生产场景。 此层限制吞吐量和使用情况，并且不具有服务级别协议 (SLA)。

| 项目 | 免费 | 基本 | 标准 |
|----------|------|-------|----------|
| EDI 贸易协议 | 10 | 1 | 1,000 |
| EDI 参与方 | 25 | 2 | 1,000 |
| 地图 | 25 | 500 | 1,000 |
| 架构 | 25 | 500 | 1,000 |
| 程序集 | 10 | 25 | 1,000 |
| 证书 | 25 | 2 | 1,000 |
| 批处理配置 | 5 | 1 | 50 |
||||

<a name="artifact-capacity-limits"></a>

### <a name="artifact-capacity-limits"></a>项目容量限制

| 项目 | 限制 | 注释 |
| -------- | ----- | ----- |
| Assembly | 8 MB | 若要上传大于 2 MB 的文件，请使用 [Azure 存储帐户和 blob 容器](../logic-apps/logic-apps-enterprise-integration-schemas.md)。 |
| 映射（XSLT 文件） | 8 MB | 若要上传大于 2 MB 的文件，请使用 [Azure 逻辑应用 REST API - 映射](/rest/api/logic/maps/createorupdate)。 <p><p>**注意**：映射可以成功处理的数据或记录量取决于 Azure 逻辑应用中的消息大小和操作超时限制。 例如，如果使用 HTTP 操作，则根据 [HTTP 消息大小和超时限制](#request-limits)，在操作能够在 HTTP 超时限制内完成的情况下，映射最多可以处理达到 HTTP 消息大小限制的数据量。 |
| 架构 | 8 MB | 若要上传大于 2 MB 的文件，请使用 [Azure 存储帐户和 blob 容器](../logic-apps/logic-apps-enterprise-integration-schemas.md)。 |
||||

<a name="integration-account-throughput-limits"></a>

### <a name="throughput-limits"></a>吞吐量限制

| 运行时终结点 | 免费 | 基本 | Standard | 说明 |
|------------------|------|-------|----------|-------|
| 每 5 分钟读取调用 | 3,000 | 30,000 | 60,000 | 此限制适用于从逻辑应用的运行历史记录获取原始输入和输出的调用。 你可根据需要在多个帐户之间分配工作负荷。 |
| 每 5 分钟调用调用 | 3,000 | 30,000 | 45,000 | 你可根据需要在多个帐户之间分配工作负荷。 |
| 每 5 分钟跟踪调用 | 3,000 | 30,000 | 45,000 | 你可根据需要在多个帐户之间分配工作负荷。 |
| 阻止并发调用 | ~1,000 | ~1,000 | ~1,000 | 对于所有 SKU 均相同。 你可减少并发请求数，或根据需要减少持续时间。 |
||||

<a name="b2b-protocol-limits"></a>

### <a name="b2b-protocol-as2-x12-edifact-message-size"></a>B2B 协议（AS2、X12、EDIFACT）消息大小

以下消息大小限制适用于 B2B 协议：

| 名称 | 多租户限制 | 集成服务环境限制 | 注释 |
|------|--------------------|---------------------------------------|-------|
| AS2 | v2 - 100 MB<br>v1 - 50 MB | v2 - 200 MB <br>v1 - 50 MB | 适用于解码和编码 |
| X12 | 50 MB | 50 MB | 适用于解码和编码 |
| EDIFACT | 50 MB | 50 MB | 适用于解码和编码 |
||||

<a name="disable-delete"></a>

## <a name="disabling-or-deleting-logic-apps"></a>禁用或删除逻辑应用

禁用逻辑应用后，任何新运行都不会实例化。 所有正在进行的和挂起的运行将继续进行，直到完成，这可能要花费一些时间才能完成。

删除逻辑应用后，任何新运行都不会实例化。 所有正在进行和挂起的运行都将取消。 如果有成千上万个运行，取消操作可能需要很长时间才能完成。

<a name="configuration"></a>

## <a name="firewall-configuration-ip-addresses-and-service-tags"></a>防火墙配置：IP 地址和服务标记

Azure 逻辑应用用于传入和传出调用的 IP 地址由逻辑应用所在的区域决定。 同一区域中的所有逻辑应用都使用相同的 IP 地址范围。 某些 [Power Automate](/power-automate/getting-started) 调用（例如 HTTP 和 HTTP + OpenAPI 请求）直接通过 Azure 逻辑应用服务执行并来自此处列出的 IP 地址。 要详细了解 Power Automate 使用的 IP 地址，请参阅 [Power Automate 中的限制和配置](/flow/limits-and-config#ip-address-configuration)。

> [!TIP]
> 为帮助你更简单地创建安全规则，可选择性地使用[服务标记](../virtual-network/service-tags-overview.md)，而不是为每个区域指定逻辑应用 IP 地址，如此部分中稍后所述。
> 这些标记适用于可使用逻辑应用服务的区域：
>
> * **LogicAppsManagement**：表示逻辑应用服务的入站 IP 地址前缀。
> * **LogicApps**：表示逻辑应用服务的出站 IP 地址前缀。

* 对于 [Azure 中国世纪互联](/azure/china/)，固定或保留 IP 地址不可用于[自定义连接器](../logic-apps/custom-connector-overview.md)和[托管连接器](../connectors/apis-list.md#managed-api-connectors)（例如，Azure 存储、SQL Server、Office 365 Outlook 等）。

* 若要支持逻辑应用通过 [HTTP](../connectors/connectors-native-http.md)、[HTTP + Swagger](../connectors/connectors-native-http-swagger.md) 和其他 HTTP 请求直接发出的调用，请根据逻辑应用所在的区域，使用逻辑应用服务所用的所有[入站](#inbound)和[出站](#outbound) IP 地址对防火墙进行设置。 这些地址显示在本部分的“入站”和“出站”标题下，并按区域进行排序。

* 若要支持[托管连接器](../connectors/apis-list.md#managed-api-connectors)发出的调用，请根据逻辑应用所在的区域，使用这些连接器所用的全部[出站](#outbound) IP 地址对防火墙进行设置。 这些地址显示在本部分的“出站”标题下，并按区域进行排序。

* 为了使集成服务环境 (ISE) 中运行的逻辑应用能够通信，请确保你已[打开这些端口](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#network-ports-for-ise)

* 如果你的逻辑应用在访问使用[防火墙和防火墙规则](../storage/common/storage-network-security.md)的 Azure 存储帐户时遇到问题，可采用[多种方式来实现访问](../connectors/connectors-create-api-azureblobstorage.md#access-storage-accounts-behind-firewalls)。

  例如，逻辑应用不能直接访问使用防火墙规则的存储帐户，因此存在于同一区域中。 但是，如果允许[区域中托管连接器的出站 IP 地址](../logic-apps/logic-apps-limits-and-config.md#outbound)，则逻辑应用可以访问其他区域中的存储帐户，除非你使用 Azure 表存储或 Azure 队列存储连接器。 若要访问表存储或队列存储，则可改用 HTTP 触发器和操作。 有关其他选项，请参阅[访问受防火墙保护的存储帐户](../connectors/connectors-create-api-azureblobstorage.md#access-storage-accounts-behind-firewalls)。

<a name="inbound"></a>

### <a name="inbound-ip-addresses"></a>入站 IP 地址

该部分仅列出了 Azure 逻辑应用服务的入站 IP 地址。 如果你有 Azure 政府，请参阅 [Azure 政府 - 入站 IP 地址](#azure-government-inbound)。

> [!TIP]
> 为帮助你更简单地创建安全规则，可选择性地使用[服务标记](../virtual-network/service-tags-overview.md) LogicAppsManagement，而不是为每个区域指定入站逻辑应用 IP 地址前缀。
> 此标记可使用逻辑应用服务的区域。

<a name="multi-tenant-inbound"></a>

#### <a name="multi-tenant-azure---inbound-ip-addresses"></a>多租户 Azure - 入站 IP 地址

| 多租户区域 | IP |
|---------------------|----|
| 澳大利亚东部 | 13.75.153.66、104.210.89.222、104.210.89.244、52.187.231.161 |
| 澳大利亚东南部 | 13.73.115.153、40.115.78.70、40.115.78.237、52.189.216.28 |
| 巴西南部 | 191.235.86.199、191.235.95.229、191.235.94.220、191.234.166.198 |
| 加拿大中部 | 13.88.249.209、52.233.30.218、52.233.29.79、40.85.241.105 |
| 加拿大东部 | 52.232.129.143、52.229.125.57、52.232.133.109、40.86.202.42 |
| 印度中部 | 52.172.157.194、52.172.184.192、52.172.191.194、104.211.73.195 |
| 美国中部 | 13.67.236.76、40.77.111.254、40.77.31.87、104.43.243.39 |
| 东亚 | 168.63.200.173、13.75.89.159、23.97.68.172、40.83.98.194 |
| 美国东部 | 137.135.106.54、40.117.99.79、40.117.100.228、137.116.126.165 |
| 美国东部 2 | 40.84.25.234、40.79.44.7、40.84.59.136、40.70.27.253 |
| 法国中部 | 52.143.162.83、20.188.33.169、52.143.156.55、52.143.158.203 |
| 法国南部 | 52.136.131.145、52.136.129.121、52.136.130.89、52.136.131.4 |
| 日本东部 | 13.71.146.140、13.78.84.187、13.78.62.130、13.78.43.164 |
| 日本西部 | 40.74.140.173、40.74.81.13、40.74.85.215、40.74.68.85 |
| 韩国中部 | 52.231.14.182、52.231.103.142、52.231.39.29、52.231.14.42 |
| 韩国南部 | 52.231.166.168、52.231.163.55、52.231.163.150、52.231.192.64 |
| 美国中北部 | 168.62.249.81、157.56.12.202、65.52.211.164、65.52.9.64 |
| 北欧 | 13.79.173.49、52.169.218.253、52.169.220.174、40.112.90.39 |
| 南非北部 | 102.133.228.4、102.133.224.125、102.133.226.199、102.133.228.9 |
| 南非西部 | 102.133.72.190、102.133.72.145、102.133.72.184、102.133.72.173 |
| 美国中南部 | 13.65.98.39、13.84.41.46、13.84.43.45、40.84.138.132 |
| 印度南部 | 52.172.9.47、52.172.49.43、52.172.51.140、104.211.225.152 |
| 东南亚 | 52.163.93.214、52.187.65.81、52.187.65.155、104.215.181.6 |
| 阿联酋中部 | 20.45.75.193、20.45.64.29、20.45.64.87、20.45.71.213 |
| 英国南部 | 51.140.79.109、51.140.78.71、51.140.84.39、51.140.155.81 |
| 英国西部 | 51.141.48.98、51.141.51.145、51.141.53.164、51.141.119.150 |
| 美国中西部 | 52.161.26.172、52.161.8.128、52.161.19.82、13.78.137.247 |
| 西欧 | 13.95.155.53、52.174.54.218、52.174.49.6 |
| 印度西部 | 104.211.164.112、104.211.165.81、104.211.164.25、104.211.157.237 |
| 美国西部 | 52.160.90.237、138.91.188.137、13.91.252.184、157.56.160.212 |
| 美国西部 2 | 13.66.224.169、52.183.30.10、52.183.39.67、13.66.128.68 |
|||

<a name="azure-government-inbound"></a>

#### <a name="azure-government---inbound-ip-addresses"></a>Azure 政府 - 入站 IP 地址

| Azure 政府区域 | IP |
|-------------------------|----|
| US Gov 亚利桑那州 | 52.244.67.164、52.244.67.64、52.244.66.82 |
| US Gov 德克萨斯州 | 52.238.119.104、52.238.112.96、52.238.119.145 |
| US Gov 弗吉尼亚州 | 52.227.159.157、52.227.152.90、23.97.4.36 |
| US DoD 中部 | 52.182.49.204、52.182.52.106 |
|||

<a name="outbound"></a>

### <a name="outbound-ip-addresses"></a>出站 IP 地址

该部分列出了 Azure 逻辑应用服务和托管连接器的出站 IP 地址。 如果你有 Azure 政府，请参阅 [Azure 政府 - 出站 IP 地址](#azure-government-outbound)。

> [!TIP]
> 为帮助你更简单地创建安全规则，可选择性地使用[服务标记](../virtual-network/service-tags-overview.md) LogicApps，而不是为每个区域指定出站逻辑应用 IP 地址前缀。
> 对于托管连接器，可以选择使用**AzureConnectors**服务标记，而不是为每个区域指定出站托管连接器 IP 地址前缀。 这些标记适用于逻辑应用服务可用的区域。 

<a name="multi-tenant-outbound"></a>

#### <a name="multi-tenant-azure---outbound-ip-addresses"></a>多租户 Azure - 出站 IP 地址

| 多租户区域 | 逻辑应用 IP | 托管连接器 IP |
|---------------------|---------------|-----------------------|
| 澳大利亚东部 | 13.75.149.4、104.210.91.55、104.210.90.241、52.187.227.245、52.187.226.96、52.187.231.184、52.187.229.130、52.187.226.139 | 13.70.72.192 - 13.70.72.207, 13.72.243.10, 40.126.251.213, 52.237.214.72, 13.70.78.224 - 13.70.78.255 |
| 澳大利亚东南部 | 13.73.114.207、13.77.3.139、13.70.159.205、52.189.222.77、13.77.56.167、13.77.58.136、52.189.214.42、52.189.220.75 | 13.70.136.174, 13.77.50.240 - 13.77.50.255, 40.127.80.34, 52.255.48.202, 13.77.55.160 - 13.77.55.191 |
| 巴西南部 | 191.235.82.221、191.235.91.7、191.234.182.26、191.237.255.116、191.234.161.168、191.234.162.178、191.234.161.28、191.234.162.131 | 104.41.59.51, 191.232.38.129, 191.233.203.192 - 191.233.203.207, 191.232.191.157, 191.233.207.160 - 191.233.207.191 |
| 加拿大中部 | 52.233.29.92, 52.228.39.244, 40.85.250.135, 40.85.250.212, 13.71.186.1, 40.85.252.47, 13.71.184.150 | 13.71.170.208 - 13.71.170.223, 52.228.33.76, 52.228.34.13, 52.228.42.205, 52.233.31.197, 52.237.24.126, 52.237.32.212, 13.71.175.160 - 13.71.175.191, 13.71.170.224 - 13.71.170.239 |
| 加拿大东部 | 52.232.128.155、52.229.120.45、52.229.126.25、40.86.203.228、40.86.228.93、40.86.216.241、40.86.226.149、40.86.217.241 | 40.69.106.240 - 40.69.106.255, 52.229.120.52, 52.229.120.178, 52.229.123.98, 52.229.126.202, 52.242.35.152, 52.242.30.112, 40.69.111.0 - 40.69.111.31 |
| 印度中部 | 52.172.154.168、52.172.186.159、52.172.185.79、104.211.101.108、104.211.102.62、104.211.90.169、104.211.90.162、104.211.74.145 | 52.172.211.12, 104.211.81.192 - 104.211.81.207, 104.211.98.164, 52.172.212.129, 20.43.123.0 - 20.43.123.31 |
| 美国中部 | 13.67.236.125、104.208.25.27、40.122.170.198、40.113.218.230、23.100.86.139、23.100.87.24、23.100.87.56、23.100.82.16 | 13.89.171.80 - 13.89.171.95, 40.122.49.51, 52.173.245.164, 52.173.241.27, 40.77.68.110, 13.89.178.64 - 13.89.178.95 |
| 东亚 | 13.75.94.173、40.83.127.19、52.175.33.254、40.83.73.39、65.52.175.34、40.83.77.208、40.83.100.69、40.83.75.165 | 13.75.36.64 - 13.75.36.79, 23.99.116.181, 52.175.23.169, 13.75.110.131, 104.214.164.0 - 104.214.164.31 |
| 美国东部 | 13.92.98.111、40.121.91.41、40.114.82.191、23.101.139.153、23.100.29.190、23.101.136.201、104.45.153.81、23.101.132.208 | 40.71.11.80 - 40.71.11.95, 40.71.249.205, 40.114.40.132, 40.71.249.139, 52.188.157.160, 40.71.15.160 - 40.71.15.191 |
| 美国东部 2 | 40.84.30.147、104.208.155.200、104.208.158.174、104.208.140.40、40.70.131.151、40.70.29.214、40.70.26.154、40.70.27.236 | 40.70.146.208 - 40.70.146.223, 52.232.188.154, 104.208.233.100, 104.209.247.23, 52.225.129.144, 40.65.220.25, 40.70.151.96 - 40.70.151.127 |
| 法国中部 | 52.143.164.80、52.143.164.15、40.89.186.30、20.188.39.105、40.89.191.161、40.89.188.169、40.89.186.28、40.89.190.104 | 40.79.130.208 - 40.79.130.223, 40.89.135.2, 40.89.186.239, 40.79.148.96 - 40.79.148.127 |
| 法国南部 | 52.136.132.40、52.136.129.89、52.136.131.155、52.136.133.62、52.136.139.225、52.136.130.144、52.136.140.226、52.136.129.51 | 40.79.178.240 - 40.79.178.255, 52.136.133.184, 52.136.142.154, 40.79.180.224 - 40.79.180.255 |
| 日本东部 | 13.71.158.3、13.73.4.207、13.71.158.120、13.78.18.168、13.78.35.229、13.78.42.223、13.78.21.155、13.78.20.232 | 13.71.153.19, 13.78.108.0 - 13.78.108.15, 40.115.186.96, 13.73.21.230, 40.79.189.64 - 40.79.189.95 |
| 日本西部 | 40.74.140.4、104.214.137.243、138.91.26.45、40.74.64.207、40.74.76.213、40.74.77.205、40.74.74.21、40.74.68.85 | 40.74.100.224 - 40.74.100.239, 40.74.130.77, 104.215.61.248, 104.215.27.24, 40.80.180.64 - 40.80.180.95 |
| 韩国中部 | 52.231.14.11、52.231.14.219、52.231.15.6、52.231.10.111、52.231.14.223、52.231.77.107、52.231.8.175、52.231.9.39 | 52.231.18.208 - 52.231.18.223, 52.141.36.214, 52.141.1.104, 20.44.29.64 - 20.44.29.95 |
| 韩国南部 | 52.231.204.74、52.231.188.115、52.231.189.221、52.231.203.118、52.231.166.28、52.231.153.89、52.231.155.206、52.231.164.23 | 52.231.147.0 - 52.231.147.15, 52.231.163.10, 52.231.201.173, 52.231.148.224 - 52.231.148.255 |
| 美国中北部 | 168.62.248.37、157.55.210.61、157.55.212.238、52.162.208.216、52.162.213.231、65.52.10.183、65.52.9.96、65.52.8.225 | 52.162.107.160 - 52.162.107.175, 52.162.242.161, 65.52.218.230, 52.162.126.4, 52.162.111.192 - 52.162.111.223 |
| 北欧 | 40.113.12.95、52.178.165.215、52.178.166.21、40.112.92.104、40.112.95.216、40.113.4.18、40.113.3.202、40.113.1.181 | 13.69.227.208 - 13.69.227.223, 52.178.150.68, 104.45.93.9, 94.245.91.93, 52.169.28.181, 40.115.108.29, 13.69.231.192 - 13.69.231.223 |
| 南非北部 | 102.133.231.188、102.133.231.117、102.133.230.4、102.133.227.103、102.133.228.6、102.133.230.82、102.133.231.9、102.133.231.51 | 102.133.168.167, 40.127.2.94, 102.133.155.0 - 102.133.155.15, 102.133.253.0 - 102.133.253.31 |
| 南非西部 | 102.133.72.98、102.133.72.113、102.133.75.169、102.133.72.179、102.133.72.37、102.133.72.183、102.133.72.132、102.133.75.191 | 102.133.72.85, 102.133.75.194, 102.133.27.0 - 102.133.27.15, 102.37.64.0 - 102.37.64.31 |
| 美国中南部 | 104.210.144.48、13.65.82.17、13.66.52.232、23.100.124.84、70.37.54.122、70.37.50.6、23.100.127.172、23.101.183.225 | 13.65.86.57, 104.214.19.48 - 104.214.19.63, 104.214.70.191, 52.171.130.92, 13.73.244.224 - 13.73.244.255 |
| 印度南部 | 52.172.50.24、52.172.55.231、52.172.52.0、104.211.229.115、104.211.230.129、104.211.230.126、104.211.231.39、104.211.227.229 | 13.71.125.22、40.78.194.240 - 40.78.194.255、104.211.227.225、13.71.127.26 |
| 东南亚 | 13.76.133.155、52.163.228.93、52.163.230.166、13.76.4.194、13.67.110.109、13.67.91.135、13.76.5.96、13.67.107.128 | 13.67.8.240 - 13.67.8.255, 13.76.231.68, 52.187.68.19, 52.187.115.69, 13.67.15.32 - 13.67.15.63 |
| 阿联酋中部 | 20.45.75.200、20.45.72.72、20.45.75.236、20.45.79.239、20.45.67.170、20.45.72.54、20.45.67.134、20.45.67.135 | 20.45.67.28、20.45.67.45、20.37.74.192 - 20.37.74.207、40.120.8.0 - 40.120.8.31 |
| 英国南部 | 51.140.74.14、51.140.73.85、51.140.78.44、51.140.137.190、51.140.153.135、51.140.28.225、51.140.142.28、51.140.158.24 | 51.140.80.51, 51.140.148.0 - 51.140.148.15, 51.140.61.124, 51.140.74.150, 51.105.77.96 - 51.105.77.127 |
| 英国西部 | 51.141.54.185、51.141.45.238、51.141.47.136、51.141.114.77、51.141.112.112、51.141.113.36、51.141.118.119、51.141.119.63 | 51.140.211.0 - 51.140.211.15, 51.141.47.105, 51.141.124.13, 51.141.52.185, 51.140.212.224 - 51.140.212.255 |
| 美国中西部 | 52.161.27.190、52.161.18.218、52.161.9.108、13.78.151.161、13.78.137.179、13.78.148.140、13.78.129.20、13.78.141.75 | 13.71.195.32 - 13.71.195.47, 52.161.102.22, 13.78.132.82, 52.161.101.204, 13.71.199.192 - 13.71.199.223 |
| 西欧 | 40.68.222.65、40.68.209.23、13.95.147.65、23.97.218.130、51.144.182.201、23.97.211.179、104.45.9.52、23.97.210.126 | 13.69.64.208 - 13.69.64.223, 40.115.50.13, 52.174.88.118, 40.91.208.65, 52.166.78.89, 13.93.36.78, 13.69.71.192 - 13.69.71.223 |
| 印度西部 | 104.211.164.80、104.211.162.205、104.211.164.136、104.211.158.127、104.211.156.153、104.211.158.123、104.211.154.59、104.211.154.7 | 104.211.146.224 - 104.211.146.239, 104.211.161.203, 104.211.189.218, 104.211.189.124, 20.38.128.224 - 20.38.128.255 |
| 美国西部 | 52.160.92.112、40.118.244.241、40.118.241.243、157.56.162.53、157.56.167.147、104.42.49.145、40.83.164.80、104.42.38.32 | 40.112.243.160 - 40.112.243.175, 104.40.51.248, 104.42.122.49, 40.112.195.87, 13.93.148.62, 13.86.223.32 - 13.86.223.63 |
| 美国西部 2 | 13.66.210.167、52.183.30.169、52.183.29.132、13.66.210.167、13.66.201.169、13.77.149.159、52.175.198.132、13.66.246.219 | 13.66.140.128 - 13.66.140.143, 52.183.78.157, 52.191.164.250, 13.66.164.219, 13.66.145.96 - 13.66.145.127 |
||||

<a name="azure-government-outbound"></a>

#### <a name="azure-government---outbound-ip-addresses"></a>Azure 政府 - 出站 IP 地址

| 区域 | 逻辑应用 IP | 托管连接器 IP |
|--------|---------------|-----------------------|
| US DoD 中部 | 52.182.48.215、52.182.92.143 | 52.127.58.160 - 52.127.58.175, 52.182.54.8, 52.182.48.136, 52.127.61.192 - 52.127.61.223 |
| US Gov 亚利桑那州 | 52.244.67.143、52.244.65.66、52.244.65.190 | 52.127.2.160 - 52.127.2.175, 52.244.69.0, 52.244.64.91, 52.127.5.224 - 52.127.5.255 |
| US Gov 德克萨斯州 | 52.238.114.217、52.238.115.245、52.238.117.119 | 52.127.34.160 - 52.127.34.175, 40.112.40.25, 52.238.161.225, 20.140.137.128 - 20.140.137.159 |
| US Gov 弗吉尼亚州 | 13.72.54.205、52.227.138.30、52.227.152.44 | 52.127.42.128 - 52.127.42.143、52.227.143.61、52.227.162.91 |
||||

## <a name="next-steps"></a>后续步骤

* 了解如何[创建第一个逻辑应用](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* 了解[常见示例和方案](../logic-apps/logic-apps-examples-and-scenarios.md)
