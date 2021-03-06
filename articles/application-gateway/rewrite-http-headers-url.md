---
title: Azure 应用程序网关重写 HTTP 标头和 URL |Microsoft Docs
description: 本文概述了如何在 Azure 应用程序网关中重写 HTTP 标头和 URL
services: application-gateway
author: surajmb
ms.service: application-gateway
ms.topic: conceptual
ms.date: 07/16/2020
ms.author: surmb
ms.openlocfilehash: 2ee34e1a7959aafa5db949b443fd58cca58719c6
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2020
ms.locfileid: "87281185"
---
# <a name="rewrite-http-headers-and-url-with-application-gateway"></a>用应用程序网关重写 HTTP 标头和 URL

 应用程序网关允许重写请求和响应的所选内容。 利用此功能，可以转换 Url、查询字符串参数以及修改请求和响应标头。 它还允许您添加条件，以确保仅当满足某些条件时才重写 URL 或指定的标头。 这些条件基于请求和响应信息。

>[!NOTE]
>HTTP 标头和 URL 重写功能仅适用于[应用程序网关 V2 SKU](application-gateway-autoscaling-zone-redundant.md)

## <a name="rewrite-types-supported"></a>支持的重写类型

### <a name="request-and-response-headers"></a>请求和响应标头

HTTP 标头可让客户端和服务器连同请求或响应一起传递附加的信息。 重写这些标头可以完成重要的任务，例如，添加安全相关的标头字段（如 HSTS/ X-XSS-Protection）、删除可能透露敏感信息的响应标头字段，以及从 X-Forwarded-For 标头中删除端口信息。

当请求和响应数据包在客户端与后端池之间移动时，可以通过应用程序网关添加、删除或更新 HTTP 请求和响应标头。

若要了解如何使用 Azure 门户重写应用程序网关的请求和响应标头，请参阅[此处](rewrite-url-portal.md)。

![img](./media/rewrite-http-headers-url/header-rewrite-overview.png)


**支持的标头**

您可以重写请求和响应中的所有标头（连接除外）和升级标头。 还可以使用应用程序网关创建自定义标头，并将其添加到通过该网关路由的请求和响应。

### <a name="url-path-and-query-string-preview"></a>URL 路径和查询字符串（预览）

利用应用程序网关上的 URL 重写功能，你可以：

* 重写请求 URL 的主机名、路径和查询字符串 

* 选择重新编写侦听器上所有请求的 URL，或者只重写与所设置的一个或多个条件匹配的请求。 这些条件基于请求和响应属性（请求、标头、响应标头和服务器变量）。

* 选择基于原始 URL 或重写的 URL 来路由请求（选择后端池）

若要了解如何使用 Azure 门户使用应用程序网关重写 URL，请参阅[此处](rewrite-url-portal.md)。

![img](./media/rewrite-http-headers-url/url-rewrite-overview.png)

>[!NOTE]
> URL 重写功能处于预览中，仅可用于应用程序网关 Standard_v2 和 WAF_v2 SKU。 不建议在生产环境中使用它。 若要了解有关预览的详细信息，请参阅[此处的使用条款](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)。

## <a name="rewrite-actions"></a>重写操作

使用重写操作来指定要重写的 URL、请求标头或响应标头以及要将其重写到的新值。 URL 或新的或现有的标头的值可以设置为以下类型的值：

* 文本
* 请求标头。 若要指定请求标头，需使用语法 {http_req_*headerName*}
* 响应标头。 若要指定响应标头，需使用语法 {http_resp_*headerName*}
* 服务器变量。 若要指定服务器变量，需使用语法 {var_*serverVariable*}。 请参阅支持的服务器变量列表
* 文本、请求标头、响应标头和服务器变量的组合。 

## <a name="rewrite-conditions"></a>重写条件

您可以使用重写条件（可选配置）来评估 HTTP （S）请求和响应的内容，并且仅在满足一个或多个条件时才执行重写。 应用程序网关使用以下类型的变量来评估请求和响应的内容：

* 请求中的 HTTP 标头
* 响应中的 HTTP 标头
* 应用程序网关服务器变量

可以使用条件来评估指定的变量是否存在、指定的变量是否与特定的值匹配，或指定的变量是否与特定的模式匹配。 


