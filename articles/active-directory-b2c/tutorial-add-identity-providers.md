---
title: 教學課程：將識別提供者新增到應用程式
titleSuffix: Azure AD B2C
description: 請遵循本教學課程以了解如何使用 Azure 入口網站在 Azure Active Directory B2C 中將識別提供者新增至您的應用程式。
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: tutorial
ms.date: 07/30/2020
ms.author: mimart
ms.subservice: B2C
ms.custom: fasttrack-edit
ms.openlocfilehash: 166bdb7a2cf15a84e1b826a9a798042c568bb227
ms.sourcegitcommit: 4c89d9ea4b834d1963c4818a965eaaaa288194eb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/04/2020
ms.locfileid: "96608226"
---
# <a name="tutorial-add-identity-providers-to-your-applications-in-azure-active-directory-b2c"></a>教學課程：在 Azure Active Directory B2C 中將識別提供者新增至您的應用程式

在您的應用程式中，您可以讓使用者使用不同的識別提供者登入。 *識別提供者* 可建立、維護及管理身分識別資訊，同時對應用程式提供驗證服務。 您可以使用 Azure 入口網站，將 Azure Active Directory B2C (Azure AD B2C) 支援的識別提供者新增至您的[使用者流程](user-flow-overview.md)。

在本文中，您將學會如何：

> [!div class="checklist"]
> * 建立識別提供者應用程式
> * 將身分識別提供者新增至您的租用戶 - 在 Facebook 和 Azure Active Directory 中
> * 將識別提供者新增至您的使用者流程

您在應用程式中通常只會使用一個識別提供者，但您可以選擇新增更多個。 本教學課程說明如何將 Azure AD 識別提供者和 Facebook 識別提供者新增至您的應用程式。 將這兩個這些識別提供者新增至應用程式是選用作業。 您可以也新增其他識別提供者，例如 [Amazon](identity-provider-amazon.md)、[GitHub](identity-provider-github.md)、[Google](identity-provider-google.md)、[LinkedIn](identity-provider-linkedin.md)、[Microsoft](identity-provider-microsoft-account.md) 或 [Twitter](identity-provider-twitter.md)。

