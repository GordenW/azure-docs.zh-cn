---
title: 配置 Windows ASP.NET Core 应用
description: 了解如何在应用服务的本机 Windows 实例中配置 ASP.NET Core 应用。 本文介绍最常见的配置任务。
ms.devlang: dotnet
ms.topic: article
ms.date: 06/02/2020
ms.openlocfilehash: 5819fc5b2d6e64d1812dacd88a2a4f840f6e03c5
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "84907871"
---
# <a name="configure-a-windows-aspnet-core-app-for-azure-app-service"></a>为 Azure App Service 配置 Windows ASP.NET Core 应用

> [!NOTE]
> 有关 .NET Framework 中的 ASP.NET，请参阅为[Azure App Service 配置 ASP.NET 应用](configure-language-dotnet-framework.md)

必须将 ASP.NET Core 应用部署到 Azure App Service 编译的二进制文件。 Visual Studio 发布工具生成解决方案，然后直接部署已编译的二进制文件，而应用服务部署引擎首先部署代码存储库，然后编译二进制文件。 有关 Linux 应用的信息，请参阅[为 Azure App Service 配置 linux ASP.NET Core 应用](containers/configure-language-dotnetcore.md)。

本指南提供了有关 ASP.NET Core 开发人员的重要概念和说明。 如果你从未使用过 Azure App Service，请先按照[ASP.NET 快速入门](app-service-web-get-started-dotnet.md)和[使用 SQL 数据库教程 ASP.NET Core 教程进行](app-service-web-tutorial-dotnetcore-sqldb.md)操作。

## <a name="show-supported-net-core-runtime-versions"></a>显示受支持的 .NET Core 运行时版本

在应用服务中，Windows 实例已安装了所有受支持的 .NET Core 版本。 若要显示可用的 .NET Core 运行时和 SDK 版本，请 `https://<app-name>.scm.azurewebsites.net/DebugConsole` 在基于浏览器的控制台中导航到并运行以下命令：

```azurecli-interactive
dotnet --info
```

## <a name="set-net-core-version"></a>设置 .NET Core 版本

