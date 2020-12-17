---
title: 查詢存放區-適用於 PostgreSQL 的 Azure 資料庫-單一伺服器
description: 本文說明適用於 PostgreSQL 的 Azure 資料庫單一伺服器中的查詢存放區功能。
author: sunilagarwal
ms.author: sunila
ms.service: postgresql
ms.topic: conceptual
ms.date: 07/01/2020
ms.openlocfilehash: 5dff78989eef17f95d8b8dd108baafc53a3f761a
ms.sourcegitcommit: ad677fdb81f1a2a83ce72fa4f8a3a871f712599f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/17/2020
ms.locfileid: "97657017"
---
# <a name="monitor-performance-with-the-query-store"></a>使用查詢存放區監視效能

**適用于：** 適用於 PostgreSQL 的 Azure 資料庫-單一伺服器版本9.6 和更新版本

適用於 PostgreSQL 的 Azure 資料庫查詢存放區功能提供的方法可追蹤一段時間的查詢效能。 查詢存放區可協助您快速找到執行時間最長又最耗資源的查詢，簡化效能疑難排解。 查詢存放區會自動擷取查詢的歷程記錄和執行階段統計資料，並予以保留以供您檢閱。 依時間範圍區分資料，以便查看資料庫使用模式。 所有使用者、資料庫及查詢的資料都會儲存在適用於 PostgreSQL 的 Azure 資料庫執行個體中名為 **azure_sys** 的資料庫。

> [!IMPORTANT]
> 請勿修改 **azure_sys** 資料庫或其結構描述。 這樣做會造成查詢存放區與相關的效能功能無法正確運作。

## <a name="enabling-query-store"></a>啟用查詢存放區
查詢存放區是選擇加入的功能，因此預設不會在伺服器上啟用。 存放區是針對指定伺服器上的所有資料庫全域啟用或停用的，並無法個別開啟或關閉資料庫。

### <a name="enable-query-store-using-the-azure-portal"></a>使用 Azure 入口網站啟用查詢存放區
1. 登入 Azure 入口網站，然後選取適用於 PostgreSQL 的 Azure 資料庫伺服器。
2. 在功能表的 [設定] 區段中，選取 [伺服器參數]。
3. 搜尋 `pg_qs.query_capture_mode` 參數。
4. 將值設定為 `TOP` ，然後 **儲存**。

若要啟用查詢存放區中的等候統計資料： 
1. 搜尋 `pgms_wait_sampling.query_capture_mode` 參數。
1. 將值設定為 `ALL` ，然後 **儲存**。


或者，您可以使用 Azure CLI 來設定這些參數。
```azurecli-interactive
az postgres server configuration set --name pg_qs.query_capture_mode --resource-group myresourcegroup --server mydemoserver --value TOP
az postgres server configuration set --name pgms_wait_sampling.query_capture_mode --resource-group myresourcegroup --server mydemoserver --value ALL
```

請等候 20 分鐘，以讓第一批資料保存在 azure_sys 資料庫中。

## <a name="information-in-query-store"></a>查詢存放區中的資訊
查詢存放區有兩個存放區：
- 執行階段統計資料存放區，用於保存查詢執行統計資料資訊。
- 等候統計資料存放區，用於保存等候統計資料資訊。

使用查詢存放區的常見案例包括：
- 判斷查詢在指定時間範圍內的執行次數
- 跨時間範圍比較查詢的平均值行時間，查看大幅差異
- 識別在過去 X 小時中執行最久的查詢
- 識別等候資源的前 N 項查詢
- 了解特定查詢的等候性質

為了讓空間使用量降到最低，會經過一段固定且可設定的時間範圍，才彙總執行階段統計資料存放區的執行階段執行統計資料。 查詢這些查詢存放區檢視，即可看到這些存放區中的資訊。

## <a name="access-query-store-information"></a>存取查詢存放區資訊

查詢存放區的資料會儲存在 Postgres 伺服器的 azure_sys 資料庫中。 

下列查詢會傳回查詢存放區中的相關資訊：
```sql
SELECT * FROM query_store.qs_view; 
``` 

或者，此查詢可取得等候統計資料：
```sql
SELECT * FROM query_store.pgms_wait_sampling_view;
```

## <a name="finding-wait-queries"></a>尋找等候查詢
等候事件類型會依相似性，將不同的等候事件結合成貯體。 查詢存放區提供等候事件類型、特定的等候事件名稱，以及有問題的查詢。 能夠將此等候資訊與查詢執行階段相互關聯，表示您可以更深入了解查詢效能特性從何而來。

