---
title: 教學課程：Azure Active Directory 單一登入 (SSO) 與 TextMagic 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 TextMagic 之間的單一登入。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 10/17/2019
ms.author: jeedes
ms.openlocfilehash: f1e6cd222c9ee8f40f81d4db3750956e8e698e3e
ms.sourcegitcommit: e15c0bc8c63ab3b696e9e32999ef0abc694c7c41
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/16/2020
ms.locfileid: "97607664"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-textmagic"></a>教學課程：Azure Active Directory 單一登入 (SSO) 與 TextMagic 整合

在本教學課程中，您將了解如何整合 TextMagic 與 Azure Active Directory (Azure AD)。 在整合 TextMagic 與 Azure AD 時，您可以︰

* 在 Azure AD 中管控可存取 TextMagic 的人員。
* 讓使用者使用其 Azure AD 帳戶自動登入 TextMagic。
* 在 Azure 入口網站集中管理您的帳戶。

若要深入了解 SaaS 應用程式與 Azure AD 整合，請參閱[什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>Prerequisites

若要開始，您需要下列項目：

* Azure AD 訂用帳戶。 如果沒有訂用帳戶，您可以取得[免費帳戶](https://azure.microsoft.com/free/)。
* 已啟用 TextMagic 單一登入 (SSO) 的訂用帳戶。

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD SSO。

* TextMagic 支援由 **IDP** 起始的 SSO

* TextMagic 支援 **Just In Time** 使用者佈建

> [!NOTE]
> 此應用程式的識別碼是固定的字串值，因此一個租用戶中只能設定一個執行個體。

## <a name="adding-textmagic-from-the-gallery"></a>從資源庫新增 TextMagic

若要設定將 TextMagic 整合到 Azure AD 中，您必須從資源庫將 TextMagic 新增到受控 SaaS 應用程式清單。

1. 使用公司或學校帳戶或個人的 Microsoft 帳戶登入 [Azure 入口網站](https://portal.azure.com)。
1. 在左方瀏覽窗格上，選取 [Azure Active Directory]  服務。
1. 巡覽至 [企業應用程式]  ，然後選取 [所有應用程式]  。
1. 若要新增應用程式，請選取 [新增應用程式]  。
1. 在 [從資源庫新增]  區段的搜尋方塊中輸入 **TextMagic**。
1. 從結果面板選取 [TextMagic]  ，然後新增應用程式。 當應用程式新增至您的租用戶時，請等候幾秒鐘。

## <a name="configure-and-test-azure-ad-single-sign-on-for-textmagic"></a>設定及測試 TextMagic 的 Azure AD 單一登入

以名為 **B.Simon** 的測試使用者，設定及測試與 TextMagic 搭配運作的 Azure AD SSO。 若要讓 SSO 能夠運作，您必須建立 Azure AD 使用者與 TextMagic 中相關使用者之間的連結關聯性。

若要設定及測試與 TextMagic 搭配運作的 Azure AD SSO，請完成下列構成要素：

1. **[設定 Azure AD SSO](#configure-azure-ad-sso)** - 讓您的使用者能夠使用此功能。
    1. **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 B.Simon 測試 Azure AD 單一登入。
    1. **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 B.Simon 能夠使用 Azure AD 單一登入。
1. **[設定 TextMagic SSO](#configure-textmagic-sso)** - 在應用程式端設定單一登入設定。
    1. **[建立 TextMagic 測試使用者](#create-textmagic-test-user)** - 使 TextMagic 中對應的 B.Simon 連結到該使用者在 Azure AD 中的代表項目。
1. **[測試 SSO](#test-sso)** - 驗證組態是否能運作。

## <a name="configure-azure-ad-sso"></a>設定 Azure AD SSO

依照下列步驟在 Azure 入口網站中啟用 Azure AD SSO。

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [TextMagic]  應用程式整合頁面上，尋找 [管理]  區段並選取 [單一登入]  。
1. 在 [**選取單一登入方法**] 頁面上，選取 [**SAML**]。
1. 在 [以 SAML 設定單一登入]  頁面上，按一下 [基本 SAML 設定]  的編輯/畫筆圖示，以編輯設定。

   ![編輯基本 SAML 組態](common/edit-urls.png)

1. 在 [基本 SAML 組態]  區段上，輸入下列欄位的值：

    在 [識別碼]  文字方塊中，鍵入 URL：`https://my.textmagic.com/saml/metadata`

5. TextMagic 應用程式需要特定格式的 SAML 判斷提示，而您需要將自訂屬性對應新增到 SAML 權杖屬性組態。 下列螢幕擷取畫面顯示預設屬性清單，其中的 **nameidentifier** 與 **user.userprincipalname** 相對應。 TextMagic 應用程式要求 **nameidentifier** 需與 **user.mail** 相對應，因此您必須按一下 [編輯]  圖示以編輯屬性對應，並變更屬性對應。

    ![image](common/edit-attribute.png)

1. 除了上述屬性外，TextMagic 應用程式還需要在 SAML 回應中多傳回幾個屬性，如下所示。 這些屬性也會預先填入，但您可以根據您的需求來檢閱這些屬性。

    | 名稱 |   來源屬性| 命名空間  |
    | --------------- | --------------- | --------------- |
    | company | user.companyname | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | firstName | user.givenname |  http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | lastName | user.surname |  http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | 電話 | user.telephonenumber |  http://schemas.xmlsoap.org/ws/2005/05/identity/claims |

1. 在 [以 SAML 設定單一登入]  頁面的 [SAML 簽署憑證]  區段中，尋找 [憑證 (Base64)]  並選取 [下載]  ，以下載憑證並將其儲存在電腦上。

    ![憑證下載連結](common/certificatebase64.png)

1. 在 [設定 TextMagic]  區段上，依據您的需求複製適當的 URL。

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

在本節中，您會將 TextMagic 的存取權授與 B.Simon，讓其能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，選取 [企業應用程式]  ，然後選取 [所有應用程式]  。
1. 在應用程式清單中，選取 [TextMagic]  。
1. 在應用程式的概觀頁面中尋找 [管理]  區段，然後選取 [使用者和群組]  。

   ![[使用者和群組] 連結](common/users-groups-blade.png)

1. 選取 [新增使用者]  ，然後在 [新增指派]  對話方塊中選取 [使用者和群組]  。

    ![[新增使用者] 連結](common/add-assign-user.png)

1. 在 [使用者和群組]  對話方塊的 [使用者] 清單中選取 [B.Simon]  ，然後按一下畫面底部的 [選取]  按鈕。
1. 如果您在 SAML 判斷提示中需要任何角色值，請在 [選取角色]  對話方塊的清單中為使用者選取適當的角色，然後按一下畫面底部的 [選取]  按鈕。
1. 在 [新增指派]  對話方塊中，按一下 [指派]  按鈕。

### <a name="configure-textmagic-sso"></a>設定 TextMagic SSO

1. 若要自動執行 TextMagic 內的設定，您必須按一下 [安裝擴充功能]  來安裝「我的應用程式安全登入瀏覽器擴充功能」  。

    ![我的應用程式擴充功能](common/install-myappssecure-extension.png)

2. 將延伸模組新增至瀏覽器之後，按一下 [安裝 TextMagic]  便會將您導向到 TextMagic 應用程式。 請從該處提供用以登入 TextMagic 的管理員認證。 瀏覽器擴充功能會自動為您設定應用程式，並自動執行步驟 3 到 5。

    ![設定組態](common/setup-sso.png)

3. 如果您想要手動設定 TextMagic，請開啟新的網頁瀏覽器視窗，並以系統管理員身分登入 TextMagic 公司網站，然後執行下列步驟：

4. 選取使用者名稱下的 [帳戶設定]  。

    ![螢幕擷取畫面：顯示已從使用者選取 [帳戶設定]。](./media/textmagic-tutorial/config1.png)

5. 按一下 [單一登入 (SSO)]  索引標籤並填寫下列欄位：  

    ![此螢幕擷取畫面顯示 [單一登入] 索引標籤，您可以在其中輸入所述的值。](./media/textmagic-tutorial/config2.png)

    a. 在 [識別提供者實體識別碼：]  文字方塊中，貼上您從 Azure 入口網站複製的 [Azure AD 識別碼]  值。

    b. 在 [識別提供者 SSO URL：]  文字方塊中，貼上您從 Azure 入口網站複製的 [登入 URL]  值。

    c. 在 [識別提供者 SLO URL：]  文字方塊中，貼上您從 Azure 入口網站複製的 [登入 URL]  值。

    d. 在從 Azure 入口網站下載的記事本檔案中開啟您的 **base-64** 編碼憑證，將憑證的內容複製到剪貼簿，再貼到 [公開 x509 憑證]  文字方塊。

    e. 按一下 [檔案]  。

### <a name="create-textmagic-test-user"></a>建立 TextMagic 測試使用者

應用程式支援 **Just in time 使用者佈建**，而在驗證之後，會在應用程式中自動建立使用者。 第一次登入以啟動子帳戶至系統時，您需要填入資訊一次。
在這一節沒有您需要進行的動作項目。

## <a name="test-sso"></a>測試 SSO 

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您在存取面板中按一下 TextMagic 圖格時，應該會自動登入您已設定 SSO 的 TextMagic。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](../user-help/my-apps-portal-end-user-access.md)。

## <a name="additional-resources"></a>其他資源

- [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](./tutorial-list.md)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](../manage-apps/what-is-single-sign-on.md)

- [什麼是 Azure Active Directory 中的條件式存取？](../conditional-access/overview.md)

- [嘗試搭配 Azure AD 使用 TextMagic](https://aad.portal.azure.com/)