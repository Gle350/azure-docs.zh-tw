---
title: CLI：手動調整應用程式
description: 了解如何使用 Azure CLI 將 App Service 應用程式的部署和管理自動化。 此範例說明如何手動調整應用程式。
author: msangapu-msft
tags: azure-service-management
ms.assetid: 251d9074-8fff-4121-ad16-9eca9556ac96
ms.devlang: azurecli
ms.topic: sample
ms.date: 12/11/2017
ms.author: msangapu
ms.custom: mvc, seodec18, devx-track-azurecli
ms.openlocfilehash: 9b909d041cf6acba0f3b12ad69018ebc371ff599
ms.sourcegitcommit: 273c04022b0145aeab68eb6695b99944ac923465
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97005608"
---
# <a name="scale-an-app-service-app-manually-using-azure-cli"></a>使用 Azure CLI 來手動調整 App Service 應用程式

此範例指令碼會建立資源群組、App Service 方案和應用程式。 然後將 App Service 方案從單一執行個體調整為多個執行個體。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

 - 本教學課程需要 2.0 版或更新版本的 Azure CLI。 如果您是使用 Azure Cloud Shell，就已安裝最新版本。

## <a name="sample-script"></a>範例指令碼

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/scale-manual/scale-manual.sh "Manual Scale")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>指令碼說明

此指令碼會使用下列命令來建立資源群組、App Service 應用程式及所有相關資源。 下表中的每個命令都會連結至命令特定的文件。

| Command | 注意 |
|---|---|
| [`az group create`](/cli/azure/group#az-group-create) | 建立用來存放所有資源的資源群組。 |
| [`az appservice plan create`](/cli/azure/appservice/plan#az-appservice-plan-create) | 建立 App Service 方案。 |
| [`az webapp create`](/cli/azure/webapp#az-webapp-create) | 建立 App Service 應用程式。 |
| [`az appservice plan update`](/cli/azure/appservice/plan#az-appservice-plan-update) | 更新 App Service 方案的屬性。 |

## <a name="next-steps"></a>後續步驟

如需 Azure CLI 的詳細資訊，請參閱 [Azure CLI 文件](/cli/azure)。

您可以在 [Azure App Service 文件](../samples-cli.md)中找到其他的 App Service CLI 指令碼範例。
