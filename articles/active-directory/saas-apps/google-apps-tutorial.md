---
title: 教學課程：Azure Active Directory 單一登入 (SSO) 與 Google Cloud (G Suite) Connector 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 Google Cloud (G Suite) Connector 之間的單一登入。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 05/06/2020
ms.author: jeedes
ms.openlocfilehash: 0dd66e246e5e172ad359f5e6e953b360e6e74ebd
ms.sourcegitcommit: ab829133ee7f024f9364cd731e9b14edbe96b496
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/28/2020
ms.locfileid: "97796970"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-google-cloud-g-suite-connector"></a>教學課程：Azure Active Directory 單一登入 (SSO) 與 Google Cloud (G Suite) Connector 整合

在本教學課程中，您將了解如何整合 Google Cloud (G Suite) Connector 與 Azure Active Directory (Azure AD)。 在整合 Google Cloud (G Suite) Connector 與 Azure AD 時，您可以︰

* 在 Azure AD 中控制可存取 Google Cloud (G Suite) Connector 的人員。
* 讓使用者使用其 Azure AD 帳戶自動登入 Google Cloud (G Suite) Connector。
* 在 Azure 入口網站集中管理您的帳戶。

若要深入了解 SaaS 應用程式與 Azure AD 整合，請參閱[什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>Prerequisites

若要開始，您需要下列項目：

- Azure AD 訂用帳戶。
- 已啟用 Google Cloud (G Suite) Connector 單一登入 (SSO) 的訂用帳戶。
- Google Apps 訂用帳戶或 Google Cloud Platform 訂用帳戶。

> [!NOTE]
> 若要測試本教學課程中的步驟，我們不建議使用生產環境。 本文件是使用新的使用者單一登入體驗所建立。 如果您仍然使用舊版，則安裝程式看起來會不同。 您可以在 G-Suite 應用程式的單一登入設定中啟用新的體驗。 移至 [Azure AD]、[企業應用程式]  ，選取 [Google Cloud (G Suite) Connector]  ，選取 [單一登入]  ，然後按一下 [試用新的體驗]  。

若要測試本教學課程中的步驟，您應該遵循這些建議：

- 除非必要，否則請勿使用生產環境。
- 如果沒有訂用帳戶，您可以取得[免費帳戶](https://azure.microsoft.com/free/)。

## <a name="frequently-asked-questions"></a>常見問題集

1. **問：這項整合支援 Google Cloud Platform 單一登入與 Azure AD 的整合嗎？**

    A：是。 Google Cloud Platform 和 Google Apps 共用相同的驗證平台。 因此，為了進行 GCP 整合，您需要設定 Google Apps 單一登入。

2. **問：Chromebook 及其他 Chrome 裝置是否與 Azure AD 單一登入相容？**
  
    A：是，使用者能夠使用其 Azure AD 認證來登入其 Chromebook 裝置。 若要了解為何使用者可能收到兩次提供認證的提示，請參閱此 [Google Cloud (G Suite) Connector 支援文章](https://support.google.com/chrome/a/answer/6060880)。

3. **問：如果我啟用單一登入，使用者是否將能夠使用其 Azure AD 認證來登入任何 Google 產品 (例如 Google Classroom、GMail、Google 雲端硬碟、YouTube 等)？**

    A：是，視您選擇為組織啟用或停用的 [Google Cloud (G Suite) Connector](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) 而定。

4. **問：我是否可以只為一部分 Google Cloud (G Suite) Connector 使用者啟用單一登入？**

    A：否，開啟單一登入會立即要求您的所有 Google Cloud (G Suite) Connector 使用者使用其 Azure AD 認證進行驗證。 由於 Google Cloud (G Suite) Connector 不支援使用多個身分識別提供者，因此您 Google Cloud (G Suite) Connector 環境的身分識別提供者可以是 Azure AD 或 Google 其中之一，但不可同時是兩者。

5. **問：如果使用者透過 Windows 登入，他們是否會自動向 Google Cloud (G Suite) Connector 進行驗證，而不會收到輸入密碼的提示？**

    A：有兩個選項可允許這樣的情況。 第一個是，使用者可以透過 [Azure Active Directory Join](../devices/overview.md)登入 Windows 10 裝置。 另一個是，使用者可以登入已加入某個內部部署 Active Directory 網域的 Windows 裝置，其中此內部部署 Active Directory 已透過 [Active Directory 同盟服務 (AD FS)](../hybrid/plan-connect-user-signin.md) 部署而能夠單一登入到 Azure AD。 這兩個選項都需要您執行下列教學課程的步驟，才能在 Azure AD 與 Google Cloud (G Suite) Connector 之間啟用單一登入。

6. **問：當我收到「無效的電子郵件」錯誤訊息時該怎麼辦？**

    A：此設定需要電子郵件屬性，使用者才能夠登入。 目前無法手動設定此屬性。

    系統針對任何具備有效 Exchange 授權的使用者，自動填入電子郵件屬性。 如果使用者並未啟用電子郵件功能，則會收到此錯誤，因為應用程式需要取得此屬性才能授與存取權。

    您可以使用系統管理員帳戶前往 portal.office.com，然後按一下系統管理中心、帳單、訂用帳戶，選取您的 Microsoft 365 訂用帳戶，然後按一下 [指派使用者]，選取您要檢查其訂用帳戶的使用者，然後在右窗格中按一下編輯授權。

    指派 Microsoft 365 授權後，可能需要幾分鐘的時間才會套用。 然後，系統會自動填入 user.mail 屬性，此問題應該就解決了。

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD SSO。

* Google Cloud (G Suite) Connector 支援由 **SP** 起始的 SSO

* Google Cloud (G Suite) Connector 支援[自動  使用者佈建](g-suite-provisioning-tutorial.md)
* 設定 Google Cloud (G Suite) Connector 後，您可以強制執行工作階段控制項，以即時防止組織的敏感資料遭到外洩和滲透。 工作階段控制項會從條件式存取延伸。 [了解如何使用 Microsoft Cloud App Security 來強制執行工作階段控制項](/cloud-app-security/proxy-deployment-aad)

## <a name="adding-google-cloud-g-suite-connector-from-the-gallery"></a>從資源庫新增 Google Cloud (G Suite) Connector

若要設定 Google Cloud (G Suite) Connector 與 Azure AD 整合，您需要從資源庫將 Google Cloud (G Suite) Connector 新增到受控 SaaS 應用程式清單。

1. 使用公司或學校帳戶或個人的 Microsoft 帳戶登入 [Azure 入口網站](https://portal.azure.com)。
1. 在左方瀏覽窗格上，選取 [Azure Active Directory]  服務。
1. 巡覽至 [企業應用程式]  ，然後選取 [所有應用程式]  。
1. 若要新增應用程式，請選取 [新增應用程式]  。
1. 在 [從資源庫新增]  區段的搜尋方塊中，輸入 **Google Cloud (G Suite) Connector**。
1. 從結果面板選取 [Google Cloud (G Suite) Connector]  ，然後新增應用程式。 當應用程式新增至您的租用戶時，請等候幾秒鐘。

## <a name="configure-and-test-azure-ad-single-sign-on-for-google-cloud-g-suite-connector"></a>設定及測試 Google Cloud (G Suite) Connector 的 Azure AD 單一登入

以名為 **B.Simon** 的測試使用者，設定及測試與 Google Cloud (G Suite) Connector 搭配運作的 Azure AD SSO。 若要讓 SSO 能夠運作，您必須建立 Azure AD 使用者與 Google Cloud (G Suite) Connector 中相關使用者之間的連結關聯性。

若要使用 Google Cloud (G Suite) Connector 來設定並測試 Azure AD SSO，您需要完成下列建構元素：

1. **[設定 Azure AD SSO](#configure-azure-ad-sso)** - 讓您的使用者能夠使用此功能。
    1. **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 B.Simon 測試 Azure AD 單一登入。
    1. **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 B.Simon 能夠使用 Azure AD 單一登入。
1. **[設定 Google Cloud (G Suite) Connector SSO](#configure-google-cloud-g-suite-connector-sso)** - 在應用程式端設定單一登入設定。
    1. **[建立 Google Cloud (G Suite) Connector 測試使用者](#create-google-cloud-g-suite-connector-test-user)** - 使 Google Cloud (G Suite) Connector 中對應的 B.Simon 連結到該使用者在 Azure AD 中的代表項目。
1. **[測試 SSO](#test-sso)** - 驗證組態是否能運作。

## <a name="configure-azure-ad-sso"></a>設定 Azure AD SSO

依照下列步驟在 Azure 入口網站中啟用 Azure AD SSO。

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [Google Cloud (G Suite) Connector]  應用程式整合頁面上，尋找 [管理]  區段並選取 [單一登入]  。
1. 在 [**選取單一登入方法**] 頁面上，選取 [**SAML**]。
1. 在 [以 SAML 設定單一登入]  頁面上，按一下 [基本 SAML 設定]  的編輯/畫筆圖示，以編輯設定。

   ![編輯基本 SAML 組態](common/edit-urls.png)

1. 在 [基本 SAML 設定]  區段上，如果要設定 **Gmail**，執行下列步驟：

    a. 在 [登入 URL]  文字方塊中，使用下列模式輸入 URL︰ `https://www.google.com/a/<yourdomain.com>/ServiceLogin?continue=https://mail.google.com`

    b. 在 [識別碼]  文字方塊中，使用下列模式輸入 URL：

    ```http
    google.com/a/<yourdomain.com>
    google.com
    https://google.com
    https://google.com/a/<yourdomain.com>
    ```

    c. 在 [回覆 URL]  文字方塊中，以下列模式輸入 URL： 

    ```http
    https://www.google.com
    https://www.google.com/a/<yourdomain.com>
    ```

1. 在 [基本 SAML 設定]  區段上，如果要設定 **Google Cloud Platform**，執行下列步驟：

    a. 在 [登入 URL]  文字方塊中，使用下列模式輸入 URL︰ `https://www.google.com/a/<yourdomain.com>/ServiceLogin?continue=https://console.cloud.google.com`

    b. 在 [識別碼]  文字方塊中，使用下列模式輸入 URL：
    
    ```http
    google.com/a/<yourdomain.com>
    google.com
    https://google.com
    https://google.com/a/<yourdomain.com>
    ```
    
    c. 在 [回覆 URL]  文字方塊中，以下列模式輸入 URL： 
    
    ```http
    https://www.google.com
    https://www.google.com/a/<yourdomain.com>
    ```

    > [!NOTE]
    > 這些都不是真正的值。 使用實際的「登入 URL」及「識別碼」來更新這些值。 Google Cloud (G Suite) Connector 不會在單一登入設定上提供實體識別碼/識別碼值，因此，當您取消勾選 [網域特定簽發者]  選項時，識別碼值將會是 `google.com`。 如果您勾選 [網域特定簽發者]  選項，其將是 `google.com/a/<yourdomainname.com>`。 若要勾選/取消勾選 [網域特定簽發者]  選項，您必須移至 [設定 Google Cloud (G Suite) Connector SSO]  區段，此教學課程稍後將會說明此區段。 如需詳細資訊，請連絡 [Google Cloud (G Suite) Connector 用戶端支援小組](https://www.google.com/contact/)。

1. Google Cloud (G Suite) Connector 應用程式需要特定格式的 SAML 判斷提示，因此您必須將自訂屬性對應新增至您的 SAML 權杖屬性組態。 以下螢幕擷取畫面顯示上述的範例。 [唯一的使用者識別碼]  的預設值是 **user.userprincipalname**，但是 Google Cloud (G Suite) Connector 會預期這是與使用者電子郵件地址對應的值。 對此您可以使用清單中的 **user.mail** 屬性，或者根據組織組態使用適當的屬性值。

    ![image](common/default-attributes.png)


1. 在 [以 SAML 設定單一登入]  頁面的 [SAML 簽署憑證]  區段中，尋找 [憑證 (Base64)]  並選取 [下載]  ，以下載憑證並將其儲存在電腦上。

    ![憑證下載連結](common/certificatebase64.png)

1. 在 [設定 Google Cloud (G Suite) Connector]  區段上，依據您的需求複製適當的 URL。

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

在本節中，您會將 Google Cloud (G Suite) Connector 的存取權授與 B.Simon，讓她能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，選取 [企業應用程式]  ，然後選取 [所有應用程式]  。
1. 在應用程式清單中，選取 [Google Cloud (G Suite) Connector]  。
1. 在應用程式的概觀頁面中尋找 [管理]  區段，然後選取 [使用者和群組]  。

   ![[使用者和群組] 連結](common/users-groups-blade.png)

1. 選取 [新增使用者]  ，然後在 [新增指派]  對話方塊中選取 [使用者和群組]  。

    ![[新增使用者] 連結](common/add-assign-user.png)

1. 在 [使用者和群組]  對話方塊的 [使用者] 清單中選取 [B.Simon]  ，然後按一下畫面底部的 [選取]  按鈕。
1. 如果您在 SAML 判斷提示中需要任何角色值，請在 [選取角色]  對話方塊的清單中為使用者選取適當的角色，然後按一下畫面底部的 [選取]  按鈕。
1. 在 [新增指派]  對話方塊中，按一下 [指派]  按鈕。

## <a name="configure-google-cloud-g-suite-connector-sso"></a>設定 Google Cloud (G Suite) Connector SSO

1. 在瀏覽器中開啟新索引標籤，然後使用系統管理員帳戶登入 [Google Cloud (G Suite) Connector 管理控制台](https://admin.google.com/)。

2. 按一下 **[安全性]** 。 如果您沒有看到連結，它可能隱藏在畫面底部的 [其他控制項]  功能表之下。

    ![按一下 [安全性]。][10]

3. 在 [安全性]  頁面上，按一下 [設定單一登入 (SSO)]  。

    ![按一下 [SSO]。][11]

4. 執行下列組態變更：

    ![設定 SSO][12]

    a. 選取 [使用第三方識別提供者來設定 SSO]  。

    b. 在 Google Cloud (G Suite) Connector 的 [登入頁面 URL]  欄位中，貼上您從 Azure 入口網站複製的 [登入 URL]  值。

    c. 在 Google Cloud (G Suite) Connector 的 [登出頁面 URL] 欄位中，貼上您從 Azure 入口網站複製的 [登入 URL] 值。

    > [!NOTE]
    > Google 雲端 (G Suite) 是以 SAML 登出通訊協定為基礎。 因此，在 **登出頁面 URL** 欄位中，我們必須使用 SAML 登出 URL，也就是登入 URL 作為相同的值。

    d. 在 Google Cloud (G Suite) Connector 中，對於 [驗證憑證]  ，上傳您已從 Azure 入口網站下載的憑證。   

    e. 根據 Azure AD 中上述 [基本 SAML 設定]  區段中提及的每個注意事項，勾選/取消勾選 [使用網域特定簽發者]  選項。

    f. 在 Google Cloud (G Suite) Connector 的 [變更密碼 URL] 欄位中，貼上您從 Azure 入口網站複製的 [變更密碼 URL] 值。

    g. 按一下 [檔案] 。

### <a name="create-google-cloud-g-suite-connector-test-user"></a>建立 Google Cloud (G Suite) Connector 測試使用者

本節的目標是要[在 Google Cloud (G Suite) Connector 中建立名為 B.Simon 的使用者](https://support.google.com/a/answer/33310?hl=en)。 在 Google Cloud (G Suite) Connector 中手動建立使用者之後，使用者現在就可以使用其 Microsoft 365 登入認證進行登入。

Google Cloud (G Suite) Connector 也支援自動使用者佈建。 若要設定自動使用者佈建，您必須先[設定 Google Cloud (G Suite) Connector 來自動佈建使用者](g-suite-provisioning-tutorial.md)。

> [!NOTE]
> 在測試單一登入之前，如果尚未在 Azure AD 中開啟佈建，請確定您的使用者已經存在於 Google Cloud (G Suite) Connector 中。

> [!NOTE]
> 如果您需要手動建立使用者，請連絡 [Google 支援小組](https://www.google.com/contact/)。

## <a name="test-sso"></a>測試 SSO 

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您在存取面板中按一下 [Google Cloud (G Suite) Connector] 圖格時，應該會自動登入您已設定 SSO 的 Google Cloud (G Suite) Connector。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](../user-help/my-apps-portal-end-user-access.md)。

## <a name="additional-resources"></a>其他資源

- [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](./tutorial-list.md)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](../manage-apps/what-is-single-sign-on.md)。

- [什麼是 Azure Active Directory 中的條件式存取？](../conditional-access/overview.md)

- [設定使用者佈建](g-suite-provisioning-tutorial.md)

- [嘗試搭配 Azure AD 使用 Google Cloud (G Suite) Connector](https://aad.portal.azure.com/)

- [什麼是 Microsoft Cloud App Security 中的工作階段控制項？](/cloud-app-security/proxy-intro-aad)

- [如何使用進階可見性和控制項保護 Google Cloud (G Suite) Connector](/cloud-app-security/protect-gsuite)

<!--Image references-->

[10]: ./media/google-apps-tutorial/gapps-security.png
[11]: ./media/google-apps-tutorial/security-gapps.png
[12]: ./media/google-apps-tutorial/gapps-sso-config.png