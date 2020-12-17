---
title: 檔案層級的 SAP HANA Azure 備份 | Microsoft Docs
description: 備份 Azure 虛擬機器上的 SAP HANA 有兩種主要做法，本文涵蓋檔案層級的 SAP HANA Azure 備份
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: juergent
editor: ''
ms.service: virtual-machines-linux
ms.subservice: workloads
ms.topic: article
ums.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/01/2020
ms.author: juergent
ms.openlocfilehash: 70b0f8178a94735a6ef37a225044984508cc2233
ms.sourcegitcommit: 86acfdc2020e44d121d498f0b1013c4c3903d3f3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/17/2020
ms.locfileid: "97617131"
---
# <a name="sap-hana-azure-backup-on-file-level"></a>檔案層級的 SAP HANA Azure 備份

## <a name="introduction"></a>簡介

本文是 [Azure 虛擬機器上 SAP Hana 備份指南](./sap-hana-backup-guide.md)的相關文章，其中提供有關如何開始使用的總覽和資訊，以及有關 Azure 備份服務和儲存體快照集的詳細資訊。 

Azure 中的不同 VM 類型允許連接不同數目的 VHD。 確切的詳細資料記載於 [Azure 中的 Linux 虛擬機器大小](../../sizes.md)。 針對本檔中所參考的測試，我們使用了 GS5 的 Azure VM，以允許64連接的資料磁片。 在較大型的 SAP HANA 系統中，資料和記錄檔可能已佔據大量磁碟，可能還加上用於最佳磁碟 IO 輸送量的軟體等量。 如需 Azure Vm 上 SAP Hana 部署建議磁片設定的詳細資訊，請參閱 [SAP Hana azure 虛擬機器儲存體](./hana-vm-operations-storage.md)設定的文章。 所提供的建議也包括本機備份的磁碟空間建議。

