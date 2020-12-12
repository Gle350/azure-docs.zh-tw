---
title: 範本函式-數值
description: 描述在 Azure Resource Manager 範本中使用的函式 (ARM 範本) 以使用數位。
ms.topic: conceptual
ms.date: 11/18/2020
ms.openlocfilehash: f3687581d94f80cc923614a0655da1813bd5c97b
ms.sourcegitcommit: dfc4e6b57b2cb87dbcce5562945678e76d3ac7b6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2020
ms.locfileid: "97359705"
---
# <a name="numeric-functions-for-arm-templates"></a>ARM 範本的數值函數

Resource Manager 提供下列函式，可在 Azure Resource Manager 範本中使用整數 (ARM 範本) ：

* [add](#add)
* [copyIndex](#copyindex)
* [div](#div)
* [float](#float)
* [int](#int)
* [max](#max)
* [min](#min)
* [mod](#mod)
* [mul](#mul)
* [sub](#sub)

[!INCLUDE [Bicep preview](../../../includes/resource-manager-bicep-preview.md)]

## <a name="add"></a>add

`add(operand1, operand2)`

傳回兩個所提供整數加總後的數字。 `add`Bicep 中不支援此函數。 請改用 `+` 運算子。

### <a name="parameters"></a>參數

| 參數 | 必要 | 類型 | 說明 |
|:--- |:--- |:--- |:--- |
|operand1 |是 |int |要新增的第一個數字。 |
|operand2 |是 |int |要新增的第二個數字。 |

### <a name="return-value"></a>傳回值

整數，其中包含參數的總和。

### <a name="example"></a>範例

下列[範例範本](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/add.json)會新增兩個參數。

# <a name="json"></a>[JSON](#tab/json)

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "first": {
      "type": "int",
      "defaultValue": 5,
      "metadata": {
        "description": "First integer to add"
      }
    },
    "second": {
      "type": "int",
      "defaultValue": 3,
      "metadata": {
        "description": "Second integer to add"
      }
    }
  },
  "resources": [
  ],
  "outputs": {
    "addResult": {
      "type": "int",
      "value": "[add(parameters('first'), parameters('second'))]"
    }
  }
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
param first int = 5
param second int = 3

output addResult int = first + second
```

---

上述範例中具有預設值的輸出如下：

| 名稱 | 類型 | 值 |
| ---- | ---- | ----- |
| addResult | Int | 8 |

## <a name="copyindex"></a>copyIndex

`copyIndex(loopName, offset)`

傳回反覆項目迴圈的索引。

### <a name="parameters"></a>參數

| 參數 | 必要 | 類型 | 說明 |
|:--- |:--- |:--- |:--- |
| loopName | 否 | 字串 | 用來取得反覆項目的迴圈名稱。 |
| Offset |否 |int |要加入到以零為起始之反覆項目值的數字。 |

### <a name="remarks"></a>備註

這個函式一律搭配 **copy** 物件使用。 如果未針對 **offset** 提供任何值，則會傳回目前的反覆項目值。 反覆項目值是從零開始。

**LoopName** 屬性可讓您指定 copyIndex 要參考資源的反覆項目還是屬性的反覆項目。 如果未提供任何 **loopName** 的值，就會使用目前的資源類型反覆項目。 逐一查看屬性時，請提供 **loopName** 的值。

如需使用複製的詳細資訊，請參閱：

* [ARM 範本中的資源反復專案](copy-resources.md)
* [ARM 範本中的屬性反復專案](copy-properties.md)
* [ARM 範本中的變數反復專案](copy-variables.md)
* [ARM 範本中的輸出反復專案](copy-outputs.md)

### <a name="example"></a>範例

下列範例顯示複製迴圈以及名稱中所包含的索引值。

# <a name="json"></a>[JSON](#tab/json)

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageCount": {
      "type": "int",
      "defaultValue": 2
    }
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2019-04-01",
      "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": {},
      "copy": {
        "name": "storagecopy",
        "count": "[parameters('storageCount')]"
      }
    }
  ],
  "outputs": {}
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

> [!NOTE]
> 迴圈和尚未 `copyIndex` 在 Bicep 中執行。  請參閱 [迴圈](https://github.com/Azure/bicep/blob/main/docs/spec/loops.md)。

---

### <a name="return-value"></a>傳回值

整數，代表目前的反覆項目索引。

## <a name="div"></a>div

`div(operand1, operand2)`

傳回兩個所提供整數相除後的商。 `div`Bicep 中不支援此函數。 請改用 `/` 運算子。

### <a name="parameters"></a>參數

| 參數 | 必要 | 類型 | 說明 |
|:--- |:--- |:--- |:--- |
| operand1 |是 |int |被除數。 |
| operand2 |是 |int |除數。 不可為0。 |

### <a name="return-value"></a>傳回值

代表除法的整數。

### <a name="example"></a>範例

下列[範例範本](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/div.json)會使用一個參數除以另一個參數。

# <a name="json"></a>[JSON](#tab/json)

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "first": {
      "type": "int",
      "defaultValue": 8,
      "metadata": {
        "description": "Integer being divided"
      }
    },
    "second": {
      "type": "int",
      "defaultValue": 3,
      "metadata": {
        "description": "Integer used to divide"
      }
    }
  },
  "resources": [
  ],
  "outputs": {
    "divResult": {
      "type": "int",
      "value": "[div(parameters('first'), parameters('second'))]"
    }
  }
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
param first int = 8
param second int = 3

output addResult int = first / second
```

---

上述範例中具有預設值的輸出如下：

| 名稱 | 類型 | 值 |
| ---- | ---- | ----- |
| divResult | Int | 2 |

## <a name="float"></a>FLOAT

`float(arg1)`

將值轉換為浮點數。 您只會在將自訂參數傳遞給應用程式 (例如邏輯應用程式) 時使用這個函式。 `float`Bicep 中不支援此函數。  請參閱 [支援32位整數以外的數位類型](https://github.com/Azure/bicep/issues/486)。

### <a name="parameters"></a>參數

| 參數 | 必要 | 類型 | 說明 |
|:--- |:--- |:--- |:--- |
| arg1 |是 |字串或整數 |要轉換為浮點數的值。 |

### <a name="return-value"></a>傳回值

浮點數。

### <a name="example"></a>範例

下列範例顯示如何使用 float 將參數傳遞給邏輯應用程式：

# <a name="json"></a>[JSON](#tab/json)

```json
{
  "type": "Microsoft.Logic/workflows",
  "properties": {
    ...
    "parameters": {
      "custom1": {
        "value": "[float('3.0')]"
      },
      "custom2": {
        "value": "[float(3)]"
      },
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

> [!NOTE]
> `float`Bicep 中不支援此函數。  請參閱 [支援32位整數以外的數位類型](https://github.com/Azure/bicep/issues/486)。

---

## <a name="int"></a>int

`int(valueToConvert)`

將指定的值轉換成整數。

### <a name="parameters"></a>參數

| 參數 | 必要 | 類型 | 說明 |
|:--- |:--- |:--- |:--- |
| valueToConvert |是 |字串或整數 |要轉換成整數的值。 |

### <a name="return-value"></a>傳回值

轉換值的整數。

### <a name="example"></a>範例

下列[範例範本](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/int.json)會將使用者提供的參數值轉換為整數。

# <a name="json"></a>[JSON](#tab/json)

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "stringToConvert": {
      "type": "string",
      "defaultValue": "4"
    }
  },
  "resources": [
  ],
  "outputs": {
    "intResult": {
      "type": "int",
      "value": "[int(parameters('stringToConvert'))]"
    }
  }
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
param stringToConvert string = '4'

output inResult int = int(stringToConvert)
```

---

上述範例中具有預設值的輸出如下：

| 名稱 | 類型 | 值 |
| ---- | ---- | ----- |
| intResult | Int | 4 |

## <a name="max"></a>max

`max (arg1)`

傳回整數陣列的最大值，或以逗號分隔的整數清單。

### <a name="parameters"></a>參數

| 參數 | 必要 | 類型 | 說明 |
|:--- |:--- |:--- |:--- |
| arg1 |是 |整數的陣列，或以逗號分隔的整數清單 |要用來取得最大值的集合。 |

### <a name="return-value"></a>傳回值

整數，代表集合中的最大值。

### <a name="example"></a>範例

下列[範例範本](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/max.json)顯示如何搭配使用 max 與陣列和整數清單：

# <a name="json"></a>[JSON](#tab/json)

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "arrayToTest": {
      "type": "array",
      "defaultValue": [ 0, 3, 2, 5, 4 ]
    }
  },
  "resources": [],
  "outputs": {
    "arrayOutput": {
      "type": "int",
      "value": "[max(parameters('arrayToTest'))]"
    },
    "intOutput": {
      "type": "int",
      "value": "[max(0,3,2,5,4)]"
    }
  }
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
param arrayToTest array = [
  0
  3
  2
  5
  4
]

output arrayOutPut int = max(arrayToTest)
output intOutput int = max(0,3,2,5,4)
```

---

上述範例中具有預設值的輸出如下：

| 名稱 | 類型 | 值 |
| ---- | ---- | ----- |
| arrayOutput | Int | 5 |
| intOutput | Int | 5 |

## <a name="min"></a>分鐘

`min (arg1)`

傳回整數陣列的最小值，或以逗號分隔的整數清單。

### <a name="parameters"></a>參數

| 參數 | 必要 | 類型 | 說明 |
|:--- |:--- |:--- |:--- |
| arg1 |是 |整數的陣列，或以逗號分隔的整數清單 |要用來取得最小值的集合。 |

### <a name="return-value"></a>傳回值

整數，代表集合中的最小值。

### <a name="example"></a>範例

下列[範例範本](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/min.json)顯示如何搭配使用 min 與陣列和整數清單：

# <a name="json"></a>[JSON](#tab/json)

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "arrayToTest": {
      "type": "array",
      "defaultValue": [ 0, 3, 2, 5, 4 ]
    }
  },
  "resources": [],
  "outputs": {
    "arrayOutput": {
      "type": "int",
      "value": "[min(parameters('arrayToTest'))]"
    },
    "intOutput": {
      "type": "int",
      "value": "[min(0,3,2,5,4)]"
    }
  }
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
param arrayToTest array = [
  0
  3
  2
  5
  4
]

output arrayOutPut int = min(arrayToTest)
output intOutput int = min(0,3,2,5,4)
```

---

上述範例中具有預設值的輸出如下：

| 名稱 | 類型 | 值 |
| ---- | ---- | ----- |
| arrayOutput | Int | 0 |
| intOutput | Int | 0 |

## <a name="mod"></a>mod

`mod(operand1, operand2)`

傳回兩個所提供整數相除後的餘數。 `mod`Bicep 中不支援此函數。 請改用 `%` 運算子。

### <a name="parameters"></a>參數

| 參數 | 必要 | 類型 | 說明 |
|:--- |:--- |:--- |:--- |
| operand1 |是 |int |被除數。 |
| operand2 |是 |int |用來除的數位不能為0。 |

### <a name="return-value"></a>傳回值

代表餘數的整數。

### <a name="example"></a>範例

下列[範例範本](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/mod.json)傳回的是一個參數除以另一個參數的餘數。

# <a name="json"></a>[JSON](#tab/json)

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "first": {
      "type": "int",
      "defaultValue": 7,
      "metadata": {
        "description": "Integer being divided"
      }
    },
    "second": {
      "type": "int",
      "defaultValue": 3,
      "metadata": {
        "description": "Integer used to divide"
      }
    }
  },
  "resources": [
  ],
  "outputs": {
    "modResult": {
      "type": "int",
      "value": "[mod(parameters('first'), parameters('second'))]"
    }
  }
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
param first int = 7
param second int = 3

output modResult int = first % second
```

---

上述範例中具有預設值的輸出如下：

| 名稱 | 類型 | 值 |
| ---- | ---- | ----- |
| modResult | Int | 1 |

## <a name="mul"></a>mul

`mul(operand1, operand2)`

傳回兩個所提供整數相乘後的數字。 `mul`Bicep 中不支援此函數。 請改用 `*` 運算子。

### <a name="parameters"></a>參數

| 參數 | 必要 | 類型 | 說明 |
|:--- |:--- |:--- |:--- |
| operand1 |是 |int |要相乘的第一個數字。 |
| operand2 |是 |int |要相乘的第二個數字。 |

### <a name="return-value"></a>傳回值

代表乘法的整數。

### <a name="example"></a>範例

下列[範例範本](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/mul.json)會使用一個參數乘以另一個參數。

# <a name="json"></a>[JSON](#tab/json)

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "first": {
      "type": "int",
      "defaultValue": 5,
      "metadata": {
        "description": "First integer to multiply"
      }
    },
    "second": {
      "type": "int",
      "defaultValue": 3,
      "metadata": {
        "description": "Second integer to multiply"
      }
    }
  },
  "resources": [
  ],
  "outputs": {
    "mulResult": {
      "type": "int",
      "value": "[mul(parameters('first'), parameters('second'))]"
    }
  }
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
param first int = 5
param second int = 3

output mulResult int = first * second
```

---

上述範例中具有預設值的輸出如下：

| 名稱 | 類型 | 值 |
| ---- | ---- | ----- |
| mulResult | Int | 15 |

## <a name="sub"></a>sub

`sub(operand1, operand2)`

傳回兩個所提供整數相減後的數字。 `sub`Bicep 中不支援此函數。 請改用 `-` 運算子。

### <a name="parameters"></a>參數

| 參數 | 必要 | 類型 | 說明 |
|:--- |:--- |:--- |:--- |
| operand1 |是 |int |減數。 |
| operand2 |是 |int |被減數。 |

### <a name="return-value"></a>傳回值

代表減法的整數。

### <a name="example"></a>範例

下列[範例範本](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/sub.json)會使用一個參數乘以另一個參數。

# <a name="json"></a>[JSON](#tab/json)

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "first": {
      "type": "int",
      "defaultValue": 7,
      "metadata": {
        "description": "Integer subtracted from"
      }
    },
    "second": {
      "type": "int",
      "defaultValue": 3,
      "metadata": {
        "description": "Integer to subtract"
      }
    }
  },
  "resources": [
  ],
  "outputs": {
    "subResult": {
      "type": "int",
      "value": "[sub(parameters('first'), parameters('second'))]"
    }
  }
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
param first int = 7
param second int = 3

output subResult int = first - second
```

---

上述範例中具有預設值的輸出如下：

| 名稱 | 類型 | 值 |
| ---- | ---- | ----- |
| subResult | Int | 4 |

## <a name="next-steps"></a>後續步驟

* 如需 ARM 範本中各區段的說明，請參閱 [瞭解 arm 範本的結構和語法](template-syntax.md)。
* 若要在建立資源類型時反覆運算指定的次數，請參閱 [ARM 範本中的資源反復](copy-resources.md)專案。
