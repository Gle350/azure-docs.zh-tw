---
title: Azure VM 上的 SQL Server 備份與還原 | Microsoft Docs
description: 描述 Azure 虛擬機器上執行的 SQL Server 資料庫之備份和還原考量。
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
editor: ''
tags: azure-resource-management
ms.assetid: 95a89072-0edf-49b5-88ed-584891c0e066
ms.service: virtual-machines-sql
ms.subservice: backup
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/04/2018
ms.author: mikeray
ms.openlocfilehash: 2fcba81bcd20db321d791fcda589f40fb0699702
ms.sourcegitcommit: a89a517622a3886b3a44ed42839d41a301c786e0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/22/2020
ms.locfileid: "97733068"
---
# <a name="backup-and-restore-for-sql-server-on-azure-vms"></a>Azure VM 上的 SQL Server 備份與還原
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

本文針對在 Azure 的 Windows 虛擬機器 (VM) 上執行的 SQL Server，提供可用備份和還原選項的指引。 Azure 儲存體會針對每個 Azure VM 磁碟維護三個複本，以取得針對資料遺失或實體資料損毀的保護。 因此，您並不需要和 SQL Server 內部部署一樣地過度擔心發生硬體失敗的情形。 不過，您仍應該備份 SQL Server 資料庫，以取得針對應用程式或使用者錯誤的保護，例如意外的資料插入或刪除。 在此情況下，具備還原到特定時間點的能力是非常重要的。

本文的第一個部分會提供備份和還原可用選項的概觀。 後續各節則會針對每一種策略提供詳細資訊。

## <a name="backup-and-restore-options"></a>備份與還原選項

下表提供 Azure VM 上 SQL Server 的各種備份與還原選項資訊：

