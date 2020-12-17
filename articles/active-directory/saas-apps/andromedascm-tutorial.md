---
title: 教學課程：Azure Active Directory 與 Andromeda 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 Andromeda 之間的單一登入。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 04/16/2019
ms.author: jeedes
ms.openlocfilehash: e8cb939b48f8cfe311ec10c0850cfb234de04fad
ms.sourcegitcommit: d2d1c90ec5218b93abb80b8f3ed49dcf4327f7f4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/16/2020
ms.locfileid: "97589741"
---
# <a name="tutorial-azure-active-directory-integration-with-andromeda"></a>教學課程：Azure Active Directory 與 Andromeda

在本教學課程中，您將了解如何整合 Andromeda 與 Azure Active Directory (Azure AD)。
Andromeda 與 Azure AD 整合提供下列優點：

* 您可以在 Azure AD 中控制可存取 Andromeda 的人員。
* 您可以讓使用者使用他們的 Azure AD 帳戶自動登入 Andromeda (單一登入)。
* 您可以在 Azure 入口網站中集中管理您的帳戶。

若您想了解 SaaS app 與 Azure AD 整合的更多詳細資訊，請參閱 [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](../manage-apps/what-is-single-sign-on.md)。
如果您沒有 Azure 訂用帳戶，請在開始之前先[建立免費帳戶](https://azure.microsoft.com/free/)。

## <a name="prerequisites"></a>Prerequisites

若要設定與 Andromeda 的 Azure AD 整合，您需要下列項目：

* Azure AD 訂用帳戶。 如果您沒有 Azure AD 環境，您可以申請[免費帳戶](https://azure.microsoft.com/free/)
* 已啟用 Andromeda 單一登入的訂用帳戶

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD 單一登入。

* Andromeda 支援由 **SP 和 IDP** 起始的 SSO
* Andromeda 支援 **Just In Time** 使用者佈建

## <a name="adding-andromeda-from-the-gallery"></a>從資源庫新增 Andromeda

若要設定 Andromeda 與 Azure AD 整合，您需要從資源庫將 Andromeda 新增至受控 SaaS 應用程式清單中。

**若要從資源庫新增 Andromeda，請執行下列步驟：**

1. 在 **[Azure 入口網站](https://portal.azure.com)** 的左方瀏覽窗格中，按一下 [Azure Active Directory]  圖示。

    ![Azure Active Directory 按鈕](common/select-azuread.png)

2. 瀏覽至 [企業應用程式]  ，然後選取 [所有應用程式]  選項。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

3. 若要新增新的應用程式，請按一下對話方塊頂端的 [新增應用程式]  按鈕。

    ![新增應用程式按鈕](common/add-new-app.png)

4. 在搜尋方塊中，輸入 **Andromeda**，從結果面板中選取 [Andromeda]  ，然後按一下 [新增]  按鈕以新增應用程式。

    ![結果清單中的 Andromeda](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>設定和測試 Azure AD 單一登入

在本節中，您會以名為 **Britta Simon** 的測試使用者為基礎，設定及測試與 Andromeda 搭配運作的 Azure AD 單一登入。
若要讓單一登入能夠運作，必須建立 Azure AD 使用者與 Andromeda 中相關使用者之間的連結關聯性。

若要設定及測試與 Andromeda 搭配運作的 Azure AD 單一登入，您需要完成下列構成要素：

1. **[設定 Azure AD 單一登入](#configure-azure-ad-single-sign-on)** - 讓您的使用者能夠使用此功能。
2. **[設定 Andromeda 單一登入](#configure-andromeda-single-sign-on)** - 在應用程式端設定單一登入設定。
3. **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 Britta Simon 測試 Azure AD 單一登入。
4. **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 Britta Simon 能夠使用 Azure AD 單一登入。
5. **[建立 Andromeda 測試使用者](#create-andromeda-test-user)** - 讓 Andromeda 中對應的 Britta Simon 連結到該使用者在 Azure AD 中的代表項目。
6. **[測試單一登入](#test-single-sign-on)** ，驗證組態是否能運作。

### <a name="configure-azure-ad-single-sign-on"></a>設定 Azure AD 單一登入

在本節中，您會在 Azure 入口網站中啟用 Azure AD 單一登入。

若要使用 Andromeda 設定 Azure AD 單一登入，請執行下列步驟：

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [Andromeda]  應用程式整合頁面上，選取 [單一登入]  。

    ![設定單一登入連結](common/select-sso.png)

2. 在 [選取單一登入方法]  對話方塊中，選取 [SAML/WS-Fed]  模式以啟用單一登入。

    ![單一登入選取模式](common/select-saml-option.png)

3. 在 [以 SAML 設定單一登入] 頁面上，按一下 [編輯] 圖示以開啟 [基本 SAML 設定] 對話方塊。   

    ![編輯基本 SAML 組態](common/edit-urls.png)

4. 在 [基本 SAML 設定]  區段上，如果您想要以 **IDP** 起始模式設定應用程式，請執行下列步驟：

    ![顯示基本 SAML 設定的螢幕擷取畫面，您可以在其中輸入識別碼、回覆 URL 以及選取 [儲存]。](common/idp-intiated.png)

    a. 在 [識別碼]  文字方塊中，使用下列模式來輸入 URL：`https://<tenantURL>.ngcxpress.com/`

    b. 在 [回覆 URL]  文字方塊中，使用下列模式來輸入 URL：`https://<tenantURL>.ngcxpress.com/SAMLConsumer.aspx`

5. 如果您想要以 **SP** 起始模式設定應用程式，請按一下 [設定其他 URL]，然後執行下列步驟：

    ![顯示您可以在其中輸入登入 URL 的設定額外 URL 螢幕擷取畫面。](common/metadata-upload-additional-signon.png)

    在 [登入 URL]  文字方塊中，以下列模式輸入 URL︰`https://<tenantURL>.ngcxpress.com/SAMLLogon.aspx`

    > [!NOTE]
    > 這些都不是真正的值。 您將會使用實際的「識別碼」、「回覆 URL」與「登入 URL」來更新值，稍後會在本教學課程中說明。

6. Andromeda 應用程式需要特定格式的 SAML 判斷提示。 設定此應用程式的下列宣告。 您可以在應用程式整合頁面的 [使用者屬性]  區段中，管理這些屬性的值。 在 [以 SAML 設定單一登入]  頁面上，按一下 [編輯]  按鈕以開啟 [使用者屬性]  對話方塊。

    ![顯示使用者屬性的螢幕擷取畫面，例如名字 user.givenname 和電子郵件地址 user.mail。](common/edit-attribute.png)

    > [!Important]
    > 在設定這些項目時清除 NameSpace 定義。

7. 在 [使用者屬性] 對話方塊的 [使用者宣告] 區段中，使用 [編輯] 圖示來編輯宣告或使用 [新增宣告] 來新增宣告，如上圖所示設定 SAML 權杖屬性，然後執行下列步驟： 

    | 名稱 | 來源屬性|
    | ------ | -----------|
    | 角色 (role) | 應用程式專屬角色 |
    | type | 應用程式類型 |
    | company | CompanyName |

    > [!NOTE]
    > 這些不是真正的值。 這些值僅供示範之用，請使用您的組織角色。

    1. 按一下 [新增宣告]  以開啟 [管理使用者宣告]  對話方塊。

        ![顯示使用者宣告選項以新增新宣告並且儲存的螢幕擷取畫面。](common/new-save-attribute.png)

        ![顯示管理使用者宣告的螢幕擷取畫面，您可以在其中輸入此步驟中所述的值。](common/new-attribute-details.png)

    1. 在 [名稱]  文字方塊中，輸入該資料列所顯示的屬性名稱。

    1. 讓 [命名空間]  保持空白。

    1. 選取 [來源] 作為 [屬性]  。

    1. 在 [來源屬性]  清單中，輸入該資料列所顯示的屬性值。

    1. 按一下 [確定]  。

    1. 按一下 [檔案]  。

8. 在 [以 SAML 設定單一登入]  頁面的 [SAML 簽署憑證]  區段中，按一下 [下載]  ，以依據您的需求從指定選項下載 [憑證 (Base64)]  ，並儲存在您的電腦上。

    ![憑證下載連結](common/certificatebase64.png)

9. 在 [設定 Andromeda]  區段上，依據您的需求複製適當的 URL。

    ![複製組態 URL](common/copy-configuration-urls.png)

    1. 登入 URL

    1. Azure AD 識別碼

    1. 登出 URL

### <a name="configure-andromeda-single-sign-on"></a>設定 Andromeda 單一登入

1. 以系統管理員身分登入您的 Andromeda 公司網站。

2. 在功能表列頂端按一下 [系統管理員]  ，並且瀏覽至 [系統管理]  。

    ![Andromeda 系統管理](./media/andromedascm-tutorial/tutorial_andromedascm_admin.png)

3. 在 [介面]  區段底下的工具列左側，按一下 [SAML 設定]  。

    ![Andromeda SAML](./media/andromedascm-tutorial/tutorial_andromedascm_saml.png)

4. 在 [SAML 設定]  區段頁面上，執行下列步驟：

    ![Andromeda 設定](./media/andromedascm-tutorial/tutorial_andromedascm_config.png)

    1. 勾選 [以 SAML 進行 SSO]  。

    1. 在 [Andromeda 資訊] 區段底下，複製 [SP 身分識別] 值並將其貼至 [基本 SAML 設定] 區段的 [識別碼] 文字方塊。

    1. 複製 [取用者 URL] 值並將其貼至 [基本 SAML 設定] 區段的 [回覆 URL] 文字方塊。

    1. 複製 [登入 URL] 值並將其貼至 [基本 SAML 設定] 區段的 [登入 URL] 文字方塊。

    1. 在 [SAML 識別提供者]  區段底下，輸入您的 IDP 名稱。

    1. 在 [單一登入端點]  文字方塊中，貼上您從 Azure 入口網站複製的 [登入 URL]  值。

    1. 從記事本中的 Azure 入口網站開啟已下載的 [Base64 編碼憑證]  ，將它貼至 [X 509 憑證]  文字方塊。

    1. 以個別值對應下列屬性，以便加速從 Azure AD 的 SSO 登入。 **User ID** 屬性是登入的必要項目。 佈建需要 **電子郵件**、**公司**、**使用者類型** 和 **角色**。 我們會在本節中定義屬性對應 (名稱和值)，此對應會與在 Azure 入口網站中定義的項目相互關聯。

        ![Andromeda 屬性對應](./media/andromedascm-tutorial/tutorial_andromedascm_attbmap.png)

    1. 按一下 [檔案]  。

### <a name="create-an-azure-ad-test-user"></a>建立 Azure AD 測試使用者

本節的目標是要在 Azure 入口網站中建立一個名為 Britta Simon 的測試使用者。

1. 在 Azure 入口網站的左窗格中，依序選取 [Azure Active Directory]  、[使用者]  和 [所有使用者]  。

    ![[使用者和群組] 與 [所有使用者] 連結](common/users.png)

2. 在畫面頂端選取 [新增使用者]  。

    ![[新增使用者] 按鈕](common/new-user.png)

3. 在 [使用者] 屬性中，執行下列步驟。

    ![[使用者] 對話方塊](common/user-properties.png)

    a. 在 [名稱]  欄位中，輸入 **BrittaSimon**。

    b. 在 [使用者名稱]  欄位中，輸入 `brittasimon@yourcompanydomain.extension`。 例如， BrittaSimon@contoso.com

    c. 選取 [顯示密碼]  核取方塊，然後記下 [密碼] 方塊中顯示的值。

    d. 按一下頁面底部的 [新增]  。

### <a name="assign-the-azure-ad-test-user"></a>指派 Azure AD 測試使用者

在本節中，您會將 Andromeda 的存取權授與 Britta Simon，讓她能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，依序選取 [企業應用程式]  、[所有應用程式]  及 [Andromeda]  。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

2. 在應用程式清單中，選取 [Andromeda]  。

    ![應用程式清單中的 Andromeda 連結](common/all-applications.png)

3. 在左側功能表中，選取 [使用者和群組]  。

    ![[使用者和群組] 連結](common/users-groups-blade.png)

4. 按一下 [新增使用者] 按鈕，然後在 [新增指派] 對話方塊中，選取 [使用者和群組]。

    ![[新增指派] 窗格](common/add-assign-user.png)

5. 在 [使用者和群組]  對話方塊的 [使用者] 清單中，選取 [Britta Simon]  ，然後按一下畫面底部的 [選取]  按鈕。

6. 如果您預期使用 SAML 判斷提示中的任何角色值，請在 [選取角色]  對話方塊的清單中選取適當使用者角色，然後按一下畫面底部的 [選取]  按鈕。

7. 在 [新增指派]  對話方塊中，按一下 [指派]  按鈕。

### <a name="create-andromeda-test-user"></a>建立 Andromeda 測試使用者

本節會在 Andromeda 中建立名為 Britta Simon 的使用者。 Andromeda 支援預設啟用的 Just-In-Time 使用者佈建。 在這一節沒有您需要進行的動作項目。 如果 Andromeda 中還沒有任何使用者存在，在驗證之後就會建立新的使用者。 如果您需要手動建立使用者，請連絡 [Andromeda 用戶端支援小組](https://www.ngcsoftware.com/support/)。

### <a name="test-single-sign-on"></a>測試單一登入 

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您在存取面板中按一下 [Andromeda] 圖格時，應該會自動登入您設定 SSO 的 Andromeda。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](../user-help/my-apps-portal-end-user-access.md)。

## <a name="additional-resources"></a>其他資源

- [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](./tutorial-list.md)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](../manage-apps/what-is-single-sign-on.md)

- [什麼是 Azure Active Directory 中的條件式存取？](../conditional-access/overview.md)