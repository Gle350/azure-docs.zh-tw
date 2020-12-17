---
title: 教學課程：Azure Active Directory 單一登入 (SSO) 與 TimeOffManager 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 TimeOffManager 之間的單一登入。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 12/10/2019
ms.author: jeedes
ms.openlocfilehash: 849236b9ac33cec92cc145bb32b4271b73476057
ms.sourcegitcommit: e15c0bc8c63ab3b696e9e32999ef0abc694c7c41
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/16/2020
ms.locfileid: "97608811"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-timeoffmanager"></a>教學課程：Azure Active Directory 單一登入 (SSO) 與 TimeOffManager 整合

在本教學課程中，您將了解如何整合 TimeOffManager 與 Azure Active Directory (Azure AD)。 在整合 TimeOffManager 與 Azure AD 時，您可以︰

* 在 Azure AD 中控制可存取 TimeOffManager 的人員。
* 讓使用者使用其 Azure AD 帳戶自動登入 TimeOffManager。
* 在 Azure 入口網站集中管理您的帳戶。

若要深入了解 SaaS 應用程式與 Azure AD 整合，請參閱[什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>Prerequisites

若要開始，您需要下列項目：

* Azure AD 訂用帳戶。 如果沒有訂用帳戶，您可以取得[免費帳戶](https://azure.microsoft.com/free/)。
* 已啟用 TimeOffManager 單一登入 (SSO) 的訂用帳戶。

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD SSO。


* TimeOffManager 支援由 **IDP** 起始的 SSO

* TimeOffManager 支援 **Just In Time** 使用者佈建

> [!NOTE]
> 此應用程式的識別碼是固定的字串值，因此一個租用戶中只能設定一個執行個體。


## <a name="adding-timeoffmanager-from-the-gallery"></a>從資源庫新增 TimeOffManager

若要設定將 TimeOffManager 整合到 Azure AD 中，您需要從資源庫將 TimeOffManager 新增到受控 SaaS 應用程式清單中。

1. 使用公司或學校帳戶或個人的 Microsoft 帳戶登入 [Azure 入口網站](https://portal.azure.com)。
1. 在左方瀏覽窗格上，選取 [Azure Active Directory]  服務。
1. 巡覽至 [企業應用程式]  ，然後選取 [所有應用程式]  。
1. 若要新增應用程式，請選取 [新增應用程式]  。
1. 在 [從資源庫新增]  區段的搜尋方塊中，輸入 **TimeOffManager**。
1. 從結果面板中選取 [TimeOffManager]  ，然後新增應用程式。 當應用程式新增至您的租用戶時，請等候幾秒鐘。


## <a name="configure-and-test-azure-ad-single-sign-on-for-timeoffmanager"></a>設定及測試 TimeOffManager 的 Azure AD 單一登入

以名為 **B.Simon** 的測試使用者，設定及測試與 TimeOffManager 搭配運作的 Azure AD SSO。 若要讓 SSO 能夠運作，您必須建立 Azure AD 使用者與 TimeOffManager 中相關使用者之間的連結關聯性。

若要設定及測試與 TimeOffManager 搭配運作的 Azure AD SSO，請完成下列建置組塊：

1. **[設定 Azure AD SSO](#configure-azure-ad-sso)** - 讓您的使用者能夠使用此功能。
    1. **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 B.Simon 測試 Azure AD 單一登入。
    1. **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 B.Simon 能夠使用 Azure AD 單一登入。
1. **[設定 TimeOffManager SSO](#configure-timeoffmanager-sso)** - 在應用程式端設定單一登入設定。
    1. **[建立 TimeOffManager 測試使用者](#create-timeoffmanager-test-user)** - 使 TimeOffManager 中對應的 B.Simon 連結到該使用者在 Azure AD 中的代表項目。
1. **[測試 SSO](#test-sso)** - 驗證組態是否能運作。

## <a name="configure-azure-ad-sso"></a>設定 Azure AD SSO

依照下列步驟在 Azure 入口網站中啟用 Azure AD SSO。

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [TimeOffManager]  應用程式整合頁面上，尋找 [管理]  區段並選取 [單一登入]  。
1. 在 [**選取單一登入方法**] 頁面上，選取 [**SAML**]。
1. 在 [以 SAML 設定單一登入]  頁面上，按一下 [基本 SAML 設定]  的編輯/畫筆圖示，以編輯設定。

   ![編輯基本 SAML 組態](common/edit-urls.png)

1. 在 [基本 SAML 組態]  區段上，輸入下列欄位的值：

    在 [回覆 URL]  文字方塊中，使用下列模式來輸入 URL：`https://www.timeoffmanager.com/cpanel/sso/consume.aspx?company_id=<companyid>`

    > [!NOTE]
    > 這不是真實的值。 請使用實際的「回覆 URL」來更新此值。 您可以從 [單一登入設定]  頁面取得此值，稍後會在本教學課程中加以說明，或連絡 [TimeOffManager 支援小組](https://www.purelyhr.com/contact-us)。 您也可以參考 Azure 入口網站中 **基本 SAML 組態** 區段所示的模式。

1. TimeOffManager 應用程式需要特定格式的 SAML 判斷提示，因此您必須將自訂屬性對應新增到 SAML 權杖屬性組態。 以下螢幕擷取畫面顯示預設屬性清單。

    ![image](common/edit-attribute.png)

1. 除了上述屬性外，TimeOffManager 應用程式還需要在 SAML 回應中多傳回幾個屬性，如下所示。 這些屬性也會預先填入，但您可以根據您的需求來檢閱這些屬性。

    | 名稱 | 來源屬性|
    | --- | --- |
    | Firstname |User.givenname |
    | lastname |User.surname |
    | 電子郵件 |User.mail |

1. 在 [以 SAML 設定單一登入]  頁面的 [SAML 簽署憑證]  區段中，尋找 [憑證 (Base64)]  並選取 [下載]  ，以下載憑證並將其儲存在電腦上。

    ![憑證下載連結](common/certificatebase64.png)

1. 在 [設定 TimeOffManager]  區段上，根據您的需求複製適當的 URL。

    ![複製組態 URL](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>建立 Azure AD 測試使用者

在本節中，您將在 Azure 入口網站中建立名為 B.Simon 的測試使用者。

1. 在 Azure 入口網站的左窗格中，依序選取 [Azure Active Directory]  、[使用者]  和 [所有使用者]  。
1. 在畫面頂端選取 [新增使用者]  。
1. 在 [使用者]  屬性中，執行下列步驟：
   1. 在 [名稱]  欄位中，輸入 `B.Simon`。  
   1. 在 [使用者名稱]  欄位中，輸入 username@companydomain.extension。 例如： `B.Simon@contoso.com` 。
   1. 選取 [顯示密碼]  核取方塊，然後記下 [密碼]  方塊中顯示的值。
   1. 按一下頁面底部的 [新增]  。

### <a name="assign-the-azure-ad-test-user"></a>指派 Azure AD 測試使用者

在本節中，您會將 TimeOffManager 的存取權授與 B.Simon，讓其能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，選取 [企業應用程式]  ，然後選取 [所有應用程式]  。
1. 在應用程式清單中，選取 [TimeOffManager]  。
1. 在應用程式的概觀頁面中尋找 [管理]  區段，然後選取 [使用者和群組]  。

   ![[使用者和群組] 連結](common/users-groups-blade.png)

1. 選取 [新增使用者]  ，然後在 [新增指派]  對話方塊中選取 [使用者和群組]  。

    ![[新增使用者] 連結](common/add-assign-user.png)

1. 在 [使用者和群組]  對話方塊的 [使用者] 清單中選取 [B.Simon]  ，然後按一下畫面底部的 [選取]  按鈕。
1. 如果您在 SAML 判斷提示中需要任何角色值，請在 [選取角色]  對話方塊的清單中為使用者選取適當的角色，然後按一下畫面底部的 [選取]  按鈕。
1. 在 [新增指派]  對話方塊中，按一下 [指派]  按鈕。

## <a name="configure-timeoffmanager-sso"></a>設定 TimeOffManager SSO

1. 在不同的網頁瀏覽器視窗中，以系統管理員身分登入您的 TimeOffManager 公司網站。

2. 移至 [帳戶] \> [帳戶選項] \> [單一登入設定]  。
   
    ![螢幕擷取畫面：顯示已從 [帳戶選項] 選取 [單一登入設定]。](./media/timeoffmanager-tutorial/ic795917.png "單一登入設定")

3. 在 [單一登入設定]  區段中，執行下列步驟：
   
    ![此螢幕擷取畫面顯示 [單一登入設定] 區段，您可以在其中輸入所述的值。](./media/timeoffmanager-tutorial/ic795918.png "單一登入設定")
   
    a. 在記事本中開啟您的 base-64 編碼的憑證，將它的內容複製到您的剪貼簿，然後將整個憑證貼到 [X.509 憑證]  文字方塊中。
   
    b. 在 [IdP 簽發者]  文字方塊中，貼上您從 Azure 入口網站複製的 [Azure AD 識別碼]  值。
   
    c. 在 [IdP 端點 URL]  文字方塊中，貼上您從 Azure 入口網站複製的 [登入 URL]  值。
   
    d. 在 [強制 SAML]  選取 [否]  。
   
    e. 在 [自動建立使用者]  選取 [是]  。
   
    f. 在 [登出 URL]  文字方塊中，貼上您從 Azure 入口網站複製的 [登出 URL]  值。
   
    g. 按一下 [儲存變更]  。

4. 在 [單一登入設定]  頁面中，複製 [判斷提示取用者服務 URL]  的值，並將其貼在 Azure 入口網站中 [基本 SAML 組態]  區段下的 [回覆 URL]  文字方塊中。 

    ![螢幕擷取畫面：顯示 [判斷提示取用者服務 URL 連結]。](./media/timeoffmanager-tutorial/ic795915.png "單一登入設定")

### <a name="create-timeoffmanager-test-user"></a>建立 TimeOffManager 測試使用者

本節會在 TimeOffManager 中建立名為 Britta Simon 的使用者。 TimeOffManager 支援依預設啟用的 Just-In-Time 使用者佈建。 在這一節沒有您需要進行的動作項目。 如果 TimeOffManager 中還沒有任何使用者存在，在驗證之後就會建立新的使用者。

>[!NOTE]
>您可以使用任何其他的 TimeOffManager 使用者帳戶建立工具或 TimeOffManager 提供的 API 來佈建 Azure AD 使用者帳戶。

## <a name="test-sso"></a>測試 SSO 

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您在存取面板中按一下 TimeOffManager 圖格時，應該會自動登入您設定 SSO 的 TimeOffManager。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](../user-help/my-apps-portal-end-user-access.md)。

## <a name="additional-resources"></a>其他資源

- [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](./tutorial-list.md)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](../manage-apps/what-is-single-sign-on.md)

- [什麼是 Azure Active Directory 中的條件式存取？](../conditional-access/overview.md)

- [嘗試搭配 Azure AD 使用 TimeOffManager](https://aad.portal.azure.com/)