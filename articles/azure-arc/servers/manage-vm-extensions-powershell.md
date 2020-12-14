---
title: 使用 Azure PowerShell 啟用 VM 擴充功能
description: 本文說明如何使用 Azure PowerShell 將虛擬機器擴充功能部署到在混合式雲端環境中執行的 Azure Arc 啟用的伺服器。
ms.date: 11/24/2020
ms.topic: conceptual
ms.openlocfilehash: 6b38c425042c260a29682db11212a1f6324abd38
ms.sourcegitcommit: ea17e3a6219f0f01330cf7610e54f033a394b459
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97387405"
---
# <a name="enable-azure-vm-extensions-using-azure-powershell"></a>使用 Azure PowerShell 啟用 Azure VM 擴充功能

本文說明如何使用 Azure PowerShell 將 Azure Arc 啟用的伺服器所支援的 Azure VM 擴充功能部署和卸載至 Linux 或 Windows 混合式電腦。

## <a name="prerequisites"></a>必要條件

- 具備 Azure PowerShell 的電腦。 如需指示，請參閱 [安裝並設定 Azure PowerShell](/powershell/azure/)。

在使用 Azure PowerShell 管理已啟用 Arc 之伺服器所管理的混合式伺服器上的 VM 擴充功能之前，您必須安裝該 `Az.ConnectedMachine` 模組。 在啟用 Arc 的伺服器上執行下列命令：

`Install-Module -Name Az.ConnectedMachine`.

當安裝完成時，會傳回下列訊息：

`The installed extension `Az. ConnectedMachine` is experimental and not covered by customer support. Please use with discretion.`

## <a name="enable-extension"></a>啟用延伸模組

若要在啟用 Arc 的伺服器上啟用 VM 擴充功能，請使用 [新的-AzConnectedMachineExtension](/powershell/module/az.connectedmachine/new-azconnectedmachineextension) 搭配 `-Name` 、 `-ResourceGroupName` 、 `-MachineName` 、 `-Location` 、 `-Publisher` 、- `ExtensionType` 和 `-Settings` 參數。

下列範例會在啟用 Arc 的 Linux 伺服器上啟用 Log Analytics VM 延伸模組：

```powershell
PS C:\> $Setting = @{ "workspaceId" = "workspaceId" }
PS C:\> $protectedSetting = @{ "workspaceKey" = "workspaceKey" }
PS C:\> New-AzConnectedMachineExtension -Name OMSLinuxAgent -ResourceGroupName "myResourceGroup" -MachineName "myMachine" -Location "eastus" -Publisher "Microsoft.EnterpriseCloud.Monitoring" -TypeHandlerVersion "1.10" -Settings $Setting -ProtectedSetting $protectedSetting -ExtensionType "OmsAgentforLinux"
```

下列範例會在啟用 Arc 的伺服器上啟用自訂腳本延伸模組：

```powershell
PS C:\> $Setting = @{ "commandToExecute" = "powershell.exe -c Get-Process" }
PS C:\> New-AzConnectedMachineExtension -Name custom -ResourceGroupName myResourceGroup -MachineName myMachineName -Location eastus -Publisher "Microsoft.Compute" -TypeHandlerVersion 1.10 -Settings $Setting -ExtensionType CustomScriptExtension
```

### <a name="key-vault-vm-extension-preview"></a>Key Vault VM 延伸模組 (預覽) 

> [!WARNING]
> PowerShell 用戶端通常 `\` `"` 會在 settings.js中新增，這會導致 akvvm_service 失敗，並出現錯誤： `[CertificateManagementConfiguration] Failed to parse the configuration settings with:not an object.`

下列範例會在啟用 Arc 的伺服器上啟用 Key Vault VM 延伸模組 (preview) ：

```powershell
# Build settings
    $settings = @{
      secretsManagementSettings = @{
       observedCertificates = @(
        "observedCert1"
       )
      certificateStoreLocation = "myMachineName" # For Linux use "/var/lib/waagent/Microsoft.Azure.KeyVault.Store/"
      certificateStore = "myCertificateStoreName"
      pollingIntervalInS = "pollingInterval"
      }
    authenticationSettings = @{
     msiEndpoint = "http://localhost:40342/metadata/identity"
     }
    }

    $resourceGroup = "resourceGroupName"
    $machineName = "myMachineName"
    $location = "regionName"

    # Start the deployment
    New-AzConnectedMachineExtension -ResourceGroupName $resourceGRoup -Location $location -MachineName $machineName -Name "KeyVaultForWindows or KeyVaultforLinux" -Publisher "Microsoft.Azure.KeyVault" -ExtensionType "KeyVaultforWindows or KeyVaultforLinux" -Setting (ConvertTo-Json $settings)
```

## <a name="list-extensions-installed"></a>已安裝清單延伸模組

若要取得已啟用 Arc 之伺服器上的 VM 延伸模組清單，請使用 [AzConnectedMachineExtension](/powershell/module/az.connectedmachine/get-azconnectedmachineextension) 搭配 `-MachineName` 和 `-ResourceGroupName` 參數。

範例：

```powershell
Get-AzConnectedMachineExtension -ResourceGroupName myResourceGroup -MachineName myMachineName

Name    Location  PropertiesType        ProvisioningState
----    --------  --------------        -----------------
custom  westus2   CustomScriptExtension Succeeded
```

## <a name="remove-an-installed-extension"></a>移除已安裝的擴充功能

若要移除已安裝 Arc 的伺服器上已安裝的 VM 擴充功能，請使用 [AzConnectedMachineExtension](/powershell/module/az.connectedmachine/remove-azconnectedmachineextension) 搭配 `-Name` 、 `-MachineName` 和 `-ResourceGroupName` 參數。

例如，若要移除適用于 Linux 的 Log Analytics VM 擴充功能，請執行下列命令：

```powershell
Remove-AzConnectedMachineExtension -MachineName myMachineName -ResourceGroupName myResourceGroup -Name OmsAgentforLinux
```

## <a name="next-steps"></a>後續步驟

- 您可以使用 [Azure CLI](manage-vm-extensions-cli.md)、 [Azure 入口網站](manage-vm-extensions-portal.md)或 [Azure Resource Manager 範本](manage-vm-extensions-template.md)，來部署、管理和移除 VM 擴充功能。

- 疑難排解資訊可在 [VM 延伸模組指南](troubleshoot-vm-extensions.md)中找到。
