---
title: 語言支援 - 文字分析 API
titleSuffix: Azure Cognitive Services
description: 文字分析 API 支援的自然語言清單。 本文說明每項作業支援的語言。
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 12/17/2020
ms.author: aahi
ms.openlocfilehash: 180de56e3c158802460d2ff995041e8572d4dcd7
ms.sourcegitcommit: 5ef018fdadd854c8a3c360743245c44d306e470d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/01/2021
ms.locfileid: "97844966"
---
# <a name="text-analytics-api-v3-language-support"></a>文字分析 API v3 語言支援 

#### <a name="sentiment-analysis"></a>[情感分析](#tab/sentiment-analysis)

| Language              | 語言代碼 | v2 支援 | v3 支援 | 起始 v3 模型版本： |              注意 |
|:----------------------|:-------------:|:----------:|:----------:|:--------------------------:|-------------------:|
| 簡體中文    |   `zh-hans`   |     ✓      |     ✓      |         2019-10-01         | 也接受 `zh` |
| 繁體中文   |   `zh-hant`   |            |     ✓      |         2019-10-01         |                    |
| 丹麥文               |     `da`      |     ✓      |            |                            |                    |
| 荷蘭文                 |     `nl`      |     ✓      |            |                            |                    |
| 英文               |     `en`      |     ✓      |     ✓      |         2019-10-01         |                    |
| 芬蘭文               |     `fi`      |     ✓      |            |                            |                    |
| 法文                |     `fr`      |     ✓      |     ✓      |         2019-10-01         |                    |
| 德文                |     `de`      |     ✓      |     ✓      |         2019-10-01         |                    |
| 希臘文                 |     `el`      |     ✓      |            |                            |                    |
| Hindi                 |     `hi`      |            |      ✓     |          2020-04-01        |                    |
| 義大利文               |     `it`      |     ✓      |     ✓      |         2019-10-01         |                    |
| 日文              |     `ja`      |     ✓      |     ✓      |         2019-10-01         |                    |
| 韓文                |     `ko`      |            |     ✓      |         2019-10-01         |                    |
| 挪威文 (巴克摩)   |     `no`      |     ✓      |     ✓      |         2020-07-01         |                    |
| 波蘭文                |     `pl`      |     ✓      |            |                            |                    |
| 葡萄牙文 (巴西)   |    `pt-BR`    |            |     ✓      |         2020-04-01         |                    |
| 葡萄牙文 (葡萄牙) |    `pt-PT`    |     ✓      |     ✓      |         2019-10-01         | 也接受 `pt` |
| 俄文               |     `ru`      |     ✓      |            |                            |                    |
| 西班牙文               |     `es`      |     ✓      |     ✓      |         2019-10-01         |                    |
| 瑞典文               |     `sv`      |     ✓      |            |                            |                    |
| 土耳其文               |     `tr`      |     ✓      |     ✓       |         2020-07-01        |                    |

### <a name="opinion-mining-v31-preview-only"></a>意見挖掘 (3.1-僅限預覽) 

| Language              | 語言代碼 | 從 v3 模型版本開始： |              注意 |
|:----------------------|:-------------:|:------------------------------------:|-------------------:|
| 英文               |     `en`      |              2020-04-01              |                    |


#### <a name="named-entity-recognition-ner"></a>[命名實體辨識 (NER) ](#tab/named-entity-recognition)

> [!NOTE]
> * NER v3 目前僅支援英文和西班牙文語言。 如果您使用不同的語言來呼叫 NER v3，則 API 會傳回2.1 的結果，前提是版本2.1 中支援該語言。
> * 2.1 版只會針對英文、簡體中文、法文、德文和西班牙文等語言傳回一組完整的可用實體。  針對其他支援的語言，會傳回「人員」、「位置」和「組織」實體。

