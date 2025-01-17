---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 12/21/2020
ms.author: alkohli
ms.openlocfilehash: f2443765ecc9116193cefbc729ced25fa5657e59
ms.sourcegitcommit: 799f0f187f96b45ae561923d002abad40e1eebd6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/24/2020
ms.locfileid: "97763418"
---
在 Azure Stack Edge 裝置上部署 Vm 之前，您必須先將用戶端設定為透過 Azure Resource Manager 透過 Azure PowerShell 連接到裝置。 如需詳細步驟，請移至 [Azure Stack Edge 裝置上的 [連線到 Azure Resource Manager]](../articles/databox-online/azure-stack-edge-j-series-connect-resource-manager.md)。


請確定可以使用下列步驟，從您的用戶端存取裝置 (您在連線至 Azure Resource Manager 時完成這項設定，只是驗證設定是否成功。 ) ： 

1. 確認 Azure Resource Manager 通訊正在運作。 輸入：     

    ```powershell
    Add-AzureRmEnvironment -Name <Environment Name> -ARMEndpoint "https://management.<appliance name>.<DNSDomain>"
    ```

1. 呼叫本機裝置 Api 進行驗證。 輸入： 

    `login-AzureRMAccount -EnvironmentName <Environment Name>`

    提供使用者名稱 *EdgeARMuser* 和密碼，以透過 Azure Resource Manager 連接。

1. 如果您已設定 Kubernetes 的 **計算** ，則可以略過此步驟。 繼續進行，以確定您已啟用用於計算的網路介面。 在本機 UI 中，移至 [ **計算** 設定]。 選取您將用來建立虛擬交換器的網路介面。 您建立的 Vm 將會連接到連接到此埠的虛擬交換器以及相關聯的網路。 請務必選擇符合您將用於 VM 之 IP 位址的網路。  

    ![啟用計算設定1](../articles/databox-online/media/azure-stack-edge-gpu-deploy-virtual-machine-templates/enable-compute-setting.png)

    在網路介面上啟用計算。 Azure Stack Edge 將會建立和管理對應于該網路介面的虛擬交換器。 此時請勿輸入 Kubernetes 的特定 Ip。 啟用計算可能需要幾分鐘的時間。

    > [!NOTE]
    > 如果要建立 GPU Vm，請選取連線到網際網路的網路介面。 這可讓您在裝置上安裝 GPU 擴充功能。


1. 從 Azure 入口網站啟用 VM 角色。 此步驟會為您的裝置建立唯一的訂用帳戶，此訂用帳戶是用來透過裝置的本機 Api 建立 Vm。 

    1. 若要啟用 VM 角色，請在 Azure 入口網站中，移至 Azure Stack Edge 裝置的 Azure Stack Edge 資源。 前往 **Edge 計算 > 虛擬機器**。

        ![新增 VM 映射1](../articles/databox-online/media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-image-1.png)

    1. 選取 [ **虛擬機器** ] 以移至 [ **總覽** ] 頁面。 **啟用** 虛擬機器雲端管理。
        ![新增 VM 映射2](../articles/databox-online/media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-image-2.png)