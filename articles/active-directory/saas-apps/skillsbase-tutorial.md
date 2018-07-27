---
title: 教程：Azure Active Directory 与 Skills Base 集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 Skills Base 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 237d90c4-8243-4f80-a305-b5ad9204159e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2018
ms.author: jeedes
ms.openlocfilehash: 84aac0017496c50f0006fd6e184537e4c14f10c7
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2018
ms.locfileid: "39057609"
---
# <a name="tutorial-azure-active-directory-integration-with-skills-base"></a>教程：Azure Active Directory 与 Skills Base 集成

本教程介绍如何将 Skills Base 与 Azure Active Directory (Azure AD) 集成。

将 Skills Base 与 Azure AD 集成可提供以下优势：

- 可以在 Azure AD 中控制谁有权访问 Skills Base。
- 可以让用户使用其 Azure AD 帐户自动登录到 Skills Base（单一登录）。
- 可在中心位置（即 Azure 门户）管理帐户。

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 Skills Base 的集成，需要以下项：

- Azure AD 订阅
- 已启用 Skills Base 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以[获取一个月的试用版](https://azure.microsoft.com/pricing/free-trial/)。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 Skills Base
2. 配置和测试 Azure AD 单一登录

## <a name="adding-skills-base-from-the-gallery"></a>从库中添加 Skills Base
若要配置 Skills Base 与 Azure AD 的集成，需要从库中将 Skills Base 添加到托管 SaaS 应用列表。

**若要从库中添加 Skills Base，请执行以下步骤：**

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![“Azure Active Directory”按钮][1]

2. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![“企业应用程序”边栏选项卡][2]
    
3. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![“新增应用程序”按钮][3]

4. 在搜索框中，键入“Skills Base”，在结果面板中选择“Skills Base”，然后单击“添加”按钮添加该应用程序。

    ![结果列表中的 Skills Base](./media/skillsbase-tutorial/tutorial_skillsbase_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录

在本部分中，将基于名为“Britta Simon”的测试用户配置和测试 Skills Base 的 Azure AD 单一登录。

若要运行单一登录，Azure AD 需要知道与 Azure AD 用户相对应的 Skills Base 用户。 换句话说，需要在 Azure AD 用户与 Skills Base 中的相关用户之间建立链接关系。

若要配置和测试 Skills Base 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configure-azure-ad-single-sign-on)** - 使用户能够使用此功能。
2. **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
3. **[创建 Skills Base 测试用户](#create-a-skills-base-test-user)** - 在 Skills Base 中创建 Britta Simon 的对应用户，并将其关联到该用户的 Azure AD 表示形式。
4. **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 Britta Simon 能够使用 Azure AD 单一登录。
5. **[测试单一登录](#test-single-sign-on)** - 验证配置是否正常工作。

### <a name="configure-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，将在 Azure 门户中启用 Azure AD 单一登录并在 Skills Base 应用程序中配置单一登录。

**若要配置 Skills Base 的 Azure AD 单一登录，请执行以下步骤：**

1. 在 Azure 门户中的 **Skills Base** 应用程序集成页上，单击“单一登录”。

    ![配置单一登录链接][4]

2. 在“单一登录”对话框中，选择“基于 SAML 的单一登录”作为“模式”以启用单一登录。
 
    ![“单一登录”对话框](./media/skillsbase-tutorial/tutorial_skillsbase_samlbase.png)

3. 在“Skills Base 域和 URL”部分中，执行以下步骤：

    ![Skills Base 域和 URL 单一登录信息](./media/skillsbase-tutorial/tutorial_skillsbase_url.png)

    在“登录 URL”文本框中，使用以下模式键入 URL： `https://app.skills-base.com/o/<customer-unique-key>`

    > [!NOTE] 
    > 登录 URL 值不是实际值。 请使用实际登录 URL 更新此值。 若要获取此值，请与 [Skills Base 客户端支持团队](mailto:support@skills-base.com)联系。

4. 在“SAML 签名证书”部分中，单击“元数据 XML”，并在计算机上保存元数据文件。

    ![证书下载链接](./media/skillsbase-tutorial/tutorial_skillsbase_certificate.png) 

5. 单击“保存”按钮。

    ![配置单一登录“保存”按钮](./media/skillsbase-tutorial/tutorial_general_400.png)

6. 在另一个 Web 浏览器窗口中，以安全管理员身份登录到 Skills Base。

7. 在菜单左侧的“管理”下，单击“身份验证”。

    ![管理员](./media/skillsbase-tutorial/tutorial_skillsbase_auth.png)

8. 在“身份验证”页上，选择“单一登录”作为 **SAML 2**。

    ![单人](./media/skillsbase-tutorial/tutorial_skillsbase_single.png)

9. 在“身份验证”页上，执行以下步骤：

    ![单人](./media/skillsbase-tutorial/tutorial_skillsbase_save.png)

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。 单击“状态”选项旁边的“更新 IdP 元数据”按钮，并将从 Azure 门户下载的元数据 XML 的内容粘贴到指定的文本框中。

    > [!Note]
    > 还可以通过上面屏幕截图中突出显示的“元数据验证器”工具验证 idp 元数据。

    b. 单击“ **保存**”。
    

### <a name="create-an-azure-ad-test-user"></a>创建 Azure AD 测试用户

本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

   ![创建 Azure AD 测试用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 Azure 门户的左窗格中，单击“Azure Active Directory”按钮。

    ![“Azure Active Directory”按钮](./media/skillsbase-tutorial/create_aaduser_01.png)

2. 若要显示用户列表，请转到“用户和组”，然后单击“所有用户”。

    ![“用户和组”以及“所有用户”链接](./media/skillsbase-tutorial/create_aaduser_02.png)

3. 若要打开“用户”对话框，在“所有用户”对话框顶部单击“添加”。

    ![“添加”按钮](./media/skillsbase-tutorial/create_aaduser_03.png)

4. 在“用户”对话框中，执行以下步骤：

    ![“用户”对话框](./media/skillsbase-tutorial/create_aaduser_04.png)

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。 在“姓名”框中，键入“BrittaSimon”。

    b. 在“用户名”框中，键入用户 Britta Simon 的电子邮件地址。

    c. 选中“显示密码”复选框，然后记下“密码”框中显示的值。

    d. 单击“创建”。
 
### <a name="create-a-skills-base-test-user"></a>创建 Skills Base 测试用户

本部分的目的是在 Skills Base 中创建名为 Britta Simon 的用户。 Skills Base 支持在默认情况下启用的实时预配。 此部分不存在任何操作项。 尝试访问 Skills Base 期间，如果该用户尚不存在，则将创建一个新用户。

>[!Note]
>如果需要手动创建用户，请联系 [Skills Base 客户端支持团队](mailto:support@skills-base.com)。

### <a name="assign-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过授予 Britta Simon 访问 Skills Base 的权限，允许其使用 Azure 单一登录。

![分配用户角色][200] 

**若要将 Britta Simon 分配到 Skills Base，请执行以下步骤：**

1. 在 Azure 门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”，并单击“所有应用程序”。

    ![分配用户][201] 

2. 在应用程序列表中，选择“Skills Base”。

    ![应用程序列表中的 Skills Base 链接](./media/skillsbase-tutorial/tutorial_skillsbase_app.png)  

3. 在左侧菜单中，单击“用户和组”。

    ![“用户和组”链接][202]

4. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![“添加分配”窗格][203]

5. 在“用户和组”对话框的“用户”列表中，选择“Britta Simon”。

6. 在“用户和组”对话框中单击“选择”按钮。

7. 在“添加分配”对话框中单击“分配”按钮。
    
### <a name="test-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

单击访问面板中的“Skills Base”磁贴时，用户应自动登录到 Skills Base 应用程序。
有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](../active-directory-saas-access-panel-introduction.md)（访问面板简介）。 

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [什么是使用 Azure Active Directory 的应用程序访问和单一登录？](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/skillsbase-tutorial/tutorial_general_01.png
[2]: ./media/skillsbase-tutorial/tutorial_general_02.png
[3]: ./media/skillsbase-tutorial/tutorial_general_03.png
[4]: ./media/skillsbase-tutorial/tutorial_general_04.png

[100]: ./media/skillsbase-tutorial/tutorial_general_100.png

[200]: ./media/skillsbase-tutorial/tutorial_general_200.png
[201]: ./media/skillsbase-tutorial/tutorial_general_201.png
[202]: ./media/skillsbase-tutorial/tutorial_general_202.png
[203]: ./media/skillsbase-tutorial/tutorial_general_203.png