### <a name="pattern-matching"></a>模式匹配 

应用程序网关在条件中使用正则表达式进行模式匹配。 您可以使用[Perl 兼容的正则表达式（PCRE）库](https://www.pcre.org/)来设置条件中的正则表达式模式匹配。 若要了解正则表达式语法，请参阅 [Perl 正则表达式主页](https://perldoc.perl.org/perlre.html)。

### <a name="capturing"></a>捕获

若要捕获子字符串供以后使用，请在条件 regex 定义中的匹配子模式的子模式两侧加上括号。 第一对括号将其子字符串存储在1中，第二对用2，依此类推。 你可以根据需要使用任意多个括号;Perl 只需为你定义更多的编号变量来表示这些捕获的字符串。 [Ref](https://docstore.mik.ua/orelly/perl/prog3/ch05_07.htm)中的一些示例： 

*  /（\d）（\d）/# 匹配两个数字，将它们捕获到组1和2中 

* /（\d +）/# 匹配一个或多个数字，将全部捕获到组1 

* /（\d） +/# 与一个数字匹配一次或多次，捕获最后一个到组1

捕获后，可以使用以下格式在操作集中引用它们：

* 对于请求标头捕获，必须使用 {http_req_headerName_groupNumber}。 例如，{http_req_User Agent_1} 或 {http_req_User-Agent_2}
* 对于响应标头捕获，必须使用 {http_resp_headerName_groupNumber}。 例如，{http_resp_Location_1} 或 {http_resp_Location_2}
* 对于服务器变量，必须使用 {var_serverVariableName_groupNumber}。 例如，{var_uri_path_1} 或 {var_uri_path_2}

如果要使用整个值，则不应提到数字。 只需使用 {http_req_headerName} 等格式，而无需使用 groupNumber。

## <a name="server-variables"></a>服务器变量

应用程序网关使用服务器变量来存储有关服务器、与客户端建立的连接以及对连接的当前请求的有用信息。 例如，存储的信息包括客户端的 IP 地址和 Web 浏览器类型。 服务器变量会动态更改，例如，加载新页或发布表单时就会更改。 可以使用这些变量来评估重写条件和重写标头。 若要使用服务器变量的值重写标头，需要在语法 {var_*serverVariableName*} 中指定这些变量

应用程序网关支持以下服务器变量：

|   变量名称    |                   说明                                           |
| ------------------------- | ------------------------------------------------------------ |
| add_x_forwarded_for_proxy | 带有变量（请参阅此表后面的说明）的 X 转发的客户端请求标头字段 `client_ip` 的格式为 IP1、IP2、IP3 等。 如果的 X 转发的字段不在客户端请求标头中，则该 `add_x_forwarded_for_proxy` 变量等于 `$client_ip` 变量。   如果要重写应用程序网关设置的 X 转发的标头，使标头只包含没有端口信息的 IP 地址，则此变量特别有用。 |
| ciphers_supported         | 客户端支持的加密法列表。               |
| ciphers_used              | 用于建立的 TLS 连接的密码字符串。 |
| client_ip                 | 应用程序网关从中接收请求的客户端的 IP 地址。 如果在应用程序网关和发起方的客户端之前有反向代理， `client_ip` 将返回反向代理的 IP 地址。 |
| client_port               | 客户端端口。                                             |
| client_tcp_rtt            | 有关客户端 TCP 连接的信息。 在支持 TCP_INFO 套接字选项的系统上可用。 |
| client_user               | 使用 HTTP 身份验证时，提供用于身份验证的用户名。 |
| 主机                      | 此优先级顺序如下：请求行中的主机名、主机请求标头字段中的主机名或与请求匹配的服务器名称。 示例：在请求中 `http://contoso.com:8080/article.aspx?id=123&title=fabrikam` ，主机值将是`contoso.com` |
| cookie_*name*             | *name* Cookie。                                           |
| http_method               | 用于发出 URL 请求的方法。 例如，GET 或 POST。 |
| http_status               | 会话状态。 例如 200、400 或 403。           |
| http_version              | 请求协议。 通常为 HTTP/1.0、HTTP/1.1 或 HTTP/2.0。 |
| query_string              | 请求的 URL 中的 "？" 后面的变量/值对的列表。 示例：在请求中 `http://contoso.com:8080/article.aspx?id=123&title=fabrikam` ，query_string 值将为`id=123&title=fabrikam` |
| received_bytes            | 请求的长度（包括请求行、标头和请求正文）。 |
| request_query             | 请求行中的参数。                           |
| request_scheme            | 请求方案：http 或 https。                           |
| request_uri               | 完整的原始请求 URI（带参数）。 示例：在请求中 `http://contoso.com:8080/article.aspx?id=123&title=fabrikam*` ，request_uri 值将为`/article.aspx?id=123&title=fabrikam` |
| sent_bytes                | 发送到客户端的字节数。                        |
| server_port               | 接受请求的服务器端口。              |
| ssl_connection_protocol   | 已建立的 TLS 连接的协议。               |
| ssl_enabled               | 如果连接在 TLS 模式下建立，则为“On”。 否则为空字符串。 |
| uri_path                  | 标识宿主中 web 客户端要访问的特定资源。 这是请求 URI 中没有参数的部分。 示例：在请求中 `http://contoso.com:8080/article.aspx?id=123&title=fabrikam` ，uri_path 值将为`/article.aspx` |

 

## <a name="rewrite-configuration"></a>重写配置

若要配置重写规则，需要创建重写规则集，并在其中添加重写规则配置。

重写规则集包含：

* **请求路由规则关联：** 重写配置通过路由规则与源侦听器关联。 使用基本路由规则时，重写配置与源侦听器相关联，并且是全局标头重写。 使用基于路径的路由规则时，将在 URL 路径映射上定义重写配置。 在这种情况下，该规则只会应用到站点的特定路径区域。 可以创建多个重写集，并将每个重写设置应用于多个侦听器。 但是，对于一个特定的侦听器，只能应用一个重写集。

* **重写条件**：此为可选配置。 重写条件评估 HTTP(S) 请求和响应的内容。 如果 HTTP(S) 请求或响应与重写条件匹配，则会发生重写操作。 如果将多个条件关联到一个操作，仅当满足所有条件时，才会发生该操作。 换言之，操作属于逻辑 AND 运算。

* **重写类型**：有3种类型的重写可用：
   * 重写请求标头 
   * 重写响应标头。
   * 重写 URL： URL 重写类型包含3个组件
      * **URL 路径**：要将路径重写到的值。 
      * **URL 查询字符串**：查询字符串要重写到的值。 
      * **重新计算路径映射**：用于确定是否重新计算 URL 路径映射。 如果未选中，则原始 URL 路径将用于匹配 URL 路径映射中的路径模式。 如果设置为 true，则会重新计算 URL 路径映射，以检查与重写的路径是否匹配。 启用此开关有助于在重写后将请求路由到不同的后端池。

### <a name="common-scenarios-for-header-rewrite"></a>标头重写的常见方案

#### <a name="remove-port-information-from-the-x-forwarded-for-header"></a>从 X-Forwarded-For 标头中删除端口信息

应用程序网关先在所有请求中插入 X-Forwarded-For 标头，然后将请求转发到后端。 此标头是 IP 端口的逗号分隔列表。 在某些情况下，后端服务器只需在标头中包含 IP 地址。 你可以使用标头重写从 X-Forwarded-For 标头中删除端口信息。 执行此操作的一种方法是将标头设置为 add_x_forwarded_for_proxy 服务器变量。 此外，还可以使用变量 client_ip：

![删除端口](./media/rewrite-http-headers-url/remove-port.png)


#### <a name="modify-a-redirection-url"></a>修改重定向 URL

当后端应用程序发送重定向响应时，你可能希望将客户端重定向到不同的 URL，而不是后端应用程序指定的 URL。 例如，当应用服务托管在应用程序网关后面，并要求客户端重定向到其相对路径时，你可能希望这样做。 （例如，从 contoso.azurewebsites.net/path1 重定向到 contoso.azurewebsites.net/path2。）

由于应用服务是多租户服务，因此它会使用请求中的主机标头将请求路由到正确的终结点。 应用服务的默认域名为 *. azurewebsites.net （例如 contoso.azurewebsites.net），不同于应用程序网关的域名（例如，contoso.com）。 因为来自客户端的原始请求将应用程序网关的域名（contoso.com）作为主机名，所以，应用程序网关会将主机名更改为 contoso.azurewebsites.net。 做出此更改的目的是使应用服务能够将请求路由到正确的终结点。

当应用服务发送重定向响应时，它会在其响应的位置标头中，使用它从应用程序网关收到的请求中的相同主机名。 因此，客户端将直接向 contoso.azurewebsites.net/path2 发出请求，而不是通过应用程序网关（contoso.com/path2）。 不应该绕过应用程序网关。

将 location 标头中的主机名设置为应用程序网关的域名即可解决此问题。

下面是替换主机名的步骤：

1. 创建一个包含条件的重写规则，该规则评估响应中的 location 标头是否包含 azurewebsites.net。 输入模式 `(https?):\/\/.*azurewebsites\.net(.*)$`。
2. 执行相应的操作来重写 location 标头，使其包含应用程序网关的主机名。 为此，请输入 `{http_resp_Location_1}://contoso.com{http_resp_Location_2}` 作为标头值。 另外，还可以使用服务器变量 `host` 来设置主机名，使其与原始请求相匹配。

![修改 location 标头](./media/rewrite-http-headers-url/app-service-redirection.png)

#### <a name="implement-security-http-headers-to-prevent-vulnerabilities"></a>实现安全 HTTP 标头以防止漏洞

在应用程序响应中实现必要的标头可以修复多个安全漏洞。 这些安全标头包括 X-XSS-Protection、Strict-Transport-Security 和 Content-Security-Policy。 可以使用应用程序网关为所有响应设置这些标头。

![安全标头](./media/rewrite-http-headers-url/security-header.png)

### <a name="delete-unwanted-headers"></a>删除不需要的标头

你可能想要从 HTTP 响应中删除透露敏感信息的标头。 例如，你可能想要删除后端服务器名称、操作系统或库详细信息等信息。 可以使用应用程序网关删除以下标头：

![删除标头](./media/rewrite-http-headers-url/remove-headers.png)

#### <a name="check-for-the-presence-of-a-header"></a>检查某个标头是否存在

可以在 HTTP 请求或响应标头中评估某个标头或服务器变量是否存在。 如果你希望只有存在特定的标头时执行标头重写，则此评估非常有用。

![检查某个标头是否存在](./media/rewrite-http-headers-url/check-presence.png)

### <a name="common-scenarios-for-url-rewrite"></a>URL 重写常见方案

#### <a name="parameter-based-path-selection"></a>基于参数的路径选择

若要完成根据标头的值、URL 的一部分或请求中的查询字符串选择后端池的方案，可以使用 URL 重写功能和基于路径的路由的组合。  例如，如果你有一个购物网站，产品类别作为查询字符串在 URL 中传递，并且你想要基于查询字符串将请求路由到后端，则：

**步骤1：** 创建如下图所示的路径映射

:::image type="content" source="./media/rewrite-http-headers-url/url-scenario1-1.png" alt-text="URL 重写方案1-1。":::

**步骤2（a）：** 创建具有3种重写规则的重写集： 

* 第一条规则具有一个条件，用于检查*category = 鞋*的*query_string*变量，并且具有一项操作，该操作将 URL 路径重写为/*listing1*并已启用**重新评估路径映射**

* 第二个规则具有一个条件，用于检查*类别 = 包*的*query_string*变量，并具有将 URL 路径重写为/*listing2*并已启用**重新评估路径映射**的操作

* 第三个规则具有一个条件，用于检查*类别 = 附件*的*query_string*变量，并具有将 URL 路径重写为/*listing3*并已启用**重新评估路径映射**的操作

:::image type="content" source="./media/rewrite-http-headers-url/url-scenario1-2.png" alt-text="URL 重写方案1-2。":::

 

**步骤2（b）：** 将此重写集与上述基于路径的规则的默认路径相关联

:::image type="content" source="./media/rewrite-http-headers-url/url-scenario1-3.png" alt-text="URL 重写方案1-3。":::

现在，如果用户请求*contoso.com/listing?category=any*，则它将与默认路径匹配，因为路径映射中的任何路径模式（/listing1、/listing2、/listing3）都不匹配。 由于已将上述重写集与此路径相关联，因此将对此重写集进行评估。 由于查询字符串将不匹配此重写集中的任何3个重写规则中的条件，因此不会发生重写操作，因此，请求将以不更改的方式路由到与默认路径（即*GenericList*）关联的后端。

 如果用户请求*contoso.com/listing?category=shoes，* 则会再次匹配默认路径。 但是，在这种情况下，第一个规则中的条件将匹配，因此将执行与条件相关联的操作，这会将 URL 路径重写为/*listing1* ，并重新计算路径映射。 重新计算路径映射时，请求现在将与模式 */listing1*相关联的路径匹配，请求将路由到与此模式关联的后端，即 ShoesListBackendPool

>[!NOTE]
>可以根据定义的条件将此方案扩展到任何标头或 cookie 值、URL 路径、查询字符串或服务器变量，并且实质上使你能够根据这些条件路由请求。

#### <a name="rewrite-query-string-parameters-based-on-the-url"></a>基于 URL 重写查询字符串参数

假设有一个购物网站方案，其中用户的可见性链接应简单明了，但后端服务器需要查询字符串参数来显示正确的内容。

在这种情况下，应用程序网关可以从 URL 捕获参数，并从 URL 添加查询字符串键值对。 例如，假设用户想要重写 `https://www.contoso.com/fashion/shirts` 到 `https://www.contoso.com/buy.aspx?category=fashion&product=shirts` ，可以通过以下 URL 重写配置来实现。

**条件**-如果服务器变量 `uri_path` 等于模式`/(.+)/(.+)`

:::image type="content" source="./media/rewrite-http-headers-url/url-scenario2-1.png" alt-text="URL 重写方案2-1。":::

**操作**-将 URL 路径设置为 `buy.aspx` ，并将字符串查询到`category={var_uri_path_1}&product={var_uri_path_2}`

:::image type="content" source="./media/rewrite-http-headers-url/url-scenario2-2.png" alt-text="URL 重写方案2-2。":::

有关实现上述方案的循序渐进指南，请参阅[使用应用程序网关重写 URL Azure 门户](rewrite-url-portal.md)

### <a name="url-rewrite-vs-url-redirect"></a>URL 重写 vs URL 重定向

对于 URL 重写，应用程序网关会在将请求发送到后端之前重写 URL。 这不会更改用户在浏览器中看到的内容，因为用户无法看到所做的更改。

对于 URL 重定向，应用程序网关会将重定向响应发送到使用新 URL 的客户端。 这反过来要求客户端将其请求重新发送到重定向中提供的新 URL。 用户在浏览器中看到的 URL 将更新到新的 URL

:::image type="content" source="./media/rewrite-http-headers-url/url-rewrite-vs-redirect.png" alt-text="重写 vs 重定向。":::

## <a name="limitations"></a>限制

- 如果响应中包含多个同名的标头，则重写其中某个标头的值会导致删除该响应中的其他标头。 这种情况往往出现于 Set-Cookie 标头，因为在一个响应中可以包含多个 Set-Cookie 标头。 例如，如果将应用服务与应用程序网关一起使用，并在应用程序网关上配置了基于 Cookie 的会话关联，则就会出现此类情况。 在这种情况下，响应将包含两个 Set-Cookie 标头：一个用于应用服务（例如 `Set-Cookie: ARRAffinity=ba127f1caf6ac822b2347cc18bba0364d699ca1ad44d20e0ec01ea80cda2a735;Path=/;HttpOnly;Domain=sitename.azurewebsites.net`），另一个用于应用程序网关关联（例如 `Set-Cookie: ApplicationGatewayAffinity=c1a2bd51lfd396387f96bl9cc3d2c516; Path=/`）。 在此情况下重写其中一个 Set-Cookie 标头可能会导致从响应中删除另一个 Set-Cookie 标头。
- 当应用程序网关配置为重定向请求或显示自定义错误页时，不支持重写。
- 标头名称可以包含任何字母数字字符和 [RFC 7230](https://tools.ietf.org/html/rfc7230#page-27) 中定义的特定符号。 当前不支持标头名称中有下划线（_）的特殊字符。
- 无法重写连接和升级标头

## <a name="next-steps"></a>后续步骤

- [了解如何使用 Azure 门户重写应用程序网关的 HTTP 标头](rewrite-http-headers-portal.md)
- [了解如何使用应用程序网关重写 URL Azure 门户](rewrite-url-portal.md)
