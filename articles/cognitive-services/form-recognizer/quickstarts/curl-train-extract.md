---
title: 快速入門：使用 cURL 定型模型並擷取資料 - 表單辨識器
titleSuffix: Azure Cognitive Services
description: 在本快速入門中，您將搭配使用表單辨識器 REST API 和 cURL 來定型模型，並從表單中擷取資料。
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 10/05/2020
ms.author: pafarley
ms.openlocfilehash: 0738e06fbd26526ed78991d5db18e7f8c5c58ea7
ms.sourcegitcommit: 2ba6303e1ac24287762caea9cd1603848331dd7a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/15/2020
ms.locfileid: "97505336"
---
# <a name="quickstart-train-a-form-recognizer-model-and-extract-form-data-by-using-the-rest-api-with-curl"></a>快速入門：搭配使用 REST API 與 cURL 將表單辨識器模型定型並擷取表單資料

在本快速入門中，您將搭配使用 Azure 表單辨識器的 REST API 與 cURL 來定型及評分表單，以擷取金鑰/值組和資料表。

如果您沒有 Azure 訂用帳戶，請在開始前建立[免費帳戶](https://azure.microsoft.com/free/cognitive-services/)。

## <a name="prerequisites"></a>Prerequisites

若要完成此快速入門，您必須：
- 已安裝 [cURL](https://curl.haxx.se/windows/)。
- 至少有六個相同類型的表單。 您將使用其中的五個來定型模型，然後使用第六個表單加以測試。 您的表單可以是不同的檔案類型，但必須是相同類型的文件。 您可使用 [範例資料集](https://go.microsoft.com/fwlink/?linkid=2090451) (下載 *sample_data .zip* 並將其解壓縮) 來進行本快速入門。 將訓練檔案上傳至標準效能層級 Azure 儲存體帳戶中 Blob 儲存體容器的根目錄。 您可以將測試檔案放在不同的資料夾中。

## <a name="create-a-form-recognizer-resource"></a>建立表單辨識器資源

[!INCLUDE [create resource](../includes/create-resource.md)]

## <a name="train-a-form-recognizer-model"></a>定型表單辨識器模型

首先，您需要一組 Azure 儲存體 Blob 中的定型資料。 您至少要有五個類型/結構相同的已填妥表單 (PDF 文件和/或影像) 作為主要輸入資料。 或者，您可以使用單一空白表單和兩個已填寫的表單。 空白表單的檔案名稱必須包含「空白」一詞。 請參閱[為自訂模型建置訓練資料集](../build-training-data-set.md)，以獲得如何將訓練資料放在一起的祕訣和選項。

> [!NOTE]
> 您可以使用標記資料功能，事先手動為部分或所有定型資料加上標籤。 這是更複雜的程序，但可產生較佳的定型模型。 若要深入了解此功能，請參閱概觀的[以標籤定型](../overview.md#train-with-labels)一節。



若要使用 Azure Blob 容器中的文件來定型表單辨識器模型，請執行下列 cURL 命令以呼叫 **[定型自訂模型](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/TrainCustomModelAsync)** API。 執行命令之前，請進行下列變更：

1. 將 `<Endpoint>` 取代為您使用表單辨識器訂用帳戶取得的端點。
1. 將 `<subscription key>` 取代為您在先前的步驟中複製的訂用帳戶金鑰。
1. 將 `<SAS URL>` 取代為 Azure Blob 儲存體容器的共用存取簽章 (SAS) URL。 [!INCLUDE [get SAS URL](../includes/sas-instructions.md)]


    # <a name="v20"></a>[v2.0](#tab/v2-0)    
    ```bash
    curl -i -X POST "https://<Endpoint>/formrecognizer/v2.0/custom/models" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>" --data-ascii "{ \"source\": \""<SAS URL>"\"}"
    ```
    # <a name="v21-preview"></a>[v2.1 預覽](#tab/v2-1)    
    ```bash
    curl -i -X POST "https://<Endpoint>/formrecognizer/v2.1-preview.2/custom/models" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>" --data-ascii "{ \"source\": \""<SAS URL>"\"}"
    ```
    
    ---



您會收到含有 **位置** 標頭的 `201 (Success)` 回應。 此標頭的值是要定型之新模型的識別碼。

## <a name="get-training-results"></a>取得定型結果

哀使進行定型作業後，您可以使用新的作業 ( **[取得自訂模型](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/GetCustomModel)** ) 來檢查定型狀態。 請將模型識別碼傳入此 API 呼叫以檢查定型狀態：

1. 將 `<Endpoint>` 取代為您使用表單辨識器訂用帳戶金鑰取得的端點。
1. 將 `<subscription key>` 取代為訂用帳戶金鑰
1. 將 `<model ID>` 取代為您在先前的步驟中取得的模型識別碼



# <a name="v20"></a>[v2.0](#tab/v2-0)    
```bash
curl -X GET "https://<Endpoint>/formrecognizer/v2.0/custom/models/<model ID>" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>"
```
# <a name="v21-preview"></a>[v2.1 預覽](#tab/v2-1)    
```bash
curl -X GET "https://<Endpoint>/formrecognizer/v2.1-preview.2/custom/models/<model ID>" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>"
```ce\": \""<SAS URL>"\"}"
```
    
---

您將會收到 `200 (Success)` 回應，內含下列格式的 JSON 本文。 請留意 `"status"` 欄位。 定型完成後，其值將是 `"ready"`。 如果模型未完成定型，您必須重新執行命令，以重新查詢服務。 我們建議您在每個呼叫之前間隔一秒以上的時間。

`"modelId"` 欄位包含您要定型之模型的識別碼。 下一個步驟將需要這項資料。

```json
{
  "modelInfo":{
    "status":"ready",
    "createdDateTime":"2019-10-08T10:20:31.957784",
    "lastUpdatedDateTime":"2019-10-08T14:20:41+00:00",
    "modelId":"1cfb372bab404ba3aa59481ab2c63da5"
  },
  "trainResult":{
    "trainingDocuments":[
      {
        "documentName":"invoices\\Invoice_1.pdf",
        "pages":1,
        "errors":[

        ],
        "status":"succeeded"
      },
      {
        "documentName":"invoices\\Invoice_2.pdf",
        "pages":1,
        "errors":[

        ],
        "status":"succeeded"
      },
      {
        "documentName":"invoices\\Invoice_3.pdf",
        "pages":1,
        "errors":[

        ],
        "status":"succeeded"
      },
      {
        "documentName":"invoices\\Invoice_4.pdf",
        "pages":1,
        "errors":[

        ],
        "status":"succeeded"
      },
      {
        "documentName":"invoices\\Invoice_5.pdf",
        "pages":1,
        "errors":[

        ],
        "status":"succeeded"
      }
    ],
    "errors":[

    ]
  },
  "keys":{
    "0":[
      "Address:",
      "Invoice For:",
      "Microsoft",
      "Page"
    ]
  }
}
```

## <a name="analyze-forms-for-key-value-pairs-and-tables"></a>分析索引鍵/值組和資料表的表單

接下來，您會使用新定型的模型來分析文件，並從中擷取索引鍵/值組和資料表。 執行下列 cURL 命令，以呼叫 **[分析表單](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeWithCustomForm)** API。 執行命令之前，請進行下列變更：

1. 將 `<Endpoint>` 取代為您從表單辨識器訂用帳戶金鑰中取得的端點。 您可以在表單辨識器的資源 [概觀]  索引標籤上找到此項目。
1. 將 `<model ID>` 取代為您在上一節中取得的模型識別碼。
1. 將 `<SAS URL>` 取代為您在 Azure 儲存體中的檔案的 SAS URL。 請依照「訓練」一節中的步驟操作，但不要取得整個 Blob 容器的 SAS URL，而是取得您要分析之特定檔案的 URL。
1. 將 `<subscription key>` 取代為訂用帳戶金鑰。

# <a name="v20"></a>[v2.0](#tab/v2-0)    
```bash
curl -v "https://<Endpoint>/formrecognizer/v2.0/custom/models/<model ID>/analyze" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>" -d "{ \"source\": \""<SAS URL>"\" } "
```
# <a name="v21-preview"></a>[v2.1 預覽](#tab/v2-1)    
```bash
```bash
curl -v "https://<Endpoint>/formrecognizer/v2.1-preview.2/custom/models/<model ID>/analyze" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>" -d "{ \"source\": \""<SAS URL>"\" } "
```
    
---



您會收到 `202 (Success)` 回應，其中包含 **Operation-Location** 標頭。 此標頭的值會包含您用來追蹤分析作業結果的結果識別碼。 請儲存此結果識別碼以供下一個步驟使用。

## <a name="get-the-analyze-results"></a>取得分析結果

使用下列 API 來查詢分析作業的結果。

1. 將 `<Endpoint>` 取代為您從表單辨識器訂用帳戶金鑰中取得的端點。 您可以在表單辨識器的資源 [概觀]  索引標籤上找到此項目。
1. 將 `<result ID>` 取代為您在上一節中取得的識別碼。
1. 將 `<subscription key>` 取代為訂用帳戶金鑰。


# <a name="v20"></a>[v2.0](#tab/v2-0)    
```bash
curl -X GET "https://<Endpoint>/formrecognizer/v2.0/custom/models/<model ID>/analyzeResults/<result ID>" -H "Ocp-Apim-Subscription-Key: <subscription key>"
```
# <a name="v21-preview"></a>[v2.1 預覽](#tab/v2-1)    
```bash
curl -X GET "https://<Endpoint>/formrecognizer/v2.1-preview/custom/models/<model ID>/analyzeResults/<result ID>" -H "Ocp-Apim-Subscription-Key: <subscription key>"
```
    
---

您將會收到 `200 (Success)` 回應，內含下列格式的 JSON 本文。 為了簡單起見，我們已將輸出內容縮短。 請留意底部附近的 `"status"` 欄位。 當分析作業完成時，其值將是 `"succeeded"`。 如果分析作業未完成，您必須重新執行命令，以重新查詢服務。 我們建議您在每個呼叫之前間隔一秒以上的時間。

主要索引鍵/值組的關聯和資料表位於 `"pageResults"` 節點中。 如果您也透過 *includeTextDetails* URL 參數指定了純文字擷取，則 `"readResults"` 節點會顯示文件中所有文字的內容和位置。

為了簡單起見，此範例 JSON 輸出已縮短。

# <a name="v20"></a>[v2.0](#tab/v2-0)
```JSON
{
  "status": "succeeded",
  "createdDateTime": "2020-08-21T00:46:25Z",
  "lastUpdatedDateTime": "2020-08-21T00:46:32Z",
  "analyzeResult": {
    "version": "2.0.0",
    "readResults": [
      {
        "page": 1,
        "angle": 0,
        "width": 8.5,
        "height": 11,
        "unit": "inch",
        "lines": [
          {
            "text": "Project Statement",
            "boundingBox": [
              5.0153,
              0.275,
              8.0944,
              0.275,
              8.0944,
              0.7125,
              5.0153,
              0.7125
            ],
            "words": [
              {
                "text": "Project",
                "boundingBox": [
                  5.0153,
                  0.275,
                  6.2278,
                  0.275,
                  6.2278,
                  0.7125,
                  5.0153,
                  0.7125
                ]
              },
              {
                "text": "Statement",
                "boundingBox": [
                  6.3292,
                  0.275,
                  8.0944,
                  0.275,
                  8.0944,
                  0.7125,
                  6.3292,
                  0.7125
                ]
              }
            ]
          }, 
        ...
        ]
      }
    ],
    "pageResults": [
      {
        "page": 1,
        "keyValuePairs": [
          {
            "key": {
              "text": "Date:",
              "boundingBox": [
                6.9722,
                1.0264,
                7.3417,
                1.0264,
                7.3417,
                1.1931,
                6.9722,
                1.1931
              ],
              "elements": [
                "#/readResults/0/lines/2/words/0"
              ]
            },
            "confidence": 1
          },
         ...
        ],
        "tables": [
          {
            "rows": 4,
            "columns": 5,
            "cells": [
              {
                "text": "Training Date",
                "rowIndex": 0,
                "columnIndex": 0,
                "boundingBox": [
                  0.6931,
                  4.2444,
                  1.5681,
                  4.2444,
                  1.5681,
                  4.4125,
                  0.6931,
                  4.4125
                ],
                "confidence": 1,
                "rowSpan": 1,
                "columnSpan": 1,
                "elements": [
                  "#/readResults/0/lines/15/words/0",
                  "#/readResults/0/lines/15/words/1"
                ],
                "isHeader": true,
                "isFooter": false
              },
              ...
            ]
          }
        ], 
        "clusterId": 0
      }
    ],
    "documentResults": [],
    "errors": []
  }
}
```    
# <a name="v21-preview"></a>[v2.1 預覽](#tab/v2-1)    
```JSON
{
  "status": "succeeded",
  "createdDateTime": "2020-08-21T01:13:28Z",
  "lastUpdatedDateTime": "2020-08-21T01:13:42Z",
  "analyzeResult": {
    "version": "2.1.0",
    "readResults": [
      {
        "page": 1,
        "angle": 0,
        "width": 8.5,
        "height": 11,
        "unit": "inch",
        "lines": [
          {
            "text": "Project Statement",
            "boundingBox": [
              5.0444,
              0.3613,
              8.0917,
              0.3613,
              8.0917,
              0.6718,
              5.0444,
              0.6718
            ],
            "words": [
              {
                "text": "Project",
                "boundingBox": [
                  5.0444,
                  0.3587,
                  6.2264,
                  0.3587,
                  6.2264,
                  0.708,
                  5.0444,
                  0.708
                ]
              },
              {
                "text": "Statement",
                "boundingBox": [
                  6.3361,
                  0.3635,
                  8.0917,
                  0.3635,
                  8.0917,
                  0.6396,
                  6.3361,
                  0.6396
                ]
              }
            ]
          }, 
          ...
        ] 
      }
    ],
    "pageResults": [
      {
        "page": 1,
        "keyValuePairs": [
          {
            "key": {
              "text": "Date:",
              "boundingBox": [
                6.9833,
                1.0615,
                7.3333,
                1.0615,
                7.3333,
                1.1649,
                6.9833,
                1.1649
              ],
              "elements": [
                "#/readResults/0/lines/2/words/0"
              ]
            },
            "value": {
              "text": "9/10/2020",
              "boundingBox": [
                7.3833,
                1.0802,
                7.925,
                1.0802,
                7.925,
                1.174,
                7.3833,
                1.174
              ],
              "elements": [
                "#/readResults/0/lines/3/words/0"
              ]
            },
            "confidence": 1
          },
          ...
        ], 
        "tables": [
          {
            "rows": 5,
            "columns": 5,
            "cells": [
              {
                "text": "Training Date",
                "rowIndex": 0,
                "columnIndex": 0,
                "boundingBox": [
                  0.6944,
                  4.2779,
                  1.5625,
                  4.2779,
                  1.5625,
                  4.4005,
                  0.6944,
                  4.4005
                ],
                "confidence": 1,
                "rowSpan": 1,
                "columnSpan": 1,
                "elements": [
                  "#/readResults/0/lines/15/words/0",
                  "#/readResults/0/lines/15/words/1"
                ],
                "isHeader": true,
                "isFooter": false
              },
              ...
            ]
          }
        ], 
        "clusterId": 0
      }
    ], 
    "documentResults": [],
    "errors": []
  }
}
``` 

---


## <a name="improve-results"></a>改善結果

[!INCLUDE [improve results](../includes/improve-results-unlabeled.md)]

## <a name="next-steps"></a>後續步驟

在本快速入門中，您已搭配使用表單辨識器 REST API 和 cURL 來定型模型，並在範例案例中加以執行。 接下來，請參閱參考文件來深入探索表單辨識器 API。

> [!div class="nextstepaction"]
> [REST API 參考文件](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeWithCustomForm)
