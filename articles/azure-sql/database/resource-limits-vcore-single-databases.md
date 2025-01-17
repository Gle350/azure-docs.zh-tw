---
title: 單一資料庫 vCore 資源限制
description: 此頁面描述 Azure SQL Database 中單一資料庫的一些常見 vCore 資源限制。
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: reference
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 10/15/2020
ms.openlocfilehash: 4ffe663c1a1651891af5f6e65ee231cbe3e8d650
ms.sourcegitcommit: 6d6030de2d776f3d5fb89f68aaead148c05837e2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/05/2021
ms.locfileid: "97882287"
---
# <a name="resource-limits-for-single-databases-using-the-vcore-purchasing-model"></a>使用虛擬核心購買模型的單一資料庫資源限制
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

本文提供使用 vCore 購買模型 Azure SQL Database 中單一資料庫的詳細資源限制。

如需伺服器上單一資料庫的 DTU 購買模型限制，請參閱 [伺服器上的資源限制總覽](resource-limits-logical-server.md)。

您可以使用 [Azure 入口網站](single-database-manage.md#the-azure-portal)、 [transact-sql](single-database-manage.md#transact-sql-t-sql)、 [PowerShell](single-database-manage.md#powershell)、 [Azure CLI](single-database-manage.md#the-azure-cli)或 [REST API](single-database-manage.md#rest-api)，為單一資料庫設定服務層、計算大小 (服務目標) 和儲存體數量。

> [!IMPORTANT]
> 如需調整指導方針和考慮，請參閱 [調整單一資料庫](single-database-scale.md)。

## <a name="general-purpose---serverless-compute---gen5"></a>一般用途-無伺服器計算-第5代

[無伺服器計算層](serverless-tier-overview.md)目前僅適用于第5代硬體。

### <a name="gen5-compute-generation-part-1"></a>第5代計算世代 (第1部) 

|計算大小 (服務目標)|GP_S_Gen5_1|GP_S_Gen5_2|GP_S_Gen5_4|GP_S_Gen5_6|GP_S_Gen5_8|
|:--- | --: |--: |--: |--: |--: |
|計算世代|Gen5|Gen5|Gen5|Gen5|Gen5|
|最小值-最大值虛擬核心|0.5-1|0.5-2|0.5-4|0.75-6|1.0-8|
|最小值-最大記憶體 (GB) |2.02-3|2.05-6|2.10-12|2.25-18|3.00-24|
|最小值-最大自動暫停延遲 (分鐘) |60-10080|60-10080|60-10080|60-10080|60-10080|
|資料行存放區支援|是|是|是|是|是|
|OLTP 記憶體內部儲存體 (GB)|N/A|N/A|N/A|N/A|N/A|
|資料大小上限 (GB)|512|1024|1024|1024|1536|
|記錄大小上限 (GB)|154|307|307|307|461|
|TempDB 資料大小上限 (GB) |32|64|128|192|256|
|儲存體類型|遠端 SSD|遠端 SSD|遠端 SSD|遠端 SSD|遠端 SSD|
|IO 延遲 (大約)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|
|最大資料 IOPS *|320|640|1280|1920|2560|
|最大記錄速率 (MBps) |4.5|9|18|27|36|
|並行背景工作 (要求) 數上限|75|150|300|450|600|
|並行工作階段數上限|30,000|30,000|30,000|30,000|30,000|
|複本數目|1|1|1|1|1|
|多重 AZ|N/A|N/A|N/A|N/A|N/A|
|讀取向外延展|N/A|N/A|N/A|N/A|N/A|
|內含備份儲存體|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|

\* IO 大小的最大值，範圍介於 8 KB 到 64 KB 之間。 實際的 IOPS 取決於工作負載。 如需詳細資訊，請參閱 [資料 IO 治理](resource-limits-logical-server.md#resource-governance)。

### <a name="gen5-compute-generation-part-2"></a>第5代計算世代 (第2部分) 

|計算大小 (服務目標)|GP_S_Gen5_10|GP_S_Gen5_12|GP_S_Gen5_14|GP_S_Gen5_16|
|:--- | --: |--: |--: |--: |
|計算世代|Gen5|Gen5|Gen5|Gen5|
|最小值-最大值虛擬核心|1.25-10|1.50-12|1.75-14|2.00-16|
|最小值-最大記憶體 (GB) |3.75-30|4.50-36|5.25-42|6.00-48|
|最小值-最大自動暫停延遲 (分鐘) |60-10080|60-10080|60-10080|60-10080|
|資料行存放區支援|是|是|是|是|
|OLTP 記憶體內部儲存體 (GB)|N/A|N/A|N/A|N/A|
|資料大小上限 (GB)|1536|3072|3072|3072|
|記錄大小上限 (GB)|461|461|461|922|
|TempDB 資料大小上限 (GB) |320|384|448|512|
|儲存體類型|遠端 SSD|遠端 SSD|遠端 SSD|遠端 SSD|
|IO 延遲 (大約)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|
|最大資料 IOPS *|3200|3840|4480|5120|
|最大記錄速率 (MBps) |36|36|36|36|
|並行背景工作 (要求) 數上限|750|900|1050|1200|
|並行工作階段數上限|30,000|30,000|30,000|30,000|
|複本數目|1|1|1|1|
|多重 AZ|N/A|N/A|N/A|N/A|
|讀取向外延展|N/A|N/A|N/A|N/A|
|內含備份儲存體|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|

\* IO 大小的最大值，範圍介於 8 KB 到 64 KB 之間。 實際的 IOPS 取決於工作負載。 如需詳細資訊，請參閱 [資料 IO 治理](resource-limits-logical-server.md#resource-governance)。

### <a name="gen5-compute-generation-part-3"></a>第5代計算世代 (第3部) 

|計算大小 (服務目標)|GP_S_Gen5_18|GP_S_Gen5_20|GP_S_Gen5_24|GP_S_Gen5_32|GP_S_Gen5_40|
|:--- | --: |--: |--: |--: |--:|
|計算世代|Gen5|Gen5|Gen5|Gen5|Gen5|
|最小值-最大值虛擬核心|2.25-18|2.5-20|3-24|4-32|5-40|
|最小值-最大記憶體 (GB) |6.75-54|7.5-60|9-72|12-96|15-120|
|最小值-最大自動暫停延遲 (分鐘) |60-10080|60-10080|60-10080|60-10080|60-10080|
|資料行存放區支援|是|是|是|是|是|
|OLTP 記憶體內部儲存體 (GB)|N/A|N/A|N/A|N/A|N/A|
|資料大小上限 (GB)|3072|3072|4096|4096|4096|
|記錄大小上限 (GB)|922|922|1024|1024|1024|
|TempDB 資料大小上限 (GB) |576|640|768|1024|1280|
|儲存體類型|遠端 SSD|遠端 SSD|遠端 SSD|遠端 SSD|遠端 SSD|
|IO 延遲 (大約)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|
|最大資料 IOPS *|5760|6400|7680|10240|12800|
|最大記錄速率 (MBps) |36|36|36|36|36|
|並行背景工作 (要求) 數上限|1350|1500|1800|2400|3000|
|並行工作階段數上限|30,000|30,000|30,000|30,000|30,000|
|複本數目|1|1|1|1|1|
|多重 AZ|N/A|N/A|N/A|N/A|N/A|
|讀取向外延展|N/A|N/A|N/A|N/A|N/A|
|內含備份儲存體|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|

\* IO 大小的最大值，範圍介於 8 KB 到 64 KB 之間。 實際的 IOPS 取決於工作負載。 如需詳細資訊，請參閱 [資料 IO 治理](resource-limits-logical-server.md#resource-governance)。


## <a name="hyperscale---provisioned-compute---gen4"></a>超大規模-布建的計算-第4代

### <a name="gen4-compute-generation-part-1"></a>第4代計算世代 (第1部) 

|計算大小 (服務目標)|HS_Gen4_1|HS_Gen4_2|HS_Gen4_3|HS_Gen4_4|HS_Gen4_5|HS_Gen4_6|
|:--- | --: |--: |--: |---: | --: |--: |
|計算世代|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|虛擬核心|1|2|3|4|5|6|
|記憶體 (GB)|7|14|21|28|35|42|
|[RBPEX](service-tier-hyperscale.md#compute) 大小|3倍記憶體|3倍記憶體|3倍記憶體|3倍記憶體|3倍記憶體|3倍記憶體|
|資料行存放區支援|是|是|是|是|是|是|
|OLTP 記憶體內部儲存體 (GB)|N/A|N/A|N/A|N/A|N/A|N/A|
|資料大小上限 (TB)|100 |100 |100 |100 |100 |100|
|記錄大小上限 (TB)|無限制 |無限制 |無限制 |無限制 |無限制 |無限制 |
|TempDB 資料大小上限 (GB) |32|64|96|128|160|192|
|儲存體類型| [附注1](#notes) |[附注1](#notes)|[附注1](#notes) |[附注1](#notes) |[附注1](#notes) |[附注1](#notes) |
|最大本機 SSD IOPS *|4000 |8000 |12000 |16000 |20000 |24000 |
|最大記錄速率 (MBps) |100 |100 |100 |100 |100 |100 |
|IO 延遲 (大約)|[附注2](#notes)|[附注2](#notes)|[附注2](#notes)|[附注2](#notes)|[附注2](#notes)|[附注2](#notes)|
|並行背景工作 (要求) 數上限|200|400|600|800|1000|1200|
|並行工作階段數上限|30,000|30,000|30,000|30,000|30,000|30,000|
|您應該|0-4|0-4|0-4|0-4|0-4|0-4|
|多重 AZ|N/A|N/A|N/A|N/A|N/A|N/A|
|讀取向外延展|是|是|是|是|是|是|
|備份儲存體保留期|7 天|7 天|7 天|7 天|7 天|7 天|
|||

\* 除了本機 SSD IO，工作負載會使用遠端 [頁面伺服器](service-tier-hyperscale.md#page-server) io。 有效的 IOPS 將視工作負載而定。 如需詳細資訊，請參閱 [資料 Io 治理](resource-limits-logical-server.md#resource-governance)和 [資源使用量統計資料中的資料 io](hyperscale-performance-diagnostics.md#data-io-in-resource-utilization-statistics)。

### <a name="gen4-compute-generation-part-2"></a>第4代計算世代 (第2部分) 

|計算大小 (服務目標)|HS_Gen4_7|HS_Gen4_8|HS_Gen4_9|HS_Gen4_10|HS_Gen4_16|HS_Gen4_24|
|:--- | ---: |--: |--: | --: |--: |--: |
|計算世代|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|虛擬核心|7|8|9|10|16|24|
|記憶體 (GB)|49|56|63|70|112|159.5|
|[RBPEX](service-tier-hyperscale.md#compute) 大小|3倍記憶體|3倍記憶體|3倍記憶體|3倍記憶體|3倍記憶體|3倍記憶體|
|資料行存放區支援|是|是|是|是|是|是|
|OLTP 記憶體內部儲存體 (GB)|N/A|N/A|N/A|N/A|N/A|N/A|
|資料大小上限 (TB)|100 |100 |100 |100 |100 |100 |
|記錄大小上限 (TB)|無限制 |無限制 |無限制 |無限制 |無限制 |無限制 |
|TempDB 資料大小上限 (GB) |224|256|288|320|512|768|
|儲存體類型| [附注1](#notes) |[附注1](#notes) |[附注1](#notes) |[附注1](#notes) |[附注1](#notes) |[附注1](#notes) |
|最大本機 SSD IOPS *|28000 |32000 |36000 |40000 |64000 |76800 |
|最大記錄速率 (MBps) |100 |100 |100 |100 |100 |100 |
|IO 延遲 (大約)|[附注2](#notes)|[附注2](#notes)|[附注2](#notes)|[附注2](#notes)|[附注2](#notes)|[附注2](#notes)|
|並行背景工作 (要求) 數上限|1400|1600|1800|2000|3200|4800|
|並行工作階段數上限|30,000|30,000|30,000|30,000|30,000|30,000|
|您應該|0-4|0-4|0-4|0-4|0-4|0-4|
|多重 AZ|N/A|N/A|N/A|N/A|N/A|N/A|
|讀取向外延展|是|是|是|是|是|是|
|備份儲存體保留期|7 天|7 天|7 天|7 天|7 天|7 天|
|||

\* 除了本機 SSD IO，工作負載會使用遠端 [頁面伺服器](service-tier-hyperscale.md#page-server) io。 有效的 IOPS 將視工作負載而定。 如需詳細資訊，請參閱 [資料 Io 治理](resource-limits-logical-server.md#resource-governance)和 [資源使用量統計資料中的資料 io](hyperscale-performance-diagnostics.md#data-io-in-resource-utilization-statistics)。

## <a name="hyperscale---provisioned-compute---gen5"></a>超大規模-布建的計算-第5代

### <a name="gen5-compute-generation-part-1"></a>第5代計算世代 (第1部) 

|計算大小 (服務目標)|HS_Gen5_2|HS_Gen5_4|HS_Gen5_6|HS_Gen_8|HS_Gen5_10|HS_Gen5_12|HS_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |
|計算世代|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|虛擬核心|2|4|6|8|10|12|14|
|記憶體 (GB)|10.4|20.8|31.1|41.5|51.9|62.3|72.7|
|[RBPEX](service-tier-hyperscale.md#compute) 大小|3倍記憶體|3倍記憶體|3倍記憶體|3倍記憶體|3倍記憶體|3倍記憶體|3倍記憶體|
|資料行存放區支援|是|是|是|是|是|是|是|
|OLTP 記憶體內部儲存體 (GB)|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|資料大小上限 (TB)|100 |100 |100 |100 |100 |100 |100|
|記錄大小上限 (TB)|無限制 |無限制 |無限制 |無限制 |無限制 |無限制 |無限制 |
|TempDB 資料大小上限 (GB) |64|128|192|256|320|384|448|
|儲存體類型| [附注1](#notes) |[附注1](#notes)|[附注1](#notes) |[附注1](#notes) |[附注1](#notes) |[附注1](#notes) |[附注1](#notes) |
|最大本機 SSD IOPS *|8000 |16000 |24000 |32000 |40000 |48000 |56000 |
|最大記錄速率 (MBps) |100 |100 |100 |100 |100 |100 |100 |
|IO 延遲 (大約)|[附注2](#notes)|[附注2](#notes)|[附注2](#notes)|[附注2](#notes)|[附注2](#notes)|[附注2](#notes)|[附注2](#notes)|
|並行背景工作 (要求) 數上限|200|400|600|800|1000|1200|1400|
|並行工作階段數上限|30,000|30,000|30,000|30,000|30,000|30,000|30,000|
|您應該|0-4|0-4|0-4|0-4|0-4|0-4|0-4|
|多重 AZ|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|讀取向外延展|是|是|是|是|是|是|是|
|備份儲存體保留期|7 天|7 天|7 天|7 天|7 天|7 天|7 天|
|||

\* 除了本機 SSD IO，工作負載會使用遠端 [頁面伺服器](service-tier-hyperscale.md#page-server) io。 有效的 IOPS 將視工作負載而定。 如需詳細資訊，請參閱 [資料 Io 治理](resource-limits-logical-server.md#resource-governance)和 [資源使用量統計資料中的資料 io](hyperscale-performance-diagnostics.md#data-io-in-resource-utilization-statistics)。

### <a name="gen5-compute-generation-part-2"></a>第5代計算世代 (第2部分) 

|計算大小 (服務目標)|HS_Gen5_16|HS_Gen5_18|HS_Gen5_20|HS_Gen_24|HS_Gen5_32|HS_Gen5_40|HS_Gen5_80|
|:--- | --: |--: |--: |--: |---: |--: |--: |
|計算世代|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|虛擬核心|16|18|20|24|32|40|80|
|記憶體 (GB)|83|93.4|103.8|124.6|166.1|207.6|415.2|
|[RBPEX](service-tier-hyperscale.md#compute) 大小|3倍記憶體|3倍記憶體|3倍記憶體|3倍記憶體|3倍記憶體|3倍記憶體|3倍記憶體|
|資料行存放區支援|是|是|是|是|是|是|是|
|OLTP 記憶體內部儲存體 (GB)|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|資料大小上限 (TB)|100 |100 |100 |100 |100 |100 |100 |
|記錄大小上限 (TB)|無限制 |無限制 |無限制 |無限制 |無限制 |無限制 |無限制 |
|TempDB 資料大小上限 (GB) |512|576|640|768|1024|1280|2560|
|儲存體類型| [附注1](#notes) |[附注1](#notes)|[附注1](#notes)|[附注1](#notes) |[附注1](#notes) |[附注1](#notes) |[附注1](#notes) |
|最大本機 SSD IOPS *|64000 |72000 |80000 |96000 |160000 |192000 |204800 |
|最大記錄速率 (MBps) |100 |100 |100 |100 |100 |100 |100 |
|IO 延遲 (大約)|[附注2](#notes)|[附注2](#notes)|[附注2](#notes)|[附注2](#notes)|[附注2](#notes)|[附注2](#notes)|[附注2](#notes)|
|並行背景工作 (要求) 數上限|1600|1800|2000|2400|3200|4000|8000|
|並行工作階段數上限|30,000|30,000|30,000|30,000|30,000|30,000|30,000|
|您應該|0-4|0-4|0-4|0-4|0-4|0-4|0-4|
|多重 AZ|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|讀取向外延展|是|是|是|是|是|是|是|
|備份儲存體保留期|7 天|7 天|7 天|7 天|7 天|7 天|7 天|
|||

\* 除了本機 SSD IO，工作負載會使用遠端 [頁面伺服器](service-tier-hyperscale.md#page-server) io。 有效的 IOPS 將視工作負載而定。 如需詳細資訊，請參閱 [資料 Io 治理](resource-limits-logical-server.md#resource-governance)和 [資源使用量統計資料中的資料 io](hyperscale-performance-diagnostics.md#data-io-in-resource-utilization-statistics)。

#### <a name="notes"></a>注意

**附注 1**：超大規模是具有個別計算和儲存元件的多層式架構： [超大規模服務層架構](service-tier-hyperscale.md#distributed-functions-architecture)

**附注 2**：本機計算複本 SSD 上資料的延遲為1-2 毫秒，可快取最常使用的資料頁。 從頁面伺服器抓取資料的延遲較高。

## <a name="general-purpose---provisioned-compute---gen4"></a>一般用途-布建的計算-第4代

> [!IMPORTANT]
> 澳大利亞東部或巴西南部區域不再支援新的第4代資料庫。

### <a name="gen4-compute-generation-part-1"></a>第4代計算世代 (第1部) 

|計算大小 (服務目標)|GP_Gen4_1|GP_Gen4_2|GP_Gen4_3|GP_Gen4_4|GP_Gen4_5|GP_Gen4_6
|:--- | --: |--: |--: |--: |--: |--: |
|計算世代|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|虛擬核心|1|2|3|4|5|6|
|記憶體 (GB)|7|14|21|28|35|42|
|資料行存放區支援|是|是|是|是|是|是|
|OLTP 記憶體內部儲存體 (GB)|N/A|N/A|N/A|N/A|N/A|N/A|
|資料大小上限 (GB)|1024|1024|1536|1536|1536|3072|
|記錄大小上限 (GB)|307|307|461|461|461|922|
|TempDB 資料大小上限 (GB) |32|64|96|128|160|192|
|儲存體類型|遠端 SSD|遠端 SSD|遠端 SSD|遠端 SSD|遠端 SSD|遠端 SSD|
|IO 延遲 (大約)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|
|最大資料 IOPS *|320|640|960|1280|1600|1920|
|最大記錄速率 (MBps) |4.5|9|13.5|18|22.5|27|
|並行背景工作 (要求) 數上限|200|400|600|800|1000|1200|
|並行工作階段數上限|30,000|30,000|30,000|30,000|30,000|30,000|
|複本數目|1|1|1|1|1|1|
|多重 AZ|N/A|N/A|N/A|N/A|N/A|N/A|
|讀取向外延展|N/A|N/A|N/A|N/A|N/A|N/A|
|內含備份儲存體|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|

\* IO 大小的最大值，範圍介於 8 KB 到 64 KB 之間。 實際的 IOPS 取決於工作負載。 如需詳細資訊，請參閱 [資料 IO 治理](resource-limits-logical-server.md#resource-governance)。

### <a name="gen4-compute-generation-part-2"></a>第4代計算世代 (第2部分) 

|計算大小 (服務目標)|GP_Gen4_7|GP_Gen4_8|GP_Gen4_9|GP_Gen4_10|GP_Gen4_16|GP_Gen4_24
|:--- | --: |--: |--: |--: |--: |--: |
|計算世代|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|虛擬核心|7|8|9|10|16|24|
|記憶體 (GB)|49|56|63|70|112|159.5|
|資料行存放區支援|是|是|是|是|是|是|
|OLTP 記憶體內部儲存體 (GB)|N/A|N/A|N/A|N/A|N/A|N/A|
|資料大小上限 (GB)|3072|3072|3072|3072|4096|4096|
|記錄大小上限 (GB)|922|922|922|922|1229|1229|
|TempDB 資料大小上限 (GB) |224|256|288|320|512|768|
|儲存體類型|遠端 SSD|遠端 SSD|遠端 SSD|遠端 SSD|遠端 SSD|遠端 SSD|
|IO 延遲 (大約)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)
|最大資料 IOPS *|2240|2560|2880|3200|5120|7680|
|最大記錄速率 (MBps) |31.5|36|36|36|36|36|
|並行背景工作 (要求) 數上限|1400|1600|1800|2000|3200|4800|
|並行工作階段數上限|30,000|30,000|30,000|30,000|30,000|30,000|
|複本數目|1|1|1|1|1|1|
|多重 AZ|N/A|N/A|N/A|N/A|N/A|N/A|
|讀取向外延展|N/A|N/A|N/A|N/A|N/A|N/A|
|內含備份儲存體|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|

\* IO 大小的最大值，範圍介於 8 KB 到 64 KB 之間。 實際的 IOPS 取決於工作負載。 如需詳細資訊，請參閱 [資料 IO 治理](resource-limits-logical-server.md#resource-governance)。

## <a name="general-purpose---provisioned-compute---gen5"></a>一般用途-布建的計算-第5代

### <a name="gen5-compute-generation-part-1"></a>第5代計算世代 (第1部) 

|計算大小 (服務目標)|GP_Gen5_2|GP_Gen5_4|GP_Gen5_6|GP_Gen5_8|GP_Gen5_10|GP_Gen5_12|GP_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|計算世代|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|虛擬核心|2|4|6|8|10|12|14|
|記憶體 (GB)|10.4|20.8|31.1|41.5|51.9|62.3|72.7|
|資料行存放區支援|是|是|是|是|是|是|是|
|OLTP 記憶體內部儲存體 (GB)|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|資料大小上限 (GB)|1024|1024|1536|1536|1536|3072|3072|
|記錄大小上限 (GB)|307|307|461|461|461|922|922|
|TempDB 資料大小上限 (GB) |64|128|192|256|320|384|384|
|儲存體類型|遠端 SSD|遠端 SSD|遠端 SSD|遠端 SSD|遠端 SSD|遠端 SSD|遠端 SSD|
|IO 延遲 (大約)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|
|最大資料 IOPS *|640|1280|1920|2560|3200|3840|4480|
|最大記錄速率 (MBps) |9|18|27|36|36|36|36|
|並行背景工作 (要求) 數上限|200|400|600|800|1000|1200|1400|
|並行工作階段數上限|30,000|30,000|30,000|30,000|30,000|30,000|30,000|
|複本數目|1|1|1|1|1|1|1|
|多重 AZ|[提供預覽](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[提供預覽](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[提供預覽](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[提供預覽](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[提供預覽](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[提供預覽](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[提供預覽](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|
|讀取向外延展|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|內含備份儲存體|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|

\* IO 大小的最大值，範圍介於 8 KB 到 64 KB 之間。 實際的 IOPS 取決於工作負載。 如需詳細資訊，請參閱 [資料 IO 治理](resource-limits-logical-server.md#resource-governance)。

### <a name="gen5-compute-generation-part-2"></a>第5代計算世代 (第2部分) 

|計算大小 (服務目標)|GP_Gen5_16|GP_Gen5_18|GP_Gen5_20|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|計算世代|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|虛擬核心|16|18|20|24|32|40|80|
|記憶體 (GB)|83|93.4|103.8|124.6|166.1|207.6|415.2|
|資料行存放區支援|是|是|是|是|是|是|是|
|OLTP 記憶體內部儲存體 (GB)|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|資料大小上限 (GB)|3072|3072|3072|4096|4096|4096|4096|
|記錄大小上限 (GB)|922|922|922|1024|1024|1024|1024|
|TempDB 資料大小上限 (GB) |512|576|640|768|1024|1280|2560|
|儲存體類型|遠端 SSD|遠端 SSD|遠端 SSD|遠端 SSD|遠端 SSD|遠端 SSD|遠端 SSD|
|IO 延遲 (大約)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|
|最大資料 IOPS *|5120|5760|6400|7680|10240|12800|12800|
|最大記錄速率 (MBps) |36|36|36|36|36|36|36|
|並行背景工作 (要求) 數上限|1600|1800|2000|2400|3200|4000|8000|
|並行工作階段數上限|30,000|30,000|30,000|30,000|30,000|30,000|30,000|
|複本數目|1|1|1|1|1|1|1|
|多重 AZ|[提供預覽](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[提供預覽](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[提供預覽](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[提供預覽](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[提供預覽](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[提供預覽](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[提供預覽](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|
|讀取向外延展|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|內含備份儲存體|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|

\* IO 大小的最大值，範圍介於 8 KB 到 64 KB 之間。 實際的 IOPS 取決於工作負載。 如需詳細資訊，請參閱 [資料 IO 治理](resource-limits-logical-server.md#resource-governance)。

## <a name="general-purpose---provisioned-compute---fsv2-series"></a>一般用途-布建的計算-Fsv2 系列

### <a name="fsv2-series-compute-generation-part-1"></a>Fsv2 系列計算世代 (第1部) 

|計算大小 (服務目標)|GP_Fsv2_8|GP_Fsv2_10|GP_Fsv2_12|GP_Fsv2_14| GP_Fsv2_16|
|:---| ---:|---:|---:|---:|---:|
|計算世代|Fsv2 系列|Fsv2 系列|Fsv2 系列|Fsv2 系列|Fsv2 系列|
|虛擬核心|8|10|12|14|16|
|記憶體 (GB)|15.1|18.9|22.7|26.5|30.2|
|資料行存放區支援|是|是|是|是|是|
|OLTP 記憶體內部儲存體 (GB)|N/A|N/A|N/A|N/A|N/A|
|資料大小上限 (GB)|1024|1024|1024|1024|1536|
|記錄大小上限 (GB)|336|336|336|336|512|
|TempDB 資料大小上限 (GB) |333|333|333|333|333|
|儲存體類型|遠端 SSD|遠端 SSD|遠端 SSD|遠端 SSD|遠端 SSD|
|IO 延遲 (大約)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|
|最大資料 IOPS *|2560|3200|3840|4480|5120|
|最大記錄速率 (MBps) |36|36|36|36|36|
|並行背景工作 (要求) 數上限|400|500|600|700|800|
|並行登入數上限|800|1000|1200|1400|1600|
|並行工作階段數上限|30,000|30,000|30,000|30,000|30,000|
|複本數目|1|1|1|1|1|
|多重 AZ|N/A|N/A|N/A|N/A|N/A|
|讀取向外延展|N/A|N/A|N/A|N/A|N/A|
|內含備份儲存體|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|

\* IO 大小的最大值，範圍介於 8 KB 到 64 KB 之間。 實際的 IOPS 取決於工作負載。 如需詳細資訊，請參閱 [資料 IO 治理](resource-limits-logical-server.md#resource-governance)。

### <a name="fsv2-series-compute-generation-part-2"></a>Fsv2 系列計算世代 (第2部分) 

|計算大小 (服務目標)|GP_Fsv2_18|GP_Fsv2_20|GP_Fsv2_24|GP_Fsv2_32| GP_Fsv2_36|GP_Fsv2_72|
|:---| ---:|---:|---:|---:|---:|---:|
|計算世代|Fsv2 系列|Fsv2 系列|Fsv2 系列|Fsv2 系列|Fsv2 系列|Fsv2 系列|
|虛擬核心|18|20|24|32|36|72|
|記憶體 (GB)|34.0|37.8|45.4|60.5|68.0|136.0|
|資料行存放區支援|是|是|是|是|是|是|
|OLTP 記憶體內部儲存體 (GB)|N/A|N/A|N/A|N/A|N/A|N/A|
|資料大小上限 (GB)|1536|1536|1536|3072|3072|4096|
|記錄大小上限 (GB)|512|512|512|1024|1024|1024|
|TempDB 資料大小上限 (GB) |83.25|92.5|111|148|166.5|333|
|儲存體類型|遠端 SSD|遠端 SSD|遠端 SSD|遠端 SSD|遠端 SSD|遠端 SSD|
|IO 延遲 (大約)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|5-7 毫秒 (寫入)<br>5-10 毫秒 (讀取)|
|最大資料 IOPS *|5760|6400|7680|10240|11520|23040|
|最大記錄速率 (MBps) |36|36|36|36|36|36|
|並行背景工作 (要求) 數上限|900|1000|1200|1600|1800|3600|
|並行登入數上限|1800|2000|2400|3200|3600|7200|
|並行工作階段數上限|30,000|30,000|30,000|30,000|30,000|30,000|
|複本數目|1|1|1|1|1|1|
|多重 AZ|N/A|N/A|N/A|N/A|N/A|N/A|
|讀取向外延展|N/A|N/A|N/A|N/A|N/A|N/A|
|內含備份儲存體|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|

\* IO 大小的最大值，範圍介於 8 KB 到 64 KB 之間。 實際的 IOPS 取決於工作負載。 如需詳細資訊，請參閱 [資料 IO 治理](resource-limits-logical-server.md#resource-governance)。

## <a name="business-critical---provisioned-compute---gen4"></a>商務關鍵性-布建的計算-第4代

> [!IMPORTANT]
> 澳大利亞東部或巴西南部區域不再支援新的第4代資料庫。

### <a name="gen4-compute-generation-part-1"></a>第4代計算世代 (第1部) 

|計算大小 (服務目標)|BC_Gen4_1|BC_Gen4_2|BC_Gen4_3|BC_Gen4_4|BC_Gen4_5|BC_Gen4_6|
|:--- | --: |--: |--: |--: |--: |--: |
|計算世代|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|虛擬核心|1|2|3|4|5|6|
|記憶體 (GB)|7|14|21|28|35|42|
|資料行存放區支援|是|是|是|是|是|是|
|OLTP 記憶體內部儲存體 (GB)|1|2|3|4|5|6|
|儲存體類型|本機 SSD|本機 SSD|本機 SSD|本機 SSD|本機 SSD|本機 SSD|
|資料大小上限 (GB)|1024|1024|1024|1024|1024|1024|
|記錄大小上限 (GB)|307|307|307|307|307|307|
|TempDB 資料大小上限 (GB) |32|64|96|128|160|192|
|IO 延遲 (大約)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|
|最大資料 IOPS *|4,000|8,000|12,000|16,000|20,000|24,000|
|最大記錄速率 (MBps) |8|16|24|32|40|48|
|並行背景工作 (要求) 數上限|200|400|600|800|1000|1200|
|並行登入數上限|200|400|600|800|1000|1200|
|並行工作階段數上限|30,000|30,000|30,000|30,000|30,000|30,000|
|複本數目|4|4|4|4|4|4|
|多重 AZ|是|是|是|是|是|是|
|讀取向外延展|是|是|是|是|是|是|
|內含備份儲存體|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|

\* IO 大小的最大值，範圍介於 8 KB 到 64 KB 之間。 實際的 IOPS 取決於工作負載。 如需詳細資訊，請參閱 [資料 IO 治理](resource-limits-logical-server.md#resource-governance)。

### <a name="gen4-compute-generation-part-2"></a>第4代計算世代 (第2部分) 

|計算大小 (服務目標)|BC_Gen4_7|BC_Gen4_8|BC_Gen4_9|BC_Gen4_10|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|計算世代|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|虛擬核心|7|8|9|10|16|24|
|記憶體 (GB)|49|56|63|70|112|159.5|
|資料行存放區支援|是|是|是|是|是|是|
|OLTP 記憶體內部儲存體 (GB)|7|8|9.5|11|20|36|
|儲存體類型|本機 SSD|本機 SSD|本機 SSD|本機 SSD|本機 SSD|本機 SSD|
|資料大小上限 (GB)|1024|1024|1024|1024|1024|1024|
|記錄大小上限 (GB)|307|307|307|307|307|307|
|TempDB 資料大小上限 (GB) |224|256|288|320|512|768|
|IO 延遲 (大約)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|
|資料 IOPS 上限 |28,000|32,000|36000|40,000|64,000|76,800|
|最大記錄速率 (MBps) |56|64|64|64|64|64|
|並行背景工作 (要求) 數上限|1400|1600|1800|2000|3200|4800|
| (要求的並行登入數上限) |1400|1600|1800|2000|3200|4800|
|並行工作階段數上限|30,000|30,000|30,000|30,000|30,000|30,000|
|複本數目|4|4|4|4|4|4|
|多重 AZ|是|是|是|是|是|是|
|讀取向外延展|是|是|是|是|是|是|
|內含備份儲存體|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|

\* IO 大小的最大值，範圍介於 8 KB 到 64 KB 之間。 實際的 IOPS 取決於工作負載。 如需詳細資訊，請參閱 [資料 IO 治理](resource-limits-logical-server.md#resource-governance)。

## <a name="business-critical---provisioned-compute---gen5"></a>商務關鍵性-布建的計算-第5代

### <a name="gen5-compute-generation-part-1"></a>第5代計算世代 (第1部) 

|計算大小 (服務目標)|BC_Gen5_2|BC_Gen5_4|BC_Gen5_6|BC_Gen5_8|BC_Gen5_10|BC_Gen5_12|BC_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|計算世代|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|虛擬核心|2|4|6|8|10|12|14|
|記憶體 (GB)|10.4|20.8|31.1|41.5|51.9|62.3|72.7|
|資料行存放區支援|是|是|是|是|是|是|是|
|OLTP 記憶體內部儲存體 (GB)|1.57|3.14|4.71|6.28|8.65|11.02|13.39|
|資料大小上限 (GB)|1024|1024|1536|1536|1536|3072|3072|
|記錄大小上限 (GB)|307|307|461|461|461|922|922|
|TempDB 資料大小上限 (GB) |64|128|192|256|320|384|448|
|儲存體類型|本機 SSD|本機 SSD|本機 SSD|本機 SSD|本機 SSD|本機 SSD|本機 SSD|
|IO 延遲 (大約)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|
|最大資料 IOPS *|8000|16,000|24,000|32,000|40,000|48000|56000|
|最大記錄速率 (MBps) |24|48|72|96|96|96|96|
|並行背景工作 (要求) 數上限|200|400|600|800|1000|1200|1400|
|並行登入數上限|200|400|600|800|1000|1200|1400|
|並行工作階段數上限|30,000|30,000|30,000|30,000|30,000|30,000|30,000|
|複本數目|4|4|4|4|4|4|4|
|多重 AZ|是|是|是|是|是|是|是|
|讀取向外延展|是|是|是|是|是|是|是|
|內含備份儲存體|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|

\* IO 大小的最大值，範圍介於 8 KB 到 64 KB 之間。 實際的 IOPS 取決於工作負載。 如需詳細資訊，請參閱 [資料 IO 治理](resource-limits-logical-server.md#resource-governance)。

### <a name="gen5-compute-generation-part-2"></a>第5代計算世代 (第2部分) 

|計算大小 (服務目標)|BC_Gen5_16|BC_Gen5_18|BC_Gen5_20|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|計算世代|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|虛擬核心|16|18|20|24|32|40|80|
|記憶體 (GB)|83|93.4|103.8|124.6|166.1|207.6|415.2|
|資料行存放區支援|是|是|是|是|是|是|是|
|OLTP 記憶體內部儲存體 (GB)|15.77|18.14|20.51|25.25|37.94|52.23|131.64|
|資料大小上限 (GB)|3072|3072|3072|4096|4096|4096|4096|
|記錄大小上限 (GB)|922|922|922|1024|1024|1024|1024|
|TempDB 資料大小上限 (GB) |512|576|640|768|1024|1280|2560|
|儲存體類型|本機 SSD|本機 SSD|本機 SSD|本機 SSD|本機 SSD|本機 SSD|本機 SSD|
|IO 延遲 (大約)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|
|最大資料 IOPS *|64,000|72000|80,000|96000|128000|160,000|204800|
|最大記錄速率 (MBps) |96|96|96|96|96|96|96|
|並行背景工作 (要求) 數上限|1600|1800|2000|2400|3200|4000|8000|
|並行登入數上限|1600|1800|2000|2400|3200|4000|8000|
|並行工作階段數上限|30,000|30,000|30,000|30,000|30,000|30,000|30,000|
|複本數目|4|4|4|4|4|4|4|
|多重 AZ|是|是|是|是|是|是|是|
|讀取向外延展|是|是|是|是|是|是|是|
|內含備份儲存體|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|

\* IO 大小的最大值，範圍介於 8 KB 到 64 KB 之間。 實際的 IOPS 取決於工作負載。 如需詳細資訊，請參閱 [資料 IO 治理](resource-limits-logical-server.md#resource-governance)。

## <a name="business-critical---provisioned-compute---m-series"></a>商務關鍵性-布建的計算-M 系列

### <a name="m-series-compute-generation-part-1"></a>M 系列計算世代 (第1部) 

|計算大小 (服務目標)|BC_M_8|BC_M_10|BC_M_12|BC_M_14|BC_M_16|BC_M_18|
|:---| ---:|---:|---:|---:|---:|---:|
|計算世代|M 系列|M 系列|M 系列|M 系列|M 系列|M 系列|
|虛擬核心|8|10|12|14|16|18|
|記憶體 (GB)|235.4|294.3|353.2|412.0|470.9|529.7|
|資料行存放區支援|是|是|是|是|是|是|
|OLTP 記憶體內部儲存體 (GB)|64|80|96|112|128|150|
|資料大小上限 (GB)|512|640|768|896|1024|1152|
|記錄大小上限 (GB)|171|213|256|299|341|384|
|TempDB 資料大小上限 (GB) |256|320|384|448|512|576|
|儲存體類型|本機 SSD|本機 SSD|本機 SSD|本機 SSD|本機 SSD|本機 SSD|
|IO 延遲 (大約)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|
|最大資料 IOPS *|12499|15624|18748|21873|24998|28123|
|最大記錄速率 (MBps) |48|60|72|84|96|108|
|並行背景工作 (要求) 數上限|800|1,000|1,200|1,400|1,600|1,800|
|並行登入數上限|800|1,000|1,200|1,400|1,600|1,800|
|並行工作階段數上限|30000|30000|30000|30000|30000|30000|
|複本數目|4|4|4|4|4|4|
|多重 AZ|否|否|否|否|否|否|
|讀取向外延展|是|是|是|是|是|是|
|內含備份儲存體|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|

\* IO 大小的最大值，範圍介於 8 KB 到 64 KB 之間。 實際的 IOPS 取決於工作負載。 如需詳細資訊，請參閱 [資料 IO 治理](resource-limits-logical-server.md#resource-governance)。

> [!IMPORTANT]
> 在某些情況下，您可能需要壓縮資料庫來回收未使用的空間。 如需詳細資訊，請參閱 [管理 Azure SQL Database 中](file-space-manage.md)的檔案空間。

### <a name="m-series-compute-generation-part-2"></a>M 系列計算產生 (第2部分) 

|計算大小 (服務目標)|BC_M_20|BC_M_24|BC_M_32|BC_M_64|BC_M_128|
|:---| ---:|---:|---:|---:|---:|
|計算世代|M 系列|M 系列|M 系列|M 系列|M 系列|
|虛擬核心|20|24|32|64|128|
|記憶體 (GB)|588.6|706.3|941.8|1883.5|3767.0|
|資料行存放區支援|是|是|是|是|是|
|OLTP 記憶體內部儲存體 (GB)|172|216|304|704|1768|
|資料大小上限 (GB)|1280|1536|2048|4096|4096|
|記錄大小上限 (GB)|427|512|683|1024|1024|
|TempDB 資料大小上限 (GB) |4096|2048|1024|768|640|
|儲存體類型|本機 SSD|本機 SSD|本機 SSD|本機 SSD|本機 SSD|
|IO 延遲 (大約)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|1-2 毫秒 (寫入)<br>1-2 毫秒 (讀取)|
|最大資料 IOPS *|31248|37497|49996|99993|160,000|
|最大記錄速率 (MBps) |120|144|192|264|264|
|並行背景工作 (要求) 數上限|2,000|2,400|3,200|6,400|12,800|
|並行登入數上限|2,000|2,400|3,200|6,400|12,800|
|並行工作階段數上限|30000|30000|30000|30000|30000|
|複本數目|4|4|4|4|4|
|多重 AZ|否|否|否|否|否|
|讀取向外延展|是|是|是|是|是|
|內含備份儲存體|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|1X DB 大小|

\* IO 大小的最大值，範圍介於 8 KB 到 64 KB 之間。 實際的 IOPS 取決於工作負載。 如需詳細資訊，請參閱 [資料 IO 治理](resource-limits-logical-server.md#resource-governance)。

> [!IMPORTANT]
> 在某些情況下，您可能需要壓縮資料庫來回收未使用的空間。 如需詳細資訊，請參閱 [管理 Azure SQL Database 中](file-space-manage.md)的檔案空間。



## <a name="next-steps"></a>後續步驟

- 如需單一資料庫的 DTU 資源限制，請參閱 [使用 DTU 購買模型的單一資料庫資源限制](resource-limits-dtu-single-databases.md)
- 如需彈性集區的 vCore 資源限制，請參閱 [使用 vCore 購買模型的彈性集區資源限制](resource-limits-vcore-elastic-pools.md)
- 如需彈性集區的 DTU 資源限制，請參閱 [使用 DTU 購買模型的彈性集區資源限制](resource-limits-dtu-elastic-pools.md)
- 如需 SQL 受控執行個體的資源限制，請參閱 [sql 受控執行個體資源限制](../managed-instance/resource-limits.md)。
- 如需一般 Azure 限制的相關資訊，請參閱 [Azure 訂用帳戶和服務限制、配額及條件約束](../../azure-resource-manager/management/azure-subscription-service-limits.md)。
- 如需伺服器上資源限制的詳細資訊，請參閱 [伺服器上的資源限制總覽](resource-limits-logical-server.md) ，以取得伺服器和訂閱層級之限制的相關資訊。
