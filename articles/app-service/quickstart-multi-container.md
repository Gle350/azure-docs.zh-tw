---
title: 快速入門：建立多個容器應用程式
description: 藉由部署您的第一個多容器應用程式，在 Azure App Service 上開始使用多容器應用程式。
keywords: azure 應用程式服務, web 應用程式, linux, docker, compose, 多容器, 多重容器, 適用於容器的 web 應用程式, 多個容器, 容器, wordpress, 適用於 mysql 的 azure db, 具有容器的生產資料庫
author: msangapu-msft
ms.topic: quickstart
ms.date: 08/23/2019
ms.author: msangapu
ms.custom: mvc, seodec18, devx-track-azurecli
ms.openlocfilehash: 2920aad07ac54a19962f552debb8cfa809e17294
ms.sourcegitcommit: 65a4f2a297639811426a4f27c918ac8b10750d81
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2020
ms.locfileid: "96558346"
---
# <a name="create-a-multi-container-preview-app-using-a-docker-compose-configuration"></a>使用 Docker Compose 設定建立多容器 (預覽) 應用程式

> [!NOTE]
> 多容器處於預覽狀態。

[適用於容器的 Web 應用程式](overview.md#app-service-on-linux)提供彈性的 Docker 映像使用方式。 本快速入門說明如何使用 Docker Compose 組態，在 [Cloud Shell](../cloud-shell/overview.md) 中將多容器應用程式 (預覽) 部署至用於容器的 Web App。

![適用於容器的 Web 應用程式上的範例多容器應用程式][1]

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)]

本文需要 2.0.32 版或更新版本的 Azure CLI。 如果您是使用 Azure Cloud Shell，就已安裝最新版本。

## <a name="download-the-sample"></a>下載範例

在本快速入門中，您會使用從 [Docker](https://docs.docker.com/compose/wordpress/#define-the-project) 取得的 Compose 檔案。 組態檔位於 [Azure 範例](https://github.com/Azure-Samples/multicontainerwordpress)中。

[!code-yml[Main](../../azure-app-service-multi-container/docker-compose-wordpress.yml)]

在 Cloud Shell 中，建立快速入門目錄並變更為此目錄。

```bash
mkdir quickstart

cd $HOME/quickstart
```

下一步，執行下列命令，將範例應用程式存放庫複製到您的快速入門目錄。 然後，變更為 `multicontainerwordpress` 目錄。

```bash
git clone https://github.com/Azure-Samples/multicontainerwordpress

cd multicontainerwordpress
```

## <a name="create-a-resource-group"></a>建立資源群組

[!INCLUDE [resource group intro text](../../includes/resource-group.md)]

在 Cloud Shell 中，使用 [`az group create`](/cli/azure/group?view=azure-cli-latest#az-group-create) 命令來建立資源群組。 下列範例會在 *美國中南部* 位置中建立名為 *myResourceGroup* 的資源群組。 若要查看 **標準** 層中 Linux 上之 App Service 的所有支援位置，請執行 [`az appservice list-locations --sku S1 --linux-workers-enabled`](/cli/azure/appservice?view=azure-cli-latest#az-appservice-list-locations) 命令。

```azurecli-interactive
az group create --name myResourceGroup --location "South Central US"
```

您通常會在附近的區域中建立資源群組和資源。

當命令完成時，JSON 輸出會顯示資源群組屬性。

## <a name="create-an-azure-app-service-plan"></a>建立 Azure App Service 方案

在 Cloud Shell 中，使用 [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az-appservice-plan-create) 命令在資源群組中建立 App Service 方案。

下列範例會在 **標準** 定價層 (`--sku S1`) 和 Linux 容器 (`--is-linux`) 中，建立名為 `myAppServicePlan` 的 App Service 方案。

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku S1 --is-linux
```

建立 App Service 方案後，Azure CLI 會顯示類似下列範例的資訊：

<pre>
{
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "South Central US",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "linux",
  "location": "South Central US",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  &lt; JSON data removed for brevity. &gt;
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
}
</pre>

## <a name="create-a-docker-compose-app"></a>建立 Docker Compose 應用程式

> [!NOTE]
> Azure App Service 上的 Docker Compose 目前有 4,000 個字元的限制。

在 Cloud Shell 終端機中，使用 [az webapp create](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) 命令，在 `myAppServicePlan` App Service 方案中建立多容器 [Web 應用程式](overview.md#app-service-on-linux)。 別忘了將 _\<app_name>_ 取代為唯一的應用程式名稱 (有效字元為 `a-z`、`0-9` 及 `-`)。

```azurecli
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --multicontainer-config-type compose --multicontainer-config-file compose-wordpress.yml
```

建立 Web 應用程式後，Azure CLI 會顯示類似下列範例的輸出：

<pre>
{
  "additionalProperties": {},
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "&lt;app_name&gt;.azurewebsites.net",
  "enabled": true,
  &lt; JSON data removed for brevity. &gt;
}
</pre>

### <a name="browse-to-the-app"></a>瀏覽至應用程式

瀏覽至已部署的應用程式 (位於 `http://<app_name>.azurewebsites.net`)。 此應用程式可能需要數分鐘才能載入。 如果發生錯誤，請再等待數分鐘，然後重新整理瀏覽器。

![適用於容器的 Web 應用程式上的範例多容器應用程式][1]

**恭喜**，您已在適用於容器的 Web 應用程式中建立多容器應用程式。

[!INCLUDE [Clean-up section](../../includes/cli-script-clean-up.md)]

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [教學課程：多容器 WordPress 應用程式](tutorial-multi-container-app.md)

> [!div class="nextstepaction"]
> [設定自訂容器](configure-custom-container.md)

<!--Image references-->
[1]: media/tutorial-multi-container-app/azure-multi-container-wordpress-install.png