以下是如何查詢存放區中的等候統計資料，以更多深入了解工作負載的一些範例：

| **觀測** | **動作** |
|---|---|
|高鎖定等候數 | 查看受影響查詢的查詢文字，並找出目標實體。 查看查詢存放區，針對經常執行和/或持續時間很長的實體，尋找修改同一實體的其他查詢。 找出這些查詢之後，請考慮變更應用程式邏輯，改善並行存取，或使用限制較少的隔離等級。|
| 高緩衝區 IO 等候數 | 在查詢存放區中尋找實體讀取次數高的查詢。 如果與高 IO 等候數的查詢相符，請考慮對基礎實體引進索引，以執行搜尋，而不是掃描。 這可將查詢的 IO 額外負荷降到最低。 請在入口網站檢查伺服器的 **效能建議**，以查看是否有此伺服器的索引建議，可供將查詢最佳化。|
| 高記憶體等候數 | 找出查詢存放區中記憶體耗用量名列前茅的查詢。 這些查詢可能會進一步延遲受影響查詢的進度。 請在入口網站檢查伺服器的 **效能建議**，以查看是否有索引建議，可供將這些查詢最佳化。|

## <a name="configuration-options"></a>設定選項
啟用查詢存放區時，會以每 15 分鐘的彙總時間範圍儲存資料一次，每個範圍內最多可有 500 個相異的查詢。 

下列選項可用於設定查詢存放區參數。

| **參數** | **說明** | **預設值** | **Range**|
|---|---|---|---|
| pg_qs.query_capture_mode | 設定追蹤哪些陳述式。 | 無 | none、top、all |
| pg_qs.max_query_text_length | 設定可以儲存的最大查詢長度。 較長的查詢會遭截斷。 | 6000 | 100 - 10K |
| pg_qs.retention_period_in_days | 設定保留期限。 | 7 | 1 - 30 |
| pg_qs.track_utility | 設定是否要追蹤公用程式命令 | on | on、off |

下列選項特別適用於等候統計資料。

| **參數** | **說明** | **預設值** | **Range**|
|---|---|---|---|
| pgms_wait_sampling.query_capture_mode | 設定追蹤等候統計資料的哪些陳述式。 | 無 | none、all|
| Pgms_wait_sampling.history_period | 設定以毫秒為單位的等候事件取樣頻率。 | 100 | 1-600000 |

> [!NOTE] 
> **pg_qs.query_capture_mode** 已取代 **pgms_wait_sampling.query_capture_mode**。 若 pg_qs.query_capture_mode 是 NONE，則 pgms_wait_sampling.query_capture_mode 設定沒有影響。


使用 [Azure 入口網站](howto-configure-server-parameters-using-portal.md)或 [Azure CLI](howto-configure-server-parameters-using-cli.md)，為參數取得或設定不同的值。

## <a name="views-and-functions"></a>檢視和函式
使用下列檢視和函式來檢視和管理查詢存放區。 PostgreSQL 公用角色中的任何人都可以使用這些檢視，查看查詢存放區中的資料。 這些檢視僅適用於 **azure_sys** 資料庫。

移除常值和常數之後，查看查詢結構，其會呈現標準化。 如果兩個查詢完全相同 (但常值除外)，則兩者會有相同的雜湊碼。

### <a name="query_storeqs_view"></a>query_store.qs_view
此檢視會傳回查詢存放區中的所有資料。 不同的資料庫識別碼、使用者識別碼及查詢識別碼都會自成一資料列。 

