---
title: 叢集設定最佳作法
description: 當您為 Azure 虛擬機器上的 SQL Server 設定高可用性和嚴重損壞修復 (HADR) 時，請瞭解支援的叢集設定，例如支援的仲裁或連接路由選項。
services: virtual-machines
documentationCenter: na
author: MashaMSFT
editor: monicar
tags: azure-service-management
ms.service: virtual-machines-sql
ms.subservice: hadr
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/02/2020
ms.author: mathoma
ms.openlocfilehash: 5a2540aeb36cfcb2048ec994bbb486badc8a68d1
ms.sourcegitcommit: dfc4e6b57b2cb87dbcce5562945678e76d3ac7b6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2020
ms.locfileid: "97358804"
---
# <a name="cluster-configuration-best-practices-sql-server-on-azure-vms"></a>叢集設定最佳做法 (Azure VM 上的 SQL Server)
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

叢集用於高可用性和嚴重損壞修復 (HADR) 搭配 Azure 虛擬機器 (Vm) 上的 SQL Server。 

本文針對 [容錯移轉叢集實例 (fci) ](failover-cluster-instance-overview.md) 和 [可用性群組](availability-group-overview.md) 提供叢集設定的最佳做法，以搭配 Azure vm 上的 SQL Server 使用。 


## <a name="networking"></a>網路功能

針對每部伺服器使用單一 NIC (叢集節點) 和單一子網。 Azure 網路功能具有實體冗余，讓 Azure 虛擬機器來賓叢集上不需要額外的 Nic 和子網。 叢集驗證報告會警告您節點只能在單一網路上觸達。 您可以在 Azure 虛擬機器來賓容錯移轉叢集上忽略此警告。

### <a name="tuning-failover-cluster-network-thresholds"></a>調整容錯移轉叢集網路閾值

在具有 SQL Server AlwaysOn 的 Azure Vm 中執行 Windows 容錯移轉叢集節點時，建議將叢集設定變更為較寬鬆的監視狀態。  這會讓叢集更穩定且更可靠。  如需有關此功能的詳細資訊，請參閱 [使用 SQL AlwaysOn 的 IaaS-調整容錯移轉叢集網路閾值](/windows-server/troubleshoot/iaas-sql-failover-cluster)。

## <a name="quorum"></a>Quorum

雖然雙節點叢集在沒有 [仲裁資源](/windows-server/storage/storage-spaces/understand-quorum)的情況下運作，但客戶絕對必須使用仲裁資源才能取得生產環境支援。 叢集驗證不會傳遞任何無仲裁資源的叢集。 

