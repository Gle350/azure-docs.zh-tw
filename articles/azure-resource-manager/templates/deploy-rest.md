---
title: 使用 REST API 和範本部署資源
description: 使用 Azure Resource Manager 和 Resource Manager REST API 將資源部署到 Azure。 資源會定義在 Resource Manager 範本中。
ms.topic: conceptual
ms.date: 10/22/2020
ms.openlocfilehash: 77192aff9ed4fe33269b5e11891c30e15bc312dd
ms.sourcegitcommit: e46f9981626751f129926a2dae327a729228216e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98028959"
---
# <a name="deploy-resources-with-arm-templates-and-azure-resource-manager-rest-api"></a>使用 ARM 範本部署資源，並 Azure Resource Manager REST API

本文說明如何使用 Azure Resource Manager REST API 搭配 Azure Resource Manager 範本 (ARM 範本) 將您的資源部署到 Azure。

您可以在要求本文中納入範本，也可以連結至檔案。 使用檔案時，該檔案可以是本機檔案，也可以是透過 URI 所取得的外部檔案。 當範本位於儲存體帳戶中時，您可以限制範本的存取權，並在部署期間提供共用存取簽章 (SAS) 權杖。

## <a name="deployment-scope"></a>部署範圍

您可以將部署的目標設為資源群組、Azure 訂用帳戶、管理群組或租使用者。 視部署的範圍而定，您可以使用不同的命令。

- 若要部署至 **資源群組**，請使用 [[部署 - 建立]](/rest/api/resources/deployments/createorupdate)。 要求會傳送至：

  ```HTTP
  PUT https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2020-06-01
  ```

- 若要部署至 **訂用帳戶**，請使用 [[部署 - 在訂用帳戶範圍建立]](/rest/api/resources/deployments/createorupdateatsubscriptionscope)。 要求會傳送至：

  ```HTTP
  PUT https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2020-06-01
  ```

  如需訂用帳戶層級部署的詳細資訊，請參閱[在訂用帳戶層級建立資源群組和資源](deploy-to-subscription.md)。

- 若要部署到 **管理群組**，請使用 [[部署 - 在管理群組範圍建立]](/rest/api/resources/deployments/createorupdateatmanagementgroupscope)。 要求會傳送至：

  ```HTTP
  PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{groupId}/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2020-06-01
  ```

  如需管理群組層級部署的詳細資訊，請參閱[在管理群組層級建立資源](deploy-to-management-group.md)。

- 若要部署至 **租用戶**，請使用 [[部署 - 在租用戶範圍建立或更新]](/rest/api/resources/deployments/createorupdateattenantscope)。 要求會傳送至：

  ```HTTP
  PUT https://management.azure.com/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2020-06-01
  ```

  如需租用戶層級部署的詳細資訊，請參閱[在租用戶層級建立資源](deploy-to-tenant.md)。

本文中的範例會使用資源群組部署。

## <a name="deploy-with-the-rest-api"></a>使用 REST API 部署

1. 設定[一般參數和標頭](/rest/api/azure/) (包括驗證權杖)。

1. 如果您要部署到不存在的資源群組，請建立資源群組。 提供您的訂用帳戶識別碼、新資源群組的名稱，以及需要解決方案的位置。 如需詳細資訊，請參閱[建立資源群組](/rest/api/resources/resourcegroups/createorupdate)。

   ```HTTP
   PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2020-06-01
   ```

   使用如下的要求本文：

   ```json
   {
    "location": "West US",
    "tags": {
      "tagname1": "tagvalue1"
    }
   }
   ```

1. 在部署範本之前，您可以預覽範本將對您的環境進行的變更。 使用「 [假設](template-deploy-what-if.md) 」作業來確認範本會進行您預期的變更。 假設也會驗證範本是否有錯誤。

1. 若要部署範本，請在要求 URI 中提供您的訂用帳戶識別碼、資源群組的名稱、部署的名稱。

   ```HTTP
   PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2019-10-01
   ```

   在要求本文中，提供您的範本和參數檔案的連結。 如需參數檔案的詳細資訊，請參閱[建立 Resource Manager 參數檔案](parameter-files.md)。

   請注意， `mode` 已設定為 **增量**。 若要執行完整部署，請將設定 `mode` 為 [ **完成**]。 使用完整模式時請務必謹慎，因為您可能會不小心刪除不在範本中的資源。

   ```json
   {
    "properties": {
      "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0"
      },
      "parametersLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
        "contentVersion": "1.0.0.0"
      },
      "mode": "Incremental"
    }
   }
   ```

    如果您想要記錄回應內容、要求內容（或兩者），請 `debugSetting` 在要求中包含。

   ```json
   {
    "properties": {
      "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0"
      },
      "parametersLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
        "contentVersion": "1.0.0.0"
      },
      "mode": "Incremental",
      "debugSetting": {
        "detailLevel": "requestContent, responseContent"
      }
    }
   }
   ```

    您可以設定儲存體帳戶以使用共用存取簽章 (SAS) Token。 如需詳細資訊，請參閱 [使用共用存取簽章來委派存取權](/rest/api/storageservices/delegate-access-with-shared-access-signature)。

    如果您需要提供參數機密的值 (例如密碼)，請將該值加入金鑰保存庫。 在部署期間擷取金鑰保存庫，如先前範例所示。 如需詳細資訊，請參閱[在部署期間使用 Azure Key Vault 以傳遞安全的參數值](key-vault-parameter.md)。

