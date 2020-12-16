---
title: 在 Azure 自動化中使用原始檔控制整合 - 舊版
description: 本文說明如何使用原始檔控制整合。
services: automation
ms.subservice: process-automation
ms.date: 12/04/2019
ms.topic: conceptual
ms.openlocfilehash: 4dedbcf58e76b8c969f8607db6922e87a85f08e5
ms.sourcegitcommit: d2d1c90ec5218b93abb80b8f3ed49dcf4327f7f4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/16/2020
ms.locfileid: "97591868"
---
# <a name="use-source-control-integration-in-azure-automation---legacy"></a>在 Azure 自動化中使用原始檔控制整合 - 舊版

> [!NOTE]
> 原始檔控制有新的體驗。 若要深入了解新的體驗，請參閱[原始檔控制 (預覽)](source-control-integration.md)。

原始檔控制整合可讓您將自動化帳戶中的 Runbook 關聯至 GitHub 原始檔控制儲存機制。 原始檔控制可讓您輕鬆地與小組共同作業、追蹤變更，以及回復至舊版的 Runbook。 例如，原始檔控制可讓您將原始檔控制中的不同分支同步處理至您的開發、測試或生產自動化帳戶，以輕鬆地將已在開發環境中測試過的程式碼提升至生產自動化帳戶。

原始檔控制可讓您從 Azure 自動化推送程式碼至原始檔控制，或將 Runbook 從原始檔控制提取至 Azure 自動化。 本文說明如何在 Azure 自動化環境中設定原始檔控制。 我們會先設定 Azure 自動化存取 GitHub 存放庫，並逐步解說可使用原始檔控制整合完成的不同作業。 