如果您沒有 Azure 訂用帳戶，請在開始前建立[免費帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。

## <a name="prerequisites"></a>必要條件

[建立使用者流程](tutorial-create-user-flows.md)，讓使用者註冊並登入您的應用程式。

## <a name="create-applications"></a>建立應用程式

識別提供者應用程式會提供識別碼和金鑰，以啟用與 Azure AD B2C 租用戶之間的通訊。 在本節的教學課程中，您將建立 Azure AD 應用程式和 Facebook 應用程式，以從中取得識別碼和金鑰，將識別提供者新增至您的租用戶。 如果您只要新增其中一個識別提供者，則只需建立該提供者的應用程式。

### <a name="create-an-azure-active-directory-application"></a>建立 Azure Active Directory 應用程式

若要讓使用者可從 Azure AD 登入，您必須在 Azure AD 租用戶內註冊應用程式。 此 Azure AD 租用戶與您的 Azure AD B2C 租用戶不同。

1. 登入 [Azure 入口網站](https://portal.azure.com)。
1. 選取頂端功能表中的 [目錄 + 訂用帳戶] 篩選，然後選擇包含您 Azure AD 租用戶的目錄，以確定您使用的是包含 Azure AD 租用戶的目錄。
1. 選擇 Azure 入口網站左上角的 [所有服務]，然後搜尋並選取 [應用程式註冊]。
1. 選取 [新增註冊]。
1. 輸入應用程式的名稱。 例如： `Azure AD B2C App` 。
1. 在此應用程式中，接受 [此組織目錄中的帳戶] 選項。
1. 針對 [重新導向 URL]，接受 **Web** 值並以全小寫字母輸入下列 URL，並以您 Azure AD B2C 租用戶的名稱取代 `your-B2C-tenant-name`。

    ```
    https://your-B2C-tenant-name.b2clogin.com/your-B2C-tenant-name.onmicrosoft.com/oauth2/authresp
    ```

    例如： `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp` 。

    所有 URL 現在都應會使用 [b2clogin.com](b2clogin.md)。

1. 選取 [註冊]，然後記錄您在稍後步驟中使用的 **應用程式 (用戶端) 識別碼**。
1. 在 [應用程式] 功能表的 [管理] 底下，選取 [憑證和祕密]，然後選取 [新的用戶端密碼]。
1. 輸入用戶端密碼的 **描述**。 例如： `Azure AD B2C App Secret` 。
1. 選取到期期間。 針對此應用程式，請接受 **1 年內** 的選項。
1. 選取 [新增]，然後記錄您在稍後步驟中使用的新用戶端密碼的值。

### <a name="create-a-facebook-application"></a>建立 Facebook 應用程式

若要在 Azure AD B2C 中使用 Facebook 帳戶作為識別提供者，您必須在 Facebook 建立應用程式。 如果您還沒有 Facebook 帳戶，可以至 [https://www.facebook.com/](https://www.facebook.com/) 取得。

1. 請以您的 Facebook 帳戶認證登入 [Facebook for developers (開發人員專用的 Facebook)](https://developers.facebook.com/)。
1. 如果您尚未這麼做，您必須註冊為 Facebook 開發人員。 若要這樣做，請選取頁面右上角的 [開始]，接受 Facebook 的原則，然後完成註冊步驟。
1. 選取 [我的應用程式]，然後選取 [建立應用程式]。
1. 輸入 **顯示名稱** 和有效的 **連絡人電子郵件**。
1. 按一下 [建立應用程式識別碼]。 這可能會要求您接受 Facebook 平台原則，並完成線上安全性檢查。
1. 選取 [設定] > [基本]。
1. 選擇 **分類**，例如 `Business and Pages`。 此為 Facebook 所需的值，但不會用於 Azure AD B2C。
1. 在頁面底部選取 [新增平台]，然後選取 [網站]。
1. 在 [Site URL] \(網站 URL\) 中，輸入 `https://your-tenant-name.b2clogin.com/`，並將 `your-tenant-name` 取代為您的租用戶名稱。
1. 輸入 **隱私權原則 URL** 的 URL，如 `http://www.contoso.com/`。 隱私權原則 URL 是您需維護以提供應用程式隱私權資訊的網頁。
1. 選取 [Save Changes] \(儲存變更\)。
1. 記錄頁面頂端的 [應用程式識別碼] 值。
1. 在 [應用程式祕密] 旁，選取 [顯示] 並記錄其值。 您必須同時使用應用程式識別碼和應用程式祕密這兩個值，將 Facebook 設定為租用戶中的身分識別提供者。 **應用程式祕密** 是重要的安全性認證，您應該安全地加以儲存。
1. 選取 [產品] 旁的加號，然後選取 **Facebook 登入** 下的 [設定]。
1. 在左側功能表中的 **Facebook 登入** 下，選取 [設定]。
1. 在 [Valid OAuth redirect URIs] \(有效的 OAuth 重新導向 URI\) 中，輸入 `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp`。 以您的租用戶名稱取代 `your-tenant-name`。 選取頁面底部的 [儲存變更]。
1. 若要讓 Azure AD B2C 能使用您的 Facebook 應用程式，請按一下頁面右上方的 [狀態選取器]，然後將其切換至 [開啟]，以將應用程式設為公用，然後按一下 [確認]。 此時，狀態應該會從 [開發] 變更為 [作用中]。

## <a name="add-the-identity-providers"></a>新增識別提供者

在您為要新增的識別提供者建立應用程式之後，請將識別提供者新增至您的租用戶。

### <a name="add-the-azure-active-directory-identity-provider"></a>新增 Azure Active Directory 識別提供者

1. 確定您使用的目錄包含 Azure AD B2C 租用戶。 在頂端功能表中選取 [目錄 + 訂閱] 篩選，並選擇包含您 Azure AD B2C 租用戶的目錄。
1. 選擇 Azure 入口網站左上角的 [所有服務]，然後搜尋並選取 [Azure AD B2C]。
1. 選取 [識別提供者]，然後選取 [新增 OpenID Connect 提供者]。
1. 輸入 [名稱]。 例如，輸入 *Contoso Azure AD*。
1. 針對 [中繼資料 URL] 輸入以下 URL，並將 `{tenant}` 取代為您的 Azure AD 租用戶網域名稱：

    ```
    https://login.microsoftonline.com/{tenant}/v2.0/.well-known/openid-configuration
    ```

    例如： `https://login.microsoftonline.com/contoso.onmicrosoft.com/v2.0/.well-known/openid-configuration` 。
    例如： `https://login.microsoftonline.com/contoso.com/v2.0/.well-known/openid-configuration` 。

1. 在 [用戶端識別碼]中，輸入您先前記錄的應用程式識別碼。
1. 在 [用戶端密碼] 中，輸入您先前記錄的用戶端密碼。
1. 在 [範圍] 中，輸入 `openid profile`。
1. 保留 **回應類型** 和 **回應模式** 的預設值。
1. (選擇性) 在 [網域提示] 中，輸入 `contoso.com`。 如需詳細資訊，請參閱[使用 Azure Active Directory B2C 設定直接登入](direct-signin.md#redirect-sign-in-to-a-social-provider)。
1. 在 [識別提供者宣告對應] 底下，選取下列宣告：

    * **使用者識別碼**：oid
    * **顯示名稱**：name
    * **指定的名稱**：given_name
    * **姓氏**：family_name
    * **電子郵件**：unique_name

1. 選取 [儲存]。

### <a name="add-the-facebook-identity-provider"></a>新增 Facebook 識別提供者

1. 選取 [識別提供者]，然後選取 [Facebook]。
1. 輸入 [名稱]。 例如，Facebook。
1. 在 [用戶端識別碼] 中，輸入稍早所建立 Facebook 應用程式的應用程式識別碼。
1. 在 [用戶端密碼] 中，輸入您記下的應用程式祕密。
1. 選取 [儲存]。

## <a name="update-the-user-flow"></a>更新使用者流程

在教學課程中，您已完成必要條件的部分，建立了名為 *B2C_1_signupsignin1* 的註冊和登入使用者流程。 在本節中，您會將識別提供者新增至 *B2C_1_signupsignin1* 使用者流程。

1. 選取 [使用者流程]，然後選取 *B2C_1_signupsignin1* 使用者流程。
2. 選取 [識別提供者]，然後選取您新增的 **Facebook** 和 **Contoso Azure AD** 識別提供者。
3. 選取 [儲存]。

## <a name="test-the-user-flow"></a>測試使用者流程

1. 在您所建立使用者流程的 [概觀] 頁面上，選取 [執行使用者流程]。
1. 針對 [應用程式]，選取您先前註冊名為 *webapp1* 的 Web 應用程式。 **Reply URL** 應顯示 `https://jwt.ms`。
1. 選取 [執行使用者流程]，然後使用您先前新增的識別提供者登入。
1. 針對您新增的其他識別提供者重複步驟 1 到 3。

如果登入作業成功，系統會將您重新導向至 `https://jwt.ms`，其中會顯示已解碼的權杖，如下所示：

```json
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "<key-ID>"
}.{
  "exp": 1562346892,
  "nbf": 1562343292,
  "ver": "1.0",
  "iss": "https://your-b2c-tenant.b2clogin.com/10000000-0000-0000-0000-000000000000/v2.0/",
  "sub": "20000000-0000-0000-0000-000000000000",
  "aud": "30000000-0000-0000-0000-000000000000",
  "nonce": "defaultNonce",
  "iat": 1562343292,
  "auth_time": 1562343292,
  "name": "User Name",
  "idp": "facebook.com",
  "postalCode": "12345",
  "tfp": "B2C_1_signupsignin1"
}.[Signature]
```

## <a name="next-steps"></a>後續步驟

在本文中，您已了解如何：

> [!div class="checklist"]
> * 建立識別提供者應用程式
> * 將識別提供者新增至您的租用戶
> * 將識別提供者新增至您的使用者流程

接下來，了解如何在您的應用程式中，自訂向使用者顯示的頁面 UI，作為其身分識別體驗的一部分：

> [!div class="nextstepaction"]
> [在 Azure Active Directory B2C 中自訂應用程式的使用者介面](tutorial-customize-ui.md)
