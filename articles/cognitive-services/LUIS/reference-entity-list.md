---
title: 清單實體類型-LUIS
description: 清單實體代表一組固定的封閉式相關字組及其同義字。 LUIS 並不會探索清單實體的額外值。 使用建議功能，以根據目前的清單查看適用於新字組的建議。
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 04/14/2020
ms.openlocfilehash: 410b33b5c6078d096fa4b2acaa7b49bc14c95e31
ms.sourcegitcommit: e15c0bc8c63ab3b696e9e32999ef0abc694c7c41
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/16/2020
ms.locfileid: "97608267"
---
# <a name="list-entity"></a>清單實體

清單實體代表一組固定的封閉式相關字組及其同義字。 LUIS 並不會探索清單實體的額外值。 使用 **建議** 功能，以根據目前的清單查看適用於新字組的建議。 如果有多個清單實體具有相同的值，則在端點查詢中會傳回每個實體。

清單實體不是機器學習實體。 它是全文相符的項目。 LUIS 會將與任何清單中項目相符的項目，在回應中標示為實體。

**當文字資料有下列特性時，最適用此實體：**

* 是已知的組合。
* 不會經常變更。 如果您需要經常變更清單，或想要讓清單自行展開，則具有片語清單的簡單實體會是較佳的選擇。
* 此組合不會超過此實體類型的最大 LUIS [界限](luis-limits.md)。
* 語句中的文字是與同義字或正式名稱相符的項目 (不區分大小寫)。 LUIS 不會將清單用於相符項目以外的範圍。 模糊比對、詞幹分析、複數和其他變化都不會使用清單實體來解析。 若要管理變化，請考慮使用[模式](reference-pattern-syntax.md#syntax-to-mark-optional-text-in-a-template-utterance)並搭配選擇性的文字語法。

![清單實體](./media/luis-concept-entities/list-entity.png)

## <a name="example-json-to-import-into-list-entity"></a>要匯入清單實體的 json 範例

  您可以使用下列 json 格式，將值匯入現有的清單實體：

  ```JSON
  [
      {
          "canonicalForm": "Blue",
          "list": [
              "navy",
              "royal",
              "baby"
          ]
      },
      {
          "canonicalForm": "Green",
          "list": [
              "kelly",
              "forest",
              "avacado"
          ]
      }
  ]
  ```

## <a name="example-json-response"></a>範例 JSON 回應

假設應用程式具有一個名為 `Cities` 的清單，其中可允許城市名稱的各種變化，包括機場城市 (Sea-tac)、機場代碼 (SEA)、郵遞區號 (98101) 及電話區碼 (206)。

|清單項目|項目同義字|
|---|---|
|`Seattle`|`sea-tac`, `sea`, `98101`, `206`, `+1` |
|`Paris`|`cdg`, `roissy`, `ory`, `75001`, `1`, `+33`|

`book 2 tickets to paris`

在上述語句中，`paris` 一字會對應至 `Cities` 清單實體中的 paris 項目。 此清單實體會同時比對項目的正規化名稱及項目同義字。

#### <a name="v2-prediction-endpoint-response"></a>[V2 預測端點回應](#tab/V2)

```JSON
  "entities": [
    {
      "entity": "paris",
      "type": "Cities",
      "startIndex": 18,
      "endIndex": 22,
      "resolution": {
        "values": [
          "Paris"
        ]
      }
    }
  ]
```

#### <a name="v3-prediction-endpoint-response"></a>[V3 預測端點回應](#tab/V3)

如果在 `verbose=false` 查詢字串中設定，則這是 JSON：

```json
"entities": {
    "Cities": [
        [
            "Paris"
        ]
    ]
}
```

如果在 `verbose=true` 查詢字串中設定，則這是 JSON：

```json
"entities": {
    "Cities": [
        [
            "Paris"
        ]
    ],
    "$instance": {
        "Cities": [
            {
                "type": "Cities",
                "text": "paris",
                "startIndex": 18,
                "length": 5,
                "modelTypeId": 5,
                "modelType": "List Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```

* * *

|資料物件|實體名稱|值|
|--|--|--|
|列出實體|`Cities`|`paris`|

## <a name="next-steps"></a>後續步驟

深入瞭解實體：

* [概念](luis-concept-entity-types.md)
* [如何建立](luis-how-to-add-entities.md)