|**名稱**   |**型別** | **參考**  | **描述**|
|---|---|---|---|
|runtime_stats_entry_id |BIGINT | | 來自 runtime_stats_entries 資料表的識別碼|
|user_id    |oid    |pg_authid.oid  |執行陳述式的使用者物件識別 (OID)|
|db_id  |oid    |pg_database.oid    |在其中執行陳述式的資料庫物件識別 (OID)|
|query_id   |BIGINT  || 從陳述式的剖析樹狀結構計算的內部雜湊碼|
|query_sql_text |Varchar(10000)  || 代表性陳述式的文字。 結構相同的不同查詢會群集在一起；此文字就式叢集中第一個查詢的文字。|
|plan_id    |BIGINT |   |對應到此查詢的方案識別碼，尚無法使用|
|start_time |timestamp  ||  依時間貯體彙總的查詢 - 根據預設，貯體的時間範圍為 15 分鐘。 這是和此查詢的時間貯體對應的開始時間。|
|end_time   |timestamp  ||  和此查詢的時間貯體對應的結束時間。|
|calls  |BIGINT  || 查詢執行的次數|
|total_time |雙精度   ||  查詢總執行時間 (以毫秒為單位)|
|min_time   |雙精度   ||  查詢最短執行時間 (以毫秒為單位)|
|max_time   |雙精度   ||  查詢最長執行時間 (以毫秒為單位)|
|mean_time  |雙精度   ||  查詢平均執行時間 (以毫秒為單位)|
|stddev_time|   雙精度    ||  查詢執行時間標準差 (以毫秒為單位) |
|rows   |BIGINT ||  由陳述式擷取或受其影響的資料列總數|
|shared_blks_hit|   BIGINT  ||  依據陳述式的共用區塊快取點擊總次數|
|shared_blks_read|  BIGINT  ||  由陳述式讀取的共用區塊總數|
|shared_blks_dirtied|   BIGINT   || 由陳述式變動的共用區塊總數 |
|shared_blks_written|   BIGINT  ||  由陳述式寫入的共用區塊總數|
|local_blks_hit|    BIGINT ||   依據陳述式的本機區塊快取點擊總次數|
|local_blks_read|   BIGINT   || 由陳述式讀取的本機區塊總數|
|local_blks_dirtied|    BIGINT  ||  由陳述式變動的本機區塊總數|
|local_blks_written|    BIGINT  ||  由陳述式寫入的本機區塊總數|
|temp_blks_read |BIGINT  || 由陳述式讀取的暫存區塊總數|
|temp_blks_written| BIGINT   || 由陳述式寫入的暫存區塊總數|
|blk_read_time  |雙精度    || 陳述式讀取區塊花費的總時間 (以毫秒為單位) (若已啟用 track_io_timing 的話，否則為零)|
|blk_write_time |雙精度    || 陳述式寫入區塊花費的總時間 (以毫秒為單位) (若已啟用 track_io_timing，否則為零)|
    
### <a name="query_storequery_texts_view"></a>query_store.query_texts_view
此檢視會傳回查詢存放區中的查詢文字資料。 不同的 query_text 都會自成一資料列。

| **名稱** | **型別** | **說明** |
|--|--|--|
| query_text_id | BIGINT | query_texts 資料表識別碼 |
| query_sql_text | Varchar(10000) | 代表性陳述式的文字。 結構相同的不同查詢會群集在一起；此文字就式叢集中第一個查詢的文字。 |

### <a name="query_storepgms_wait_sampling_view"></a>query_store.pgms_wait_sampling_view
此檢視會傳回查詢存放區中的等候事件資料。 不同的資料庫識別碼、使用者識別碼、查詢識別碼及事件都會自成一資料列。

| **名稱** | **型別** | **參考** | **描述** |
|--|--|--|--|
| user_id | oid | pg_authid.oid | 執行陳述式的使用者物件識別 (OID) |
| db_id | oid | pg_database.oid | 在其中執行陳述式的資料庫物件識別 (OID) |
| query_id | BIGINT |  | 從陳述式的剖析樹狀結構計算的內部雜湊碼 |
| event_type | 文字 |  | 後端等候中事件的類型 |
| event | 文字 |  | 如果後端目前正在等候，為該等候事件的名稱 |
| calls | 整數 |  | 擷取到相同事件的次數 |

### <a name="functions"></a>函數

Query_store.qs_reset() 傳回 void

`qs_reset` 捨棄查詢存放區至今收集到的所有統計資料。 只有伺服器管理員角色可以執行此函式。

Query_store.staging_data_reset() 傳回 void

`staging_data_reset` 捨棄查詢存放區在記憶體中收集的統計資料 (亦即，記憶體中的資料尚未排清至資料庫)。 只有伺服器管理員角色可以執行此函式。


## <a name="azure-monitor"></a>Azure 監視器
適用於 PostgreSQL 的 Azure 資料庫已與 [Azure 監視器診斷設定](../azure-monitor/platform/diagnostic-settings.md)整合。 診斷設定可讓您將 JSON 格式的 Postgres 記錄傳送至 [Azure 監視器記錄](../azure-monitor/log-query/log-query-overview.md) ，以進行分析和警示、串流的事件中樞，以及用於封存的 Azure 儲存體。

>[!IMPORTANT]
> 的此診斷功能僅適用于一般用途和記憶體優化定價層。

### <a name="configure-diagnostic-settings"></a>設定診斷設定
您可以使用 Azure 入口網站、CLI、REST API 和 PowerShell 來啟用 Postgres 伺服器的診斷設定。 要設定的記錄類別為 **QueryStoreRuntimeStatistics** 和 **QueryStoreWaitStatistics**。 

