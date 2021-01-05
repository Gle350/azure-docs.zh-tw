---
title: 語言支援 - LUIS
titleSuffix: Azure Cognitive Services
description: LUIS 在服務內有各種不同的功能。 並非所有功能都有相同的語言地位。 請確定您有興趣的功能支援您所針對的語言文化特性。 LUIS 應用程式是特定文化特性，一旦設定就無法變更。
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 12/09/2019
ms.openlocfilehash: 9363a2dacd91d3868e69e47381eea528e358935c
ms.sourcegitcommit: 5ef018fdadd854c8a3c360743245c44d306e470d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/01/2021
ms.locfileid: "97845456"
---
# <a name="language-and-region-support-for-luis"></a>LUIS 支援的語言與區域

LUIS 在服務內有各種不同的功能。 並非所有功能都有相同的語言地位。 請確定您有興趣的功能支援您所針對的語言文化特性。 LUIS 應用程式是特定文化特性，一旦設定就無法變更。

## <a name="multi-language-luis-apps"></a>多語言 LUIS 應用程式

如果您需要多語言 LUIS 用戶端應用程式 (例如聊天機器人)，您會有幾個選項可用。 如果 LUIS 支援所有語言，請針對每種語言開發 LUIS 應用程式。 每個 LUIS 應用程式都有唯一的應用程式識別碼和端點記錄。 如果您需要為語言 LUIS 提供不支援的語言理解，您可以使用 [Translator 服務](../Translator/translator-info-overview.md) 將語句轉譯成支援的語言、將語句提交至 LUIS 端點，並接收產生的分數。

## <a name="languages-supported"></a>支援的語言

LUIS 可理解下列語言的語句：