| Language               | 語言代碼 | 2.1 版支援 | v3 支援 | 從 v3 模型版本開始： |       注意        |
|:-----------------------|:-------------:|:----------:|:----------:|:-------------------------------:|:------------------:|
| 阿拉伯文                |     `ar`      |     ✓      |            |                                 |                    |
| 簡體中文     |   `zh-hans`   |     ✓      |            |                                 | 也接受 `zh` |
| 繁體中文   |   `zh-hant`   |     ✓      |            |                                 |                    |
| 捷克文                 |     `cs`      |     ✓      |            |                                 |                    |
| 丹麥文                |     `da`      |     ✓      |            |                                 |                    |
| 荷蘭文                 |     `nl`      |     ✓      |            |                                 |                    |
| 英文                |     `en`      |     ✓      |     ✓      |           2019-10-01            |                    |
| 芬蘭文               |     `fi`      |     ✓      |            |                                 |                    |
| 法文                 |     `fr`      |     ✓      |            |                                 |                    |
| 德文                 |     `de`      |     ✓      |            |                                 |                    |
| Hebrew                |     `he`      |     ✓      |            |                                 |                    |
| 匈牙利文             |     `hu`      |     ✓      |            |                                 |                    |
| 義大利文               |     `it`      |     ✓      |            |                                 |                    |
| 日文              |     `ja`      |     ✓      |            |                                 |                    |
| 韓文                |     `ko`      |     ✓      |            |                                 |                    |
| 挪威文 (巴克摩)   |     `no`      |     ✓      |            |                                 | 也接受 `nb` |
| 波蘭文                |     `pl`      |     ✓      |            |                                 |                    |
| 葡萄牙文 (巴西)   |    `pt-BR`    |     ✓      |            |                                 |                    |
| 葡萄牙文 (葡萄牙) |    `pt-PT`    |     ✓      |            |                                 | 也接受 `pt` |
| 俄文              |     `ru`      |     ✓      |            |                                 |                    |
| 西班牙文               |     `es`      |     ✓      |     ✓       |              2020-04-01                   |                    |
| 瑞典文               |     `sv`      |     ✓      |            |                                 |                    |
| 土耳其文               |     `tr`      |     ✓      |            |                                 |                    |

#### <a name="key-phrase-extraction"></a>[關鍵片語擷取](#tab/key-phrase-extraction)

> [!NOTE]
> 2020-07-01 之前關鍵片語擷取的模型版本具有64個字元的限制。 這項限制不會出現在較新的模型版本中。

| Language              | 語言代碼 | v2 支援 | v3 支援 | 從 v3 模型版本開始提供： |       注意        |
|:----------------------|:-------------:|:----------:|:----------:|:-----------------------------------------:|:------------------:|
| 荷蘭文                 |     `nl`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| 英文               |     `en`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| 芬蘭文               |     `fi`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| 法文                |     `fr`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| 德文                |     `de`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| 義大利文               |     `it`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| 日文              |     `ja`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| 韓文                |     `ko`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| 挪威文 (巴克摩)   |     `no`      |     ✓      |     ✓      |                2019-10-01                 | 也接受 `nb` |
| 波蘭文                |     `pl`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| 葡萄牙文 (巴西)   |    `pt-BR`    |     ✓      |     ✓      |                2019-10-01                 |                    |
| 葡萄牙文 (葡萄牙) |    `pt-PT`    |     ✓      |     ✓      |                2019-10-01                 | 也接受 `pt` |
| 俄文               |     `ru`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| 西班牙文               |     `es`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| 瑞典文               |     `sv`      |     ✓      |     ✓      |                2019-10-01                 |                    |

#### <a name="entity-linking"></a>[實體連結](#tab/entity-linking)

| Language | 語言代碼 | v2 支援 | v3 支援 | 從 v3 模型版本開始提供： | 注意 |
|:---------|:-------------:|:----------:|:----------:|:-----------------------------------------:|:-----:|
| 英文  |     `en`      |     ✓      |     ✓      |                2019-10-01                 |       |
| 西班牙文  |     `es`      |     ✓      |     ✓      |                2019-10-01                 |       |

#### <a name="language-detection"></a>[語言偵測](#tab/language-detection)