若要使用 Azure 入口網站啟用資源記錄：

1. 在入口網站中，移至 Postgres 伺服器的導覽功能表中的 [診斷設定]。
2. 選取 [新增診斷設定]。
3. 為此設定命名。
4. 選取您慣用的端點 (儲存體帳戶、事件中樞、log analytics) 。
5. 選取記錄類型 **QueryStoreRuntimeStatistics** 和 **QueryStoreWaitStatistics**。
6. 儲存您的設定。

若要使用 PowerShell、CLI 或 REST API 來啟用這項設定，請造訪 [診斷設定文章](../azure-monitor/platform/diagnostic-settings.md)。

### <a name="json-log-format"></a>JSON 記錄格式
下表描述兩個記錄類型的欄位。 視您選擇的輸出端點而定，所含欄位及其出現順序可能會有所不同。

#### <a name="querystoreruntimestatistics"></a>QueryStoreRuntimeStatistics
|**欄位** | **描述** |
|---|---|
| TimeGenerated [UTC] | 以 UTC 記錄記錄時的時間戳記 |
| ResourceId | Postgres 伺服器的 Azure 資源 URI |
| 類別 | `QueryStoreRuntimeStatistics` |
| OperationName | `QueryStoreRuntimeStatisticsEvent` |
| LogicalServerName_s | Postgres 伺服器名稱 | 
| runtime_stats_entry_id_s | 來自 runtime_stats_entries 資料表的識別碼 |
| user_id_s | 執行陳述式的使用者物件識別 (OID) |
| db_id_s | 在其中執行陳述式的資料庫物件識別 (OID) |
| query_id_s | 從陳述式的剖析樹狀結構計算的內部雜湊碼 |
| end_time_s | 對應到這個專案的時間值區的結束時間 |
| calls_s | 查詢執行的次數 |
| total_time_s | 查詢總執行時間 (以毫秒為單位) |
| min_time_s | 查詢最短執行時間 (以毫秒為單位) |
| max_time_s | 查詢最長執行時間 (以毫秒為單位) |
| mean_time_s | 查詢平均執行時間 (以毫秒為單位) |
| ResourceGroup | 資源群組 | 
| SubscriptionId | 您的訂用帳戶識別碼 |
| ResourceProvider | `Microsoft.DBForPostgreSQL` | 
| 資源 | Postgres 伺服器名稱 |
| ResourceType | `Servers` | 


#### <a name="querystorewaitstatistics"></a>QueryStoreWaitStatistics
|**欄位** | **描述** |
|---|---|
| TimeGenerated [UTC] | 以 UTC 記錄記錄時的時間戳記 |
| ResourceId | Postgres 伺服器的 Azure 資源 URI |
| 類別 | `QueryStoreWaitStatistics` |
| OperationName | `QueryStoreWaitEvent` |
| user_id_s | 執行陳述式的使用者物件識別 (OID) |
| db_id_s | 在其中執行陳述式的資料庫物件識別 (OID) |
| query_id_s | 查詢的內部雜湊碼 |
| calls_s | 擷取到相同事件的次數 |
| event_type_s | 後端等候中事件的類型 |
| event_s | 如果後端目前正在等候，則為等候事件名稱 |
| start_time_t | 事件開始時間 |
| end_time_s | 事件結束時間 | 
| LogicalServerName_s | Postgres 伺服器名稱 | 
| ResourceGroup | 資源群組 | 
| SubscriptionId | 您的訂用帳戶識別碼 |
| ResourceProvider | `Microsoft.DBForPostgreSQL` | 
| 資源 | Postgres 伺服器名稱 |
| ResourceType | `Servers` | 

## <a name="limitations-and-known-issues"></a>限制與已知問題
- 如果 PostgreSQL 伺服器開啟 default_transaction_read_only 參數，查詢存放區會無法擷取資料。
- 如果遇到長時間的 Unicode 查詢 (> = 6000 個位元組)，查詢存放區功能可能會中斷。
- [讀取複本](concepts-read-replicas.md) 會從主伺服器複寫查詢存放區資料。 這表示讀取複本的查詢存放區不會提供有關在讀取複本上執行之查詢的統計資料。


## <a name="next-steps"></a>後續步驟
- 深入了解[查詢存放區特別實用的案例](concepts-query-store-scenarios.md)。
- 深入了解[使用查詢存放區的最佳做法](concepts-query-store-best-practices.md)。
