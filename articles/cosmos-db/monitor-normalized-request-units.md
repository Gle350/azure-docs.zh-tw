---
title: 監視 Azure Cosmos 容器或帳戶的正規化 RU/秒
description: 瞭解如何在 Azure Cosmos DB 中監視作業的標準化要求單位使用量。 Azure Cosmos DB 帳戶的擁有者可以瞭解哪些作業耗用更多的要求單位。
ms.service: cosmos-db
ms.topic: how-to
author: kanshiG
ms.author: govindk
ms.date: 01/07/2021
ms.openlocfilehash: ec82532b54e7834b62fcc03d3ee7de1345a0f546
ms.sourcegitcommit: e46f9981626751f129926a2dae327a729228216e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98027765"
---
# <a name="how-to-monitor-normalized-rus-for-an-azure-cosmos-container-or-an-account"></a>如何監視 Azure Cosmos 容器或帳戶的正規化 RU/秒
[!INCLUDE[appliesto-all-apis](includes/appliesto-all-apis.md)]

Azure Cosmos DB 的 Azure 監視器會提供計量視圖，以監視您的帳戶和建立儀表板。 預設會收集 Azure Cosmos DB 計量，這項功能不會要求您明確啟用或設定任何設定。

**正規化的 RU 耗用量** 計量是用來查看分割區索引鍵範圍對於流量的飽和程度。 Azure Cosmos DB 會在所有分割區索引鍵範圍內平均分配輸送量。 此計量會提供資料分割索引鍵範圍的最大輸送量使用率的每秒查看量。 使用此計量來計算指定容器之分割區索引鍵範圍內的 RU/秒使用量。 藉由使用此計量，如果您在 Azure 監視器中看到所有分割區索引鍵範圍的要求單位使用率百分比很高，您應該增加輸送量以符合工作負載的需求。 範例-標準化使用率會定義為所有資料分割索引鍵範圍的 RU/秒使用率上限。 例如，假設您的最大輸送量為 20000 RU/秒，而您有兩個分割區索引鍵範圍，P_1 和 P_2，每個都可以調整為 10000 RU/秒。 在指定的秒數內，如果 P_1 已使用 6000 RU，P_2 已使用 8000 RU，則標準化使用率為 MAX(6000 RU / 10,000 RU，8000 RU / 10,000 RU) = 0.8。

## <a name="what-to-expect-and-do-when-normalized-rus-is-higher"></a>當正規化的 RU/秒較高時的預期和效果

當正規化的 RU/s 耗用量達到指定分割區索引鍵範圍的100% 時，而且如果用戶端仍在該時間範圍內對該特定分割區索引鍵範圍提出要求，則會收到速率限制錯誤。 用戶端應該遵守建議的等候時間，然後重試要求。 SDK 可讓您藉由適當等候來重試預先設定的時間，藉此輕鬆處理這種情況。  您不一定會看到 RU 速率限制錯誤，因為正規化的 RU 已達100%。 這是因為正規化的 RU 是單一值，表示所有資料分割索引鍵範圍的最大使用量，一個資料分割索引鍵範圍可能會很忙碌，但是其他資料分割索引鍵範圍可以提供要求而不會有問題。 例如，在分割區索引鍵範圍上取用所有 RU/s 的單一作業（例如預存程式），會導致正規化的 RU/秒耗用量出現短暫的尖峰。 在這種情況下，如果要求速率很低，或對不同分割區索引鍵範圍上的其他資料分割提出要求，將不會有任何立即速率限制錯誤。 

Azure 監視器計量可協助您使用「 **要求總數** 」度量來尋找 SQL API 的每個狀態碼作業。 您稍後可以依429狀態碼篩選這些要求，並依作業 **類型** 進行分割。  

若要尋找速率受限的要求，建議的方法是透過診斷記錄取得這項資訊。

