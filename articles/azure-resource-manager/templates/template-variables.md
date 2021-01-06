---
title: 範本中的變數
description: 描述如何在 Azure Resource Manager 範本中定義 (ARM 範本) 的變數。
ms.topic: conceptual
ms.date: 11/24/2020
ms.openlocfilehash: 7f782f9c7d3107472a74fcab73290c4cebf73693
ms.sourcegitcommit: 2aa52d30e7b733616d6d92633436e499fbe8b069
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97934657"
---
# <a name="variables-in-arm-template"></a>ARM 範本中的變數

本文說明如何在 Azure Resource Manager 範本中定義和使用變數 (ARM 範本) 。 您可以使用變數來簡化您的範本。 您可以定義包含複雜運算式的變數，而不是在整個範本中重複複雜的運算式。 然後，您可以在整個範本中視需要參考該變數。

Resource Manager 在開始部署作業之前解析變數。 只要在範本中使用變數，Resource Manager 就會以已解析的值來加以取代。

每個變數的格式都必須符合其中一種 [資料類型](template-syntax.md#data-types)。

## <a name="define-variable"></a>定義變數

下列範例顯示變數定義。 它會建立儲存體帳戶名稱的字串值。 它會使用數個範本函式來取得參數值，並將它串連成唯一的字串。

```json
"variables": {
  "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
},
```

您無法使用 [參考](template-functions-resource.md#reference) 函數或區段中的任何 [清單](template-functions-resource.md#list) 函數 `variables` 。 這些函式會取得資源的執行時間狀態，而且在解析變數之前無法執行部署。

## <a name="use-variable"></a>使用變數

在範本中，您可以使用 [variables](template-functions-deployment.md#variables) 函數來參考參數的值。 下列範例顯示如何將變數用於資源屬性。

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageName')]",
    ...
  }
]
```

## <a name="configuration-variables"></a>設定變數

您可以定義包含設定環境相關值的變數。 您可以將變數定義為具有值的物件。 下列範例顯示的物件包含兩個環境的值- **測試** 和 **生產** 環境。

```json
"variables": {
  "environmentSettings": {
    "test": {
      "instanceSize": "Small",
      "instanceCount": 1
    },
    "prod": {
      "instanceSize": "Large",
      "instanceCount": 4
    }
  }
},
```

在中 `parameters` ，您會建立一個值，指出要使用的設定值。

```json
"parameters": {
  "environmentName": {
    "type": "string",
    "allowedValues": [
      "test",
      "prod"
    ]
  }
},
```

若要取得指定環境的設定，請一起使用變數和參數。

```json
"[variables('environmentSettings')[parameters('environmentName')].instanceSize]"
```

## <a name="example-templates"></a>範本的範例

下列範例示範使用變數的案例。

|範本  |描述  |
|---------|---------|
| [變數定義](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/variables.json) | 示範不同類型的變數。 範本不會部署任何資源。 它會建構變數值並傳回這些值。 |
| [組態變數](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/variablesconfigurations.json) | 示範如何使用可定義組態值的變數。 範本不會部署任何資源。 它會建構變數值並傳回這些值。 |
| [網路安全性規則](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json)和[參數檔案](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.parameters.json) | 建構正確格式的陣列，以便將安全性規則指派給網路安全性群組。 |

## <a name="next-steps"></a>後續步驟

* 若要瞭解變數的可用屬性，請參閱 [瞭解 ARM 範本的結構和語法](template-syntax.md)。
* 如需有關建立變數的建議，請參閱 [最佳做法-變數](template-best-practices.md#variables)。
