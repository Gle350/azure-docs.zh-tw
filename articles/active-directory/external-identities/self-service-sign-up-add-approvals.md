---
title: 將自訂核准新增至自助式註冊流程-Azure AD
description: '將自訂核准工作流程的 API 連接器新增至外部身分識別自助式註冊-Azure Active Directory (Azure AD) '
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: article
ms.date: 06/16/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3165bc28e6d6283bf8578d9c10b11f7b19981002
ms.sourcegitcommit: dfc4e6b57b2cb87dbcce5562945678e76d3ac7b6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2020
ms.locfileid: "97355234"
---
# <a name="add-a-custom-approval-workflow-to-self-service-sign-up"></a>將自訂核准工作流程新增至自助式註冊

使用 [API 連接器](api-connectors-overview.md)，您可以透過自助式註冊與您自己的自訂核准工作流程整合，以便管理您的租使用者中所建立的來賓使用者帳戶。

本文提供如何與核准系統整合的範例。 在此範例中，自助註冊使用者流程會在註冊程式期間收集使用者資料，並將其傳遞至您的核准系統。 然後，核准系統可以：

- 自動核准使用者，並允許 Azure AD 建立使用者帳戶。
- 觸發手動審核。 如果要求已核准，核准系統會使用 Microsoft Graph 來布建使用者帳戶。 核准系統也可以通知使用者他們的帳戶已建立。

