---
title: 使用 Azure 自動化監看員工作追蹤更新的檔案
description: 本文說明如何在 Azure 自動化帳戶中建立監看員工作，以監看在資料夾中建立的新檔案。
services: automation
ms.subservice: process-automation
ms.topic: conceptual
ms.date: 12/17/2020
ms.openlocfilehash: ca4c4013e0044811a5bac8786996761b8a5c45f1
ms.sourcegitcommit: e0ec3c06206ebd79195d12009fd21349de4a995d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/18/2020
ms.locfileid: "97682754"
---
# <a name="track-updated-files-with-a-watcher-task"></a>使用監看員工作追蹤更新的檔案

Azure 自動化會使用監看員工作搭配 PowerShell Runbook 來尋找事件和觸發動作。 監看員工作包含兩個部分：監看員和動作。 監看員 Runbook 會在監看員工作中定義的間隔執行，並將資料輸出到動作 Runbook。

> [!NOTE]
> Azure China 21Vianet 中不支援監看員工作。

> [!IMPORTANT]
> 從5月2020開始，使用 Azure Logic Apps 是監視事件、排程週期性工作和觸發動作的建議和支援方式。 [使用 Azure Logic Apps 排定和執行週期性自動化工作、程序和工作流程](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md)。

本文將逐步引導您建立監看員工作，以在新檔案新增至目錄時進行監視。 您會了解如何：

* 匯入監看員 Runbook
* 建立自動化變數
* 建立動作 Runbook
* 建立監看員工作
* 觸發監看員
* 檢查輸出

## <a name="prerequisites"></a>Prerequisites

若要完成這篇文章，需要下列專案：

