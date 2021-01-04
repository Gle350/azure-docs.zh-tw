---
title: '同義字 c # 範例'
titleSuffix: Azure Cognitive Search
description: '在此 c # 範例中，您將瞭解如何將同義字功能新增至 Azure 認知搜尋中的索引。 同義字對應是一份對等詞彙清單。 支援同義字的欄位會展開查詢，以包含使用者提供的詞彙和所有相關的同義字。'
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/18/2020
ms.custom: devx-track-csharp
ms.openlocfilehash: 50d5d73e71b8129f061ec49b363a0ebb13d22bdf
ms.sourcegitcommit: e7152996ee917505c7aba707d214b2b520348302
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/20/2020
ms.locfileid: "97704651"
---
# <a name="example-add-synonyms-for-azure-cognitive-search-in-c"></a>範例：在 C 中新增 Azure 認知搜尋的同義字#

同義字可藉由比對在語意上視為等於輸入詞彙的詞彙來展開查詢。 例如，您可能希望 "car" 比對包含 "automobile" 或 "vehicle" 詞彙的文件。 

在 Azure 認知搜尋中，同義字定義于 *同義字對應* 中，透過 *對應規則* 來建立對等詞彙的關聯。 此範例涵蓋使用現有的索引來新增和使用同義字的基本步驟。

在此範例中，您將瞭解如何：

> [!div class="checklist"]
> * 使用 [SynonymMap 類別](/dotnet/api/azure.search.documents.indexes.models.synonymmap)建立同義字地圖。 
> * 在應支援透過同義字進行查詢擴充的欄位上設定 [SynonymMapsName 屬性](/dotnet/api/azure.search.documents.indexes.models.searchfield.synonymmapnames) 。

您可以如往常般查詢啟用同義字的欄位。 存取同義字不需要額外的查詢語法。

您可以建立多個同義字對應、將它們張貼為可供任何索引使用的全服務資源，然後參照哪一個要在欄位層級使用。 在查詢時，除了搜尋索引之外，Azure 認知搜尋會在同義字對應中執行查閱（如果在查詢中使用的欄位上有指定的話）。

> [!NOTE]
> 同義字可以用程式設計的方式建立，但不能在入口網站中建立。

## <a name="prerequisites"></a>必要條件

教學課程包含下列需求︰

