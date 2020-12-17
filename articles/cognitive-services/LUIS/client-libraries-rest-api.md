---
title: 快速入門：Language Understanding (LUIS) SDK 用戶端程式庫和 REST API
description: 使用 LUIS SDK 用戶端程式庫和 REST API，建立並查詢 LUIS 應用程式。
ms.topic: quickstart
ms.date: 12/09/2020
ms.service: cognitive-services
ms.author: aahi
manager: nitinme
ms.subservice: language-understanding
author: aahill
keywords: Azure, 人工智慧, ai, 自然語言處理, nlp, LUIS, azure luis, 自然語言理解, ai 聊天機器人, 聊天機器人製作者, 理解自然語言
ms.custom: devx-track-python, devx-track-js, devx-track-csharp, cog-serv-seo-aug-2020
zone_pivot_groups: programming-languages-set-luis
ms.openlocfilehash: 7ff5844cf9f1bce45df438fc00e24da28c438b05
ms.sourcegitcommit: dea56e0dd919ad4250dde03c11d5406530c21c28
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/09/2020
ms.locfileid: "96941517"
---
# <a name="quickstart-language-understanding-luis-client-libraries-and-rest-api"></a>快速入門：Language Understanding (LUIS) 用戶端程式庫和 REST API

搭配此快速入門使用 C#、Python 或 JavaScript，建立並查詢具有 LUIS SDK 用戶端程式庫的 Azure LUIS 人工智慧 (AI) 應用程式。 您也可以使用 cURL，利用 REST API 來傳送要求。

Language Understanding (LUIS) 可讓您將自然語言處理 (NLP) 套用至使用者的對話、自然語言文字中，以預測整體意義，並找出相關的詳細資訊。

* **撰寫** 用戶端程式庫和 REST API 可讓您建立、編輯、定型及發佈 LUIS 應用程式。
* **預測執行階段** 用戶端程式庫和 REST API 可讓您查詢已發佈的應用程式。

::: zone pivot="programming-language-csharp"
[!INCLUDE [LUIS development with C# SDK](./includes/sdk-csharp.md)]
::: zone-end

::: zone pivot="programming-language-javascript"
[!INCLUDE [LUIS development with Node.js SDK](./includes/sdk-nodejs.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [LUIS development with Python SDK](./includes/sdk-python.md)]
::: zone-end

::: zone pivot="rest-api"
[!INCLUDE [LUIS development with REST API](./includes/rest-api.md)]
::: zone-end

## <a name="clean-up-resources"></a>清除資源

您可以從 [LUIS 入口網站](https://www.luis.ai)刪除應用程式，並從 [Azure 入口網站](https://portal.azure.com/)刪除 Azure 資源。

如果您是使用 REST API，請在完成快速入門時，從檔案系統中刪除 `ExampleUtterances.JSON` 檔案。

## <a name="troubleshooting"></a>疑難排解

* 向用戶端程式庫進行驗證 - 驗證錯誤通常表示使用了錯誤的金鑰及端點。 為方便起見，本快速入門會針對預測執行階段使用撰寫金鑰和端點，但只有在您尚未使用每月配額時才適用。 如果您無法使用撰寫金鑰和端點，則必須在存取預測執行階段 SDK 用戶端程式庫時使用預測執行階段金鑰和端點。
* 建立實體 - 如果您在建立本教學課程中使用的巢狀機器學習實體時發生錯誤，請確定您已複製程式碼，但未改變程式碼來建立不同的實體。
* 建立範例語句 - 如果您在建立本教學課程中使用的已標記範例語句時發生錯誤，請確定您已複製程式碼，但未改變程式碼來建立不同的已標記範例。
* 定型 - 如果您收到定型錯誤，這通常表示應用程式是空的 (沒有包含範例語句的意圖)，或應用程式的意圖或實體格式不正確。
* 雜項錯誤 - 由於程式碼會以文字和 JSON 物件呼叫用戶端程式庫，因此請確定您沒有變更程式碼。

其他錯誤 - 如果您收到上述清單中未涵蓋的錯誤，請在此頁面底部提供意見反應來讓我們知道。 包含您所安裝用戶端程式庫的程式設計語言和版本。

## <a name="next-steps"></a>後續步驟

* [什麼是 Language Understanding (LUIS) API？](what-is-luis.md)
* [新功能](whats-new.md)
* [意圖](luis-concept-intent.md)、[實體](luis-concept-entity-types.md)、[範例語句](luis-concept-utterance.md)和[預建實體](luis-reference-prebuilt-entities.md)
* 此範例的原始程式碼可以在 [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code) 上找到。
* 理解自然語言：[自然語言理解 (NLU) 和自然語言處理 (NLP)](artificial-intelligence.md)
* Bot：[AI 聊天機器人](luis-csharp-tutorial-bf-v4.md "聊天機器人製作者教學課程")
