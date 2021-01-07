---
title: 教學課程：建立自訂原則定義
description: 在本教學課程中，針對 Azure 原則製作自訂原則定義，以在您的 Azure 資源上強制執行自訂商務規則。
ms.date: 10/05/2020
ms.topic: tutorial
ms.openlocfilehash: 817e6f494b024b9a789f39a4101236f64d8fa0cd
ms.sourcegitcommit: 6d6030de2d776f3d5fb89f68aaead148c05837e2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/05/2021
ms.locfileid: "97882886"
---
# <a name="tutorial-create-a-custom-policy-definition"></a>教學課程：建立自訂原則定義

自訂原則定義可讓客戶定義自己的 Azure 使用規則。 這些規則通常會強制執行：

- 安全性作法
- 成本管理
- 組織專屬規則 (例如命名或位置)

無論導致客戶建立自訂原則的商務誘因是什麼，用來定義新自訂原則的步驟都一樣。

在建立自訂原則之前，請先看一下[原則範例](../samples/index.md)，以了解是否已有符合您需求的原則存在。

用來建立自訂原則的方法會遵循下列步驟：

> [!div class="checklist"]
> - 識別您的商務需求
> - 將每項需求與 Azure 資源屬性進行對應
> - 將屬性與別名進行對應
> - 決定要使用的效果
> - 撰寫原則定義

## <a name="prerequisites"></a>必要條件

