---
title: 在 Azure 应用服务中为 .NET 应用启用 Snapshot Debugger | Microsoft Docs
description: 在 Azure 应用服务中为 .NET 应用启用快照调试器
ms.topic: conceptual
author: brahmnes
ms.author: bfung
ms.date: 03/26/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 6928da704236c4bb5492f99a4a5327bf297a323d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "84676830"
---
# <a name="enable-snapshot-debugger-for-net-apps-in-azure-app-service"></a>在 Azure 应用服务中为 .NET 应用启用快照调试器

快照调试器当前适用于按 Windows 服务计划在 Azure 应用服务上运行的 ASP.NET 和 ASP.NET Core 应用。

## <a name="enable-snapshot-debugger"></a><a id="installation"></a>启用 Snapshot Debugger
若要为应用启用快照调试器，请遵循下面的说明。 如果你在运行另一种类型的 Azure 服务，则下面提供了用于在其他受支持平台上启用快照调试器的说明：
* [Azure 云服务](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Azure Service Fabric 服务](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Azure 虚拟机和虚拟机规模集](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [本地虚拟机或物理计算机](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)

如果使用的是 .NET Core 预览版，请按照[为其他环境启用 Snapshot Debugger 的说明](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)首先将 [Microsoft.ApplicationInsights.SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet 包包含在应用程序中，然后完成下面的其余说明。 

预安装 Application Insights 快照调试器作为应用程序服务运行时的一部分，但需启用它才能获得适用于应用服务应用的快照。 部署应用后，即使在源代码中包括了 Application Insights SDK，也要执行以下步骤来启用快照调试器。

1. 导航到应用服务的 Azure 控制面板。
2. 请 **> Application Insights** "页中转到" 设置 "。

   ![在应用服务门户上启用 App Insights](./media/snapshot-debugger/applicationinsights-appservices.png)

3. 按照页面上的说明创建新资源，或选择现有的 App Insights 资源来监视应用。 另外，请确保快照调试器的两个开关都为“开”  。

   ![添加 App Insights 站点扩展][Enablement UI]

4. 现已使用应用服务应用设置启用了快照调试器。

    ![快照调试器的应用设置][snapshot-debugger-app-setting]

## <a name="disable-snapshot-debugger"></a>禁用快照调试器

执行与**启用快照调试器**相同的步骤，但将快照调试器的两个开关都切换到“关”  。
我们建议在所有应用上启用快照调试器，以简化对应用程序异常的诊断。

## <a name="azure-resource-manager-template"></a>Azure 资源管理器模板

对于 Azure App Service，可以在 Azure 资源管理器模板中设置应用设置，以启用 Snapshot Debugger 和探查器。 将包含应用设置的配置资源添加为网站的子资源：

```json
{
  "apiVersion": "2015-08-01",
  "name": "[parameters('webSiteName')]",
  "type": "Microsoft.Web/sites",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[variables('hostingPlanName')]"
  ],
  "tags": { 
    "[concat('hidden-related:', resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName')))]": "empty",
    "displayName": "Website"
  },
  "properties": {
    "name": "[parameters('webSiteName')]",
    "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]"
  },
  "resources": [
    {
      "apiVersion": "2015-08-01",
      "name": "appsettings",
      "type": "config",
      "dependsOn": [
        "[parameters('webSiteName')]",
        "[concat('AppInsights', parameters('webSiteName'))]"
      ],
      "properties": {
        "APPINSIGHTS_INSTRUMENTATIONKEY": "[reference(resourceId('Microsoft.Insights/components', concat('AppInsights', parameters('webSiteName'))), '2014-04-01').InstrumentationKey]",
        "APPINSIGHTS_PROFILERFEATURE_VERSION": "1.0.0",
        "APPINSIGHTS_SNAPSHOTFEATURE_VERSION": "1.0.0",
        "DiagnosticServices_EXTENSION_VERSION": "~3",
        "ApplicationInsightsAgent_EXTENSION_VERSION": "~2"
      }
    }
  ]
},
```

## <a name="next-steps"></a>后续步骤

- 为应用程序生成可触发异常的流量。 然后等待 10 到 15 分钟，这样快照就会发送到 Application Insights 实例。
- 请参见 Azure 门户中的[快照](snapshot-debugger.md?toc=/azure/azure-monitor/toc.json#view-snapshots-in-the-portal)。
- 排查 Snapshot Debugger 问题时如需帮助，请参阅 [ Snapshot Debugger 故障排除](snapshot-debugger-troubleshoot.md?toc=/azure/azure-monitor/toc.json)。

[Enablement UI]: ./media/snapshot-debugger/enablement-ui.png
[snapshot-debugger-app-setting]:./media/snapshot-debugger/snapshot-debugger-app-setting.png

