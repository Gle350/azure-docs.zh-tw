---
title: 教學課程：Azure Active Directory 與 SpringCM 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 SpringCM 之間的單一登入。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 04/08/2019
ms.author: jeedes
ms.openlocfilehash: 9cfc48e3fdb96ba5b63b28288a801095f7b36f43
ms.sourcegitcommit: d2d1c90ec5218b93abb80b8f3ed49dcf4327f7f4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/16/2020
ms.locfileid: "97589809"
---
# <a name="tutorial-azure-active-directory-integration-with-springcm"></a>教學課程：Azure Active Directory 與 SpringCM 整合

在本教學課程中，您將了解如何整合 SpringCM 與 Azure Active Directory (Azure AD)。
SpringCM 與 Azure AD 整合提供下列優點：

* 您可以在 Azure AD 中控制可存取 SpringCM 的人員。
* 您可以讓使用者使用其 Azure AD 帳戶自動登入 SpringCM (單一登入)。
* 您可以在 Azure 入口網站中集中管理您的帳戶。

若您想了解 SaaS app 與 Azure AD 整合的更多詳細資訊，請參閱 [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](../manage-apps/what-is-single-sign-on.md)。
如果您沒有 Azure 訂用帳戶，請在開始之前先[建立免費帳戶](https://azure.microsoft.com/free/)。

## <a name="prerequisites"></a>必要條件

若要設定 Azure AD 與 SpringCM 的整合作業，您需要下列項目：

* Azure AD 訂用帳戶。 如果您沒有 Azure AD 環境，您可以申請[免費帳戶](https://azure.microsoft.com/free/)
* 已啟用 SpringCM 單一登入的訂用帳戶

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD 單一登入。

* SpringCM 支援 **SP** 起始的 SSO

## <a name="adding-springcm-from-the-gallery"></a>從資源庫新增 SpringCM

若要設定 SpringCM 與 Azure AD 整合，您需要從資源庫將 SpringCM 加入受控 SaaS 應用程式清單中。

**若要從資源庫新增 SpringCM，請執行下列步驟：**

1. 在 **[Azure 入口網站](https://portal.azure.com)** 的左側導覽面板中，按一下 [Azure Active Directory] 圖示。

    ![Azure Active Directory 按鈕](common/select-azuread.png)

2. 瀏覽至 [企業應用程式]，然後選取 [所有應用程式] 選項。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

3. 若要新增應用程式，請按一下對話方塊頂端的 [新增應用程式] 按鈕。

    ![新增應用程式按鈕](common/add-new-app.png)

4. 在搜尋方塊中，輸入 **SpringCM**，從結果面板中選取 [SpringCM]，然後按一下 [新增] 按鈕以新增應用程式。

    ![結果清單中的 SpringCM](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>設定和測試 Azure AD 單一登入

在本節中，您會以名為 **Britta Simon** 的測試使用者身分，設定及測試與 SpringCM 搭配運作的 Azure AD 單一登入。
若要讓單一登入能夠運作，必須建立 Azure AD 使用者與 SpringCM 中相關使用者之間的連結關聯性。

若要設定及測試與 SpringCM 搭配運作的 Azure AD 單一登入，您需要完成下列建構元素：

1. **[設定 Azure AD 單一登入](#configure-azure-ad-single-sign-on)** - 讓您的使用者能夠使用此功能。
2. **[設定 SpringCM 單一登入](#configure-springcm-single-sign-on)** - 在應用程式端設定單一登入設定。
3. **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 Britta Simon 測試 Azure AD 單一登入。
4. **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 Britta Simon 能夠使用 Azure AD 單一登入。
5. **[建立 SpringCM 測試使用者](#create-springcm-test-user)** - 使 SpringCM 中 Britta Simon 的對應使用者連結到該使用者在 Azure AD 中的代表項目。
6. **[測試單一登入](#test-single-sign-on)** ，驗證組態是否能運作。

### <a name="configure-azure-ad-single-sign-on"></a>設定 Azure AD 單一登入

在本節中，您會在 Azure 入口網站中啟用 Azure AD 單一登入。

若要使用 SpringCM 設定 Azure AD 單一登入功能，請執行下列步驟：

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [SpringCM] 應用程式整合頁面上，選取 [單一登入]。

    ![設定單一登入連結](common/select-sso.png)

2. 在 [選取單一登入方法] 對話方塊中，選取 [SAML/WS-Fed] 模式以啟用單一登入。

    ![單一登入選取模式](common/select-saml-option.png)

3. 在 [以 SAML 設定單一登入] 頁面上，按一下 [編輯] 圖示以開啟 [基本 SAML 設定] 對話方塊。   

    ![編輯基本 SAML 組態](common/edit-urls.png)

4. 在 [基本 SAML 組態] 區段上，執行下列步驟：

    ![SpringCM 網域與 URL 單一登入資訊](common/sp-signonurl.png)

    在 [登入 URL] 文字方塊中，以下列模式輸入 URL︰`https://na11.springcm.com/atlas/SSO/SSOEndpoint.ashx?aid=<identifier>`

    > [!NOTE]
    > 這不是真正的值。 請使用實際的「登入 URL」來更新此值。 請連絡 [SpringCM 用戶端支援小組](https://knowledge.springcm.com/support)以取得此值。 您也可以參考 Azure 入口網站中 **基本 SAML 組態** 區段所示的模式。

4. 在 [以 SAML 設定單一登入] 頁面的 [SAML 簽署憑證] 區段中，按一下 [下載]，以依據您的需求從指定選項下載 [憑證 (原始)]，並儲存在您的電腦上。

    ![憑證下載連結](common/certificateraw.png)

6. 在 [設定 SpringCM] 區段上，依據您的需求複製適當的 URL。

    ![複製組態 URL](common/copy-configuration-urls.png)

    a. 登入 URL

    b. Azure AD 識別碼

    c. 登出 URL

### <a name="configure-springcm-single-sign-on"></a>設定 SpringCM 單一登入

1. 在不同的 Web 瀏覽器視窗中，以系統管理員身分登入您的 **SpringCM** 公司網站。

1. 在上方功能表中，依序按一下 [移至]、[喜好設定]，以及 [帳戶喜好設定] 區段中的 [SAML SSO]。

    ![SAML SSO](./media/spring-cm-tutorial/ic797051.png "SAML SSO")

1. 在 [識別提供者組態] 區段執行下列步驟：

    ![識別提供者組態](./media/spring-cm-tutorial/ic797052.png "識別提供者組態")

    a. 若要上傳已下載的 Azure Active Directory 憑證，請按一下 [選取簽發者憑證] 或 [變更簽發者憑證]。

    b. 在 [簽發者] 文字方塊中，貼上您從 Azure 入口網站複製的 **Azure AD 識別碼** 值。

    c. 在 [服務提供者 (SP) 起始端點] 文字方塊中，貼上您從 Azure 入口網站複製的 [登入 URL] 值。

    d. 針對 [已啟用 SAML] 選取 [啟用]。

    e. 按一下 [檔案] 。

### <a name="create-an-azure-ad-test-user"></a>建立 Azure AD 測試使用者 

本節的目標是要在 Azure 入口網站中建立一個名為 Britta Simon 的測試使用者。

1. 在 Azure 入口網站的左窗格中，依序選取 [Azure Active Directory]、[使用者] 和 [所有使用者]。

    ![[使用者和群組] 與 [所有使用者] 連結](common/users.png)

2. 在畫面頂端選取 [新增使用者]。

    ![[新增使用者] 按鈕](common/new-user.png)

3. 在 [使用者] 屬性中，執行下列步驟。

    ![[使用者] 對話方塊](common/user-properties.png)

    a. 在 [名稱] 欄位中，輸入 **BrittaSimon**。

    b. 在 [使用者名稱] 欄位中，輸入 `brittasimon@yourcompanydomain.extension`。 例如， BrittaSimon@contoso.com

    c. 選取 [顯示密碼] 核取方塊，然後記下 [密碼] 方塊中顯示的值。

    d. 按一下 [建立]。

### <a name="assign-the-azure-ad-test-user"></a>指派 Azure AD 測試使用者

在本節中，您會將 SpringCM 的存取權授與 Britta Simon，讓她能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，依序選取 [企業應用程式]、[所有應用程式] 及 [SpringCM]。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

2. 在應用程式清單中，選取 [SpringCM]。

    ![應用程式清單中的 SpringCM 連結](common/all-applications.png)

3. 在左側功能表中，選取 [使用者和群組]。

    ![[使用者和群組] 連結](common/users-groups-blade.png)

4. 按一下 [新增使用者] 按鈕，然後在 [新增指派] 對話方塊中，選取 [使用者和群組]。

    ![[新增指派] 窗格](common/add-assign-user.png)

5. 在 [使用者和群組] 對話方塊的 [使用者] 清單中，選取 [Britta Simon]，然後按一下畫面底部的 [選取] 按鈕。

6. 如果您預期使用 SAML 判斷提示中的任何角色值，請在 [選取角色] 對話方塊的清單中選取適當使用者角色，然後按一下畫面底部的 [選取] 按鈕。

7. 在 [新增指派] 對話方塊中，按一下 [指派] 按鈕。

### <a name="create-springcm-test-user"></a>建立 SpringCM 測試使用者

若要讓 Azure Active Directory 使用者能夠登入 SpringCM，必須將他們佈建到 SpringCM。 SpringCM 需以手動方式佈建。

> [!NOTE]
> 如需詳細資訊，請參閱[建立和編輯 SpringCM 使用者](http://community.springcm.com/s/article/Create-and-Edit-a-SpringCM-User-1619481053) (英文)。 

**若要將使用者帳戶佈建到 SpringCM，請執行下列步驟：**

1. 以系統管理員身分登入您的 **SpringCM** 公司網站。

1. 按一下 [移至]，然後按一下 [通訊錄]。

    ![建立使用者](./media/spring-cm-tutorial/ic797054.png "建立使用者")

1. 按一下 [建立使用者] 。

1. 選取 [使用者角色] 。

1. 選取 [傳送啟用電子郵件] 。

1. 在相關的文字方塊中，輸入您想要佈建之有效 Azure Active Directory 使用者帳戶的名字、姓氏和電子郵件地址。

1. 將該使用者加入 [安全性群組] 。

1. 按一下 [檔案] 。

   > [!NOTE]
   > 您可以使用任何其他的 SpringCM 使用者帳戶建立工具或 SpringCM 提供的 API，來佈建 Azure AD 使用者帳戶。

### <a name="test-single-sign-on"></a>測試單一登入 

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您在存取面板中按一下 [SpringCM] 圖格時，應該會自動登入您設定 SSO 的 SpringCM。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](../user-help/my-apps-portal-end-user-access.md)。

## <a name="additional-resources"></a>其他資源

- [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](./tutorial-list.md)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](../manage-apps/what-is-single-sign-on.md)

- [什麼是 Azure Active Directory 中的條件式存取？](../conditional-access/overview.md)