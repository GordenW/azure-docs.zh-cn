---
title: 教程：Azure Active Directory 与 ExpenseIn 集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 ExpenseIn 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 6ac8053b-a216-45d8-bf5e-ecd37d808e57
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 07/17/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 377499b1dd263398e1be42379f8db60e8a0477f9
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/23/2020
ms.locfileid: "87017493"
---
# <a name="tutorial-integrate-expensein-with-azure-active-directory"></a>教程：将 ExpenseIn 与 Azure Active Directory 集成

本教程介绍如何将 ExpenseIn 与 Azure Active Directory (Azure AD) 集成。 将 ExpenseIn 与 Azure AD 集成后，可以：

* 在 Azure AD 中控制谁有权访问 ExpenseIn。
* 让用户使用其 Azure AD 帐户自动登录到 ExpenseIn。
* 在一个中心位置（Azure 门户）管理帐户。

若要了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)。

## <a name="prerequisites"></a>先决条件

若要开始操作，需备齐以下项目：

* 一个 Azure AD 订阅。 如果没有订阅，可以获取一个[免费帐户](https://azure.microsoft.com/free/)。
* 已启用 ExpenseIn 单一登录 (SSO) 的订阅。

## <a name="scenario-description"></a>方案描述

本教程在测试环境中配置并测试 Azure AD SSO。 
* ExpenseIn 支持 SP 和 IDP 发起的 SSO  。
* 配置 ExpenseIn 后，可以强制实施会话控制，实时防止组织的敏感数据外泄和渗透。 会话控制从条件访问扩展而来。 [了解如何通过 Microsoft Cloud App Security 强制实施会话控制](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)。


## <a name="adding-expensein-from-the-gallery"></a>从库中添加 ExpenseIn

要配置 ExpenseIn 与 Azure AD 的集成，需要从库中将 ExpenseIn 添加到托管 SaaS 应用列表。

1. 使用工作或学校帐户或个人 Microsoft 帐户登录到 [Azure 门户](https://portal.azure.com)。
1. 在左侧导航窗格中，选择“Azure Active Directory”服务。
1. 导航到“企业应用程序”，选择“所有应用程序” 。
1. 若要添加新的应用程序，请选择“新建应用程序”。
1. 在“从库中添加”部分的搜索框中，键入“ExpenseIn” 。
1. 从结果面板中选择“ExpenseIn”，然后添加该应用。 在该应用添加到租户时等待几秒钟。

## <a name="configure-and-test-azure-ad-sso-for-expensein"></a>配置并测试 ExpenseIn 的 Azure AD SSO

使用名为 B.Simon 的测试用户配置和测试 ExpenseIn 的 Azure AD SSO。 若要运行 SSO，需要在 Azure AD 用户与 ExpenseIn 相关用户之间建立链接关系。

若要配置和测试 ExpenseIn 的 Azure AD SSO，请完成以下构建基块：

1. **[配置 Azure AD SSO](#configure-azure-ad-sso)** ，使用户能够使用此功能。
    1. **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** ，以使用 B.Simon 测试 Azure AD 单一登录。
    1. **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** ，使 B.Simon 能够使用 Azure AD 单一登录。
1. [配置 ExpenseIn SSO](#configure-expensein-sso)，以在应用程序端配置 SSO 设置。
    1. **[创建 ExpenseIn 测试用户](#create-expensein-test-user)** ，以便在 ExpenseIn 中创建 B.Simon 的对应用户，并将其链接到用户的 Azure AD 表示形式。
1. **[测试 SSO](#test-sso)** ，验证配置是否正常工作。

## <a name="configure-azure-ad-sso"></a>配置 Azure AD SSO

按照下列步骤在 Azure 门户中启用 Azure AD SSO。

1. 在 [Azure 门户](https://portal.azure.com/)的“ExpenseIn”应用程序集成页上，找到“管理”部分，选择“单一登录”  。
1. 在“选择单一登录方法”页上选择“SAML” 。
1. 在“设置 SAML 单一登录”页上，单击“基本 SAML 配置”的编辑/笔形图标以编辑设置 。

   ![编辑基本 SAML 配置](common/edit-urls.png)

4. 在“基本 SAML 配置”部分中，用户不必执行任何步骤，因为该应用已经与 Azure 预先集成。

5. 如果要在 SP 发起的模式下配置应用程序，请单击“设置其他 URL”，并执行以下步骤：

    在“登录 URL”文本框中，键入 URL：`https://app.expensein.com/saml`

1. 在“设置 SAML 单一登录”页的“SAML 签名证书”部分中，单击复制按钮，以复制“应用联合元数据 URL”，并单击“下载”以下载“证书(Base64)”并将其保存在计算机上。    

   ![证书下载链接](./media/expensein-tutorial/copy-metdataurl-certificate.png)

1. 在“设置 ExpenseIn”部分中，根据要求复制相应 URL。

   ![复制配置 URL](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>创建 Azure AD 测试用户

在本部分，我们将在 Azure 门户中创建名为 B.Simon 的测试用户。

1. 在 Azure 门户的左侧窗格中，依次选择“Azure Active Directory”、“用户”和“所有用户”  。
1. 选择屏幕顶部的“新建用户”。
1. 在“用户”属性中执行以下步骤：
   1. 在“名称”字段中，输入 `B.Simon`。  
   1. 在“用户名”字段中输入 username@companydomain.extension。 例如，`B.Simon@contoso.com`。
   1. 选中“显示密码”复选框，然后记下“密码”框中显示的值。
   1. 单击“创建”。

### <a name="assign-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过授予 B.Simon 访问 ExpenseIn 的权限，允许其使用 Azure 单一登录。

1. 在 Azure 门户中，依次选择“企业应用程序”、“所有应用程序”。 
1. 在应用程序列表中，选择“ExpenseIn”。
1. 在应用的概述页中，找到“管理”部分，选择“用户和组” 。

   ![“用户和组”链接](common/users-groups-blade.png)

1. 选择“添加用户”，然后在“添加分配”对话框中选择“用户和组”。

    ![“添加用户”链接](common/add-assign-user.png)

1. 在“用户和组”对话框中，从“用户”列表中选择“B.Simon”，然后单击屏幕底部的“选择”按钮。
1. 如果在 SAML 断言中需要任何角色值，请在“选择角色”对话框的列表中为用户选择合适的角色，然后单击屏幕底部的“选择”按钮。
1. 在“添加分配”对话框中，单击“分配”按钮。


## <a name="configure-expensein-sso"></a>配置 ExpenseIn SSO

1. 打开新的 Web 浏览器窗口，以管理员身份登录到 ExpenseIn 公司站点。

1. 单击页面顶部的“管理员”并导航到“单一登录”，然后单击“添加提供程序”  。

     ![ExpenseIn 配置](./media/expenseIn-tutorial/config01.png)

1. 在“新建标识提供者”弹出窗口中，执行以下步骤：

    ![ExpenseIn 配置](./media/expenseIn-tutorial/config02.png)

    a. 在“提供者名称”文本框中键入名称，例如 Azure。

    b. 对于“允许提供商发起的登录”选择“是”。

    c. 在“目标 URL”文本框中，粘贴从 Azure 门户复制的“登录 URL”值 。

    d. 在“颁发者”文本框中，粘贴从 Azure 门户复制的“Azure AD 标识符”值 。

    e. 在记事本中打开“证书(Base64)”文件，复制其内容并将其粘贴到“证书”文本框中。

    f. 单击“创建”。

### <a name="create-expensein-test-user"></a>创建 ExpenseIn 测试用户

要使 Azure AD 用户能够登录到 ExpenseIn，必须将其预配到 ExpenseIn 中。 在 ExpenseIn 中，预配属于手动任务。

**若要预配用户帐户，请执行以下步骤：**

1. 以管理员身份登录到 ExpenseIn。

2. 单击页面顶部的“管理员”并导航到“用户”，然后单击“新建用户”  。

     ![ExpenseIn 配置](./media/expenseIn-tutorial/config03.png)

3. 在“详细信息”弹出窗口中，执行以下步骤：

    ![ExpenseIn 配置](./media/expenseIn-tutorial/config04.png)

    a. 在“名字”文本框中，输入用户的名字，例如 B 。

    b. 在“姓氏”文本框中，输入用户的名字，如 Simon 。

    c. 在“电子邮件”文本框中，输入用户的电子邮件，如 `B.Simon@contoso.com`。

    d. 单击“创建”。

## <a name="test-sso"></a>测试 SSO

在访问面板中选择“ExpenseIn”磁贴时，应会自动登录到设置了 SSO 的 ExpenseIn。 有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)（访问面板简介）。

## <a name="additional-resources"></a>其他资源

- [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory 的应用程序访问与单一登录是什么？](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [什么是 Azure Active Directory 中的条件访问？](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [通过 Azure AD 试用 ExpenseIn](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security 中的会话控制是什么？](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [如何通过高级可见性和控制来保护 ExpenseIn](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
