---
title: Azure 自訂角色-Azure RBAC
description: 瞭解如何使用 Azure 角色型存取控制來建立 Azure 自訂角色 (Azure RBAC) 以進行 Azure 資源的更細緻存取管理。
services: active-directory
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.topic: conceptual
ms.workload: identity
ms.date: 12/15/2020
ms.author: rolyon
ms.openlocfilehash: 79aaeee942a6d46243ee1c72d5904484b8698ebe
ms.sourcegitcommit: 86acfdc2020e44d121d498f0b1013c4c3903d3f3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/17/2020
ms.locfileid: "97617318"
---
# <a name="azure-custom-roles"></a>Azure 自訂角色 (機器翻譯)

> [!IMPORTANT]
> 將管理群組新增至 `AssignableScopes` 的服務目前為預覽狀態。
> 此預覽版本是在沒有服務等級協定的情況下提供，不建議用於生產工作負載。 可能不支援特定功能，或可能已經限制功能。
> 如需詳細資訊，請參閱 [Microsoft Azure 預覽版增補使用條款](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)。

如果 [Azure 內建的角色](built-in-roles.md)無法滿足您組織的特定需求，您可以建立自己的自訂角色。 就像內建角色一樣，您可以在管理群組、訂用帳戶和資源群組範圍中，將自訂角色指派給使用者、群組和服務主體。

自訂角色可以在信任相同 Azure AD 目錄的訂用帳戶之間共用。 每個目錄的自訂角色限制為 **5000** 。  (Azure 德國和 Azure 中國的世紀，限制為2000個自訂角色。您可以使用 Azure 入口網站、Azure PowerShell、Azure CLI 或 REST API 來建立 ) 自訂角色。

## <a name="steps-to-create-a-custom-role"></a>建立自訂角色的步驟

以下是建立自訂角色的基本步驟。

