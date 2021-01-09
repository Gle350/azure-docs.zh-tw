---
title: 快速入門：使用 PowerShell 從 Key Vault 設定及擷取祕密
description: 在本快速入門中，您將了解如何使用 Azure PowerShell 從 Azure Key Vault 建立、擷取和刪除祕密。
services: key-vault
author: msmbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: secrets
ms.topic: quickstart
ms.custom: mvc, devx-track-azurepowershell
ms.date: 09/30/2020
ms.author: mbaldwin
ms.openlocfilehash: d1fa63da035cba35538d13ffe4c3897458364a65
ms.sourcegitcommit: 2aa52d30e7b733616d6d92633436e499fbe8b069
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97936646"
---
# <a name="quickstart-set-and-retrieve-a-secret-from-azure-key-vault-using-powershell"></a>快速入門：使用 PowerShell 從 Azure Key Vault 設定及擷取祕密

Azure Key Vault 是一項雲端服務，可作為安全的祕密存放區。 您也可以安全地儲存金鑰、密碼、憑證和其他祕密。 如需 Key Vault 的詳細資訊，您可以檢閱[概觀](../general/overview.md)。 在本快速入門中，您會使用 PowerShell 來建立金鑰保存庫。 然後，將祕密存放在新建立的保存庫中。

如果您沒有 Azure 訂用帳戶，請在開始前建立[免費帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

如果您選擇在本機安裝和使用 PowerShell，則在執行本教學課程時，您必須使用 Azure PowerShell 模組 5.0.0 版或更新版本。 執行 `$PSVersionTable.PSVersion` 以尋找版本。 如果您需要升級，請參閱[安裝 Azure PowerShell 模組](/powershell/azure/install-az-ps)。 如果您在本機執行 PowerShell，則也需要執行 `Connect-AzAccount` 以建立與 Azure 的連線。

```azurepowershell-interactive
Connect-AzAccount
```

## <a name="create-a-resource-group"></a>建立資源群組

使用 [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) 來建立 Azure 資源群組。 資源群組是在其中部署與管理 Azure 資源的邏輯容器。

```azurepowershell-interactive
New-AzResourceGroup -Name ContosoResourceGroup -Location EastUS
```

## <a name="create-a-key-vault"></a>建立金鑰保存庫

接下來，您會建立 Key Vault。 執行此步驟時，您需要一些資訊：

雖然我們在此快速入門中使用 "Contoso KeyVault2" 作為我們的 Key Vault 名稱，但您必須使用唯一的名稱。

- **保存庫名稱**：Contoso-Vault2。
- **資源群組名稱**：ContosoResourceGroup。
- **位置**：美國東部。

```azurepowershell-interactive
New-AzKeyVault -Name 'Contoso-Vault2' -ResourceGroupName 'ContosoResourceGroup' -Location 'East US'
```

此 Cmdlet 的輸出會顯示新建立金鑰保存庫的屬性。 請記下下列兩個屬性：

* **保存庫名稱**：在此範例中是 **Contoso-Vault2**。 您將在其他金鑰保存庫 Cmdlet 中使用此名稱。
* **保存庫 URI**：在此範例中是 https://Contoso-Vault2.vault.azure.net/ 。 透過其 REST API 使用保存庫的應用程式必須使用此 URI。

在保存庫建立後，您的 Azure 帳戶是唯一能夠在這個新保存庫上執行任何作業的帳戶。

## <a name="give-your-user-account-permissions-to-manage-secrets-in-key-vault"></a>授與使用者帳戶在 Key Vault 中管理密碼的權限

使用Azure PowerShell Set-AzKeyVaultAccessPolicy Cmdlet 來更新 Key Vault 存取原則，並將祕密權限授與您的使用者帳戶。
```azurepowershell-interactive
Set-AzKeyVaultAccessPolicy -VaultName 'Contoso-Vault2' -UserPrincipalName 'user@domain.com' -PermissionsToSecrets get,set,delete
```

## <a name="adding-a-secret-to-key-vault"></a>將祕密新增至 Key Vault

若要將祕密新增至保存庫，您只需要採取一些步驟。 在此情況下，您會新增應用程式可以使用的密碼。 此密碼稱為 **ExamplePassword**，且其中會儲存 **hVFkk965BuUv** 值。

第一次將 **hVFkk965BuUv** 值轉換為安全的字串時，請輸入：

```azurepowershell-interactive
$secretvalue = ConvertTo-SecureString 'hVFkk965BuUv' -AsPlainText -Force
```

然後，輸入下列 PowerShell 命令，以在 Key Vault 中建立稱為 **ExamplePassword** 並具有 **hVFkk965BuUv** 值的祕密：


```azurepowershell-interactive
$secret = Set-AzKeyVaultSecret -VaultName 'Contoso-Vault2' -Name 'ExamplePassword' -SecretValue $secretvalue
```

## <a name="retrieve-a-secret-from-key-vault"></a>從 Key Vault 擷取祕密

若要以純文字檢視包含在祕密中的值：

```azurepowershell-interactive
$secret = Get-AzKeyVaultSecret -VaultName 'Contoso-Vault2' -Name 'ExamplePassword'
$ssPtr = [System.Runtime.InteropServices.Marshal]::SecureStringToBSTR($secret.SecretValue)
try {
   $secretValueText = [System.Runtime.InteropServices.Marshal]::PtrToStringBSTR($ssPtr)
} finally {
   [System.Runtime.InteropServices.Marshal]::ZeroFreeBSTR($ssPtr)
}
Write-Output $secretValueText
```

現在，您已建立 Key Vault，儲存祕密，並擷取它。

## <a name="clean-up-resources"></a>清除資源

 此集合中的其他快速入門和教學課程會以本快速入門為基礎。 如果您打算繼續進行其他快速入門和教學課程，您可以讓這些資源留在原處。

若不再需要，您可以使用 [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) 命令來移除資源群組、Key Vault 和所有相關資源。

```azurepowershell-interactive
Remove-AzResourceGroup -Name ContosoResourceGroup
```

## <a name="next-steps"></a>後續步驟

在本快速入門中，您已建立 Key Vault 並在其中儲存祕密。 若要深入了解 Key Vault 以及要如何將其與應用程式整合，請繼續閱讀下列文章。

- 閱讀 [Azure Key Vault 概觀](../general/overview.md)
- 請參閱 [Azure PowerShell Key Vault Cmdlet](/powershell/module/az.keyvault/#key_vault) 的參考
- 請參閱 [Key Vault 安全性概觀](../general/security-overview.md)
