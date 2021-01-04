---
title: 教學課程：Azure Active Directory 與 Comeet Recruiting Software 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 Comeet Recruiting Software 之間的單一登入。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/22/2019
ms.author: jeedes
ms.openlocfilehash: d458d94a4bee4fce523819af2fd3e555db6e70df
ms.sourcegitcommit: d79513b2589a62c52bddd9c7bd0b4d6498805dbe
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/18/2020
ms.locfileid: "97671991"
---
# <a name="tutorial-azure-active-directory-integration-with-comeet-recruiting-software"></a>教學課程：Azure Active Directory 與 Comeet Recruiting Software 整合

在此教學課程中，您將了解如何整合 Comeet Recruiting Software 與 Azure Active Directory (Azure AD)。
Comeet Recruiting Software 與 Azure AD 整合提供下列優點：

* 您可以在 Azure AD 中控制可存取 Comeet Recruiting Software 的人員。
* 您可以讓使用者使用其 Azure AD 帳戶自動登入 Comeet Recruiting Software (單一登入)。
* 您可以在 Azure 入口網站中集中管理您的帳戶。

若您想了解 SaaS app 與 Azure AD 整合的更多詳細資訊，請參閱 [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](../manage-apps/what-is-single-sign-on.md)。
如果您沒有 Azure 訂用帳戶，請在開始之前先[建立免費帳戶](https://azure.microsoft.com/free/)。

## <a name="prerequisites"></a>Prerequisites

若要設定 Azure AD 與 Comeet Recruiting Software 整合，您需要以下項目：

* Azure AD 訂用帳戶。 如果您沒有 Azure AD 環境，您可以在[這裡](https://azure.microsoft.com/pricing/free-trial/)取得一個月的試用帳戶
* 已啟用 Comeet Recruiting Software 單一登入的訂用帳戶

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD 單一登入。

* Comeet Recruiting Software 支援由 **SP 和 IDP** 起始的 SSO

## <a name="adding-comeet-recruiting-software-from-the-gallery"></a>從資源庫新增 Comeet Recruiting Software

若要設定 Comeet Recruiting Software 與 Azure AD 整合，您需要從資源庫將 Comeet Recruiting Software 新增到受控 SaaS 應用程式清單。

**若要從資源庫新增 Comeet Recruiting Software，請執行下列步驟：**

1. 在 **[Azure 入口網站](https://portal.azure.com)** 的左方瀏覽窗格中，按一下 [Azure Active Directory]  圖示。

    ![Azure Active Directory 按鈕](common/select-azuread.png)

2. 瀏覽至 [企業應用程式]  ，然後選取 [所有應用程式]  選項。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

3. 若要新增新的應用程式，請按一下對話方塊頂端的 [新增應用程式]  按鈕。

    ![新增應用程式按鈕](common/add-new-app.png)

4. 在搜尋方塊中，輸入 **Comeet Recruiting Software**，從結果面板中選取 [Comeet Recruiting Software]  ，然後按一下 [新增]  按鈕以新增應用程式。

    ![結果清單中的 Comeet Recruiting Software](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>設定和測試 Azure AD 單一登入

在本節中，您會以名為 **Britta Simon** 的測試使用者為基礎，設定及測試與 Comeet Recruiting Software 搭配運作的 Azure AD 單一登入。
若要讓單一登入能夠運作，必須建立 Azure AD 使用者與 Comeet Recruiting Software 中相關使用者之間的連結關聯性。

若要使用 Comeet Recruiting Software 設定並測試 Azure AD 單一登入，您需要完成下列建置組塊：

1. **[設定 Azure AD 單一登入](#configure-azure-ad-single-sign-on)** - 讓您的使用者能夠使用此功能。
2. **[設定 Comeet Recruiting Software 單一登入](#configure-comeet-recruiting-software-single-sign-on)** - 在應用程式端設定單一登入設定。
3. **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 Britta Simon 測試 Azure AD 單一登入。
4. **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 Britta Simon 能夠使用 Azure AD 單一登入。
5. **[建立 Comeet Recruiting Software 測試使用者](#create-comeet-recruiting-software-test-user)** - 讓 Comeet Recruiting Software 中對應的 Britta Simon 連結到使用者在 Azure AD 中的代表項目。
6. **[測試單一登入](#test-single-sign-on)** ，驗證組態是否能運作。

### <a name="configure-azure-ad-single-sign-on"></a>設定 Azure AD 單一登入

在本節中，您會在 Azure 入口網站中啟用 Azure AD 單一登入。

若要使用 Comeet Recruiting Software 設定 Azure AD 單一登入，請執行下列步驟：

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [Comeet Recruiting Software]  應用程式整合頁面上，選取 [單一登入]  。

    ![設定單一登入連結](common/select-sso.png)

2. 在 [選取單一登入方法]  對話方塊中，選取 [SAML/WS-Fed]  模式以啟用單一登入。

    ![單一登入選取模式](common/select-saml-option.png)

3. 在 [以 SAML 設定單一登入] 頁面上，按一下 [編輯] 圖示以開啟 [基本 SAML 設定] 對話方塊。   

    ![編輯基本 SAML 組態](common/edit-urls.png)

4. 在 [基本 SAML 組態]  區段上，若您想要以 **IDP** 起始模式設定應用程式，請執行下列步驟：

    ![顯示 [基本 SAML 設定] 頁面的螢幕擷取畫面，其中醒目提示 [儲存] 動作、[識別碼] 欄位和 [回覆 URL] 欄位。](common/idp-intiated.png)

    a. 在 [識別碼]  文字方塊中，使用下列模式來輸入 URL：`https://app.comeet.co/adfs_auth/acs/<UNIQUEID>/`

    b. 在 [回覆 URL]  文字方塊中，使用下列模式來輸入 URL：`https://app.comeet.co/adfs_auth/acs/<UNIQUEID>/`

    > [!NOTE]
    > 這些都不是真正的值。 請使用實際的識別碼和回覆 URL 更新這些值。 請連絡 [Comeet Recruiting Software 用戶端支援小組](https://support.comeet.co/knowledgebase/adfs-single-sign-on/)以取得這些值。 您也可以參考 Azure 入口網站中 **基本 SAML 組態** 區段所示的模式。

5. 如果您想要以 **SP** 起始模式設定應用程式，請按一下 [設定其他 URL]，然後執行下列步驟：

    ![Comeet Recruiting Software 網域與 URL 單一登入資訊](common/metadata-upload-additional-signon.png)

    在 [登入 URL]  文字方塊中，輸入 URL：`https://app.comeet.co`

5. Comeet Recruiting Software 應用程式預期要有特定格式的 SAML 判斷提示。 設定此應用程式的下列宣告。 您可以在應用程式整合頁面的 [使用者屬性]  區段中，管理這些屬性的值。 在 [以 SAML 設定單一登入]  頁面上，按一下 [編輯]  按鈕以開啟 [使用者屬性]  對話方塊。

    ![顯示 [使用者屬性] 區段的螢幕擷取畫面，其中已選取 [編輯] 按鈕。](common/edit-attribute.png)

6. 在 [使用者屬性] 對話方塊的 [使用者宣告] 區段中，使用 [編輯] 圖示來編輯宣告或使用 [新增宣告] 來新增宣告，如上圖所示設定 SAML 權杖屬性，然後執行下列步驟： 

    | 名稱 |  來源屬性|
    | ---------------| --------------- |
    | nameidentifier | user.mail |
    | comeet_id | user.userprincipalname |

    a. 按一下 [新增宣告]  以開啟 [管理使用者宣告]  對話方塊。

    ![顯示 [使用者宣告] 區段的螢幕擷取畫面，其中醒目提示 [新增宣告] 和 [儲存] 動作。](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. 在 [名稱]  文字方塊中，輸入該資料列所顯示的屬性名稱。

    c. 讓 [命名空間]  保持空白。

    d. 選取 [來源] 作為 [屬性]  。

    e. 在 [來源屬性]  清單中，輸入該資料列所顯示的屬性值。

    f. 按一下 [確定]  。

    g. 按一下 [檔案]  。

4. 在 [以 SAML 設定單一登入]  頁面的 [SAML 簽署憑證]  區段中按一下 [下載]  ，以依據您的需求從指定選項下載 **同盟中繼資料 XML**，並儲存在您的電腦上。

    ![憑證下載連結](common/metadataxml.png)

6. 在 [設定 Comeet Recruiting Software]  區段上，依據您的需求複製適當的 URL。

    ![複製組態 URL](common/copy-configuration-urls.png)

    a. 登入 URL

    b. Azure AD 識別碼

    c. 登出 URL

### <a name="configure-comeet-recruiting-software-single-sign-on"></a>設定 Comeet Recruiting Software 單一登入

若要在 **Comeet Recruiting Software** 端設定單一登入，您必須將從 Azure 入口網站下載的 [同盟中繼資料 XML]  和複製的適當 URL 傳送給 [Comeet Recruiting Software 支援小組](https://support.comeet.co/knowledgebase/adfs-single-sign-on/)。 他們會進行此設定，讓兩端的 SAML SSO 連線都設定正確。

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

在本節中，您會把 Comeet Recruiting Software 的存取權授與 Britta Simon，讓她能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，依序選取 [企業應用程式]  、[所有應用程式]  及 [Comeet Recruiting Software]  。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

2. 在應用程式清單中，選取 [Comeet Recruiting Software]  。

    ![應用程式清單中的 Comeet Recruiting Software 連結](common/all-applications.png)

3. 在左側功能表中，選取 [使用者和群組]  。

    ![[使用者和群組] 連結](common/users-groups-blade.png)

4. 按一下 [新增使用者] 按鈕，然後在 [新增指派] 對話方塊中，選取 [使用者和群組]。

    ![[新增指派] 窗格](common/add-assign-user.png)

5. 在 [使用者和群組]  對話方塊的 [使用者] 清單中，選取 [Britta Simon]  ，然後按一下畫面底部的 [選取]  按鈕。

6. 如果您預期使用 SAML 判斷提示中的任何角色值，請在 [選取角色]  對話方塊的清單中選取適當使用者角色，然後按一下畫面底部的 [選取]  按鈕。

7. 在 [新增指派]  對話方塊中，按一下 [指派]  按鈕。

### <a name="create-comeet-recruiting-software-test-user"></a>建立 Comeet Recruiting Software 測試使用者

在本節中，您要在 Comeet Recruiting Software 中建立名為 Britta Simon 的使用者。 請與 [Comeet Recruiting Software 支援團隊](mailto:support@comeet.co) 合作，以在 Comeet Recruiting Software 平台中新增使用者。 您必須先建立和啟動使用者，然後才能使用單一登入。

### <a name="test-single-sign-on"></a>測試單一登入 

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您在存取面板中按一下 [Comeet Recruiting Software] 圖格時，應該會自動登入您設定 SSO 的 Comeet Recruiting Software。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](../user-help/my-apps-portal-end-user-access.md)。

## <a name="additional-resources"></a>其他資源

- [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](./tutorial-list.md)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](../manage-apps/what-is-single-sign-on.md)

- [什麼是 Azure Active Directory 中的條件式存取？](../conditional-access/overview.md)