* [Visual Studio](https://www.visualstudio.com/downloads/)
* [Azure 認知搜尋服務](search-create-service-portal.md)
* [Azure.Search.Documents 套件](https://www.nuget.org/packages/Azure.Search.Documents/)

如果您不熟悉 .NET 用戶端程式庫，請參閱 [如何在 .net 中使用 Azure 認知搜尋](search-howto-dotnet-sdk.md)。

## <a name="sample-code"></a>範例程式碼

您可以在 [GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms)上找到此範例中所使用之範例應用程式的完整原始碼。

## <a name="overview"></a>總覽

使用前後查詢來示範同義字的值。 在此範例中，範例應用程式會執行查詢，並在填入兩份檔的範例「飯店」索引上傳回結果。 首先，應用程式會使用未出現在索引中的詞彙和片語來執行搜尋查詢。 其次，此程式碼會啟用同義字功能，然後重新發出相同的查詢，這次會根據同義字對應中的相符專案傳回結果。 

下列程式碼示範整體流程。

```csharp
static void Main(string[] args)
{
   SearchIndexClient indexClient = CreateSearchIndexClient();

   Console.WriteLine("Cleaning up resources...\n");
   CleanupResources(indexClient);

   Console.WriteLine("Creating index...\n");
   CreateHotelsIndex(indexClient);

   SearchClient searchClient = indexClient.GetSearchClient("hotels");

   Console.WriteLine("Uploading documents...\n");
   UploadDocuments(searchClient);

   SearchClient searchClientForQueries = CreateSearchClientForQueries();

   RunQueriesWithNonExistentTermsInIndex(searchClientForQueries);

   Console.WriteLine("Adding synonyms...\n");
   UploadSynonyms(indexClient);

   Console.WriteLine("Enabling synonyms in the test index...\n");
   EnableSynonymsInHotelsIndexSafely(indexClient);
   Thread.Sleep(10000); // Wait for the changes to propagate

   RunQueriesWithNonExistentTermsInIndex(searchClientForQueries);

   Console.WriteLine("Complete.  Press any key to end application...\n");

   Console.ReadKey();
}
```

## <a name="before-queries"></a>「之前」查詢

在 `RunQueriesWithNonExistentTermsInIndex` 中，使用 "five star"、"internet" 和 "economy AND hotel" 發出搜尋查詢。

片語查詢（例如「五顆星」）必須以引號括住，而且可能也需要根據您的用戶端來換用字元。

```csharp
Console.WriteLine("Search the entire index for the phrase \"five star\":\n");
results = searchClient.Search<Hotel>("\"five star\"", searchOptions);
WriteDocuments(results);

Console.WriteLine("Search the entire index for the term 'internet':\n");
results = searchClient.Search<Hotel>("internet", searchOptions);
WriteDocuments(results);

Console.WriteLine("Search the entire index for the terms 'economy' AND 'hotel':\n");
results = searchClient.Search<Hotel>("economy AND hotel", searchOptions);
WriteDocuments(results);
```

這兩個已編制索引的檔都不包含詞彙，所以我們會從第一個取得下列輸出 `RunQueriesWithNonExistentTermsInIndex` ：  **沒有符合的檔**。

## <a name="enable-synonyms"></a>啟用同義字

在執行「before」查詢之後，範例程式碼會啟用同義字。 啟用同義字的程序包含兩步驟。 首先，定義並上傳同義字規則。 其次，設定欄位以使用它們。 `UploadSynonyms` 和 `EnableSynonymsInHotelsIndex` 會簡要說明此程序。

1. 將同義字對應新增到您的搜尋服務。 在 `UploadSynonyms` 中，我們會在同義字對應'desc-synonymmap' 中定義四個規則並上傳至服務。

   ```csharp
   private static void UploadSynonyms(SearchIndexClient indexClient)
   {
      var synonymMap = new SynonymMap("desc-synonymmap", "hotel, motel\ninternet,wifi\nfive star=>luxury\neconomy,inexpensive=>budget");

      indexClient.CreateOrUpdateSynonymMap(synonymMap);
   }
   ```

1. 設定可搜尋的欄位，以使用索引定義中的同義字對應。 在 `AddSynonymMapsToFields` 中，我們會將 `SynonymMapNames` 屬性設定為新上傳的同義字對應名稱，以在 `category` 和 `tags` 兩個欄位上啟用同義字。

   ```csharp
   private static SearchIndex AddSynonymMapsToFields(SearchIndex index)
   {
      index.Fields.First(f => f.Name == "category").SynonymMapNames.Add("desc-synonymmap");
      index.Fields.First(f => f.Name == "tags").SynonymMapNames.Add("desc-synonymmap");
      return index;
   }
   ```

   當您新增同義字對應時，不需要重建索引。 您可以將同義字對應新增到您的服務，然後修改任何索引中的現有欄位定義，以使用新的同義字對應。 新增屬性對於索引可用性沒有任何影響。 停用欄位的同義字也是如此。 您只要將 `SynonymMapNames` 屬性設定為空白清單。

   ```csharp
   index.Fields.First(f => f.Name == "category").SynonymMapNames.Add("desc-synonymmap");
   ```

## <a name="after-queries"></a>「之後」查詢

上傳同義字對應並將索引更新為使用同義字對應之後，第二個 `RunQueriesWithNonExistentTermsInIndex` 就會呼叫下列輸出︰

```dos
Search the entire index for the phrase "five star":

Name: Fancy Stay        Category: Luxury        Tags: [pool, view, wifi, concierge]

Search the entire index for the term 'internet':

Name: Fancy Stay        Category: Luxury        Tags: [pool, view, wifi, concierge]

Search the entire index for the terms 'economy' AND 'hotel':

Name: Roach Motel       Category: Budget        Tags: [motel, budget]
```

第一個查詢會尋找 `five star=>luxury` 規則所產生的文件。 第二個查詢會使用 `internet,wifi` 展開搜尋，而第三個查詢會同時使用 `hotel, motel` 和 `economy,inexpensive=>budget` 來尋找相符的文件。

新增同義字會全然改變搜尋經驗。 在此範例中，即使索引中的檔是相關的，原始查詢還是無法傳回有意義的結果。 啟用同義字，我們就可以展開索引以包含常用的詞彙，而不需變更索引中的基礎資料。

## <a name="clean-up-resources"></a>清除資源

在範例之後進行清除的最快方法是刪除包含 Azure 認知搜尋服務的資源群組。 您現在可以刪除資源群組，以永久刪除當中所包含的所有項目。 在入口網站中，資源群組名稱位在 Azure 認知搜尋服務的 [概觀] 頁面上。

## <a name="next-steps"></a>後續步驟

此範例示範 c # 程式碼中的同義字功能，以建立和貼上對應規則，然後在查詢上呼叫同義字對應。 您可以在 [.NET SDK](/dotnet/api/overview/azure/search.documents-readme) 和 [REST API](/rest/api/searchservice/) 參考文件中找到其他資訊。

> [!div class="nextstepaction"]
> [如何在 Azure 認知搜尋中使用同義字](search-synonyms.md)