| 策略 | SQL 版本 | 描述 |
|---|---|---|
| [自動備份](#automated) | 2014<br/> 2016<br/> 2017 | 自動備份可讓您為 SQL Server VM 上的所有資料庫排定定期備份。 備份會儲存在 Azure 儲存體長達 30 天的時間。 從 SQL Server 2016 開始，自動備份 v2 會提供其他選項，例如設定手動排程及完整和記錄備份的頻率。 |
| [SQL VM 的 Azure 備份](#azbackup) | 2008<br/> 2012<br/> 2014<br/> 2016<br/> 2017<br/> 2019 | Azure 備份針對 Azure VM 上的 SQL Server 提供企業級備份功能。 您可以透過此服務，集中管理多部伺服器和數千個資料庫的備份。 資料庫可在入口網站中還原至特定時間點。 它能提供可自訂的保留原則，並能維護備份長達數年的時間。 |
| [手動備份](#manual) | 全部 | 依據 SQL Server 版本的不同，有各種技術可手動備份和還原 Azure VM 上的 SQL Server。 在此案例中，您必須負責處理資料庫的備份方式與儲存位置，並管理這些備份。 |

下列各節會更詳細地說明每一種選項。 本文的最後一節會以功能矩陣的形式提供摘要。

## <a name="automated-backup"></a><a id="automated"></a> 自動備份

自動備份可為在 Azure 的 Windows VM 上執行的 SQL Server Standard 與 Enterprise 版提供自動備份服務。 此服務是由 [SQL Server IaaS 代理程式延伸模組](sql-server-iaas-agent-extension-automate-management.md)提供的，並會自動在 Azure 入口網站中安裝於 SQL Server Windows 虛擬機器映像上。

所有資料庫都會備份到您所設定的 Azure 儲存體帳戶。 備份可以被加密，並保留長達 30 天的時間。

SQL Server 2016 和更新版本的 VM，會透過自動備份 v2 提供更多自訂選項。 這些改善功能包括︰

- 系統資料庫備份
- 手動備份排程與時間範圍
- 完整和記錄檔備份頻率

若要還原資料庫，您必須在儲存體帳戶中找到所需的備份檔案，並使用 SQL Server Management Studio (SSMS) 或 Transact-SQL 命令在 SQL VM 上執行還原。

如需如何針對 SQL VM 設定自動備份的詳細資訊，請參閱下列其中一篇文章：

- **SQL Server 2016/2017**：[Azure 虛擬機器的自動備份 v2](automated-backup.md)
- **SQL Server 2014**：[SQL Server 2014 虛擬機器的自動備份](automated-backup-sql-2014.md)

## <a name="azure-backup-for-sql-vms"></a><a id="azbackup"></a> SQL VM 的 Azure 備份

[Azure 備份](../../../backup/index.yml)針對 Azure VM 上的 SQL Server 提供企業級備份功能。 所有備份都會儲存在復原服務保存庫儲存中，並在那裡進行管理。 此解決方案能提供數項特別適用於企業的優點：

- **零基礎架構備份**：您不必管理備份伺服器或儲存位置。
- **調整**：可保護許多 SQL VM 與數以千計的資料庫。
- **隨用隨付**：這項功能是由 Azure 備份提供的個別服務，但和所有 Azure 服務一樣，您只需依據使用量付費。
- **集中管理和監視**：從 Azure 中的單一儀表板集中管理所有備份，包括 Azure 備份支援的其他工作負載。
- **原則驅動的備份和保留**：針對定期備份建立標準備份原則。 建立保留原則以維護備份長達數年的時間。
- **支援 SQL Always On**：偵測和保護 SQL Server Always On 設定，並接受備份的可用性群組備份喜好設定。
- **15 分鐘復原點目標 (RPO)** ：設定最多以 15 分鐘為間隔的 SQL 交易記錄備份。
- **還原時間點**：使用入口網站將資料庫復原至特定時間點，而不必手動還原多個完整、差異和記錄備份。
- **彙總的失敗電子郵件警示**：設定針對任何失敗的彙總電子郵件通知。
- **Azure 角色型存取控制**：決定可透過入口網站管理備份和還原作業的人員。

如需其運作方式及示範的快速概觀，請觀看下列影片：

> [!VIDEO https://www.youtube.com/embed/wmbANpHos_E]

適用於 SQL VM 的這個 Azure 備份解決方案已正式推出。 如需詳細資訊，請參閱[將 SQL Server 資料庫備份至 Azure](../../../backup/backup-azure-sql-database.md)。

## <a name="manual-backup"></a><a id="manual"></a> 手動備份

如果您想要在 SQL VM 上手動管理備份和還原作業，根據使用的 SQL Server 版本而定，會有數個選項可供您使用。 如需備份和還原的概觀，請根據您的 SQL Server 版本參閱下列其中一篇文章：

- [SQL Server 2016 和更新版本的備份與還原](/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases)
- [SQL Server 2014 的備份與還原](/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases?viewFallbackFrom=sql-server-2014)
- [SQL Server 2012 的備份與還原](/previous-versions/sql/sql-server-2012/ms187048(v=sql.110))
- [SQL Server 2008 R2 的備份與還原](/previous-versions/sql/sql-server-2008-r2/ms187048(v=sql.105))
- [SQL Server 2008 的備份和還原](/previous-versions/sql/sql-server-2008/ms187048(v=sql.100))

下列各節將詳細說明數個手動備份和還原選項。

### <a name="backup-to-attached-disks"></a>備份至已連接磁碟

針對 Azure VM 上的 SQL Server，您可以使用原生備份和還原技術，使用 VM 上的已連接磁碟作為備份檔案的目的地。 不過，根據該 [虛擬機器的大小](../../../virtual-machines/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)而定，您可以連接到 Azure 虛擬機器的磁碟數有所限制。 磁碟管理的負擔也需要加以考量。

如需如何使用 Server Management Studio (SSMS) 或 Transact-SQL 手動建立完整資料庫備份的範例，請參閱[建立完整資料庫備份](/sql/relational-databases/backup-restore/create-a-full-database-backup-sql-server)。

### <a name="backup-to-url"></a>備份至 URL

從 SQL Server 2012 SP1 CU2 開始，您可以直接備份和還原至 Microsoft Azure Blob 儲存體，此功能也稱為「備份至 URL」。 SQL Server 2016 也針對此功能導入下列增強功能：

| 2016 增強功能 | 詳細資料 |
| --- | --- |
| **串接** |當備份至 Microsoft Azure Blob 儲存體時，SQL Server 2016 支援備份至多個 Blob，以啟用可高達 12.8 TB 之大型資料庫的備份。 |
| **快照集備份** |藉由 Azure 快照集，SQL Server 檔案快照集備份對使用 Azure Blob 儲存體服務儲存的資料庫檔案，提供幾乎即時的備份及快速還原。 這項功能可簡化備份和還原原則。 檔案快照集備份也支援還原時間點。 如需詳細資訊，請參閱 [適用於在 Azure 中的資料庫檔案的快照集備份](/sql/relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure)。 |

如需詳細資訊，請根據您的 SQL Server 版本參閱下列其中一篇文章：

- **SQL Server 2016/2017**：[SQL Server 備份至 URL](/sql/relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service)
- **SQL Server 2014**：[SQL Server 2014 備份至 URL](/sql/relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service?viewFallbackFrom=sql-server-2014)
- **SQL Server 2012**：[SQL Server 2012 備份至 URL](/previous-versions/sql/sql-server-2012/jj919148(v=sql.110))

### <a name="managed-backup"></a>受控備份

從 SQL Server 2014 開始，受控備份會將針對 Azure 儲存體建立備份的程序自動化。 受控備份會在幕後利用於本文先前小節所述的「備份至 URL」功能。 受控備份也是支援 SQL Server VM 自動備份服務的基礎功能。

從 SQL Server 2016 開始，受控備份會提供適用於排程、系統資料庫備份，以及完整與記錄備份頻率的其他選項。

如需詳細資訊，請根據您的 SQL Server 版本參閱下列其中一篇文章：

- [適用於 SQL Server 2016 和更新版本的 Managed Backup to Microsoft Azure](/sql/relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure)
- [適用於 SQL Server 2014 的 Managed Backup to Microsoft Azure](/sql/relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure?viewFallbackFrom=sql-server-2014)

## <a name="decision-matrix"></a>決策矩陣

下表會摘要說明適用於 Azure 中 SQL Server 虛擬機器的每個備份和還原選項功能。

| 選項 | 自動備份 | 適用於 SQL 的 Azure 備份 | 手動備份 |
|---|---|---|---|
| 需要其他 Azure 服務 |   | ![綠色核取記號。](./media/backup-restore/yes.png) |   |
| 在 Azure 入口網站中設定備份原則 | ![綠色核取記號。](./media/backup-restore/yes.png) | ![綠色核取記號。](./media/backup-restore/yes.png) |   |
| 在 Azure 入口網站中還原資料庫 |   | ![綠色核取記號。](./media/backup-restore/yes.png) |   |
| 於單一儀表板中管理多部伺服器 |   | ![綠色核取記號。](./media/backup-restore/yes.png) |   |
| 時間點還原 | ![綠色核取記號。](./media/backup-restore/yes.png) | ![綠色核取記號。](./media/backup-restore/yes.png) | ![綠色核取記號。](./media/backup-restore/yes.png) |
| 15 分鐘復原點目標 (RPO) | ![綠色核取記號。](./media/backup-restore/yes.png) | ![綠色核取記號。](./media/backup-restore/yes.png) | ![綠色核取記號。](./media/backup-restore/yes.png) |
| 短期備份保留原則 (天) | ![綠色核取記號。](./media/backup-restore/yes.png) | ![綠色核取記號。](./media/backup-restore/yes.png) |   |
| 長期備份保留原則 (月、年) |   | ![綠色核取記號。](./media/backup-restore/yes.png) |   |
| 針對 SQL Server Always On 的內建支援 |   | ![綠色核取記號。](./media/backup-restore/yes.png) |   |
| 備份至 Azure 儲存體帳戶 | ![綠色核取記號。](./media/backup-restore/yes.png)(自動) | ![綠色核取記號。](./media/backup-restore/yes.png)(自動) | ![綠色核取記號。](./media/backup-restore/yes.png)(由客戶管理) |
| 儲存體與備份檔案的管理 | | ![綠色核取記號。](./media/backup-restore/yes.png) |  |
| 備份至 VM 上的已連接磁碟 |   |   | ![綠色核取記號。](./media/backup-restore/yes.png) |
| 可集中自訂的備份報告 |   | ![綠色核取記號。](./media/backup-restore/yes.png) |   |
| 彙總的失敗電子郵件警示 |   | ![綠色核取記號。](./media/backup-restore/yes.png) |   |
| 根據 Azure 監視器記錄來自訂監視 |   | ![綠色核取記號。](./media/backup-restore/yes.png) |   |
| 使用 SSMS 或 Transact-SQL 指令碼監視備份作業 | ![綠色核取記號。](./media/backup-restore/yes.png) | ![綠色核取記號。](./media/backup-restore/yes.png) | ![綠色核取記號。](./media/backup-restore/yes.png) |
| 使用 SSMS 或 Transact-SQL 指令碼還原資料庫 | ![綠色核取記號。](./media/backup-restore/yes.png) |   | ![綠色核取記號。](./media/backup-restore/yes.png) |

## <a name="next-steps"></a>後續步驟

如果您正計畫要在 Azure VM 上部署 SQL Server，則可以在下列指南中找到佈建指引：[如何在 Azure 入口網站中佈建 Windows SQL Server 虛擬機器](create-sql-vm-portal.md)。

雖然備份和還原可用來將資料移轉，但對於移轉到 VM 上的 SQL Server，可能仍有更容易的資料移轉路徑。 如需移轉選項和建議的完整討論，請參閱[將資料庫移轉至 Azure VM 上的 SQL Server](migrate-to-vm-from-sql-server.md)。
