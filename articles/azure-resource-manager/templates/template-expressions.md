---
title: 範本語法和運算式
description: 描述)  (ARM 範本 Azure Resource Manager 範本的宣告式 JSON 語法。
ms.topic: conceptual
ms.date: 03/17/2020
ms.openlocfilehash: 44a386ed849771dfba717c8d1414e64422d0c7bd
ms.sourcegitcommit: ab829133ee7f024f9364cd731e9b14edbe96b496
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/28/2020
ms.locfileid: "97797038"
---
# <a name="syntax-and-expressions-in-arm-templates"></a>ARM 範本中的語法和運算式

Azure Resource Manager 範本 (ARM 範本的基本語法) 是 JavaScript 物件標記法 (JSON) 。 不過，您可以使用運算式來擴充範本內可用的 JSON 值。  運算式開頭和結尾括號分別是 `[` 和 `]`。 部署範本時，會評估運算式的值。 運算式可以傳回字串、整數、布林值、陣列或物件。

範本運算式不能超過24576個字元。

## <a name="use-functions"></a>使用函式

Azure Resource Manager 提供您可以在範本中使用的 [函數](template-functions.md) 。 下列範例顯示在參數的預設值中使用函數的運算式：

```json
"parameters": {
  "location": {
    "type": "string",
    "defaultValue": "[resourceGroup().location]"
  }
},
```

在運算式中，語法 `resourceGroup()` 會呼叫 Resource Manager 提供的其中一個函式，以便在範本內使用。 在此情況下，它是 [resourceGroup](template-functions-resource.md#resourcegroup) 函數。 和在 JavaScript 中相同，函式呼叫的格式為 `functionName(arg1,arg2,arg3)`。 此語法 `.location` 會從該函式所傳回的物件中抓取一個屬性。

範本函數和其參數不區分大小寫。 例如，Resource Manager 會解析 `variables('var1')` 和 `VARIABLES('VAR1')` 相同。 進行評估時，除非函式明確地修改 case (例如 `toUpper` 或 `toLower`) ，否則函數會保留大小寫。 某些資源類型可能會有與函式的評估方式不同的案例需求。

若要將字串值作為參數傳遞至函式，請使用單引號。

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]"
```

無論部署至資源群組、訂用帳戶、管理群組或租使用者，大部分的函式都有相同的作用。 下列函式根據範圍有限制：

* [resourceGroup](template-functions-resource.md#resourcegroup) -只能在資源群組的部署中使用。
* [resourceId](template-functions-resource.md#resourceid) -可用於任何範圍，但有效的參數會根據範圍而變更。
* [訂](template-functions-resource.md#subscription) 用帳戶-只能在資源群組或訂用帳戶的部署中使用。

## <a name="escape-characters"></a>逸出字元

若要讓常值字串以左括弧開頭 `[` 並以右括弧結尾 `]` ，但未將它解釋為運算式，請加入額外的括弧，以使用來啟動字串 `[[` 。 例如，變數：

```json
"demoVar1": "[[test value]"
```

解析為 `[test value]` 。

但是，如果常值字串不是以括弧結尾，請不要將第一個括弧 escape。 例如，變數：

```json
"demoVar2": "[test] value"
```

解析為 `[test] value` 。

若要在運算式中將雙引號換成雙引號，例如在範本中新增 JSON 物件，請使用反斜線。

```json
"tags": {
    "CostCenter": "{\"Dept\":\"Finance\",\"Environment\":\"Production\"}"
},
```

傳入參數值時，使用 escape 字元取決於指定參數值的位置。 如果您在範本中設定預設值，您需要額外的左括弧。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "demoParam1":{
            "type": "string",
            "defaultValue": "[[test value]"
        }
    },
    "resources": [],
    "outputs": {
        "exampleOutput": {
            "type": "string",
            "value": "[parameters('demoParam1')]"
        }
    }
}
```

如果您使用預設值，則範本會傳回 `[test value]` 。

但是，如果您透過命令列傳遞參數值，則會以字面方式解讀字元。 使用下列方式部署先前的範本：

```azurepowershell
New-AzResourceGroupDeployment -ResourceGroupName demoGroup -TemplateFile azuredeploy.json -demoParam1 "[[test value]"
```

傳回 `[[test value]`。 請改用：

```azurepowershell
New-AzResourceGroupDeployment -ResourceGroupName demoGroup -TemplateFile azuredeploy.json -demoParam1 "[test value]"
```

從參數檔傳遞值時，會套用相同的格式。 這些字元會以字面方式解讀。 搭配上述範本使用時，下列參數檔會傳回 `[test value]` ：

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "demoParam1": {
            "value": "[test value]"
        }
   }
}
```

## <a name="null-values"></a>Null 值

若要將屬性設定為 null，您可以使用 `null` 或 `[json('null')]`。 當您提供做為參數時， [json 函數](template-functions-object.md#json) 會傳回空的物件 `null` 。 在這兩種情況下，Resource Manager 範本都會將它視為屬性不存在。

```json
"stringValue": null,
"objectValue": "[json('null')]"
```

## <a name="next-steps"></a>後續步驟

* 如需範本函式的完整清單，請參閱 [ARM 範本函數](template-functions.md)。
* 如需範本檔案的詳細資訊，請參閱 [瞭解 ARM 範本的結構和語法](template-syntax.md)。
