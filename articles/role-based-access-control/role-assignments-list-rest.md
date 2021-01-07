---
title: 使用 REST API 列出 Azure 角色指派-Azure RBAC
description: 瞭解如何使用 REST API 和 Azure 角色型存取控制 (Azure RBAC) ，判斷使用者、群組、服務主體或受控識別有哪些資源可以存取哪些資源。
services: active-directory
documentationcenter: na
author: rolyon
manager: mtillman
editor: ''
ms.assetid: 1f90228a-7aac-4ea7-ad82-b57d222ab128
ms.service: role-based-access-control
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: how-to
ms.date: 05/06/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 7d00c40a021bbe087d906fd6d9b767188a7b169a
ms.sourcegitcommit: f6f928180504444470af713c32e7df667c17ac20
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/07/2021
ms.locfileid: "97964318"
---
# <a name="list-azure-role-assignments-using-the-rest-api"></a>使用 REST API 列出 Azure 角色指派

[!INCLUDE [Azure RBAC definition list access](../../includes/role-based-access-control/definition-list.md)] 本文說明如何使用 REST API 列出角色指派。

> [!NOTE]
> 如果您的組織具有外包管理功能給使用 [Azure 委派資源管理](../lighthouse/concepts/azure-delegated-resource-management.md)的服務提供者，此服務提供者所授權的角色指派將不會顯示在此處。

## <a name="list-role-assignments"></a>列出角色指派

在 Azure RBAC 中，若要列出存取權，您可以列出角色指派。 若要列出角色指派，請使用其中一個[角色指派 - 列出](/rest/api/authorization/roleassignments/list) REST API。 若要精簡您的結果，請指定範圍和選擇性篩選條件。

1. 從下列要求著手：

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter={filter}
    ```

1. 在 URI 中，將 *{scope}* 取代為要列出角色指派的範圍。

    > [!div class="mx-tableFixed"]
    > | 影響範圍 | 類型 |
    > | --- | --- |
    > | `providers/Microsoft.Management/managementGroups/{groupId1}` | 管理群組 |
    > | `subscriptions/{subscriptionId1}` | 訂用帳戶 |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | 資源群組 |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1` | 資源 |

    在先前的範例中，microsoft 是參考 App Service 實例的資源提供者。 同樣地，您可以使用任何其他資源提供者，並指定範圍。 如需詳細資訊，請參閱 [Azure 資源提供者和類型](../azure-resource-manager/management/resource-providers-and-types.md) ，以及支援的 [azure 資源提供者作業](resource-provider-operations.md)。  
     
1. 將 *{filter}* 取代為您要針對角色指派清單篩選套用的條件。

    > [!div class="mx-tableFixed"]
    > | Filter | 描述 |
    > | --- | --- |
    > | `$filter=atScope()` | 僅列出指定範圍的角色指派，而不包含子範圍內的角色指派。 |
    > | `$filter=assignedTo('{objectId}')` | 列出指定之使用者或服務主體的角色指派。<br/>如果使用者是具有角色指派之群組的成員，也會列出該角色指派。 此篩選準則可轉移給群組，這表示如果使用者是群組的成員，而且該群組是具有角色指派之另一個群組的成員，則也會列出該角色指派。<br/>此篩選器只接受使用者或服務主體的物件識別碼。 您無法傳遞群組的物件識別碼。 |
    > | `$filter=atScope()+and+assignedTo('{objectId}')` | 列出指定之使用者或服務主體的角色指派，以及指定範圍內的角色指派。 |
    > | `$filter=principalId+eq+'{objectId}'` | 列出指定之使用者、群組或服務主體的角色指派。 |

下列要求會列出訂用帳戶範圍中指定使用者的所有角色指派：

```http
GET https://management.azure.com/subscriptions/{subscriptionId1}/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=atScope()+and+assignedTo('{objectId1}')
```

以下顯示輸出的範例：

```json
{
    "value": [
        {
            "properties": {
                "roleDefinitionId": "/subscriptions/{subscriptionId1}/providers/Microsoft.Authorization/roleDefinitions/2a2b9908-6ea1-4ae2-8e65-a410df84e7d1",
                "principalId": "{objectId1}",
                "scope": "/subscriptions/{subscriptionId1}",
                "createdOn": "2019-01-15T21:08:45.4904312Z",
                "updatedOn": "2019-01-15T21:08:45.4904312Z",
                "createdBy": "{createdByObjectId1}",
                "updatedBy": "{updatedByObjectId1}"
            },
            "id": "/subscriptions/{subscriptionId1}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentId1}",
            "type": "Microsoft.Authorization/roleAssignments",
            "name": "{roleAssignmentId1}"
        }
    ]
}
```

## <a name="next-steps"></a>後續步驟

- [使用 REST API 新增或移除 Azure 角色指派](role-assignments-rest.md)
- [Azure REST API 參考](/rest/api/azure/)