> [!NOTE]
> 原始檔控制支援提取和推送 [PowerShell 工作流程 Runbook](automation-runbook-types.md#powershell-workflow-runbooks) 以及 [PowerShell Runbook](automation-runbook-types.md#powershell-runbooks)。 尚未支援[圖形化 Runbook](automation-runbook-types.md#graphical-runbooks)。

## <a name="configure-source-control"></a>設定原始檔控制

為您的自動化帳戶設定原始檔控制時，必須執行兩個簡單的步驟，而如果您已經有 GitHub 帳戶，則只需要執行一個步驟。 

### <a name="create-a-github-repository"></a>建立 GitHub 存放庫

如果您已經有想要連結到 Azure 自動化的 GitHub 帳戶和存放庫，則登入您現有的帳戶並從下面的步驟 2 開始執行。 否則，瀏覽至 [GitHub](https://github.com/)，註冊新的帳戶以及[建立新的儲存機制](https://help.github.com/articles/create-a-repo/)。

### <a name="set-up-source-control"></a>設定原始檔控制

1. 在 Azure 入口網站的 [自動化帳戶] 頁面中，於 [帳戶設定] 下按一下 [原始檔控制]。

2. [原始檔控制] 頁面隨即開啟，您可以在其中設定您的 GitHub 帳戶詳細資料。 以下是要設定的參數清單：  

   | **參數** | **說明** |
   |:--- |:--- |
   | 選擇原始檔 |選取原始檔。 目前只支援 **GitHub** 。 |
   | 授權 |按一下 [授權]  按鈕，授與 GitHub 儲存機制的 Azure 自動化存取權。 如果您已在不同的視窗中登入您的 GitHub 帳戶，則會使用該帳戶的認證。 成功授權之後，頁面會在 [授權屬性] 之下顯示您的 GitHub 使用者名稱。 |
   | 選擇儲存機制 |從可用的儲存機制清單中選取 GitHub 儲存機制。 |
   | 選擇分支 |從可用的分支清單中選取分支。 如果您尚未建立任何分支，則只會顯示 **主要** 分支。 |
   | Runbook 資料夾路徑 |Runbook 資料夾路徑可指定 GitHub 儲存機制中的路徑，以便您從中推送或提取程式碼。 它必須以 **/foldername/subfoldername** 格式輸入。 只有 Runbook 資料夾路徑中的 Runbook 會同步處理至您的自動化帳戶。 Runbook 資料夾路徑之子資料夾中的 Runbook **不會** 進行同步處理。 使用 **/** 來同步處理儲存機制下的所有 Runbook。 |
3. 例如，如果您有名為 **PowerShellScripts** 的儲存機制，其中包含名為 **RootFolder** 的資料夾，而該資料夾內含名為 **SubFolder** 的資料夾。 您可以使用下列字串來同步處理每個資料夾層級：

   1. 若要從 **存放庫** 同步處理 Runbook，則 Runbook 資料夾路徑為 **/** 。
   2. 若要從 **RootFolder** 同步處理 Runbook，則 Runbook 資料夾路徑為 **/RootFolder**。
   3. 若要從 **SubFolder** 同步處理 Runbook，則 Runbook 資料夾路徑為 **/RootFolder/SubFolder**。
4. 設定參數之後，其會顯示在 [設定原始檔控制] 頁面。  

    ![顯示設定的原始檔控制頁面](media/source-control-integration-legacy/automation-SourceControlConfigure.png)
5. 按一下 [確定] 後，原始檔控制整合現已針對您的自動化帳戶設定，而且應以您的 GitHub 資訊進行更新。 您現在可以按一下此部分來檢視所有原始檔控制同步處理工作歷程記錄。  

    ![目前已設定的原始檔控制設定所擁有的值](media/source-control-integration-legacy/automation-RepoValues.png)
6. 在設定原始檔控制後，系統會在您的自動化帳戶中建立兩個[變數資產](./shared-resources/variables.md)。 此外，您的 GitHub 帳戶中也會新增已授權的應用程式。

   * **Microsoft.Azure.Automation.SourceControl.Connection** 變數包含連接字串的值，如下所示。  

     | **參數** | **ReplTest1** |
     |:--- |:--- |
     | `Name`  |Microsoft.Azure.Automation.SourceControl.Connection |
     | `Type`  |String |
     | `Value` |{"Branch"： \<*Your branch name*> ，"RunbookFolderPath"： \<*Runbook folder path*> ，"ProviderType"： \<*has a value 1 for GitHub*> ，"Repository"： \<*Name of your repository*> ，"Username"： \<*Your GitHub user name*> } |

   * **Microsoft.Azure.Automation.SourceControl.OauthToken** 變數包含 OAuthToken 的安全加密值。  

     |**參數**            |**ReplTest1** |
     |:---|:---|
     | `Name`  | `Microsoft.Azure.Automation.SourceControl.OAuthToken` |
     | `Type`  | `Unknown(Encrypted)` |
     | `Value` | <`Encrypted OAuthToken`> |  

     ![顯示原始檔控制變數的視窗](media/source-control-integration-legacy/automation-Variables.png)  

   * **自動化原始檔控制** 已做為已授權的應用程式加入至您的 GitHub 帳戶。 若要檢視應用程式，請從 GitHub 首頁瀏覽至 [設定檔] > [設定] > [應用程式]。 此應用程式可讓 Azure 自動化將 GitHub 儲存機制同步至自動化帳戶。  

     ![GitHub 中的應用程式設定](media/source-control-integration-legacy/automation-GitApplication.png)

## <a name="use-source-control-in-automation"></a>在自動化中使用原始檔控制

Runbook 簽入可讓您將對 Azure 自動化中的 Runbook 所做的變更推送至原始檔控制存放庫。 以下是簽入 Runbook 的步驟：

1. 從您的自動化帳戶，[建立新的文字 Runbook](./learn/automation-tutorial-runbook-textual.md)，或[編輯現有的文字 Runbook](automation-edit-textual-runbook.md)。 此 Runbook 可以是 PowerShell 工作流程或 PowerShell 指令碼 Runbook。  
2. 編輯 Runbook 之後，加以儲存並按一下編輯頁面上的 [簽入]。  

    ![顯示 [簽入至 GitHub] 按鈕的視窗](media/source-control-integration-legacy/automation-CheckinButton.png)

     > [!NOTE] 
     > 從 Azure 自動化簽入會覆寫原始檔控制中目前存在的程式碼。 要簽入的 Git 對等命令列指示為 **git add + git commit + git push**  

3. 當您按一下 [簽入] 時，將會出現一個確認訊息，按一下 [是] 繼續進行。  

    ![確認簽入至原始檔控制的對話方塊](media/source-control-integration-legacy/automation-CheckinMessage.png)
4. 簽入會啟動原始檔控制 Runbook：**Sync-MicrosoftAzureAutomationAccountToGitHubV1**。 此 Runbook 會連接到 GitHub 並將 Azure 自動化中的變更推送至您的儲存機制。 若要檢視簽入工作歷程記錄，請回到 [原始檔控制整合] 索引標籤，按一下以開啟 [存放庫同步處理] 分頁。 此頁面會顯示所有的原始檔控制作業。 選取您要檢視的工作並按一下以檢視詳細資料。  

    ![顯示同步作業結果的視窗](media/source-control-integration-legacy/automation-CheckinRunbook.png)

   > [!NOTE]
   > 原始檔控制 Runbook 是特殊的自動化 Runbook，無法檢視或編輯。 雖然它們不會出現在 Runbook 清單上，但您會看到同步處理作業顯示在您的工作清單。

5. 修改過的 Runbook 名稱會當作輸入參數傳送至簽入 Runbook。 在 [存放庫同步處理] 頁面中展開 Runbook，即可[檢視作業詳細資料](automation-runbook-execution.md#job-statuses)。  

    ![顯示同步作業輸入的視窗](media/source-control-integration-legacy/automation-CheckinInput.png)
6. 在工作完成時重新整理您的 GitHub 儲存機制，即可檢視變更。  您的存放庫中應有一項認可，其認可訊息為：**已在 Azure 自動化中更新 Runbook 名稱。**  

### <a name="sync-runbooks-from-source-control-to-azure-automation"></a>將原始檔控制中的 Runbook 同步處理至 Azure 自動化

[存放庫同步處理] 分頁上的 [同步處理] 按鈕可讓您將存放庫的 Runbook 資料夾路徑中的所有 Runbook 提取至您的自動化帳戶。 相同的儲存機制可以同步處理至多個自動化帳戶。 以下是同步處理 Runbook 的步驟：

1. 從您設定原始檔控制的自動化帳戶，開啟 [原始檔控制整合/存放庫同步處理] 頁面，然後按一下 [同步處理]。系統會出現一個確認訊息提示您，按一下 [是] 繼續進行。  

    ![[同步處理] 按鈕，內有訊息以供確認將會同步處理所有 Runbook](media/source-control-integration-legacy/automation-SyncButtonwithMessage.png)

2. 同步作業會啟動 **Sync-MicrosoftAzureAutomationAccountFromGitHubV1** Runbook，這會連線到 GitHub，並從您的存放庫將變更提取至 Azure 自動化。 您應該會在此動作的 [存放庫同步處理] 頁面看到新作業。 若要檢視同步處理工作的詳細資料，按一下以開啟作業詳細資料分頁。  

    ![顯示 GitHub 存放庫同步作業結果的視窗](media/source-control-integration-legacy/automation-SyncRunbook.png)

    > [!NOTE]
    > 從原始檔控制進行的同步處理會針對目前在原始檔控制中的 **所有** Runbook，覆寫目前存在於您的自動化帳戶中的 Runbook 草稿版本。 要同步處理的 Git 對等命令列指示為 **git pull**

![顯示已暫停原始檔控制同步作業中所有記錄的視窗](media/source-control-integration-legacy/automation-AllLogs.png)

## <a name="disconnect-source-control"></a>中斷原始檔控制連線

若要中斷與 GitHub 帳戶的連線，請開啟 [存放庫同步處理] 分頁，然後按一下 [中斷連線]。 一旦中斷與原始檔控制的連線，先前同步處理的 Runbook 仍會保留在您的自動化帳戶中，但不會啟用 [存放庫同步處理] 分頁。  

  ![顯示 [中斷連線] 按鈕以供中斷原始檔控制的視窗](media/source-control-integration-legacy/automation-Disconnect.png)

## <a name="next-steps"></a>後續步驟

* 若要整合 Azure 自動化中的原始檔控制，請參閱 [Azure 自動化：Azure 自動化中的原始檔控制整合](https://azure.microsoft.com/blog/azure-automation-source-control-13/)。  
* 若要將 Runbook 原始檔控制與 Visual Studio Online 整合，請參閱 [Azure 自動化：使用 Visual Studio Online 整合 Runbook 原始檔控制](https://azure.microsoft.com/blog/azure-automation-integrating-runbook-source-control-using-visual-studio-online/)。  