> [!IMPORTANT]
>自 **2021 年1月4日起**，Google 將 [淘汰 web 服務登入支援](https://developers.googleblog.com/2020/08/guidance-for-our-effort-to-block-less-secure-browser-and-apps.html)。 如果您要使用 Google 同盟或使用 Gmail 的自助式註冊，您應該 [測試企業營運原生應用程式的相容性](google-federation.md#deprecation-of-webview-sign-in-support)。

## <a name="register-an-application-for-your-approval-system"></a>為您的核准系統註冊應用程式

您必須將您的核准系統註冊為 Azure AD 租使用者中的應用程式，讓它可以使用 Azure AD 進行驗證，並且擁有建立使用者的許可權。 深入瞭解 [Microsoft Graph 的驗證和授權基本概念](/graph/auth/auth-concepts)。

1. 以 Azure AD 系統管理員身分登入 [Azure 入口網站](https://portal.azure.com)。
2. 在 [Azure 服務] 底下，選取 [Azure Active Directory]。
3. 在左側功能表中，選取 [ **應用程式註冊**]，然後選取 [ **新增註冊**]。
4. 輸入應用程式的 **名稱** ，例如 _註冊核准_。

   <!-- ![Register an application for the approval system](./self-service-sign-up-add-approvals/approvals/register-an-approvals-application.png) -->

5. 選取 [註冊]。 您可以將其他欄位保留為預設值。

   ![醒目顯示 [註冊] 按鈕的螢幕擷取畫面。](media/self-service-sign-up-add-approvals/register-approvals-app.png)

6. 在左側功能表的 [ **管理** ] 底下，選取 [ **API 許可權**]，然後選取 [ **新增許可權**]。
7. 在 [ **要求 API 許可權** ] 頁面上，選取 [ **Microsoft Graph**]，然後選取 [ **應用程式許可權**]。
8. 在 [ **選取許可權**] 下，展開 [ **使用者**]，然後選取 [使用者]。 [ **全部** ] 核取方塊。 此許可權可讓核准系統在核准時建立使用者。 然後選取 [新增權限]。

   ![註冊應用程式的頁面](media/self-service-sign-up-add-approvals/request-api-permissions.png)

9. 在 [ **API 許可權** ] 頁面上，選取 **[授與系統管理員同意，以 (您的租使用者名稱)**]，然後選取 **[是]**。
10. 在左側功能表中的 [ **管理** ] 底下，選取 [ **憑證 & 秘密**]，然後選取 [ **新增用戶端密碼**]。
11. 輸入秘密的 **描述** ，例如 _核准用戶端密碼_，然後選取用戶端密碼 **到期** 的持續時間。 然後選取 [新增]  。
12. 複製用戶端密碼的值。

    ![複製用戶端密碼以用於核准系統](media/self-service-sign-up-add-approvals/client-secret-value-copy.png)

13. 將您的核准系統設定為使用 **應用程式識別碼** 作為用戶端識別碼，並使用您所產生的 **用戶端密碼** 來向 Azure AD 進行驗證。

## <a name="create-the-api-connectors"></a>建立 API 連接器

接下來，您將建立自助註冊使用者流程 [的 API 連接器](self-service-sign-up-add-api-connector.md#create-an-api-connector) 。 您的核准系統 API 需要兩個連接器和對應的端點，如下列範例所示。 這些 API 連接器會執行下列動作：

- **檢查核准狀態**。 在使用者登入身分識別提供者之後，立即傳送對核准系統的呼叫，以檢查使用者是否有現有的核准要求或已遭到拒絕。 如果您的核准系統只執行自動核准決策，則可能不需要此 API 連接器。 「檢查核准狀態」 API 連接器範例。

  ![檢查核准狀態 API 連接器設定](./media/self-service-sign-up-add-approvals/check-approval-status-api-connector-config-alt.png)

- **要求核准** -在使用者完成 [屬性集合] 頁面之後，但在建立使用者帳戶之前，傳送對核准系統的呼叫，以要求核准。 核准要求可以自動授與或手動檢查。 「要求核准」 API 連接器範例。 

  ![要求核准 API 連接器設定](./media/self-service-sign-up-add-approvals/create-approval-request-api-connector-config-alt.png)

若要建立這些連接器，請遵循 [建立 API 連接器](self-service-sign-up-add-api-connector.md#create-an-api-connector)中的步驟。

## <a name="enable-the-api-connectors-in-a-user-flow"></a>在使用者流程中啟用 API 連接器

現在，您會使用下列步驟，將 API 連接器新增至自助註冊使用者流程：

1. 以 Azure AD 系統管理員身分登入 [Azure 入口網站](https://portal.azure.com/)。
2. 在 [Azure 服務] 底下，選取 [Azure Active Directory]。
3. 在左側功能表中，選取 [外部身分識別]。
4. 選取 [ **使用者流程 (預覽])**，然後選取您想要啟用 API 連接器的使用者流程。
5. 選取 [ **api 連接器**]，然後選取您想要在使用者流程的下列步驟中叫用的 api 端點：

   - **使用身分識別提供者登入之後**：選取您的核准狀態 API 連接器，例如 _檢查核准狀態_。
   - **建立使用者之前**：請選取您的核准要求 API 連接器，例如 _要求核准_。

   ![將 Api 新增至使用者流程](./media/self-service-sign-up-add-approvals/api-connectors-user-flow-api.png)

6. 選取 [儲存]。

## <a name="control-the-sign-up-flow-with-api-responses"></a>使用 API 回應控制註冊流程

當您的核准系統呼叫來控制註冊流程時，可以使用其回應。 

### <a name="request-and-responses-for-the-check-approval-status-api-connector"></a>「檢查核准狀態」 API 連接器的要求和回應

API 從「檢查核准狀態」 API 連接器收到的要求範例：

```http
POST <API-endpoint>
Content-type: application/json

{
 "email": "johnsmith@fabrikam.onmicrosoft.com",
 "identities": [ //Sent for Google and Facebook identity providers
     {
     "signInType":"federated",
     "issuer":"facebook.com",
     "issuerAssignedId":"0123456789"
     }
 ],
 "displayName": "John Smith",
 "givenName":"John",
 "lastName":"Smith",
 "ui_locales":"en-US"
}
```

傳送至 API 的確切宣告取決於身分識別提供者所提供的資訊。 一律會傳送「電子郵件」。

#### <a name="continuation-response-for-check-approval-status"></a>「檢查核准狀態」的接續回應

如果有下列情況，則 **檢查核准狀態** API 端點應該會傳回接續回應：

- 使用者先前尚未要求核准。

接續回應的範例：

```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "version": "1.0.0",
    "action": "Continue"
}
```

#### <a name="blocking-response-for-check-approval-status"></a>封鎖「檢查核准狀態」的回應

如果有下列情況，則 **檢查核准狀態** API 端點應該會傳回封鎖回應：

- 使用者核准已暫止。
- 已拒絕使用者，不應允許再次要求核准。

以下是封鎖回應的範例：

```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "version": "1.0.0",
    "action": "ShowBlockPage",
    "userMessage": "Your access request is already processing. You'll be notified when your request has been approved.",
    "code": "CONTOSO-APPROVAL-PENDING"
}
```

```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "version": "1.0.0",
    "action": "ShowBlockPage",
    "userMessage": "Your sign up request has been denied. Please contact an administrator if you believe this is an error",
    "code": "CONTOSO-APPROVAL-DENIED"
}
```

### <a name="request-and-responses-for-the-request-approval-api-connector"></a>「要求核准」 API 連接器的要求和回應

API 從「要求核准」 API 連接器收到的 HTTP 要求範例：

```http
POST <API-endpoint>
Content-type: application/json

{
 "email": "johnsmith@fabrikam.onmicrosoft.com",
 "identities": [ //Sent for Google and Facebook identity providers
     {
     "signInType":"federated",
     "issuer":"facebook.com",
     "issuerAssignedId":"0123456789"
     }
 ],
 "displayName": "John Smith",
 "givenName":"John",
 "surname":"Smith",
 "jobTitle":"Supplier",
 "streetAddress":"1000 Microsoft Way",
 "city":"Seattle",
 "postalCode": "12345",
 "state":"Washington",
 "country":"United States",
 "extension_<extensions-app-id>_CustomAttribute1": "custom attribute value",
 "extension_<extensions-app-id>_CustomAttribute2": "custom attribute value",
 "ui_locales":"en-US"
}
```

傳送至 API 的確切宣告取決於從使用者收集的資訊，或是由身分識別提供者所提供的資訊。

#### <a name="continuation-response-for-request-approval"></a>「要求核准」的接續回應

**要求核准** API 端點應該會在下列情況傳回接續回應：

- 使用者可以 **_自動核准_**。

接續回應的範例：

```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "version": "1.0.0",
    "action": "Continue"
}
```

> [!IMPORTANT]
> 如果收到接續回應，Azure AD 會建立使用者帳戶，並將使用者導向至應用程式。

#### <a name="blocking-response-for-request-approval"></a>封鎖「要求核准」的回應

**要求核准** API 端點應該會在下列情況傳回封鎖回應：

- 已建立使用者核准要求，但現在已暫止。
- 已自動拒絕使用者核准要求。

以下是封鎖回應的範例：

```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "version": "1.0.0",
    "action": "ShowBlockPage",
    "userMessage": "Your account is now waiting for approval. You'll be notified when your request has been approved.",
    "code": "CONTOSO-APPROVAL-REQUESTED"
}
```

```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "version": "1.0.0",
    "action": "ShowBlockPage",
    "userMessage": "Your sign up request has been denied. Please contact an administrator if you believe this is an error",
    "code": "CONTOSO-APPROVAL-AUTO-DENIED"
}
```

`userMessage`回應中的會顯示給使用者，例如：

![待核准頁面範例](./media/self-service-sign-up-add-approvals/approval-pending.png)

## <a name="user-account-creation-after-manual-approval"></a>手動核准之後的使用者帳戶建立

取得手動核准之後，自訂核准系統會使用[Microsoft Graph](/graph/use-the-api)來建立[使用者](/graph/azuread-users-concept-overview)帳戶。 核准系統布建使用者帳戶的方式，取決於使用者所使用的身分識別提供者。

### <a name="for-a-federated-google-or-facebook-user"></a>適用于同盟 Google 或 Facebook 使用者

> [!IMPORTANT]
> 核准系統應該明確檢查 `identities` ， `identities[0]` 而且 `identities[0].issuer` 存在且 `identities[0].issuer` 等於 ' facebook ' 或 ' google '，才能使用此方法。

如果您的使用者已使用 Google 或 Facebook 帳戶登入，您可以使用 [使用者建立 API](/graph/api/user-post-users?tabs=http)。

1. 核准系統使用會接收來自使用者流程的 HTTP 要求。

```http
POST <Approvals-API-endpoint>
Content-type: application/json

{
 "email": "johnsmith@outlook.com",
 "identities": [
     {
     "signInType":"federated",
     "issuer":"facebook.com",
     "issuerAssignedId":"0123456789"
     }
 ],
 "displayName": "John Smith",
 "city": "Redmond",
 "extension_<extensions-app-id>_CustomAttribute": "custom attribute value",
 "ui_locales":"en-US"
}
```

2. 核准系統會使用 Microsoft Graph 來建立使用者帳戶。

```http
POST https://graph.microsoft.com/v1.0/users
Content-type: application/json

{
 "userPrincipalName": "johnsmith_outlook.com#EXT@contoso.onmicrosoft.com",
 "accountEnabled": true,
 "mail": "johnsmith@outlook.com",
 "userType": "Guest",
 "identities": [
     {
     "signInType":"federated",
     "issuer":"facebook.com",
     "issuerAssignedId":"0123456789"
     }
 ],
 "displayName": "John Smith",
 "city": "Redmond",
 "extension_<extensions-app-id>_CustomAttribute": "custom attribute value"
}
```

| 參數                                           | 必要 | 說明                                                                                                                                                            |
| --------------------------------------------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| userPrincipalName                                   | 是      | 可以藉由取得 `email` 傳送至 API 的宣告、將 `@` 字元取代 `_` 為，並將其預先暫止至來產生 `#EXT@<tenant-name>.onmicrosoft.com` 。 |
| accountEnabled                                      | 是      | 必須設為 `true`。                                                                                                                                                 |
| mail                                                | 是      | 相當於 `email` 傳送至 API 的宣告。                                                                                                               |
| userType                                            | 是      | 必須是 `Guest`。 將此使用者指定為來賓使用者。                                                                                                                 |
| 身分識別                                          | 是      | 同盟身分識別資訊。                                                                                                                                    |
| \<otherBuiltInAttribute>                            | 否       | 其他內建屬性，例如 `displayName` 、 `city` 和其他。 參數名稱與 API 連接器所傳送的參數相同。                            |
| \<extension\_\{extensions-app-id}\_CustomAttribute> | 否       | 關於使用者的自訂屬性。 參數名稱與 API 連接器所傳送的參數相同。                                                            |

### <a name="for-a-federated-azure-active-directory-user"></a>同盟 Azure Active Directory 使用者

如果使用者以同盟 Azure Active Directory 帳戶登入，您必須使用 [邀請 api](/graph/api/invitation-post) 來建立使用者，然後選擇性地使用 [使用者更新 api](/graph/api/user-update) ，將更多屬性指派給使用者。

1. 核准系統會從使用者流程接收 HTTP 要求。

```http
POST <Approvals-API-endpoint>
Content-type: application/json

{
 "email": "johnsmith@fabrikam.onmicrosoft.com",
 "displayName": "John Smith",
 "city": "Redmond",
 "extension_<extensions-app-id>_CustomAttribute": "custom attribute value",
 "ui_locales":"en-US"
}
```

2. 核准系統會使用 `email` API 連接器提供的來建立邀請。

```http
POST https://graph.microsoft.com/v1.0/invitations
Content-type: application/json

{
    "invitedUserEmailAddress":"johnsmith@fabrikam.onmicrosoft.com",
    "inviteRedirectUrl" : "https://myapp.com"
}
```

回應的範例：

```http
HTTP/1.1 201 OK
Content-type: application/json

{
    ...
    "invitedUser": {
        "id": "<generated-user-guid>"
    }
}
```

3. 核准系統會使用受邀使用者的識別碼來更新使用者的帳戶，並將使用者屬性 (選擇性) 。

```http
PATCH https://graph.microsoft.com/v1.0/users/<generated-user-guid>
Content-type: application/json

{
    "displayName": "John Smith",
    "city": "Redmond",
    "extension_<extensions-app-id>_AttributeName": "custom attribute value"
}
```

## <a name="next-steps"></a>後續步驟

- 開始使用我們的 [Azure 函數快速入門範例](code-samples-self-service-sign-up.md#api-connector-azure-function-quickstarts)。
- 簽出 [具有手動核准範例之來賓使用者的自助式註冊](code-samples-self-service-sign-up.md#custom-approval-workflows)。