1. 判斷您需要的許可權。

    當您建立自訂角色時，您需要知道可用來定義許可權的作業。 一般而言，您可以從使用現有的內建角色開始，然後針對您的需求進行修改。 您會將作業加入至 `Actions` `NotActions` [角色定義](role-definitions.md)的或屬性。 如果您有資料作業，您會將它們加入至 `DataActions` 或 `NotDataActions` 屬性。

    如需詳細資訊，請參閱下一節 [如何判斷您需要的許可權](#how-to-determine-the-permissions-you-need)。

1. 決定您要如何建立自訂角色。

    您可以使用 [Azure 入口網站](custom-roles-portal.md)、 [Azure PowerShell](custom-roles-powershell.md)、 [Azure CLI](custom-roles-cli.md)或 [REST API](custom-roles-rest.md)來建立自訂角色。

1. 建立自訂角色。

    最簡單的方式是使用 Azure 入口網站。 如需有關如何使用 Azure 入口網站建立自訂角色的步驟，請參閱 [使用 Azure 入口網站建立或更新 Azure 自訂角色](custom-roles-portal.md)。

1. 測試自訂角色。

    一旦具有自訂角色，您必須測試它來驗證是否如預期般運作。 稍後如需進行調整，您可以更新自訂角色。

## <a name="how-to-determine-the-permissions-you-need"></a>如何判斷您需要的許可權

Azure 具有數以千計的許可權，您可以將其包含在您的自訂角色中。 以下是一些可協助您決定要新增至自訂角色之許可權的方法：

- 查看現有的 [內建角色](built-in-roles.md)。

    您可能會想要修改現有的角色，或合併多個角色中所使用的許可權。

- 列出您想要授與存取權的 Azure 服務。

- 判斷 [對應至 Azure 服務的資源提供者](../azure-resource-manager/management/azure-services-resource-providers.md)。

    Azure 服務會透過 [資源提供者](../azure-resource-manager/management/overview.md)公開其功能和許可權。 例如，Microsoft 計算資源提供者會提供虛擬機器資源，而 Microsoft 帳單資源提供者會提供訂用帳戶和帳單資源。 瞭解資源提供者可協助您縮小並判斷自訂角色所需的許可權。

    當您使用 Azure 入口網站建立自訂角色時，您也可以藉由搜尋關鍵字來判斷資源提供者。 [使用 Azure 入口網站建立或更新 Azure 自訂角色](custom-roles-portal.md#step-4-permissions)時，會說明這項搜尋功能。

    ![使用資源提供者新增許可權窗格](./media/custom-roles-portal/add-permissions-provider.png)

- 搜尋 [可用的許可權](resource-provider-operations.md) 以尋找您想要包含的許可權。

    當您使用 Azure 入口網站建立自訂角色時，您可以依關鍵字搜尋許可權。 例如，您可以搜尋 *虛擬機器* 或 *帳單* 許可權。 您也可以下載擁有權限作為 CSV 檔案，然後搜尋此檔案。 [使用 Azure 入口網站建立或更新 Azure 自訂角色](custom-roles-portal.md#step-4-permissions)時，會說明這項搜尋功能。

    ![新增許可權清單](./media/custom-roles-portal/add-permissions-list.png)

## <a name="custom-role-example"></a>自訂角色範例

以下顯示使用 JSON 格式 Azure PowerShell 的自訂角色看起來的樣子。 此自訂角色可用於監視和重新啟動虛擬機器。

```json
{
  "Name": "Virtual Machine Operator",
  "Id": "88888888-8888-8888-8888-888888888888",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Authorization/*/read",
    "Microsoft.ResourceHealth/availabilityStatuses/read",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Insights/diagnosticSettings/*",
    "Microsoft.Support/*"
  ],
  "NotActions": [],
  "DataActions": [],
  "NotDataActions": [],
  "AssignableScopes": [
    "/subscriptions/{subscriptionId1}",
    "/subscriptions/{subscriptionId2}",
    "/providers/Microsoft.Management/managementGroups/{groupId1}"
  ]
}
```

以下顯示使用 Azure CLI 所顯示的相同自訂角色。

```json
[
  {
    "assignableScopes": [
      "/subscriptions/{subscriptionId1}",
      "/subscriptions/{subscriptionId2}",
      "/providers/Microsoft.Management/managementGroups/{groupId1}"
    ],
    "description": "Can monitor and restart virtual machines.",
    "id": "/subscriptions/{subscriptionId1}/providers/Microsoft.Authorization/roleDefinitions/88888888-8888-8888-8888-888888888888",
    "name": "88888888-8888-8888-8888-888888888888",
    "permissions": [
      {
        "actions": [
          "Microsoft.Storage/*/read",
          "Microsoft.Network/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action",
          "Microsoft.Authorization/*/read",
          "Microsoft.ResourceHealth/availabilityStatuses/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Insights/diagnosticSettings/*",
          "Microsoft.Support/*"
        ],
        "dataActions": [],
        "notActions": [],
        "notDataActions": []
      }
    ],
    "roleName": "Virtual Machine Operator",
    "roleType": "CustomRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

建立自訂角色時，它會以橙色資源圖示顯示在 Azure 入口網站中。

![自訂角色圖示](./media/custom-roles/roles-custom-role-icon.png)

## <a name="custom-role-properties"></a>自訂角色屬性

下表說明自訂角色屬性的意義。

| 屬性 | 必要 | 類型 | 描述 |
| --- | --- | --- | --- |
| `Name`</br>`roleName` | 是 | String | 自訂角色的顯示名稱。 當角色定義是管理群組或訂用帳戶層級資源時，角色定義可以用於多個共用相同 Azure AD 目錄的訂用帳戶。 此顯示名稱在 Azure AD 目錄範圍中必須是唯一的。 可以包含字母、數字、空格和特殊字元。 字元數目上限是 128。 |
| `Id`</br>`name` | 是 | String | 自訂角色的唯一識別碼。 針對 Azure PowerShell 和 Azure CLI，當您建立新角色時，會自動產生這個識別碼。 |
| `IsCustom`</br>`roleType` | 是 | String | 表示這是否為自訂角色。 `true` `CustomRole` 針對自訂角色設定為或。 `false` `BuiltInRole` 針對內建角色設定為或。 |
| `Description`</br>`description` | 是 | String | 自訂角色的描述。 可以包含字母、數字、空格和特殊字元。 字元數目上限是 1024。 |
| `Actions`</br>`actions` | 是 | String[] | 字串陣列，指定角色允許執行的管理作業。 如需詳細資訊，請參閱 [Actions](role-definitions.md#actions)。 |
| `NotActions`</br>`notActions` | 否 | String[] | 字串陣列，指定從所允許 `Actions` 中排除的管理作業。 如需詳細資訊，請參閱 [NotActions](role-definitions.md#notactions)。 |
| `DataActions`</br>`dataActions` | 否 | String[] | 字串陣列，指定角色允許對物件內資料執行的管理作業。 如果您使用建立自訂角色 `DataActions` ，則無法在管理群組範圍指派該角色。 如需詳細資訊，請參閱 [DataActions](role-definitions.md#dataactions)。 |
| `NotDataActions`</br>`notDataActions` | 否 | String[] | 字串陣列，指定從所允許 `DataActions` 中排除的資料作業。 如需詳細資訊，請參閱 [NotDataActions](role-definitions.md#notdataactions)。 |
| `AssignableScopes`</br>`assignableScopes` | 是 | String[] | 字串陣列，指定自訂角色可用於指派的範圍。 您只能在自訂角色中定義一個管理群組 `AssignableScopes` 。 將管理群組新增至 `AssignableScopes` 的服務目前為預覽狀態。 如需詳細資訊，請參閱 [AssignableScopes](role-definitions.md#assignablescopes)。 |

## <a name="wildcard-permissions"></a>萬用字元許可權

`Actions`、 `NotActions` 、 `DataActions` 和 `NotDataActions` 支援萬用字元 (`*`) 來定義許可權。 萬用字元 () 會將 `*` 許可權延伸到符合您提供之動作字串的所有專案。 例如，假設您想要加入與 Azure 成本管理和匯出相關的擁有權限。 您可以加入所有這些動作字串：

```
Microsoft.CostManagement/exports/action
Microsoft.CostManagement/exports/read
Microsoft.CostManagement/exports/write
Microsoft.CostManagement/exports/delete
Microsoft.CostManagement/exports/run/action
```

您可以只新增萬用字元字串，而不是加入所有這些字串。 例如，下列萬用字元字串相當於前五個字串。 這也包含任何未來可能新增的匯出許可權。

```
Microsoft.CostManagement/exports/*
```

您也可以在字串中使用多個萬用字元。 例如，下列字串代表成本管理的所有查詢許可權。

```
Microsoft.CostManagement/*/query/*
```

## <a name="who-can-create-delete-update-or-view-a-custom-role"></a>誰可以建立、刪除、更新或檢視自訂角色

就像內建角色一樣，`AssignableScopes` 屬性會指定角色可用於指派的範圍。 自訂角色的 `AssignableScopes` 屬性也會控制誰可以建立、刪除、更新或檢視自訂角色。

| 工作 | 作業 | 描述 |
| --- | --- | --- |
| 建立/刪除自訂角色 | `Microsoft.Authorization/ roleDefinitions/write` | 獲得授權可對自訂角色的所有 `AssignableScopes` 執行此作業的使用者，可以建立 (或刪除) 用於這些範圍的自訂角色。 例如，管理群組、訂用帳戶和資源群組的 [擁有](built-in-roles.md#owner) 者和 [使用者存取系統管理員](built-in-roles.md#user-access-administrator) 。 |
| 更新自訂角色 | `Microsoft.Authorization/ roleDefinitions/write` | 獲得授權可對自訂角色的所有 `AssignableScopes` 執行此作業的使用者，可以在這些範圍中更新自訂角色。 例如，管理群組、訂用帳戶和資源群組的 [擁有](built-in-roles.md#owner) 者和 [使用者存取系統管理員](built-in-roles.md#user-access-administrator) 。 |
| 檢視自訂角色 | `Microsoft.Authorization/ roleDefinitions/read` | 獲得授權可在範圍中執行此作業的使用者，可以檢視可指派給該範圍的自訂角色。 所有內建角色都允許自訂角色以供指派。 |

## <a name="custom-role-limits"></a>自訂角色限制

下列清單說明自訂角色的限制。

- 每個目錄最多可有 **5000** 個自訂角色。
- Azure 德國和 Azure 中國世紀最多可為每個目錄擁有2000個自訂角色。
- 您無法 `AssignableScopes` () 設定為根範圍 `"/"` 。
- 在中，您無法使用 (`*`) 的萬用字元 `AssignableScopes` 。 此萬用字元限制有助於確保使用者無法藉由更新角色定義來取得範圍的存取權。
- 您只能在自訂角色中定義一個管理群組 `AssignableScopes` 。 將管理群組新增至 `AssignableScopes` 的服務目前為預覽狀態。
- `DataActions`無法在管理群組範圍指派具有的自訂角色。
- Azure Resource Manager 不會驗證管理群組是否存在角色定義的可指派範圍中。

如需自訂角色和管理群組的詳細資訊，請參閱 [使用 Azure 管理群組來組織您的資源](../governance/management-groups/overview.md#azure-custom-role-definition-and-assignment)。

## <a name="input-and-output-formats"></a>輸入和輸出格式

若要使用命令列建立自訂角色，您通常會使用 JSON 來指定您要用於自訂角色的屬性。 視您使用的工具而定，輸入和輸出格式看起來會稍有不同。 此區段會根據工具列出輸入和輸出格式。

### <a name="azure-powershell"></a>Azure PowerShell

若要使用 Azure PowerShell 建立自訂角色，您必須提供下列輸入。

```json
{
  "Name": "",
  "Description": "",
  "Actions": [],
  "NotActions": [],
  "DataActions": [],
  "NotDataActions": [],
  "AssignableScopes": []
}
```

若要使用 Azure PowerShell 更新自訂角色，您必須提供下列輸入。 請注意，已 `Id` 加入屬性。 

```json
{
  "Name": "",
  "Id": "",
  "Description": "",
  "Actions": [],
  "NotActions": [],
  "DataActions": [],
  "NotDataActions": [],
  "AssignableScopes": []
}
```

以下顯示當您使用 Azure PowerShell 和 [ConvertTo-Json](/powershell/module/microsoft.powershell.utility/convertto-json) 命令列出自訂角色時的輸出範例。 

```json
{
  "Name": "",
  "Id": "",
  "IsCustom": true,
  "Description": "",
  "Actions": [],
  "NotActions": [],
  "DataActions": [],
  "NotDataActions": [],
  "AssignableScopes": []
}
```

### <a name="azure-cli"></a>Azure CLI

若要使用 Azure CLI 建立或更新自訂角色，您必須提供下列輸入。 當您使用 Azure PowerShell 建立自訂角色時，此格式會是相同的格式。

```json
{
  "Name": "",
  "Description": "",
  "Actions": [],
  "NotActions": [],
  "DataActions": [],
  "NotDataActions": [],
  "AssignableScopes": []
}
```

以下顯示當您使用 Azure CLI 列出自訂角色時的輸出範例。

```json
[
  {
    "assignableScopes": [],
    "description": "",
    "id": "",
    "name": "",
    "permissions": [
      {
        "actions": [],
        "dataActions": [],
        "notActions": [],
        "notDataActions": []
      }
    ],
    "roleName": "",
    "roleType": "CustomRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

### <a name="rest-api"></a>REST API

若要使用 REST API 建立或更新自訂角色，您必須提供下列輸入。 當您使用 Azure 入口網站建立自訂角色時，此格式會產生相同的格式。

```json
{
  "properties": {
    "roleName": "",
    "description": "",
    "assignableScopes": [],
    "permissions": [
      {
        "actions": [],
        "notActions": [],
        "dataActions": [],
        "notDataActions": []
      }
    ]
  }
}
```

以下顯示當您使用 REST API 列出自訂角色時的輸出範例。

```json
{
    "properties": {
        "roleName": "",
        "type": "CustomRole",
        "description": "",
        "assignableScopes": [],
        "permissions": [
            {
                "actions": [],
                "notActions": [],
                "dataActions": [],
                "notDataActions": []
            }
        ],
        "createdOn": "",
        "updatedOn": "",
        "createdBy": "",
        "updatedBy": ""
    },
    "id": "",
    "type": "Microsoft.Authorization/roleDefinitions",
    "name": ""
}
```

## <a name="next-steps"></a>後續步驟

- [教學課程：使用 Azure PowerShell 建立 Azure 自訂角色](tutorial-custom-role-powershell.md)
- [教學課程：使用 Azure CLI 建立 Azure 自訂角色](tutorial-custom-role-cli.md)
- [瞭解 Azure 角色定義](role-definitions.md)
- [針對 Azure RBAC 進行疑難排解](troubleshooting.md)
