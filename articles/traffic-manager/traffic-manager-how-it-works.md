---
title: Azure 流量管理器的工作原理 | Microsoft Docs
description: 本文将帮助你了解流量管理器如何路由流量以确保 Web 应用程序的高性能和可用性
services: traffic-manager
documentationcenter: ''
author: rohinkoul
manager: twooley
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/05/2019
ms.author: rohink
ms.openlocfilehash: 4863ffd383cfcd46bad462156e26293d145fd418
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "80294859"
---
# <a name="how-traffic-manager-works"></a>流量管理器的工作原理

使用 Azure 流量管理器可以控制流量在应用程序终结点之间的分布。 终结点可以是托管在 Azure 内部或外部的任何面向 Internet 的服务。

流量管理器具有两大优势：

- 根据某个[流量路由方法](traffic-manager-routing-methods.md)对流量进行分布
- [连续监视终结点运行状况](traffic-manager-monitoring.md)，在终结点发生故障时自动进行故障转移

当客户端尝试连接到某个服务时，必须先将该服务的 DNS 名称解析成 IP 地址。 然后，客户端就可以连接到该 IP 地址以访问相关服务。

**需要了解的最重要一点是，流量管理器在 DNS 级别工作。**  流量管理器根据流量路由方法的规则，使用 DNS 将客户端导向到特定的服务终结点。 客户端**直接**连接到选定的终结点。 流量管理器不是代理或网关。 流量管理器看不到流量在客户端与服务之间传递。

## <a name="traffic-manager-example"></a>流量管理器示例

Contoso Corp 开发了一个新的合作伙伴门户。 此门户的 URL 为 `https://partners.contoso.com/login.aspx` 。 该应用程序托管在三个 Azure 区域中。 为了改善可用性并在全球最大程度地提高性能，他们使用流量管理器将客户端流量分布到最靠近的可用终结点。

为了实现此配置，他们完成以下步骤：

1. 部署服务的三个实例。 这些部署的 DNS 名称为“contoso-us.cloudapp.net”、“contoso-eu.cloudapp.net”和“contoso-asia.cloudapp.net”。
1. 创建一个名为“contoso.trafficmanager.net”的流量管理器配置文件，并将它配置为对三个终结点使用“性能”流量路由方法。
1. 使用 DNS CNAME 记录将虚构域名“partners.contoso.com”配置为指向“contoso.trafficmanager.net”。

![流量管理器 DNS 配置][1]

> [!NOTE]
> 通过 Azure 流量管理器来使用虚构域时，必须使用 CNAME 将虚构域名指向流量管理器域名。 DNS 标准不允许在域的“顶点”（或根）位置创建 CNAME。 因此，无法为“contoso.com”（有时称为“裸”域）创建 CNAME。 只能为“contoso.com”下的域（例如“www.contoso.com”）创建 CNAME。 为了克服此限制，建议在 [Azure DNS](../dns/dns-overview.md) 上托管 DNS 域并使用[别名记录](../dns/tutorial-alias-tm.md)以指向流量管理器配置文件。 或者可以使用简单的 HTTP 重定向将针对“contoso.com”的请求定向到某个备用名称（例如“www.contoso.com”）。

### <a name="how-clients-connect-using-traffic-manager"></a>客户端如何使用流量管理器进行连接

沿用前面的示例，当客户端请求页面 `https://partners.contoso.com/login.aspx` 时，会执行以下步骤来解析 DNS 名称并建立连接：

![使用流量管理器建立连接][2]

1. 客户端向已配置的递归 DNS 服务发送 DNS 查询，以解析名称“partners.contoso.com”。 递归 DNS 服务有时称为“本地 DNS”服务，并不直接托管 DNS 域。 客户端将联系各种权威 DNS 服务的工作负荷转移到 Internet，以便解析 DNS 名称。
2. 为了解析 DNS 名称，递归 DNS 服务将查找“contoso.com”域的名称服务器。 然后，它会联系这些名称服务器以请求“partners.contoso.com”DNS 记录。 contoso.com DNS 服务器返回指向 contoso.trafficmanager.net 的 CNAME 记录。
3. 接下来，递归 DNS 服务查找“trafficmanager.net”域的名称服务器，这些服务器由 Azure 流量管理器服务提供。 然后，将针对“contoso.trafficmanager.net”DNS 记录发出的请求发送到这些 DNS 服务器。
4. 流量管理器名称服务器接收该请求。 终结点的选择依据为：

    - 每个终结点的已配置状态（不返回已禁用的终结点）
    - 每个终结点的当前运行状况，可通过流量管理器运行状况检查来确定。 有关详细信息，请参阅[流量管理器终结点监视](traffic-manager-monitoring.md)。
    - 所选的流量路由方法。 有关详细信息，请参阅[流量管理器路由方法](traffic-manager-routing-methods.md)。

