---
title: 監視私用雲端活動
titleSuffix: Azure VMware Solution by CloudSimple
description: 描述 Azure VMware Solution by CloudSimple 環境中活動可用的資訊，包括警示、事件、工作和審核。
author: Ajayan1008
ms.author: v-hborys
ms.date: 08/13/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: fb597cc71d228a1daaaa4e52d76b301764ff9292
ms.sourcegitcommit: d7d5f0da1dda786bda0260cf43bd4716e5bda08b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/05/2021
ms.locfileid: "97899298"
---
# <a name="monitor-vmware-solution-by-cloudsimple-activity"></a>依 CloudSimple 活動監視 VMware 解決方案

CloudSimple 活動記錄可讓您深入瞭解在 CloudSimple 入口網站上完成的作業。  此清單包含警示、事件、工作和審核。  使用活動記錄來判斷誰、何時及哪些作業已執行。  活動記錄不包含使用者所執行的任何讀取作業。

## <a name="sign-in-to-azure"></a>登入 Azure

登入 Azure 入口網站：[https://portal.azure.com](https://portal.azure.com)。

## <a name="access-the-cloudsimple-portal"></a>存取 CloudSimple 入口網站

存取 [CloudSimple 入口網站](access-cloudsimple-portal.md)。

## <a name="activity-information"></a>活動資訊

若要存取活動頁面，請選取側邊功能表上的 [ **活動** ]。

![活動頁面總覽](media/activity-page-overview.png)

若要查看活動頁面上任何活動的詳細資料，請選取活動。 [詳細資料] 面板會在右側開啟。 面板中的動作取決於活動的類型。 按一下 [ **X** ] 關閉面板。

按一下欄標題以排序顯示。  您可以篩選要查看的特定值的資料行。  按一下 [ **下載為 CSV** ] 圖示，以下載活動報告。

## <a name="alerts"></a>警示

警示是 CloudSimple 環境中任何重大活動的通知。  警示包含會影響帳單或使用者存取權的事件。

若要確認警示並將其從清單中移除，請從清單中選取一或多個警示，然後按一下 [ **認可**]。

下列資訊資料行適用于警示。 按一下 [ **編輯資料行** ]，然後選取您想要查看的資料行。

| Column | 描述 |
------------ | ------------- |
| 警示類型 | 警示的類別。|
| Time | 警示發生的時間。 |
| 嚴重性 | 警示的重要性。|
| 資源名稱 | 指派給資源的名稱，例如私用雲端名稱。 |
| 資源類型 | 資源類別：私用雲端、雲端機架。 |
| 資源識別碼 | 資源的識別碼。 |
| 描述 | 觸發警示的描述。 |
| 已認可 | 指出是否已確認警示。 |

## <a name="events"></a>事件

事件會在 CloudSimple 入口網站上顯示使用者和系統活動。 [事件] 頁面會列出與特定資源相關聯的活動，以及影響的嚴重性。

下列資訊資料行適用于警示。 按一下 [ **編輯資料行** ]，然後選取您想要查看的資料行。

| Column | 描述 |
------------ | ------------- |
| Time | 事件發生的日期和時間。 |
| 事件類型 | 識別事件的數值代碼。 |
| 嚴重性 | 事件嚴重性。|
| 資源名稱 | 指派給資源的名稱，例如私用雲端名稱。 |
| 資源類型 | 資源類別：私用雲端、雲端機架。 |
| 描述 | 觸發警示的描述。 |

## <a name="tasks"></a>工作

工作是需要30秒或更多時間才能完成的私用雲端活動。 預期需要少於30秒的 (活動只會回報為事件。 ) 開啟 [工作] 頁面，即可追蹤私用雲端的工作進度。

下列資訊資料行適用于警示。 按一下 [ **編輯資料行** ]，然後選取您想要查看的資料行。

| Column | 描述 |
------------ | ------------- |
| 工作識別碼 | 工作的唯一識別碼。 |
| 作業 | 工作執行的動作。 |
| User | 使用者已指派完成工作。 |
| 資源名稱 | 指派給資源的名稱。 |
| 資源類型 | 資源類別：私用雲端、雲端機架。 |
| 資源識別碼 | 資源的識別碼。 |
| 開始 | 工作的開始時間。 |
| 結束 | 工作的結束時間。 |
| 狀態 | 目前的工作狀態。 |
| 經過時間 | 完成工作所需的時間 (（如果已完成）) 或正在進行中) 時正在進行 (。 |
| 描述 | 工作描述。 |

## <a name="audit"></a>稽核

Audit 記錄檔會追蹤使用者活動。 您可以使用 audit 記錄來監視所有使用者的使用者活動。

下列資訊資料行適用于警示。 按一下 [ **編輯資料行** ]，然後選取您想要查看的資料行。

| Column | 描述 |
------------ | ------------- |
| Time | Audit 專案的時間。 |
| 作業 | 工作執行的動作。 |
| User | 指派給工作的使用者。 |
| 資源名稱 | 指派給資源的名稱。 |
| 資源類型 | 資源類別：私用雲端、雲端機架。 |
| 資源識別碼 | 資源的識別碼。 |
| 結果 | 活動的結果，例如 **Success**。 |
| 花費的時間 | 完成工作的時間。 |
| 描述 | 動作的描述。 |

## <a name="next-steps"></a>後續步驟

* [在 Azure 上取用 VMware VM](quickstart-create-vmware-virtual-machine.md)
* 深入了解[私人雲端](cloudsimple-private-cloud.md)
