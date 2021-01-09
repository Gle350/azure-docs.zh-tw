---
title: 將標記新增至數位 twins
titleSuffix: Azure Digital Twins
description: 瞭解如何在數位 twins 上執行標記
author: baanders
ms.author: baanders
ms.date: 7/22/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 9a1a55bdf21b74116450ca32f66d891f1aa206d3
ms.sourcegitcommit: 8dd8d2caeb38236f79fe5bfc6909cb1a8b609f4a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98045405"
---
# <a name="add-tags-to-digital-twins"></a>將標記新增至數位 twins 

您可以使用標記的概念，進一步識別和分類您的數位 twins。 尤其是，使用者可能會想要從 Azure 數位 Twins 實例內的現有系統（例如 [大海撈針標記](https://project-haystack.org/doc/TagModel)）複寫標記。 

本檔說明可以用來在數位 twins 上執行標記的模式。

標記會先加入至 [模型](concepts-models.md) 中的屬性，以描述數位對應項。 然後，當對應項根據模型建立時，就會在對應項上設定該屬性。 之後，您可以在 [查詢](concepts-query-language.md) 中使用這些標記來識別和篩選您的 twins。

## <a name="marker-tags"></a>標記標記 

**標記標記** 是用來標記或分類數位對應項的簡單字串，例如 "blue" 或 "red"。 這個字串是標籤的名稱，而且標記標記沒有任何有意義的值，標記只是在目前狀態 (或缺少) 時才有意義。 

### <a name="add-marker-tags-to-model"></a>將標記標記新增至模型 

標記標記會模型化為的 [DTDL](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md) 對應 `string` `boolean` 。 此布林值 `mapValue` 會被忽略，因為標記的存在是很重要的。 

以下是將標記標記實作為屬性之對應項模型的摘錄：

:::code language="json" source="~/digital-twins-docs-samples/models/tags.json" range="2-16":::

### <a name="add-marker-tags-to-digital-twins"></a>將標記標記新增至數位 twins

一旦 `tags` 屬性是數位對應項模型的一部分，您就可以藉由設定這個屬性的值，在數位對應項中設定標記標記。 

以下範例會填入 `tags` 三個 twins 的標記：

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/twin_operations_other.cs" id="TagPropertiesMarker":::

### <a name="query-with-marker-tags"></a>使用標記標記進行查詢

一旦將標記新增至數位 twins 之後，就可以使用標記來篩選查詢中的 twins。 

以下查詢會取得已標記為 "red" 的所有 twins： 

:::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="QueryMarkerTags1":::

您也可以合併標記以進行更複雜的查詢。 以下查詢會取得所有四捨五入的 twins，而不是紅色： 

:::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="QueryMarkerTags2":::

## <a name="value-tags"></a>值標記 

**值標記** 是一組索引鍵/值組，用來為每個標記提供一個值，例如 `"color": "blue"` 或 `"color": "red"` 。 一旦建立值標記之後，也可以藉由略過標記的值，將其當做標記標記。 

### <a name="add-value-tags-to-model"></a>將值標記新增至模型 

值標記會模型化為的 [DTDL](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md) 對應 `string` `string` 。 `mapKey`和 `mapValue` 都是很重要的。 

以下是將值標記實作為屬性之對應項模型的摘錄：

:::code language="json" source="~/digital-twins-docs-samples/models/tags.json" range="17-31":::

### <a name="add-value-tags-to-digital-twins"></a>將數值標記新增至數位 twins

如同標記標記，您可以從模型設定這個屬性的值，以設定數位對應項中的 value 標記 `tags` 。 若要使用值標記做為標記標籤，您可以將 `tagValue` 欄位設定為空字串值 (`""`) 。 

以下範例會填入 `tags` 三個 twins 的值：

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/twin_operations_other.cs" id="TagPropertiesValue":::

請注意 `red` ， `purple` 在此範例中，和會用來做為標記標記。

### <a name="query-with-value-tags"></a>具有值標記的查詢

如同標記標記，您可以使用值標記來篩選查詢中的 twins。 您也可以同時使用值標記和標記標記。

在上述範例中， `red` 是用來做為標記標記。 請記住，這是一個查詢，可取得標記為 "red" 的所有 twins： 

:::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="QueryMarkerTags1":::

以下查詢會取得小型 (值標記) ，而非紅色的所有實體： 

:::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="QueryMarkerValueTags":::

## <a name="next-steps"></a>下一步

深入瞭解如何設計和管理數位對應項模型：
* [操作說明：管理自訂模型](how-to-manage-model.md)

深入瞭解查詢對應項圖形：
* [*How to：查詢對應項圖形*](how-to-query-graph.md)