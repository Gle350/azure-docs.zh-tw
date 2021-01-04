---
title: 教學課程：Azure Active Directory 與 Brightidea 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 Brightidea 之間的單一登入。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/23/2019
ms.author: jeedes
ms.openlocfilehash: 9967f349011b52a2218681956885c33456ba1d46
ms.sourcegitcommit: d79513b2589a62c52bddd9c7bd0b4d6498805dbe
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/18/2020
ms.locfileid: "97672754"
---
# <a name="tutorial-azure-active-directory-integration-with-brightidea"></a>教學課程：Azure Active Directory 與 Brightidea 整合

在本教學課程中，您會了解如何將 Brightidea 與 Azure Active Directory (Azure AD) 整合。
將 Brightidea 與 Azure AD 整合可提供下列優點：

* 您可以在 Azure AD 中控制可存取 Brightidea 的人員。
* 您可以讓使用者使用其 Azure AD 帳戶自動登入 Brightidea (單一登入)。
* 您可以在 Azure 入口網站中集中管理您的帳戶。

若您想了解 SaaS app 與 Azure AD 整合的更多詳細資訊，請參閱 [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](../manage-apps/what-is-single-sign-on.md)。
如果您沒有 Azure 訂用帳戶，請在開始之前先[建立免費帳戶](https://azure.microsoft.com/free/)。

## <a name="prerequisites"></a>Prerequisites

若要設定 Azure AD 與 Brightidea 的整合，您需要下列項目：

* Azure AD 訂用帳戶。 如果您沒有 Azure AD 環境，您可以在[這裡](https://azure.microsoft.com/pricing/free-trial/)取得一個月的試用帳戶
* 已啟用 Brightidea 單一登入的訂用帳戶

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD 單一登入。


* Brightidea 支援由 **SP 和 IDP** 起始的 SSO
* Brightidea 支援 **Just In Time** 使用者佈建


## <a name="adding-brightidea-from-the-gallery"></a>從資源庫新增 Brightidea

若要設定將 Brightidea 整合到 Azure AD 中，您需要從資源庫將 Brightidea 新增到受控 SaaS 應用程式清單。

**若要從資源庫新增 Brightidea，請執行下列步驟：**

1. 在 **[Azure 入口網站](https://portal.azure.com)** 的左方瀏覽窗格中，按一下 [Azure Active Directory]  圖示。

    ![Azure Active Directory 按鈕](common/select-azuread.png)

2. 瀏覽至 [企業應用程式]  ，然後選取 [所有應用程式]  選項。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

3. 若要新增新的應用程式，請按一下對話方塊頂端的 [新增應用程式]  按鈕。

    ![新增應用程式按鈕](common/add-new-app.png)

4. 在搜尋方塊中，輸入 **Brightidea**，從結果面板中選取 [Brightidea]  ，然後按一下 [新增]  按鈕以新增應用程式。

    ![結果清單中的 Brightidea](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>設定和測試 Azure AD 單一登入

在本節中，您會以名為 **Britta Simon** 的測試使用者為基礎，設定及測試與 Brightidea 搭配運作的 Azure AD 單一登入。
若要讓單一登入能夠運作，必須建立 Azure AD 使用者與 Brightidea 中相關使用者之間的連結關聯性。

若要設定及測試與 Brightidea 搭配運作的 Azure AD 單一登入，您需要完成下列構成要素：

1. **[設定 Azure AD 單一登入](#configure-azure-ad-single-sign-on)** - 讓您的使用者能夠使用此功能。
2. **[設定 Brightidea 單一登入](#configure-brightidea-single-sign-on)** - 在應用程式端設定單一登入設定。
3. **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 Britta Simon 測試 Azure AD 單一登入。
4. **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 Britta Simon 能夠使用 Azure AD 單一登入。
5. **[建立 Brightidea 測試使用者](#create-brightidea-test-user)** - 在 Brightidea 中建立一個與 Azure AD 中代表 Britta Simon 之使用者連結的 Britta Simon 對應項目。
6. **[測試單一登入](#test-single-sign-on)** ，驗證組態是否能運作。

### <a name="configure-azure-ad-single-sign-on"></a>設定 Azure AD 單一登入

在本節中，您會在 Azure 入口網站中啟用 Azure AD 單一登入。

若要設定與 Brightidea 搭配運作的 Azure AD 單一登入，請執行下列步驟：

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [Brightidea]  應用程式整合頁面上，選取 [單一登入]  。

    ![設定單一登入連結](common/select-sso.png)

2. 在 [選取單一登入方法]  對話方塊中，選取 [SAML/WS-Fed]  模式以啟用單一登入。

    ![單一登入選取模式](common/select-saml-option.png)

3. 在 [以 SAML 設定單一登入] 頁面上，按一下 [編輯] 圖示以開啟 [基本 SAML 設定] 對話方塊。   

    ![編輯基本 SAML 組態](common/edit-urls.png)

4. 在 [基本 SAML 設定]  區段上，如果您有 **服務提供者中繼資料檔案**，而想要以 **IDP** 起始模式進行設定，請執行下列步驟：

    a. 按一下 [上傳中繼資料檔案]  。

    ![上傳中繼資料檔案](common/upload-metadata.png)

    b. 按一下 **資料夾圖示** 以選取中繼資料檔案，然後按一下 [上傳]  。

    ![選擇中繼資料檔案](common/browse-upload-metadata.png)

    c. 在成功上傳中繼資料檔案之後，會自動在 [Brightidea] 區段文字方塊中填入 [識別碼]  和 [回覆 URL]  值：

    ![顯示基本 SAML 設定的螢幕擷取畫面，您可以在其中輸入識別碼、回覆 URL 以及選取 [儲存]。](common/idp-intiated.png)

    > [!Note]
    > 如果 [識別碼]  和 [回覆 URL]  值未自動填入，則請根據您的需求手動填入這些值。

5. 如果您想要以 **SP** 起始模式設定應用程式，請按一下 [設定其他 URL]  ，然後執行下列步驟：

    ![顯示您可以在其中輸入登入 URL 的設定額外 URL 螢幕擷取畫面。](common/metadata-upload-additional-signon.png)

    在 [登入 URL]  文字方塊中，以下列模式輸入 URL︰`https://<SUBDOMAIN>.brightidea.com`

4. 在 [以 SAML 設定單一登入]  頁面的 [SAML 簽署憑證]  區段中按一下 [下載]  ，以依據您的需求從指定選項下載 [同盟中繼資料 XML]  ，並儲存在您的電腦上。

    ![憑證下載連結](common/metadataxml.png)

6. 在 [設定 Brightidea]  區段上，依據您的需求複製適當的 URL。

    ![複製組態 URL](common/copy-configuration-urls.png)

    a. 登入 URL

    b. Azure AD 識別碼

    c. 登出 URL

### <a name="configure-brightidea-single-sign-on"></a>設定 Brightidea 單一登入

1. 在不同的網頁瀏覽器視窗中，使用系統管理員認證來登入 Brightidea。

2. 若要移至您 Brightidea 系統的 SSO 功能，請瀏覽至 [Enterprise Setup] \(企業設定\)   -> [Authentication] \(驗證\)  索引標籤。您會在該處看到兩個子索引標籤：[Auth Selection]\(驗證選取項目\) 和 [SAML Profiles] \(SAML 設定檔\)。

    ![顯示 Brightidea 網站的螢幕擷取畫面，其中已選取 [驗證] 索引標籤。](./media/brightidea-tutorial/configure1.png)

3. 選取 [Auth Selection]\(驗證選取項目\)  。 預設只會顯示兩個標準方法：Brightidea [Login] \(登入\) 和 [Registration] \(註冊\)。 新增 SSO 方法之後，它就會顯示在清單中。

    ![顯示 Brightidea 驗證索引標籤的螢幕擷取畫面，其中已選取驗證選項。](./media/brightidea-tutorial/configure2.png)

4. 選取 [SAML Profiles] \(SAML 設定檔\)  ，然後執行下列步驟：

    ![顯示已選取 [SAML 設定檔] 的 Brightidea 驗證索引標籤螢幕擷取畫面，其中提供下載中繼資料和新增的選項。](./media/brightidea-tutorial/configure3.png)

    a. 按一下 [Download Metadata] \(下載中繼資料\)  ，然後在 Azure 入口網站中的 [基本 SAML 設定]  區段上傳該中繼資料。

    b. 按一下 [Identity Provider Setting] \(身分識別提供者設定\)  底下的 [Add New] \(新增\)  按鈕，然後執行下列步驟：

    ![顯示 Brightidea 識別提供者設定的螢幕擷取畫面，您可以在其中輸入資訊。](./media/brightidea-tutorial/configure4.png)

   * 輸入 [SAML Profile Name] \(SAML 設定檔名稱\)  ，例如 `Azure Ad SSO`

   * 針對 [Upload Metadata] \(上傳中繼資料\)  ，按一下 [Choose File] \(選擇檔案\)，然後上傳從 Azure 入口網站下載的中繼資料檔案。

     > [!NOTE]
     > 上傳中繼資料檔案之後，剩餘的欄位 [Single Sign-on Service] \(單一登入服務\)、[Identity Provider Issuer] \(身分識別提供者簽發者\)、[Upload Public Key] \(上傳公開金鑰\)  中將會自動填入資料。

   * 在 [Email] \(電子郵件\)  文字方塊中，將值輸入為 `mail`。

   * 在 [Screen Name] \(畫面名稱\)  文字方塊中，將值輸入為 `givenName`。

   * 按一下 **[儲存變更]** 。  

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

在本節中，您會將 Brightidea 的存取權授與 Britta Simon，讓她能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，依序選取 [企業應用程式]  、[所有應用程式]  及 [Brightidea]  。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

2. 在應用程式清單中，選取 [Brightidea]  。

    ![應用程式清單中的 Brightidea 連結](common/all-applications.png)

3. 在左側功能表中，選取 [使用者和群組]  。

    ![[使用者和群組] 連結](common/users-groups-blade.png)

4. 按一下 [新增使用者]  按鈕，然後在 [新增指派]  對話方塊中，選取 [使用者和群組]  。

    ![[新增指派] 窗格](common/add-assign-user.png)

5. 在 [使用者和群組]  對話方塊的 [使用者] 清單中，選取 [Britta Simon]  ，然後按一下畫面底部的 [選取]  按鈕。

6. 如果您預期使用 SAML 判斷提示中的任何角色值，請在 [選取角色]  對話方塊的清單中選取適當使用者角色，然後按一下畫面底部的 [選取]  按鈕。

7. 在 [新增指派]  對話方塊中，按一下 [指派]  按鈕。

### <a name="create-brightidea-test-user"></a>建立 Brightidea 測試使用者

本節會在 Brightidea 中建立名為 Britta Simon 的使用者。 Brightidea 支援預設啟用的 Just-In-Time 使用者佈建。 在這一節沒有您需要進行的動作項目。 如果 Brightidea 中還沒有任何使用者存在，在驗證之後就會建立新的使用者。

### <a name="test-single-sign-on"></a>測試單一登入 

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您在「存取面板」中按一下 [Brightidea] 圖格時，應該會自動登入您已設定 SSO 的 Brightidea。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](../user-help/my-apps-portal-end-user-access.md)。

## <a name="additional-resources"></a>其他資源

- [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](./tutorial-list.md)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](../manage-apps/what-is-single-sign-on.md)

- [什麼是 Azure Active Directory 中的條件式存取？](../conditional-access/overview.md)