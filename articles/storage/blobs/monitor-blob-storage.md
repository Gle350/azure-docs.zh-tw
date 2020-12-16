---
title: 監視 Azure Blob 儲存體 |Microsoft Docs
description: 瞭解如何監視 Azure Blob 儲存體的效能和可用性。 監視 Azure Blob 儲存體資料、瞭解設定，以及分析計量和記錄資料。
author: normesta
services: storage
ms.service: storage
ms.topic: conceptual
ms.date: 10/26/2020
ms.author: normesta
ms.reviewer: fryu
ms.custom: monitoring, devx-track-csharp, devx-track-azurecli
ms.openlocfilehash: 9224d02e36dbca96d3e54946330d3135ff811829
ms.sourcegitcommit: d2d1c90ec5218b93abb80b8f3ed49dcf4327f7f4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/16/2020
ms.locfileid: "97590761"
---
# <a name="monitoring-azure-blob-storage"></a>監視 Azure Blob 儲存體

當您有依賴 Azure 資源的重要應用程式和商務程序時，您會想要監視這些資源的可用性、效能和操作。 本文說明 Azure Blob 儲存體所產生的監視資料，以及您可以如何使用 Azure 監視器的功能來分析此資料的警示。

> [!NOTE]
> Azure 監視器中的 Azure 儲存體記錄處於公開預覽狀態，可在所有公用雲端區域中進行預覽測試。 此預覽可讓 blob (記錄，其中包含 Azure Data Lake Storage Gen2) 、檔案、佇列和資料表。 這項功能適用于使用 Azure Resource Manager 部署模型建立的所有儲存體帳戶。 請參閱 [儲存體帳戶總覽](../common/storage-account-overview.md)。

## <a name="monitor-overview"></a>監視概觀

每個 Blob 儲存體資源的 Azure 入口網站中的 **總覽** 頁面包含資源使用量的簡短觀點，例如要求和每小時計費。 此資訊很實用，但只有少量的監視資料可供使用。 這部分資料會自動收集，並在您建立資源時立即可供分析。 您可使用一些設定來啟用其他類型的資料收集。

## <a name="what-is-azure-monitor"></a>Azure 監視器是什麼？
Azure Blob 儲存體會使用 [Azure 監視器](../../azure-monitor/overview.md)來建立監視資料，這是 Azure 中的完整堆疊監視服務。 Azure 監視器提供一組完整的功能，可監視您的 Azure 資源以及其他雲端和內部部署環境中的資源。 

請從使用下列內容來 [監視 Azure 資源](../../azure-monitor/insights/monitor-azure-resource.md) 的文章開始 Azure 監視器說明下列各項：

- Azure 監視器是什麼？
- 與監視相關聯的成本
- 在 Azure 中收集的監視資料
- 設定資料收集
- Azure 中用來分析和警示監視資料的標準工具

下列各節以本文為基礎，描述從 Azure 儲存體收集的特定資料。 其中的範例示範如何使用 Azure 工具來設定資料收集及分析此資料。

## <a name="monitoring-data"></a>監視資料