| Language |地區設定  |  預建網域 | 預建實體 | 片語清單建議 | **[文字分析](../text-analytics/language-support.md)<br>(情感和<br>關鍵字)|
|--|--|:--:|:--:|:--:|:--:|
| 阿拉伯文 (preview-新式標準阿拉伯文)  |`ar-AR`|-|-|-|-|
| *[中文](#chinese-support-notes) |`zh-CN` | ✔ | ✔ |✔|-|
| 荷蘭文 |`nl-NL` |✔|-|-|✔|
| 英文 (美國) |`en-US` | ✔ | ✔  |✔|✔|
| 法文 (加拿大) |`fr-CA` |-|-|-|✔|
| 法文 (法國) |`fr-FR` |✔| ✔ |✔ |✔|
| 德文 |`de-DE` |✔| ✔ |✔ |✔|
| 古吉拉特文 | `gu-IN`|-|-|-|-|
| Hindi | `hi-IN`|-|✔|-|-|
| 義大利文 |`it-IT` |✔| ✔ |✔|✔|
| *[日語](#japanese-support-notes) |`ja-JP` |✔| ✔ |✔|僅限關鍵片語|
| 韓文 |`ko-KR` |✔|-|-|僅限關鍵片語|
| 馬拉地文 | `mr-IN`|-|-|-|-|
| 葡萄牙文 (巴西) |`pt-BR` |✔| ✔ |✔ |並非所有的次文化特性|
| 西班牙文 (墨西哥)|`es-MX` |-|-|✔|✔|
| 西班牙文 (西班牙) |`es-ES` |✔| ✔ |✔|✔|
| 坦米爾文 | `ta-IN`|-|-|-|-|
| 泰盧固文 | `te-IN`|-|-|-|-|
| 土耳其文 | `tr-TR` |✔|✔|-|僅限情感|




語言支援會因[預建實體](luis-reference-prebuilt-entities.md)和[網域](luis-reference-prebuilt-domains.md)而有所不同。

[!INCLUDE [Chinese language support notes](includes/chinese-language-support-notes.md)]

### <a name="japanese-support-notes"></a>*日文支援附註

 - 因為 LUIS 不提供語法分析而無法理解 Keigo (敬語) 與非正式日文之間的差異，所以您必須將不同的正式層級合併為您應用程式的訓練範例。
     - でございます 與 です 不同。
     - です 與 だ 不同。

[!INCLUDE [Text Analytics support notes](includes/text-analytics-support-notes.md)]

### <a name="speech-api-supported-languages"></a>語音 API 支援的語言
請參閱語音[支援的語言](../speech-service/speech-to-text.md)，以取得語音聽寫模式語言。

### <a name="bing-spell-check-supported-languages"></a>Bing 拼字檢查支援的語言
如需支援的語言清單和狀態，請參閱 Bing 拼字檢查[支援的語言](../bing-spell-check/language-support.md)。

## <a name="rare-or-foreign-words-in-an-application"></a>應用程式中的罕見或外來字
在 `en-us` 文化特性中，LUIS 會學習辨識大部分的英文字，包括俚語。 在 `zh-cn` 文化特性中，LUIS 會學習辨識大部分的中文字元。 如果您使用 `en-us` 中的罕見字組或 `zh-cn` 中的字元，而且您發現 LUIS 似乎無法辨識該字組或字元，您可以將該字組或字元新增到[片語清單功能](luis-how-to-add-features.md)。 例如，應用程式文化特性外部的字組 (也就是外來字組) 應新增至片語清單功能。

<!--This phrase list should be marked non-interchangeable, to indicate that the set of rare words forms a class that LUIS should learn to recognize, but they are not synonyms or interchangeable with each other.-->

### <a name="hybrid-languages"></a>混合式語言
混合式語言結合來自兩個文化特性 (例如英文和中文) 的文字。 LUIS 中不支援這些語言，因為應用程式是以單一文化特性為基礎。

## <a name="tokenization"></a>Token 化
為了執行機器學習，LUIS 根據文化特性將語句分成數個[語彙基元](luis-glossary.md#token)。

|Language|  每個空格或特殊字元 | 字元層級|複合字組
|--|:--:|:--:|:--:|
|阿拉伯文|✔|||
|中文||✔||
|荷蘭文|✔||✔|
|英文 (en-us)|✔ |||
|法文 (fr-FR)|✔|||
|法文 (fr-CA)|✔|||
|德文|✔||✔|
|古吉拉特文|✔|||
|Hindi|✔|||
|義大利文|✔|||
|日文|||✔
|韓文||✔||
|馬拉地文|✔|||
|葡萄牙文 (巴西)|✔|||
|西班牙文 (es-ES)|✔|||
|西班牙文 (es-MX)|✔|||
|坦米爾文|✔|||
|泰盧固文|✔|||
|土耳其文|✔|||


### <a name="custom-tokenizer-versions"></a>自訂 tokenizer 版本

下列文化特性具有自訂 tokenizer 版本：

|文化特性|版本|目的|
|--|--|--|
|德文<br>`de-de`|1.0.0|Token 化文字，方法是使用以機器學習為基礎的 tokenizer 來分割它們，以嘗試將複合字細分成單一元件。<br>如果使用者輸入 `Ich fahre einen krankenwagen` 為語句，則會變成 `Ich fahre einen kranken wagen` 。 允許將 `kranken` 和獨立標記 `wagen` 為不同的實體。|
|德文<br>`de-de`|1.0.2|在空格上分割以 token 化單字。<br> 如果使用者輸入 `Ich fahre einen krankenwagen` 為語句，它會維持單一權杖。 因此 `krankenwagen` ，會標示為單一實體。 |
|荷蘭文<br>`nl-nl`|1.0.0|Token 化文字，方法是使用以機器學習為基礎的 tokenizer 來分割它們，以嘗試將複合字細分成單一元件。<br>如果使用者輸入 `Ik ga naar de kleuterschool` 為語句，則會變成 `Ik ga naar de kleuter school` 。 允許將 `kleuter` 和獨立標記 `school` 為不同的實體。|
|荷蘭文<br>`nl-nl`|1.0.1|在空格上分割以 token 化單字。<br> 如果使用者輸入 `Ik ga naar de kleuterschool` 為語句，它會維持單一權杖。 因此 `kleuterschool` ，會標示為單一實體。 |


### <a name="migrating-between-tokenizer-versions"></a>在 tokenizer 版本之間進行遷移
<!--
Your first choice is to change the tokenizer version in the app file, then import the version. This action changes how the utterances are tokenized but allows you to keep the same app ID.

Tokenizer JSON for 1.0.0. Notice the property value for  `tokenizerVersion`.

```JSON
{
    "luis_schema_version": "3.2.0",
    "versionId": "0.1",
    "name": "german_app_1.0.0",
    "desc": "",
    "culture": "de-de",
    "tokenizerVersion": "1.0.0",
    "intents": [
        {
            "name": "i1"
        },
        {
            "name": "None"
        }
    ],
    "entities": [
        {
            "name": "Fahrzeug",
            "roles": []
        }
    ],
    "composites": [],
    "closedLists": [],
    "patternAnyEntities": [],
    "regex_entities": [],
    "prebuiltEntities": [],
    "model_features": [],
    "regex_features": [],
    "patterns": [],
    "utterances": [
        {
            "text": "ich fahre einen krankenwagen",
            "intent": "i1",
            "entities": [
                {
                    "entity": "Fahrzeug",
                    "startPos": 23,
                    "endPos": 27
                }
            ]
        }
    ],
    "settings": []
}
```

Tokenizer JSON for version 1.0.1. Notice the property value for  `tokenizerVersion`.

```JSON
{
    "luis_schema_version": "3.2.0",
    "versionId": "0.1",
    "name": "german_app_1.0.1",
    "desc": "",
    "culture": "de-de",
    "tokenizerVersion": "1.0.1",
    "intents": [
        {
            "name": "i1"
        },
        {
            "name": "None"
        }
    ],
    "entities": [
        {
            "name": "Fahrzeug",
            "roles": []
        }
    ],
    "composites": [],
    "closedLists": [],
    "patternAnyEntities": [],
    "regex_entities": [],
    "prebuiltEntities": [],
    "model_features": [],
    "regex_features": [],
    "patterns": [],
    "utterances": [
        {
            "text": "ich fahre einen krankenwagen",
            "intent": "i1",
            "entities": [
                {
                    "entity": "Fahrzeug",
                    "startPos": 16,
                    "endPos": 27
                }
            ]
        }
    ],
    "settings": []
}
```
-->

Token 化會在應用層級進行。 不支援版本層級 token 化。

將檔案匯[入為新的應用程式](luis-how-to-start-new-app.md)，而不是版本。 此動作表示新的應用程式有不同的應用程式識別碼，但使用檔案中指定的 tokenizer 版本。