* Azure 訂用帳戶。 如果您沒有這類帳戶，可以[啟用自己的 MSDN 訂戶權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)或註冊[免費帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。
* [自動化帳戶](./index.yml)以保存監看員和動作 Runbook，以及監看員工作。
* 執行監看員工作的[混合式 Runbook 背景工作角色](automation-hybrid-runbook-worker.md)。
* PowerShell Runbook。 監看員工作不支援 PowerShell 工作流程 Runbook。

## <a name="import-a-watcher-runbook"></a>匯入監看員 Runbook

本文使用稱為 **Watch-NewFile 的監** 看員 runbook 來尋找目錄中的新檔案。 監看員 Runbook 會擷取資料夾檔案的最後已知寫入時間，並查看比該浮水印還新的任何檔案。

您可以從 [Azure 自動化 GitHub 組織](https://github.com/azureautomation)下載 runbook。

1. 流覽至 [Watch-NewFile.ps1](https://github.com/azureautomation/watcher-action-that-processes-events-triggerd-by-a-watcher-runbook)的 Azure 自動化 GitHub 組織頁面。
2. 若要從 GitHub 下載 runbook，請選取頁面右側的 [程式 **代碼** ]，然後選取 [ **下載 zip** ] 以下載 zip 檔案中的完整程式碼。
3. 將內容解壓縮並匯 [入 runbook](manage-runbooks.md#import-a-runbook-from-the-azure-portal)。

您也可以使用下列步驟，從入口網站將此 runbook 匯入您的自動化帳戶。

1. 開啟自動化帳戶，然後按一下 [Runbook] 頁面。
2. 按一下 **[流覽資源庫]** ，然後在 [ **來源** ] 下拉式清單中選取 [ **GitHub**]。
3. 搜尋 [監看員 Runbook]、選取 [尋找目錄中新檔案的監看員 Runbook]，然後按一下 [匯入]。
4. 提供 Runbook 的名稱和描述，然後按一下 [確定]，以將 Runbook 匯入到自動化帳戶。
5. 選取 [編輯]，然後按一下 [發佈]。 出現提示時，按一下 [是] 以發佈 Runbook。

## <a name="create-an-automation-variable"></a>建立自動化變數

[Automation 變數](./shared-resources/variables.md)可用來儲存先前 runbook 從每個檔案讀取和儲存的時間戳記。

1. 選取 [共用資源] 底下的 [變數]，然後按一下 [+ 新增變數]。
1. 輸入 **Watch-NewFileTimestamp** 作為名稱。
1. 選取 [日期時間] 作為類型。
1. 按一下 [建立]  來建立自動化變數。

## <a name="create-an-action-runbook"></a>建立動作 Runbook

在監看員工作中，動作 Runbook 會用來處理從監看員 Runbook 傳來的資料。 您必須從 [Azure 自動化 GitHub 組織](https://github.com/azureautomation)匯入名為 **Process-NewFile** 的預先定義動作 runbook。

若要建立動作 Runbook：

1. 流覽至 [Process-NewFile.ps1](https://github.com/azureautomation/watcher-action-that-processes-events-triggerd-by-a-watcher-runbook)的 Azure 自動化 GitHub 組織頁面。
2. 若要從 GitHub 下載 runbook，請選取頁面右側的 [程式 **代碼** ]，然後選取 [ **下載 zip** ] 以下載 zip 檔案中的完整程式碼。
3. 將內容解壓縮並匯 [入 runbook](manage-runbooks.md#import-a-runbook-from-the-azure-portal)。

您也可以從 Azure 入口網站將此 Runbook 匯入到您的自動化帳戶：

1. 瀏覽至自動化帳戶，然後選取 [程序自動化] 下的 [Runbook]。
1. 按一下 **[流覽資源庫]** ，然後在 [ **來源** ] 下拉式清單中選取 [ **GitHub**]。
1. 搜尋 [監看員動作]、選取 [處理監看員 Runbook 所觸發事件的監看員動作]，然後按一下 [匯入]。
1. 提供 Runbook 的名稱和描述，然後按一下 [確定]，以將 Runbook 匯入到自動化帳戶。
1. 選取 [編輯]，然後按一下 [發佈]。 出現提示時，按一下 [是] 以發佈 Runbook。

## <a name="create-a-watcher-task"></a>建立監看員工作

在此步驟中，您要將監看員工作設定為參考前面章節中所定義的監看員和動作 Runbook。

1. 瀏覽至自動化帳戶，然後選取 [程序自動化] 下的 [監看員工作]。
1. 選取 [監看員工作] 頁面，然後按一下 [+ 新增監看員工作]。
1. 輸入 **WatchMyFolder** 作為名稱。

1. 選取 [設定監看員]，然後選擇 [Watch-NewFile] Runbook。

1. 為各個參數輸入下列值︰

   * **FOLDERPATH** - 混合式背景工作角色上的資料夾，新檔案建立於其中，例如，**d:\examplefiles**。
   * **EXTENSION** - 組態的副檔名。 保留空白以處理所有副檔名。
   * **RECURSE** - 遞迴作業。 將此值保持為預設值。
   * **RUN SETTINGS** - 用於執行 Runbook 的設定。 挑選混合式背景工作角色。

1. 按一下 [確定]，然後按一下 [選取] 以返回 [監看員] 頁面。
1. 選取 [設定動作]，然後選擇 [Process-NewFile] Runbook。
1. 為各個參數輸入下列值︰

   * **EVENTDATA** - 事件資料。 保留空白。 資料會從監看員 Runbook 傳入。
   * **Run Settings**-用於執行 Runbook 的設定。 保留為 Azure，因為此 Runbook 是在 Azure 自動化中執行。

1. 按一下 [確定]，然後按一下 [選取] 以返回 [監看員] 頁面。
1. 按一下 [確定] 以建立監看員工作。

    ![從 UI 設定監看員動作](media/automation-watchers-tutorial/watchertaskcreation.png)

## <a name="trigger-a-watcher"></a>觸發監看員

您必須執行如下所述的測試，以確保監看員工作會如預期般運作。 

1. 遠端連線至混合式 Runbook 背景工作角色。
2. 開啟 **PowerShell** 並在資料夾中建立測試檔案。

```azurepowerShell-interactive
New-Item -Name ExampleFile1.txt
```

下列範例會顯示預期的輸出。

```output
    Directory: D:\examplefiles


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----       12/11/2017   9:05 PM              0 ExampleFile1.txt
```

## <a name="inspect-the-output"></a>檢查輸出

1. 瀏覽至自動化帳戶，然後選取 [程序自動化] 下的 [監看員工作]。
1. 選取 **WatchMyFolder** 監看員工作。
1. 按一下 [串流] 底下的 [檢視監看員串流]，確認監看員已找到新檔案並已啟動動作 Runbook。
1. 若要查看動作 Runbook 作業，請按一下 [檢視監看員動作作業]。 您可以選取每個作業來檢視作業的詳細資料。

   ![UI 中的監看員動作作業](media/automation-watchers-tutorial/WatcherActionJobs.png)

您可以在下列範例中看到系統找到新檔案時的預期輸出：

```output
Message is Process new file...

Passed in data is @{FileName=D:\examplefiles\ExampleFile1.txt; Length=0}
```

## <a name="next-steps"></a>後續步驟

若要深入瞭解如何撰寫您自己的 runbook，請參閱 [建立 PowerShell runbook](learn/automation-tutorial-runbook-textual-powershell.md)。
