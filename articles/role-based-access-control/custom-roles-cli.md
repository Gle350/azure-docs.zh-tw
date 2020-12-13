---
title: 使用 Azure CLI 建立或更新 Azure 自訂角色-Azure RBAC
description: 瞭解如何使用 Azure CLI 和 Azure 角色型存取控制來列出、建立、更新或刪除 azure 自訂角色 (Azure RBAC) 。
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 3483ee01-8177-49e7-b337-4d5cb14f5e32
ms.service: role-based-access-control
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/17/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 31dabcf77f0db76047919fa76d00f1c5ed3c96d6
ms.sourcegitcommit: 1bdcaca5978c3a4929cccbc8dc42fc0c93ca7b30
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/13/2020
ms.locfileid: "97369135"
---
# <a name="create-or-update-azure-custom-roles-using-azure-cli"></a>使用 Azure CLI 建立或更新 Azure 自訂角色

> [!IMPORTANT]
> 將管理群組新增至 `AssignableScopes` 的服務目前為預覽狀態。
> 此預覽版本是在沒有服務等級協定的情況下提供，不建議用於生產工作負載。 可能不支援特定功能，或可能已經限制功能。
> 如需詳細資訊，請參閱 [Microsoft Azure 預覽版增補使用條款](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)。

如果 [Azure 內建的角色](built-in-roles.md)無法滿足您組織的特定需求，您可以建立自己的自訂角色。 本文說明如何使用 Azure CLI 列出、建立、更新或刪除自訂角色。

如需如何建立自訂角色的逐步教學課程，請參閱 [教學課程：使用 Azure CLI 建立 Azure 自訂角色](tutorial-custom-role-cli.md)。

## <a name="prerequisites"></a>Prerequisites

若要建立自訂角色，您需要：

- 建立自訂角色的權限，例如[擁有者](built-in-roles.md#owner)或[使用者存取管理員](built-in-roles.md#user-access-administrator)
- [Azure Cloud Shell](../cloud-shell/overview.md) 或 [Azure CLI](/cli/azure/install-azure-cli)

## <a name="list-custom-roles"></a>列出自訂角色

若要列出可供指派的自訂角色，請使用 [az role definition list](/cli/azure/role/definition#az-role-definition-list)。 下列範例會列出目前訂用帳戶中的所有自訂角色。

```azurecli
az role definition list --custom-role-only true --output json --query '[].{roleName:roleName, roleType:roleType}'
```

```json
[
  {
    "roleName": "My Management Contributor",
    "type": "CustomRole"
  },
  {
    "roleName": "My Service Reader Role",
    "type": "CustomRole"
  },
  {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole"
  }
]
```

## <a name="list-a-custom-role-definition"></a>列出自訂角色定義

若要列出自訂角色定義，請使用 [az 角色定義清單](/cli/azure/role/definition#az-role-definition-list)。 這是您將用於內建角色的相同命令。

```azurecli
az role definition list --name {roleName}
```

下列範例會列出 *虛擬機器操作員* 角色定義：

```azurecli
az role definition list --name "Virtual Machine Operator"
```

```json
[
  {
    "assignableScopes": [
      "/subscriptions/{subscriptionId}"
    ],
    "description": "Can monitor and restart virtual machines.",
    "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/00000000-0000-0000-0000-000000000000",
    "name": "00000000-0000-0000-0000-000000000000",
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

下列範例只會列出 *虛擬機器操作員* 角色的動作：

```azurecli
az role definition list --name "Virtual Machine Operator" --output json --query '[].permissions[0].actions'
```

```json
[
  [
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
  ]
]
```

## <a name="create-a-custom-role"></a>建立自訂角色

若要建立自訂角色，請使用 [az role definition create](/cli/azure/role/definition#az-role-definition-create)。 角色定義可以是 JSON 描述或包含 JSON 描述的檔案路徑。

```azurecli
az role definition create --role-definition {roleDefinition}
```

下列範例會建立一個名為 Virtual Machine Operator 的自訂角色。 這個自訂角色會指派 Microsoft.Compute、Microsoft.Storage 和 Microsoft.Network 資源提供者之所有讀取作業的存取權，以及指派啟動、重新啟動和監視虛擬機器的存取權。 這個自訂角色可用於兩個訂用帳戶中。 這個範例使用 JSON 檔案做為輸入。

vmoperator.json

```json
{
  "Name": "Virtual Machine Operator",
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
    "Microsoft.Support/*"
  ],
  "NotActions": [

  ],
  "AssignableScopes": [
    "/subscriptions/{subscriptionId1}",
    "/subscriptions/{subscriptionId2}"
  ]
}
```

```azurecli
az role definition create --role-definition ~/roles/vmoperator.json
```

## <a name="update-a-custom-role"></a>更新自訂角色

若要更新自訂角色，請先使用 [az role definition list](/cli/azure/role/definition#az-role-definition-list) 來擷取角色定義。 接著，對角色定義進行想要的變更。 最後，使用 [az role definition update](/cli/azure/role/definition#az-role-definition-update)儲存更新的角色定義。

```azurecli
az role definition update --role-definition {roleDefinition}
```

下列範例會將 *diagnosticSettings/* 作業加入至 `Actions` `AssignableScopes` *虛擬機器操作員* 自訂角色的管理群組，並將其新增至。 將管理群組新增至 `AssignableScopes` 的服務目前為預覽狀態。

vmoperator.json

```json
{
  "Name": "Virtual Machine Operator",
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
  "NotActions": [

  ],
  "AssignableScopes": [
    "/subscriptions/{subscriptionId1}",
    "/subscriptions/{subscriptionId2}",
    "/providers/Microsoft.Management/managementGroups/marketing-group"
  ]
}
```

```azurecli
az role definition update --role-definition ~/roles/vmoperator.json
```

## <a name="delete-a-custom-role"></a>刪除自訂角色

若要刪除自訂角色，請使用 [az role definition delete](/cli/azure/role/definition#az-role-definition-delete)。 若要指定要刪除的角色，請使用角色名稱或角色識別碼。 若要決定角色識別碼，請使用 [az role definition list](/cli/azure/role/definition#az-role-definition-list)。

```azurecli
az role definition delete --name {roleNameOrId}
```

下列範例會刪除 *虛擬機器操作員* 自訂角色。

```azurecli
az role definition delete --name "Virtual Machine Operator"
```

## <a name="next-steps"></a>後續步驟

- [教學課程：使用 Azure CLI 建立 Azure 自訂角色](tutorial-custom-role-cli.md)
- [Azure 自訂角色](custom-roles.md) (機器翻譯)
- [Azure 資源提供者作業](resource-provider-operations.md)