在檔案層級管理備份/還原的標準方式，是透過 SAP HANA Studio 或 SAP HANA SQL 陳述式使用以檔案為基礎的備份。 如需詳細資訊，請參閱 [SQL 和系統檢視參考的 SAP Hana](https://help.sap.com/viewer/4fe29514fd584807ac9f2a04f6754767/2.0.05/en-US/3859e48180bb4cf8a207e15cf25a7e57.html)文章。

![本圖顯示 SAP HANA Studio 中的備份的功能表對話方塊](media/sap-hana-backup-file-level/backup-menue-dialog.png)

本圖顯示 SAP HANA Studio 中的備份的功能表對話方塊。 當選擇 [檔案]&quot;&quot; 類型，必須指定檔案系統中的路徑讓 SAP HANA 寫入備份檔案。 還原的運作方式亦同。

雖然這項選擇聽起來簡單又直接，但仍有一些事要考量。 Azure VM 會限制可連接的資料磁片數目。 可能無法在 VM 的檔案系統上儲存 SAP HANA 備份檔案，取決於資料庫的大小和磁碟輸送量需求，其中可能牽涉跨多個資料磁碟的軟體等量。 在本文稍後提供多種選項，可用於在處理數 TB 資料時，移動這些備份檔案以及管理檔案大小限制和效能。

另一個可以不計總容量提供更多自由的選項是 Azure Blob 儲存體。 雖然單一 blob 也有 1 TB 的限制，單一 blob 容器的總容量目前為 500 TB。 此外，它讓客戶可以選擇較具成本效益的「非經常性」&quot;&quot;blob 儲存體。 如需非經常性 Blob 儲存體的詳細資訊，請參閱 [Azure Blob 儲存體：經常性存取層、非經常性存取層和封存存取層](../../../storage/blobs/storage-blob-storage-tiers.md?tabs=azure-portal) 。

如果想要更安全，使用異地複寫儲存體帳戶來存放 SAP HANA 備份。 如需儲存體冗余和儲存體複寫的詳細資訊，請參閱 [Azure 儲存體冗余](../../../storage/common/storage-redundancy.md) 。

一個人將 SAP HANA 備份專用的 VHD 放在異地複寫的專用備份儲存體帳戶。 而另一個人可以將存放 SAP HANA 備份的 VHD 複製到異地複寫的儲存體帳戶中，或複製到不同區域中的儲存體帳戶。

## <a name="azure-blobxfer-utility-details"></a>Azure blobxfer 公用程式詳細資料

為了在 Azure 儲存體上儲存目錄和檔案，可以使用 CLI 或 PowerShell，或開發使用其中一種 [Azure SDK](https://azure.microsoft.com/downloads/) 的工具。 另外還有一個可供使用的公用程式 AzCopy，可將資料複製到 Azure 儲存體。  (參閱 [使用 AzCopy Command-Line 公用程式傳送資料](../../../storage/common/storage-use-azcopy-v10.md)) 。

因此，使用 Blobxfer 來複製 SAP HANA 備份檔案。 blobxfer 是開放原始碼，可從 [GitHub](https://github.com/Azure/blobxfer) 取得，許多客戶在生產環境中使用它。 此工具不允許將資料直接複製到 Azure blob 儲存體或 Azure 檔案共用。 它也提供許多有用的功能，例如 md5 雜湊，或複製具有多個檔案的目錄時的自動平行處理原則。

## <a name="sap-hana-backup-performance"></a>SAP HANA 備份的效能
本章將討論效能考慮。 所達到的數位可能不代表最新狀態，因為有穩定的開發，可為 Azure 儲存體達到更佳的輸送量。 因此，您應該針對您的設定和 Azure 區域執行個別測試。

![這個螢幕擷取畫面是 SAP HANA Studio 中的 SAP HANA 備份主控台](media/sap-hana-backup-file-level/backup-console-hana-studio.png)

此螢幕擷取畫面顯示 SAP Hana Studio 的 SAP Hana 備份主控台。 在一個磁片上使用 XFS 檔案系統，在連接到 HANA VM 的單一 Azure 標準 HDD 儲存體磁片上，執行 230 GB 的備份大約需要42分鐘。

![這個螢幕擷取畫面是 SAP HANA 測試 VM 上的 YaST](media/sap-hana-backup-file-level/one-backup-disk.png)

這個螢幕擷取畫面是 SAP HANA 測試 VM 上的 YaST。 您可以看到 SAP Hana 備份的 1 TB 單一磁片。 備份 230 GB 花費約 42 分鐘。 此外，還連結了五個 200 GB 的磁碟並建立了軟體 RAID md0，而且在這五個 Azure 資料磁碟上串接。

![對軟體 RAID 重複相同的備份，並串接五個連結的 Azure 標準儲存體資料磁碟](media/sap-hana-backup-file-level/five-backup-disks.png)

對軟體 RAID 重複相同的備份，並串接五個連結的 Azure 標準儲存體資料磁碟，將備份時間從 42 分鐘降到 10 分鐘。 磁碟已連結，且沒有快取至 VM。 此練習示範磁片寫入輸送量的重要性，以達到良好的備份時間。 您可以切換到 Azure 標準 SSD 儲存體或 Azure 進階儲存體，進一步加速處理以獲得最佳效能。 一般而言，不建議使用 Azure 標準 HDD 儲存體，而且僅供示範之用。 建議您在生產系統中使用 Azure 標準 SSD 儲存體或 Azure 進階儲存體的最小值。

## <a name="copy-sap-hana-backup-files-to-azure-blob-storage"></a>將 SAP HANA 備份檔案複製到 Azure Blob 儲存體
提及的效能數位、備份持續時間和複製持續時間數位可能不代表 Azure 技術的最新狀態。 Microsoft 會穩定地改善 Azure 儲存體，以提供更多的輸送量和較低的延遲。 因此，數位僅供示範之用。 您需要在您選擇的 Azure 區域中測試個人需求，才能使用方法來判斷是否最適合您。

快速儲存 SAP HANA 備份檔案的另一個選項是 Azure Blob 儲存體。 一個單一 blob 容器的限制 SAP Hana 為大約 500 TB，可使用 M32ts、M32ls、M64ls 和 GS5 VM 類型的 Azure，以保留足夠的 SAP Hana 備份。 客戶可以選擇經常性存取 &quot; 和非經常性 &quot; &quot; &quot; 存取 blob 儲存體 (請參閱 [Azure blob 儲存體：經常性存取、非經常性存取層和封存存取層](../../../storage/blobs/storage-blob-storage-tiers.md?tabs=azure-portal)) 。

使用 blobxfer 工具，將 SAP HANA 備份檔案直接複製到 Azure Blob 儲存體很容易。

![在這裡可以看到完整的 SAP HANA 檔案備份的檔案](media/sap-hana-backup-file-level/list-of-backups.png)

您可以看到完整 SAP Hana 檔案備份的檔案。 在四個檔案中，最大的一個是大約 230 GB 的大小。

![將 230 GB 複製至 Azure 的標準儲存體帳戶 blob 容器花了大約 3000 秒](media/sap-hana-backup-file-level/copy-duration-blobxfer.png)

在初始測試中沒有使用 md5 雜湊，將 230 GB 複製至 Azure 的標準儲存體帳戶 blob 容器花了大約 3000 秒。

HANA Studio 備份主控台可讓您限制 HANA 備份檔案的檔案大小上限。 在範例環境中，它會藉由使用多個較小的備份檔案，而不是一個大型 230 GB 的檔案，來改善效能。

設定 HANA 端的備份檔案大小限制，並不&#39;可改善備份時間，因為檔案會依序寫入。 檔案大小上限設定為 60 GB，因此備份會建立四個大型資料檔案，而不是 230 GB 的單一檔案。 如果您的備份目標對 blob 大小的檔案大小有所限制，則使用多個備份檔案可能成為備份 HANA 資料庫的必要條件。

![為了測試 blobxfer 工具的平行處理原則，而將 HANA 備份的檔案大小上限設為 15 GB](media/sap-hana-backup-file-level/parallel-copy-multiple-backup-files.png)

為了測試 blobxfer 工具的平行處理原則，而將 HANA 備份的檔案大小上限設為 15 GB，得到 19 個備份檔案。 如此設定，將 blobxfer 複製 230 GB 至 Azure Blob 儲存體的時間，從 3000 秒降至 875 秒 。

當您探索將對本機磁片執行的備份複製到其他位置（例如 Azure blob 儲存體）時，請記住，最終平行複製程式所使用的頻寬是針對個別 VM 類型的網路或儲存體配額進行計算。 因此，您需要在 VM 中執行的一般工作負載，將複製的持續時間與網路和儲存頻寬進行平衡。 


## <a name="copy-sap-hana-backup-files-to-nfs-share"></a>將 SAP HANA 備份檔案複製到 NFS 共用

Microsoft Azure 透過 [Azure NetApp Files](https://azure.microsoft.com/services/netapp/)提供原生 NFS 共用。 您可以在容量中建立數十 Tb 的不同磁片區，以儲存及管理備份。 您也可以根據 NetApp 的技術來建立這些磁片區的快照集。 Azure NetApp Files (ANF) 提供三種不同的服務層級，以提供不同的儲存體輸送量。 如需詳細資訊，請參閱 [Azure NetApp Files 的文章服務層級](../../../azure-netapp-files/azure-netapp-files-service-levels.md)。 您可以依照文章 [快速入門：設定 Azure NetApp Files 並建立 nfs 磁片](../../../azure-netapp-files/azure-netapp-files-quickstart-set-up-account-create-volumes.md?tabs=azure-portal)區中所述，從 ANF 建立和掛接 nfs 磁片區。

除了使用 Azure 的原生 NFS 磁片區透過 ANF，在 Azure 上建立自己的部署以提供 NFS 共用的可能性有很多種。 全都具有您自行部署及管理這些解決方案所需的缺點。 這些可能的部分可能會記載于下列文章中：

- [SUSE Linux Enterprise Server 上 Azure VM 的 NFS 高可用性](./high-availability-guide-suse-nfs.md)
- [Red Hat Enterprise Linux for SAP NetWeaver 上適用於 Azure VM 的 GlusterFS](./high-availability-guide-rhel-glusterfs.md)

以上述方式建立的 NFS 共用可以用來直接執行 HANA 備份，或將對本機磁片執行的備份複製到這些 NFS 共用。

> [!NOTE]
> SAP Hana 支援 NFS v3 和 NFS v4. x。 不支援任何其他格式（如 SMB 與 CIFS 檔案系統）來撰寫 HANA 備份。 另請參閱 [SAP 支援附注 #1820529](https://launchpad.support.sap.com/#/notes/1820529)

## <a name="copy-sap-hana-backup-files-to-azure-files"></a>將 SAP HANA 備份檔案複製到 Azure 檔案

可以將 Azure 檔案共用掛接在 Azure Linux VM 內部。 如何搭配 [Linux 使用 Azure 檔案儲存體](../../../storage/files/storage-how-to-use-files-linux.md) 的文章會提供如何執行設定的詳細資料。 如需 Azure 檔案儲存體或 Azure premium 檔案的限制，請參閱 Azure 檔案儲存體的擴充 [性和效能目標](../../../storage/files/storage-files-scale-targets.md)一文。

> [!NOTE]
> SAP Hana 寫入 HANA 備份時，不支援 SMB 與 CIFS 檔案系統。 另請參閱 [SAP 支援附注 #1820529](https://launchpad.support.sap.com/#/notes/1820529)。 如此一來，您就只能使用此解決方案作為 HANA 資料庫備份的最終目的地，此備份是直接針對本機連接的磁片進行的
> 

在針對 Azure 檔案儲存體（而非 Azure Premium 檔案）進行的測試中，將19個備份檔案複製到 230 GB 的整體磁片區大約需要929秒。 我們預期時間使用 Azure Premium 檔案的方式更好。 不過，您必須記住，您需要以工作負載在網路頻寬上的需求來平衡快速複製的興趣。 因為每個 Azure VM 類型都會強制執行網路頻寬配額，所以您必須在工作負載加上備份檔案複本的情況下，維持在該配額範圍內。

![本圖顯示複製 19 個 SAP HANA 備份檔案需要約 929 秒](media/sap-hana-backup-file-level/parallel-copy-to-azure-files.png)


將 SAP Hana 備份檔案儲存在 Azure 檔案儲存體上，可能是個有趣的選項。 尤其是改善 Azure Premium 檔案的延遲和輸送量。

## <a name="next-steps"></a>後續步驟
* [Azure 虛擬機器上 SAP HANA 的備份指南](sap-hana-backup-guide.md)提供快速入門的概觀和詳細資訊。
* 若要瞭解如何在 Azure (大型實例) 上建立高可用性並規劃 SAP Hana 的災難復原，請參閱 [SAP Hana (大型實例) 高可用性和 azure 上的嚴重損壞修復](hana-overview-high-availability-disaster-recovery.md)。