在项目文件中为 ASP.NET Core 项目设置目标框架。 有关详细信息，请参阅在 .NET Core 文档中[选择要使用的 .Net core 版本](https://docs.microsoft.com/dotnet/core/versions/selection)。

## <a name="access-environment-variables"></a>访问环境变量

在应用服务中，可以在应用代码外部[设置应用设置](configure-common.md#configure-app-settings)。 然后，你可以使用标准 ASP.NET Core 依赖关系注入模式在任何类中访问它们：

```csharp
using Microsoft.Extensions.Configuration;

namespace SomeNamespace 
{
    public class SomeClass
    {
        private IConfiguration _configuration;
    
        public SomeClass(IConfiguration configuration)
        {
            _configuration = configuration;
        }
    
        public SomeMethod()
        {
            // retrieve nested App Service app setting
            var myHierarchicalConfig = _configuration["My:Hierarchical:Config:Data"];
            // retrieve App Service connection string
            var myConnString = _configuration.GetConnectionString("MyDbConnection");
        }
    }
}
```

例如，如果在应用服务中配置具有相同名称的应用设置，并且在的*appsettings.js*中，应用服务值优先于值的*appsettings.js* 。 利用本地*appsettings.js*值，你可以在本地调试应用程序，但应用服务值允许你在产品和生产设置中运行应用。 连接字符串的工作方式与此相同。 这样，你可以将应用程序机密保存在代码存储库外，并访问适当的值，而无需更改代码。

> [!NOTE]
> 请注意，使用 .NET Core 标准的分隔符访问*appsettings.js上*的[层次结构配置数据](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/#hierarchical-configuration-data) `:` 。 若要覆盖应用服务中的特定层次结构配置设置，请在密钥中设置具有相同分隔格式的应用设置名称。 可以在[Cloud Shell](https://shell.azure.com)中运行以下示例：

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings My:Hierarchical:Config:Data="some value"
```

## <a name="deploy-multi-project-solutions"></a>部署多项目解决方案

当 Visual Studio 解决方案包含多个项目时，Visual Studio 发布过程已包括选择要部署的项目。 当你部署到应用服务部署引擎（如带 Git 或 with ZIP 部署）时，如果启用了 "生成自动化"，则应用服务部署引擎会选取作为应用服务应用的第一个网站或 Web 应用程序项目。 可以通过指定应用设置来指定应用服务应使用的项目 `PROJECT` 。 例如，在[Cloud Shell](https://shell.azure.com)中运行以下内容：

```azurecli-interactive
az webapp config appsettings set --resource-group <resource-group-name> --name <app-name> --settings PROJECT="<project-name>/<project-name>.csproj"
```

## <a name="access-diagnostic-logs"></a>访问诊断日志

ASP.NET Core 提供[应用服务的内置日志记录提供程序](https://docs.microsoft.com/aspnet/core/fundamentals/logging/#azure-app-service)。 在项目的*Program.cs*中，通过扩展方法将提供程序添加到应用程序 `ConfigureLogging` ，如以下示例中所示：

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureLogging(logging =>
        {
            logging.AddAzureWebAppDiagnostics();
        })
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

然后，你可以配置和生成具有[标准 .Net Core 模式](https://docs.microsoft.com/aspnet/core/fundamentals/logging)的日志。

[!INCLUDE [Access diagnostic logs](../../includes/app-service-web-logs-access-no-h.md)]

有关应用服务中 ASP.NET Core 应用疑难解答的详细信息，请参阅对[Azure App Service 和 IIS 上的 ASP.NET Core 进行故障排除](https://docs.microsoft.com/aspnet/core/test/troubleshoot-azure-iis)

## <a name="get-detailed-exceptions-page"></a>获取详细的异常页

当你的 ASP.NET Core 应用在 Visual Studio 调试器中生成异常时，浏览器将显示一个详细的异常页，但在应用服务中，此页将替换为一般的**HTTP 500**错误，否则在**处理请求时出错。** 消息。 若要在应用服务中显示详细的异常页面，请 `ASPNETCORE_ENVIRONMENT` 在<a target="_blank" href="https://shell.azure.com" >Cloud Shell</a>中运行以下命令，将应用设置添加到应用。

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings ASPNETCORE_ENVIRONMENT="Development"
```

## <a name="detect-https-session"></a>检测 HTTPS 会话

在应用服务中，[SSL 终止](https://wikipedia.org/wiki/TLS_termination_proxy)在网络负载均衡器上发生，因此，所有 HTTPS 请求将以未加密的 HTTP 请求形式访问你的应用。 如果应用逻辑需要知道用户请求是否已加密，请在*Startup.cs*中配置转发的标头中间件：

- 使用 [ForwardedHeadersOptions](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) 配置中间件，以转接 `Startup.ConfigureServices` 中的 `X-Forwarded-For` 和 `X-Forwarded-Proto` 标头。
- 向已知网络添加专用 IP 地址范围，以便中间件可以信任应用服务负载均衡器。
- 调用其他中间件之前，调用中的[UseForwardedHeaders](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders)方法 `Startup.Configure` 。

将所有这三个元素放在一起，代码类似于以下示例：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.Configure<ForwardedHeadersOptions>(options =>
    {
        options.ForwardedHeaders =
            ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto;
        options.KnownNetworks.Add(new IPNetwork(IPAddress.Parse("::ffff:10.0.0.0"), 104));
        options.KnownNetworks.Add(new IPNetwork(IPAddress.Parse("::ffff:192.168.0.0"), 112));
        options.KnownNetworks.Add(new IPNetwork(IPAddress.Parse("::ffff:172.16.0.0"), 108));
    });
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseForwardedHeaders();

    ...

    app.UseMvc();
}
```

有关详细信息，请参阅[配置 ASP.NET Core 以使用代理服务器和负载均衡器](https://docs.microsoft.com/aspnet/core/host-and-deploy/proxy-load-balancer)。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [教程：将 ASP.NET Core 应用与 SQL 数据库配合使用](app-service-web-tutorial-dotnetcore-sqldb.md)
