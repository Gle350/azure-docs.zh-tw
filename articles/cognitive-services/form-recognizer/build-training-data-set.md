---
title: 如何建立自訂模型的訓練資料集-表單辨識器
titleSuffix: Azure Cognitive Services
description: 瞭解如何確保訓練資料集已針對定型表單辨識器模型進行優化。
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: pafarley
ms.openlocfilehash: 661b0bbf1aa389dc76567d95ad917548255a1b35
ms.sourcegitcommit: 5ef018fdadd854c8a3c360743245c44d306e470d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/01/2021
ms.locfileid: "97845593"
---
# <a name="build-a-training-data-set-for-a-custom-model"></a>建立自訂模型的訓練資料集

當您使用表單辨識器自訂模型時，您會提供自己的定型資料給 [定型自訂模型](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/TrainCustomModelAsync) 作業，讓模型可以定型至您的產業特定表單。 請遵循本指南來瞭解如何收集和準備資料，以有效地訓練模型。

如果您沒有手動標籤的訓練，您可以使用五個填滿的表單或空白表單 (您必須在) 檔案名中包含 "empty" 一字，再加上兩個填滿的表單。 即使您有足夠的填寫表單，將空白表單加入訓練資料集，也可以改善模型的精確度。

如果您想要使用手動加上標籤的定型資料，您必須使用相同類型的至少五個填滿表單來開始。 除了所需的資料集之外，您仍然可以使用未標記的表單和空白的表單。

## <a name="custom-model-input-requirements"></a>自訂模型輸入需求

首先，請確定您的訓練資料集遵循表單辨識器的輸入需求。

[!INCLUDE [input requirements](./includes/input-requirements.md)]

## <a name="training-data-tips"></a>訓練資料秘訣

請遵循這些額外的秘訣，進一步優化您的資料集進行定型。

* 可能的話，請使用以文字為基礎的 PDF 檔，而不是以影像為基礎的檔。 掃描的 Pdf 會以影像的形式處理。
* 針對填寫的表單，請使用已填入所有欄位的範例。
* 使用在每個欄位中具有不同值的表單。
* 如果您的表單影像品質較低，請使用較大的資料集 (10-15 影像，例如) 。

## <a name="upload-your-training-data"></a>上傳定型資料

當您將用於定型的一組表單檔彙集在一起時，您需要將它上傳至 Azure blob 儲存體容器。 如果您不知道如何使用容器建立 Azure 儲存體帳戶，請遵循 [Azure 入口網站的 Azure 儲存體快速入門](../../storage/blobs/storage-quickstart-blobs-portal.md)。 使用標準效能層級。

如果您想要使用手動加上標籤的資料，您也必須上傳 *.labels.json* ，並.ocr.js對應至訓練檔的檔案 *上* 。 您可以使用 [範例標籤工具](./quickstarts/label-tool.md) (或您自己的 UI) 來產生這些檔案。

### <a name="organize-your-data-in-subfolders-optional"></a>在子資料夾中組織您的資料 (選擇性) 

根據預設， [定型自訂模型](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/TrainCustomModelAsync) API 只會使用位於儲存體容器根目錄的表單檔。 但是，如果您在 API 呼叫中指定資料，則可以使用子資料夾中的資料進行定型。 一般來說， [定型自訂模型](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/TrainCustomModelAsync) 呼叫的主體具有下列格式，其中 `<SAS URL>` 是您容器的共用存取簽章 URL：

```json
{
  "source":"<SAS URL>"
}
```

如果您將下列內容新增至要求本文，則 API 會使用位於子資料夾中的檔進行定型。 此 `"prefix"` 欄位是選擇性的，會將訓練資料集限制為其路徑開頭為指定字串的檔案。 例如，的值 `"Test"` 會讓 API 只查看以 "Test" 一字開頭的檔案或資料夾。

```json
{
  "source": "<SAS URL>",
  "sourceFilter": {
    "prefix": "<prefix string>",
    "includeSubFolders": true
  },
  "useLabelFile": false
}
```

## <a name="next-steps"></a>後續步驟

現在您已瞭解如何建立訓練資料集，接下來請遵循快速入門來訓練自訂表單辨識器模型，並開始在您的表單上使用它。

* [使用用戶端程式庫或 REST API，將模型定型並將表單資料解壓縮](./quickstarts/client-library.md)
* [使用範例標籤工具以標籤定型](./quickstarts/label-tool.md)

## <a name="see-also"></a>請參閱

* [什麼是表單辨識器？](./overview.md)