---
title: 教學課程：Azure Active Directory 與 Central Desktop 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 Central Desktop 之間的單一登入。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/12/2019
ms.author: jeedes
ms.openlocfilehash: 36ba61c86082e191831c2c890de4466181f1a4db
ms.sourcegitcommit: d79513b2589a62c52bddd9c7bd0b4d6498805dbe
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/18/2020
ms.locfileid: "97674177"
---
# <a name="tutorial-azure-active-directory-integration-with-central-desktop"></a>教學課程：Azure Active Directory 與 Central Desktop 整合

在本教學課程中，您將了解如何整合 Central Desktop 與 Azure Active Directory (Azure AD)。
將 Central Desktop 與 Azure AD 整合可提供下列優點：

* 您可以在 Azure AD 中控制可存取 Central Desktop 的人員。
* 您可以讓使用者使用他們的 Azure AD 帳戶自動登入 Central Desktop (單一登入)。
* 您可以在 Azure 入口網站中集中管理您的帳戶。

若您想了解 SaaS app 與 Azure AD 整合的更多詳細資訊，請參閱 [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](../manage-apps/what-is-single-sign-on.md)。
如果您沒有 Azure 訂用帳戶，請在開始之前先[建立免費帳戶](https://azure.microsoft.com/free/)。

## <a name="prerequisites"></a>Prerequisites

若要設定 Azure AD 與 Central Desktop 整合，您需要下列項目：

* Azure AD 訂用帳戶。 如果您沒有 Azure AD 環境，您可以在[這裡](https://azure.microsoft.com/pricing/free-trial/)取得一個月的試用帳戶
* 已啟用 Central Desktop 單一登入的訂用帳戶

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD 單一登入。

* Central Desktop 支援 **SP** 起始的 SSO

## <a name="adding-central-desktop-from-the-gallery"></a>從資源庫新增 Central Desktop

若要設定將 Central Desktop 整合到 Azure AD 中，您需要將 Central Desktop 從資源庫新增到受控 SaaS 應用程式清單。

**若要從資源庫新增 Central Desktop，請執行下列步驟：**

1. 在 **[Azure 入口網站](https://portal.azure.com)** 的左方瀏覽窗格中，按一下 [Azure Active Directory]  圖示。

    ![Azure Active Directory 按鈕](common/select-azuread.png)

2. 瀏覽至 [企業應用程式]  ，然後選取 [所有應用程式]  選項。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

3. 若要新增新的應用程式，請按一下對話方塊頂端的 [新增應用程式]  按鈕。

    ![新增應用程式按鈕](common/add-new-app.png)

4. 在搜尋方塊中，輸入 **Central Desktop**，從結果面板中選取 [Central Desktop]  ，然後按一下 [新增]  按鈕以新增應用程式。

    ![結果清單中的 Central Desktop](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>設定和測試 Azure AD 單一登入

在本節中，您會以名為 **Britta Simon** 的測試使用者為基礎，設定及測試與 Central Desktop 搭配運作的 Azure AD 單一登入。
若要讓單一登入能夠運作，必須建立 Azure AD 使用者與 Central Desktop 中相關使用者之間的連結關聯性。

若要設定及測試與 Central Desktop 搭配運作的 Azure AD 單一登入，您需要完成下列構成要素：

1. **[設定 Azure AD 單一登入](#configure-azure-ad-single-sign-on)** - 讓您的使用者能夠使用此功能。
2. **[設定 Central Desktop 單一登入](#configure-central-desktop-single-sign-on)** - 在應用程式端設定單一登入設定。
3. **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 Britta Simon 測試 Azure AD 單一登入。
4. **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 Britta Simon 能夠使用 Azure AD 單一登入。
5. **[建立 Central Desktop 測試使用者](#create-central-desktop-test-user)** ，以在 Central Desktop 中建立一個與 Azure AD 中代表 Britta Simon 之使用者連結的 Britta Simon 對應項目。
6. **[測試單一登入](#test-single-sign-on)** ，驗證組態是否能運作。

### <a name="configure-azure-ad-single-sign-on"></a>設定 Azure AD 單一登入

在本節中，您會在 Azure 入口網站中啟用 Azure AD 單一登入。

若要設定與 Central Desktop 搭配運作的 Azure AD 單一登入，請執行下列步驟：

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [Central Desktop]  應用程式整合頁面上，選取 [單一登入]  。

    ![設定單一登入連結](common/select-sso.png)

2. 在 [選取單一登入方法]  對話方塊中，選取 [SAML/WS-Fed]  模式以啟用單一登入。

    ![單一登入選取模式](common/select-saml-option.png)

3. 在 [以 SAML 設定單一登入] 頁面上，按一下 [編輯] 圖示以開啟 [基本 SAML 設定] 對話方塊。   

    ![編輯基本 SAML 組態](common/edit-urls.png)

4. 在 [基本 SAML 組態]  區段上，執行下列步驟：

    ![Central Desktop 網域及 URL 單一登入資訊](common/sp-identifier-reply.png)

    a. 在 [登入 URL]  文字方塊中，以下列模式輸入 URL︰`https://<companyname>.centraldesktop.com`

    b. 在 [識別碼]  方塊中，使用下列模式輸入 URL：

    ```http
    https://<companyname>.centraldesktop.com/saml2-metadata.php
    https://<companyname>.imeetcentral.com/saml2-metadata.php
    ```

    c. 在 [回覆 URL]  文字方塊中，使用下列模式來輸入 URL：`https://<companyname>.centraldesktop.com/saml2-assertion.php`

    > [!NOTE]
    > 這些都不是真正的值。 使用實際的「單一登入 URL」、「識別碼」及「回覆 URL」來更新這些值。 請連絡 [Central Desktop 用戶端支援小組](https://imeetcentral.com/contact-us)以取得這些值。 您也可以參考 Azure 入口網站中 **基本 SAML 組態** 區段所示的模式。

5. 在 [以 SAML 設定單一登入]  頁面的 [SAML 簽署憑證]  區段中，按一下 [下載]  ，以依據您的需求從指定選項下載 [憑證 (原始)]  ，並儲存在您的電腦上。

    ![憑證下載連結](common/certificateraw.png)

6. 在 [設定 Central Desktop]  區段上，依據您的需求複製適當的 URL。

    ![複製組態 URL](common/copy-configuration-urls.png)

    a. 登入 URL

    b. Azure AD 識別碼

    c. 登出 URL

### <a name="configure-central-desktop-single-sign-on"></a>設定 Central Desktop 單一登入

1. 登入您的 **Central Desktop** 租用戶。

2. 移至 [設定]  。 選取 [Advanced] \(進階\)  ，然後選取 [Single Sign On] \(單一登入\)  。

    ![設定 - 進階](./media/central-desktop-tutorial/ic769563.png "設定 - 進階")

3. 在 [Single Sign On Settings] \(單一登入設定\)  頁面上，執行下列步驟：

    ![單一登入設定](./media/central-desktop-tutorial/ic769564.png "單一登入設定")

    a. 選取 [啟用 SAML v2 單一登入]  。

    b. 在 [SSO URL]  方塊中，貼上您從 Azure 入口網站複製的 **Azure AD 識別碼** 值。

    c. 在 [SSO 登入 URL]  方塊中，貼上您從 Azure 入口網站複製的 **登入 URL** 值。

    d. 在 [SSO 登出 URL]  方塊中，貼上您從 Azure 入口網站複製的 **登出 URL** 值。

4. 在 [Message Signature Verification Method] \(訊息簽章驗證方法\)  區段中，執行下列步驟：

    ![訊息簽章驗證方法](./media/central-desktop-tutorial/ic769565.png "訊息簽章驗證方法")

    a. 選取 [憑證]  。

    b. 在 [SSO Certificate] \(SSO 憑證\)  清單中，選取 [RSH SHA256]  。

    c. 在「記事本」中開啟所下載的憑證。 接著，複製憑證的內容並將它貼到 [SSO Certificate] \(SSO 憑證\)  欄位中。

    d. 選取 [顯示 SAMLv2 登入頁面的連結]  。

    e. 選取 [更新]  。

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

在本節中，您會將 Central Desktop 的存取權授與 Britta Simon，讓她能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，依序選取 [企業應用程式]  、[所有應用程式]  及 [Central Desktop]  。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

2. 在應用程式清單中，選取 [Central Desktop]  。

    ![應用程式清單中的 Central Desktop 連結](common/all-applications.png)

3. 在左側功能表中，選取 [使用者和群組]  。

    ![[使用者和群組] 連結](common/users-groups-blade.png)

4. 按一下 [新增使用者] 按鈕，然後在 [新增指派] 對話方塊中，選取 [使用者和群組]。

    ![[新增指派] 窗格](common/add-assign-user.png)

5. 在 [使用者和群組]  對話方塊的 [使用者] 清單中，選取 [Britta Simon]  ，然後按一下畫面底部的 [選取]  按鈕。

6. 如果您預期使用 SAML 判斷提示中的任何角色值，請在 [選取角色]  對話方塊的清單中選取適當使用者角色，然後按一下畫面底部的 [選取]  按鈕。

7. 在 [新增指派]  對話方塊中，按一下 [指派]  按鈕。

### <a name="create-central-desktop-test-user"></a>建立 Central Desktop 測試使用者

若要讓 Azure AD 使用者能夠登入，必須先在 Central Desktop 應用程式中佈建他們。 本節說明如何在 Central Desktop 中建立 Azure AD 使用者帳戶。

> [!NOTE]
> 若要佈建 Azure AD 使用者帳戶，您可以使用任何其他 Central Desktop 使用者帳戶建立工具，或是 Central Desktop 所提供的 API。

**將使用者帳戶佈建到 Central Desktop：**

1. 登入您的 Central Desktop 租用戶。

2. 選取 [人員]  ，然後選取 [新增內部成員]  。

    ![人員](./media/central-desktop-tutorial/ic781051.png "人員")

3. 在 [Email Address of New Members] \(新成員的電子郵件地址\)  方塊中，輸入您想要佈建的 Azure AD 帳戶，然後選取 [Next] \(下一步\)  。

    ![新成員的電子郵件地址](./media/central-desktop-tutorial/ic781052.png "新成員的電子郵件地址")

4. 選取 [Add Internal member(s)] \(新增內部成員\)  。

    ![新增內部成員](./media/central-desktop-tutorial/ic781053.png "新增內部成員")

   > [!NOTE]
   > 您新增的使用者會收到一封電子郵件，其中包含用來啟用其帳戶的確認連結。

### <a name="test-single-sign-on"></a>測試單一登入

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您在存取面板中按一下 [Central Desktop] 圖格時，應該會自動登入您設定 SSO 的 Central Desktop。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](../user-help/my-apps-portal-end-user-access.md)。

## <a name="additional-resources"></a>其他資源

- [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](./tutorial-list.md)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](../manage-apps/what-is-single-sign-on.md)

- [什麼是 Azure Active Directory 中的條件式存取？](../conditional-access/overview.md)