1. 不要連結至含有範本和參數的檔案，而是將它們納入要求本文中。 下列範例顯示內嵌範本和參數的要求本文：

   ```json
   {
      "properties": {
      "mode": "Incremental",
      "template": {
        "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
          "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_LRS",
            "allowedValues": [
              "Standard_LRS",
              "Standard_GRS",
              "Standard_ZRS",
              "Premium_LRS"
            ],
            "metadata": {
              "description": "Storage Account type"
            }
          },
          "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
              "description": "Location for all resources."
            }
          }
        },
        "variables": {
          "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
        },
        "resources": [
          {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2018-02-01",
            "name": "[variables('storageAccountName')]",
            "location": "[parameters('location')]",
            "sku": {
              "name": "[parameters('storageAccountType')]"
            },
            "kind": "StorageV2",
            "properties": {}
          }
        ],
        "outputs": {
          "storageAccountName": {
            "type": "string",
            "value": "[variables('storageAccountName')]"
          }
        }
      },
      "parameters": {
        "location": {
          "value": "eastus2"
        }
      }
    }
   }
   ```

1. 若要取得範本部署的狀態，請使用 [Deployments - Get](/rest/api/resources/deployments/get)。

   ```HTTP
   GET https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2019-10-01
   ```

## <a name="deployment-name"></a>部署名稱

您可以為您的部署命名，例如 `ExampleDeployment` 。

每次執行部署時，會將專案新增至資源群組的部署歷程記錄，並提供部署名稱。 如果您執行另一個部署並指定相同名稱，則會將先前的專案取代為目前的部署。 如果您想要在部署歷程記錄中維護唯一的專案，請為每個部署提供一個唯一的名稱。

若要建立唯一的名稱，您可以指派一個亂數字。 或者，加入日期值。

如果您對相同的部署名稱執行相同資源群組的並行部署，則只會完成最後一個部署。 具有相同名稱但未完成的任何部署都會由最後一個部署取代。 例如，如果您執行名為的部署， `newStorage` 並部署名為的儲存體帳戶 `storage1` ，同時執行另一個名為的部署，並部署 `newStorage` 名為的儲存體帳戶 `storage2` ，您就只會部署一個儲存體帳戶。 產生的儲存體帳戶名為 `storage2` 。

但是，如果您執行名為 `newStorage` 的部署，並部署名為的儲存體帳戶 `storage1` ，並在完成後立即執行另一個名為的部署，並部署 `newStorage` 名為的儲存體帳戶 `storage2` ，則您會有兩個儲存體帳戶。 其中一個名為 `storage1` ，另一個名為 `storage2` 。 但是，您在部署歷程記錄中只會有一個專案。

當您為每個部署指定唯一的名稱時，您可以同時執行它們，而不會發生衝突。 如果您執行名為的部署， `newStorage1` 並部署名為的儲存體帳戶 `storage1` ，同時執行另一個名為 `newStorage2` 的部署，並部署名為的儲存體帳戶 `storage2` ，則您在部署歷程記錄中會有兩個儲存體帳戶和兩個專案。

為了避免與並行部署發生衝突，並確保部署歷程記錄中的唯一專案，請為每個部署提供一個唯一的名稱。

## <a name="next-steps"></a>後續步驟

- 在您收到錯誤時，若要回復為成功的部署，請參閱[錯誤回復至成功部署](rollback-on-error.md)。
- 若要指定如何處理存在於資源群組中、但尚未定義於範本中的資源，請參閱 [Azure Resource Manager 部署模式](deployment-modes.md)。
- 若要了解如何處理非同步 REST 作業，請參閱[追蹤非同步 Azure 作業 (英文)](../management/async-operations.md)。
- 若要深入了解範本，請參閱[瞭解 ARM 範本的結構和語法](template-syntax.md)。