文字分析 API 可以偵測各式各樣的語言、變體、方言，以及某些區域/文化特性語言，並使用其名稱和程式碼傳回偵測到的語言。 文字分析語言偵測語言程式碼參數符合 [BCP-47](https://tools.ietf.org/html/bcp47) 標準，其中大部分都符合 [ISO-639-1](https://www.iso.org/iso-639-language-codes.html) 識別碼。 

如果您有以較不常用的語言表示的內容，您可以嘗試使用「語言偵測」，看它是否會傳回代碼。 對於無法偵測到的語言，會產生 `unknown` 回應。

| Language | 語言代碼 | v3 支援 | 從 v3 模型版本開始提供： |
|:-|:-:|:-:|:-:|
| 南非荷蘭文 | `af` | ✓ |  |
| 阿爾巴尼亞文 | `sq` | ✓ |  |
| 阿拉伯文 | `ar` | ✓ |  |
| 亞美尼亞文 | `hy` | ✓ |  |
| 巴斯克文 | `eu` | ✓ |  |
| 白俄羅斯文 | `be` | ✓ |  |
| 孟加拉文 | `bn` | ✓ |  |
| 波士尼亞文 | `bs` | ✓ | 2020-09-01 |
| 保加利亞文 | `bg` | ✓ |  |
| 緬甸 | `my` | ✓ |  |
| 加泰羅尼亞文，Valencian | `ca` | ✓ |  |
| 集中式高棉文 | `km` | ✓ |  |
| 中文 | `zh` | ✓ |  |
| 簡體中文 | `zh_chs` | ✓ |  |
| 繁體中文 | `zh_cht` | ✓ |  |
| 克羅埃西亞文 | `hr` | ✓ |  |
| 捷克文 | `cs` | ✓ |  |
| 丹麥文 | `da` | ✓ |  |
| 達利文 | `prs` | ✓ | 2020-09-01 |
| 迪維、Dhivehi、Maldivian | `dv` | ✓ |  |
| 荷蘭文，佛蘭德文 | `nl` | ✓ |  |
| 英文 | `en` | ✓ |  |
| 世界文 | `eo` | ✓ |  |
| 愛沙尼亞文 | `et` | ✓ |  |
| 斐濟文 | `fj` | ✓ | 2020-09-01 |
| 芬蘭文 | `fi` | ✓ |  |
| 法文 | `fr` | ✓ |  |
| 加利西亞文 | `gl` | ✓ |  |
| 喬治亞文 | `ka` | ✓ |  |
| 德文 | `de` | ✓ |  |
| 希臘文 | `el` | ✓ |  |
| 古吉拉特文 | `gu` | ✓ |  |
| 海地、海地克裡奧爾文 | `ht` | ✓ |  |
| Hebrew | `he` | ✓ |  |
| Hindi | `hi` | ✓ |  |
| 白苗文 | `mww` | ✓ | 2020-09-01 |
| 匈牙利文 | `hu` | ✓ |  |
| 冰島文 | `is` | ✓ |  |
| 印尼文 | `id` | ✓ |  |
| 因紐特文 | `iu` | ✓ |  |
| 愛爾蘭文 | `ga` | ✓ |  |
| 義大利文 | `it` | ✓ |  |
| 日文 | `ja` | ✓ |  |
| 坎那達文 | `kn` | ✓ |  |
| 哈薩克文 | `kk` | ✓ | 2020-09-01 |
| 韓文 | `ko` | ✓ |  |
| 庫爾德文 | `ku` | ✓ |  |
| 寮文 | `lo` | ✓ |  |
| 拉丁文 | `la` | ✓ |  |
| 拉脫維亞文 | `lv` | ✓ |  |
| 立陶宛文 | `lt` | ✓ |  |
| 馬其頓文 | `mk` | ✓ |  |
| 馬達加斯加文 | `mg` | ✓ | 2020-09-01 |
| 馬來文 | `ms` | ✓ |  |
| 馬來亞拉姆文 | `ml` | ✓ |  |
| 馬爾他文 | `mt` | ✓ |  |
| 毛利文 | `mi` | ✓ | 2020-09-01 |
| 馬拉地文 | `mr` | ✓ | 2020-09-01 |
| 挪威文 | `no` | ✓ |  |
| 挪威文 (耐諾斯克) | `nn` | ✓ |  |
| 歐利亞文 | `or` | ✓ |  |
| 普什圖文，Pushto | `ps` | ✓ |  |
| 波斯文 | `fa` | ✓ |  |
| 波蘭文 | `pl` | ✓ |  |
| 葡萄牙文 | `pt` | ✓ |  |
| 旁遮普文，Panjabi | `pa` | ✓ |  |
| 奎雷塔洛歐多米文 | `otq` | ✓ | 2020-09-01 |
| 羅馬尼亞文、Moldavian、摩爾多瓦 | `ro` | ✓ |  |
| 俄文 | `ru` | ✓ |  |
| 薩摩亞文 | `sm` | ✓ | 2020-09-01 |
| 塞爾維亞文 | `sr` | ✓ |  |
| 僧伽羅語、Sinhalese | `si` | ✓ |  |
| 斯洛伐克文 | `sk` | ✓ |  |
| 斯洛維尼亞文 | `sl` | ✓ |  |
| 索馬利文 | `so` | ✓ |  |
| 西班牙文，標準 | `es` | ✓ |  |
| 史瓦西里文 | `sw` | ✓ |  |
| 瑞典文 | `sv` | ✓ |  |
| 他加祿文 | `tl` | ✓ |  |
| 大溪地文 | `ty` | ✓ | 2020-09-01 |
| 坦米爾文 | `ta` | ✓ |  |
| 泰盧固文 | `te` | ✓ |  |
| 泰文 | `th` | ✓ |  |
| 東加文 | `to` | ✓ | 2020-09-01 |
| 土耳其文 | `tr` | ✓ |  |
| 烏克蘭文 | `uk` | ✓ |  |
| 烏都文 | `ur` | ✓ |  |
| 烏玆別克文 | `uz` | ✓ |  |
| 越南文 | `vi` | ✓ |  |
| 威爾斯文 | `cy` | ✓ |  |
| 意第緒文 | `yi` | ✓ |  |
| 猶加敦馬雅文 | `yua` | ✓ |  |

---

## <a name="see-also"></a>請參閱

* [什麼是文字分析 API？](overview.md)   
