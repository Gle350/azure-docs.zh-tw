---
title: 層級和 sku 的服務限制
titleSuffix: Azure Cognitive Search
description: 容量計劃中使用的服務限制，以及 Azure 認知搜尋服務要求和回應的最大限制。
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/15/2020
ms.openlocfilehash: 5d265fe02d801cf0d2d66be37a8dc2a220e19b34
ms.sourcegitcommit: d2d1c90ec5218b93abb80b8f3ed49dcf4327f7f4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/16/2020
ms.locfileid: "97591339"
---
# <a name="service-limits-in-azure-cognitive-search"></a>Azure 認知搜尋中的服務限制

儲存體與工作負載的最大限制，以及索引、文件和其他物件的數量上限，皆取決於您是在 **免費**、**基本**、**標準**，還是 **儲存體最佳化** 定價層中 [佈建 Azure 認知搜尋](search-create-service-portal.md)。

+ **免費** 的是 Azure 訂用帳戶隨附的多租用戶共用服務。 

+ **基本** 以較小的規模，為生產工作負載提供專用的計算資源，但與其他租用戶共用一些網路基礎架構。

+ **標準** 是在專用的機器上執行，在各層級具有更多的儲存和處理容量。 標準共有四個等級︰S1、S2、S3 及 S3 HD。 S3 高密度 (S3 HD) 是針對[多租用戶](search-modeling-multitenant-saas-applications.md)和大量的小型索引 (每個服務有三千個索引) 所設計。 S3 HD 未提供[索引子功能](search-indexer-overview.md)，而且資料擷取必須利用將資料從來源推送至索引的 API。 

+ **儲存體最佳化** 會在比 **標準** 更多儲存體、儲存體頻寬和記憶體的專用機器上執行。 此階層的目標是變更緩慢的大型索引。 儲存體最佳化分為兩個層級：L1 和 L2。

## <a name="subscription-limits"></a>訂用帳戶限制
[!INCLUDE [azure-search-limits-per-subscription](../../includes/azure-search-limits-per-subscription.md)]

## <a name="storage-limits"></a>儲存體限制
[!INCLUDE [azure-search-limits-per-service](../../includes/azure-search-limits-per-service.md)]

<a name="index-limits"></a>

## <a name="index-limits"></a>索引限制

| 資源 | 免費 | 基本&nbsp;<sup>1</sup>  | S1 | S2 | S3 | S3&nbsp;HD | L1 | L2 |
| -------- | ---- | ------------------- | --- | --- | --- | --- | --- | --- |
| 索引上限 |3 |5 或 15 |50 |200 |200 |每個分割區 1000 個或每個服務 3000 個 |10 |10 |
| 每個索引的簡單欄位數上限 |1000 |100 |1000 |1000 |1000 |1000 |1000 |1000 |
| 每個索引的複雜集合欄位數上限 |40 |40 |40 |40 |40 |40 |40 |40 |
| 每份文件所有複雜集合之間的元素上限&nbsp;<sup>2</sup> |3000 |3000 |3000 |3000 |3000 |3000 |3000 |3000 |
| 複雜欄位的最大深度 |10 |10 |10 |10 |10 |10 |10 |10 |
| 每個索引的[建議工具](/rest/api/searchservice/suggesters)上限 |1 |1 |1 |1 |1 |1 |1 |1 |
| 每個索引的[評分設定檔](/rest/api/searchservice/add-scoring-profiles-to-a-search-index)上限 |100 |100 |100 |100 |100 |100 |100 |100 |
| 每個設定檔的函式上限 |8 |8 |8 |8 |8 |8 |8 |8 |

<sup>1</sup> 2017 年 12 月之前建立的基本服務，在索引方面具有較低的限制 (5 個，而不是 15 個)。 基本層是唯一具有較低限制 (每個索引 100 個欄位) 的 SKU。