如果連續尖峰100% 的正規化 RU/秒耗用量，或接近100% 之間的多個分割區索引鍵範圍，建議您增加輸送量。 您可以利用 Azure 監視器計量和 Azure 監視器診斷記錄，找出哪些作業很繁重，以及其尖峰使用量。

總而言之，「 **正規化的 RU 耗用量** 」度量會用來查看哪些資料分割索引鍵範圍在使用方面更暖。 因此，它可讓您對資料分割索引鍵範圍的輸送量產生扭曲。 您稍後可以在 Azure 監視器記錄檔中查看 **PartitionKeyRUConsumption** 記錄，以取得有關使用方式最忙碌的邏輯分割區索引鍵的相關資訊。 這會指向分割區索引鍵選擇中的變更，或應用程式邏輯中的變更。 若要解決速率限制，請將資料的負載分散在多個分割區，或只在需要時增加輸送量。 

## <a name="view-the-normalized-request-unit-consumption-metric"></a>查看正規化的要求單位耗用量度量

1. 登入 [Azure 入口網站](https://portal.azure.com/)。

2. 從左側導覽列選取 [監視器]，然後選取 [計量]。

   :::image type="content" source="./media/monitor-normalized-request-units/monitor-metrics-blade.png" alt-text="Azure 監視器中的 [計量] 窗格":::

3. 從 [計量] 窗格 > **選取資源** > 選擇必要的 **訂用帳戶** 和 **資源群組**。 在 [資源類型] 中，選取 [Azure Cosmos DB 帳戶]，然後選擇其中一個現有的 Azure Cosmos 帳戶並選取 [套用]。

   :::image type="content" source="./media/monitor-normalized-request-units/select-cosmos-db-account.png" alt-text="選擇 Azure Cosmos 帳戶以查看計量":::

4. 接下來，您可以從可用的計量清單中選取計量。 您可以選取 [要求單位]、[儲存體]、[延遲]、[可用性]、[Cassandra] 等等的專屬計量。 若要深入了解此清單中所有可用的計量，請參閱[依類別區分的計量](monitor-cosmos-db-reference.md)一文。 在此範例中，讓我們選取 **正規化的 RU 耗用量** 計量和 **最大** 值作為匯總值。

   除了這些詳細資料之外，您也可以選取計量的 [時間範圍] 和 [時間細微性]。 在 [最大值] 中，您可以檢視過去 30 天的計量。  套用篩選之後，圖表就會根據您的篩選條件來顯示。

   :::image type="content" source="./media/monitor-normalized-request-units/normalized-request-unit-usage-metric.png" alt-text="從 Azure 入口網站選擇計量":::

### <a name="filters-for-normalized-request-unit-consumption"></a>標準化要求單位耗用量的篩選

您也可以篩選特定 **CollectionName**、 **DatabaseName**、 **PartitionKeyRangeID** 和 **區域** 所顯示的計量和圖表。 若要篩選計量，請選取 [ **加入篩選** ]，然後選擇必要的屬性，例如 **CollectionName** 和您感興趣的對應值。 然後，圖形會顯示所選期間內針對容器取用的正規化 RU 耗用量單位。  

您可以使用 [套用分割] 選項來將計量分組。 針對共用輸送量資料庫，正規化的 RU 度量只會顯示資料庫資料細微性的資料，而不會顯示每個集合的任何資料。 因此對於共用輸送量資料庫，當您依集合名稱套用分割時，將不會看到任何資料。

每個容器的標準化要求單位耗用量度量會顯示如下圖所示：

:::image type="content" source="./media/monitor-normalized-request-units/normalized-request-unit-usage-filters.png" alt-text="將篩選套用至標準化的要求單位耗用量度量":::

## <a name="next-steps"></a>後續步驟

* 使用 Azure 中的 [診斷設定](cosmosdb-monitor-resource-logs.md) 來監視 Azure Cosmos DB 資料。
* [Audit Azure Cosmos DB 控制平面作業](audit-control-plane-logs.md)
