---
title: 教學課程：Azure Active Directory 與 Coupa 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 Coupa 之間的單一登入。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/25/2019
ms.author: jeedes
ms.openlocfilehash: c6d102f4d9a67f58469242cb44d2c6466782f006
ms.sourcegitcommit: d79513b2589a62c52bddd9c7bd0b4d6498805dbe
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/18/2020
ms.locfileid: "97674169"
---
# <a name="tutorial-azure-active-directory-integration-with-coupa"></a>教學課程：Azure Active Directory 與 Coupa 整合

在本教學課程中，您將了解如何整合 Coupa 與 Azure Active Directory (Azure AD)。
將 Coupa 與 Azure AD 整合提供下列優點：

* 您可以在 Azure AD 中控制可存取 Coupa 的人員。
* 您可以讓使用者使用他們的 Azure AD 帳戶自動登入 Coupa (單一登入)。
* 您可以在 Azure 入口網站中集中管理您的帳戶。

若您想了解 SaaS app 與 Azure AD 整合的更多詳細資訊，請參閱 [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](../manage-apps/what-is-single-sign-on.md)。
如果您沒有 Azure 訂用帳戶，請在開始之前先[建立免費帳戶](https://azure.microsoft.com/free/)。

## <a name="prerequisites"></a>Prerequisites

若要設定 Azure AD 與 Coupa 整合，您需要下列項目：

* Azure AD 訂用帳戶。 如果您沒有 Azure AD 環境，您可以在[這裡](https://azure.microsoft.com/pricing/free-trial/)取得一個月的試用帳戶
* 已啟用 Coupa 單一登入的訂用帳戶

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD 單一登入。

* Coupa 支援 **SP** 起始的 SSO

## <a name="adding-coupa-from-the-gallery"></a>從資源庫新增 Coupa

若要設定將 Coupa 整合到 Azure AD 中，您需要將 Coupa 從資源庫新增到受控 SaaS 應用程式清單。

**若要從資源庫新增 Coupa，請執行下列步驟：**

1. 在 **[Azure 入口網站](https://portal.azure.com)** 的左方瀏覽窗格中，按一下 [Azure Active Directory]  圖示。

    ![Azure Active Directory 按鈕](common/select-azuread.png)

2. 瀏覽至 [企業應用程式]  ，然後選取 [所有應用程式]  選項。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

3. 若要新增新的應用程式，請按一下對話方塊頂端的 [新增應用程式]  按鈕。

    ![新增應用程式按鈕](common/add-new-app.png)

4. 在搜尋方塊中，輸入 **Coupa**，從結果面板中選取 [Coupa]  ，然後按一下 [新增]  按鈕以新增應用程式。

    ![結果清單中的 Coupa](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>設定和測試 Azure AD 單一登入

在本節中，您會以名為 **Britta Simon** 的測試使用者為基礎，設定及測試與 Coupa 搭配運作的 Azure AD 單一登入。
若要讓單一登入能夠運作，必須建立 Azure AD 使用者與 Coupa 中相關使用者之間的連結關聯性。

若要設定及測試與 Coupa 搭配運作的 Azure AD 單一登入，您需要完成下列構成要素：

1. **[設定 Azure AD 單一登入](#configure-azure-ad-single-sign-on)** - 讓您的使用者能夠使用此功能。
2. **[設定 Coupa 單一登入](#configure-coupa-single-sign-on)** - 在應用程式端設定單一登入設定。
3. **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 Britta Simon 測試 Azure AD 單一登入。
4. **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 Britta Simon 能夠使用 Azure AD 單一登入。
5. **[建立 Coupa 測試使用者](#create-coupa-test-user)** - 在 Coupa 中建立一個與 Azure AD 中代表 Britta Simon 之項目連結的對應項目。
6. **[測試單一登入](#test-single-sign-on)** ，驗證組態是否能運作。

### <a name="configure-azure-ad-single-sign-on"></a>設定 Azure AD 單一登入

在本節中，您會在 Azure 入口網站中啟用 Azure AD 單一登入。

若要設定與 Coupa 搭配運作的 Azure AD 單一登入，請執行下列步驟：

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [Coupa]  應用程式整合頁面上，選取 [單一登入]  。

    ![設定單一登入連結](common/select-sso.png)

2. 在 [選取單一登入方法]  對話方塊中，選取 [SAML/WS-Fed]  模式以啟用單一登入。

    ![單一登入選取模式](common/select-saml-option.png)

3. 在 [以 SAML 設定單一登入] 頁面上，按一下 [編輯] 圖示以開啟 [基本 SAML 設定] 對話方塊。   

    ![編輯基本 SAML 組態](common/edit-urls.png)

4. 在 [基本 SAML 組態]  區段上，執行下列步驟：

    ![Coupa 網域及 URL 單一登入資訊](common/sp-identifier-reply.png)

    a. 在 [登入 URL]  文字方塊中，以下列模式輸入 URL︰`https://<companyname>.coupahost.com`

    > [!NOTE]
    > [登入 URL] 的值不是真正的值。 請使用實際的登入 URL 來更新此值。 請連絡 [Coupa 用戶端支援小組](https://success.coupa.com/Support/Contact_Us?) 以取得這些值。

    b. 在 [識別碼]  方塊中，輸入 URL：

    | 環境  | URL |
    |:-------------|----|
    | 沙箱 | `sso-stg1.coupahost.com`|
    | Production | `sso-prd1.coupahost.com`|
    | | |

    c. 在 [回覆 URL]  文字方塊中，鍵入 URL：

    | 環境 | URL |
    |------------- |----|
    | 沙箱 | `https://sso-stg1.coupahost.com/sp/ACS.saml2`|
    | Production | `https://sso-prd1.coupahost.com/sp/ACS.saml2`|
    | | |

4. 在 [以 SAML 設定單一登入]  頁面的 [SAML 簽署憑證]  區段中按一下 [下載]  ，以依據您的需求從指定選項下載 **同盟中繼資料 XML**，並儲存在您的電腦上。

    ![憑證下載連結](common/metadataxml.png)

6. 在 [設定 Coupa]  區段上，依據您的需求複製適當的 URL。

    ![複製組態 URL](common/copy-configuration-urls.png)

    a. 登入 URL

    b. Azure AD 識別碼

    c. 登出 URL

### <a name="configure-coupa-single-sign-on"></a>設定 Coupa 單一登入

1. 以系統管理員身分登入您的 Coupa 公司網站。

2. 移至 [設定] \> [安全性控制]。

    ![安全性控制](./media/coupa-tutorial/ic791900.png "安全性控制")

3. 在 [使用 Coupa 認證登入]  區段中，執行下列步驟：

    ![Coupa 預存程序中繼資料](./media/coupa-tutorial/ic791901.png "Coupa 預存程序中繼資料")

    a. 選取 [使用 SAML 登入]  。

    b. 按一下 [Browse] \(瀏覽\)  以上傳您從 Azure 入口網站下載的中繼資料。

    c. 按一下 [檔案]  。

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

在本節中，您會將 Coupa 的存取權授與 Britta Simon，讓她能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，依序選取 [企業應用程式]  、[所有應用程式]  及 [Coupa]  。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

2. 在應用程式清單中，選取 [Coupa]  。

    ![應用程式清單中的 [Coupa] 連結](common/all-applications.png)

3. 在左側功能表中，選取 [使用者和群組]  。

    ![[使用者和群組] 連結](common/users-groups-blade.png)

4. 按一下 [新增使用者] 按鈕，然後在 [新增指派] 對話方塊中，選取 [使用者和群組]。

    ![[新增指派] 窗格](common/add-assign-user.png)

5. 在 [使用者和群組]  對話方塊的 [使用者] 清單中，選取 [Britta Simon]  ，然後按一下畫面底部的 [選取]  按鈕。

6. 如果您預期使用 SAML 判斷提示中的任何角色值，請在 [選取角色]  對話方塊的清單中選取適當使用者角色，然後按一下畫面底部的 [選取]  按鈕。

7. 在 [新增指派]  對話方塊中，按一下 [指派]  按鈕。

### <a name="create-coupa-test-user"></a>建立 Coupa 測試使用者

若要讓 Azure AD 使用者可以登入 Coupa，則必須將他們佈建到 Coupa。  

* Coupa 需以手動的方式佈建。

**若要設定使用者佈建，請執行下列步驟：**

1. 以系統管理員身分登入您的 **Coupa** 公司網站。

2. 在頂端的功能表中，按一下 [設定]  ，然後按一下 [使用者]  。

    ![使用者](./media/coupa-tutorial/ic791908.png "使用者")

3. 按一下頁面底部的 [新增]  。

    ![建立使用者](./media/coupa-tutorial/ic791909.png "建立使用者")

4. 在 [使用者建立]  區段中，執行下列步驟：

    ![使用者詳細資料](./media/coupa-tutorial/ic791910.png "使用者詳細資料")

    a. 在相關的文字方塊中，輸入您要佈建之有效 Azure Active Directory 帳戶的 [登入]  、[名字]  、[姓氏]  、[單一登入識別碼]  、[電子郵件]  屬性。

    b. 按一下頁面底部的 [新增]  。

    >[!NOTE]
    >Azure Active Directory 帳戶的持有者會收到一封包含連結的電子郵件，以在啟用帳戶前進行確認。
    >

>[!NOTE]
>您可以使用任何其他的 Coupa 使用者帳戶建立工具或 Coupa 提供的 API，來佈建 Azure AD 使用者帳戶。

### <a name="test-single-sign-on"></a>測試單一登入 

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您在存取面板中按一下 [Coupa] 圖格時，應該會自動登入您設定 SSO 的 Coupa。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](../user-help/my-apps-portal-end-user-access.md)。

## <a name="additional-resources"></a>其他資源

- [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](./tutorial-list.md)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](../manage-apps/what-is-single-sign-on.md)

- [什麼是 Azure Active Directory 中的條件式存取？](../conditional-access/overview.md)