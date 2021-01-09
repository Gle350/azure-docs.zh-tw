---
title: 快速入門 - 利用成本分析探索 Azure 成本
description: 本快速入門可協助您使用成本分析，來探索及分析您的 Azure 組織成本。
author: bandersmsft
ms.author: banders
ms.date: 01/04/2021
ms.topic: quickstart
ms.service: cost-management-billing
ms.subservice: cost-management
ms.reviewer: micflan
ms.custom: contperf-fy21q2
ms.openlocfilehash: 83f2d87e3f4a03ff17526ea5706e4f87b8f39487
ms.sourcegitcommit: 6d6030de2d776f3d5fb89f68aaead148c05837e2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/05/2021
ms.locfileid: "97882444"
---
# <a name="quickstart-explore-and-analyze-costs-with-cost-analysis"></a>快速入門：使用成本分析探索及分析成本

您必須先了解成本源自組織內的何處，才能正確地控制 Azure 成本並進行最佳化。 了解您的服務成本，以及支援哪些環境與系統也很實用。 使各個層面的成本透明化，對於精確地了解組織費用模式而言至關重要。 您可以使用費用模式來強制執行成本控制機制 (例如預算)。

在本快速入門中，您可以使用成本分析來探索及分析組織成本。 您可依組織檢視彙總的成本，以了解經過一段時間所產生的成本，並找出費用趨勢。 您可以檢視經過一段時間累積的成本，對照預算來預估每月、每季，或甚至每年的成本趨勢。 預算有助於遵守財務限制。 預算同時也能用於檢視每天或每月的成本，以找出費用的異常情況。 此外，您可以下載目前報表的資料以供進一步分析，或在外部系統中使用。

在此快速入門中，您可了解如何：

- 在成本分析中檢閱成本
- 自訂成本檢視
- 下載成本分析資料

## <a name="prerequisites"></a>必要條件

成本分析支援不同的 Azure 帳戶類型。 若要檢視所支援帳戶類型的完整清單，請參閱[了解成本管理資料](understand-cost-mgt-data.md)。 若要檢視成本資料，您至少需要 Azure 帳戶的讀取存取。

如需 Azure 成本管理資料的存取權指派相關資訊，請參閱[指派資料的存取權](./assign-access-acm-data.md)。

如果您有新的訂用帳戶，您無法立即使用成本管理功能。 最多可能需要48小時的時間，才能使用所有的成本管理功能。

## <a name="sign-in-to-azure"></a>登入 Azure

- 在 https://portal.azure.com 登入 Azure 入口網站。

## <a name="review-costs-in-cost-analysis"></a>在成本分析中檢閱成本

若要在成本分析中檢閱您的成本，請在 Azure 入口網站中開啟範圍，然後選取功能表中的 [成本分析]。 例如，移至 [訂用帳戶]、從清單中選取訂用帳戶，然後在功能表中選取 [成本分析]。 使用 [範圍] 框，以切換至成本分析中的不同範圍。

您選取的範圍會在成本管理過程中全程使用，以提供資料彙總，並控制成本資訊的存取權。 當您使用範圍時，您不會多重選取。 而是會選取一個其他項目所積存的較大範圍，然後篩選出需要的巢狀範圍。 了解這個方法很重要，因為某些人並沒有單一父項範圍 (包含多個巢狀範圍) 的存取權。