5. 选择的终结点以另一个 DNS CNAME 记录的形式返回。 在本例中，假设返回的是 contoso-us.cloudapp.net。
6. 接下来，递归 DNS 服务将查找“cloudapp.net”域的名称服务器。 它会联系这些名称服务器以请求“contoso-us.cloudapp.net”DNS 记录。 返回的 DNS“A”记录包含位于美国的服务终结点的 IP 地址。
7. 递归 DNS 服务将结果合并，向客户端返回单个 DNS 响应。
8. 客户端接收 DNS 结果，并连接到给定的 IP 地址。 客户端直接连接到应用程序服务终结点，而不是通过流量管理器连接。 由于这是一个 HTTPS 终结点，客户端将执行必要的 SSL/TLS 握手，然后针对“/login.aspx”页面发出 HTTP GET 请求。

递归 DNS 服务缓存它所收到的 DNS 响应。 客户端设备上的 DNS 解析程序也会缓存结果。 通过缓存可以加快后续 DNS 查询的响应速度，因为使用的是缓存中的数据，不需要查询其他名称服务器。 缓存的持续时间取决于每个 DNS 记录的“生存时间”(TTL) 属性。 该属性值越小，缓存过期时间就越短，因此访问流量管理器名称服务器所需的往返次数就越多。 如果指定较大的值，则意味着从故障终结点定向流量需要更长的时间。 使用流量管理器，可以将流量管理器 DNS 响应中使用的 TTL 配置为最短 0 秒，最长 2,147,483,647 秒（符合[ RFC-1035 ](https://www.ietf.org/rfc/rfc1035.txt)的最大范围），从而可选择使应用程序的需求实现最佳平衡的值。

## <a name="faqs"></a>常见问题解答

* [流量管理器使用什么 IP 地址？](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#what-ip-address-does-traffic-manager-use)

* [可以使用流量管理器路由什么类型的流量？](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#what-types-of-traffic-can-be-routed-using-traffic-manager)

* [流量管理器是否支持“粘滞”会话？](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#does-traffic-manager-support-sticky-sessions)

* [使用流量管理器时为何出现 HTTP 错误？](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#why-am-i-seeing-an-http-error-when-using-traffic-manager)

* [使用流量管理器对性能有什么影响？](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#what-is-the-performance-impact-of-using-traffic-manager)

* [流量管理器允许使用什么应用程序协议？](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#what-application-protocols-can-i-use-with-traffic-manager)

* [是否可以对“裸”域名使用流量管理器？](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#can-i-use-traffic-manager-with-a-naked-domain-name)

* [处理 DNS 查询时流量管理器是否会考虑客户端子网地址？](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#does-traffic-manager-consider-the-client-subnet-address-when-handling-dns-queries)

* [什么是 DNS TTL，它如何影响我的用户？](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#what-is-dns-ttl-and-how-does-it-impact-my-users)

* [可将流量管理器响应的 TTL 设置为多高或多低？](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-high-or-low-can-i-set-the-ttl-for-traffic-manager-responses)

* [如何了解传入到我的配置文件的查询数量？](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-can-i-understand-the-volume-of-queries-coming-to-my-profile)

## <a name="next-steps"></a>后续步骤

详细了解流量管理器[终结点监视和自动故障转移](traffic-manager-monitoring.md)。

详细了解流量管理器[流量路由方法](traffic-manager-routing-methods.md)。

<!--Image references-->
[1]: ./media/traffic-manager-how-traffic-manager-works/dns-configuration.png
[2]: ./media/traffic-manager-how-traffic-manager-works/flow.png

