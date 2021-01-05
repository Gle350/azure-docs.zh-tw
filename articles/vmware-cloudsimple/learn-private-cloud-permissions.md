---
title: Azure VMware Solution by CloudSimple-私用雲端許可權模型
description: 描述 CloudSimple 私用雲端許可權模型、群組及類別
author: Ajayan1008
ms.author: v-hborys
ms.date: 08/16/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 1c8cfeda008955006f2fbad1df58c8047bd36541
ms.sourcegitcommit: d7d5f0da1dda786bda0260cf43bd4716e5bda08b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/05/2021
ms.locfileid: "97898040"
---
# <a name="cloudsimple-private-cloud-permission-model-of-vmware-vcenter"></a>VMware vCenter 的 CloudSimple 私用雲端許可權模型

CloudSimple 會保留私用雲端環境的完整系統管理存取權。 每個 CloudSimple 客戶都會獲得足夠的系統管理許可權，以便在其環境中部署及管理虛擬機器。  如有需要，您可以暫時提升您的許可權，以執行系統管理功能。

## <a name="cloud-owner"></a>雲端擁有者

當您建立私人雲端時， **CloudOwner** 使用者會在 vCenter 單一 Sign-On 網域中建立，並具有 **雲端擁有者角色** 存取權，以管理私人雲端中的物件。 此使用者也可以設定其他 [vCenter 身分識別來源](set-vcenter-identity.md)，以及其他使用者至私用雲端 vCenter。

> [!NOTE]
> CloudSimple 私人雲端 vCenter 的預設使用者是 cloudowner@cloudsimple.local 在建立私人雲端時。

## <a name="user-groups"></a>使用者群組

部署私用雲端時，會建立名為 **雲端擁有者群組** 的群組。 此群組中的使用者可以管理私用雲端上 vSphere 環境的各個部分。 此群組會自動獲得  **雲端擁有者角色** 許可權，並將  **CloudOwner** 使用者新增為此群組的成員。  CloudSimple 會建立具有有限許可權的額外群組以方便管理。  您可以將任何使用者新增至這些預先建立的群組，並將下列定義的許可權自動指派給群組中的使用者。

### <a name="pre-created-groups"></a>預先建立的群組

