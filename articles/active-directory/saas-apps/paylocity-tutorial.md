---
title: 教學課程：Azure Active Directory 單一登入 (SSO) 與 Paylocity 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 Paylocity 之間的單一登入。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/21/2020
ms.author: jeedes
ms.openlocfilehash: 59c01d5d8589b61ff0aaacb81d12fed8fba4f842
ms.sourcegitcommit: 2ba6303e1ac24287762caea9cd1603848331dd7a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/15/2020
ms.locfileid: "97505506"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-paylocity"></a>教學課程：Azure Active Directory 單一登入 (SSO) 與 Paylocity 整合

在本教學課程中，您會了解如何整合 Paylocity 與 Azure Active Directory (Azure AD)。 在整合 Paylocity 與 Azure AD 時，您可以︰

* 在 Azure AD 中控制可存取 Paylocity 的人員。
* 讓使用者使用其 Azure AD 帳戶自動登入 Paylocity。
* 在 Azure 入口網站集中管理您的帳戶。

若要深入了解 SaaS 應用程式與 Azure AD 整合，請參閱[什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>Prerequisites

若要開始，您需要下列項目：

* Azure AD 訂用帳戶。 如果沒有訂用帳戶，您可以取得[免費帳戶](https://azure.microsoft.com/free/)。
* 已啟用 Paylocity 單一登入 (SSO) 的訂用帳戶。

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD SSO。

* Paylocity 支援由 **SP 和 IDP** 起始的 SSO

* SNAT 耗盡的根本原因通常是一種反模式，說明如何建立、管理或設定計時器從其預設值變更。 工作階段控制項會從條件式存取延伸。 [了解如何使用 Microsoft Cloud App Security 來強制執行工作階段控制項](/cloud-app-security/proxy-deployment-aad)。

## <a name="adding-paylocity-from-the-gallery"></a>從資源庫新增 Paylocity

若要設定將 Paylocity 整合到 Azure AD 中，您需要從資源庫將 Paylocity 新增到受控 SaaS 應用程式清單。

1. 使用公司或學校帳戶或個人的 Microsoft 帳戶登入 [Azure 入口網站](https://portal.azure.com)。
1. 在左方瀏覽窗格上，選取 [Azure Active Directory]  服務。
1. 巡覽至 [企業應用程式]  ，然後選取 [所有應用程式]  。
1. 若要新增應用程式，請選取 [新增應用程式]  。
1. 在 [從資源庫新增]  區段的搜尋方塊中輸入 **Paylocity**。
1. 從結果面板選取 [Paylocity]  ，然後新增應用程式。 當應用程式新增至您的租用戶時，請等候幾秒鐘。

## <a name="configure-and-test-azure-ad-single-sign-on-for-paylocity"></a>設定及測試 Paylocity 的 Azure AD 單一登入

以名為 **B.Simon** 的測試使用者，設定及測試與 Paylocity 搭配運作的 Azure AD SSO。 若要讓 SSO 能夠運作，您必須建立 Azure AD 使用者與 Paylocity 中相關使用者之間的連結關聯性。

若要設定及測試與 Paylocity 搭配運作的 Azure AD SSO，請完成下列建置組塊：

1. **[設定 Azure AD SSO](#configure-azure-ad-sso)** - 讓您的使用者能夠使用此功能。
    * **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 B.Simon 測試 Azure AD 單一登入。
    * **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 B.Simon 能夠使用 Azure AD 單一登入。
1. **[設定 Paylocity SSO](#configure-paylocity-sso)** - 在應用程式端設定單一登入設定。
    * **[建立 Paylocity 測試使用者](#create-paylocity-test-user)** - 使 Paylocity 中具有與 Azure AD 中的 B.Simon 連結的相對應使用者。
1. **[測試 SSO](#test-sso)** - 驗證組態是否能運作。

## <a name="configure-azure-ad-sso"></a>設定 Azure AD SSO

依照下列步驟在 Azure 入口網站中啟用 Azure AD SSO。

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [Paylocity]  應用程式整合頁面上，尋找 [管理]  區段並選取 [單一登入]  。
1. 在 [**選取單一登入方法**] 頁面上，選取 [**SAML**]。
1. 在 [以 SAML 設定單一登入]  頁面上，按一下 [基本 SAML 設定]  的編輯/畫筆圖示，以編輯設定。

   ![編輯基本 SAML 組態](common/edit-urls.png)

1. 在 [基本 SAML 組態]  區段中，使用者不需要執行任何步驟，因為應用程式已預先與 Azure 整合。

1. 如果您想要以 **SP** 起始模式設定應用程式，請按一下 [設定其他 URL]  ，然後執行下列步驟：

    在 [登入 URL]  文字方塊中，輸入 URL：`https://access.paylocity.com/`

1. 按一下 [檔案]  。

1. Paylocity 應用程式需要特定格式的 SAML 判斷提示，需要您加入自訂屬性對應到您的 SAML 權杖屬性設定。 以下螢幕擷取畫面顯示預設屬性清單。

    ![image](common/default-attributes.png)

1. 除了上述屬性外，Paylocity 應用程式還需要在 SAML 回應中多傳回幾個屬性，如下所示。 這些屬性也會預先填入，但您必須以實際值來更新這些屬性。

    | 名稱 |  來源屬性|
    | ---------------| --------------- |
    | PartnerID | `P8000010` |
    | PaylocityUser | `user.mail`|
    | PaylocityEntity | < `PaylocityEntity` > |

    > [!NOTE]
    > PaylocityEntity 是 Paylocity Company 識別碼。

1. 在 [以 SAML 設定單一登入]  頁面上的 [SAML 簽署憑證]  區段中，尋找 [同盟中繼資料 XML]  ，然後選取 [下載]  ，以下載憑證並將其儲存在電腦上。

    ![憑證下載連結](common/metadataxml.png)

1. 在 [以 SAML 設定單一登入]  頁面上的 [SAML 簽署憑證]  區段中，按一下 **編輯圖示**。

    ![顯示 [S A M L 簽署憑證] 的螢幕擷取畫面，其中已針對 [同盟中繼資料 X M L] 選取 [下載] 動作。](./media/paylocity-tutorial/edit-samlassertion.png)

1. 針對 [簽署選項]  選取 [簽署 SAML 回應及判斷提示]  ，然後按一下 [儲存]  。

    ![SAML 簽署憑證編輯](./media/paylocity-tutorial/saml-assertion.png)

1. 在 [設定 Paylocity]  區段上，依據您的需求複製適當的 URL。

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

在本節中，您會將 Paylocity 的存取權授與 B.Simon，讓她能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，選取 [企業應用程式]  ，然後選取 [所有應用程式]  。
1. 在應用程式清單中，選取 [Paylocity]  。
1. 在應用程式的概觀頁面中尋找 [管理]  區段，然後選取 [使用者和群組]  。

   ![[使用者和群組] 連結](common/users-groups-blade.png)

1. 選取 [新增使用者]  ，然後在 [新增指派]  對話方塊中選取 [使用者和群組]  。

    ![[新增使用者] 連結](common/add-assign-user.png)

1. 在 [使用者和群組]  對話方塊的 [使用者] 清單中選取 [B.Simon]  ，然後按一下畫面底部的 [選取]  按鈕。
1. 如果您在 SAML 判斷提示中需要任何角色值，請在 [選取角色]  對話方塊的清單中為使用者選取適當的角色，然後按一下畫面底部的 [選取]  按鈕。
1. 在 [新增指派]  對話方塊中，按一下 [指派]  按鈕。

## <a name="configure-paylocity-sso"></a>設定 Paylocity SSO

若要在 **Paylocity** 端設定單一登入，

1. 請下載 **同盟中繼資料 XML**。
1. 在 Paylocity 中，瀏覽至 **人力資源與薪資** > **使用者存取** > **SSO 組態**。
1. 選取 **SSO 整合** 底下的 [新增 SSO 整合]。 隨即開啟新的選單。
1. 從下拉式清單中選取 **Microsoft Azure** 作為 SSO 提供者。
1. 從下拉式清單中選取 **狀態**。
1. 將中繼資料檔案拖放到放置區中。 Paylocity 會嘗試剖析簽發者、張貼重新導向並繫結 URL 和安全性憑證。
1. 選取 [儲存] 以確認變更。 整合應該會顯示在 **SSO 整合** 之下。

### <a name="create-paylocity-test-user"></a>建立 Paylocity 測試使用者

在本節中，您要在 Paylocity 中建立名為 B.Simon 的使用者。 請與 [Paylocity 支援小組](mailto:service@paylocity.com)合作，在 Paylocity 平台中加入使用者。 您必須先建立和啟動使用者，然後才能使用單一登入。

## <a name="test-sso"></a>測試 SSO

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您在存取面板中按一下 [Paylocity] 圖格時，應該會自動登入您已設定 SSO 的 Paylocity。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](../user-help/my-apps-portal-end-user-access.md)。

## <a name="additional-resources"></a>其他資源

- [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](./tutorial-list.md)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](../manage-apps/what-is-single-sign-on.md)

- [什麼是 Azure Active Directory 中的條件式存取？](../conditional-access/overview.md)

- [嘗試搭配 Azure AD 使用 Paylocity](https://aad.portal.azure.com/)

* [什麼是 Microsoft Cloud App Security 中的工作階段控制項？](/cloud-app-security/proxy-intro-aad)

* [如何使用進階可見性和控制項保護 Paylocity](/cloud-app-security/proxy-intro-aad)
