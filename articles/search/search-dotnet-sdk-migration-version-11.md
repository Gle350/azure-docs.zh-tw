---
title: 升級至 .NET SDK 11 版
titleSuffix: Azure Cognitive Search
description: 將程式碼遷移至舊版的 Azure 認知搜尋 .NET SDK 11 版。 了解新功能與必要的程式碼變更。
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 01/07/2021
ms.custom: devx-track-csharp
ms.openlocfilehash: c5f070f59df69bb186041af450e6ca922469d960
ms.sourcegitcommit: 8dd8d2caeb38236f79fe5bfc6909cb1a8b609f4a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98043739"
---
# <a name="upgrade-to-azure-cognitive-search-net-sdk-version-11"></a>升級至 Azure 認知搜尋 .NET SDK 11 版

如果您使用的是10.0 版或更舊版本的 [.NET SDK](/dotnet/api/overview/azure/search)，本文將協助您升級至第11版和 **Azure.Search.Documents** 用戶端程式庫。

第11版是經過完整重新設計的用戶端程式庫，由 Azure SDK 開發小組發行 (舊版是由 Azure 認知搜尋開發小組) 所產生。 此程式庫已經過重新設計，可與其他 Azure 用戶端程式庫更具一致性、相依于 [Azure Core](/dotnet/api/azure.core) 和 [System.Text.Js](/dotnet/api/system.text.json)，並針對一般工作執行熟悉的方法。

您在新版本中會注意到的一些主要差異包括：

+ 一個套件和程式庫，而非多個套件
+ 新的封裝名稱： `Azure.Search.Documents` 而不是 `Microsoft.Azure.Search` 。
+ 三個用戶端，而不是兩個： `SearchClient` 、 `SearchIndexClient` 、 `SearchIndexerClient`
+ 命名 Api 範圍和簡化部分工作的小型結構差異之間的差異

除了本文之外，您還可以複習 [變更記錄](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/CHANGELOG.md) 檔，以取得 .net SDK 11 版中的詳細變更清單。

## <a name="package-and-library-consolidation"></a>封裝和程式庫合併

第11版會將多個套件和程式庫合併成一個。 遷移後，您將會有較少的管理程式庫。

