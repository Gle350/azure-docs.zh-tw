---
title: 使用 SLES 上的 Azure NetApp Files 以待命方式進行相應放大 SAP HanaMicrosoft Docs
description: 瞭解如何使用 SUSE Linux Enterprise Server 上的 Azure NetApp Files，在 Azure Vm 上部署具有待命節點的 SAP Hana 相應放大系統。
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: rdeltcheva
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 5e514964-c907-4324-b659-16dd825f6f87
ms.service: virtual-machines-windows
ms.subservice: workloads
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 01/05/2021
ms.author: radeltch
ms.openlocfilehash: a152735d21a347262ce6485e6110f9e040a0071a
ms.sourcegitcommit: 67b44a02af0c8d615b35ec5e57a29d21419d7668
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97916230"
---
# <a name="deploy-a-sap-hana-scale-out-system-with-standby-node-on-azure-vms-by-using-azure-netapp-files-on-suse-linux-enterprise-server"></a>在 SUSE Linux Enterprise Server 上使用 Azure NetApp Files 於 Azure VM 上部署 SAP HANA 擴增系統與待命節點 \(部分機器翻譯\) 

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

[anf-azure-doc]:https://docs.microsoft.com/azure/azure-netapp-files/
[anf-avail-matrix]:https://azure.microsoft.com/global-infrastructure/services/?products=netapp&regions=all 
[anf-register]:https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-register
[anf-sap-applications-azure]:https://www.netapp.com/us/media/tr-4746.pdf

[2205917]:https://launchpad.support.sap.com/#/notes/2205917
[1944799]:https://launchpad.support.sap.com/#/notes/1944799
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[1410736]:https://launchpad.support.sap.com/#/notes/1410736
[1900823]:https://launchpad.support.sap.com/#/notes/1900823

[sap-swcenter]:https://support.sap.com/en/my-support/software-downloads.html

[suse-ha-guide]:https://www.suse.com/products/sles-for-sap/resource-library/sap-best-practices/
[suse-drbd-guide]:https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha_techguides/book_sleha_techguides.html
[suse-ha-12sp3-relnotes]:https://www.suse.com/releasenotes/x86_64/SLE-HA/12-SP3/

[sap-hana-ha]:sap-hana-high-availability.md
[nfs-ha]:high-availability-guide-suse-nfs.md


本文說明如何使用 [Azure NetApp Files](../../../azure-netapp-files/azure-netapp-files-introduction.md) 作為共用存放磁片區，以在 azure 虛擬機器上具有待命功能的相應放大設定中部署高可用性 SAP Hana 系統 () vm。  

在範例設定、安裝命令等等中，HANA 實例為 **03** ，而 HANA 系統識別碼是 **HN1**。 這些範例是以 HANA 2.0 SP4 和 SAP 12 SP4 的 SUSE Linux Enterprise Server 為基礎。 

開始之前，請參閱下列 SAP 附注和論文：

* [Azure NetApp Files 文件][anf-azure-doc] 
* SAP 附注 [1928533] 包含：  
  * SAP 軟體部署支援的 Azure VM 大小清單
  * Azure VM 大小的重要容量資訊
  * 支援的 SAP 軟體，以及作業系統 (OS) 與資料庫組合
  * Microsoft Azure 上的 Windows 和 Linux 所需的 SAP 核心版本
* SAP 附注 [2015553]：列出 AZURE 中 sap 支援的 sap 軟體部署的必要條件
* SAP 附注 [2205917]：包含適用于 SAP 應用程式之 SUSE Linux Enterprise Server 的建議作業系統設定
* SAP 附注 [1944799]：包含適用于 Sap 應用程式之 SUSE LINUX ENTERPRISE SERVER 的 Sap 指導方針
* SAP 附注 [2178632]：包含 Azure 中針對 SAP 回報的所有監視計量的詳細資訊
* SAP 附注 [2191498]：包含 Azure 中 Linux 所需的 SAP Host Agent 版本
* SAP 附注 [2243692]：包含 Azure 中 LINUX 上 sap 授權的相關資訊
* SAP 附注 [1984787]：包含 SUSE Linux Enterprise Server 12 的一般資訊
* SAP 附注 [1999351]：包含適用于 SAP 的 Azure 增強型監視延伸模組的其他疑難排解資訊
* SAP 附注 [1900823]：包含 SAP Hana 儲存體需求的相關資訊
* [Sap 社區 Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes)：包含適用于 Linux 的所有必要 SAP 附注
* [適用於 SAP on Linux 的 Azure 虛擬機器規劃和實作][planning-guide]
* [在 Linux 上為 SAP 進行 Azure 虛擬機器部署][deployment-guide]
* [適用於 SAP on Linux 的 Azure 虛擬機器 DBMS 部署][dbms-guide]
* [SUSE SAP HA 最佳做法指南][suse-ha-guide]：包含設定 NetWeaver 高可用性和 SAP Hana 系統複寫內部部署 (用來作為一般基準的所有必要資訊;他們提供更詳細的資訊) 
* [SUSE 高可用性延伸模組 12 SP3 版本資訊][suse-ha-12sp3-relnotes]
* [使用 Azure NetApp Files 在 Microsoft Azure 上的 NetApp SAP 應用程式][anf-sap-applications-azure]


## <a name="overview"></a>概觀

達到 HANA 高可用性的其中一個方法是設定主機自動容錯移轉。 若要設定主機自動容錯移轉，您可以將一或多部虛擬機器新增至 HANA 系統，並將其設定為待命節點。 當作用中節點失敗時，待命節點會自動接管。 在使用 Azure 虛擬機器所呈現的設定中，您可以使用 [Azure NetApp Files 上的 NFS](../../../azure-netapp-files/azure-netapp-files-introduction.md)來達成自動容錯移轉。  

> [!NOTE]
> 待命節點需要存取所有資料庫磁片區。 HANA 磁片區必須裝載為 NFSv4 磁片區。 NFSv4 通訊協定中改良的檔案租用型鎖定機制可用於隔離 `I/O` 。 

> [!IMPORTANT]
> 若要建立支援的設定，您必須將 HANA 資料和記錄磁片區部署為 Nfsv4.1 4.1 磁片區，然後使用 Nfsv4.1 4.1 通訊協定掛接它們。 NFSv3 不支援使用待命節點的 HANA 主機自動容錯移轉設定。

![SAP NetWeaver 高可用性概觀](./media/high-availability-guide-suse-anf/sap-hana-scale-out-standby-netapp-files-suse.png)

在上圖中（遵循 SAP Hana 網路建議），在一個 Azure 虛擬網路內表示三個子網： 
* 針對用戶端通訊
* 與儲存系統通訊
* 內部 HANA 節點間通訊

Azure NetApp 磁片區位於不同的子網中，並 [委派給 Azure Netapp Files](../../../azure-netapp-files/azure-netapp-files-delegate-subnet.md)。  

在此範例設定中，子網為：  

  - `client` 10.23.0.0/24  
  - `storage` 10.23.2.0/24  
  - `hana` 10.23.3.0/24  
  - `anf` 10.23.1.0/26  

## <a name="set-up-the-azure-netapp-files-infrastructure"></a>設定 Azure NetApp Files 基礎結構 

繼續進行 Azure NetApp Files 基礎結構的設定之前，請先熟悉 [Azure Netapp Files 檔][anf-azure-doc]。 

