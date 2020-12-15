---
title: 快速入門 - 適用於 JavaScript (版本 4) 的 Azure Key Vault 祕密用戶端程式庫
description: 了解如何使用 JavaScript 用戶端程式庫從 Azure Key Vault 建立、擷取和刪除秘密
author: msmbaldwin
ms.author: mbaldwin
ms.date: 12/6/2020
ms.service: key-vault
ms.subservice: secrets
ms.topic: quickstart
ms.custom: devx-track-js, devx-track-azurecli
ms.openlocfilehash: 8e04fcea53869fe15ebbeb3c7709cff842893931
ms.sourcegitcommit: 8b4b4e060c109a97d58e8f8df6f5d759f1ef12cf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/07/2020
ms.locfileid: "96780785"
---
# <a name="quickstart-azure-key-vault-secret-client-library-for-javascript-version-4"></a>快速入門：適用於 JavaScript (版本 4) 的 Azure Key Vault 祕密用戶端程式庫

開始使用適用於 JavaScript 的 Azure Key Vault 秘密用戶端程式庫。 [Azure Key Vault](../general/overview.md) 是一項雲端服務，可為祕密提供安全的存放區。 您也可以安全地儲存金鑰、密碼、憑證和其他祕密。 您可以透過 Azure 入口網站建立和管理 Azure 金鑰保存庫。 在本快速入門中，您會了解如何使用 JavaScript 用戶端程式庫，從 Azure Key Vault 建立、擷取和刪除秘密

Key Vault 用戶端程式庫資源：

