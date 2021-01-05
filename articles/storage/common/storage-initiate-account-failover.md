---
title: 起始儲存體帳戶容錯移轉
titleSuffix: Azure Storage
description: 了解如何在儲存體帳戶的主要端點無法使用時起始帳戶容錯移轉。 容錯移轉會更新次要區域，使其成為您儲存體帳戶的主要區域。
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 12/29/2020
ms.author: tamram
ms.reviewer: artek
ms.subservice: common
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 93bcbab9445d83bf17b37b6affc1d2bc70703bbf
ms.sourcegitcommit: 1140ff2b0424633e6e10797f6654359947038b8d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/30/2020
ms.locfileid: "97814324"
---
# <a name="initiate-a-storage-account-failover"></a>起始儲存體帳戶容錯移轉

如果您的異地複寫儲存體帳戶的主要端點因為任何原因而變成無法使用，您可以起始帳戶容錯移轉。 帳戶容錯移轉會更新次要端點，使其成為您儲存體帳戶的主要端點。 容錯移轉完成後，用戶端便可以開始寫入到新的主要區域。 強制容錯移轉可讓您維護應用程式的高可用性。

本文說明如何使用 Azure 入口網站、PowerShell 或 Azure CLI 為您的儲存體帳戶起始帳戶容錯移轉。 若要深入瞭解帳戶容錯移轉，請參閱嚴重損壞 [修復和儲存體帳戶容錯移轉](storage-disaster-recovery-guidance.md)。