Azure NetApp Files 可在數個 [azure 區域](https://azure.microsoft.com/global-infrastructure/services/?products=netapp)中使用。 查看您選取的 Azure 區域是否提供 Azure NetApp Files。  

如需 azure 區域之 Azure NetApp Files 可用性的詳細資訊，請參閱 azure [區域的 Azure Netapp Files 可用性][anf-avail-matrix]。  

在您部署 Azure NetApp Files 之前，請先向 Azure netapp files 註冊以 [註冊][anf-register]azure netapp files 指示，以要求登入 Azure netapp files。 

### <a name="deploy-azure-netapp-files-resources"></a>部署 Azure NetApp Files 資源  

下列指示假設您已部署 [Azure 虛擬網路](../../../virtual-network/virtual-networks-overview.md)。 將裝載 Azure NetApp Files 資源的 Azure NetApp Files 資源和 Vm，必須部署在相同的 Azure 虛擬網路或對等互連 Azure 虛擬網路中。  

1. 如果您尚未部署資源，請要求在 [Azure NetApp Files 上架](../../../azure-netapp-files/azure-netapp-files-register.md)。  

2. 遵循 [建立 netapp 帳戶](../../../azure-netapp-files/azure-netapp-files-create-netapp-account.md)中的指示，在您選取的 Azure 區域中建立 NetApp 帳戶。  

3. 遵循 [設定 Azure Netapp files 容量](../../../azure-netapp-files/azure-netapp-files-set-up-capacity-pool.md)集區中的指示，設定 Azure netapp files 容量集區。  

   本文所提供的 HANA 架構在 *Ultra 服務* 層級使用單一 Azure NetApp Files 容量集區。 針對 Azure 上的 HANA 工作負載，我們建議使用 Azure NetApp Files *Ultra* 或 *Premium* [服務層級](../../../azure-netapp-files/azure-netapp-files-service-levels.md)。  

4. 將子網委派給 Azure NetApp Files，如將 [子網委派至 Azure Netapp files](../../../azure-netapp-files/azure-netapp-files-delegate-subnet.md)中的指示所述。  

5. 遵循 [建立適用于 Azure Netapp files 的 NFS 磁片](../../../azure-netapp-files/azure-netapp-files-create-volumes.md)區中的指示，部署 Azure netapp files 磁片區。  

   當您要部署磁片區時，請務必選取 **nfsv4.1 4.1** 版。 目前，必須將 Nfsv4.1 4.1 的存取權新增至允許清單。 在指定的 Azure NetApp Files [子網路](/rest/api/virtualnetwork/subnets)中部署磁碟區。 Azure NetApp 磁碟區的 IP 位址會自動指派。 
   
   請記住，Azure NetApp Files 資源和 Azure Vm 必須位於相同的 Azure 虛擬網路或對等互連 Azure 虛擬網路中。 例如， **HN1**-Mnt00001、 **HN1**-log-mnt00001 等是磁片區名稱和 nfs://10.23.1.5/**HN1**-data-mnt00001、nfs://10.23.1.4/**HN1**-Log-mnt00001 等等，都是 Azure NetApp Files 磁片區的檔案路徑。  

   * 磁片區 **HN1**-資料-mnt00001 (nfs://10.23.1.5/**HN1**-data-mnt00001) 
   * 磁片區 **HN1**-資料-mnt00002 (nfs://10.23.1.6/**HN1**-data-mnt00002) 
   * 磁片區 **HN1**-log-mnt00001 (nfs://10.23.1.4/**HN1**-log mnt00001) 
   * 磁片區 **HN1**-log-mnt00002 (nfs://10.23.1.6/**HN1**-log mnt00002) 
   * 磁片區 **HN1**-共用 (nfs://10.23.1.4/**HN1**-共用) 
   
   在此範例中，我們針對每個 HANA 資料和記錄磁片區使用個別的 Azure NetApp Files 磁片區。 若要在較小或非生產力的系統上進行更具成本優化的設定，您可以將所有的資料裝載和所有記錄裝載于單一磁片區上。  

### <a name="important-considerations"></a>重要考量

當您在 SUSE 高可用性架構上建立適用于 SAP NetWeaver 的 Azure NetApp Files 時，請注意下列重要考慮：

- 容量集區的最小值是 4 tib (TiB) 。  
- 磁片區大小下限為 100 32,767 gib (GiB) 。
- Azure NetApp Files 和將掛接 Azure NetApp Files 磁片區的所有虛擬機器，都必須位於相同的 Azure 虛擬網路或相同區域中的 [對等互連虛擬網路](../../../virtual-network/virtual-network-peering-overview.md) 中。  
- 選取的虛擬網路必須有委派給 Azure NetApp Files 的子網。
- Azure NetApp Files 磁片區的輸送量是磁片區配額和服務層級的功能，如 [Azure NetApp Files 的服務層級](../../../azure-netapp-files/azure-netapp-files-service-levels.md)中所述。 當您調整 HANA Azure NetApp 磁片區的大小時，請確定產生的輸送量符合 HANA 系統需求。  
- 使用 Azure NetApp Files [匯出原則](../../../azure-netapp-files/azure-netapp-files-configure-export-policy.md)，您可以控制允許的用戶端、存取類型 (讀寫、唯讀等) 。 
- Azure NetApp Files 功能尚未感知區域。 目前，此功能不會部署在 Azure 區域中的所有可用性區域。 請留意某些 Azure 區域中可能出現的延遲情形。  
-  

> [!IMPORTANT]
> 針對 SAP Hana 工作負載，低延遲很重要。 請與您的 Microsoft 代表合作，以確保虛擬機器和 Azure NetApp Files 磁碟區已部署在附近。  

### <a name="sizing-for-hana-database-on-azure-netapp-files"></a>針對 Azure NetApp Files 上的 HANA 資料庫進行大小調整

Azure NetApp Files 磁片區的輸送量是磁片區大小和服務層級的功能，如 [Azure NetApp Files 的服務層級](../../../azure-netapp-files/azure-netapp-files-service-levels.md)中所述。 

當您在 Azure 中設計 SAP 的基礎結構時，請留意 SAP 的一些最低儲存體需求，這些需求會轉譯為最小輸送量特性：

- 在/hana/log 上啟用每秒 250 mb 的讀寫 (MB/s) 具有 1 MB 的 i/o 大小。  
- 啟用至少 400 MB/s 的讀取活動，以/hana/data 16 MB 和 64 MB 的 i/o 大小。  
- 針對具有 16 MB 和 64 MB i/o 大小的/hana/data，啟用至少 250 MB/s 的寫入活動。 

每 1 TiB 磁碟區配額的 [Azure NetApp Files 輸送量限制](../../../azure-netapp-files/azure-netapp-files-service-levels.md)如下：
- 進階儲存體層-64 MiB/秒  
- Ultra 儲存層-128 MiB/秒  

為了符合 SAP 的資料和記錄的最小輸送量需求，以及/hana/shared 的指導方針，建議的大小為：

| 磁碟區 | 的大小<br>進階儲存層 | 的大小<br>Ultra 儲存層 | 支援的 NFS 通訊協定 |
| --- | --- | --- | --- |
| /hana/log/ | 4 TiB | 2 TiB | v4.1 |
| /hana/data | 6.3 TiB | 3.2 TiB | v4.1 |
| HANA/shared | 每4個背景工作節點的最大 (512 GB、1xRAM)  | 每4個背景工作節點的最大 (512 GB、1xRAM)  | v3 或 4.1 版 |

本文中使用 Azure NetApp Files Ultra 儲存層所呈現的版面配置 SAP Hana 設定如下：

| 磁碟區 | 的大小<br>Ultra 儲存層 | 支援的 NFS 通訊協定 |
| --- | --- | --- |
| /hana/log/mnt00001 | 2 TiB | v4.1 |
| /hana/log/mnt00002 | 2 TiB | v4.1 |
| /hana/data/mnt00001 | 3.2 TiB | v4.1 |
| /hana/data/mnt00002 | 3.2 TiB | v4.1 |
| HANA/shared | 2 TiB | v3 或 4.1 版 |

> [!NOTE]
> 此處所述的 Azure NetApp Files 大小調整建議的目標是符合 SAP 針對其基礎結構提供者所建議的最低需求。 在實際的客戶部署和工作負載案例中，這些大小可能不足。 請將這些建議當作起點，並且根據特定工作負載的需求來調整。  

> [!TIP]
> 您可以動態調整 Azure NetApp Files 磁片區的大小，而不需要 *卸載* 磁片區、停止虛擬機器或停止 SAP Hana。 這種方法可讓您彈性地滿足應用程式預期和非預期的輸送量需求。

## <a name="deploy-linux-virtual-machines-via-the-azure-portal"></a>透過 Azure 入口網站部署 Linux 虛擬機器

首先您需要建立 Azure NetApp Files 磁碟區。 然後，執行下列步驟：
1. 在您的[azure 虛擬網路](../../../virtual-network/virtual-networks-overview.md)中建立[azure 虛擬網路子網](../../../virtual-network/virtual-network-manage-subnet.md)。 
1. 部署 VM。 
1. 建立額外的網路介面，並將網路介面連結至對應的 Vm。  

   每部虛擬機器都有三個網路介面，其對應至三個 Azure 虛擬網路子網 (`client` ， `storage` 並 `hana`) 。 

   如需詳細資訊，請參閱 [在 Azure 中建立具有多個網路介面卡的 Linux 虛擬機器](../../linux/multiple-nics.md)。  

> [!IMPORTANT]
> 針對 SAP Hana 工作負載，低延遲很重要。 若要達到低延遲，請與您的 Microsoft 代表合作，以確保虛擬機器和 Azure NetApp Files 磁片區會以接近的方式部署。 當您將使用 SAP Hana Azure NetApp Files 的 [新 SAP Hana 系統](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxjSlHBUxkJBjmARn57skvdUQlJaV0ZBOE1PUkhOVk40WjZZQVJXRzI2RC4u) 上線時，請提交所需的資訊。 
 
接下來的指示假設您已建立資源群組、Azure 虛擬網路和三個 Azure 虛擬網路子網： `client` `storage` 和 `hana` 。 當您部署 Vm 時，請選取用戶端子網，讓用戶端網路介面是 Vm 上的主要介面。 您也需要透過「儲存體子網閘道」來設定 Azure NetApp Files 委派子網的明確路由。 

> [!IMPORTANT]
> 請確定您選取的作業系統已通過 SAP 認證，可 SAP Hana 您所使用的特定 VM 類型。 如需這些類型的 SAP Hana 認證的 VM 類型和作業系統版本清單，請移至 [SAP Hana 認證的 IaaS 平臺](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) 網站。 按一下所列 VM 類型的詳細資料，取得該類型的 SAP Hana 支援 OS 版本的完整清單。  

1. 建立 SAP Hana 的可用性設定組。 請務必設定更新網域上限。  

2. 執行下列步驟，以建立三個虛擬機器 (**hanadb1**、 **hanadb2**、 **hanadb3**) ：  

   a. 使用 Azure 資源庫中支援 SAP Hana 的使用 SLES4SAP 映射。 在此範例中，我們使用了使用 SLES4SAP 12 SP4 映射。  

   b. 選取您稍早為 SAP Hana 建立的可用性設定組。  

   c. 選取用戶端 Azure 虛擬網路子網。 選取 [ [加速網路](../../../virtual-network/create-vm-accelerated-networking-cli.md)]。  

   當您部署虛擬機器時，系統會自動產生網路介面名稱。 為了簡單起見，我們將參考自動產生的網路介面，這些介面會附加至用戶端 Azure 虛擬網路子網，作為 **hanadb1 客戶** 端、 **hanadb2 客戶** 端和 **hanadb3 用戶端**。 

3. 為每個虛擬機器建立三個網路介面， `storage` 在此範例中 (的虛擬網路子網、 **hanadb1 儲存體**、 **hanadb2 儲存體** 和 **hanadb3 儲存體**) 。  

4. 針對虛擬網路子 (網，建立三個網路介面（每個虛擬機器各一個），例如 `hana` **hanadb1-hana**、 **hanadb2-hana** 和 **hanadb3-hana**) 。  

5. 藉由執行下列步驟，將新建立的虛擬網路介面連結至對應的虛擬機器：  

    a. 移至 [Azure 入口網站](https://portal.azure.com/#home)中的虛擬機器。  

    b. 在左窗格中，選取 [ **虛擬機器**]。 篩選虛擬機器名稱 (例如， **hanadb1**) ，然後選取虛擬機器。  

    c. 在 [ **總覽** ] 窗格中，選取 [ **停止** ] 將虛擬機器解除配置。  

    d. 選取 [ **網路**]，然後連接網路介面。 在 [ **附加網路介面** ] 下拉式清單中，選取已建立 `storage` 和子網的網路介面 `hana` 。  
    
    e. 選取 [儲存]  。 
 
    f. 針對其餘的虛擬機器重複步驟 b 至 e (在我們的範例中為  **hanadb2** 和 **hanadb3**) 。
 
    g. 暫時讓虛擬機器處於停止狀態。 接下來，我們會為所有新連接的網路介面啟用 [加速網路](../../../virtual-network/create-vm-accelerated-networking-cli.md) 。  

6. 藉 `storage` 由執行下列步驟，為和子網的額外網路介面啟用加速網路 `hana` ：  

    a. 開啟[Azure 入口網站](https://portal.azure.com/#home)中的[Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/) 。  

    b. 執行下列命令，以針對附加至 `storage` 和子網的其他網路介面啟用加速網路 `hana` 。  

    <pre><code>
    az network nic update --id /subscriptions/<b>your subscription</b>/resourceGroups/<b>your resource group</b>/providers/Microsoft.Network/networkInterfaces/<b>hanadb1-storage</b> --accelerated-networking true
    az network nic update --id /subscriptions/<b>your subscription</b>/resourceGroups/<b>your resource group</b>/providers/Microsoft.Network/networkInterfaces/<b>hanadb2-storage</b> --accelerated-networking true
    az network nic update --id /subscriptions/<b>your subscription</b>/resourceGroups/<b>your resource group</b>/providers/Microsoft.Network/networkInterfaces/<b>hanadb3-storage</b> --accelerated-networking true
    
    az network nic update --id /subscriptions/<b>your subscription</b>/resourceGroups/<b>your resource group</b>/providers/Microsoft.Network/networkInterfaces/<b>hanadb1-hana</b> --accelerated-networking true
    az network nic update --id /subscriptions/<b>your subscription</b>/resourceGroups/<b>your resource group</b>/providers/Microsoft.Network/networkInterfaces/<b>hanadb2-hana</b> --accelerated-networking true
    az network nic update --id /subscriptions/<b>your subscription</b>/resourceGroups/<b>your resource group</b>/providers/Microsoft.Network/networkInterfaces/<b>hanadb3-hana</b> --accelerated-networking true

    </code></pre>

7. 執行下列步驟來啟動虛擬機器：  

    a. 在左窗格中，選取 [ **虛擬機器**]。 篩選虛擬機器名稱 (例如， **hanadb1**) ，然後選取它。  

    b. 在 [ **總覽** ] 窗格中，選取 [ **啟動**]。  

## <a name="operating-system-configuration-and-preparation"></a>作業系統設定和準備

下一節中的指示前面會加上下列其中一項：
* **[A]**：適用于所有節點
* **[1]**：僅適用于節點1
* **[2]**：僅適用于節點2
* **[3]**：僅適用于節點3

藉由執行下列步驟來設定和準備您的作業系統：

1. **[A]** 維護虛擬機器上的主機檔案。 包含所有子網的專案。 此範例已新增下列專案 `/etc/hosts` 。  

    <pre><code>
    # Storage
    10.23.2.4   hanadb1-storage
    10.23.2.5   hanadb2-storage
    10.23.2.6   hanadb3-storage
    # Client
    10.23.0.5   hanadb1
    10.23.0.6   hanadb2
    10.23.0.7   hanadb3
    # Hana
    10.23.3.4   hanadb1-hana
    10.23.3.5   hanadb2-hana
    10.23.3.6   hanadb3-hana
    </code></pre>

2. **[A]** 針對存放裝置的網路介面變更 DHCP 和雲端設定，以避免發生非預期的主機名稱變更。  

    下列指示假設儲存體網路介面為 `eth1` 。 

    <pre><code>
    vi /etc/sysconfig/network/dhcp
    # Change the following DHCP setting to "no"
    DHCLIENT_SET_HOSTNAME="no"
    vi /etc/sysconfig/network/ifcfg-<b>eth1</b>
    # Edit ifcfg-eth1 
    #Change CLOUD_NETCONFIG_MANAGE='yes' to "no"
    CLOUD_NETCONFIG_MANAGE='no'
    </code></pre>

2. **[A]** 新增網路路由，讓 Azure NetApp Files 的通訊通過儲存體網路介面。  

    下列指示假設儲存體網路介面為 `eth1` 。  

    <pre><code>
    vi /etc/sysconfig/network/ifroute-<b>eth1</b>
    # Add the following routes 
    # RouterIPforStorageNetwork - - -
    # ANFNetwork/cidr RouterIPforStorageNetwork - -
    <b>10.23.2.1</b> - - -
    <b>10.23.1.0/26</b> <b>10.23.2.1</b> - -
    </code></pre>

    重新開機 VM 以啟用變更。  

3. **[A]** 準備 OS 以使用 NFS 在 netapp Systems 上執行 SAP Hana，如 [使用 Azure netapp Files Microsoft Azure 上的 Netapp SAP 應用程式][anf-sap-applications-azure]中所述。 為 NetApp configuration 設定建立設定檔 */etc/sysctl.d/netapp-hana.conf* 。  

    <pre><code>
    vi /etc/sysctl.d/netapp-hana.conf
    # Add the following entries in the configuration file
    net.core.rmem_max = 16777216
    net.core.wmem_max = 16777216
    net.core.rmem_default = 16777216
    net.core.wmem_default = 16777216
    net.core.optmem_max = 16777216
    net.ipv4.tcp_rmem = 65536 16777216 16777216
    net.ipv4.tcp_wmem = 65536 16777216 16777216
    net.core.netdev_max_backlog = 300000
    net.ipv4.tcp_slow_start_after_idle=0
    net.ipv4.tcp_no_metrics_save = 1
    net.ipv4.tcp_moderate_rcvbuf = 1
    net.ipv4.tcp_window_scaling = 1
    net.ipv4.tcp_timestamps = 1
    net.ipv4.tcp_sack = 1
    </code></pre>

4. **[A]** 建立與 Microsoft for Azure 設定 */etc/sysctl.d/ms-az.conf* 的設定檔。  

    <pre><code>
    vi /etc/sysctl.d/ms-az.conf
    # Add the following entries in the configuration file
    ipv6.conf.all.disable_ipv6 = 1
    net.ipv4.tcp_max_syn_backlog = 16348
    net.ipv4.conf.all.rp_filter = 0
    sunrpc.tcp_slot_table_entries = 128
    vm.swappiness=10
    </code></pre>

> [!TIP]
> 避免在 sysctl 設定檔中明確設定 net.ipv4.ip_local_port_range 和 net.ipv4.ip_local_reserved_ports，以允許 SAP 主機代理程式管理埠範圍。 如需詳細資訊，請參閱 SAP 附注 [2382421](https://launchpad.support.sap.com/#/notes/2382421)。  

4. **[A]** [使用 Azure netapp Files 來調整 Microsoft Azure 上的 NetApp SAP 應用程式][anf-sap-applications-azure]中建議的 sunrpc 設定。  

    <pre><code>
    vi /etc/modprobe.d/sunrpc.conf
    # Insert the following line
    options sunrpc tcp_max_slot_table_entries=128
    </code></pre>

## <a name="mount-the-azure-netapp-files-volumes"></a>掛接 Azure NetApp Files 磁片區

1. **[A]** 建立 HANA 資料庫磁片區的掛接點。  

    <pre><code>
    mkdir -p /hana/data/<b>HN1</b>/mnt00001
    mkdir -p /hana/data/<b>HN1</b>/mnt00002
    mkdir -p /hana/log/<b>HN1</b>/mnt00001
    mkdir -p /hana/log/<b>HN1</b>/mnt00002
    mkdir -p /hana/shared
    mkdir -p /usr/sap/<b>HN1</b>
    </code></pre>

2. **[1]** 針對 **HN1** 共用上的/usr/sap 建立節點特定目錄。  

    <pre><code>
    # Create a temporary directory to mount <b>HN1</b>-shared
    mkdir /mnt/tmp
    # if using NFSv3 for this volume, mount with the following command
    mount <b>10.23.1.4</b>:/<b>HN1</b>-shared /mnt/tmp
    # if using NFSv4.1 for this volume, mount with the following command
    mount -t nfs -o sec=sys,vers=4.1 <b>10.23.1.4</b>:/<b>HN1</b>-shared /mnt/tmp
    cd /mnt/tmp
    mkdir shared usr-sap-<b>hanadb1</b> usr-sap-<b>hanadb2</b> usr-sap-<b>hanadb3</b>
    # unmount /hana/shared
    cd
    umount /mnt/tmp
    </code></pre>

3. **[A]** 確認 NFS 網域設定。 確認網域已設為預設 Azure NetApp Files 網域，即將 **`defaultv4iddomain.com`** 和對應設為 **nobody**。  

    > [!IMPORTANT]
    > 確認在 VM 上的 `/etc/idmapd.conf` 內設定 NFS 網域，使其與 Azure NetApp Files 上的預設網域設定相符： **`defaultv4iddomain.com`** 。 若 NFS 用戶端 (即 VM) 上網域設定和 NFS 伺服器的網域設定 (即 Azure NetApp 設定) 不相符，則掛接在 VM 上 Azure NetApp 磁碟區上檔案的權限將會顯示為 `nobody`。  

    <pre><code>
    sudo cat /etc/idmapd.conf
    # Example
    [General]
    Verbosity = 0
    Pipefs-Directory = /var/lib/nfs/rpc_pipefs
    Domain = <b>defaultv4iddomain.com</b>
    [Mapping]
    Nobody-User = <b>nobody</b>
    Nobody-Group = <b>nobody</b>
    </code></pre>

4. **[A]** 驗證 `nfs4_disable_idmapping`。 其應設為 **Y**。若要建立 `nfs4_disable_idmapping` 所在的目錄結構，請執行掛接命令。 您將無法手動在 /sys/modules 下建立目錄，因為其存取已保留給核心/驅動程式。  

    <pre><code>
    # Check nfs4_disable_idmapping 
    cat /sys/module/nfs/parameters/nfs4_disable_idmapping
    # If you need to set nfs4_disable_idmapping to Y
    mkdir /mnt/tmp
    mount 10.23.1.4:/HN1-shared /mnt/tmp
    umount  /mnt/tmp
    echo "Y" > /sys/module/nfs/parameters/nfs4_disable_idmapping
    # Make the configuration permanent
    echo "options nfs nfs4_disable_idmapping=Y" >> /etc/modprobe.d/nfs.conf
    </code></pre>

5. **[A] 以** 手動方式建立 SAP Hana 群組和使用者。 群組 sapsys 和使用者 **hn1** Adm 的識別碼必須設定為登入期間所提供的相同識別碼。  (在此範例中，識別碼會設定為 **1001**) 。如果識別碼設定不正確，您將無法存取磁片區。 在所有虛擬機器上，群組 sapsys 和使用者帳戶 **hn1** adm 和 Sapadm 的識別碼必須相同。  

    <pre><code>
    # Create user group 
    sudo groupadd -g 1001 sapsys
    # Create  users 
    sudo useradd <b>hn1</b>adm -u 1001 -g 1001 -d /usr/sap/<b>HN1</b>/home -c "SAP HANA Database System" -s /bin/sh
    sudo useradd sapadm -u 1002 -g 1001 -d /home/sapadm -c "SAP Local Administrator" -s /bin/sh
    # Set the password  for both user ids
    sudo passwd hn1adm
    sudo passwd sapadm
    </code></pre>

6. **[A]** 掛接共用的 Azure NetApp Files 磁片區。  

    <pre><code>
    sudo vi /etc/fstab
    # Add the following entries
    10.23.1.5:/<b>HN1</b>-data-mnt00001 /hana/data/<b>HN1</b>/mnt00001  nfs   rw,vers=4,minorversion=1,hard,timeo=600,rsize=262144,wsize=262144,intr,noatime,lock,_netdev,sec=sys  0  0
    10.23.1.6:/<b>HN1</b>-data-mnt00002 /hana/data/<b>HN1</b>/mnt00002  nfs   rw,vers=4,minorversion=1,hard,timeo=600,rsize=262144,wsize=262144,intr,noatime,lock,_netdev,sec=sys  0  0
    10.23.1.4:/<b>HN1</b>-log-mnt00001 /hana/log/<b>HN1</b>/mnt00001  nfs   rw,vers=4,minorversion=1,hard,timeo=600,rsize=262144,wsize=262144,intr,noatime,lock,_netdev,sec=sys  0  0
    10.23.1.6:/<b>HN1</b>-log-mnt00002 /hana/log/HN1/mnt00002  nfs   rw,vers=4,minorversion=1,hard,timeo=600,rsize=262144,wsize=262144,intr,noatime,lock,_netdev,sec=sys  0  0
    10.23.1.4:/<b>HN1</b>-shared/shared /hana/shared  nfs   rw,vers=4,minorversion=1,hard,timeo=600,rsize=262144,wsize=262144,intr,noatime,lock,_netdev,sec=sys  0  0
    # Mount all volumes
    sudo mount -a 
    </code></pre>

7. **[1]** 在 **hanadb1** 上掛接節點特定磁片區。  

    <pre><code>
    sudo vi /etc/fstab
    # Add the following entries
    10.23.1.4:/<b>HN1</b>-shared/usr-sap-<b>hanadb1</b> /usr/sap/<b>HN1</b>  nfs   rw,vers=4,minorversion=1,hard,timeo=600,rsize=262144,wsize=262144,intr,noatime,lock,_netdev,sec=sys  0  0
    # Mount the volume
    sudo mount -a 
    </code></pre>

8. **[2]** 在 **hanadb2** 上掛接節點特定磁片區。  

    <pre><code>
    sudo vi /etc/fstab
    # Add the following entries
    10.23.1.4:/<b>HN1</b>-shared/usr-sap-<b>hanadb2</b> /usr/sap/<b>HN1</b>  nfs   rw,vers=4,minorversion=1,hard,timeo=600,rsize=262144,wsize=262144,intr,noatime,lock,_netdev,sec=sys  0  0
    # Mount the volume
    sudo mount -a 
    </code></pre>

9. **[3]** 在 **hanadb3** 上掛接節點特定磁片區。  

    <pre><code>
    sudo vi /etc/fstab
    # Add the following entries
    10.23.1.4:/<b>HN1</b>-shared/usr-sap-<b>hanadb3</b> /usr/sap/<b>HN1</b>  nfs   rw,vers=4,minorversion=1,hard,timeo=600,rsize=262144,wsize=262144,intr,noatime,lock,_netdev,sec=sys  0  0
    # Mount the volume
    sudo mount -a 
    </code></pre>

10. **[A]** 確認所有 HANA 磁片區都是使用 NFS 通訊協定版本 **NFSv4** 掛接。  

    <pre><code>
    sudo nfsstat -m
    # Verify that flag vers is set to <b>4.1</b> 
    # Example from <b>hanadb1</b>
    /hana/data/<b>HN1</b>/mnt00001 from 10.23.1.5:/<b>HN1</b>-data-mnt00001
     Flags: rw,noatime,vers=<b>4.1</b>,rsize=262144,wsize=262144,namlen=255,hard,proto=tcp,timeo=600,retrans=2,sec=sys,clientaddr=10.23.2.4,local_lock=none,addr=10.23.1.5
    /hana/log/<b>HN1</b>/mnt00002 from 10.23.1.6:/<b>HN1</b>-log-mnt00002
     Flags: rw,noatime,vers=<b>4.1</b>,rsize=262144,wsize=262144,namlen=255,hard,proto=tcp,timeo=600,retrans=2,sec=sys,clientaddr=10.23.2.4,local_lock=none,addr=10.23.1.6
    /hana/data/<b>HN1</b>/mnt00002 from 10.23.1.6:/<b>HN1</b>-data-mnt00002
     Flags: rw,noatime,vers=<b>4.1</b>,rsize=262144,wsize=262144,namlen=255,hard,proto=tcp,timeo=600,retrans=2,sec=sys,clientaddr=10.23.2.4,local_lock=none,addr=10.23.1.6
    /hana/log/<b>HN1</b>/mnt00001 from 10.23.1.4:/<b>HN1</b>-log-mnt00001
    Flags: rw,noatime,vers=<b>4.1</b>,rsize=262144,wsize=262144,namlen=255,hard,proto=tcp,timeo=600,retrans=2,sec=sys,clientaddr=10.23.2.4,local_lock=none,addr=10.23.1.4
    /usr/sap/<b>HN1</b> from 10.23.1.4:/<b>HN1</b>-shared/usr-sap-<b>hanadb1</b>
     Flags: rw,noatime,vers=<b>4.1</b>,rsize=262144,wsize=262144,namlen=255,hard,proto=tcp,timeo=600,retrans=2,sec=sys,clientaddr=10.23.2.4,local_lock=none,addr=10.23.1.4
    /hana/shared from 10.23.1.4:/<b>HN1</b>-shared/shared
     Flags: rw,noatime,vers=<b>4.1</b>,rsize=262144,wsize=262144,namlen=255,hard,proto=tcp,timeo=600,retrans=2,sec=sys,clientaddr=10.23.2.4,local_lock=none,addr=10.23.1.4
    </code></pre>

## <a name="installation"></a>安裝  

在此範例中，若要使用 Azure 的待命節點來部署相應放大設定中的 SAP Hana，我們已使用 HANA 2.0 SP4。  

### <a name="prepare-for-hana-installation"></a>準備 HANA 安裝

1. **[A]** 安裝 HANA 之前，請先設定根密碼。 您可以在安裝完成之後停用根密碼。 Execute as `root` 命令 `passwd` 。  

2. **[1]** 確認您可以透過 SSH 登入至 **hanadb2** 和 **hanadb3**，而不會提示您輸入密碼。  

    <pre><code>
    ssh root@<b>hanadb2</b>
    ssh root@<b>hanadb3</b>
    </code></pre>

3. **[A]** 安裝 HANA 2.0 SP4 所需的其他套件。 如需詳細資訊，請參閱 SAP 附注 [2593824](https://launchpad.support.sap.com/#/notes/2593824)。 

    <pre><code>
    sudo zypper install libgcc_s1 libstdc++6 libatomic1 
    </code></pre>

4. **[2]，[3]** 將 SAP Hana 和目錄的擁有權變更 `data` `log` 為 **hn1** adm。   

    <pre><code>
    # Execute as root
    sudo chown hn1adm:sapsys /hana/data/<b>HN1</b>
    sudo chown hn1adm:sapsys /hana/log/<b>HN1</b>
    </code></pre>

### <a name="hana-installation"></a>HANA 安裝

1. **[1]** 依照 [SAP Hana 2.0 安裝和更新指南](https://help.sap.com/viewer/2c1988d620e04368aa4103bf26f17727/2.0.04/en-US/7eb0167eb35e4e2885415205b8383584.html)中的指示來安裝 SAP Hana。 在此範例中，我們會安裝具有 master、一個背景工作角色和一個待命節點的 SAP Hana 向外延展。  

   a. 從 HANA 安裝軟體目錄啟動 **hdblcm** 程式。 使用 `internal_network` 參數並傳遞子網的位址空間（用於內部 HANA 節點間通訊）。  

    <pre><code>
    ./hdblcm --internal_network=10.23.3.0/24
    </code></pre>

   b. 在提示字元中，輸入下列值：

     * 針對 **[選擇動作**]：輸入 **1** (安裝) 
     * 如需 **安裝的其他元件**：輸入 **2、3**
     * 針對安裝路徑：按下 Enter (預設為/hana/shared) 
     * 針對 **本機主機名稱**：按 enter 接受預設值
     * 在 **[您要將主機新增到系統嗎？**： enter **y** ] 底下
     * **若要新增逗點分隔的主機名稱**：輸入 **hanadb2、hanadb3**
     * 針對 **根使用者名稱** [root]：按 enter 以接受預設值
     * 針對 **根使用者密碼**：輸入根使用者的密碼
     * 若為主機 hanadb2 的角色：為背景工作) 輸入 **1**  (
     * 針對主機 hanadb2 的 **主機容錯移轉群組** [default]：按下 enter 以接受預設值
     * 若為主機 hanadb2 的 **存放磁碟分割編號** [<<assign automatically>>]：按下 enter 以接受預設值
     * 若為主機 hanadb2 的背景 **工作角色群組** [default]：按下 enter 以接受預設值
     * 針對 [主機 hanadb3 的 **選取角色** ]：輸入 **2** (待命) 
     * 針對主機 hanadb3 的 **主機容錯移轉群組** [default]：按下 enter 以接受預設值
     * 若為主機 hanadb3 的背景 **工作角色群組** [default]：按下 enter 以接受預設值
     * 針對 **SAP Hana 系統識別碼**：輸入 **HN1**
     * **實例號碼**[00]：輸入 **03**
     * 針對 **本機主機背景工作角色群組** [default]：按下 enter 以接受預設值
     * 針對 **[選取系統使用量/輸入索引 [4]**：輸入 **4** (自訂) 
     * 針對 **資料磁片區的位置** [/hana/data/HN1]：按下 enter 以接受預設值
     * 針對 **記錄磁片區的位置** [/hana/log/HN1]：按下 enter 以接受預設值
     * 針對 **限制最大記憶體配置嗎？** [n]：輸入 **n**
     * 若為 **主機 hanadb1 的憑證主機名稱** [hanadb1]：按下 enter 以接受預設值
     * 若為 **主機 hanadb2 的憑證主機名稱** [hanadb2]：按下 enter 以接受預設值
     * 若為 **主機 hanadb3 的憑證主機名稱** [hanadb3]：按下 enter 以接受預設值
     * **系統管理員 (hn1adm) 密碼**：輸入密碼
     * **系統資料庫使用者 (系統) 密碼**：輸入系統的密碼
     * 若要 **確認系統資料庫使用者 (系統) 密碼**：輸入系統的密碼
     * **電腦重新開機後重新開機系統？** [n]：輸入 **n** 
     * 如果 **您想要繼續 (y/n)**：驗證摘要，如果一切看起來正確，請輸入 **y**


2. **[1]** 確認 global.ini  

   顯示 global.ini，並確定內部 SAP Hana 節點間通訊的設定已就緒。 確認 [ **通訊** ] 區段。 它應該要有子網的位址空間 `hana` ，而且 `listeninterface` 應該設定為 `.internal` 。 確認 **internal_hostname_resolution** 區段。 它應該有屬於子網的 HANA 虛擬機器的 IP 位址 `hana` 。  

   <pre><code>
    sudo cat /usr/sap/<b>HN1</b>/SYS/global/hdb/custom/config/global.ini
    # Example 
    #global.ini last modified 2019-09-10 00:12:45.192808 by hdbnameserve
    [communication]
    internal_network = <b>10.23.3/24</b>
    listeninterface = .internal
    [internal_hostname_resolution]
    <b>10.23.3.4</b> = <b>hanadb1</b>
    <b>10.23.3.5</b> = <b>hanadb2</b>
    <b>10.23.3.6</b> = <b>hanadb3</b>
   </code></pre>

3. **[1]** 新增主機對應，以確保用戶端 IP 位址會用於用戶端通訊。 新增區段 `public_host_resolution` ，並從用戶端子網新增對應的 IP 位址。  

   <pre><code>
    sudo vi /usr/sap/HN1/SYS/global/hdb/custom/config/global.ini
    #Add the section
    [public_hostname_resolution]
    map_<b>hanadb1</b> = <b>10.23.0.5</b>
    map_<b>hanadb2</b> = <b>10.23.0.6</b>
    map_<b>hanadb3</b> = <b>10.23.0.7</b>
   </code></pre>

4. **[1]** 重新開機 SAP Hana 以啟動變更。  

   <pre><code>
    sudo -u <b>hn1</b>adm /usr/sap/hostctrl/exe/sapcontrol -nr <b>03</b> -function StopSystem HDB
    sudo -u <b>hn1</b>adm /usr/sap/hostctrl/exe/sapcontrol -nr <b>03</b> -function StartSystem HDB
   </code></pre>

5. **[1]** 確認用戶端介面將使用子網中的 IP 位址 `client` 進行通訊。  

   <pre><code>
    sudo -u hn1adm /usr/sap/HN1/HDB03/exe/hdbsql -u SYSTEM -p "<b>password</b>" -i 03 -d SYSTEMDB 'select * from SYS.M_HOST_INFORMATION'|grep net_publicname
    # Expected result
    "<b>hanadb3</b>","net_publicname","<b>10.23.0.7</b>"
    "<b>hanadb2</b>","net_publicname","<b>10.23.0.6</b>"
    "<b>hanadb1</b>","net_publicname","<b>10.23.0.5</b>"
   </code></pre>

   如需有關如何驗證設定的詳細資訊，請參閱 SAP 附注 [2183363-SAP Hana 內部網路的](https://launchpad.support.sap.com/#/notes/2183363)設定。  

6. 若要優化基礎 Azure NetApp Files 儲存體的 SAP Hana，請設定下列 SAP Hana 參數：

   - `max_parallel_io_requests`**128**
   - `async_read_submit`**開啟**
   - `async_write_submit_active`**開啟**
   - `async_write_submit_blocks`**全部**

   如需詳細資訊，請參閱 [使用 Azure Netapp Files Microsoft Azure 上的 NETAPP SAP 應用程式][anf-sap-applications-azure]。 

   從 SAP Hana 2.0 系統開始，您可以在中設定參數 `global.ini` 。 如需詳細資訊，請參閱 SAP 附注 [1999930](https://launchpad.support.sap.com/#/notes/1999930)。  
   
   針對 SAP Hana 1.0 系統版本 SPS12 和更早版本，您可以在安裝期間設定這些參數，如 SAP Note [2267798](https://launchpad.support.sap.com/#/notes/2267798)中所述。  

7. Azure NetApp Files 使用的儲存體，其檔案大小限制為 16 tb (TB) 。 SAP Hana 不會隱含地察覺儲存體限制，而且在達到 16 TB 的檔案大小限制時，不會自動建立新的資料檔案。 當 SAP Hana 嘗試成長超過 16 TB 的檔案時，該嘗試會導致錯誤，最後會在索引伺服器損毀時產生錯誤。 

   > [!IMPORTANT]
   > 若要防止 SAP Hana 在超過儲存子系統的 [16 TB 限制](../../../azure-netapp-files/azure-netapp-files-resource-limits.md) 的情況下增加資料檔案，請在中設定下列參數 `global.ini` 。  
   > - datavolume_striping = true
   > - datavolume_striping_size_gb = 15000 如需詳細資訊，請參閱 SAP Note [2400005](https://launchpad.support.sap.com/#/notes/2400005)。
   > 請注意 SAP Note [2631285](https://launchpad.support.sap.com/#/notes/2631285)。 

## <a name="test-sap-hana-failover"></a>測試 SAP Hana 容錯移轉 

> [!NOTE]
> 本文包含「 *主要* 」和「 *從屬*」條款的參考，也就是 Microsoft 不再使用的條款。 從軟體移除這些條款之後，我們會將其從本文中移除。

1. 模擬 SAP Hana 背景工作節點上的節點損毀。 執行下列動作： 

   a. 在模擬節點損毀之前，請執行下列命令作為 **hn1** adm 來捕捉環境的狀態：  

   <pre><code>
    # Check the landscape status
    python /usr/sap/<b>HN1</b>/HDB<b>03</b>/exe/python_support/landscapeHostConfiguration.py
    | Host    | Host   | Host   | Failover | Remove | Storage   | Storage   | Failover | Failover | NameServer | NameServer | IndexServer | IndexServer | Host    | Host    | Worker  | Worker  |
    |         | Active | Status | Status   | Status | Config    | Actual    | Config   | Actual   | Config     | Actual     | Config      | Actual      | Config  | Actual  | Config  | Actual  |
    |         |        |        |          |        | Partition | Partition | Group    | Group    | Role       | Role       | Role        | Role        | Roles   | Roles   | Groups  | Groups  |
    | ------- | ------ | ------ | -------- | ------ | --------- | --------- | -------- | -------- | ---------- | ---------- | ----------- | ----------- | ------- | ------- | ------- | ------- |
    | hanadb1 | yes    | ok     |          |        |         1 |         1 | default  | default  | master 1   | master     | worker      | master      | worker  | worker  | default | default |
    | hanadb2 | yes    | ok     |          |        |         2 |         2 | default  | default  | master 2   | slave      | worker      | slave       | worker  | worker  | default | default |
    | hanadb3 | yes    | ignore |          |        |         0 |         0 | default  | default  | master 3   | slave      | standby     | standby     | standby | standby | default | -       |
    # Check the instance status
    sapcontrol -nr <b>03</b>  -function GetSystemInstanceList
    GetSystemInstanceList
    OK
    hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
    hanadb2, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb1, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb3, 3, 50313, 50314, 0.3, HDB|HDB_STANDBY, GREEN
   </code></pre>

   b. 若要模擬節點損毀，請在背景工作節點上以 root 身份執行下列命令（在此案例中為 **hanadb2** ）：  
   
   <pre><code>
    echo b > /proc/sysrq-trigger
   </code></pre>

   c. 監視系統的容錯移轉是否完成。 當容錯移轉完成時，請捕捉狀態，看起來應該如下所示：  

    <pre><code>
    # Check the instance status
    sapcontrol -nr <b>03</b>  -function GetSystemInstanceList
    GetSystemInstanceList
    OK
    hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
    hanadb1, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb3, 3, 50313, 50314, 0.3, HDB|HDB_STANDBY, GREEN
    hanadb2, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GRAY
    # Check the landscape status
    /usr/sap/HN1/HDB03/exe/python_support> python landscapeHostConfiguration.py
    | Host    | Host   | Host   | Failover | Remove | Storage   | Storage   | Failover | Failover | NameServer | NameServer | IndexServer | IndexServer | Host    | Host    | Worker  | Worker  |
    |         | Active | Status | Status   | Status | Config    | Actual    | Config   | Actual   | Config     | Actual     | Config      | Actual      | Config  | Actual  | Config  | Actual  |
    |         |        |        |          |        | Partition | Partition | Group    | Group    | Role       | Role       | Role        | Role        | Roles   | Roles   | Groups  | Groups  |
    | ------- | ------ | ------ | -------- | ------ | --------- | --------- | -------- | -------- | ---------- | ---------- | ----------- | ----------- | ------- | ------- | ------- | ------- |
    | hanadb1 | yes    | ok     |          |        |         1 |         1 | default  | default  | master 1   | master     | worker      | master      | worker  | worker  | default | default |
    | hanadb2 | no     | info   |          |        |         2 |         0 | default  | default  | master 2   | slave      | worker      | standby     | worker  | standby | default | -       |
    | hanadb3 | yes    | info   |          |        |         0 |         2 | default  | default  | master 3   | slave      | standby     | slave       | standby | worker  | default | default |
   </code></pre>

   > [!IMPORTANT]
   > 當節點發生核心異常時，請 `kernel.panic` 在 *所有* HANA 虛擬機器上設定為20秒，以避免延遲 SAP Hana 容錯移轉。 設定是在中完成 `/etc/sysctl` 。 重新開機虛擬機器以啟用變更。 如果未執行這項變更，當節點發生核心異常時，容錯移轉可能需要10或更長的時間。  

2. 執行下列動作來終止名稱伺服器：

   a. 在測試之前，請執行下列命令作為 **hn1** adm 來檢查環境的狀態：  

   <pre><code>
    #Landscape status 
    python /usr/sap/HN1/HDB03/exe/python_support/landscapeHostConfiguration.py
    | Host    | Host   | Host   | Failover | Remove | Storage   | Storage   | Failover | Failover | NameServer | NameServer | IndexServer | IndexServer | Host    | Host    | Worker | Worker  |
    |         | Active | Status | Status   | Status | Config    | Actual    | Config   | Actual   | Config     | Actual     | Config      | Actual      | Config  | Actual  | Config  | Actual  |
    |         |        |        |          |        | Partition | Partition | Group    | Group    | Role       | Role       | Role        | Role        | Roles   | Roles   | Groups  | Groups  |
    | ------- | ------ | ------ | -------- | ------ | --------- | --------- | -------- | -------- | ---------- | ---------- | ----------- | ----------- | ------- | ------- | ------- | ------- |
    | hanadb1 | yes    | ok     |          |        |         1 |         1 | default  | default  | master 1   | master     | worker      | master      | worker  | worker  | default | default |
    | hanadb2 | yes    | ok     |          |        |         2 |         2 | default  | default  | master 2   | slave      | worker      | slave       | worker  | worker  | default | default |
    | hanadb3 | no     | ignore |          |        |         0 |         0 | default  | default  | master 3   | slave      | standby     | standby     | standby | standby | default | -       |
    # Check the instance status
    sapcontrol -nr 03  -function GetSystemInstanceList
    GetSystemInstanceList
    OK
    hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
    hanadb2, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb1, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb3, 3, 50313, 50314, 0.3, HDB|HDB_STANDBY, GRAY
   </code></pre>

   b. 在使用中的主要節點上，以 **hn1** adm 的形式執行下列命令（在此案例中為 **hanadb1** ）：  

    <pre><code>
        hn1adm@hanadb1:/usr/sap/HN1/HDB03> HDB kill
    </code></pre>
    
    待命節點 **hanadb3** 將接管為主要節點。 以下是容錯移轉測試完成後的資源狀態：  

    <pre><code>
        # Check the instance status
        sapcontrol -nr 03 -function GetSystemInstanceList
        GetSystemInstanceList
        OK
        hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
        hanadb2, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
        hanadb1, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GRAY
        hanadb3, 3, 50313, 50314, 0.3, HDB|HDB_STANDBY, GREEN
        # Check the landscape status
        python /usr/sap/HN1/HDB03/exe/python_support/landscapeHostConfiguration.py
        | Host    | Host   | Host   | Failover | Remove | Storage   | Storage   | Failover | Failover | NameServer | NameServer | IndexServer | IndexServer | Host    | Host    | Worker  | Worker  |
        |         | Active | Status | Status   | Status | Config    | Actual    | Config   | Actual   | Config     | Actual     | Config      | Actual      | Config  | Actual  | Config  | Actual  |
        |         |        |        |          |        | Partition | Partition | Group    | Group    | Role       | Role       | Role        | Role        | Roles   | Roles   | Groups  | Groups  |
        | ------- | ------ | ------ | -------- | ------ | --------- | --------- | -------- | -------- | ---------- | ---------- | ----------- | ----------- | ------- | ------- | ------- | ------- |
        | hanadb1 | no     | info   |          |        |         1 |         0 | default  | default  | master 1   | slave      | worker      | standby     | worker  | standby | default | -       |
        | hanadb2 | yes    | ok     |          |        |         2 |         2 | default  | default  | master 2   | slave      | worker      | slave       | worker  | worker  | default | default |
        | hanadb3 | yes    | info   |          |        |         0 |         1 | default  | default  | master 3   | master     | standby     | master      | standby | worker  | default | default |
    </code></pre>

   c. 重新開機 **hanadb1** (上的 HANA 實例，也就是在名稱伺服器已終止) 的相同虛擬機器上。 **Hanadb1** 節點將會重新加入環境，並保留其待命角色。  

   <pre><code>
    hn1adm@hanadb1:/usr/sap/HN1/HDB03> HDB start
   </code></pre>

   在 **hanadb1** 上啟動 SAP Hana 之後，預期會有下列狀態：  

   <pre><code>
    # Check the instance status
    sapcontrol -nr 03 -function GetSystemInstanceList
    GetSystemInstanceList
    OK
    hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
    hanadb1, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb2, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb3, 3, 50313, 50314, 0.3, HDB|HDB_STANDBY, GREEN
    # Check the landscape status
    python /usr/sap/HN1/HDB03/exe/python_support/landscapeHostConfiguration.py
    | Host    | Host   | Host   | Failover | Remove | Storage   | Storage   | Failover | Failover | NameServer | NameServer | IndexServer | IndexServer | Host    | Host    | Worker  | Worker  |
    |         | Active | Status | Status   | Status | Config    | Actual    | Config   | Actual   | Config     | Actual     | Config      | Actual      | Config  | Actual  | Config  | Actual  |
    |         |        |        |          |        | Partition | Partition | Group    | Group    | Role       | Role       | Role        | Role        | Roles   | Roles   | Groups  | Groups  |
    | ------- | ------ | ------ | -------- | ------ | --------- | --------- | -------- | -------- | ---------- | ---------- | ----------- | ----------- | ------- | ------- | ------- | ------- |
    | hanadb1 | yes    | info   |          |        |         1 |         0 | default  | default  | master 1   | slave      | worker      | standby     | worker  | standby | default | -       |
    | hanadb2 | yes    | ok     |          |        |         2 |         2 | default  | default  | master 2   | slave      | worker      | slave       | worker  | worker  | default | default |
    | hanadb3 | yes    | info   |          |        |         0 |         1 | default  | default  | master 3   | master     | standby     | master      | standby | worker  | default | default |
   </code></pre>

   d. 同樣地，請在目前作用中的主要節點上終止名稱伺服器 (也就是在節點 **hanadb3**) 上。  
   
   <pre><code>
    hn1adm@hanadb3:/usr/sap/HN1/HDB03> HDB kill
   </code></pre>

   節點 **hanadb1** 將會繼續主要節點的角色。 完成容錯移轉測試之後，狀態會如下所示：

   <pre><code>
    # Check the instance status
    sapcontrol -nr 03  -function GetSystemInstanceList & python /usr/sap/HN1/HDB03/exe/python_support/landscapeHostConfiguration.py
    GetSystemInstanceList
    OK
    hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
    GetSystemInstanceList
    OK
    hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
    hanadb1, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb2, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb3, 3, 50313, 50314, 0.3, HDB|HDB_STANDBY, GRAY
    # Check the landscape status
    python /usr/sap/HN1/HDB03/exe/python_support/landscapeHostConfiguration.py
    | Host    | Host   | Host   | Failover | Remove | Storage   | Storage   | Failover | Failover | NameServer | NameServer | IndexServer | IndexServer | Host    | Host    | Worker  | Worker  |
    |         | Active | Status | Status   | Status | Config    | Actual    | Config   | Actual   | Config     | Actual     | Config      | Actual      | Config  | Actual  | Config  | Actual  |
    |         |        |        |          |        | Partition | Partition | Group    | Group    | Role       | Role       | Role        | Role        | Roles   | Roles   | Groups  | Groups  |
    | ------- | ------ | ------ | -------- | ------ | --------- | --------- | -------- | -------- | ---------- | ---------- | ----------- | ----------- | ------- | ------- | ------- | ------- |
    | hanadb1 | yes    | ok     |          |        |         1 |         1 | default  | default  | master 1   | master     | worker      | master      | worker  | worker  | default | default |
    | hanadb2 | yes    | ok     |          |        |         2 |         2 | default  | default  | master 2   | slave      | worker      | slave       | worker  | worker  | default | default |
    | hanadb3 | no     | ignore |          |        |         0 |         0 | default  | default  | master 3   | slave      | standby     | standby     | standby | standby | default | -       |
   </code></pre>

   e. 在 **hanadb3** 上啟動 SAP Hana，這將準備好作為待命節點。  

   <pre><code>
    hn1adm@hanadb3:/usr/sap/HN1/HDB03> HDB start
   </code></pre>

   在 **hanadb3** 上啟動 SAP Hana 之後，狀態看起來會像下面這樣：  

   <pre><code>
    # Check the instance status
    sapcontrol -nr 03  -function GetSystemInstanceList & python /usr/sap/HN1/HDB03/exe/python_support/landscapeHostConfiguration.py
    GetSystemInstanceList
    OK
    hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
    GetSystemInstanceList
    OK
    hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
    hanadb1, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb2, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb3, 3, 50313, 50314, 0.3, HDB|HDB_STANDBY, GRAY
    # Check the landscape status
    python /usr/sap/HN1/HDB03/exe/python_support/landscapeHostConfiguration.py
    | Host    | Host   | Host   | Failover | Remove | Storage   | Storage   | Failover | Failover | NameServer | NameServer | IndexServer | IndexServer | Host    | Host    | Worker  | Worker  |
    |         | Active | Status | Status   | Status | Config    | Actual    | Config   | Actual   | Config     | Actual     | Config      | Actual      | Config  | Actual  | Config  | Actual  |
    |         |        |        |          |        | Partition | Partition | Group    | Group    | Role       | Role       | Role        | Role        | Roles   | Roles   | Groups  | Groups  |
    | ------- | ------ | ------ | -------- | ------ | --------- | --------- | -------- | -------- | ---------- | ---------- | ----------- | ----------- | ------- | ------- | ------- | ------- |
    | hanadb1 | yes    | ok     |          |        |         1 |         1 | default  | default  | master 1   | master     | worker      | master      | worker  | worker  | default | default |
    | hanadb2 | yes    | ok     |          |        |         2 |         2 | default  | default  | master 2   | slave      | worker      | slave       | worker  | worker  | default | default |
    | hanadb3 | no     | ignore |          |        |         0 |         0 | default  | default  | master 3   | slave      | standby     | standby     | standby | standby | default | -       |
   </code></pre>

## <a name="next-steps"></a>後續步驟

* [適用於 SAP 的 Azure 虛擬機器規劃和實作][planning-guide]
* [適用於 SAP 的 Azure 虛擬機器部署][deployment-guide]
* [適用於 SAP 的 Azure 虛擬機器 DBMS 部署][dbms-guide]
* 若要瞭解如何為 Azure Vm 上的 SAP Hana 建立高可用性和規劃以進行嚴重損壞修復，請參閱 [Azure 虛擬機器 (vm) 上 SAP Hana 的高可用性 ][sap-hana-ha]。