[API 參考文件](/javascript/api/overview/azure/key-vault-index) | [程式庫原始程式碼](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/keyvault) | [套件 (npm)](https://www.npmjs.com/package/@azure/keyvault-secrets)

如需有關 Key Vault 及秘密的詳細資訊，請參閱：
- [Key Vault 概觀](../general/overview.md)
- [秘密概觀](about-secrets.md)。

## <a name="prerequisites"></a>必要條件

- Azure 訂用帳戶 - [建立免費帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。
- 適用於您作業系統的 [Node.js](https://nodejs.org) 目前版本。
- [Azure CLI](/cli/azure/install-azure-cli)
- Key Vault - 您可以使用 [Azure 入口網站](../general/quick-create-portal.md)、[Azure CLI](../general/quick-create-cli.md) 或 [Azure PowerShell](../general/quick-create-powershell.md) 建立

本快速入門假設您執行 [Azure CLI](/cli/azure/install-azure-cli)。

## <a name="sign-in-to-azure"></a>登入 Azure

1. 執行 `login` 命令。

    ```azurecli-interactive
    az login
    ```

    如果 CLI 可以開啟預設瀏覽器，它會執行這項操作，並載入 Azure 登入頁面。

    否則，請在 [https://aka.ms/devicelogin](https://aka.ms/devicelogin) 中開啟瀏覽器頁面，並輸入顯示在終端機中的授權碼。

2. 請在瀏覽器中使用您的帳戶認證登入。

## <a name="create-new-nodejs-application"></a>建立新的 Node.js 應用程式

接下來，建立可部署至雲端的 Node.js 應用程式。 

1. 在命令殼層中，建立名為 `key-vault-node-app` 的資料夾：

```azurecli
mkdir key-vault-node-app
```

1. 變更為新建立的 key-vault-node-app 目錄，然後執行 'init' 命令將節點專案初始化：

```azurecli
cd key-vault-node-app
npm init -y
```

## <a name="install-key-vault-packages"></a>安裝 Key Vault 套件

從主控台視窗安裝適用於 Node.js 的 Azure Key Vault [祕密程式庫](https://www.npmjs.com/package/@azure/keyvault-secrets)。

```azurecli
npm install @azure/keyvault-secrets
```

安裝 [azure.identity](https://www.npmjs.com/package/@azure/identity) 套件，以向 Key Vault 驗證

```azurecli
npm install @azure/identity
```

## <a name="set-environment-variables"></a>設定環境變數

此應用程式使用金鑰保存庫名稱作為稱為 `KEY_VAULT_NAME` 的環境變數。

Windows
```cmd
set KEY_VAULT_NAME=<your-key-vault-name>
````
Windows PowerShell
```powershell
$Env:KEY_VAULT_NAME=<your-key-vault-name>
```

macOS 或 Linux
```cmd
export KEY_VAULT_NAME=<your-key-vault-name>
```

## <a name="grant-access-to-your-key-vault"></a>授與對金鑰保存庫的存取權

建立金鑰保存庫的存取原則，將祕密權限授與服務主體

```azurecli
az keyvault set-policy --name <YourKeyVaultName> --upn user@domain.com --secret-permissions delete get list set purge
```

## <a name="code-examples"></a>程式碼範例

以下的程式碼範例會示範如何建立用戶端、設定祕密、擷取祕密，以及刪除秘密。 

### <a name="set-up-the-app-framework"></a>設定應用程式架構

1. 建立新的文字檔，並將儲存為 'index.js'

1. 新增必要呼叫以載入 Azure 和 Node.js 模組

1. 建立程式的結構，包括基本例外狀況處理

```javascript
const readline = require('readline');

function askQuestion(query) {
    const rl = readline.createInterface({
        input: process.stdin,
        output: process.stdout,
    });

    return new Promise(resolve => rl.question(query, ans => {
        rl.close();
        resolve(ans);
    }))
}

async function main() {
    
}

main().then(() => console.log('Done')).catch((ex) => console.log(ex.message));
```

### <a name="add-directives"></a>新增指示詞

將下列指示詞新增至程式碼頂端：

```javascript
const { DefaultAzureCredential } = require("@azure/identity");
const { SecretClient } = require("@azure/keyvault-secrets");
```

### <a name="authenticate-and-create-a-client"></a>驗證並建立用戶端

在本快速入門中，已登入的使用者是用來向金鑰保存庫進行驗證，這是本機開發的慣用方法。 針對部署至 Azure 的應用程式，應將受控識別指派給 App Service 或虛擬機器。如需詳細資訊，請參閱[受控識別概觀](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)。

在下列範例中，金鑰保存庫的名稱會以 "https://\<your-key-vault-name\>.vault.azure.net" 格式，擴充至金鑰保存庫 URI。 這個範例會使用來自 [Azure 身分識別程式庫](https://docs.microsoft.com/javascript/api/overview/azure/identity-readme)的 ['DefaultAzureCredential()'](https://docs.microsoft.com/javascript/api/@azure/identity/defaultazurecredential) 類別，允許在各種不同的環境中使用相同的程式碼，搭配不同的選項來提供身分識別。 如需對金鑰保存庫進行驗證的詳細資訊，請參閱[開發人員指南](https://docs.microsoft.com/azure/key-vault/general/developers-guide#authenticate-to-key-vault-in-code)。

將下列程式碼新增至 'main()' 函式

```javascript
const keyVaultName = process.env["KEY_VAULT_NAME"];
const KVUri = "https://" + keyVaultName + ".vault.azure.net";

const credential = new DefaultAzureCredential();
const client = new SecretClient(KVUri, credential);
```

### <a name="save-a-secret"></a>儲存秘密

既然應用程式已經過驗證，您可以使用 [setSecret 方法](/javascript/api/@azure/keyvault-secrets/secretclient?#setsecret-string--string--setsecretoptions-)，將祕密放入金鑰保存庫中。這需要秘密的名稱，我們在此範例中使用 "mySecret"。  

```javascript
await client.setSecret(secretName, secretValue);
```

### <a name="retrieve-a-secret"></a>擷取祕密

您現在可以使用 [getSecret 方法](/javascript/api/@azure/keyvault-secrets/secretclient?#getsecret-string--getsecretoptions-)擷取先前設定的值。

```javascript
const retrievedSecret = await client.getSecret(secretName);
 ```

您的祕密現在已另存為 `retrievedSecret.value`。

### <a name="delete-a-secret"></a>刪除祕密

最後，讓我們使用 [beginDeleteSecret](https://docs.microsoft.com/javascript/api/@azure/keyvault-secrets/secretclient?#beginDeleteSecret_string__BeginDeleteSecretOptions_) 和 [purgeDeletedSecret](https://docs.microsoft.com/javascript/api/@azure/keyvault-secrets/secretclient?#purgeDeletedSecret_string__PurgeDeletedSecretOptions_) 方法，從您的金鑰保存庫中清除祕密。

```javascript
const deletePoller = await client.beginDeleteSecret(secretName);
await deletePoller.pollUntilDone();
await client.purgeDeletedSecret(secretName);
```

## <a name="sample-code"></a>範例程式碼

```javascript
const { DefaultAzureCredential } = require("@azure/identity");
const { SecretClient } = require("@azure/keyvault-secrets");

const readline = require('readline');

function askQuestion(query) {
    const rl = readline.createInterface({
        input: process.stdin,
        output: process.stdout,
    });

    return new Promise(resolve => rl.question(query, ans => {
        rl.close();
        resolve(ans);
    }))
}

async function main() {

  const keyVaultName = process.env["KEY_VAULT_NAME"];
  const KVUri = "https://" + keyVaultName + ".vault.azure.net";

  const credential = new DefaultAzureCredential();
  const client = new SecretClient(KVUri, credential);

  const secretName = "mySecret";
  var secretValue = await askQuestion("Input the value of your secret > ");

  console.log("Creating a secret in " + keyVaultName + " called '" + secretName + "' with the value '" + secretValue + "` ...");
  await client.setSecret(secretName, secretValue);

  console.log("Done.");

  console.log("Forgetting your secret.");
  secretValue = "";
  console.log("Your secret is '" + secretValue + "'.");

  console.log("Retrieving your secret from " + keyVaultName + ".");

  const retrievedSecret = await client.getSecret(secretName);

  console.log("Your secret is '" + retrievedSecret.value + "'.");

  console.log("Deleting your secret from " + keyVaultName + " ...");
  const deletePoller = await client.beginDeleteSecret(secretName);
  await deletePoller.pollUntilDone();
  console.log("Done.");
  
  console.log("Purging your secret from {keyVaultName} ...");
  await client.purgeDeletedSecret(secretName);
  
}

main().then(() => console.log('Done')).catch((ex) => console.log(ex.message));

```

## <a name="test-and-verify"></a>測試和驗證

1. 執行下列命令來執行應用程式。

    ```azurecli
    npm install
    npm index.js
    ```

1. 當系統提示時，輸入祕密值。 例如，mySecretPassword。

    下列輸出的變化會出現：

    ```azurecli
    Input the value of your secret > mySecretPassword
    Creating a secret in <your-unique-keyvault-name> called 'mySecret' with the value 'mySecretPassword' ... done.
    Forgetting your secret.
    Your secret is ''.
    Retrieving your secret from <your-unique-keyvault-name>.
    Your secret is 'mySecretPassword'.
    Deleting your secret from <your-unique-keyvault-name> ... done.  
    Purging your secret from <your-unique-keyvault-name> ... done.   
    ```


## <a name="next-steps"></a>後續步驟

在本快速入門中，您已建立金鑰保存庫、儲存秘密，並擷取該秘密。 若要深入了解 Key Vault 以及要如何將其與應用程式整合，請繼續閱讀下列文章。

- 閱讀 [Azure Key Vault 概觀](../general/overview.md)
- 閱讀 [Azure Key Vault 祕密的概觀](about-secrets.md)
- 如何[針對金鑰保存庫的存取進行保護](../general/secure-your-key-vault.md)
- 參閱 [Azure Key Vault 開發人員指南](../general/developers-guide.md)
- 檢閱 [Azure Key Vault 最佳做法](../general/best-practices.md)