觀看[如何在 Azure 入口網站中使用成本管理](https://www.youtube.com/watch?v=mfxysF-kTFA)影片，以深入了解如何使用成本分析。 若要觀看其他影片，請造訪[成本管理 YouTube 頻道](https://www.youtube.com/c/AzureCostManagement)。

>[!VIDEO https://www.youtube.com/embed/mfxysF-kTFA]

最初的成本分析檢視包含下列區域。

**累積成本檢視**：代表預先定義的成本分析檢視設定。 每個檢視都包含日期範圍、資料粒度、分組依據和篩選條件設定。 預設檢視會顯示目前計費週期的累積成本，但您也可以變更為其他內建檢視。

**實際成本**：顯示當月所產生的實際使用量和購買成本，這些資料也會在您的帳單上顯示。

**預測**：顯示所選期間的總預測成本。

**預算**：顯示已針對所選取範圍規劃的費用限制 (如果有的話)。

**累積資料粒度**：顯示從計費週期起始日開始的每日成本累積總額。 在為計費帳戶或訂用帳戶建立預算之後，即可對照預算，快速查看費用趨勢。 將滑鼠停留在一個日期上方，即可檢視累積到當天的費用。

**樞紐分析圖 (環圈圖)** ：提供動態樞紐分析，將總成本依一組常用的標準屬性進行細分。 它們會顯示目前月份的最大到最小成本。 您隨時都可選取不同的樞紐分析表來變更樞紐分析圖。 成本會依據服務 (計量類別)、位置 (區域)，以及子範圍 (預設) 來進行分類。 例如，計費帳戶下的註冊帳戶、訂用帳戶下的資源群組，以及資源群組下的資源。

![Azure 入口網站中的成本分析初始檢視](./media/quick-acm-cost-analysis/cost-analysis-01.png)

### <a name="understand-forecast"></a>了解預測

成本預測會顯示所選期間的預估成本預測。 此模型是以時間序列回歸模型為基礎。 這需要至少 10 天的近期成本和使用量資料，才能精確地預測成本。 針對指定的時間週期，預測模型需要預測期間中相等數量的訓練資料。 例如，三個月的預測需要至少三個月的近期成本和使用量資料。

此模型最多會使用六個月的訓練資料來預測一年的專案成本。 最少則需要七天的訓練資料來變更其預測。 預測會以成本和使用模式中的重大變更 (例如高點和低點) 為基礎。 預測不會為 **Group by (群組依據)** 屬性中的每個項目產生個別預測。 只會提供總累計成本的預測。 如果您使用多種貨幣，此模型只會以美元提供成本的預測。

因為模型對資料下降和尖峰的依賴程度，如保留執行個體的大量購買會導致您的預測以人為方式膨脹。 預測時間週期和購買大小會影響預測受到影響的時間長度。 當支出穩定時，預測會恢復正常。

## <a name="customize-cost-views"></a>自訂成本檢視

成本分析具備四個內建的檢視，並已針對最常見的目標最佳化：

檢視 | 回答如下問題
--- | ---
累積成本 | 我這個月到目前為止支出了多少費用？ 我將維持在預算範圍內嗎？
每日成本 | 過去 30 天每天的成本是否增加？
依服務區分的成本 | 為什麼過去三張發票列出的每月使用量落差很大？
依資源區分的成本 | 這個月到目前為止，哪一項資源最花錢？
發票詳細資料 | 我的上一張發票包括哪些費用？

![顯示一個本月選取項目範例的檢視選取器](./media/quick-acm-cost-analysis/view-selector.png)

不過，在許多情況下，您需要更深入的分析。 自訂會從選取頁面上方的日期開始。

成本分析預設會顯示當月的資料。 請使用日期選取器快速切換至一般的日期範圍。 範例包括過去七天、過去一個月、今年，或自訂日期範圍。 隨用隨付訂用帳戶也會包含以您的計費週期 (例如目前或上一張發票的計費週期) 為依據的資料範圍，且計費週期不受日曆月份限制。 可使用功能表頂端的 [< 上一個] 和 [下一個 >] 連結，分別跳至上一個或下一個週期。 例如，[<上一個] 會從 [過去 7 天] 切換至 [之前 8 到 14 天] 或 [之前 15 至 21 天]。 選取自訂日期範圍時，請記住最多可以選取一年 (例如 1 月 1 日到 12 月 31 日)。

![顯示本月份範例選取項目的日期選取器](./media/quick-acm-cost-analysis/date-selector.png)

此外，成本分析預設會顯示 **累積** 的成本。 累積的成本會包含每一天和之前天數的所有成本，以提供每日累積成本的持續成長檢視。 此檢視已經過最佳化，可顯示對照於所選時間範圍，您的預算趨勢為何。

使用預測圖表檢視找出潛在的預算缺口。 若有潛在的預算缺口，便會以紅色字體顯示預計的超額支出。 圖表中也會顯示指標符號。 將滑鼠停留在該符號上，便會顯示估計會產生預算缺口的日期。

![顯示潛在預算缺口的範例](./media/quick-acm-cost-analysis/budget-breach.png)

另外，還有 **每日** 檢視，可顯示每一天的成本。 每日檢視並不會顯示成長趨勢。 因為成本尖峰或下降每日各有不同，所以該檢視設計為可顯示異常。 若您選取了預算，則每日檢視也會顯示每日預算的估計值。

若您的每日成本持續高於估計的每日預算，則您可預期將會超過每月預算。 估計的每日預算是協助您在較低層級將預算視覺化的一種方法。 當您的每日成本出現波動時，則相較於您的每月預算，估計的每日預算較不精準。

以下是開啟了費用預測的最新費用每日檢視。
![顯示當月每日成本範例的每日檢視](./media/quick-acm-cost-analysis/daily-view.png)

若關閉費用預測，就不會看到未來日期的預計花費。 此外，當您查看過去時段的成本時，成本預測也不會顯示成本。

一般而言，您可以預期會在 8 到 12 小時內看到已耗用資源的資料或通知。

可使用 [分組方式] 依適用屬性分組，以細分成本並識別成本最高的參與者。 例如，若要依資源標記分組，請選取要作為分組方式的標記索引鍵。 系統就會依每個標記值細分成本，並提供一個額外區段以顯示未套用該標記的資源。

大部分的 Azure 資源都支援標記。 不過，某些標記無法在成本管理和計費中使用。 此外，也不支援資源群組標記。 標記支援適用於將標記套用至資源「之後」所回報的使用量。 您無法追溯套用標記以彙總成本。

觀看[如何使用 Azure 成本管理檢閱標記原則](https://www.youtube.com/watch?v=nHQYcYGKuyw)影片，了解如何運用 Azure 標記原則提升成本資料可見性。

以下是當月 Azure 服務成本的檢視。

![已分組的每日累積檢視，顯示上個月的 Azure 服務成本範例](./media/quick-acm-cost-analysis/grouped-daily-accum-view.png)

根據預設，成本分析會顯示所有產生的使用量和購買成本，且這些資料也會顯示在您的發票上，而這也稱為 **實際成本**。 檢視實際成本適合用來進行發票對帳。 不過，如果您留意費用異常之處和其他成本變化，購買成本激增可能會讓您感到擔憂。 若要讓因為購買保留容量而造成成本激增的情形消失，請切換為 [分攤成本]。

![在實際成本和分攤成本之間切換，以查看將購買的保留容量分散到整個期間並配置給使用該保留容量的資源會有什麼情況](./media/quick-acm-cost-analysis/metric-picker.png)

分攤成本會將購買的保留容量細分成每日區塊，並將其分散到保留容量期間的整個時段。 例如，您不會看到 1 月 1 日購買了 365 美元，而是會看到 1 月 1 日到 12 月 31 日每天購買 1.00 美元。 除了基本攤銷，這些成本也會透過使用已使用該保留容量的特定資源來重新配置且相關聯。 例如，如果該筆 1.00 美元的每日費用由兩部虛擬機器攤銷，您會看到當天有兩筆 0.50 美元的費用。 如果當天並未使用部分保留容量，您會看到一筆與適用虛擬機器相關聯的 0.50 美元費用，以及一筆收費類型為 `UnusedReservation` 的 0.50 美元費用。 只有在檢視攤銷成本時，才會看到未使用的保留容量成本。

因為成本的呈現方式會改變，請務必注意實際成本和分攤成本檢視所顯示的總額會不同。 一般而言，在檢視攤銷成本時，有購買保留容量的月份，其總成本會減少，而在購買保留容量後的月份，則會增加。 攤銷功能僅適用於購買的保留容量，目前並不適用於 Azure Marketplace 的購買項目。

下圖顯示資源群組名稱。 您可以依標籤分組以檢視每個標籤的總成本，也可以使用 **依資源的成本** 檢視以查看特定資源的所有標籤。

![目前檢視的完整資料，顯示資源群組名稱](./media/quick-acm-cost-analysis/full-data-set.png)

依特定屬性將成本分組時，會以最高至最低的順序，顯示前 10 名成本參與者。 如果超過10個，則會顯示前九個成本參與者與 **其他** 群組，以代表所有剩餘的群組合併。 當您依標記分組時，針對未套用標記索引鍵的成本會顯示 [未標記] 群組。 **未標記** 一律放在最後，即使未標記的成本高於已標記的成本也一樣。 如果有 10 個以上的標記值存在，未標記的成本會包含在 [其他] 中。 切換至 [資料表] 檢視，並將資料細微性變更為 [無]，以查看從最高到最低成本排序的所有值。

「傳統」虛擬機器、網路與儲存體資源不會共用詳細的計費資料。 這些資源會在成本分組時合併成 [傳統服務]。

主要圖表下的樞紐分析圖會顯示不同的群組，可讓您用更廣泛的視角來檢視所選期間與篩選條件下的整體成本。 選取屬性或標記來檢視以任何維度彙總的成本。

![顯示樞紐分析圖的範例](./media/quick-acm-cost-analysis/pivot-charts.png)

您可以檢視任何檢視的完整資料集。 您所套用的任何選取項目或篩選條件都會影響所顯示的資料。 若要查看完整的資料集，請選取 [圖表類型] 清單，然後選取 [資料表] 檢視。

![在資料表檢視中目前檢視的資料](./media/quick-acm-cost-analysis/chart-type-table-view.png)

## <a name="saving-and-sharing-customized-views"></a>儲存和共用自訂檢視

若要儲存自訂檢視並與其他人共用，請將成本分析釘選到 Azure 入口網站儀表板，或是複製成本分析的連結。

觀看 [Azure 成本管理中的分享和節省成本檢視](https://www.youtube.com/watch?v=kQkXXj-SmvQ)影片，以深入了解如何使用入口網站在您的組織中分享成本知識。 若要觀看其他影片，請造訪[成本管理 YouTube 頻道](https://www.youtube.com/c/AzureCostManagement)。

>[!VIDEO https://www.youtube.com/embed/kQkXXj-SmvQ]

若要釘選成本分析，選取右上角或 [<Subscription Name> | 成本分析] 後面的釘選圖示。 釘選成本分析只會儲存主要的圖表或資料表檢視。 請共用儀表板來讓其他人可以存取圖格。 共用只會共用儀表板設定，而不會授與其他人存取基礎資料的權利。 如果您沒有成本存取權，但可以存取共用儀表板，您將會看到「存取遭拒」訊息。

若要共用成本分析的連結，請選取視窗頂端的 [共用]。 此時會顯示自訂 URL，它可以開啟此特定範圍的此特定檢視。 如果您沒有成本存取權並取得此 URL，您會看到「存取遭拒」訊息。

## <a name="download-usage-data"></a>下載使用量資料

### <a name="portal"></a>[入口網站](#tab/azure-portal)

有時候當您需要下載資料以便進一步分析時，請將其合併到您的自有資料中，或將其整合到您的自有系統。 成本管理功能可提供一些不同的選項。 作為起點，如果您需要一個快速高階摘要 (就像您在成本分析中取得的內容)，請建置您需要的檢視。 然後選取 [匯出]，並選取 [將資料下載至 CSV] 或 [將資料下載至 Excel] 來下載它。 下載到 Excel 可針對您用來產生所下載資料的檢視提供更多的內容，例如範圍、查詢設定、總計和產生日期。

如果您需要完整的未彙總資料集，請從計費帳戶下載。 然後，從入口網站左側瀏覽窗格的服務清單中，移至 [成本管理 + 計費]。 選取您的計費帳戶 (如果適用)。 移至 [使用方式 + 費用]，然後選取計費週期的 [下載] 圖示。

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

從準備適用於 Azure CLI 的環境開始：

[!INCLUDE [azure-cli-prepare-your-environment-no-header.md](../../../includes/azure-cli-prepare-your-environment-no-header.md)]

登入之後，請使用 [az costmanagement query](/cli/azure/ext/costmanagement/costmanagement#ext_costmanagement_az_costmanagement_query) 命令來查詢您訂用帳戶的每月至今使用量：

```azurecli
az costmanagement query --timeframe MonthToDate --type Usage \
   --scope "subscriptions/00000000-0000-0000-0000-000000000000"
```

您也可以使用 **--dataset-filter** 參數或其他參數來縮小查詢的範圍：

```azurecli
az costmanagement query --timeframe MonthToDate --type Usage \
   --scope "subscriptions/00000000-0000-0000-0000-000000000000" \
   --dataset-filter "{\"and\":[{\"or\":[{\"dimension\":{\"name\":\"ResourceLocation\",\"operator\":\"In\",\"values\":[\"East US\",\"West Europe\"]}},{\"tag\":{\"name\":\"Environment\",\"operator\":\"In\",\"values\":[\"UAT\",\"Prod\"]}}]},{\"dimension\":{\"name\":\"ResourceGroup\",\"operator\":\"In\",\"values\":[\"API\"]}}]}"
```

**--dataset-filter** 參數接受 JSON 字串或 `@json-file`。

您也可以選擇使用 [az costmanagement export](/cli/azure/ext/costmanagement/costmanagement/export) 命令，將使用量資料匯出至 Azure 儲存體帳戶。 您可以從這裡下載資料。

1. 建立資源群組，或使用現有的資源群組。 執行 [az group create](/cli/azure/group#az_group_create) 命令以建立資源群組：

   ```azurecli
   az group create --name TreyNetwork --location "East US"
   ```

1. 建立儲存體帳戶以接收匯出，或使用現有的儲存體帳戶。 若要建立帳戶，請使用 [az storage account create](/cli/azure/storage/account#az_storage_account_create) 命令：

   ```azurecli
   az storage account create --resource-group TreyNetwork --name cmdemo
   ```

1. 執行 [az costmanagement export create](/cli/azure/ext/costmanagement/costmanagement/export#ext_costmanagement_az_costmanagement_export_create) 命令以建立匯出：

   ```azurecli
   az costmanagement export create --name DemoExport --type Usage \
   --scope "subscriptions/00000000-0000-0000-0000-000000000000" --storage-account-id cmdemo \
   --storage-container democontainer --timeframe MonthToDate --storage-directory demodirectory
   ```

---

## <a name="clean-up-resources"></a>清除資源

- 如果您已釘選成本分析的自訂檢視，但不再需要用到，請移至您釘選所在的儀表板，並刪除已釘選的檢視。
- 如果您已下載使用量資料檔案，但不再需要用到，請務必將其刪除。

## <a name="next-steps"></a>後續步驟

請前進到第一個教學課程，了解如何建立及管理預算。

> [!div class="nextstepaction"]
> [建立及管理預算](tutorial-acm-create-budgets.md)