---
title: 教學課程：Azure Active Directory 與 Kantega SSO for JIRA 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 Kantega SSO for JIRA 之間的單一登入。
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
ms.openlocfilehash: 9643d0e63e85a9b500021a415e3cdaf3edc756c5
ms.sourcegitcommit: e15c0bc8c63ab3b696e9e32999ef0abc694c7c41
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/16/2020
ms.locfileid: "97608726"
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-jira"></a>教學課程：Azure Active Directory 與 Kantega SSO for JIRA 整合

在本教學課程中，您會了解如何整合 Kantega SSO for JIRA 與 Azure Active Directory (Azure AD)。
Kantega SSO for JIRA 與 Azure AD 整合提供下列優點：

* 您可以在 Azure AD 中控制可存取 Kantega SSO for JIRA 的人員。
* 您可以讓使用者使用其 Azure AD 帳戶自動登入 Kantega SSO for JIRA (單一登入)。
* 您可以在 Azure 入口網站中集中管理您的帳戶。

若您想了解 SaaS app 與 Azure AD 整合的更多詳細資訊，請參閱 [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](../manage-apps/what-is-single-sign-on.md)。
如果您沒有 Azure 訂用帳戶，請在開始之前先[建立免費帳戶](https://azure.microsoft.com/free/)。

## <a name="prerequisites"></a>Prerequisites

若要設定 Azure AD 與 Kantega SSO for JIRA 整合，您需要下列項目：

* Azure AD 訂用帳戶。 如果您沒有 Azure AD 環境，您可以申請[免費帳戶](https://azure.microsoft.com/free/)
* 已啟用 Kantega SSO for JIRA 單一登入的訂用帳戶

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD 單一登入。

* Kantega SSO for JIRA 支援由 **SP 和 IDP** 起始的 SSO

## <a name="adding-kantega-sso-for-jira-from-the-gallery"></a>從資源庫新增 Kantega SSO for JIRA

若要設定將 Kantega SSO for JIRA 整合到 Azure AD 中，您需要從資源庫將 Kantega SSO for JIRA 新增到受控 SaaS 應用程式清單。

**若要從資源庫新增 Kantega SSO for JIRA，請執行下列步驟：**

1. 在 **[Azure 入口網站](https://portal.azure.com)** 的左方瀏覽窗格中，按一下 [Azure Active Directory]  圖示。

    ![Azure Active Directory 按鈕](common/select-azuread.png)

2. 瀏覽至 [企業應用程式]  ，然後選取 [所有應用程式]  選項。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

3. 若要新增新的應用程式，請按一下對話方塊頂端的 [新增應用程式]  按鈕。

    ![新增應用程式按鈕](common/add-new-app.png)

4. 在搜尋方塊中，輸入 **Kantega SSO for JIRA**，從結果面板中選取 [Kantega SSO for JIRA]  ，然後按一下 [新增]  按鈕以新增應用程式。

    ![結果清單中的 Kantega SSO for JIRA](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>設定和測試 Azure AD 單一登入

在本節中，您會以名為 **Britta Simon** 的測試使用者身分，設定及測試與 Kantega SSO for JIRA 搭配運作的 Azure AD 單一登入。
若要讓單一登入能夠運作，必須建立 Azure AD 使用者與 Kantega SSO for JIRA 中相關使用者之間的連結關聯性。

若要設定及測試與 Kantega SSO for JIRA 搭配運作的 Azure AD 單一登入，您需要完成下列建置組塊：

1. **[設定 Azure AD 單一登入](#configure-azure-ad-single-sign-on)** - 讓您的使用者能夠使用此功能。
2. **[設定 Kantega SSO for JIRA 單一登入](#configure-kantega-sso-for-jira-single-sign-on)** - 在應用程式端設定單一登入設定。
3. **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 Britta Simon 測試 Azure AD 單一登入。
4. **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 Britta Simon 能夠使用 Azure AD 單一登入。
5. **[建立 Kantega SSO for JIRA 測試使用者](#create-kantega-sso-for-jira-test-user)** - 使 Kantega SSO for JIRA 中對應的 Britta Simon 連結到該使用者在 Azure AD 中的代表項目。
6. **[測試單一登入](#test-single-sign-on)** ，驗證組態是否能運作。

### <a name="configure-azure-ad-single-sign-on"></a>設定 Azure AD 單一登入

在本節中，您會在 Azure 入口網站中啟用 Azure AD 單一登入。

若要設定與 Kantega SSO for JIRA 搭配運作的 Azure AD 單一登入，請執行下列步驟：

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [Kantega SSO for JIRA]  應用程式整合頁面上，選取 [單一登入]  。

    ![設定單一登入連結](common/select-sso.png)

2. 在 [選取單一登入方法]  對話方塊中，選取 [SAML/WS-Fed]  模式以啟用單一登入。

    ![單一登入選取模式](common/select-saml-option.png)

3. 在 [以 SAML 設定單一登入] 頁面上，按一下 [編輯] 圖示以開啟 [基本 SAML 設定] 對話方塊。   

    ![編輯基本 SAML 組態](common/edit-urls.png)

4. 在 [基本 SAML 設定]  區段上，如果您想要以 **IDP** 起始模式設定應用程式，請執行下列步驟：

    ![顯示 [基本 SAML 設定] 的螢幕擷取畫面，其中已反白顯示 [識別碼] 和 [回覆 U R L] 文字方塊，並已選取 [儲存] 按鈕。](common/idp-intiated.png)

    a. 在 [識別碼]  文字方塊中，使用下列模式來輸入 URL：`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    b. 在 [回覆 URL]  文字方塊中，使用下列模式來輸入 URL：`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

5. 如果您想要以 **SP** 起始模式設定應用程式，請按一下 [設定其他 URL]，然後執行下列步驟：

    ![Kantega SSO for JIRA 網域與 URL 單一登入資訊](common/metadata-upload-additional-signon.png)

    在 [登入 URL]  文字方塊中，以下列模式輸入 URL︰`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    > [!NOTE]
    > 這些都不是真正的值。 使用實際的識別碼、回覆 URL 和登入 URL 來更新這些值。 在設定 Jira 外掛程式 (本教學課程稍後會說明) 期間會收到這些值。

6. 在 [以 SAML 設定單一登入]  頁面的 [SAML 簽署憑證]  區段中按一下 [下載]  ，以依據您的需求從指定選項下載 **同盟中繼資料 XML**，並儲存在您的電腦上。

    ![憑證下載連結](common/metadataxml.png)

7. 在 [設定 Kantega SSO for JIRA]  區段上，依據您的需求複製適當的 URL。

    ![複製組態 URL](common/copy-configuration-urls.png)

    a. 登入 URL

    b. Azure AD 識別碼

    c. 登出 URL

### <a name="configure-kantega-sso-for-jira-single-sign-on"></a>設定 Kantega SSO for JIRA 單一登入

1. 在不同的網頁瀏覽器視窗中，以系統管理員身分登入您的 JIRA 內部部署伺服器。

1. 將滑鼠停留在 cog 上，然後按一下 [附加元件]  。

    ![顯示已選取 [齒輪] 圖示，並已從下拉式清單中選取 [附加元件] 的螢幕擷取畫面。](./media/kantegassoforjira-tutorial/addon1.png)

1. 在附加元件索引標籤區段下，按一下 [尋找新的附加元件]  。 搜尋 **Kantega SSO for JIRA (SAML & Kerberos)** ，然後按一下 [安裝]  按鈕以安裝新的 SAML 外掛程式。

    ![顯示 [尋找新的附加元件] 區段的螢幕擷取畫面，其中包含搜尋方塊中的 [Kantego S S O for JIRA (S A M L & Kerberos)]，並已選取 [安裝] 按鈕。](./media/kantegassoforjira-tutorial/addon2.png)

1. 外掛程式會開始安裝。

    ![顯示外掛程式 [安裝中] 對話方塊的螢幕擷取畫面。](./media/kantegassoforjira-tutorial/addon3.png)

1. 當安裝完成時。 按一下 [關閉]  。

    ![顯示 [已安裝並準備就緒!] 對話方塊的螢幕擷取畫面， 其中已選取「關閉」動作。](./media/kantegassoforjira-tutorial/addon33.png)

1.  按一下 [管理] 。

    ![顯示 [Kantega S S O] 應用程式頁面的螢幕擷取畫面，其中已選取 [管理] 按鈕。](./media/kantegassoforjira-tutorial/addon34.png)
    
1. [整合] 下會列出新的外掛程式。 按一下 [設定] 來設定新的外掛程式。

    ![顯示已在左側導覽功能表中反白顯示 [整合]，並已在 [管理附加元件] 區段中選取 [設定] 按鈕的螢幕擷取畫面。](./media/kantegassoforjira-tutorial/addon35.png)

1. 在 [SAML] 區段中。 從 [新增識別提供者] 下拉式清單中，選取 [Azure Active Directory]\(Azure AD\)。

    ![顯示 [新增識別提供者] 下拉式清單的螢幕擷取畫面，其中已選取 [Azure Active Directory (Azure A D)]。](./media/kantegassoforjira-tutorial/addon4.png)

1. 選取 [基本] 作為訂用帳戶層級。

    ![顯示 [準備 Azure A D] 區段的螢幕擷取畫面，其中已選取 [基本]。](./media/kantegassoforjira-tutorial/addon5.png)

1. 在 [應用程式屬性] 區段中，執行下列步驟： 

    ![顯示 [應用程式屬性] 區段的螢幕擷取畫面，其中已反白顯示 [應用程式 I D U R L] 文字方塊和 [複製] 按鈕，並已選取 [下一步] 按鈕。](./media/kantegassoforjira-tutorial/addon6.png)

    1. 複製 [應用程式識別碼 URI] 值，然後在 Azure 入口網站的 [基本 SAML 設定] 區段上，使用此 URI 作為 [識別碼]、[回覆 URL] 和 [登入 URL]。

    1. 按 [下一步]  。

1. 在 [中繼資料匯入]  區段中，執行下列步驟： 

    ![顯示 [中繼資料匯入] 區段的螢幕擷取畫面，其中已選取 [我的電腦上的中繼資料檔案]。](./media/kantegassoforjira-tutorial/addon7.png)

    1. 選取 [我的電腦上的中繼資料檔案]  ，上傳您從 Azure 入口網站下載的中繼資料檔案。

    1. 按 [下一步]  。

1. 在 [名稱和 SSO 位置]  區段中，執行下列步驟：

    ![顯示 [名稱和 S S O 位置] 的螢幕擷取畫面，其中已反白顯示 [識別提供者名稱] 文字方塊，並已選取 [下一步] 按鈕。](./media/kantegassoforjira-tutorial/addon8.png)

    1. 在 [識別提供者名稱]  文字方塊中，新增識別提供者的名稱 (例如 Azure AD)。

    1. 按 [下一步]  。

1. 確認簽署憑證，然後按 [下一步]  。

    ![顯示 [簽章驗證] 區段的螢幕擷取畫面，其中已選取 [下一步] 按鈕。](./media/kantegassoforjira-tutorial/addon9.png)

1. 在 [JIRA 使用者帳戶] 區段中，執行下列步驟：

    ![顯示 [JIRA 使用者帳戶] 的螢幕擷取畫面，其中已反白顯示 [在 JIRA 的內部目錄中建立使用者] 選項，並已選取 [下一步] 按鈕。](./media/kantegassoforjira-tutorial/addon10.png)

    1. 選取 [如果需要，在 JIRA 的內部目錄中建立使用者]，然後輸入使用者群組的適當名稱 (可以是多個 群組，以逗號分隔)。

    1. 按 [下一步]  。

1. 按一下 [完成] 。

    ![顯示 [摘要] 區段的螢幕擷取畫面，其中已選取 [完成] 按鈕。](./media/kantegassoforjira-tutorial/addon11.png)

1. 在 [Azure AD 的已知網域] 區段中，執行下列步驟：

    ![設定單一登入](./media/kantegassoforjira-tutorial/addon12.png)

    1. 從頁面的左面板中，選取 [已知網域]  。

    2. 在 [已知網域]  文字方塊中，輸入網域名稱。

    3. 按一下 [檔案]  。

### <a name="create-an-azure-ad-test-user"></a>建立 Azure AD 測試使用者

本節的目標是要在 Azure 入口網站中建立一個名為 Britta Simon 的測試使用者。

1. 在 Azure 入口網站的左窗格中，依序選取 [Azure Active Directory]  、[使用者]  和 [所有使用者]  。

    ![[使用者和群組] 與 [所有使用者] 連結](common/users.png)

2. 在畫面頂端選取 [新增使用者]  。

    ![[新增使用者] 按鈕](common/new-user.png)

3. 在 [使用者] 屬性中，執行下列步驟。

    ![[使用者] 對話方塊](common/user-properties.png)

    1. 在 [名稱]  欄位中，輸入 **BrittaSimon**。

    1. 在 [使用者名稱]  欄位中，輸入 `brittasimon@yourcompanydomain.extension`。 例如， BrittaSimon@contoso.com

    1. 選取 [顯示密碼]  核取方塊，然後記下 [密碼] 方塊中顯示的值。

    1. 按一下頁面底部的 [新增]  。

### <a name="assign-the-azure-ad-test-user"></a>指派 Azure AD 測試使用者

在本節中，您會將 Kantega SSO for JIRA 的存取權授與 Britta Simon，讓她能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，依序選取 [企業應用程式]、[所有應用程式] 及 [Kantega SSO for JIRA]。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

2. 在應用程式清單中，選取 [Kantega SSO for JIRA]。

    ![應用程式清單中的 Kantega SSO for JIRA 連結](common/all-applications.png)

3. 在左側功能表中，選取 [使用者和群組]  。

    ![[使用者和群組] 連結](common/users-groups-blade.png)

4. 按一下 [新增使用者] 按鈕，然後在 [新增指派] 對話方塊中，選取 [使用者和群組]。

    ![[新增指派] 窗格](common/add-assign-user.png)

5. 在 [使用者和群組]  對話方塊的 [使用者] 清單中，選取 [Britta Simon]  ，然後按一下畫面底部的 [選取]  按鈕。

6. 如果您預期使用 SAML 判斷提示中的任何角色值，請在 [選取角色]  對話方塊的清單中選取適當使用者角色，然後按一下畫面底部的 [選取]  按鈕。

7. 在 [新增指派]  對話方塊中，按一下 [指派]  按鈕。

### <a name="create-kantega-sso-for-jira-test-user"></a>建立 Kantega SSO for JIRA 測試使用者

若要讓 Azure AD 使用者能夠登入 JIRA，必須將他們佈建到 JIRA。 在 Kantega SSO for JIRA 中，佈建是手動工作。

**若要佈建使用者帳戶，請執行下列步驟：**

1. 以系統管理員身分登入您的 JIRA 內部部署伺服器。

1. 將滑鼠停留在 cog 上，然後按一下 [使用者管理]  。

    ![顯示已選取 [齒輪] 圖示，並已從下拉式清單中選取 [使用者管理] 的螢幕擷取畫面。](./media/kantegassoforjira-tutorial/user1.png) 

1. 在 [使用者管理] 索引標籤區段下，按一下 [建立使用者]。

    ![顯示 [使用者管理] 區段的螢幕擷取畫面，其中已選取 [建立使用者] 按鈕。](./media/kantegassoforjira-tutorial/user2.png) 

1. 在 [建立新的使用者]  對話方塊中，執行下列步驟：

    ![新增員工](./media/kantegassoforjira-tutorial/user3.png) 

    1. 在 [電子郵件地址]  文字方塊中，輸入像是 Brittasimon@contoso.com 的使用者電子郵件地址。

    2. 在 [全名]  文字方塊中，輸入像是 Britta Simon 的使用者全名。

    3. 在 [使用者名稱]  文字方塊中，輸入像是 Brittasimon@contoso.com 的使用者電子郵件。

    4. 在 [密碼]  文字方塊中，輸入使用者的密碼。

    5. 按一下 [建立使用者]  。

### <a name="test-single-sign-on"></a>測試單一登入 

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您在存取面板中按一下 [Kantega SSO for JIRA] 圖格時，應該會自動登入您已設定 SSO 的 Kantega SSO for JIRA。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](../user-help/my-apps-portal-end-user-access.md)。

## <a name="additional-resources"></a>其他資源

- [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](./tutorial-list.md)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](../manage-apps/what-is-single-sign-on.md)

- [什麼是 Azure Active Directory 中的條件式存取？](../conditional-access/overview.md)