如果您沒有 Azure 訂用帳戶，請在開始前建立[免費帳戶](https://azure.microsoft.com/free/)。

## <a name="identify-requirements"></a>識別需求

在建立原則定義之前，請務必先了解原則的用途。 在本教學課程中，我們會使用一般的企業安全性需求作為目標，來說明所需執行的步驟：

- 每個儲存體帳戶都必須針對 HTTPS 加以啟用
- 每個儲存體帳戶都必須針對 HTTP 加以停用

您的需求應該清楚地識別「要」和「不要」的資源狀態。

我們雖已定義資源的預期狀態，但尚未定義不符合規範的資源所需的狀態。 Azure 原則支援許多[效果](../concepts/effects.md)。 在本教學課程中，我們會將商務需求定義為防止建立不符合商務規則規範的資源。 為了達到此目標，我們會使用[拒絕](../concepts/effects.md#deny)效果。 我們也想要可以暫停特定指派原則的選項。 因此，我們會使用[停用](../concepts/effects.md#disabled)效果，然後讓該效果成為原則定義中的[參數](../concepts/definition-structure.md#parameters)。

## <a name="determine-resource-properties"></a>決定資源屬性

根據商務需求，Azure 原則所要稽核的 Azure 資源是儲存體帳戶。 不過，我們不知道要在原則定義中使用的屬性。 Azure 原則會針對資源的 JSON 表示法進行評估，因此我們必須了解該資源上可用的屬性。

用來決定 Azure 資源屬性的方式很多。 在本教學課程中，我們會就每一種方式進行探討：

- 適用於 VS Code 的Azure 原則擴充功能
- Azure Resource Manager 範本 (ARM 範本)
  - 匯出現有資源
  - 建立體驗
  - 快速入門範本 (GitHub)
  - 範本參考文件
- Azure 資源總管

### <a name="view-resources-in-vs-code-extension"></a>檢視 VS Code 擴充功能中的資源

[VS Code 擴充功能](../how-to/extension-for-vscode.md#search-for-and-view-resources)可以用來瀏覽環境中的資源，以及檢視每個資源上的 Resource Manager 屬性。

### <a name="arm-templates"></a>ARM 範本

對於包含您所要管理屬性的 [ARM](../../../azure-resource-manager/templates/template-tutorial-use-template-reference.md)，其查看方式有好幾種。

#### <a name="existing-resource-in-the-portal"></a>入口網站中的現有資源

若要尋找屬性，最簡單的方式是查看相同類型的現有資源。 已使用所要強制執行的設定進行設定的資源，也會提供可用來比較的值。
在 Azure 入口網站中，查看該項資源的 [匯出範本] 頁面 (在 [設定] 下方)。

> [!WARNING]
> Azure 入口網站所匯出的 ARM 範本無法直接插入至 [deployIfNotExists](../concepts/effects.md#deployifnotexists) 原則定義中 ARM 範本的 `deployment` 屬性。

:::image type="content" source="../media/create-custom-policy-definition/export-template.png" alt-text="Azure 入口網站中現有資源的匯出範本頁面的螢幕擷取畫面。" border="false":::

對儲存體帳戶執行此操作，就會顯示類似此範例的範本：

```json
...
"resources": [{
    "comments": "Generalized from resource: '/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount'.",
    "type": "Microsoft.Storage/storageAccounts",
    "sku": {
        "name": "Standard_LRS",
        "tier": "Standard"
    },
    "kind": "Storage",
    "name": "[parameters('storageAccounts_mystorageaccount_name')]",
    "apiVersion": "2018-07-01",
    "location": "westus",
    "tags": {
        "ms-resource-usage": "azure-cloud-shell"
    },
    "scale": null,
    "properties": {
        "networkAcls": {
            "bypass": "AzureServices",
            "virtualNetworkRules": [],
            "ipRules": [],
            "defaultAction": "Allow"
        },
        "supportsHttpsTrafficOnly": false,
        "encryption": {
            "services": {
                "file": {
                    "enabled": true
                },
                "blob": {
                    "enabled": true
                }
            },
            "keySource": "Microsoft.Storage"
        }
    },
    "dependsOn": []
}]
...
```

[屬性] 底下是名為 **supportsHttpsTrafficOnly**、且設定為 **false** 的值。 這個屬性似乎就是我們要尋找的屬性。 此外，該資源的 **類型** 是 **Microsoft.Storage/storageAccounts**。 該類型可讓我們將原則限制為僅限此類型的資源。

#### <a name="create-a-resource-in-the-portal"></a>在入口網站建立資源

另一種透過入口網站的方式是資源建立體驗。 在透過入口網站建立儲存體帳戶時，[進階] 索引標籤下有 [需要安全性傳輸] 選項。 此屬性具有 [停用] 和 [啟用] 選項。 資訊圖示會有額外的文字，可確認此選項或許就是我們想要的屬性。 不過，入口網站不會在此畫面上告訴我們屬性名稱。

在 [檢閱 + 建立] 索引標籤上，頁面底部會有用來 **下載自動化的範本** 的連結。 選取連結就會開啟範本，以建立我們所設定的資源。 在此案例中，我們會看到兩項關鍵資訊：

```json
...
"supportsHttpsTrafficOnly": {
    "type": "bool"
}
...
"properties": {
    "accessTier": "[parameters('accessTier')]",
    "supportsHttpsTrafficOnly": "[parameters('supportsHttpsTrafficOnly')]"
}
...
```

此資訊會告訴我們屬性類型，並確認 **supportsHttpsTrafficOnly** 就是我們要尋找的屬性。

#### <a name="quickstart-templates-on-github"></a>GitHub 上的快速入門範本

GitHub 上的 [Azure 快速入門範本](https://github.com/Azure/azure-quickstart-templates)有數百個針對不同資源所建置的 ARM 範本。 這些範本非常適合用來找出您要尋找的資源屬性。 某些屬性看起來可能像是您要尋找的屬性，但其所控制的項目不同。

#### <a name="resource-reference-docs"></a>資源參考文件

若要驗證 **supportsHttpsTrafficOnly** 是否為正確屬性，請在該儲存體提供者上查看 [儲存體帳戶資源](/azure/templates/microsoft.storage/2018-07-01/storageaccounts)的 ARM 範本參考。 屬性物件有一份有效參數清單。 選取 [StorageAccountPropertiesCreateParameters-object](/azure/templates/microsoft.storage/2018-07-01/storageaccounts#storageaccountpropertiescreateparameters-object) 連結即可顯示所能接受屬性的資料表。 **supportsHttpsTrafficOnly** 存在，且其描述符合我們為了符合商務需求而所要尋找的內容。

### <a name="azure-resource-explorer"></a>Azure 資源總管

另一種瀏覽 Azure 資源的方式是透過 [Azure 資源總管](https://resources.azure.com) (預覽)。 此工具會使用您訂用帳戶的內容，因此您必須使用 Azure 認證向該網站進行驗證。 通過驗證後，即可依提供者、訂用帳戶、資源群組和資源來進行瀏覽。

找出儲存體帳戶資源，並查看其屬性。 我們在這裡也看到 **supportsHttpsTrafficOnly** 屬性。 選取 [文件] 索引標籤，我們會看到屬性描述符合我們稍早在參考文件中找到的資訊。

## <a name="find-the-property-alias"></a>尋找屬性別名

我們已識別出資源屬性，但我們需要將該屬性與[別名](../concepts/definition-structure.md#aliases)進行對應。

有幾種方式可決定 Azure 資源的別名。 在本教學課程中，我們會就每一種方式進行探討：

- 適用於 VS Code 的Azure 原則擴充功能
- Azure CLI
- Azure PowerShell

### <a name="get-aliases-in-vs-code-extension"></a>取得 VS Code 擴充功能中的別名

VS Code 擴充功能的 Azure 原則擴充功能，可讓您輕鬆地瀏覽資源及[探索別名](../how-to/extension-for-vscode.md#discover-aliases-for-resource-properties)。

> [!NOTE]
> VS Code 擴充功能只會公開 Resource Manager 模式屬性，而不會顯示任何[資源提供者模式](../concepts/definition-structure.md#mode)屬性。

### <a name="azure-cli"></a>Azure CLI

在 Azure CLI 中，`az provider` 命令群組可用來搜尋資源別名。 我們會根據稍早取得的 Azure 資源相關詳細資料，來篩選 **Microsoft.Storage** 命名空間。

```azurecli-interactive
# Login first with az login if not using Cloud Shell

# Get Azure Policy aliases for type Microsoft.Storage
az provider show --namespace Microsoft.Storage --expand "resourceTypes/aliases" --query "resourceTypes[].aliases[].name"
```

在結果中，我們會看到名為 **supportsHttpsTrafficOnly**、且為儲存體帳戶所支援的別名。 存在此別名表示我們可以撰寫原則來強制執行商務需求！

### <a name="azure-powershell"></a>Azure PowerShell

在 Azure PowerShell 中，`Get-AzPolicyAlias` Cmdlet 可用來搜尋資源別名。 我們會根據稍早取得的 Azure 資源相關詳細資料，來篩選 **Microsoft.Storage** 命名空間。

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Use Get-AzPolicyAlias to list aliases for Microsoft.Storage
(Get-AzPolicyAlias -NamespaceMatch 'Microsoft.Storage').Aliases
```

和 Azure CLI 一樣，結果會顯示名為 **supportsHttpsTrafficOnly**、且為儲存體帳戶所支援的別名。

## <a name="determine-the-effect-to-use"></a>決定要使用的效果

決定要如何處理不符合規範的資源，與決定要先評估什麼資源有著幾乎一樣的重要性。 針對不符合規範的資源，其每個可能的回應稱為[效果](../concepts/effects.md)。 效果會控制不符合規範的資源是否要加以記錄、封鎖、是否有附加的資料，或是否有與其相關聯的部署，而可讓資源恢復符合規範的狀態。

在我們範例中，因為不想在 Azure 環境中建立不符合規範的資源，所以拒絕是我們想要的效果。 第一個優異的原則效果選擇是稽核，其可先決定原則的影響範圍，再將原則設定為拒絕。 若要讓變更每一指派的效果變得更容易，有一種方法是將效果參數化。 請參閱下面的[參數](#parameters)，來了解其操作方式。

## <a name="compose-the-definition"></a>撰寫定義

針對我們打算管理的資源，我們現在已有其屬性詳細資料和別名。 接下來，我們會撰寫原則規則本身。 如果您還不熟悉原則的語言，請參考[原則定義結構](../concepts/definition-structure.md)來了解如何建構原則定義。 以下是原則定義樣貌的空白範本：

```json
{
    "properties": {
        "displayName": "<displayName>",
        "description": "<description>",
        "mode": "<mode>",
        "parameters": {
                <parameters>
        },
        "policyRule": {
            "if": {
                <rule>
            },
            "then": {
                "effect": "<effect>"
            }
        }
    }
}
```

### <a name="metadata"></a>中繼資料

前三個元件是原則的中繼資料。 由於我們知道為何要建立規則，所以為這些元件提供值並不難。 [模式](../concepts/definition-structure.md#mode)主要是和標記與資源位置有關。 我們並不需要限制只對支援標記的資源進行評估，因此會對 **mode** 使用 all 這個值。

```json
"displayName": "Deny storage accounts not using only HTTPS",
"description": "Deny storage accounts not using only HTTPS. Checks the supportsHttpsTrafficOnly property on StorageAccounts.",
"mode": "all",
```

### <a name="parameters"></a>參數

雖然我們不會使用參數來變更評估，但我們會使用參數來允許變更用於進行疑難排解的 **效果**。 我們會定義 **effectType** 參數，並將其限制為只能使用 **Deny** 和 **Disabled**。 這兩個選項符合我們的商務需求。 已完成的參數區塊如下列範例所示：

```json
"parameters": {
    "effectType": {
        "type": "string",
        "defaultValue": "Deny",
        "allowedValues": [
            "Deny",
            "Disabled"
        ],
        "metadata": {
            "displayName": "Effect",
            "description": "Enable or disable the execution of the policy"
        }
    }
},
```

### <a name="policy-rule"></a>原則規則

撰寫[原則規則](../concepts/definition-structure.md#policy-rule)是建置自訂原則定義時的最後一個步驟。 我們已識別兩個要測試的陳述式：

- 儲存體帳戶 **type** 是 **Microsoft.Storage/storageAccounts**
- 儲存體帳戶 **supportsHttpsTrafficOnly** 不是 **true**

這兩個陳述式都必須為 true，所以我們會使用 **allOf** [邏輯運算子](../concepts/definition-structure.md#logical-operators)。 我們會將 **effectType** 參數傳遞至效果，而不是發出靜態宣告。 已完成的規則如下列範例所示：

```json
"if": {
    "allOf": [
        {
            "field": "type",
            "equals": "Microsoft.Storage/storageAccounts"
        },
        {
            "field": "Microsoft.Storage/storageAccounts/supportsHttpsTrafficOnly",
            "notEquals": "true"
        }
    ]
},
"then": {
    "effect": "[parameters('effectType')]"
}
```

### <a name="completed-definition"></a>完成的定義

原則的三個組件全都定義好之後，完成的定義如下：

```json
{
    "properties": {
        "displayName": "Deny storage accounts not using only HTTPS",
        "description": "Deny storage accounts not using only HTTPS. Checks the supportsHttpsTrafficOnly property on StorageAccounts.",
        "mode": "all",
        "parameters": {
            "effectType": {
                "type": "string",
                "defaultValue": "Deny",
                "allowedValues": [
                    "Deny",
                    "Disabled"
                ],
                "metadata": {
                    "displayName": "Effect",
                    "description": "Enable or disable the execution of the policy"
                }
            }
        },
        "policyRule": {
            "if": {
                "allOf": [
                    {
                        "field": "type",
                        "equals": "Microsoft.Storage/storageAccounts"
                    },
                    {
                        "field": "Microsoft.Storage/storageAccounts/supportsHttpsTrafficOnly",
                        "notEquals": "true"
                    }
                ]
            },
            "then": {
                "effect": "[parameters('effectType')]"
            }
        }
    }
}
```

完成的定義可用來建立新的原則。 入口網站和每個 SDK (Azure CLI、Azure PowerShell 和 REST API) 會以不同的方式接受定義，因此請檢閱其各自的命令，以驗證正確的使用方式。 然後使用參數化的效果將其指派至適當的資源，以管理儲存體帳戶的安全性。

## <a name="clean-up-resources"></a>清除資源

如果您已完成使用本教學課程中的資源，請使用下列步驟來刪除前面建立的任何指派或定義：

1. 選取 Azure 原則頁面左側 [製作] 下的 [定義] (如果您嘗試刪除指派，則選取 [指派])。

1. 搜尋您要移除的新計畫或原則定義 (或指派)。

1. 以滑鼠右鍵按一下資料列，或選取定義 (或指派) 結尾的省略符號，然後選取 [刪除定義] (或 [刪除指派])。

## <a name="review"></a>檢閱

在本教學課程中，您已成功完成下列工作：

> [!div class="checklist"]
> - 識別您的商務需求
> - 將每項需求與 Azure 資源屬性進行對應
> - 將屬性與別名進行對應
> - 決定要使用的效果
> - 撰寫原則定義

## <a name="next-steps"></a>後續步驟

接下來，請使用自訂原則定義來建立及指派原則：

> [!div class="nextstepaction"]
> [建立及指派原則定義](../how-to/programmatically-create.md#create-and-assign-a-policy-definition)
