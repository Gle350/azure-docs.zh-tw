---
title: 限制-LUIS
description: 本文包含 Azure 認知服務 Language Understanding (LUIS) 的已知限制。 LUIS 有數個限制區域。 模型限制控制 LUIS 中的意圖、實體和功能。 以金鑰類型為基礎的配額限制。 鍵盤組合可控制 LUIS 網站。
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 06/04/2020
ms.openlocfilehash: 41423ce34a62dfdbd5b9a60f683a2366a94d1bfd
ms.sourcegitcommit: 8f0803d3336d8c47654e119f1edd747180fe67aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/07/2021
ms.locfileid: "97976787"
---
# <a name="limits-for-your-luis-model-and-keys"></a>LUIS 模型和金鑰的限制
LUIS 有數個限制區域。 第一個是 [模型限制](#model-limits)，可控制 LUIS 中的意圖、實體和功能。 第二個領域是以金鑰類型為基礎的[配額限制](#key-limits)。 第三個限制區域是用來控制 LUIS 網站的 [鍵盤組合](#keyboard-controls) 。 第四個領域是 LUIS 撰寫網站和 LUIS [端點](luis-glossary.md#endpoint) API 之間的[世界區域對應](luis-reference-regions.md)。

<a name="model-boundaries"></a>

## <a name="model-limits"></a>模型限制

如果您的應用程式超過 LUIS 模型限制，請考慮使用 [LUIS 分派](luis-concept-enterprise.md#dispatch-tool-and-model) 應用程式或使用 [LUIS 容器](luis-container-howto.md)。

|區域|限制|
|--|:--|
| [應用程式名稱][luis-get-started-create-app] | *預設字元上限 |
| 應用程式| 每個 Azure 撰寫資源500個應用程式 |
| [批次測試][batch-testing]| 10 個資料集，每個資料集 1000 個語句|
| 明確清單 | 每個應用程式 50 個|
| 外部實體 | 無限制 |
| [意圖][intents]|每個應用程式500：499個自訂意圖，以及所需的 _None_ 意圖。<br>[分派](https://aka.ms/dispatch-tool) 應用程式有對應的500分派來源。|
| [列出實體](./luis-concept-entity-types.md) | 父系：50 個項目，子系：20,000 個項目。 正式名稱為*預設字元上限。同義值沒有長度限制。 |
| [機器學習實體 + 角色](./luis-concept-entity-types.md)：<br> 複合<br>簡單<br>實體角色|100父實體或330實體的限制，以較小者為准。 角色會計算為此限制用途的實體。 範例是具有簡單實體的複合，其中有2個角色：1個複合 + 1 個簡單 + 2 個角色 = 4 個330實體。<br>子實體最多可以嵌套到5個層級，每個層級最多10個子系。|
|模型即功能| 可做為特定模型之功能的最大模型數目，以做為10個模型。 用來作為特定模型之功能的片語清單最大數目是10個片語清單。|
| [預覽-動態清單實體](./luis-migration-api-v3.md)|2個每個查詢預測端點要求的清單 ~ 1k|
| [模式](luis-concept-patterns.md)|每個應用程式 500 個模式。<br>模式的長度上限為 400 個字元。<br>每個模式 3 個 pattern.any 實體<br>模式中最多有 2 個巢狀選擇性文字|
| [Pattern.any](./luis-concept-entity-types.md)|每個應用程式 100 個，每個模式 3 個 pattern.any 實體 |
| [片語清單][phrase-list]|500片語清單。 10個全域片語清單，因為模型是功能限制。 不可交換的片語清單最多5000個片語。 可交換的片語清單最多50000個片語。 500000片語每個應用程式的總片語數上限。|
| [預先建置的實體](./howto-add-prebuilt-models.md) | 沒有限制|
| [規則運算式實體](./luis-concept-entity-types.md)|20 個實體<br>每個規則運算式實體模式 具有 500 個字元的上限|
| [角色](./luis-concept-entity-types.md)|每個應用程式 300 個角色。 每個實體 10 個角色|
| [語句][utterances] | 500 個字元<br><br>如果您的文字長度超過此字元限制，則必須在輸入至 LUIS 之前將語句區隔，而且您會收到每個區段的個別意圖回應。 您可以使用明顯的中斷，例如語音中的標點符號和長時間的暫停。|
| [語句範例][utterances] | 每個應用程式 15000-每個意圖的語句數目沒有限制<br><br>如果您需要使用更多範例來訓練應用程式，請使用 [分派](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/Dispatch) 模型方法。 您可以使用一或多個意圖將個別的 LUIS 應用程式（稱為子應用程式） (稱為子應用程式) ，然後將來自每個子 LUIS 應用程式語句範例的分派應用程式定型，以將預測要求導向至正確的子應用程式。 |
| [版本](./luis-concept-app-iteration.md)| 每個應用程式100個版本 |
| [版本名稱][luis-how-to-manage-versions] | 128 個字元 |

*預設字元上限為 50 個字元。

<a name="intent-and-entity-naming"></a>

## <a name="name-uniqueness"></a>名稱唯一性

相較于相同層級的其他物件，物件名稱必須是唯一的。

|物件|限制|
|--|--|
|意圖、實體|所有意圖和機構名稱在應用程式版本中都必須是唯一的。|
|ML 實體元件|所有機器學習實體元件 (子實體) 在相同層級的元件的實體內必須是唯一的。|
|功能 | 所有命名的功能（例如片語清單）在應用程式版本中都必須是唯一的。|
|實體角色|實體或實體元件上的所有角色都必須是唯一的，因為它們位於相同的實體層級 (父系、子系、孫節點等 ) 。|

## <a name="object-naming"></a>物件命名

請勿在下列名稱中使用下列字元：

|Object|排除字元|
|--|--|
|意圖、實體和角色名稱|`:`<br>`$` <br> `&`|
|版本名稱|`\`<br> `/`<br> `:`<br> `?`<br> `&`<br> `=`<br> `*`<br> `+`<br> `(`<br> `)`<br> `%`<br> `@`<br> `$`<br> `~`<br> `!`<br> `#`|

## <a name="resource-usage-and-limits"></a>資源使用量和限制

語言理解具有不同的資源、一個用於撰寫的類型，以及一個用於查詢預測端點的類型。 若要深入了解金鑰類型之間的差異，請參閱 [LUIS 中的撰寫與查詢預測端點金鑰](luis-how-to-azure-subscription.md)。

<a name="key-limits"></a>

### <a name="authoring-resource-limits"></a>撰寫資源限制

 `LUIS.Authoring` 篩選 Azure 入口網站中的資源時，請使用類型。 LUIS 會限制每個 Azure 撰寫資源的500個應用程式。

|撰寫資源|編寫 TPS|
|--|--|
|入門|1 百萬個/月，5 個/秒|
|F0 免費層 |1 百萬個/月，5 個/秒|

* TP = 每秒的交易數

[深入瞭解定價。][pricing]

### <a name="query-prediction-resource-limits"></a>查詢預測資源限制

 `LUIS` 篩選 Azure 入口網站中的資源時，請使用類型。在執行時間上使用的 LUIS 查詢預測端點資源僅對端點查詢有效。

|查詢預測資源|查詢 TP|
|--|--|
|F0 免費層 |1 萬個/月，5 個/秒|
|S0-標準層|50 個/秒|

### <a name="sentiment-analysis"></a>情感分析

提供情感資訊的[情感分析整合](luis-how-to-publish-app.md#enable-sentiment-analysis)提供，不需要其他 Azure 資源。

### <a name="speech-integration"></a>語音整合

[語音整合](../speech-service/how-to-recognize-intents-from-speech-csharp.md) 提供每單位成本1000的端點要求。

[深入瞭解定價。][pricing]

## <a name="keyboard-controls"></a>鍵盤控制項

|鍵盤輸入 | 描述 |
|--|--|
|Ctrl+E|在語句清單上的權杖和實體之間切換|

## <a name="website-sign-in-time-period"></a>網站登入時段

您登入存取的時間為 **60 分鐘**。 在此時段之後，您將會收到這個錯誤。 您需要再次登入。

[luis-get-started-create-app]: ./luis-get-started-create-app.md
[batch-testing]: ./luis-concept-test.md#batch-testing
[intents]: ./luis-concept-intent.md
[phrase-list]: ./luis-concept-feature.md
[utterances]: ./luis-concept-utterance.md
[luis-how-to-manage-versions]: ./luis-how-to-manage-versions.md
[pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/
<!-- TBD: fix this link -->
[speech-to-intent-pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/