Azure Blob 儲存體會收集與其他 Azure 資源相同的監視資料類型，如 [azure 資源的監視資料](../../azure-monitor/insights/monitor-azure-resource.md#monitoring-data)中所述。 

如需 Azure Blob 儲存體所建立計量和記錄計量的詳細資訊，請參閱 [Azure blob 儲存體監視資料參考](monitor-blob-storage-reference.md) 。

Azure 監視器中的計量和記錄只支援 Azure Resource Manager 儲存體帳戶。 Azure 監視器不支援傳統儲存體帳戶。 如果您想在傳統儲存體帳戶上使用計量或記錄，則必須遷移至 Azure Resource Manager 儲存體帳戶。 請參閱[遷移至 Azure Resource Manager](../../virtual-machines/migration-classic-resource-manager-overview.md)。

如果想要，您可繼續使用傳統計量和記錄。 事實上，傳統計量和記錄可與 Azure 監視器中的計量和記錄平行提供。 在 Azure 儲存體結束舊版計量和記錄的服務之前，支援維持不變。

## <a name="collection-and-routing"></a>收集和路由

系統會自動收集平臺計量和活動記錄，但是可以使用診斷設定將其路由至其他位置。 

若要收集資源記錄，您必須建立診斷設定。 當您建立設定時，請選擇 [ **blob** ] 作為您想要啟用記錄的儲存體類型。 然後，指定您要收集記錄的下列其中一種作業類別目錄。 

| 類別 | 描述 |
|:---|:---|
| StorageRead | 物件的讀取作業。 |
| StorageWrite | 對物件進行寫入作業。 |
| StorageDelete | 刪除物件上的作業。 |

> [!NOTE]
> Data Lake Storage Gen2 不會顯示為儲存體類型。 這是因為 Data Lake Storage Gen2 是適用于 Blob 儲存體的一組功能。 

## <a name="creating-a-diagnostic-setting"></a>建立診斷設定

您可以使用 Azure 入口網站、PowerShell、Azure CLI 或 Azure Resource Manager 範本來建立診斷設定。 

如需一般指引，請參閱 [建立診斷設定以收集 Azure 中的平臺記錄和計量](../../azure-monitor/platform/diagnostic-settings.md)。

> [!NOTE]
> Azure 監視器中的 Azure 儲存體記錄處於公開預覽狀態，可在所有公用雲端區域中進行預覽測試。 此預覽可讓 blob (記錄，其中包含 Azure Data Lake Storage Gen2) 、檔案、佇列和資料表。 這項功能適用于使用 Azure Resource Manager 部署模型建立的所有儲存體帳戶。 請參閱 [儲存體帳戶總覽](../common/storage-account-overview.md)。

### <a name="azure-portal"></a>[Azure 入口網站](#tab/azure-portal)

1. 登入 Azure 入口網站。

2. 瀏覽至儲存體帳戶。

3. 在 [ **監視** ] 區段中，按一下 [ **診斷設定 (預覽])**。

   > [!div class="mx-imgBorder"]
   > ![入口網站 - 診斷記錄](media/monitor-blob-storage/diagnostic-logs-settings-pane.png)   

4. 選擇 [ **blob** ] 作為您想要啟用記錄的儲存體類型。

5. 按一下「新增診斷設定」  。

   > [!div class="mx-imgBorder"]
   > ![入口網站-資源記錄-新增診斷設定](media/monitor-blob-storage/diagnostic-logs-settings-pane-2.png)

   [ **診斷設定** ] 頁面隨即出現。

   > [!div class="mx-imgBorder"]
   > ![資源記錄頁面](media/monitor-blob-storage/diagnostic-logs-page.png)

6. 在頁面的 [ **名稱** ] 欄位中，輸入此資源記錄檔設定的名稱。 然後，選取您要記錄哪些作業 (讀取、寫入和刪除作業) ，以及要將記錄傳送至何處。

#### <a name="archive-logs-to-a-storage-account"></a>將記錄封存至儲存體帳戶

如果您選擇將記錄封存至儲存體帳戶，則會支付傳送至儲存體帳戶的記錄數量。 如需特定的定價，請參閱 [Azure 監視器定價](https://azure.microsoft.com/pricing/details/monitor/#platform-logs)頁面的 [**平臺記錄**] 區段。

1. 選取 [封存 **至儲存體帳戶** ] 核取方塊，然後按一下 [ **設定** ] 按鈕。

   > [!div class="mx-imgBorder"]   
   > ![診斷設定頁面封存儲存體](media/monitor-blob-storage/diagnostic-logs-settings-pane-archive-storage.png)

2. 在 [ **儲存體帳戶** ] 下拉式清單中，選取您要封存記錄的儲存體帳戶，按一下 [ **確定]** 按鈕，然後按一下 [ **儲存** ] 按鈕。

   > [!NOTE]
   > 在您選擇儲存體帳戶作為匯出目的地之前，請參閱封存 [Azure 資源記錄](../../azure-monitor/platform/resource-logs.md#send-to-azure-storage) 以瞭解儲存體帳戶上的必要條件。

#### <a name="stream-logs-to-azure-event-hubs"></a>將記錄串流至 Azure 事件中樞

如果您選擇將記錄串流至事件中樞，您將會支付傳送至事件中樞的記錄數量。 如需特定的定價，請參閱 [Azure 監視器定價](https://azure.microsoft.com/pricing/details/monitor/#platform-logs)頁面的 [**平臺記錄**] 區段。

1. 選取 [ **串流至事件中樞** ] 核取方塊，然後按一下 [ **設定** ] 按鈕。

2. 在 [ **選取事件中樞** ] 窗格中，選擇您想要串流記錄的事件中樞命名空間、名稱和原則名稱。 

   > [!div class="mx-imgBorder"]
   > ![診斷設定頁面事件中樞](media/monitor-blob-storage/diagnostic-logs-settings-pane-event-hub.png)

3. 按一下 [ **確定]** 按鈕，然後按一下 [ **儲存** ] 按鈕。

#### <a name="send-logs-to-azure-log-analytics"></a>將記錄傳送至 Azure Log Analytics

1. 選取 [ **傳送至 Log analytics** ] 核取方塊，並選取 Log Analytics 工作區，然後按一下，再按一下 [ **儲存** ] 按鈕。

   > [!div class="mx-imgBorder"]   
   > ![診斷設定頁面記錄分析](media/monitor-blob-storage/diagnostic-logs-settings-pane-log-analytics.png)

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

1. 開啟 Windows PowerShell 命令視窗，然後使用命令登入您的 Azure 訂用帳戶 `Connect-AzAccount` 。 然後，依照畫面上的指示進行。

   ```powershell
   Connect-AzAccount
   ```

2. 將使用中的訂用帳戶設定為您想要啟用記錄的儲存體帳戶訂用帳戶。

   ```powershell
   Set-AzContext -SubscriptionId <subscription-id>
   ```

#### <a name="archive-logs-to-a-storage-account"></a>將記錄封存至儲存體帳戶

如果您選擇將記錄封存至儲存體帳戶，則會支付傳送至儲存體帳戶的記錄數量。 如需特定的定價，請參閱 [Azure 監視器定價](https://azure.microsoft.com/pricing/details/monitor/#platform-logs)頁面的 [**平臺記錄**] 區段。

使用 [>set-azdiagnosticsetting](/powershell/module/az.monitor/set-azdiagnosticsetting) PowerShell Cmdlet 搭配參數來啟用記錄 `StorageAccountId` 。

```powershell
Set-AzDiagnosticSetting -ResourceId <storage-service-resource-id> -StorageAccountId <storage-account-resource-id> -Enabled $true -Category <operations-to-log> -RetentionEnabled <retention-bool> -RetentionInDays <number-of-days>
```

將 `<storage-service-resource--id>` 此程式碼片段中的預留位置取代為 blob 服務的資源識別碼。 在 Azure 入口網站中，開啟您儲存體帳戶的 [屬性] 頁面，即可尋找資源識別碼。

您可以使用 `StorageRead` 、 `StorageWrite` 和做 `StorageDelete` 為 **Category** 參數的值。

以下是範例：

`Set-AzDiagnosticSetting -ResourceId /subscriptions/208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount/blobServices/default -StorageAccountId /subscriptions/208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/myloggingstorageaccount -Enabled $true -Category StorageWrite,StorageDelete`

如需每個參數的說明，請參閱透過 [Azure PowerShell 封存 Azure 資源記錄](../../azure-monitor/platform/resource-logs.md#send-to-azure-storage)。

#### <a name="stream-logs-to-an-event-hub"></a>將記錄串流至事件中樞

如果您選擇將記錄串流至事件中樞，您將會支付傳送至事件中樞的記錄數量。 如需特定的定價，請參閱 [Azure 監視器定價](https://azure.microsoft.com/pricing/details/monitor/#platform-logs)頁面的 [**平臺記錄**] 區段。

使用 [>set-azdiagnosticsetting](/powershell/module/az.monitor/set-azdiagnosticsetting) PowerShell Cmdlet 搭配參數來啟用記錄 `EventHubAuthorizationRuleId` 。

```powershell
Set-AzDiagnosticSetting -ResourceId <storage-service-resource-id> -EventHubAuthorizationRuleId <event-hub-namespace-and-key-name> -Enabled $true -Category <operations-to-log> -RetentionEnabled <retention-bool> -RetentionInDays <number-of-days>
```

以下是範例：

`Set-AzDiagnosticSetting -ResourceId /subscriptions/208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount/blobServices/default -EventHubAuthorizationRuleId /subscriptions/20884142-a14v3-4234-5450-08b10c09f4/resourceGroups/myresourcegroup/providers/Microsoft.EventHub/namespaces/myeventhubnamespace/authorizationrules/RootManageSharedAccessKey -Enabled $true -Category StorageDelete`

如需每個參數的描述，請參閱透過 [PowerShell Cmdlet 將資料串流至事件中樞](../../azure-monitor/platform/resource-logs.md#send-to-azure-event-hubs)。

#### <a name="send-logs-to-log-analytics"></a>將記錄傳送至 Log Analytics

使用 [>set-azdiagnosticsetting](/powershell/module/az.monitor/set-azdiagnosticsetting) PowerShell Cmdlet 搭配參數來啟用記錄 `WorkspaceId` 。

```powershell
Set-AzDiagnosticSetting -ResourceId <storage-service-resource-id> -WorkspaceId <log-analytics-workspace-resource-id> -Enabled $true -Category <operations-to-log> -RetentionEnabled <retention-bool> -RetentionInDays <number-of-days>
```

以下是範例：

`Set-AzDiagnosticSetting -ResourceId /subscriptions/208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount/blobServices/default -WorkspaceId /subscriptions/208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/myresourcegroup/providers/Microsoft.OperationalInsights/workspaces/my-analytic-workspace -Enabled $true -Category StorageDelete`

如需詳細資訊，請參閱 [Azure 監視器中的將 Azure 資源記錄串流至 Log Analytics 工作區](../../azure-monitor/platform/resource-logs.md#send-to-log-analytics-workspace)。

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

1. 首先，開啟 [Azure Cloud Shell](../../cloud-shell/overview.md)，或若已在本機[安裝](/cli/azure/install-azure-cli) Azure CLI，請開啟 Windows PowerShell 等命令主控台應用程式。

2. 如果您的身分識別與多個訂用帳戶相關聯，請將您的使用中訂用帳戶設定為您想要啟用記錄的儲存體帳戶訂用帳戶。

   ```azurecli-interactive
   az account set --subscription <subscription-id>
   ```

   使用訂閱識別碼取代 `<subscription-id>` 預留位置值。

#### <a name="archive-logs-to-a-storage-account"></a>將記錄封存至儲存體帳戶

如果您選擇將記錄封存至儲存體帳戶，則會支付傳送至儲存體帳戶的記錄數量。 如需特定的定價，請參閱 [Azure 監視器定價](https://azure.microsoft.com/pricing/details/monitor/#platform-logs)頁面的 [**平臺記錄**] 區段。

使用 [az monitor 診斷-settings create](/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create) 命令來啟用記錄。

```azurecli-interactive
az monitor diagnostic-settings create --name <setting-name> --storage-account <storage-account-name> --resource <storage-service-resource-id> --resource-group <resource-group> --logs '[{"category": <operations>, "enabled": true "retentionPolicy": {"days": <number-days>, "enabled": <retention-bool}}]'
```

`<storage-service-resource--id>`將此程式碼片段中的預留位置取代為資源識別碼 Blob 儲存體服務。 在 Azure 入口網站中，開啟您儲存體帳戶的 [屬性] 頁面，即可尋找資源識別碼。

您可以使用 `StorageRead` 、 `StorageWrite` 和做 `StorageDelete` 為 **category** 參數的值。

以下是範例：

`az monitor diagnostic-settings create --name setting1 --storage-account mystorageaccount --resource /subscriptions/938841be-a40c-4bf4-9210-08bcf06c09f9/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/myloggingstorageaccount/blobServices/default --resource-group myresourcegroup --logs '[{"category": StorageWrite, "enabled": true, "retentionPolicy": {"days": 90, "enabled": true}}]'`

如需每個參數的描述，請參閱透過 Azure CLI 的封存 [資源記錄](../../azure-monitor/platform/resource-logs.md#send-to-azure-storage)檔。

#### <a name="stream-logs-to-an-event-hub"></a>將記錄串流至事件中樞

如果您選擇將記錄串流至事件中樞，您將會支付傳送至事件中樞的記錄數量。 如需特定的定價，請參閱 [Azure 監視器定價](https://azure.microsoft.com/pricing/details/monitor/#platform-logs)頁面的 [**平臺記錄**] 區段。

使用 [az monitor 診斷-settings create](/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create) 命令來啟用記錄。

```azurecli-interactive
az monitor diagnostic-settings create --name <setting-name> --event-hub <event-hub-name> --event-hub-rule <event-hub-namespace-and-key-name> --resource <storage-account-resource-id> --logs '[{"category": <operations>, "enabled": true "retentionPolicy": {"days": <number-days>, "enabled": <retention-bool}}]'
```

以下是範例：

`az monitor diagnostic-settings create --name setting1 --event-hub myeventhub --event-hub-rule /subscriptions/938841be-a40c-4bf4-9210-08bcf06c09f9/resourceGroups/myresourcegroup/providers/Microsoft.EventHub/namespaces/myeventhubnamespace/authorizationrules/RootManageSharedAccessKey --resource /subscriptions/938841be-a40c-4bf4-9210-08bcf06c09f9/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/myloggingstorageaccount/blobServices/default --logs '[{"category": StorageDelete, "enabled": true }]'`

如需每個參數的描述，請參閱透過 [Azure CLI 將串流資料串流至事件中樞](../../azure-monitor/platform/resource-logs.md#send-to-azure-event-hubs)。

#### <a name="send-logs-to-log-analytics"></a>將記錄傳送至 Log Analytics

使用 [az monitor 診斷-settings create](/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create) 命令來啟用記錄。

```azurecli-interactive
az monitor diagnostic-settings create --name <setting-name> --workspace <log-analytics-workspace-resource-id> --resource <storage-account-resource-id> --logs '[{"category": <category name>, "enabled": true "retentionPolicy": {"days": <days>, "enabled": <retention-bool}}]'
```

以下是範例：

`az monitor diagnostic-settings create --name setting1 --workspace /subscriptions/208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/myresourcegroup/providers/Microsoft.OperationalInsights/workspaces/my-analytic-workspace --resource /subscriptions/938841be-a40c-4bf4-9210-08bcf06c09f9/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/myloggingstorageaccount/blobServices/default --logs '[{"category": StorageDelete, "enabled": true ]'`

 如需詳細資訊，請參閱 [Azure 監視器中的將 Azure 資源記錄串流至 Log Analytics 工作區](../../azure-monitor/platform/resource-logs.md#send-to-log-analytics-workspace)。

### <a name="template"></a>[範本](#tab/template)

若要查看建立診斷設定的 Azure Resource Manager 範本，請參閱 [Azure 儲存體的診斷設定](../../azure-monitor/samples/resource-manager-diagnostic-settings.md#diagnostic-setting-for-azure-storage)。

---

## <a name="analyzing-metrics"></a>分析計量

您可使用計量瀏覽器，利用其他 Azure 服務的計量來分析 Azure 儲存體的計量。 從 [Azure 監視器] 功能表中選擇 [計量]，以開啟計量瀏覽器。 如需使用此工具的詳細資訊，請參閱[開始使用 Azure 計量瀏覽器](../../azure-monitor/platform/metrics-getting-started.md)。 

此範例說明如何檢視帳戶層級的 **交易**。

![在 Azure 入口網站中存取計量的螢幕擷取畫面](./media/monitor-blob-storage/access-metrics-portal.png)

針對支援維度的計量，您可使用所需的維度值來篩選計量。 此範例說明如何藉由選取 [API 名稱] 維度的值，檢視特定作業在帳戶層級的 [交易]。

![在 Azure 入口網站中存取含維度之計量的螢幕擷取畫面](./media/monitor-blob-storage/access-metrics-portal-with-dimension.png)

如需 Azure 儲存體支援的完整維度清單，請參閱[計量維度](monitor-blob-storage-reference.md#metrics-dimensions)。

Azure Blob 儲存體的計量位於下列命名空間： 

- Microsoft.Storage/storageAccounts
- Microsoft.Storage/storageAccounts/blobServices

如需所有 Azure 監視器支援計量（包括 Azure Blob 儲存體）的清單，請參閱 [Azure 監視器支援的度量](../../azure-monitor/platform/metrics-supported.md)。


### <a name="accessing-metrics"></a>存取計量

> [!TIP]
> 若要檢視 Azure CLI 或 .NET 範例，請選擇此處所列的對應索引標籤。

### <a name="net"></a>[.NET](#tab/azure-portal)

Azure 監視器提供 [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Management.Monitor/) 來讀取計量定義和值。 [範例程式碼](https://azure.microsoft.com/resources/samples/monitor-dotnet-metrics-api/)示範如何使用 SDK 搭配不同的參數。 您必須使用 `0.18.0-preview` 或更新版本的儲存體計量。
 
在這些範例中，請將 `<resource-ID>` 預留位置取代為整個儲存體帳戶或 Blob 儲存體服務的資源識別碼。 您可在 Azure 入口網站中儲存體帳戶的 [屬性] 頁面上找到這些資源識別碼。

將 `<subscription-ID>` 變數取代為您的訂用帳戶識別碼。 如需有關如何取得 `<tenant-ID>`、`<application-ID>` 和 `<AccessKey>`值的指引，請參閱[使用入口網站來建立可存取資源的 Azure AD 應用程式和服務主體](../../active-directory/develop/howto-create-service-principal-portal.md)。 

#### <a name="list-the-account-level-metric-definition"></a>列出帳戶層級的計量定義

下列範例說明如何列出帳戶層級的計量定義：

```csharp
    public static async Task ListStorageMetricDefinition()
    {
        var resourceId = "<resource-ID>";
        var subscriptionId = "<subscription-ID>";
        var tenantId = "<tenant-ID>";
        var applicationId = "<application-ID>";
        var accessKey = "<AccessKey>";


        MonitorManagementClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;
        IEnumerable<MetricDefinition> metricDefinitions = await readOnlyClient.MetricDefinitions.ListAsync(resourceUri: resourceId, cancellationToken: new CancellationToken());

        foreach (var metricDefinition in metricDefinitions)
        {
            // Enumrate metric definition:
            //    Id
            //    ResourceId
            //    Name
            //    Unit
            //    MetricAvailabilities
            //    PrimaryAggregationType
            //    Dimensions
            //    IsDimensionRequired
        }
    }

```

#### <a name="reading-account-level-metric-values"></a>讀取帳戶層級的度量值

下列範例說明如何讀取帳戶層級的 `UsedCapacity` 資料：

```csharp
    public static async Task ReadStorageMetricValue()
    {
        var resourceId = "<resource-ID>";
        var subscriptionId = "<subscription-ID>";
        var tenantId = "<tenant-ID>";
        var applicationId = "<application-ID>";
        var accessKey = "<AccessKey>";

        MonitorClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;

        Microsoft.Azure.Management.Monitor.Models.Response Response;

        string startDate = DateTime.Now.AddHours(-3).ToUniversalTime().ToString("o");
        string endDate = DateTime.Now.ToUniversalTime().ToString("o");
        string timeSpan = startDate + "/" + endDate;

        Response = await readOnlyClient.Metrics.ListAsync(
            resourceUri: resourceId,
            timespan: timeSpan,
            interval: System.TimeSpan.FromHours(1),
            metricnames: "UsedCapacity",

            aggregation: "Average",
            resultType: ResultType.Data,
            cancellationToken: CancellationToken.None);

        foreach (var metric in Response.Value)
        {
            // Enumrate metric value
            //    Id
            //    Name
            //    Type
            //    Unit
            //    Timeseries
            //        - Data
            //        - Metadatavalues
        }
    }

```

#### <a name="reading-multidimensional-metric-values"></a>讀取多維度度量值

對於多維度的計量，如果您想要讀取特定維度值的計量資料，就必須定義中繼資料篩選條件。

下列範例示範如何讀取支援多維度計量的計量資料：

```csharp
    public static async Task ReadStorageMetricValueTest()
    {
        // Resource ID for blob storage
        var resourceId = "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/blobServices/default";
        var subscriptionId = "<subscription-ID}";
        // How to identify Tenant ID, Application ID and Access Key: https://azure.microsoft.com/documentation/articles/resource-group-create-service-principal-portal/
        var tenantId = "<tenant-ID>";
        var applicationId = "<application-ID>";
        var accessKey = "<AccessKey>";

        MonitorManagementClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;

        Microsoft.Azure.Management.Monitor.Models.Response Response;

        string startDate = DateTime.Now.AddHours(-3).ToUniversalTime().ToString("o");
        string endDate = DateTime.Now.ToUniversalTime().ToString("o");
        string timeSpan = startDate + "/" + endDate;
        // It's applicable to define meta data filter when a metric support dimension
        // More conditions can be added with the 'or' and 'and' operators, example: BlobType eq 'BlockBlob' or BlobType eq 'PageBlob'
        ODataQuery<MetadataValue> odataFilterMetrics = new ODataQuery<MetadataValue>(
            string.Format("BlobType eq '{0}'", "BlockBlob"));

        Response = readOnlyClient.Metrics.List(
                        resourceUri: resourceId,
                        timespan: timeSpan,
                        interval: System.TimeSpan.FromHours(1),
                        metricnames: "BlobCapacity",
                        odataQuery: odataFilterMetrics,
                        aggregation: "Average",
                        resultType: ResultType.Data);

        foreach (var metric in Response.Value)
        {
            //Enumrate metric value
            //    Id
            //    Name
            //    Type
            //    Unit
            //    Timeseries
            //        - Data
            //        - Metadatavalues
        }
    }

```

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

#### <a name="list-the-metric-definition"></a>列出計量定義

您可以列出儲存體帳戶或 Blob 儲存體服務的度量定義。 使用 [Get-AzMetricDefinition](/powershell/module/az.monitor/get-azmetricdefinition) Cmdlet。

在此範例中，請將 `<resource-ID>` 預留位置取代為整個儲存體帳戶的資源識別碼或 Blob 儲存體服務的資源識別碼。  您可在 Azure 入口網站中儲存體帳戶的 [屬性] 頁面上找到這些資源識別碼。

```powershell
   $resourceId = "<resource-ID>"
   Get-AzMetricDefinition -ResourceId $resourceId
```

#### <a name="reading-metric-values"></a>讀取度量值

您可以讀取儲存體帳戶或 Blob 儲存體服務的帳戶層級度量值。 使用 [Get-AzMetric](/powershell/module/Az.Monitor/Get-AzMetric) Cmdlet。

```powershell
   $resourceId = "<resource-ID>"
   Get-AzMetric -ResourceId $resourceId -MetricNames "UsedCapacity" -TimeGrain 01:00:00
```

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

#### <a name="list-the-account-level-metric-definition"></a>列出帳戶層級的計量定義

您可以列出儲存體帳戶或 Blob 儲存體服務的度量定義。 使用 [az monitor metrics list-definitions](/cli/azure/monitor/metrics#az-monitor-metrics-list-definitions) 命令。
 
在此範例中，請將 `<resource-ID>` 預留位置取代為整個儲存體帳戶的資源識別碼或 Blob 儲存體服務的資源識別碼。 您可在 Azure 入口網站中儲存體帳戶的 [屬性] 頁面上找到這些資源識別碼。

```azurecli-interactive
   az monitor metrics list-definitions --resource <resource-ID>
```

#### <a name="read-account-level-metric-values"></a>讀取帳戶層級的計量值

您可以讀取儲存體帳戶或 Blob 儲存體服務的度量值。 使用 [az monitor metrics list](/cli/azure/monitor/metrics#az-monitor-metrics-list) 命令。

```azurecli-interactive
   az monitor metrics list --resource <resource-ID> --metric "UsedCapacity" --interval PT1H
```
### <a name="template"></a>[範本](#tab/template)

N/A。

---

## <a name="analyzing-logs"></a>分析記錄

您可以儲存體帳戶中的 Blob 形式、以事件資料形式，或透過 Log Analytics 查詢來存取資源記錄。

如需這些記錄中出現之欄位的詳細參考，請參閱 [Azure Blob 儲存體監視資料參考](monitor-blob-storage-reference.md)。

> [!NOTE]
> Azure 監視器中的 Azure 儲存體記錄處於公開預覽狀態，可在所有公用雲端區域中進行預覽測試。 此預覽可啟用 Blob (包括 Azure Data Lake Storage Gen2)、檔案、佇列、資料表、一般用途 v1 高階儲存體帳戶及一般用途 v2 儲存體帳戶的記錄。 不支援傳統儲存體帳戶。

只有在對服務端點提出要求時，才會建立記錄項目。 例如，如果儲存體帳戶在其 Blob 端點中有活動，而不是在其資料表或佇列端點中，則只會建立關於 Blob 服務的記錄。 Azure 儲存體記錄包含對儲存體服務之成功和失敗要求的詳細資訊。 這項資訊可用來監視個別要求，並診斷儲存體服務的問題。 系統會以最佳方式來記錄要求。

### <a name="log-authenticated-requests"></a>記錄已驗證的要求

 系統將記錄下列類型的驗證要求：

- 成功的要求
- 失敗的要求，包括逾時、節流、網路、授權和其他錯誤
- 使用共用存取簽章 (SAS) 或 OAuth 的要求，包括失敗和成功的要求
- 對分析資料 ( **$logs** 中的傳統記錄資料和 **$metric** 資料表中的類別計量資料) 的要求

Blob 儲存體服務本身所提出的要求（例如記錄建立或刪除）不會記錄下來。 如需已記錄資料的完整清單，請參閱[儲存體記錄的作業和狀態訊息](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages)及[儲存體記錄格式](monitor-blob-storage-reference.md)。

### <a name="log-anonymous-requests"></a>記錄匿名要求

 系統將記錄下列類型的匿名要求：

- 成功的要求
- 伺服器錯誤
- 用戶端與伺服器的逾時錯誤
- 失敗的 GET 要求，其錯誤碼為 304 (未修改)

系統不會記錄所有其他失敗的匿名要求。 如需已記錄資料的完整清單，請參閱[儲存體記錄的作業和狀態訊息](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages)及[儲存體記錄格式](monitor-blob-storage-reference.md)。

### <a name="accessing-logs-in-a-storage-account"></a>存取儲存體帳戶中的記錄

記錄會顯示為儲存在目標儲存體帳戶中容器的 Blob。 系統會以行分隔的 JSON 承載形式將資料收集並儲存在單一 Blob 內。 此 Blob 的名稱會遵循以下命名慣例：

`https://<destination-storage-account>.blob.core.windows.net/insights-logs-<storage-operation>/resourceId=/subscriptions/<subscription-ID>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<source-storage-account>/blobServices/default/y=<year>/m=<month>/d=<day>/h=<hour>/m=<minute>/PT1H.json`

以下是範例：

`https://mylogstorageaccount.blob.core.windows.net/insights-logs-storagewrite/resourceId=/subscriptions/`<br>`208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount/blobServices/default/y=2019/m=07/d=30/h=23/m=12/PT1H.json`

### <a name="accessing-logs-in-an-event-hub"></a>存取事件中樞內的記錄

傳送至事件中樞的記錄不會儲存為檔案，但您可確認事件中樞已收到記錄資訊。 在 Azure 入口網站中，移至您的事件中樞並確認 **內送郵件** 計數大於零。 

![稽核記錄](media/monitor-blob-storage/event-hub-log.png)

您可使用安全性資訊及事件管理和監視工具，存取及讀取傳送到事件中樞的記錄資料。 如需詳細資訊，請參閱[傳送至事件中樞的監視資料有何作用？](../../azure-monitor/platform/stream-monitoring-data-event-hubs.md)。

### <a name="accessing-logs-in-a-log-analytics-workspace"></a>存取 Log Analytics 工作區中的記錄

您可使用 Azure 監視器記錄查詢來存取傳送至 Log Analytics 工作區的記錄。

如需詳細資訊，請參閱[開始使用 Azure 監視器中的 Log Analytics](../../azure-monitor/log-query/log-analytics-tutorial.md)。

資料會儲存在 **StorageBlobLog** 資料表中。 Data Lake Storage Gen2 的記錄不會出現在專用資料表中。 這是因為 Data Lake Storage Gen2 不是服務。 這是您可以在儲存體帳戶中啟用的一組功能。 如果您已啟用這些功能，記錄檔將會繼續出現在 StorageBlobLogs 資料表中。 

#### <a name="sample-kusto-queries"></a>範例 Kusto 查詢

以下是一些您可以在 **記錄搜尋** 列中輸入的查詢，可協助您監視 Blob 儲存體。 這些查詢使用[新語言](../../azure-monitor/log-query/log-query-overview.md)。

> [!IMPORTANT]
> 當您從 [儲存體帳戶資源群組] 功能表選取 [ **記錄** ] 時，會開啟 Log Analytics，並將查詢範圍設定為目前的資源群組。 這表示記錄查詢只會包含該資源群組中的資料。 如果您想要執行包含來自其他 Azure 服務之其他資源或資料之資料的查詢，請從 [ **Azure 監視器**] 功能表中選取 [**記錄**]。 如需詳細資訊，請參閱 [Azure 監視器 Log Analytics 中的記錄查詢範圍和時間範圍](../../azure-monitor/log-query/scope.md)。

使用這些查詢可協助您監視 Azure 儲存體帳戶：

* 列出過去三天內最常見的 10 個錯誤。

    ```Kusto
    StorageBlobLogs
    | where TimeGenerated > ago(3d) and StatusText !contains "Success"
    | summarize count() by StatusText
    | top 10 by count_ desc
    ```
* 列出過去三天內導致最多錯誤的前 10 個作業。

    ```Kusto
    StorageBlobLogs
    | where TimeGenerated > ago(3d) and StatusText !contains "Success"
    | summarize count() by OperationName
    | top 10 by count_ desc
    ```
* 列出過去三天內最長端對端延遲的前 10 個作業。

    ```Kusto
    StorageBlobLogs
    | where TimeGenerated > ago(3d)
    | top 10 by DurationMs desc
    | project TimeGenerated, OperationName, DurationMs, ServerLatencyMs, ClientLatencyMs = DurationMs - ServerLatencyMs
    ```
* 列出過去三天內導致伺服器端節流錯誤的所有作業。

    ```Kusto
    StorageBlobLogs
    | where TimeGenerated > ago(3d) and StatusText contains "ServerBusy"
    | project TimeGenerated, OperationName, StatusCode, StatusText
    ```
* 列出過去三天內具有匿名存取權的所有要求。

    ```Kusto
    StorageBlobLogs
    | where TimeGenerated > ago(3d) and AuthenticationType == "Anonymous"
    | project TimeGenerated, OperationName, AuthenticationType, Uri
    ```
* 建立過去三天內所用作業的圓形圖。
    ```Kusto
    StorageBlobLogs
    | where TimeGenerated > ago(3d)
    | summarize count() by OperationName
    | sort by count_ desc 
    | render piechart
    ```
## <a name="faq"></a>常見問題集

**Azure 儲存體是否支援受控磁碟或非受控磁碟的計量？**

否。 Azure 計算支援磁碟的計量。 如需詳細資訊，請參閱[受控和非受控磁碟的每一磁碟計量](https://azure.microsoft.com/blog/per-disk-metrics-managed-disks/)。

## <a name="next-steps"></a>後續步驟

- 如需 Azure Blob 儲存體所建立之記錄和計量的參考，請參閱 [Azure blob 儲存體監視資料參考](monitor-blob-storage-reference.md)。
- 如需監視 Azure 資源的詳細資訊，請參閱[使用 Azure 監視器來監視 Azure 資源](../../azure-monitor/insights/monitor-azure-resource.md)。
- 如需有關計量移轉的詳細資訊，請參閱 [Azure 儲存體計量移轉](../common/storage-metrics-migration.md)。