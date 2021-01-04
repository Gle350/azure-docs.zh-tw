---
title: 教學課程：Azure Active Directory 與 AppBlade 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 AppBlade 之間的單一登入。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/17/2019
ms.author: jeedes
ms.openlocfilehash: 49fa266319423b4c1a0bc56ad2a32b49c18f168e
ms.sourcegitcommit: d79513b2589a62c52bddd9c7bd0b4d6498805dbe
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/18/2020
ms.locfileid: "97672611"
---
# <a name="tutorial-azure-active-directory-integration-with-appblade"></a>教學課程：Azure Active Directory 與 AppBlade 整合

在本教學課程中，您會了解如何將 AppBlade 與 Azure Active Directory (Azure AD) 整合。
將 AppBlade 與 Azure AD 整合可提供下列優點：

* 您可以在 Azure AD 中控制可存取 AppBlade 的人員。
* 您可以讓使用者使用其 Azure AD 帳戶自動登入 AppBlade (單一登入)。
* 您可以在 Azure 入口網站中集中管理您的帳戶。

若您想了解 SaaS app 與 Azure AD 整合的更多詳細資訊，請參閱 [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](../manage-apps/what-is-single-sign-on.md)。
如果您沒有 Azure 訂用帳戶，請在開始之前先[建立免費帳戶](https://azure.microsoft.com/free/)。

## <a name="prerequisites"></a>Prerequisites

若要設定 Azure AD 與 AppBlade 整合，您需要下列項目：

* Azure AD 訂用帳戶。 如果您沒有 Azure AD 環境，您可以在[這裡](https://azure.microsoft.com/pricing/free-trial/)取得一個月的試用帳戶
* 已啟用 AppBlade 單一登入的訂用帳戶

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD 單一登入。

* AppBlade 支援由 **SP** 起始的 SSO
* AppBlade 支援 **Just In Time** 使用者佈建

## <a name="adding-appblade-from-the-gallery"></a>從資源庫新增 AppBlade

若要設定將 AppBlade 整合到 Azure AD 中，您需要從資源庫將 AppBlade 新增到受控 SaaS 應用程式清單。

**若要從資源庫新增 AppBlade，請執行下列步驟：**

1. 在 **[Azure 入口網站](https://portal.azure.com)** 的左方瀏覽窗格中，按一下 [Azure Active Directory]  圖示。

    ![Azure Active Directory 按鈕](common/select-azuread.png)

2. 瀏覽至 [企業應用程式]  ，然後選取 [所有應用程式]  選項。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

3. 若要新增新的應用程式，請按一下對話方塊頂端的 [新增應用程式]  按鈕。

    ![新增應用程式按鈕](common/add-new-app.png)

4. 在搜尋方塊中，輸入 **AppBlade**，從結果面板中選取 [AppBlade]  ，然後按一下 [新增]  按鈕以新增應用程式。

    ![結果清單中的 AppBlade](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>設定和測試 Azure AD 單一登入

在本節中，您會以名為 **Britta Simon** 的測試使用者為基礎，設定及測試與 AppBlade 搭配運作的 Azure AD 單一登入。
若要讓單一登入能夠運作，必須建立 Azure AD 使用者與 AppBlade 中相關使用者之間的連結關聯性。

若要設定及測試與 AppBlade 搭配運作的 Azure AD 單一登入，您需要完成下列構成要素：

1. **[設定 Azure AD 單一登入](#configure-azure-ad-single-sign-on)** - 讓您的使用者能夠使用此功能。
2. **[設定 AppBlade 單一登入](#configure-appblade-single-sign-on)** - 在應用程式端設定單一登入設定。
3. **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 Britta Simon 測試 Azure AD 單一登入。
4. **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 Britta Simon 能夠使用 Azure AD 單一登入。
5. **[建立 AppBlade 測試使用者](#create-appblade-test-user)** - 在 AppBlade 中建立一個與 Azure AD 中代表 Britta Simon 之使用者連結的 Britta Simon 對應項目。
6. **[測試單一登入](#test-single-sign-on)** ，驗證組態是否能運作。

### <a name="configure-azure-ad-single-sign-on"></a>設定 Azure AD 單一登入

在本節中，您會在 Azure 入口網站中啟用 Azure AD 單一登入。

若要設定與 AppBlade 搭配運作的 Azure AD 單一登入，請執行下列步驟：

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [AppBlade]  應用程式整合頁面上，選取 [單一登入]  。

    ![設定單一登入連結](common/select-sso.png)

2. 在 [選取單一登入方法]  對話方塊中，選取 [SAML/WS-Fed]  模式以啟用單一登入。

    ![單一登入選取模式](common/select-saml-option.png)

3. 在 [以 SAML 設定單一登入] 頁面上，按一下 [編輯] 圖示以開啟 [基本 SAML 設定] 對話方塊。   

    ![編輯基本 SAML 組態](common/edit-urls.png)

4. 在 [基本 SAML 組態]  區段上，執行下列步驟：

    ![AppBlade 網域及 URL 單一登入資訊](common/sp-signonurl.png)

    在 [登入 URL]  文字方塊中，以下列模式輸入 URL︰`https://<companyname>.appblade.com/saml/<tenantid>`

    > [!NOTE]
    > 這不是真正的值。 請使用實際的「登入 URL」來更新此值。 請連絡 [AppBlade 用戶端支援小組](mailto:support@appblade.com)以取得此值。 您也可以參考 Azure 入口網站中 **基本 SAML 組態** 區段所示的模式。

5. 在 [以 SAML 設定單一登入]  頁面的 [SAML 簽署憑證]  區段中按一下 [下載]  ，以依據您的需求從指定選項下載 **同盟中繼資料 XML**，並儲存在您的電腦上。

    ![憑證下載連結](common/metadataxml.png)

6. 在 [設定 AppBlade]  區段上，依據您的需求複製適當的 URL。

    ![複製組態 URL](common/copy-configuration-urls.png)

    a. 登入 URL

    b. Azure AD 識別碼

    c. 登出 URL

### <a name="configure-appblade-single-sign-on"></a>設定 AppBlade 單一登入

若要在 **AppBlade** 端設定單一登入，您必須將從 Azure 入口網站下載的 [同盟中繼資料 XML]  和所複製的適當 URL 傳送給 [AppBlade 支援小組](mailto:support@appblade.com)。 此外，請要求他們將 [SSO 簽發者 URL]  設定為 `https://appblade.com/saml`。 單一登入必須要有這項設定才能運作。

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

在本節中，您會將 AppBlade 的存取權授與 Britta Simon，讓她能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，依序選取 [企業應用程式]  、[所有應用程式]  及 [AppBlade]  。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

2. 在應用程式清單中，選取 [AppBlade]  。

    ![應用程式清單中的 AppBlade 連結](common/all-applications.png)

3. 在左側功能表中，選取 [使用者和群組]  。

    ![[使用者和群組] 連結](common/users-groups-blade.png)

4. 按一下 [新增使用者]  按鈕，然後在 [新增指派]  對話方塊中，選取 [使用者和群組]  。

    ![[新增指派] 窗格](common/add-assign-user.png)

5. 在 [使用者和群組]  對話方塊的 [使用者] 清單中，選取 [Britta Simon]  ，然後按一下畫面底部的 [選取]  按鈕。

6. 如果您預期使用 SAML 判斷提示中的任何角色值，請在 [選取角色]  對話方塊的清單中選取適當使用者角色，然後按一下畫面底部的 [選取]  按鈕。

7. 在 [新增指派]  對話方塊中，按一下 [指派]  按鈕。

### <a name="create-appblade-test-user"></a>建立 AppBlade 測試使用者

本節的目標是要在 AppBlade 中建立一個名為 Britta Simon 的使用者。 AppBlade 支援預設啟用的 Just-In-Time 佈建。 **請確定您已搭配 AppBlade 來設定網域名稱，以便進行使用者佈建。之後，將只有 Just-In-Time 使用者佈建能夠運作。**

如果使用者的電子郵件地址結尾是 AppBlade 為您帳戶設定的網域，則使用者會自動以您指定的權限層級加入成為該帳戶的成員，指定的權限層級為「基本」(只能安裝應用程式的基本使用者)、「小組成員」(可以上傳新應用程式版本及管理專案的使用者) 或「系統管理員」(完整的帳戶系統管理員權限) 其中之一。 通常您會選擇「基本」，然後透過「系統管理員登入」手動提升使用者權限 (AppBlade 需要預先設定一個電子郵件型系統管理員登入，或是在登入後代表客戶提升使用者權限)。

在這一節沒有您需要進行的動作項目。 嘗試存取 AppBlade 時，如果使用者還不存在，就會建立新使用者。

> [!NOTE]
> 如果您需要手動建立使用者，則必須連絡 [AppBlade 支援小組](mailto:support@appblade.com)。

### <a name="test-single-sign-on"></a>測試單一登入

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您在「存取面板」中按一下 [AppBlade] 圖格時，應該會自動登入您已設定 SSO 的 AppBlade。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](../user-help/my-apps-portal-end-user-access.md)。

## <a name="additional-resources"></a>其他資源

- [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](./tutorial-list.md)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](../manage-apps/what-is-single-sign-on.md)

- [什麼是 Azure Active Directory 中的條件式存取？](../conditional-access/overview.md)