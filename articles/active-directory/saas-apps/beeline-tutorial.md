---
title: 教程：Azure Active Directory 与 Beeline 集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 Beeline 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 0726859d-1dac-44a0-810b-da56d89039ee
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/06/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: cca1b4b9f27a8711d0340389359320a2f99a918a
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/23/2020
ms.locfileid: "87018493"
---
# <a name="tutorial-azure-active-directory-integration-with-beeline"></a>教程：Azure Active Directory 与 Beeline 集成

在本教程中，了解如何将 Beeline 与 Azure Active Directory (Azure AD) 集成。
将 Beeline 与 Azure AD 集成提供以下优势：

* 可在 Azure AD 中控制谁有权访问 Beeline。
* 可让用户使用其 Azure AD 帐户自动登录 Beeline（单一登录）。
* 可在中心位置（即 Azure 门户）管理帐户。

如果要了解有关 SaaS 应用与 Azure AD 集成的更多详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)。
如果还没有 Azure 订阅，可以在开始前[创建一个免费帐户](https://azure.microsoft.com/free/)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 Beeline 的集成，需要以下项目：

* 一个 Azure AD 订阅。 如果你没有 Azure AD 环境，可以在[此处](https://azure.microsoft.com/pricing/free-trial/)获取一个月的试用版。
* 已启用 Beeline 单一登录的订阅

## <a name="scenario-description"></a>方案描述

本教程会在测试环境中配置和测试 Azure AD 单一登录。

* Beeline 仅支持 IDP 发起的 SSO

## <a name="adding-beeline-from-the-gallery"></a>从库中添加 Beeline

要配置 Beeline 与 Azure AD 的集成，需要将库中的 Beeline 添加到托管的 SaaS 应用列表。

**若要从库中添加 Beeline，请执行以下步骤：**

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”  图标。

    ![“Azure Active Directory”按钮](common/select-azuread.png)

2. 转到“企业应用”，并选择“所有应用”选项   。

    ![“企业应用程序”边栏选项卡](common/enterprise-applications.png)

3. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”  按钮。

    ![“新增应用程序”按钮](common/add-new-app.png)

4. 在搜索框中键入“Beeline”，从结果面板中选择“Beeline”，然后单击“添加”按钮以添加该应用程序  。

     ![结果列表中的 Beeline](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录

在本部分中，基于名为“Britta Simon”的测试用户配置和测试 Beeline 的 Azure AD 单一登录。
若要运行单一登录，需要在 Azure AD 用户与 Beeline 中的相关用户之间建立链接关系。

若要通过 Beeline 配置和测试 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configure-azure-ad-single-sign-on)** - 使用户能够使用此功能。
2. **[配置 Beeline 单一登录](#configure-beeline-single-sign-on)** - 在应用程序端配置单一登录。
3. **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
4. **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 Britta Simon 能够使用 Azure AD 单一登录。
5. **[创建 Beeline 测试用户](#create-beeline-test-user)** - 在 Beeline 中创建 Britta Simon 的对应用户，并将其链接到该用户的 Azure AD 表示形式。
6. **[测试单一登录](#test-single-sign-on)** - 验证配置是否正常工作。

### <a name="configure-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，将在 Azure 门户中启用 Azure AD 单一登录。

若要配置 Beeline 的 Azure AD 单一登录，请执行以下步骤：

1. 在 [Azure 门户](https://portal.azure.com/)中的“Beeline”应用程序集成页上，单击“单一登录” 。

    ![配置单一登录链接](common/select-sso.png)

2. 在**选择单一登录方法**对话框中，选择 **SAML/WS-Fed**模式以启用单一登录。

    ![单一登录选择模式](common/select-saml-option.png)

3. 在“使用 SAML 设置单一登录”页上，单击“编辑”图标以打开“基本 SAML 配置”对话框    。

    ![编辑基本 SAML 配置](common/edit-urls.png)

4. 在“设置 SAML 单一登录”页上，执行以下步骤  ：

    ![BeeLine 域和 URL 单一登录信息](common/idp-intiated.png)

    a. 在“标识符”  文本框中，使用以下模式键入 URL：`https://projects.beeline.com/<ProjInstanceName>`

    b. 在“回复 URL”文本框中，使用以下模式键入 URL：

    ```https
    https://projects.beeline.com/<ProjInstanceName>/SSO_External.ashx
    ```

    > [!NOTE]
    > 这些不是实际值。 请使用实际标识符和回复 URL 更新这些值。 请联系 [Beeline 客户端支持团队](https://www.beeline.com/support-beeline/)来获取这些值。 还可以参考 Azure 门户中的“基本 SAML 配置”  部分中显示的模式。

5. Beeline 应用程序需要采用特定格式的 SAML 断言。 请先与 [Beeline 支持团队](https://www.beeline.com/support-beeline/)协作，识别将映射到该应用程序的正确用户标识符。 此外，请接受 [ 支持团队](https://www.beeline.com/support-beeline/)的指导，了解其要用于此映射的属性。 可从应用程序的“用户属性”  选项卡管理此属性的值。 以下屏幕截图显示一个示例。 此处我们已映射了带有“userprincipalname”属性的“用户标识符”声明，这可提供唯一用户 ID，也将在每个后续 SAML 响应中发送到 Beeline 应用程序。

    ![image](common/edit-attribute.png)

6. 在“使用 SAML 设置单一登录”页的“SAML 签名证书”部分，单击“下载”以根据要求下载从给定选项提供的“联合元数据 XML”并将其保存在计算机上     。

    ![证书下载链接](common/metadataxml.png)

7. 在 [Azure 门户](https://portal.azure.com/)中的“Beeline”应用程序集成页上，选择“属性”并复制用户访问 URL。 

    ![复制用户访问 URL](media/beeline-tutorial/client-access-url.png)


### <a name="configure-beeline-single-sign-on"></a>配置 Beeline 单一登录

若要在 Beeline 端配置单一登录，需要将下载的“联合元数据 XML”以及来自 Azure 门户属性的用户访问 URL 发送给 [Beeline 支持团队](https://www.beeline.com/support-beeline/) 。 他们需要元数据和用户访问 URL，以便在两个端均正确配置 SAML SSO 连接。

### <a name="create-an-azure-ad-test-user"></a>创建 Azure AD 测试用户

本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

1. 在 Azure 门户的左侧窗格中，依次选择“Azure Active Directory”、“用户”和“所有用户”。

    ![“用户和组”以及“所有用户”链接](common/users.png)

2. 选择屏幕顶部的“新建用户”。

    ![“新建用户”按钮](common/new-user.png)

3. 在“用户属性”中，按照以下步骤操作。

    ![“用户”对话框](common/user-properties.png)

    a. 在“名称”字段中，输入 BrittaSimon。
  
    b. 在“用户名”字段中，键入 brittasimon\@yourcompanydomain.extension  
    例如： BrittaSimon@contoso.com

    c. 选中“显示密码”复选框，然后记下“密码”框中显示的值。

    d. 单击“创建”。

### <a name="assign-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过授予 Britta Simon 对 Beeline 的访问权限，使其能够使用 Azure 单一登录。

1. 在 Azure 门户中，依次选择“企业应用程序”、“所有应用程序”和“Beeline”  。

    ![“企业应用程序”边栏选项卡](common/enterprise-applications.png)

2. 在应用程序列表中，选择“Beeline”。

    ![应用程序列表中的 Beeline 链接](common/all-applications.png)

3. 在左侧菜单中，选择“用户和组”。

    ![“用户和组”链接](common/users-groups-blade.png)

4. 单击“添加用户”按钮，然后在“添加分配”对话框中选择“用户和组”。

    ![“添加分配”窗格](common/add-assign-user.png)

5. 在“用户和组”对话框中，选择“用户”列表中的 Britta Simon，然后单击屏幕底部的“选择”按钮。

6. 如果你在 SAML 断言中需要任何角色值，请在“选择角色”对话框中从列表中为用户选择合适的角色，然后单击屏幕底部的“选择”按钮。

7. 在“添加分配”对话框中，单击“分配”按钮。

### <a name="create-beeline-test-user"></a>创建 Beeline 测试用户

在本部分中，在 Beeline 中创建用户 Britta Simon。 在执行单一登录前，Beeline 应用程序需要所有用户在应用程序中均进行了预配。 因此，请与 [Beeline 支持团队](https://www.beeline.com/support-beeline/)协作，将所有这些用户预配到应用程序中。

### <a name="test-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

单击访问面板中的 Beeline 磁贴时，应会自动登录到在其中设置了 SSO 的 Beeline 实例。 有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)（访问面板简介）。

## <a name="additional-resources"></a>其他资源

- [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory 的应用程序访问与单一登录是什么？](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [什么是 Azure Active Directory 中的条件访问？](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