技術上來說，有三個節點的叢集在沒有仲裁資源) 的情況下，可以在單一節點遺失 (到兩個節點的情況下存活。 但在叢集移至兩個節點之後，如果發生節點遺失或通訊失敗，叢集資源就會離線，以防止分裂式案例。

設定仲裁資源可讓叢集在線上只能有一個節點上線。

下表列出可依建議搭配 Azure VM 使用的仲裁選項，磁片見證是慣用的選擇： 


||[磁碟見證](/windows-server/failover-clustering/manage-cluster-quorum#configure-the-cluster-quorum)  |[雲端見證](/windows-server/failover-clustering/deploy-cloud-witness)  |[檔案共用見證](/windows-server/failover-clustering/manage-cluster-quorum#configure-the-cluster-quorum)  |
|---------|---------|---------|---------|
|**支援的 OS**| 全部 |Windows Server 2016+| 全部|


### <a name="disk-witness"></a>磁碟見證

磁片見證是叢集可用儲存群組中的小型叢集磁片。 此磁片具有高可用性，而且可以在節點之間進行容錯移轉。 它包含叢集資料庫的複本，預設大小通常小於 1 GB。 磁片見證是使用 Azure 共用磁片 (或任何共用磁片解決方案，例如共用 SCSI、iSCSI 或光纖通道 SAN) 的任何叢集慣用的仲裁選項。  叢集共用磁片區不能用來做為磁片見證。

將 Azure 共用磁片設定為磁片見證。 

若要開始使用，請參閱 [設定磁片見證](/windows-server/failover-clustering/manage-cluster-quorum#configure-the-cluster-quorum)。


**支援的 OS**：全部   


### <a name="cloud-witness"></a>雲端見證

雲端見證是容錯移轉叢集仲裁見證的一種類型，會使用 Microsoft Azure 來提供叢集仲裁的投票。 預設大小約為 1 MB，而且只包含時間戳記。 雲端見證適用于多個網站、多個區域和多個區域的部署。

若要開始使用，請參閱 [設定雲端見證](/windows-server/failover-clustering/deploy-cloud-witness#CloudWitnessSetUp)。


**支援的 OS**：Windows Server 2016 及更新版本   


### <a name="file-share-witness"></a>檔案共用見證

檔案共用見證是 SMB 檔案共用，通常是在執行 Windows Server 的檔案伺服器上進行設定。 它會在見證記錄檔中維護叢集資訊，但不會儲存叢集資料庫的複本。 在 Azure 中，您可以將 Azure 檔案 [共用](../../../storage/files/storage-how-to-create-file-share.md) 設定為檔案共用見證，也可以在不同的虛擬機器上使用檔案共用。

如果您要使用 Azure 檔案共用，您可以使用用來 [掛接 Premium 檔案共用](failover-cluster-instance-premium-file-share-manually-configure.md#mount-premium-file-share)的相同程式來掛接它。 

若要開始使用，請參閱 [設定檔案共用見證](/windows-server/failover-clustering/manage-cluster-quorum#configure-the-cluster-quorum)。


**支援的 OS**：Windows Server 2012 及更新版本   

## <a name="connectivity"></a>連線能力

在傳統內部部署網路環境中，SQL Server 容錯移轉叢集實例似乎是單一電腦上所執行 SQL Server 的單一實例。 由於容錯移轉叢集實例會從節點容錯移轉到節點，因此實例的虛擬網路名稱 (VNN) 會提供統一的連接點，並允許應用程式在不知道目前作用中的節點的情況下，連接到 SQL Server 實例。 發生容錯移轉時，會在新的使用中節點啟動後，將虛擬網路名稱註冊到該節點。 此程式對連接至 SQL Server 的用戶端或應用程式而言是透明的，這會將用戶端或應用程式在失敗期間所遇到的停機時間降到最低。 同樣地，可用性群組接聽程式會使用 VNN 將流量路由傳送至適當的複本。 

使用具有 Azure Load Balancer 的 VNN 或分散式網路名稱 (DNN) ，將流量路由至容錯移轉叢集實例的 VNN，並使用 Azure Vm 上的 SQL Server，或取代可用性群組中的現有 VNN 接聽程式。 


下表將比較 HADR 連接的可支援性： 

| |**虛擬網路名稱 (VNN)**  |**分散式網路名稱 (DNN)**  |
|---------|---------|---------|
|**作業系統最低版本**| 全部 | Windows Server 2016 |
|**最低 SQL Server 版本** |全部 |適用于 FCI 的 SQL Server 2019 CU2 () <br/> AG ) 的 SQL Server 2019 CU8 (|
|**支援的 HADR 解決方案** | 容錯移轉叢集執行個體 <br/> 可用性群組 | 容錯移轉叢集執行個體 <br/> 可用性群組|


### <a name="virtual-network-name-vnn"></a>虛擬網路名稱 (VNN)

由於虛擬 IP 存取點在 Azure 中的運作方式不同，因此您必須設定 [Azure Load Balancer](../../../load-balancer/index.yml) ，以將流量路由傳送至 FCI 節點或可用性群組接聽程式的 IP 位址。 在 Azure 虛擬機器中，負載平衡器會保存叢集 SQL Server 資源所依賴的 VNN IP 位址。 負載平衡器會分散抵達前端的輸入流量，然後將該流量路由傳送至後端集區所定義的實例。 您可以使用負載平衡規則和健康情況探查來設定流量流程。 使用 SQL Server FCI，後端集區實例是執行 SQL Server 的 Azure 虛擬機器。 

使用負載平衡器時，會有輕微的容錯移轉延遲，因為健康狀態探查預設會每隔10秒執行一次運作檢查。 

若要開始使用，請瞭解如何設定[容錯移轉叢集實例](failover-cluster-instance-vnn-azure-load-balancer-configure.md)或[可用性群組](availability-group-vnn-azure-load-balancer-configure.md)的 Azure Load Balancer

**支援的 OS**：全部   
**支援的 SQL 版本**：全部   
**支援的 HADR 解決方案**：容錯移轉叢集實例和可用性群組   


### <a name="distributed-network-name-dnn"></a>分散式網路名稱 (DNN)

分散式網路名稱是適用于 SQL Server 2019 的新 Azure 功能。 DNN 提供另一種方式，讓 SQL Server 用戶端連接到 SQL Server 容錯移轉叢集實例或可用性群組，而不需要使用負載平衡器。 

建立 DNN 資源時，叢集會將 DNS 名稱系結至叢集中所有節點的 IP 位址。 SQL 用戶端會嘗試連線到此清單中的每個 IP 位址，以找出要連接的資源。  您可以藉由 `MultiSubnetFailover=True` 在連接字串中指定來加速此程式。 這項設定會指示提供者以平行方式嘗試所有 IP 位址，讓用戶端可以立即連接到 FCI 或接聽程式。 

在可能的情況下，建議您在負載平衡器上使用分散式網路名稱，原因是： 
- 端對端解決方案更健全，因為您不再需要維護負載平衡器資源。 
- 刪除負載平衡器探查可將容錯移轉持續時間降至最低。 
- DNN 可透過 Azure Vm 上的 SQL Server，簡化容錯移轉叢集實例或可用性群組接聽程式的布建和管理。 

在使用 DNN 時，大部分的 SQL Server 功能都會以透明的方式與 FCI 和可用性群組搭配運作，但有些功能可能需要特別考慮。 若要深入瞭解，請參閱 [FCI 和 DNN 互通性](failover-cluster-instance-dnn-interoperability.md) 和 [AG 和 DNN 互通性](availability-group-dnn-interoperability.md) 。 

若要開始使用，請瞭解如何為[容錯移轉叢集實例](failover-cluster-instance-distributed-network-name-dnn-configure.md)或[可用性群組](availability-group-distributed-network-name-dnn-listener-configure.md)設定分散式網路名稱資源

**支援的 OS**：Windows Server 2016 及更新版本   
**支援的 SQL 版本**： SQL SERVER 2019 CU2 (FCI) 和 SQL SERVER 2019 CU8 (AG)    
**支援的 HADR 解決方案**：容錯移轉叢集實例和可用性群組   


## <a name="limitations"></a>限制

當您在 Azure 虛擬機器上使用 FCI 或可用性群組和 SQL Server 時，請考慮下列限制。 

### <a name="msdtc"></a>MSDTC 

Azure 虛擬機器支援在儲存體位於叢集共用磁碟區 (CSV) 和 [Azure Standard Load Balancer](../../../load-balancer/load-balancer-overview.md) 上的 Windows Server 2019 上使用 Microsoft 分散式交易協調器 (MSDTC)，也支援在使用 Azure 共用磁碟的 SQL Server VM 上使用 MSDTC。 

在 Azure 虛擬機器上，具有叢集共用磁碟區的 Windows Server 2016 或更早版本不支援 MSDTC，因為：

- 叢集化的 MSDTC 資源無法設為使用共用儲存體。 若在 Windows Server 2016 上建立 MSDTC 資源，即使有儲存體可用，系統也不會顯示任何可用的共用儲存體。 Windows Server 2019 中已修正此問題。
- 基本負載平衡器不處理 RPC 連接埠。


## <a name="next-steps"></a>後續步驟

確定您的解決方案有適當的最佳作法之後，請開始 [準備 SQL SERVER VM 以進行 FCI](failover-cluster-instance-prepare-vm.md) ，或使用 [Azure 入口網站](availability-group-azure-portal-configure.md)、 [Azure CLI/PowerShell](./availability-group-az-commandline-configure.md)或 [Azure 快速入門範本](availability-group-quickstart-template-configure.md)來建立可用性群組。