| 群組名稱 | 目的 | 角色 |
| -------- | ------- | ------ |
| 雲端擁有者-群組 | 此群組的成員具有私人雲端 vCenter 的系統管理許可權 | [雲端擁有者-角色](#cloud-owner-role) |
| 雲端-全域叢集-管理群組 | 此群組的成員具有私人雲端 vCenter 叢集上的系統管理許可權 | [雲端叢集-管理員-角色](#cloud-cluster-admin-role) |
| 雲端-全域-儲存體-管理-群組 | 此群組的成員可以管理私人雲端 vCenter 上的儲存體 | [雲端-儲存體-管理員-角色](#cloud-storage-admin-role) |
| 雲端-全域-網路-系統管理-群組 | 此群組的成員可以管理私人雲端 vCenter 上的網路和分散式通訊埠群組 | [雲端-網路-管理員-角色](#cloud-network-admin-role) |
| 雲端-全域-VM-管理群組 | 此群組的成員可以管理私人雲端 vCenter 上的虛擬機器 | [雲端-VM-管理員-角色](#cloud-vm-admin-role) |

若要授與個別使用者管理私用雲端的許可權，請建立使用者帳戶，並新增至適當的群組。

> [!CAUTION]
> 新使用者只能新增至 *雲端擁有者群組*、 *雲端全域* 叢集-系統管理群組、雲端-全域 *存放裝置-* 管理群組、 *雲端全域-網路-系統管理* 群組或 *雲端全域 VM-管理群組*。  新增至系統 *管理員* 群組的使用者將會自動移除。  只有服務帳戶必須加入至系統 *管理員* 群組，而且服務帳戶不能用來登入 VSPHERE web UI。

## <a name="list-of-vcenter-privileges-for-default-roles"></a>預設角色的 vCenter 許可權清單

### <a name="cloud-owner-role"></a>雲端擁有者-角色

| **類別** | **特權** |
|----------|-----------|
| **警示** | 確認警示 <br> 建立鬧鐘 <br> 停用警示動作 <br> 修改警示 <br> 移除警示 <br> 設定警示狀態 |
| **權限** | Modify 許可權 |
| **內容庫** | 新增程式庫專案 <br> 建立本機程式庫 <br> 建立訂閱的程式庫 <br> 刪除程式庫專案 <br> 刪除本機程式庫 <br> 刪除訂閱的媒體櫃 <br> 下載檔案 <br> 收回程式庫專案 <br> 收回訂閱的程式庫 <br> 匯入儲存體 <br> 探查訂閱資訊 <br> 讀取儲存體 <br> 同步程式庫專案 <br> 同步訂閱的程式庫 <br> 輸入自我檢查 <br> 更新設定 <br> 更新檔案 <br> 更新程式庫 <br> 更新程式庫專案 <br> 更新本機程式庫 <br> 更新訂閱的程式庫 <br> View configuration settings |
| **密碼編譯作業** | 新增磁片 <br> 複製 <br> 解密 <br> 直接存取 <br> Encrypt <br> 加密新的 <br> 管理 KMS <br> 管理加密原則 <br> 管理金鑰 <br> 遷移 <br> Recrypt <br> 註冊 VM <br> 註冊主機 |
| **dvPort 群組** | 建立 <br> 刪除 <br> 修改 <br> 原則操作 <br> 範圍作業 |
| **Datastore** | 配置空間 <br> 瀏覽資料存放區 <br> 設定資料存放區 <br> 低層級檔案作業 <br> 移動資料存放區 <br> 移除資料存放區 <br> 移除檔案 <br> 重新命名資料存放區 <br> 更新虛擬機器檔案 <br> 更新虛擬機器中繼資料 |
| **ESX 代理程式管理員** | Config <br> 修改 <br> 檢視 |
| **分機** | 註冊擴充功能 <br> 取消註冊擴充功能 <br> 更新擴充功能 |
| **外部統計資料提供者**| 註冊 <br> Unregister  <br> 更新 |
| **資料夾** | 建立資料夾 <br> 刪除資料夾 <br> 移動資料夾 <br> 重新命名資料夾 |
| **全球** | 取消工作 <br> 容量規劃 <br> 診斷 <br> 停用方法 <br> Enable 方法 <br> 全域標記 <br> 醫療 <br> 授權 <br> 記錄事件 <br> 管理自訂屬性 <br> Proxy <br> 腳本動作 <br> 服務管理員 <br> 設定自訂屬性 <br> 系統磁碟區標 |
| **健全狀況更新提供者** | 註冊 <br> Unregister  <br> 更新 |
| **主機 > 設定** | 儲存體分割區設定 |
| **主機 > 清查** | 修改叢集 |
| **vSphere 標記** | 指派或取消指派 vSphere 標記 <br> 建立 vSphere 標記 <br> 建立 vSphere 標記類別 <br> 刪除 vSphere 標記 <br> 刪除 vSphere 標記類別 <br> 編輯 vSphere 標記 <br> 編輯 vSphere 標記類別目錄 <br> 修改類別的 UsedBy 欄位 <br> 修改標記的 UsedBy 欄位 |
| **Network** | 指派網路 <br> 設定 <br> 移動網路 <br> 移除 |
| **效能** | 修改間隔 |
| **主機設定檔** | 檢視 |
| **Resource** | 套用建議 <br> 將 vApp 指派給資源集區 <br> 將虛擬機器指派給資源集區 <br> 建立資源集區 <br> 遷移電源關閉的虛擬機器 <br> 在虛擬機器上遷移電源 <br> 修改資源集區 <br> 移動資源集區 <br> 查詢 vMotion <br> 移除資源集區 <br> 重新命名資源集區 |
| **排定的工作** | 建立工作 <br> 修改工作 <br> 移除工作 <br> 執行工作 |
| **工作階段** | 模擬使用者 <br> 訊息 <br> 驗證會話 <br> 查看和停止會話 |
| **資料存放區叢集** | 設定資料存放區叢集 |
| **設定檔驅動的儲存體** | 設定檔驅動的儲存體更新 <br> 設定檔驅動的儲存體視圖 |
| **儲存體視圖** | 設定服務 <br> 檢視 |
| **工作** | 建立工作 <br> 更新工作 |
| **傳送服務**| 管理 <br> 監視 |
| **vApp** | 新增虛擬機器 <br> 指派資源集區 <br> 指派 vApp <br> 複製 <br> 建立 <br> 刪除 <br> 匯出 <br> 匯入 <br> 移動 <br> 關閉電源 <br> 開啟電源 <br> 重新命名 <br> 暫止 <br> Unregister  <br> View OVF 環境 <br> vApp 應用程式設定 <br> vApp 實例設定 <br> vApp managedBy 設定 <br> vApp 資源設定 |
| **VRMPolicy** | 查詢 VRMPolicy <br> 更新 VRMPolicy |
| **虛擬機器 > 設定** | 新增現有的磁片 <br> 新增磁片 <br> 新增或移除裝置 <br> 進階 <br> 變更 CPU 計數 <br> 變更資源 <br> 設定 managedBy <br> 磁片變更追蹤 <br> 磁片租用 <br> 顯示連接設定 <br> 擴充虛擬磁片 <br> 主機 USB 裝置 <br> 記憶體 <br> 修改裝置設定 <br> 查詢容錯相容性 <br> 查詢無人擁有的檔案 <br> 原始裝置 <br> 從路徑重載 <br> 移除磁片 <br> 重新命名 <br> 重設來賓資訊 <br> 設定注釋 <br> 設定 <br> Cloud-init 放置 <br> 切換分叉父系 <br> 解除鎖定虛擬機器 <br> 升級虛擬機器相容性 |
| **虛擬機器 > 來賓作業** | 來賓操作別名修改 <br> 來賓操作別名查詢 <br> 來賓操作修改 <br> 來賓操作程式執行 <br> 來賓操作查詢 |
| **虛擬機器 > 互動** | 回答問題 <br> 虛擬機器上的備份操作 <br> 設定 CD 媒體 <br> 設定軟碟媒體 <br> 主控台互動 <br> 建立螢幕擷取畫面 <br> 重組所有磁片 <br> 裝置連線 <br> 拖放 <br> 客體作業系統管理（依 VIX API） <br> 插入 USB 隱藏掃描碼 <br> 暫停或 Unpause <br> 執行抹除或壓縮作業 <br> 關閉電源 <br> 開啟電源 <br> 在虛擬機器上記錄會話 <br> 在虛擬機器上重新執行會話 <br> 重設 <br> 繼續容錯 <br> 暫止 <br> 暫止容錯 <br> 測試容錯移轉 <br> 測試重新開機次要 VM <br> 關閉容錯功能 <br> 開啟容錯功能 <br> VMware 工具安裝 |
| **虛擬機器 > 清查** | 從現有的建立 <br> 新建 <br> 移動 <br> 註冊 <br> 移除 <br> Unregister  |
| **虛擬機器 > 布建** | 允許磁片存取 <br> 允許檔案存取 <br> 允許唯讀磁碟存取 <br> 允許虛擬機器下載 <br> 允許虛擬機器檔案上傳 <br> 複製範本 <br> 複製虛擬機器 <br> 從虛擬機器建立範本 <br> 自訂 <br> 部署範本 <br> 標記為範本 <br> 標示為虛擬機器 <br> 修改自訂規格 <br> 升級磁片 <br> 讀取自訂規格 |
| **虛擬機器 > 服務設定** | 允許通知 <br> 允許輪詢全域事件通知 <br> 管理服務設定 <br> 修改服務設定 <br> 查詢服務設定 <br> 讀取服務設定 |
| **虛擬機器 > 快照集管理** | 建立快照集 <br> 移除快照集 <br> 重新命名快照集 <br> 還原為快照集 |
| **虛擬機器 > vSphere 複寫** | 設定複寫 <br> 管理複寫 <br> 監視複寫 |
| **vService** | 建立相依性 <br> 終結相依性 <br> 重新設定相依性設定 <br> 更新相依性 |

### <a name="cloud-cluster-admin-role"></a>雲端叢集-管理員-角色

| **類別** | **特權** |
|----------|-----------|
| **Datastore** | 配置空間 <br> 瀏覽資料存放區 <br> 設定資料存放區 <br> 低層級檔案作業 <br> 移除資料存放區 <br> 重新命名資料存放區 <br> 更新虛擬機器檔案 <br> 更新虛擬機器中繼資料 |
| **資料夾** | 建立資料夾 <br> 刪除資料夾 <br> 移動資料夾 <br> 重新命名資料夾 |
| **主機 > 設定**  | 儲存體分割區設定 |
| **vSphere 標記** | 指派或取消指派 vSphere 標記 <br> 建立 vSphere 標記 <br> 建立 vSphere 標記類別 <br> 刪除 vSphere 標記 <br> 刪除 vSphere 標記類別 <br> 編輯 vSphere 標記 <br> 編輯 vSphere 標記類別目錄 <br> 修改類別的 UsedBy 欄位 <br> 修改標記的 UsedBy 欄位 |
| **Network** | 指派網路 |
| **Resource** | 套用建議 <br> 將 vApp 指派給資源集區 <br> 將虛擬機器指派給資源集區 <br> 建立資源集區 <br> 遷移電源關閉的虛擬機器 <br> 在虛擬機器上遷移電源 <br> 修改資源集區 <br> 移動資源集區 <br> 查詢 vMotion <br> 移除資源集區 <br> 重新命名資源集區 |
| **vApp** | 新增虛擬機器 <br> 指派資源集區 <br> 指派 vApp <br> 複製 <br> 建立 <br> 刪除 <br> 匯出 <br> 匯入 <br> 移動 <br> 關閉電源 <br> 開啟電源 <br> 重新命名 <br> 暫止 <br> Unregister  <br> View OVF 環境 <br> vApp 應用程式設定 <br> vApp 實例設定 <br> vApp managedBy 設定 <br> vApp 資源設定 |
| **VRMPolicy** | 查詢 VRMPolicy <br> 更新 VRMPolicy |
| **虛擬機器 > 設定** | 新增現有的磁片 <br> 新增磁片 <br> 新增或移除裝置 <br> 進階 <br> 變更 CPU 計數 <br> 變更資源 <br> 設定 managedBy <br> 磁片變更追蹤 <br> 磁片租用 <br> 顯示連接設定 <br> 擴充虛擬磁片 <br> 主機 USB 裝置 <br> 記憶體 <br> 修改裝置設定 <br> 查詢容錯相容性 <br> 查詢無人擁有的檔案 <br> 原始裝置 <br> 從路徑重載 <br> 移除磁片 <br> 重新命名 <br> 重設來賓資訊 <br> 設定注釋 <br> 設定 <br> Cloud-init 放置 <br> 切換分叉父系 <br> 解除鎖定虛擬機器 <br> 升級虛擬機器相容性 |
| **虛擬機器 > 來賓作業** | 來賓操作別名修改 <br> 來賓操作別名查詢 <br> 來賓操作修改 <br> 來賓操作程式執行 <br> 來賓操作查詢 |
| **虛擬機器 > 互動** | 回答問題 <br> 虛擬機器上的備份操作 <br> 設定 CD 媒體 <br> 設定軟碟媒體 <br> 主控台互動 <br> 建立螢幕擷取畫面 <br> 重組所有磁片 <br> 裝置連線 <br> 拖放 <br> 客體作業系統管理（依 VIX API） <br> 插入 USB 隱藏掃描碼 <br> 暫停或 Unpause <br> 執行抹除或壓縮作業 <br> 關閉電源 <br> 開啟電源 <br> 在虛擬機器上記錄會話 <br> 在虛擬機器上重新執行會話 <br> 重設 <br> 繼續容錯 <br> 暫止 <br> 暫止容錯 <br> 測試容錯移轉 <br> 測試重新開機次要 VM <br> 關閉容錯功能 <br> 開啟容錯功能 <br> VMware 工具安裝
| **虛擬機器 > 清查** | 從現有的建立 <br> 新建 <br> 移動 <br> 註冊 <br> 移除 <br> Unregister  |
| **虛擬機器 > 布建** | 允許磁片存取 <br> 允許檔案存取 <br> 允許唯讀磁碟存取 <br> 允許虛擬機器下載 <br> 允許虛擬機器檔案上傳 <br> 複製範本 <br> 複製虛擬機器 <br> 從虛擬機器建立範本 <br> 自訂 <br> 部署範本 <br> 標記為範本 <br> 標示為虛擬機器 <br> 修改自訂規格 <br> 升級磁片  <br> 讀取自訂規格 |
| **虛擬機器 > 服務設定** | 允許通知 <br> 允許輪詢全域事件通知 <br> 管理服務設定 <br> 修改服務設定 <br> 查詢服務設定 <br> 讀取服務設定
| **虛擬機器 > 快照集管理** | 建立快照集 <br> 移除快照集 <br> 重新命名快照集 <br> 還原為快照集 |
| **虛擬機器 > vSphere 複寫** | 設定複寫 <br> 管理複寫 <br> 監視複寫 |
| **vService** | 建立相依性 <br> 終結相依性 <br> 重新設定相依性設定 <br> 更新相依性 |

### <a name="cloud-storage-admin-role"></a>雲端-儲存體-管理員-角色

| **類別** | **特權** |
|----------|-----------|
| **Datastore** | 配置空間 <br> 瀏覽資料存放區 <br> 設定資料存放區 <br> 低層級檔案作業 <br> 移除資料存放區 <br> 重新命名資料存放區 <br> 更新虛擬機器檔案 <br> 更新虛擬機器中繼資料 |
| **主機 > 設定** | 儲存體分割區設定 |
| **資料存放區叢集** | 設定資料存放區叢集 |
| **設定檔驅動的儲存體** | 設定檔驅動的儲存體更新 <br> 設定檔驅動的儲存體視圖 |
| **儲存體視圖** | 設定服務 <br> 檢視 |

### <a name="cloud-network-admin-role"></a>雲端-網路-管理員-角色

| **類別** | **特權** |
|----------|-----------|
| **dvPort 群組** | 建立 <br> 刪除 <br> 修改 <br> 原則操作 <br> 範圍作業 |
| **Network** | 指派網路 <br> 設定 <br> 移動網路 <br> 移除 |
| **虛擬機器 > 設定** | 修改裝置設定 |

### <a name="cloud-vm-admin-role"></a>雲端-VM-管理員-角色

| **類別** | **特權** |
|----------|-----------|
| **Datastore** | 配置空間 <br> 瀏覽資料存放區 |
| **Network** | 指派網路 |
| **Resource** | 將虛擬機器指派給資源集區 <br> 遷移電源關閉的虛擬機器 <br> 在虛擬機器上遷移電源
| **vApp** | 匯出 <br> 匯入 |
| **虛擬機器 > 設定** | 新增現有的磁片 <br> 新增磁片 <br> 新增或移除裝置 <br> 進階 <br> 變更 CPU 計數 <br> 變更資源 <br> 設定 managedBy <br> 磁片變更追蹤 <br> 磁片租用 <br> 顯示連接設定 <br> 擴充虛擬磁片 <br> 主機 USB 裝置 <br> 記憶體 <br> 修改裝置設定 <br> 查詢容錯相容性 <br> 查詢無人擁有的檔案 <br> 原始裝置 <br> 從路徑重載 <br> 移除磁片 <br> 重新命名 <br> 重設來賓資訊 <br> 設定注釋 <br> 設定 <br> Cloud-init 放置 <br> 切換分叉父系 <br> 解除鎖定虛擬機器 <br> 升級虛擬機器相容性 |
| **虛擬機器 >來賓作業** | 來賓操作別名修改 <br> 來賓操作別名查詢 <br> 來賓操作修改 <br> 來賓操作程式執行 <br> 來賓操作查詢    |
| **虛擬機器 >互動** | 回答問題 <br> 虛擬機器上的備份操作 <br> 設定 CD 媒體 <br> 設定軟碟媒體 <br> 主控台互動 <br> 建立螢幕擷取畫面 <br> 重組所有磁片 <br> 裝置連線 <br> 拖放 <br> 客體作業系統管理（依 VIX API） <br> 插入 USB 隱藏掃描碼 <br> 暫停或 Unpause <br> 執行抹除或壓縮作業 <br> 關閉電源 <br> 開啟電源 <br> 在虛擬機器上記錄會話 <br> 在虛擬機器上重新執行會話 <br> 重設 <br> 繼續容錯 <br> 暫止 <br> 暫止容錯 <br> 測試容錯移轉 <br> 測試重新開機次要 VM <br> 關閉容錯功能 <br> 開啟容錯功能 <br> VMware 工具安裝 |
| **虛擬機器 >清查** | 從現有的建立 <br> 新建 <br> 移動 <br> 註冊 <br> 移除 <br> Unregister  |
| **虛擬機器 >布建** | 允許磁片存取 <br> 允許檔案存取 <br> 允許唯讀磁碟存取 <br> 允許虛擬機器下載 <br> 允許虛擬機器檔案上傳 <br> 複製範本 <br> 複製虛擬機器 <br> 從虛擬機器建立範本 <br> 自訂 <br> 部署範本 <br> 標記為範本 <br> 標示為虛擬機器 <br> 修改自訂規格 <br> 升級磁片 <br> 讀取自訂規格 |
| **虛擬機器 >服務設定** | 允許通知 <br> 允許輪詢全域事件通知 <br> 管理服務設定 <br> 修改服務設定 <br> 查詢服務設定 <br> 讀取服務設定
| **虛擬機器 >快照集管理** | 建立快照集 <br> 移除快照集 <br> 重新命名快照集 <br> 還原為快照集 |
| **虛擬機器 >vSphere 複寫** | 設定複寫 <br> 管理複寫 <br> 監視複寫 |
| **vService** | 建立相依性 <br> 終結相依性 <br> 重新設定相依性設定 <br> 更新相依性 |