<sup>2</sup> 元素有很大的限制，因為有大量的專案會大幅增加索引所需的儲存體。 複雜集合的元素會定義為該集合的成員。 例如，假設 [旅館檔含有房間複雜的集合](search-howto-complex-data-types.md#indexing-complex-types)，房間集合中的每個房間都會被視為一個元素。 在編制索引期間，索引引擎可以在整個檔中安全地處理最多3000個元素。 [這項限制](search-api-migration.md#upgrade-to-2019-05-06) 是在中引進，而且 `api-version=2019-05-06` 只適用于複雜的集合，而不適用於字串集合或複雜欄位。

<a name="document-limits"></a>

## <a name="document-limits"></a>文件限制 

截至 2018 年 10 月，在任何區域中任何計費層 (基本、S1、S2、S3、S3 HD) 建立的任何新的服務，都不再有任何文件計數限制。 在 2018 年 10 月之前建立的舊版服務仍可能受限於文件計數限制。

若要判斷您的服務是否有文件限制，請使用 [GET 服務統計資料 REST API](/rest/api/searchservice/get-service-statistics)。 文件限制會反映在回應中，`null` 表示沒有限制。

> [!NOTE]
> 雖然此服務並未規定任何文件限制，但在基本、S1、S2 和 S3 搜尋服務中，其分區限制為每個索引大約有 240 億個文件。 對於 S3 HD，分區限制為每個索引 20 億個文件。 在分區限制方面，複雜集合中的每個元素都會算成個別文件。

### <a name="document-size-limits-per-api-call"></a>每個 API 呼叫的文件大小限制

呼叫「索引 API」時的文件大小上限大約是 16 MB。

文件大小是索引 API 要求主體大小的實際限制。 由於您可以一次將多份文件整批傳遞給「索引 API」，因此大小限制實際上取決於批次中的文件數量。 針對具有單一文件的批次，文件大小上限為 16 MB 的 JSON。

估計文件大小時，請記得僅考慮搜尋服務可取用的欄位。 您的計算應該忽略來源文件中的任何二進位或映像資料。  

## <a name="indexer-limits"></a>索引子限制

執行時間上限是為了提供整體服務的平衡和穩定性，但較大的資料集所需的編制索引時間可能比允許的上限還多。 如果索引作業無法在允許的時間上限內完成，請按照排程嘗試執行索引作業。 排程器會追蹤索引狀態。 如果排定的索引作業因故中斷，索引子可以在下次排定的執行時間繼續從上次停止處進行。


| 資源 | 免費&nbsp;<sup>1</sup> | 基本&nbsp;<sup>2</sup>| S1 | S2 | S3 | S3&nbsp;HD&nbsp;<sup>3</sup>|L1 |L2 |
| -------- | ----------------- | ----------------- | --- | --- | --- | --- | --- | --- |
| 索引子上限 |3 |5 或 15|50 |200 |200 |N/A |10 |10 |
| 資料來源上限 |3 |5 或 15 |50 |200 |200 |N/A |10 |10 |
| 技能集上限為 <sup>4</sup> |3 |5 或 15 |50 |200 |200 |N/A |10 |10 |
| 每次叫用的索引編製負載上限 |10,000 份文件 |僅限制文件上限 |僅限制文件上限 |僅限制文件上限 |僅限制文件上限 |N/A |沒有限制 |沒有限制 |
| 排程下限 | 5 分鐘 |5 分鐘 |5 分鐘 |5 分鐘 |5 分鐘 |5 分鐘 |5 分鐘 | 5 分鐘 |
| 執行時間上限| 1-3 分鐘 |24 小時 |24 小時 |24 小時 |24 小時 |N/A  |24 小時 |24 小時 |
| 技能集<sup>5</sup>的索引子執行時間上限 | 3-10 分鐘 |2 小時 |2 小時 |2 小時 |2 小時 |N/A  |2 小時 |2 小時 |
| Blob 索引子︰Blob 大小上限，MB |16 |16 |128 |256 |256 |N/A  |256 |256 |
| Blob 索引子︰從 Blob 擷取的內容字元數上限 |32,000 |64,000 |400&nbsp;萬 |800&nbsp;萬 |1600&nbsp;萬 |N/A |400&nbsp;萬 |400&nbsp;萬 |

<sup>1</sup> 免費服務有索引子執行時間上限，針對 Blob 來源為 3 分鐘，針對其他所有資料來源為 1 分鐘。 對於呼叫認知服務的 AI 索引，免費服務限制為每天 20 筆免費交易，其中，交易定義為成功通過擴充管線的文件。

<sup>2</sup> 2017 年 12 月之前建立的基本服務，在索引子、資料來源和技能方面具有較低的限制 (5 個，而不是 15 個)。

<sup>3</sup> S3 HD 服務不包含索引子支援。

<sup>4</sup> 每個技能集上限為 30 個技術。

<sup>5</sup> AI 擴充和影像分析會耗用大量運算資源，並且需要大量的可用處理能力。 這些工作負載的執行時間已縮短，可讓佇列中的其他作業有更多機會執行。

> [!NOTE]
> 如[索引限制](#index-limits)所述，從支援複雜類型的 GA API 版本 (`2019-05-06`) 開始，索引子也會在每個文件的所有複雜集合中，執行 3000 個元素的上限。 這表示如果您已使用先前的 API 版本建立索引子，則不會受限於此限制。 為了保持最大的相容性，如果索引子是使用先前的 API 版本建立的，然後使用 API 版本 `2019-05-06` 或更高版本更新，仍會 **排除** 在限制之外。 客戶應該注意擁有非常龐大的複雜集合的負面影響 (如先前所述)，而且我們強烈建議使用最新的 GA API 版本來建立任何新的索引子。

## <a name="shared-private-link-resource-limits"></a>共用的私人連結資源限制

索引子可以透過透過[共用 private link 資源 API](/rest/api/searchmanagement/sharedprivatelinkresources)管理的[私人端點](search-indexer-howto-access-private.md)來存取其他 Azure 資源。 本節說明與這項功能相關聯的限制。

| 資源 | 免費 | 基本 | S1 | S2 | S3 | S3 HD | L1 | L2
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 私人端點索引子支援 | 否 | 是 | 是 | 是 | 是 | 否 | 是 | 是 |
| 技能集<sup>1</sup>的索引子的私用端點支援 | 否 | 否 | 否 | 是 | 是 | 否 | 是 | 是 |
| 私人端點上限 | N/A | 10或30 | 100 | 400 | 400 | N/A | 20 | 20 |
| 最大相異資源類型<sup>2</sup> | N/A | 4 | 7 | 15 | 15 | N/A | 4 | 4 |

<sup>1</sup> 個 AI 擴充和影像分析會耗用大量運算，並耗用不相稱的可用處理能力。 基於這個理由，在較低層級停用私人連線，以避免對搜尋服務本身的效能和穩定性造成負面影響。

<sup>2</sup> 相異資源類型的數目會計算為 `groupId` 指定搜尋服務的所有共用私用連結資源上使用的唯一值數目，而不論資源的狀態為何。

## <a name="synonym-limits"></a>同義字限制

同義字對應的數目上限因階層而異。 每個規則最多可以有 20 個擴充，擴充是指對等的字詞。 例如，假設「cat」(貓) 與「kitty」(小貓)、「feline」(貓科) 和「felis」(貓屬) 的關聯性計算為 3 個擴充。

| 資源 | 免費 | 基本 | S1 | S2 | S3 | S3-HD |L1 | L2 |
| -------- | -----|------ |----|----|----|-------|---|----|
| 同義字對應上限 |3 |3|5 |10 |20 |20 | 10 | 10 |
| 每個對應的規則數目上限 |5000 |20000|20000 |20000 |20000 |20000 | 20000 | 20000  |

## <a name="queries-per-second-qps"></a>每秒查詢數目 (QPS)

每個客戶必須獨立開發 QPS 估計值。 索引大小和複雜性、查詢大小和複雜性、流量，這三者是 QPS 的主要決定因素。 不知道這些因素，便無法提供有意義的估計值。

計算在專用資源 (基本和標準層) 上執行的服務，更容易預測估計值。 由於可控制較多的參數，所以能更準確地估計 QPS。 如需如何進行估計的指引，請參閱 [Azure 認知搜尋的效能與最佳化](search-performance-optimization.md)。

對於儲存體最佳化的階層 (L1 和 L2)，與標準層相比，您應該預期較低的查詢輸送量和較高的延遲。

## <a name="data-limits-ai-enrichment"></a>資料限制 (AI 擴充)

[AI 擴充管線](cognitive-search-concept-intro.md)可呼叫文字分析資源以進行[實體辨識](cognitive-search-skill-entity-recognition.md)、[關鍵片語擷取](cognitive-search-skill-keyphrases.md)、[情感分析](cognitive-search-skill-sentiment.md)、[語言偵測](cognitive-search-skill-language-detection.md)，以及[個人資訊偵測](cognitive-search-skill-pii-detection.md)，會受到資料限制的約束。 記錄的大小上限應該是 50,000 個字元 (以 [`String.Length`](/dotnet/api/system.string.length) 為測量單位)。 如果您需要先分割資料，再將該資料傳送至情感分析器，請使用[文字分割技能](cognitive-search-skill-textsplit.md)。

## <a name="throttling-limits"></a>節流限制

搜尋查詢和編制索引要求會因為系統接近尖峰容量而受到節流。 節流行為會因不同的 API 而異。 查詢 API (搜尋/建議/自動完成) 和編製索引 API 會根據服務，動態地進行節流。 索引 API 具有靜態要求速率限制。 

索引相關的作業靜態速率要求限制：

+ 列出索引 (取得/indexes) ：每個搜尋單位每秒3個
+ 取得索引 (GET /indexes/myindex)：每個搜尋單位每秒 10 個
+ 建立索引 (POST /indexes)：每個搜尋單位每分鐘 12 個
+ 建立或更新索引 (PUT /indexes/myindex)：每個搜尋單位每秒 6 個
+ 刪除索引 (DELETE/索引/myindex)：每個搜尋單位每分鐘 12 個 

## <a name="api-request-limits"></a>API 要求限制
* 每個要求最多 16 MB <sup>1</sup>
* 最長 8 KB 的 URL 長度
* 每個索引上傳、合併、或刪除批次最多包含 1000 個文件
* $orderby 子句中最多 32 個欄位
* 最大搜尋詞彙的大小是 utf-8 編碼文字的 32,766 個位元組 (32 KB 減 2 個位元組)

<sup>1</sup> 在「Azure 認知搜尋」中，要求主體的上限是 16 MB，這會針對不受理論上限制約束之個別欄位或集合的內容強加實際限制 (如需有關欄位組合和限制的詳細資訊，請參閱[支援的資料類型](/rest/api/searchservice/supported-data-types))。

## <a name="api-response-limits"></a>API 回應限制
* 每一頁搜尋結果最多傳回 1000 個文件
* 每個建議 API 要求最多傳回 100 個建議

## <a name="api-key-limits"></a>API 金鑰限制
API 金鑰可用於服務驗證。 有兩種類型。 系統管理金鑰是在要求標頭中指定，並會授與完整的服務讀寫存取。 查詢金鑰為唯讀並在 URL 上指定，且通常會發佈到用戶端應用程式。

* 每個服務最多 2 個系統管理金鑰
* 每個服務最多 50 個查詢金鑰
