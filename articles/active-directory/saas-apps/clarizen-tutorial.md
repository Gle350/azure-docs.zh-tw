---
title: 教學課程：Azure Active Directory 與 Clarizen 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 Clarizen 之間的單一登入。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/21/2019
ms.author: jeedes
ms.openlocfilehash: 38b2ff6909dae15ff0f836316d5d12140ecc331a
ms.sourcegitcommit: d79513b2589a62c52bddd9c7bd0b4d6498805dbe
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/18/2020
ms.locfileid: "97672925"
---
# <a name="tutorial-azure-active-directory-integration-with-clarizen"></a>教學課程：Azure Active Directory 與 Clarizen 整合

在本教學課程中，您會了解如何整合 Clarizen 與 Azure Active Directory (Azure AD)。
Clarizen 與 Azure AD 整合提供下列優點：

* 您可以在 Azure AD 中控制可存取 Clarizen 的人員。
* 您可以讓使用者使用其 Azure AD 帳戶自動登入 Clarizen (單一登入)。
* 您可以在 Azure 入口網站中集中管理您的帳戶。

若您想了解 SaaS app 與 Azure AD 整合的更多詳細資訊，請參閱 [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](../manage-apps/what-is-single-sign-on.md)。
如果您沒有 Azure 訂用帳戶，請在開始之前先[建立免費帳戶](https://azure.microsoft.com/free/)。

## <a name="prerequisites"></a>Prerequisites

若要設定與 Clarizen 的 Azure AD 整合，您需要下列項目：

* Azure AD 訂用帳戶。 如果您沒有 Azure AD 環境，您可以在[這裡](https://azure.microsoft.com/pricing/free-trial/)取得一個月的試用帳戶
* 已啟用 Clarizen 單一登入的訂用帳戶

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD 單一登入。

* Clarizen 支援由 **IDP** 起始的 SSO

## <a name="adding-clarizen-from-the-gallery"></a>從資源庫新增 Clarizen

若要設定 Clarizen 與 Azure AD 整合，您需要從資源庫將 Clarizen 加入受控 SaaS 應用程式清單中。

**若要從資源庫新增 Clarizen，請執行下列步驟：**

1. 在 **[Azure 入口網站](https://portal.azure.com)** 的左方瀏覽窗格中，按一下 [Azure Active Directory]  圖示。

    ![Azure Active Directory 按鈕](common/select-azuread.png)

2. 瀏覽至 [企業應用程式]  ，然後選取 [所有應用程式]  選項。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

3. 若要新增新的應用程式，請按一下對話方塊頂端的 [新增應用程式]  按鈕。

    ![新增應用程式按鈕](common/add-new-app.png)

4. 在搜尋方塊中，輸入 **Clarizen**，從結果面板中選取 [Clarizen]  ，然後按一下 [新增]  按鈕以新增應用程式。

    ![結果清單中的 Clarizen](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>設定和測試 Azure AD 單一登入

在本節中，您會以名為 **Britta Simon** 的測試使用者為基礎，設定及測試與 Clarizen 搭配運作的 Azure AD 單一登入。
若要讓單一登入能夠運作，必須建立 Azure AD 使用者與 Clarizen 中相關使用者之間的連結關聯性。

若要設定及測試與 Clarizen 搭配運作的 Azure AD 單一登入，您需要完成下列建構元素：

1. **[設定 Azure AD 單一登入](#configure-azure-ad-single-sign-on)** - 讓您的使用者能夠使用此功能。
2. **[設定 Clarizen 單一登入](#configure-clarizen-single-sign-on)** - 在應用程式端設定單一登入設定。
3. **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 Britta Simon 測試 Azure AD 單一登入。
4. **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 Britta Simon 能夠使用 Azure AD 單一登入。
5. **[建立 Clarizen 測試使用者](#create-clarizen-test-user)** ，在 Clarizen 中建立一個與 Azure AD 使用者代表 Britta Simon 連結的對應使用者。
6. **[測試單一登入](#test-single-sign-on)** ，驗證組態是否能運作。

### <a name="configure-azure-ad-single-sign-on"></a>設定 Azure AD 單一登入

在本節中，您會在 Azure 入口網站中啟用 Azure AD 單一登入。

若要設定與 Clarizen 搭配運作的 Azure AD 單一登入，請執行下列步驟：

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [Clarizen]  應用程式整合頁面上，選取 [單一登入]  。

    ![設定單一登入連結](common/select-sso.png)

2. 在 [選取單一登入方法]  對話方塊中，選取 [SAML/WS-Fed]  模式以啟用單一登入。

    ![單一登入選取模式](common/select-saml-option.png)

3. 在 [以 SAML 設定單一登入] 頁面上，按一下 [編輯] 圖示以開啟 [基本 SAML 設定] 對話方塊。   

    ![編輯基本 SAML 組態](common/edit-urls.png)

4. 在 [以 SAML 設定單一登入]  頁面上，執行下列步驟：

    ![Clarizen 網域與 URL 單一登入資訊](common/idp-intiated.png)

    a. 在 [識別碼]  文字方塊中，輸入下列值：`Clarizen`

    b. 在 [回覆 URL]  文字方塊中，使用下列模式來輸入 URL：`https://.clarizen.com/Clarizen/Pages/Integrations/SAML/SamlResponse.aspx`

    > [!NOTE]
    > 這些不是真正的值。 您必須使用實際的識別碼和回覆 URL。 在此建議您使用唯一的字串值做為識別碼。 若要取得實際的值，請連絡 [Clarizen 支援小組](https://success.clarizen.com/hc/en-us/requests/new)。

4. 在 [以 SAML 設定單一登入]  頁面的 [SAML 簽署憑證]  區段中，按一下 [下載]  ，以依據您的需求從指定選項下載 [憑證 (Base64)]  ，並儲存在您的電腦上。

    ![憑證下載連結](common/certificatebase64.png)

6. 在 [設定 Clarizen]  區段上，依據您的需求複製適當的 URL。

    ![複製組態 URL](common/copy-configuration-urls.png)

    a. 登入 URL

    b. Azure AD 識別碼

    c. 登出 URL

### <a name="configure-clarizen-single-sign-on"></a>設定 Clarizen 單一登入

1. 在不同的網頁瀏覽器視窗中，以系統管理員身分登入您的 Clarizen 公司網站。

1. 按一下您的使用者名稱，然後按一下 [設定]  。

    ![按一下您的使用者名稱底下的 [設定]](./media/clarizen-tutorial/tutorial_clarizen_001.png "設定")

1. 按一下 [全域設定]  索引標籤。然後，在 **[同盟驗證]** 旁邊，按一下 [編輯]  。

    ![全域設定](./media/clarizen-tutorial/tutorial_clarizen_002.png "全域設定")

1. 在 [同盟驗證]  對話方塊中，執行下列步驟：

    ![[同盟驗證] 對話方塊](./media/clarizen-tutorial/tutorial_clarizen_003.png "同盟驗證")

    a. 選取 [啟用同盟驗證]  。

    b. 按一下 [上傳]  來上傳您下載的憑證。

    c. 在 [登入 URL]  方塊中，輸入來自 Azure AD 應用程式組態視窗的 [登入 URL]  值。

    d. 在 [登出 URL]  方塊中，輸入來自 Azure AD 應用程式組態視窗的 [登出 URL]  值。

    e. 選取 [使用 POST]  。

    f. 按一下 [檔案]  。

### <a name="create-an-azure-ad-test-user"></a>建立 Azure AD 測試使用者 

本節的目標是要在 Azure 入口網站中建立一個名為 Britta Simon 的測試使用者。

1. 在 Azure 入口網站的左窗格中，依序選取 [Azure Active Directory]  、[使用者]  和 [所有使用者]  。

    ![[使用者和群組] 與 [所有使用者] 連結](common/users.png)

2. 在畫面頂端選取 [新增使用者]  。

    ![[新增使用者] 按鈕](common/new-user.png)

3. 在 [使用者] 屬性中，執行下列步驟。

    ![[使用者] 對話方塊](common/user-properties.png)

    a. 在 [名稱]  欄位中，輸入 **BrittaSimon**。

    b. 在 [使用者名稱]  欄位中，輸入 **brittasimon\@yourcompanydomain.extension**  
    例如， BrittaSimon@contoso.com

    c. 選取 [顯示密碼]  核取方塊，然後記下 [密碼] 方塊中顯示的值。

    d. 按一下頁面底部的 [新增]  。

### <a name="assign-the-azure-ad-test-user"></a>指派 Azure AD 測試使用者

在本節中，您會把 Clarizen 的存取權授與 Britta Simon，讓她能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，依序選取 [企業應用程式]  、[所有應用程式]  及 [Clarizen]  。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

2. 在應用程式清單中，選取 [Clarizen]  。

    ![應用程式清單中的 [Clarizen] 連結](common/all-applications.png)

3. 在左側功能表中，選取 [使用者和群組]  。

    ![[使用者和群組] 連結](common/users-groups-blade.png)

4. 按一下 [新增使用者]  按鈕，然後在 [新增指派]  對話方塊中，選取 [使用者和群組]  。

    ![[新增指派] 窗格](common/add-assign-user.png)

5. 在 [使用者和群組]  對話方塊的 [使用者] 清單中，選取 [Britta Simon]  ，然後按一下畫面底部的 [選取]  按鈕。

6. 如果您預期使用 SAML 判斷提示中的任何角色值，請在 [選取角色]  對話方塊的清單中選取適當使用者角色，然後按一下畫面底部的 [選取]  按鈕。

7. 在 [新增指派]  對話方塊中，按一下 [指派]  按鈕。

### <a name="create-clarizen-test-user"></a>建立 Clarizen 測試使用者

本節的目標是在 Clarizen 中建立名為 Britta Simon 的使用者。

**如果您需要手動建立使用者，請執行下列步驟：**

若要讓 Azure AD 使用者登入 Clarizen，您必須佈建使用者帳戶。 Clarizen 需以手動方式佈建。

1. 以系統管理員身分登入您的 Clarizen 公司網站。

2. 按一下 [人員]  。

    ![按一下 [人員]](./media/clarizen-tutorial/create_aaduser_001.png "人員")

3. 按一下 [邀請使用者]  。

    ![[邀請使用者] 按鈕](./media/clarizen-tutorial/create_aaduser_002.png "邀請使用者")

1. 在 [邀請人員]  對話方塊中，執行下列步驟：

    ![[邀請人員] 對話方塊](./media/clarizen-tutorial/create_aaduser_003.png "邀請人員")

    a. 在 [電子郵件]  方塊中，輸入 Britta Simon 帳戶的電子郵件地址。

    b. 按一下 [邀請]  。

    > [!NOTE]
    > Azure Active Directory 帳戶的持有者會收到一封電子郵件，並依照連結在啟用其帳戶前進行確認。


### <a name="test-single-sign-on"></a>測試單一登入 

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您在存取面板中按一下 Clarizen 圖格時，應該會自動登入您設定 SSO 的 Clarizen。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](../user-help/my-apps-portal-end-user-access.md)。

## <a name="additional-resources"></a>其他資源

- [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](./tutorial-list.md)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](../manage-apps/what-is-single-sign-on.md)

- [什麼是 Azure Active Directory 中的條件式存取？](../conditional-access/overview.md)