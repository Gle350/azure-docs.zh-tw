---
title: 透過連線的儲存體建立函式應用程式 - Azure CLI
description: Azure CLI 指令碼範例 - 建立連線至 Azure 儲存體的 Azure 函式
ms.topic: sample
ms.date: 04/20/2017
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: 13120ad4478febf9281ff423a3a7a8f8f3b25845
ms.sourcegitcommit: 2aa52d30e7b733616d6d92633436e499fbe8b069
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97934402"
---
# <a name="create-a-function-app-with-a-named-storage-account-connection"></a>透過具名儲存體帳戶連線建立函式應用程式 

這個 Azure Functions 範例指令碼會建立函式應用程式，並將函式連線至 Azure 儲存體帳戶。 包含連線的所建立應用程式設定可以搭配[儲存體觸發程序或繫結](../functions-bindings-storage-blob.md)使用。 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

 - 本教學課程需要 2.0 版或更新版本的 Azure CLI。 如果您是使用 Azure Cloud Shell，就已安裝最新版本。

## <a name="sample-script"></a>範例指令碼

此範例會建立 Azure 函數應用程式，然後將儲存體連接字串新增至應用程式設定。

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-storage/create-function-app-connect-to-storage-account.sh "Integrate Function App into Azure Storage Account")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>指令碼說明

此指令碼會使用下列命令。 下表中的每個命令都會連結至命令特定的文件。

| Command | 注意 |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | 使用位置建立資源群組。 |
| [az storage account create](/cli/azure/storage/account#az-storage-account-create) | 建立儲存體帳戶。 |
| [az functionapp create](/cli/azure/functionapp#az-functionapp-create) | 在無伺服器[取用方案](../consumption-plan.md)中建立函式應用程式。 |
| [az storage account show-connection-string](/cli/azure/storage/account#az-storage-account-show-connection-string) | 取得帳戶的連接字串。 |
| [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set) | 將連接字串設定為函式應用程式中的應用程式設定。 |

## <a name="next-steps"></a>後續步驟

如需 Azure CLI 的詳細資訊，請參閱 [Azure CLI 文件](/cli/azure)。

您可以在 [Azure Functions 文件](../functions-cli-samples.md)中找到其他 Azure Functions CLI 指令碼範例。
