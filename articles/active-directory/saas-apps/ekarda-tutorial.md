---
title: 教程：Azure Active Directory 单一登录 (SSO) 与 Ekarda 的集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 Ekarda 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 8e2945aa-46fc-41bc-a530-3807a5dcb76a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 06/15/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: aacaec5ff632385a1f1686610370bb92eb63c349
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/23/2020
ms.locfileid: "87017544"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-ekarda"></a>教程：Azure Active Directory 单一登录 (SSO) 与 Ekarda 的集成

本教程介绍如何将 Ekarda 与 Azure Active Directory (Azure AD) 集成。 将 Ekarda 与 Azure AD 集成后，可以：

* 在 Azure AD 中控制谁有权访问 Ekarda。
* 让用户使用其 Azure AD 帐户自动登录 Ekarda。
* 在一个中心位置（Azure 门户）管理帐户。

若要了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)。

## <a name="prerequisites"></a>先决条件

若要开始操作，需备齐以下项目：

* 一个 Azure AD 订阅。 如果没有订阅，可以获取一个[免费帐户](https://azure.microsoft.com/free/)。
* 已启用单一登录 (SSO) 的 Ekarda 订阅。

## <a name="scenario-description"></a>方案描述

本教程在测试环境中配置并测试 Azure AD SSO。

* Ekarda 支持 SP 和 IDP 发起的 SSO
* Ekarda 支持实时用户预配

* 配置 Ekarda 后，就可以强制实施会话控制，实时防止组织的敏感数据外泄和渗透。 会话控制从条件访问扩展而来。 [了解如何通过 Microsoft Cloud App Security 强制实施会话控制](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)。

## <a name="adding-ekarda-from-the-gallery"></a>从库中添加 Ekarda

要配置 Ekarda 与 Azure AD 的集成，需要从库中将 Ekarda 添加到托管 SaaS 应用列表。

1. 使用工作或学校帐户或个人 Microsoft 帐户登录到 [Azure 门户](https://portal.azure.com)。
1. 在左侧导航窗格中，选择“Azure Active Directory”服务。
1. 导航到“企业应用程序”，选择“所有应用程序” 。
1. 若要添加新的应用程序，请选择“新建应用程序”。
1. 在“从库中添加”部分的搜索框中，键入“Ekarda” 。
1. 在结果面板中选择“Ekarda”，然后添加该应用。 在该应用添加到租户时等待几秒钟。


## <a name="configure-and-test-azure-ad-single-sign-on-for-ekarda"></a>配置并测试 Ekarda 的 Azure AD 单一登录

使用名为 B.Simon 的测试用户配置和测试 Ekarda 的 Azure AD SSO。 要使 SSO 正常工作，需要在 Azure AD 用户与 Ekarda 中的相关用户之间建立链接关系。

要配置和测试 Ekarda 的 Azure AD SSO，请完成以下构建基块：

1. **[配置 Azure AD SSO](#configure-azure-ad-sso)** - 使用户能够使用此功能。
    1. **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 B. Simon 测试 Azure AD 单一登录。
    1. **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 B. Simon 能够使用 Azure AD 单一登录。
1. **[配置 Ekarda SSO](#configure-ekarda-sso)** - 在应用程序端配置单一登录设置。
    1. **[创建 Ekarda 测试用户](#create-ekarda-test-user)** - 在 Ekarda 中创建 B.Simon 的对应用户，并将其链接到用户的 Azure AD 表示形式。
1. **[测试 SSO](#test-sso)** - 验证配置是否正常工作。

## <a name="configure-azure-ad-sso"></a>配置 Azure AD SSO

按照下列步骤在 Azure 门户中启用 Azure AD SSO。

1. 在 [Azure 门户](https://portal.azure.com/)的“Ekarda”应用程序集成页上，找到“管理”部分并选择“单一登录”  。
1. 在“选择单一登录方法”页上选择“SAML” 。
1. 在“使用 SAML 设置单一登录”页上，单击“基本 SAML 配置”的编辑/笔形图标以编辑设置 。

   ![编辑基本 SAML 配置](common/edit-urls.png)

1. 在“基本 SAML 配置”部分，如果有**服务提供程序元数据文件**，请执行以下步骤：
    
    a. 单击“上传元数据文件”。

    b. 单击“文件夹徽标”来选择元数据文件并单击“上传”。

    c. 成功上传元数据文件后，“标识符”和“回复 URL”值会自动填充在“Ekarda”部分的文本框中 ：

    > [!Note]
    > 如果“标识符”和“回复 URL”值未自动填充，请根据要求手动填充这些值。

1. 在没有“服务提供程序元数据文件”的情况下，如果要在“IDP”发起的模式下配置应用程序，请在“基本 SAML 配置”部分输入以下字段的值  ：

    a. 在“标识符”文本框中，使用以下模式键入 URL：`https://my.ekarda.com/users/saml_metadata/<COMPANY_ID>`

    b. 在“回复 URL”文本框中，使用以下模式键入 URL：`https://my.ekarda.com/users/saml_acs/<COMPANY_ID>`

1. 如果要在 SP 发起的模式下配置应用程序，请单击“设置其他 URL”，并执行以下步骤：

    在“登录 URL”文本框中，使用以下模式键入 URL：`https://my.ekarda.com/users/saml_sso/<COMPANY_ID>`

    > [!NOTE]
    > 这些不是实际值。 请使用实际的“标识符”、“回复 URL”和“登录 URL”更新这些值。 请联系 [Ekarda 客户端支持团队](mailto:contact@ekarda.com)获取这些值。 还可以参考 Azure 门户中的“基本 SAML 配置”部分中显示的模式。

1. 在“使用 SAML 设置单一登录”页的“SAML 签名证书”部分中，单击复制按钮以复制“证书(Base64)”并将其保存在计算机上  。

    ![证书下载链接](common/certificatebase64.png)

1. 在“设置 Ekarda”部分中，根据要求复制相应的 URL。

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

在本部分中，将通过授予 B.Simon 访问 Ekarda 的权限，允许其使用 Azure 单一登录。

1. 在 Azure 门户中，依次选择“企业应用程序”、“所有应用程序”。
1. 在应用程序列表中，选择“Ekarda”。
1. 在应用的概述页中，找到“管理”部分，选择“用户和组” 。

   ![“用户和组”链接](common/users-groups-blade.png)

1. 选择“添加用户”，然后在“添加分配”对话框中选择“用户和组”。

    ![“添加用户”链接](common/add-assign-user.png)

1. 在“用户和组”对话框中，从“用户”列表中选择“B.Simon”，然后单击屏幕底部的“选择”按钮。
1. 如果在 SAML 断言中需要任何角色值，请在“选择角色”对话框的列表中为用户选择合适的角色，然后单击屏幕底部的“选择”按钮。
1. 在“添加分配”对话框中，单击“分配”按钮。

## <a name="configure-ekarda-sso"></a>配置 Ekarda SSO

1. 在另一个 Web 浏览器窗口中，以管理员身份登录 Ekarda 公司站点。

1. 单击“管理” -> “我的帐户” 。

    ![Ekarda 配置](./media/ekarda-tutorial/ekarda.png)    

1. 在页面底部，可找到“SAML 设置”部分，并且可在其中配置 SAML 集成。
1. 在以下页上，执行以下步骤：

    ![Ekarda 配置](./media/ekarda-tutorial/ekarda1.png)

    a. 单击“服务提供程序元数据”链接，并将其保存为计算机上的文件。

    b. 选中“启用 SAML”复选框。

    c. 在“IDP 实体 ID”文本框中，粘贴从 Azure 门户复制的“实体 ID”值 。

    d. 在“IDP 登录 URL”文本框中，粘贴从 Azure 门户复制的“登录 URL”值 。

    e. 在“IDP 注销 URL”文本框中，粘贴从 Azure 门户复制的“注销 URL”值 。

    f. 在记事本中打开从 Azure 门户下载的“证书(Base64)”，将内容粘贴到“IDP x509 证书”文本框中 。

    g. 在“选项”部分中选择“启用 SLO” 。

    h. 单击“更新”。

### <a name="create-ekarda-test-user"></a>创建 Ekarda 测试用户

本部分将在 Ekarda 中创建一个名为 B.Simon 的用户。 Ekarda 支持默认已启用的实时用户预配。 此部分不存在任何操作项。 如果 Ekarda 中尚不存在用户，则会在身份验证后创建一个新用户。

## <a name="test-sso"></a>测试 SSO 

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

单击访问面板中的 Ekarda 磁贴时，应会自动登录到为其设置了 SSO 的 Ekarda。 有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)（访问面板简介）。

## <a name="additional-resources"></a>其他资源

- [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory 的应用程序访问与单一登录是什么？](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [什么是 Azure Active Directory 中的条件访问？](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [通过 Azure AD 试用 Ekarda](https://aad.portal.azure.com/)

- 使用 [Ekarda 的企业 eCard 解决方案](https://ekarda.com/ecards-ecards-with-logo-for-business-corporate-enterprise)预配任意数量的员工，将具有公司徽标品牌的 eCard 发送给其客户和同事。 详细了解如何设置 [Ekarda 作为 SSO 解决方案](https://support.ekarda.com/#SSO-Implementation)。

- [Microsoft Cloud App Security 中的会话控制是什么？](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [如何通过高级可见性和控制保护 Ekarda](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
