---
title: 使用 DPS 自動管理裝置
titleSuffix: Azure Digital Twins
description: 瞭解如何使用裝置布建服務 (DPS) ，設定自動化的程式以在 Azure 數位 Twins 中布建及淘汰 IoT 裝置。
author: baanders
ms.author: baanders
ms.date: 9/1/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: e783e5dd3b0f1952928d1c36c682c5be1cba2599
ms.sourcegitcommit: 8dd8d2caeb38236f79fe5bfc6909cb1a8b609f4a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98044385"
---
# <a name="auto-manage-devices-in-azure-digital-twins-using-device-provisioning-service-dps"></a>使用裝置布建服務 (DPS) 自動管理 Azure 數位 Twins 中的裝置

在本文中，您將瞭解如何將 Azure 數位 Twins 與裝置布建 [服務 (DPS) ](../iot-dps/about-iot-dps.md)整合。

本文所述的解決方案可讓您 **_使用裝置布_** 建服務，將在 Azure 數位 Twins 中布建及 **_淘汰_** IoT 中樞裝置的程式自動化。 

如需有關布建和 _淘汰__階段的詳細_ 資訊，並進一步瞭解所有企業 IoT 專案通用的一般裝置管理階段，請參閱 IoT 中樞裝置管理檔的 [*裝置生命週期* 一節](../iot-hub/iot-hub-device-management-overview.md#device-lifecycle)。

## <a name="prerequisites"></a>先決條件

在您可以設定布建之前，您必須擁有包含模型和 Twins 的 **Azure 數位 Twins 實例** 。 此實例也應該設定為根據資料更新數位對應項資訊。 

如果您還沒有此設定，您可以遵循 Azure 數位 Twins [*教學課程：連接端對端解決方案*](tutorial-end-to-end.md)來建立它。 本教學課程將逐步引導您使用模型和 Twins、已連線的 Azure [IoT 中樞](../iot-hub/about-iot-hub.md)，以及用來傳播資料流程的數個 [azure](../azure-functions/functions-overview.md) 函式來設定 Azure 數位 Twins 實例。

您稍後會在本文中，從設定您的實例時，需要下列值。 如果您需要再次收集這些值，請使用下列連結以取得相關指示。
* Azure Digital Twins 執行個體 **_主機名稱_** ([在入口網站中尋找](how-to-set-up-instance-portal.md#verify-success-and-collect-important-values))
* Azure 事件中樞連接字串 **_連接字串_** ([在入口網站中尋找](../event-hubs/event-hubs-get-connection-string.md#get-connection-string-from-the-portal)) 

此範例也會使用 **裝置** 模擬器，其中包含使用裝置布建服務進行布建。 裝置模擬器位於此處： [Azure 數位 Twins 和 IoT 中樞整合範例](/samples/azure-samples/digital-twins-iothub-integration/adt-iothub-provision-sample/)。 流覽至範例連結，然後選取標題底下的 [ *下載 ZIP* ] 按鈕，以取得電腦上的範例專案。 將下載的資料夾解壓縮。

裝置模擬器是以 **Node.js** 10.0. x 版或更新版本為基礎。 [*準備您的開發環境*](https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md) ，說明如何在 Windows 或 Linux 上安裝本教學課程的 Node.js。

## <a name="solution-architecture"></a>方案架構

下圖說明此解決方案使用 Azure 數位 Twins 搭配裝置布建服務的架構。 它會顯示裝置布建和淘汰流程。

:::image type="content" source="media/how-to-provision-using-dps/flows.png" alt-text="在端對端案例中，裝置和數個 Azure 服務的觀點。資料會在控溫器裝置與 DPS 之間來回流動。資料也會從 DPS 流出至 IoT 中樞，以及透過標示為「配置」的 Azure 函式傳遞至 Azure 數位 Twins。手動「刪除裝置」動作的資料會流經 IoT 中樞 > 事件中樞 > Azure Functions > Azure 數位 Twins。":::

本文分成兩部分：
* [*使用裝置布建服務自動布建裝置*](#auto-provision-device-using-device-provisioning-service)
* [*使用 IoT 中樞生命週期事件自動淘汰裝置*](#auto-retire-device-using-iot-hub-lifecycle-events)

如需架構中每個步驟的更深入說明，請參閱本文稍後的各節。

## <a name="auto-provision-device-using-device-provisioning-service"></a>使用裝置布建服務自動布建裝置

在本節中，您會將裝置布建服務附加至 Azure 數位 Twins，以透過下列路徑自動布建裝置。 這是稍 [早](#solution-architecture)所示的完整架構摘錄。

:::image type="content" source="media/how-to-provision-using-dps/provision.png" alt-text="布建流程--解決方案架構圖的摘要，其中包含流程的數位標籤區段。控溫器裝置與 DPS 之間的資料會在裝置和 DPS (> 1 之間來回流動 > 裝置) 的 dps 和5。資料也會從 DPS 流出至 IoT 中樞 (4) ，以及透過標示為「配置」 (2) 之 Azure 函式的 Azure 數位 Twins (3) 。":::

以下是程式流程的說明：
1. 裝置會聯繫 DPS 端點，傳遞識別資訊以證明其身分識別。
2. DPS 會驗證註冊清單的註冊識別碼和金鑰，並呼叫 [Azure](../azure-functions/functions-overview.md) 函式來進行配置，藉以驗證裝置身分識別。
3. Azure 函式會在適用 [于裝置](concepts-twins-graph.md) 的 Azure 數位 Twins 中建立新的對應項。
4. DPS 會向 IoT 中樞註冊裝置，並填入裝置所需的對應項狀態。
5. IoT 中樞會將裝置識別碼資訊和 IoT 中樞連線資訊傳回給裝置。 裝置現在可以連線到 IoT 中樞。

下列各節將逐步解說設定此自動布建裝置流程的步驟。

### <a name="create-a-device-provisioning-service"></a>建立裝置佈建服務

使用裝置布建服務布建新裝置時，可以在 Azure 數位 Twins 中建立該裝置的新對應項。

建立將用來布建 IoT 裝置的裝置布建服務實例。 您可以使用下列 Azure CLI 指示，或使用 Azure 入口網站： [*快速入門：設定 IoT 中樞裝置布建服務與 Azure 入口網站*](../iot-dps/quick-setup-auto-provision.md)。

下列 Azure CLI 命令會建立裝置布建服務。 您將需要指定名稱、資源群組和區域。 如果您的[電腦上已安裝](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true)Azure CLI，命令可以在[Cloud Shell](https://shell.azure.com)中執行，或在本機執行。

```azurecli-interactive
az iot dps create --name <Device Provisioning Service name> --resource-group <resource group name> --location <region; for example, eastus>
```

### <a name="create-an-azure-function"></a>建立 Azure 函式

接下來，您將在函數應用程式內建立 HTTP 要求觸發的函式。 您可以使用在端對端教學課程中建立的函數應用程式 ([*教學課程：連接端對端解決方案*](tutorial-end-to-end.md)) 或您自己的解決方案。

裝置布建服務會使用此函式，在 [自訂配置原則](../iot-dps/how-to-use-custom-allocation-policies.md) 中布建新的裝置。 如需有關搭配 Azure 函式使用 HTTP 要求的詳細資訊，請參閱 [*Azure Functions 的 Azure HTTP 要求觸發程式*](../azure-functions/functions-bindings-http-webhook-trigger.md)。

在您的函數應用程式專案中，加入新的函式。 此外，將新的 NuGet 套件新增至專案： `Microsoft.Azure.Devices.Provisioning.Service` 。

在新建立的函式程式碼檔案中，貼上下列程式碼。

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/adtIotHub_allocate.cs":::

儲存檔案，然後重新發佈您的函數應用程式。 如需有關發佈函式應用程式的指示，請參閱端對端教學課程的 [*發佈應用程式*](tutorial-end-to-end.md#publish-the-app) 一節。

### <a name="configure-your-function"></a>設定您的函式

接下來，您必須在您的函數應用程式中設定舊版的環境變數，其中包含您所建立之 Azure 數位 Twins 實例的參考。 如果您使用了端對端教學課程 (教學課程 [*：連接端對端解決方案*](tutorial-end-to-end.md)) ，將會設定此設定。

使用這個 Azure CLI 命令新增設定：

```azurecli-interactive
az functionapp config appsettings set --settings "ADT_SERVICE_URL=https://<Azure Digital Twins instance _host name_>" -g <resource group> -n <your App Service (function app) name>
```

確定已針對函數應用程式正確設定許可權和受控識別角色指派，如端對端教學課程中的 [*將許可權指派給函數應用程式*](tutorial-end-to-end.md#assign-permissions-to-the-function-app) 一節中所述。

### <a name="create-device-provisioning-enrollment"></a>建立裝置布建註冊

接下來，您必須使用 **自訂配置** 函式，在裝置布建服務中建立註冊。 依照有關自訂配置原則的「 [*建立註冊*](../iot-dps/how-to-use-custom-allocation-policies.md#create-the-enrollment) 和 [*衍生唯一的裝置金鑰*](../iot-dps/how-to-use-custom-allocation-policies.md#derive-unique-device-keys) 」一節中的指示執行此動作。

進行該流程時，您會在步驟中選取您的函式，以 **選取要如何將裝置指派給中樞**，以將註冊連結至您剛才建立的函式。 建立註冊之後，註冊名稱和主要或次要 SAS 金鑰稍後將用來設定本文的裝置模擬器。

### <a name="set-up-the-device-simulator"></a>設定裝置模擬器

此範例會使用裝置模擬器，其中包含使用裝置布建服務進行布建。 裝置模擬器位於此處： [Azure 數位 Twins 和 IoT 中樞整合範例](/samples/azure-samples/digital-twins-iothub-integration/adt-iothub-provision-sample/)。 如果您尚未下載範例，請流覽至範例連結，然後選取標題底下的 [ *下載 ZIP* ] 按鈕，以立即取得範例。 將下載的資料夾解壓縮。

開啟命令視窗，並流覽至已下載的資料夾，然後流覽至 *裝置* 模擬器目錄。 使用下列命令來安裝專案的相依性：

```cmd
npm install
```

接下來，將 *env 範本* 檔複製到名為 *env* 的新檔案，並填入這些設定：

```cmd
PROVISIONING_HOST = "global.azure-devices-provisioning.net"
PROVISIONING_IDSCOPE = "<Device Provisioning Service Scope ID>"
PROVISIONING_REGISTRATION_ID = "<Device Registration ID>"
ADT_MODEL_ID = "dtmi:contosocom:DigitalTwins:Thermostat;1"
PROVISIONING_SYMMETRIC_KEY = "<Device Provisioning Service enrollment primary or secondary SAS key>"
```

儲存並關閉檔案。

### <a name="start-running-the-device-simulator"></a>開始執行裝置模擬器

仍在命令視窗中的 *裝置* 模擬器目錄中，使用下列命令啟動裝置模擬器：

```cmd
node .\adt_custom_register.js
```

您應該會看到裝置已註冊並聯機到 IoT 中樞，然後開始傳送訊息。
:::image type="content" source="media/how-to-provision-using-dps/output.png" alt-text="顯示裝置註冊和傳送訊息命令視窗":::

### <a name="validate"></a>Validate

由於您在本文中設定的流程，將會在 Azure 數位 Twins 中自動註冊裝置。 使用下列 [Azure 數位 TWINS CLI](how-to-use-cli.md) 命令，在您建立的 Azure 數位 Twins 實例中尋找裝置的對應項。

```azurecli-interactive
az dt twin show -n <Digital Twins instance name> --twin-id <Device Registration ID>"
```

您應該會看到在 Azure 數位 Twins 實例中找到的裝置對應項。
:::image type="content" source="media/how-to-provision-using-dps/show-provisioned-twin.png" alt-text="顯示新建立對應項的命令視窗":::

## <a name="auto-retire-device-using-iot-hub-lifecycle-events"></a>使用 IoT 中樞生命週期事件自動淘汰裝置

在本節中，您會將 IoT 中樞生命週期事件附加至 Azure 數位 Twins，以透過下列路徑自動淘汰裝置。 這是稍 [早](#solution-architecture)所示的完整架構摘錄。

:::image type="content" source="media/how-to-provision-using-dps/retire.png" alt-text="淘汰裝置流程--解決方案架構圖表的摘要，其中包含流程的數位標籤區段。控溫器裝置會顯示，且不會連接到圖表中的 Azure 服務。手動「刪除裝置」動作的資料會流經 IoT 中樞 (1) > 事件中樞 (2) > Azure Functions > Azure 數位 Twins (3) 。":::

以下是程式流程的說明：
1. 外部或手動進程會觸發在 IoT 中樞內刪除裝置。
2. IoT 中樞會刪除裝置，並產生將路由傳送至[事件中樞](../event-hubs/event-hubs-about.md)的[裝置生命週期](../iot-hub/iot-hub-device-management-overview.md#device-lifecycle)事件。
3. Azure 函式會刪除 Azure 數位 Twins 中的裝置對應項。

下列各節將逐步解說設定此自動淘汰裝置流程的步驟。

### <a name="create-an-event-hub"></a>建立事件中樞

您現在需要建立將用來接收 IoT 中樞生命週期事件的 Azure [事件中樞](../event-hubs/event-hubs-about.md)。 

使用下列資訊，逐步完成 [*建立事件中樞*](../event-hubs/event-hubs-create.md) 快速入門中所述的步驟：
* 如果您使用端對端教學課程 ([*教學課程：連接端對端解決方案*](tutorial-end-to-end.md)) ，您可以重複使用您為端對端教學課程建立的資源群組。
* 為您的事件中樞 *lifecycleevents* 命名或您選擇的其他專案，並記住您所建立的命名空間。 當您在下一節中設定生命週期函式和 IoT 中樞路由時，將會使用這些功能。

### <a name="create-an-azure-function"></a>建立 Azure 函式

接下來，您將在函數應用程式內建立事件中樞觸發的函式。 您可以使用在端對端教學課程中建立的函數應用程式 ([*教學課程：連接端對端解決方案*](tutorial-end-to-end.md)) 或您自己的解決方案。 

命名您的事件中樞觸發程式 *lifecycleevents*，並將事件中樞觸發程式連線到您在上一個步驟中建立的事件中樞。 如果您使用不同的事件中樞名稱，請將它變更為符合以下的觸發程式名稱。

此函式會使用 IoT 中樞裝置生命週期事件來淘汰現有的裝置。 如需生命週期事件的詳細資訊，請參閱 [*IoT 中樞非遙測事件*](../iot-hub/iot-hub-devguide-messages-d2c.md#non-telemetry-events)。 如需使用事件中樞搭配 Azure 函式的詳細資訊，請參閱 [*Azure Functions 的 Azure 事件中樞觸發程式*](../azure-functions/functions-bindings-event-hubs-trigger.md)。

在您已發佈的函式應用程式中，新增 *事件中樞觸發* 程式類型的新函式類別，並貼上下列程式碼。

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/adtIotHub_delete.cs":::

儲存專案，然後再次發佈函數應用程式。 如需有關發佈函式應用程式的指示，請參閱端對端教學課程的 [*發佈應用程式*](tutorial-end-to-end.md#publish-the-app) 一節。

### <a name="configure-your-function"></a>設定您的函式

接下來，您必須在您的函數應用程式中設定舊版的環境變數，其中包含您所建立之 Azure 數位 Twins 實例的參考，以及事件中樞。 如果您使用了端對端教學課程 (教學課程 [*：連接端對端解決方案*](./tutorial-end-to-end.md)) ，則會先設定第一個設定。

使用這個 Azure CLI 命令新增設定。 如果您的[電腦上已安裝](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true)Azure CLI，命令可以在[Cloud Shell](https://shell.azure.com)中執行，或在本機執行。

```azurecli-interactive
az functionapp config appsettings set --settings "ADT_SERVICE_URL=https://<Azure Digital Twins instance _host name_>" -g <resource group> -n <your App Service (function app) name>
```

接下來，您必須設定函數環境變數，以連接到新建立的事件中樞。

```azurecli-interactive
az functionapp config appsettings set --settings "EVENTHUB_CONNECTIONSTRING=<Event Hubs SAS connection string Listen>" -g <resource group> -n <your App Service (function app) name>
```

確定已針對函數應用程式正確設定許可權和受控識別角色指派，如端對端教學課程中的 [*將許可權指派給函數應用程式*](tutorial-end-to-end.md#assign-permissions-to-the-function-app) 一節中所述。

### <a name="create-an-iot-hub-route-for-lifecycle-events"></a>建立適用于生命週期事件的 IoT 中樞路由

現在您需要設定 IoT 中樞路由，以路由裝置生命週期事件。 在此情況下，您將會特別接聽裝置刪除事件（由所識別） `if (opType == "deleteDeviceIdentity")` 。 這會觸發刪除數位對應項專案，完成裝置及其數位對應項的淘汰。

本文說明如何建立 IoT 中樞路由的指示： [*使用 IoT 中樞訊息路由將裝置到雲端訊息傳送至不同的端點*](../iot-hub/iot-hub-devguide-messages-d2c.md)。 *非遙測事件* 一節說明您可以使用 **裝置生命週期事件** 作為路由的資料來源。

您必須進行此設定的步驟如下：
1. 建立自訂 IoT 中樞事件中樞端點。 此端點應該以您在 [ [*建立事件中樞*](#create-an-event-hub) ] 區段中建立的事件中樞為目標。
2. 新增 *裝置生命週期事件* 路由。 使用在上一個步驟中建立的端點。 您可以藉由新增路由查詢來限制裝置生命週期事件，只傳送刪除事件 `opType='deleteDeviceIdentity'` 。
    :::image type="content" source="media/how-to-provision-using-dps/lifecycle-route.png" alt-text="新增路由":::

完成此流程之後，所有專案都會設定為端對端淘汰裝置。

### <a name="validate"></a>Validate

若要觸發淘汰的進程，您需要從 IoT 中樞手動刪除裝置。

在本文的前半 [部](#auto-provision-device-using-device-provisioning-service)，您已在 IoT 中樞內建立裝置，以及對應的數位對應項。 

現在，移至 IoT 中樞並刪除該裝置 (您可以使用 [Azure CLI 命令](/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest&preserve-view=true#ext-azure-cli-iot-ext-az-iot-hub-device-identity-delete) 或 [Azure 入口網站](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Devices%2FIotHubs)) 來執行此動作。 

裝置將會自動從 Azure 數位 Twins 中移除。 

使用下列 [Azure 數位 TWINS CLI](how-to-use-cli.md) 命令，確認已刪除 Azure 數位 Twins 實例中的裝置對應項。

```azurecli-interactive
az dt twin show -n <Digital Twins instance name> --twin-id <Device Registration ID>"
```

您應該會看到裝置的對應項不再于 Azure 數位 Twins 實例中找到。
:::image type="content" source="media/how-to-provision-using-dps/show-retired-twin.png" alt-text="顯示找不到對應項的命令視窗":::

## <a name="clean-up-resources"></a>清除資源

如果您不再需要本文中建立的資源，請遵循下列步驟來刪除這些資源。

使用 Azure Cloud Shell 或本機 Azure CLI，您可以使用 [az group delete](/cli/azure/group?view=azure-cli-latest&preserve-view=true#az-group-delete) 命令刪除資源群組中的所有 Azure 資源。 這會移除資源群組;Azure 數位 Twins 實例;IoT 中樞和中樞裝置註冊;事件方格主題和相關聯的訂閱;事件中樞命名空間和這兩個 Azure Functions apps，包括儲存體等相關聯的資源。

> [!IMPORTANT]
> 刪除資源群組是無法回復的動作。 資源群組和其中包含的所有資源都將永久刪除。 請確定您不會不小心刪除錯誤的資源群組或資源。 

```azurecli-interactive
az group delete --name <your-resource-group>
```

然後，刪除您從本機電腦下載的專案範例資料夾。

## <a name="next-steps"></a>下一步

針對裝置所建立的數位 twins 會在 Azure 數位 Twins 中儲存為一般階層，但是可以使用模型資訊和組織的多層級階層進行擴充。 若要深入瞭解此概念，請閱讀：

* [*概念：數位 twins 和對應項圖表*](concepts-twins-graph.md)

您可以撰寫自訂邏輯，使用已儲存在 Azure 數位 Twins 中的模型和圖形資料來自動提供此資訊。 若要深入瞭解如何管理、升級及從 twins 圖中取得資訊，請參閱下列各項：

* [*如何：管理數位對應項*](how-to-manage-twin.md)
* [*How to：查詢對應項圖形*](how-to-query-graph.md)