+ [Azure.Search.Documents 套件](https://www.nuget.org/packages/Azure.Search.Documents/)

+ [用戶端程式庫的 API 參考](/dotnet/api/overview/azure/search.documents-readme)

## <a name="client-differences"></a>用戶端差異

在適用的情況下，下表會對應兩個版本之間的用戶端程式庫。

| 作業範圍 | &nbsp;V10) 的搜尋 ( | Azure.Search.Documents &nbsp; (v11)  |
|---------------------|------------------------------|------------------------------|
| 用於查詢及填入索引的用戶端。 | [SearchIndexClient](/dotnet/api/azure.search.documents.indexes.searchindexclient) | [SearchClient](/dotnet/api/azure.search.documents.searchclient) |
| 用於索引、分析器、同義字對應的用戶端 | [SearchServiceClient](/dotnet/api/microsoft.azure.search.searchserviceclient) | [SearchIndexClient](/dotnet/api/azure.search.documents.indexes.searchindexclient) |
| 用於索引子、資料來源、技能集的用戶端 | [SearchServiceClient](/dotnet/api/microsoft.azure.search.searchserviceclient) | [SearchIndexerClient (**新**)](/dotnet/api/azure.search.documents.indexes.searchindexerclient) |

> [!Important]
> `SearchIndexClient` 存在於兩個版本中，但支援不同的專案。 在第10版中， `SearchIndexClient` 建立索引和其他物件。 在第11版中， `SearchIndexClient` 可以使用現有的索引。 為了避免在更新程式碼時產生混淆，請留意用戶端參考的更新順序。 遵循 [升級步驟](#UpgradeSteps) 中的順序應有助於減輕任何字串取代問題。

<a name="naming-differences"></a>

## <a name="naming-and-other-api-differences"></a>命名和其他 API 差異

除了先前所述的用戶端差異 (也會在這裡省略) ，已重新命名多個其他 Api，而且在某些情況下已重新設計。 類別名稱差異摘要如下所示。 這份清單並不完整，但會依工作將 API 變更分組，這對特定程式碼區塊的修訂可能很有説明。 如需 API 更新的詳細清單，請參閱 GitHub 上的 [變更記錄](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/CHANGELOG.md) 檔 `Azure.Search.Documents` 。

### <a name="authentication-and-encryption"></a>驗證和加密

| 第10版 | 第11版對等專案 |
|------------|-----------------------|
| [SearchCredentials](/dotnet/api/microsoft.azure.search.searchcredentials) | [AzureKeyCredential](/dotnet/api/azure.azurekeycredential) |
| `EncryptionKey` (已在 [預覽 SDK](https://www.nuget.org/packages/Microsoft.Azure.Search/8.0.0-preview) 中以正式運作的功能存在)  | [SearchResourceEncryptionKey](/dotnet/api/azure.search.documents.indexes.models.searchresourceencryptionkey) |

### <a name="indexes-analyzers-synonym-maps"></a>索引、分析器、同義字對應

| 第10版 | 第11版對等專案 |
|------------|-----------------------|
| [Index](/dotnet/api/microsoft.azure.documents.index) | [SearchIndex](/dotnet/api/azure.search.documents.indexes.models.searchindex) |
| [欄位](/dotnet/api/microsoft.azure.search.models.field) | [>searchfield](/dotnet/api/azure.search.documents.indexes.models.searchfield) |
| [DataType](/dotnet/api/microsoft.azure.search.models.datatype) | [SearchFieldDataType](/dotnet/api/azure.search.documents.indexes.models.searchfielddatatype) |
| [ItemError](/dotnet/api/microsoft.azure.search.models.itemerror) | [SearchIndexerError](/dotnet/api/azure.search.documents.indexes.models.searchindexererror) |
| [分析儀](/dotnet/api/microsoft.azure.search.models.analyzer) | [LexicalAnalyzer](/dotnet/api/azure.search.documents.indexes.models.lexicalanalyzer) (也 `AnalyzerName` `LexicalAnalyzerName`)  |
| [AnalyzeRequest](/dotnet/api/microsoft.azure.search.models.analyzerequest) | [AnalyzeTextOptions](/dotnet/api/azure.search.documents.indexes.models.analyzetextoptions) |
| [StandardAnalyzer](/dotnet/api/microsoft.azure.search.models.standardanalyzer) | [LuceneStandardAnalyzer](/dotnet/api/azure.search.documents.indexes.models.lucenestandardanalyzer) |
| [StandardTokenizer](/dotnet/api/microsoft.azure.search.models.standardtokenizer) | [LuceneStandardTokenizer](/dotnet/api/azure.search.documents.indexes.models.lucenestandardtokenizer) (也 `StandardTokenizerV2` `LuceneStandardTokenizerV2`)  |
| [TokenInfo](/dotnet/api/microsoft.azure.search.models.tokeninfo) | [AnalyzedTokenInfo](/dotnet/api/azure.search.documents.indexes.models.analyzedtokeninfo) |
| [Tokenizer](/dotnet/api/microsoft.azure.search.models.tokenizer) | [LexicalTokenizer](/dotnet/api/azure.search.documents.indexes.models.lexicaltokenizer) (也 `TokenizerName` `LexicalTokenizerName`)  |
| [SynonymMap。格式](/dotnet/api/microsoft.azure.search.models.synonymmap.format) | 無。 移除的參考 `Format` 。 |

欄位定義經過簡化： [SearchableField](/dotnet/api/azure.search.documents.indexes.models.searchablefield)、 [SimpleField](/dotnet/api/azure.search.documents.indexes.models.simplefield)、 [ComplexField](/dotnet/api/azure.search.documents.indexes.models.complexfield) 是新的 api，可用於建立欄位定義。

### <a name="indexers-datasources-skillsets"></a>索引子、資料來源、技能集

| 第10版 | 第11版對等專案 |
|------------|-----------------------|
| [索引編製程式](/dotnet/api/microsoft.azure.search.models.indexer) | [SearchIndexer](/dotnet/api/azure.search.documents.indexes.models.searchindexer) |
| [DataSource](/dotnet/api/microsoft.azure.search.models.datasource) | [SearchIndexerDataSourceConnection](/dotnet/api/azure.search.documents.indexes.models.searchindexerdatasourceconnection) |
| [技能](/dotnet/api/microsoft.azure.search.models.skill) | [SearchIndexerSkill](/dotnet/api/azure.search.documents.indexes.models.searchindexerskill) |
| [技能集](/dotnet/api/microsoft.azure.search.models.skillset) | [SearchIndexerSkillset](/dotnet/api/azure.search.documents.indexes.models.searchindexerskill) |
| [DataSourceType](/dotnet/api/microsoft.azure.search.models.datasourcetype) | [SearchIndexerDataSourceType](/dotnet/api/azure.search.documents.indexes.models.searchindexerdatasourcetype) |

### <a name="data-import"></a>資料匯入

| 第10版 | 第11版對等專案 |
|------------|-----------------------|
| [IndexAction](/dotnet/api/microsoft.azure.search.models.indexaction) | [IndexDocumentsAction](/dotnet/api/azure.search.documents.models.indexdocumentsaction) |
| [IndexBatch](/dotnet/api/microsoft.azure.search.models.indexbatch) | [IndexDocumentsBatch](/dotnet/api/azure.search.documents.models.indexdocumentsbatch) |

### <a name="query-definitions-and-results"></a>查詢定義和結果

| 第10版 | 第11版對等專案 |
|------------|-----------------------|
| [>documentsearchresult](/dotnet/api/microsoft.azure.search.models.documentsearchresult-1) | [>.searchresult](/dotnet/api/azure.search.documents.models.searchresult-1) 或 [>.searchresults](/dotnet/api/azure.search.documents.models.searchresults-1)，視結果為單一或多個檔而定。 |
| [DocumentSuggestResult](/dotnet/api/microsoft.azure.search.models.documentsuggestresult-1) | [SuggestResults](/dotnet/api/azure.search.documents.models.suggestresults-1) |
| [>searchparameters](/dotnet/api/microsoft.azure.search.models.searchparameters) |  [SearchOptions](/dotnet/api/azure.search.documents.searchoptions)  |

### <a name="json-serialization"></a>JSON 序列化

根據預設，Azure SDK 使用 [System.Text.Json](/dotnet/api/system.text.json) 進行 JSON 序列化，依賴這些 api 的功能來處理先前透過原生 [SerializePropertyNamesAsCamelCaseAttribute](/dotnet/api/microsoft.azure.search.models.serializepropertynamesascamelcaseattribute) 類別所執行的文字轉換，在新的程式庫中沒有對應的類別。

若要將屬性名稱序列化為 camelCase，您可以使用類似[此範例](https://github.com/Azure/azure-sdk-for-net/tree/d263f23aa3a28ff4fc4366b8dee144d4c0c3ab10/sdk/search/Azure.Search.Documents#use-c-types-for-search-results)) 的[JsonPropertyNameAttribute](/dotnet/api/system.text.json.serialization.jsonpropertynameattribute) (。

或者，您也可以設定[JsonSerializerOptions](/dotnet/api/system.text.json.jsonserializeroptions)中提供的[JsonNamingPolicy](/dotnet/api/system.text.json.jsonnamingpolicy) 。 下列 System.Text.Js程式碼範例（取自 camelCase）將 [示範如何](https://github.com/Azure/azure-sdk-for-net/blob/259df3985d9710507e2454e1591811f8b3a7ad5d/sdk/core/Microsoft.Azure.Core.Spatial/README.md#deserializing-documents) 使用，而不需要為每個屬性進行屬性：

```csharp
// Get the Azure Cognitive Search endpoint and read-only API key.
Uri endpoint = new Uri(Environment.GetEnvironmentVariable("SEARCH_ENDPOINT"));
AzureKeyCredential credential = new AzureKeyCredential(Environment.GetEnvironmentVariable("SEARCH_API_KEY"));

// Create serializer options with our converter to deserialize geographic points.
JsonSerializerOptions serializerOptions = new JsonSerializerOptions
{
    Converters =
    {
        new MicrosoftSpatialGeoJsonConverter()
    },
    PropertyNamingPolicy = JsonNamingPolicy.CamelCase
};

SearchClientOptions clientOptions = new SearchClientOptions
{
    Serializer = new JsonObjectSerializer(serializerOptions)
};

SearchClient client = new SearchClient(endpoint, "mountains", credential, clientOptions);
Response<SearchResults<Mountain>> results = client.Search<Mountain>("Rainier");
```

如果您使用 Newtonsoft.Json 進行 JSON 序列化，您可以使用類似的屬性傳入全域命名原則，或使用 [JsonSerializerSettings](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonSerializerSettings.htm)上的屬性。 如需相當於上述的範例，請參閱讀我檔案的 Newtonsoft.Js中的還原 [序列化檔範例](https://github.com/Azure/azure-sdk-for-net/blob/259df3985d9710507e2454e1591811f8b3a7ad5d/sdk/core/Microsoft.Azure.Core.Spatial.NewtonsoftJson/README.md) 。


<a name="WhatsNew"></a>

## <a name="whats-in-version-11"></a>第11版的功能

Azure 認知搜尋用戶端程式庫的每個版本都是以對應的 REST API 版本為目標。 REST API 會被視為服務的基礎，而個別的 Sdk 會包裝 REST API 的版本。 如果您想要更多有關特定物件或作業的背景，請以 .NET 開發人員的身分來查看 [REST API 檔](/rest/api/searchservice/) 。

第11版以 [2020-06-30 搜尋服務](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/search/data-plane/Azure.Search/preview/2020-06-30/searchservice.json)為目標。 由於第11版也是從頭開始建立的新用戶端程式庫，因此大部分的開發工作都著重于版本10的相等，但有些 REST API 功能支援仍在擱置中。

11.0 版完全支援下列物件和作業：

+ 索引建立與管理
+ 同義字對應的建立與管理
+ 除了地理空間篩選以外，所有的查詢類型和語法 () 
+ 為 Azure 資料來源編制索引的索引子物件和作業，包括資料來源和技能集

11.1 版新增下列內容：

+ 11.1 中新增的[FieldBuilder](/dotnet/api/azure.search.documents.indexes.fieldbuilder) () 
+ 在 11.1) 中加入[序列化程式屬性](/dotnet/api/azure.search.documents.searchclientoptions.serializer) (，以支援自訂序列化

### <a name="pending-features"></a>暫止功能

第11版尚無法使用下列版本10功能。 如果您需要這些功能，請在不受支援的情況下，保留在遷移的狀態。

+ 地理空間類型
+ [知識存放區](knowledge-store-concept-intro.md)

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>升級步驟

下列步驟可讓您逐步完成第一組必要的工作，特別是關於用戶端參考，藉此開始進行程式碼遷移。

1. 以滑鼠右鍵按一下您的專案參考，然後選取 [管理 NuGet 套件 ...]，以安裝 [Azure.Search.Documents 套件](https://www.nuget.org/packages/Azure.Search.Documents/) 在 Visual Studio。

1. 將 using 指示詞取代為 Azure。搜尋如下：

   ```csharp
   using Azure;
   using Azure.Search.Documents;
   using Azure.Search.Documents.Indexes;
   using Azure.Search.Documents.Indexes.Models;
   using Azure.Search.Documents.Models;
   ```

1. 若為需要 JSON 序列化的類別，請將取代為 `using Newtonsoft.Json` `using System.Text.Json.Serialization` 。

1. 修訂用戶端驗證碼。 在舊版中，您會使用用戶端物件上的屬性來設定 API 金鑰 (例如，SearchServiceClient) 的 [認證](/dotnet/api/microsoft.azure.search.searchserviceclient.credentials) 屬性。 在目前的版本中，使用 [AzureKeyCredential](/dotnet/api/azure.azurekeycredential) 類別將金鑰以認證的形式傳遞，如此一來，您就可以在需要的情況下更新 API 金鑰，而不需要建立新的用戶端物件。

   用戶端屬性已簡化為只有 `Endpoint` 、 `ServiceName` 和 `IndexName` (適當的) 。 下列範例會使用系統 [Uri](/dotnet/api/system.uri) 類別來提供端點和 [環境](/dotnet/api/system.environment) 類別，以讀取金鑰值：

   ```csharp
   Uri endpoint = new Uri(Environment.GetEnvironmentVariable("SEARCH_ENDPOINT"));
   AzureKeyCredential credential = new AzureKeyCredential(
      Environment.GetEnvironmentVariable("SEARCH_API_KEY"));
   SearchIndexClient indexClient = new SearchIndexClient(endpoint, credential);
   ```

1. 加入索引子相關物件的新用戶端參考。 如果您使用索引子、資料來源或技能集，請將用戶端參考變更為 [SearchIndexerClient](/dotnet/api/azure.search.documents.indexes.searchindexerclient)。 此用戶端是第11版中的新用戶端，而且沒有任何之前的版本。

1. 修訂集合和清單。 在新的 SDK 中，如果清單剛好包含 null 值，則所有清單都是唯讀的，以避免下游問題。 程式碼變更是將專案加入至清單。 例如，您可以依照下列方式加入字串，而不是將字串指派給 Select 屬性：

   ```csharp
   var options = new SearchOptions
    {
       SearchMode = SearchMode.All,
       IncludeTotalCount = true
    };

    // Select fields to return in results.
    options.Select.Add("HotelName");
    options.Select.Add("Description");
    options.Select.Add("Tags");
    options.Select.Add("Rooms");
    options.Select.Add("Rating");
    options.Select.Add("LastRenovationDate");
   ```

   Select、Facet、SearchFields、SourceFields、ScoringParameters 和 OrderBy 都是現在需要重建的所有清單。

1. 更新查詢和資料匯入的用戶端參考。 [SearchIndexClient](/dotnet/api/microsoft.azure.search.searchindexclient)的實例應變更為[SearchClient](/dotnet/api/azure.search.documents.searchclient)。 為了避免名稱混淆，請務必先攔截所有實例，再繼續進行下一個步驟。

1. 更新索引、同義字地圖和分析器物件的用戶端參考。 [SearchServiceClient](/dotnet/api/microsoft.azure.search.searchserviceclient)的實例應變更為[SearchIndexClient](/dotnet/api/microsoft.azure.search.searchindexclient)。 

1. 針對程式碼的其餘部分，更新類別、方法和屬性，以使用新程式庫的 Api。 [ [命名差異](#naming-differences) ] 區段是要啟動的位置，但您也可以檢查 [變更記錄](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/CHANGELOG.md)檔。

   如果您在尋找對等的 Api 時遇到問題，建議您記錄問題， [https://github.com/MicrosoftDocs/azure-docs/issues](https://github.com/MicrosoftDocs/azure-docs/issues) 讓我們可以改善檔或調查問題。

1. 重建方案。 修正任何組建錯誤或警告之後，您可以對應用程式進行其他變更，以利用 [新功能](#WhatsNew)。

<a name="ListOfChanges"></a>

## <a name="breaking-changes"></a>重大變更

由於程式庫和 Api 的清除變更，升級至第11版並不簡單，而且會導致您的程式碼不會再與第10版及更早版本相容的重大變更。 如需差異的完整評論，請參閱的 [變更記錄](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/CHANGELOG.md) 檔 `Azure.Search.Documents` 。

就服務版本更新而言，第11版中的程式碼變更與現有的功能有關 (而不只是重構) 的 Api，您會發現下列行為變更：

+ [BM25 排名演算法](index-ranking-similarity.md) 會以較新的技術取代先前的排名演算法。 新服務會自動使用此演算法。 針對現有的服務，您必須將參數設定為使用新的演算法。

+ Null 值的[排序結果](search-query-odata-orderby.md)已在此版本中變更，如果排序是，則會先出現 null 值，如果 `asc` 排序是，則為最後一個 `desc` 。 如果您撰寫程式碼來處理 null 值的排序方式，您應該在不再需要時，檢查並可能移除該程式碼。

## <a name="next-steps"></a>下一步

+ [Azure.Search.Documents 套件](https://www.nuget.org/packages/Azure.Search.Documents/)
+ [GitHub 範例](https://github.com/azure/azure-sdk-for-net/tree/Azure.Search.Documents_11.0.0/sdk/search/Azure.Search.Documents/samples)
+ [Azure.Search.Doc>ument API 參考](/dotnet/api/overview/azure/search.documents-readme)