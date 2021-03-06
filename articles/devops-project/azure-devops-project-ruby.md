---
title: 快速入门：使用 Azure DevOps Starter 为 Ruby on Rails 创建 CI/CD 管道
description: 可以通过 Azure DevOps Starter 轻松地完成 Azure 入门。 可以快速启动 Azure 服务上的 Ruby Web 应用。
ms.prod: devops
ms.technology: devops-cicd
services: vsts
documentationcenter: vs-devops-build
author: mlearned
manager: gwallace
ms.workload: web
ms.tgt_pltfrm: na
ms.topic: quickstart
ms.date: 03/24/2020
ms.author: mlearned
ms.custom: mvc
ms.openlocfilehash: cde959d8e075b55cb6cbb37479ca49cdd8a8c0c1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "82233731"
---
# <a name="create-a-cicd-pipeline-for-ruby-on-rails-by-using-azure-devops-starter"></a>使用 Azure DevOps Starter 为 Ruby on Rails 创建 CI/CD 管道

使用 Azure DevOps Starter 配置 Ruby on Rails 应用的持续集成（CI）和持续交付（CD）。 DevOps Starter 简化了 Azure DevOps 生成和发布管道的初始配置。

如果没有 Azure 订阅，可以通过 [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) 免费获取一个。

## <a name="sign-in-to-the-azure-portal"></a>登录到 Azure 门户

Azure DevOps Starter Azure Repos 中创建 CI/CD 管道。 可以创建新的 Azure DevOps 组织，或使用现有的组织。 DevOps 入门版还将在所选的 Azure 订阅中创建 Azure 资源。

1. 登录 [Azure 门户](https://portal.azure.com)。

1. 在搜索框中键入“DevOps 入门版”，然后选择。 单击“添加”以新建一个。

    ![DevOps 入门版仪表板](_img/azure-devops-starter-aks/search-devops-starter.png) 

## <a name="select-a-sample-app-and-azure-service"></a>选择示例应用和 Azure 服务

1. 选择 **Ruby** 示例应用。

1. 选择 **Ruby on Rails** 应用程序框架。 完成后，选择“下一步”  。

1. “Linux 上的 Web 应用”  是默认的部署目标。  也可选择“适用于容器的 Web 应用”。  以前选择的应用程序框架规定了此处提供的 Azure 服务部署目标的类型。 
    
1. 选择所要的目标服务，然后选择“下一步”。 

## <a name="configure-azure-devops-and-an-azure-subscription"></a>配置 Azure DevOps 和 Azure 订阅 

1. 创建新的免费 Azure DevOps 组织，或选择现有的组织。 

1. 输入 Azure DevOps 项目的名称。 

1. 选择 Azure 订阅和位置，输入应用的名称，然后选择“完成”。   
    几分钟后，DevOps 入门仪表板将显示在 Azure 门户中。 将在 Azure DevOps 组织的存储库中设置一个示例应用，执行生成，并将应用部署到 Azure。 
    
    在此仪表板中可以查看代码存储库、CI/CD 管道，以及 Azure 中的应用。 在右侧，选择“浏览”即可查看正在运行的应用。 

    ![仪表板视图](_img/azure-devops-project-go/dashboardnopreview.png) 

## <a name="commit-your-code-changes-and-execute-the-cicd"></a>提交代码更改并执行 CI/CD

Azure DevOps Starter 在 Azure Pipelines 或 GitHub 中创建 Git 存储库。 若要查看存储库并对应用进行代码更改，请执行以下操作：

1. 在 DevOps 入门仪表板上，选择 "主分支" 的链接。 该链接会打开新建的 Git 存储库的视图。

1. 若要查看存储库克隆 URL，请在右上角选择“克隆”。  可以在常用的 IDE 中克隆 Git 存储库。 在后续几个步骤中，可以使用 Web 浏览器直接对 master 分库进行代码更改并提交所做的更改。

1. 在左侧转到 *app/views/pages/home.html.erb* 文件，然后选择“编辑”。 

1. 对该文件进行更改。 例如，在某个 div 标记内部修改某些文本。

1. 选择“提交”并保存更改。 

1. 在浏览器中，转到 DevOps 入门版仪表板。 此时应有一个生成正在进行。 所做的更改会自动通过 CI/CD 管道进行生成和部署。

## <a name="examine-the-azure-pipelines-cicd-pipeline"></a>检查 Azure Pipelines CI/CD 管道

Azure DevOps Starter 会自动在 Azure DevOps 组织中配置完整的 CI/CD 管道。 根据需要浏览和自定义管道。 若要了解 Azure DevOps 生成和发布管道，请执行以下操作：

1. 转到 DevOps Starter 仪表板。

1. 在顶部，选择“生成管道”。  浏览器标签页会显示新项目的生成管道。

1. 指向“状态”字段，然后选择省略号 (...)。菜单中会显示多个选项，例如，将新生成排队、暂停某个生成，以及编辑生成管道。

1. 选择“编辑”。

1. 在此窗格中，可以检查生成管道的各种任务。 该生成会执行各种任务，例如，从 Git 存储库提取源、还原依赖项，以及发布用于部署的输出。

1. 在生成管道的顶部，选择生成管道名称。

1. 将生成管道的名称更改为更具描述性的名称，选择“保存并排队”，然后选择“保存”。 

1. 在生成管道名称下，选择“历史记录”。 此窗格显示最近针对生成所做的更改的审核线索。 Azure DevOps 会跟踪对生成管道所做的任何更改，并允许进行版本比较。

1. 选择“触发器”。  DevOps Starter 会自动创建一个 CI 触发器，每次向存储库提交内容都会启动新的生成。 （可选）可以选择在 CI 过程中包括或排除分支。

1. 选择“保留期”。 可以根据方案指定策略，以保留或删除特定数目的生成。

1. 选择“生成和发布”，然后选择“发布”。   DevOps Starter 会创建一个发布管道，用于管理到 Azure 的部署。

1. 选择发布管道旁边的省略号 (...)，然后选择“编辑”。 发布管道包含一个*管道*，用于定义发布过程。

1. 在“项目”下选择“删除” 。 前面检查过的生成管道将生成用于项目的输出。 

1. 在“删除”图标的右侧，选择“持续部署触发器”。  此发布管道有一个已启用的 CD 触发器，每次有新的生成项目可用时，此触发器就会执行部署。 （可选）可以禁用此触发器，这样就需要手动执行部署。 

1. 在左侧，选择“任务”。**** 任务是部署过程执行的活动。 在此示例中，已创建一个用于将项目部署到 Azure 应用服务的任务。

1. 在右侧选择“查看发布”，以显示发布历史记录。

1. 选择某个发布旁边的省略号 (...)，然后选择“打开”。 可以浏览多个菜单，例如发布摘要、关联的工作项和测试。

1. 选择“提交”。 此视图显示与此部署关联的代码提交。 

1. 选择“日志”。 日志包含有关部署过程的有用信息。 可以在部署期间和之后查看日志。

## <a name="clean-up-resources"></a>清理资源

不再需要本快速入门中创建的 Azure 应用服务实例和相关资源时，可将其删除。 为此，请使用 DevOps Starter 仪表板上的“删除”功能。

## <a name="next-steps"></a>后续步骤

若要详细了解如何根据团队的需求修改生成和发布管道，请参阅：

> [!div class="nextstepaction"]
> [Define your multi-stage continuous deployment (CD) pipeline](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)（定义多阶段持续部署 (CD) 管道）