> [!WARNING]
> 帳戶容錯移轉通常會導致一些資料遺失。 若要了解帳戶容錯移轉的影響並預先因應資料遺失，請檢閱[了解帳戶容錯移轉程序](storage-disaster-recovery-guidance.md#understand-the-account-failover-process)。

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>必要條件

在您的儲存體帳戶上執行帳戶容錯移轉之前，請確定您的儲存體帳戶已設定為進行異地複寫。 您的儲存體帳戶可以使用下列任何一種冗余選項：

- 異地冗余儲存體 (GRS) 或讀取權限異地冗余儲存體 (RA-GRS) 
- 地理區域-冗余儲存體 (GZRS) 或讀取權限地理區域-冗余儲存體 (RA-GZRS) 

如需 Azure 儲存體冗余的詳細資訊，請參閱 [Azure 儲存體冗余](storage-redundancy.md)。

請記住，帳戶容錯移轉不支援下列功能和服務：

- Azure 檔案同步不支援儲存體帳戶容錯移轉。 不應該容錯移轉包含在 Azure 檔案同步中作為雲端端點使用之 Azure 檔案共用的儲存體帳戶。 這麼做將導致同步停止運作，且可能會在新分層的檔案中產生未預期的資料遺失。
- 目前不支援 ADLS Gen2 儲存體帳戶 (已啟用階層命名空間) 的帳戶。
- 無法容錯移轉包含進階區塊 Blob 的儲存體帳戶。 支援進階區塊 Blob 的儲存體帳戶目前不支援異地備援。
- 無法容錯移轉包含任何 [WORM 永久性原則](../blobs/storage-blob-immutable-storage.md) 啟用容器的儲存體帳戶。 解除鎖定/鎖定以時間為基礎的保留或合法保存原則會防止容錯移轉，以維持合規性。

## <a name="initiate-the-failover"></a>起始容錯移轉

## <a name="portal"></a>[入口網站](#tab/azure-portal)

若要從 Azure 入口網站起始帳戶容錯移轉，請遵循下列步驟：

1. 瀏覽至儲存體帳戶。
1. 在 [設定] 下方，選取 [異地複寫]。 下圖顯示儲存體帳戶的異地複寫和容錯移轉狀態。

    :::image type="content" source="media/storage-initiate-account-failover/portal-failover-prepare.png" alt-text="顯示異地複寫和容錯移轉狀態的螢幕擷取畫面":::

1. 確認您的儲存體帳戶已進行異地備援儲存體 (GRS) 或讀取權限異地備援儲存體 (RA-GRS) 的設定。 若未設定，請選取 [設定] 下方的 [組態]，將您的帳戶更新為異地備援。
1. [上次同步時間] 屬性會指出次要複本與主要複本相差了多久的時間。 [上次同步時間] 可讓您預估在容錯移轉完成後發生資料遺失的程度。 如需有關檢查 [ **上次同步處理時間** ] 屬性的詳細資訊，請參閱 [檢查儲存體帳戶的上次同步時間屬性](last-sync-time-get.md)。
1. 選取 [準備容錯移轉]。
1. 檢閱確認對話方塊。 在您準備就緒後，輸入 **是** 加以確認，並起始容錯移轉。

    :::image type="content" source="media/storage-initiate-account-failover/portal-failover-confirm.png" alt-text="顯示帳戶容錯移轉的確認對話方塊的螢幕擷取畫面":::

## <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

帳戶容錯移轉功能已正式運作，但仍依賴適用于 PowerShell 的預覽模組。 若要使用 PowerShell 起始帳戶容錯移轉，您必須先安裝 Az. Storage [1.1.1-preview](https://www.powershellgallery.com/packages/Az.Storage/1.1.1-preview) 模組。 請依照下列步驟安裝此模組：

1. 解除安裝任何先前安裝的 Azure PowerShell：

    - 使用 [設定] 底下的 [應用程式與功能]，從 Windows 移除任何先前安裝的 Azure PowerShell。
    - 從移除所有的 **Azure** 模組 `%Program Files%\WindowsPowerShell\Modules` 。

1. 確定您已安裝最新版的 PowerShellGet。 開啟 Windows PowerShell 視窗，然後執行下列命令來安裝最新版本：

    ```powershell
    Install-Module PowerShellGet –Repository PSGallery –Force
    ```

1. 在安裝完 PowerShellGet 之後，關閉並重新開啟 PowerShell 視窗。

1. 安裝最新版的 Azure PowerShell：

    ```powershell
    Install-Module Az –Repository PSGallery –AllowClobber
    ```

1. 安裝支援帳戶容錯移轉的 Azure 儲存體預覽版模組：

    ```powershell
    Install-Module Az.Storage –Repository PSGallery -RequiredVersion 1.1.1-preview –AllowPrerelease –AllowClobber –Force
    ```

若要從 PowerShell 起始帳戶容錯移轉，請執行下列命令：

```powershell
Invoke-AzStorageAccountFailover -ResourceGroupName <resource-group-name> -Name <account-name>
```

## <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

若要使用 Azure CLI 來起始帳戶容錯移轉，請執行下列命令：

```azurecli-interactive
az storage account show \ --name accountName \ --expand geoReplicationStats
az storage account failover \ --name accountName
```

---

## <a name="important-implications-of-account-failover"></a>帳戶容錯移轉的重要影響

當您為儲存體帳戶起始帳戶容錯移轉時，次要端點的 DNS 記錄將會更新，使次要端點成為主要端點。 在起始容錯移轉之前，請確實了解儲存體帳戶可能會受到的影響。

若要在您起始容錯移轉之前，估計可能遺失資料的程度，請檢查 [ **上次同步處理時間** ] 屬性。 如需有關檢查 [ **上次同步處理時間** ] 屬性的詳細資訊，請參閱 [檢查儲存體帳戶的上次同步時間屬性](last-sync-time-get.md)。

在起始之後，容錯移轉所花費的時間可能會因為通常不到一小時而有所差異。

容錯移轉之後，您的儲存體帳戶類型在新的主要區域中會自動轉換為本地備援儲存體 (LRS)。 您可以為帳戶重新啟用異地備援儲存體 (GRS) 或讀取權限異地備援儲存體 (RA-GRS)。 請注意，從 LRS 轉換為 GRS 或 RA-GRS 時，會產生額外的費用。 如需詳細資訊，請參閱[頻寬定價詳細資料](https://azure.microsoft.com/pricing/details/bandwidth/)。

為您的儲存體帳戶重新啟用 GRS 之後，Microsoft 會開始將您帳戶中的資料複寫到新的次要區域。 複寫時間取決於複寫的資料量。  

## <a name="next-steps"></a>後續步驟

- [災害復原和儲存體帳戶容錯移轉](storage-disaster-recovery-guidance.md)
- [檢查儲存體帳戶的上次同步時間屬性](last-sync-time-get.md)
- [使用異地備援來設計高可用性的應用程式](geo-redundant-design.md)
- [教學課程：建置採用 Blob 儲存體的高可用性應用程式](../blobs/storage-create-geo-redundant-storage.md)
