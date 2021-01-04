---
title: 包含檔案
description: 包含檔案
services: azure-communication-services
author: tomaschladek
manager: nmurav
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 08/20/2020
ms.topic: include
ms.custom: include file
ms.author: tchladek
ms.openlocfilehash: 245dd9abf93771d5be142367679d622a3908b7d5
ms.sourcegitcommit: 17e9cb8d05edaac9addcd6e0f2c230f71573422c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/21/2020
ms.locfileid: "97717892"
---
## <a name="prerequisites"></a>必要條件

- 具有有效訂用帳戶的 Azure 帳戶。 [免費建立帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。
- [Node.js](https://nodejs.org/) 作用中 LTS 和維修 LTS 版本 (建議使用 8.11.1 和 10.14.1)。
- 作用中的 Azure 通訊服務資源和連接字串。 [建立通訊服務資源](../create-communication-resource.md)。

## <a name="setting-up"></a>設定

### <a name="create-a-new-nodejs-application"></a>建立新的 Node.js 應用程式

開啟您的終端機或命令視窗，為您的應用程式建立新的目錄，並瀏覽至該目錄。

```console
mkdir access-tokens-quickstart && cd access-tokens-quickstart
```

執行 `npm init -y` 以使用預設設定建立 **package.json** 檔案。

```console
npm init -y
```

### <a name="install-the-package"></a>安裝套件

使用 `npm install` 命令來安裝適用於 JavaScript 的 Azure 通訊服務系統管理用戶端程式庫。

```console

npm install @azure/communication-administration --save

```

`--save` 選項會在您的 **package.json** 檔案中，將程式庫列為相依性。

## <a name="set-up-the-app-framework"></a>設定應用程式架構

從專案目錄：

1. 在程式碼編輯器中開啟新的文字檔
1. 新增 `require` 呼叫以載入 `CommunicationIdentityClient`
1. 建立程式的結構，包括基本例外狀況處理

使用下列程式碼開始作業：

```javascript
const { CommunicationIdentityClient } = require('@azure/communication-administration');

const main = async () => {
  console.log("Azure Communication Services - Access Tokens Quickstart")

  // Quickstart code goes here
};

main().catch((error) => {
  console.log("Encountered an error");
  console.log(error);
})
```

1. 在 *access-tokens-quickstart* 目錄中將新檔案儲存為 **issue-access-token.js**。

## <a name="authenticate-the-client"></a>驗證用戶端

使用連接字串具現化 `CommunicationIdentityClient`。 以下程式碼會從名為 `COMMUNICATION_SERVICES_CONNECTION_STRING` 的環境變數中，擷取資源的連接字串。 了解如何[管理資源的連接字串](../create-communication-resource.md#store-your-connection-string)。

將下列程式碼加入 `main` 方法：

```javascript
// This code demonstrates how to fetch your connection string
// from an environment variable.
const connectionString = process.env['COMMUNICATION_SERVICES_CONNECTION_STRING'];

// Instantiate the identity client
const identityClient = new CommunicationIdentityClient(connectionString);
```

## <a name="create-an-identity"></a>建立身分識別

Azure 通訊服務會維護輕量身分識別目錄。 使用 `createUser` 方法，在目錄中建立具有唯一 `Id` 的新項目。 儲存已接收的身分識別，並對應至您應用程式的使用者。 例如，將其儲存在您應用程式伺服器的資料庫中。 後續將需要以身分識別簽發存取權杖。

```javascript
let identityResponse = await identityClient.createUser();
console.log(`\nCreated an identity with ID: ${identityResponse.communicationUserId}`);
```

## <a name="issue-access-tokens"></a>發行存取權杖

使用 `issueToken` 方法來簽發現有通訊服務身分識別的存取權杖。 參數 `scopes` 會定義一組基本類型，進行此存取權杖的授權。 請參閱[支援的動作清單](../../concepts/authentication.md)。 您可以根據 Azure 通訊服務身分識別的字串表示法，建立參數 `communicationUser` 的新執行個體。

```javascript
// Issue an access token with the "voip" scope for an identity
let tokenResponse = await identityClient.issueToken(identityResponse, ["voip"]);
const { token, expiresOn } = tokenResponse;
console.log(`\nIssued an access token with 'voip' scope that expires at ${expiresOn}:`);
console.log(token);
```

存取權杖是需要重新簽發的短期認證。 若未這麼做，可能會導致應用程式的使用者體驗中斷。 `expiresOn` 回應屬性會指出存取權杖的存留期。


## <a name="refresh-access-tokens"></a>重新整理存取權杖

重新整理存取權杖就像使用與簽發權杖相同的身分識別呼叫 `issueToken` 一樣容易。 您也需要提供已重新整理權杖的 `scopes`。 

```javascript
// // Value of identityResponse represents the Azure Communication Services identity stored during identity creation and then used to issue the tokens being refreshed
let refreshedTokenResponse = await identityClient.issueToken(identityResponse, ["voip"]);
```


## <a name="revoke-access-tokens"></a>撤銷存取權杖

在某些情況下，您可能要明確地撤銷存取權杖。 例如，當應用程式的使用者變更用來向您的服務進行驗證的密碼時。 方法 `revokeTokens` 會使簽發給身分識別的所有作用中存取權杖失效。

```javascript  
await identityClient.revokeTokens(identityResponse);
console.log(`\nSuccessfully revoked all access tokens for identity with ID: ${identityResponse.communicationUserId}`);
```

## <a name="delete-an-identity"></a>刪除身分識別

刪除身分識別會撤銷所有作用中的存取權杖，並防止您核發身分識別的存取權杖。 此外也會移除所有與身分識別相關聯的保存內容。

```javascript
await identityClient.deleteUser(identityResponse);
console.log(`\nDeleted the identity with ID: ${identityResponse.communicationUserId}`);
```

## <a name="run-the-code"></a>執行程式碼

從主控台提示中，瀏覽至包含 *issue-access-token.js* 檔案的目錄，然後執行下列 `node` 命令以執行應用程式。

```console
node ./issue-access-token.js
```
