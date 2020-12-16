---
title: 使用 Git 共同作業撰寫程式碼 - Team Data Science Process
description: 如何使用 Git 搭配敏捷式計劃進行資料科學專案的共同作業程式碼開發。
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: ca24a781f4f3ad5c210813dabbb896de35056ed6
ms.sourcegitcommit: d2d1c90ec5218b93abb80b8f3ed49dcf4327f7f4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/16/2020
ms.locfileid: "97588704"
---
# <a name="collaborative-coding-with-git"></a>使用 Git 共同撰寫程式碼

本文說明如何使用 Git 作為資料科學專案的共同作業程式碼開發架構。 本文涵蓋如何將 Azure Repos 中的程式碼連結至 Azure Boards 中的 [agile 開發](agile-development.md) 工作專案、如何進行程式碼審核，以及如何建立和合併變更的提取要求。

## <a name="link-a-work-item-to-an-azure-repos-branch"></a><a name='Linkaworkitemwithagitbranch-1'></a>將工作專案連結至 Azure Repos 分支 

Azure DevOps 提供一個便利的方式，將 Azure Boards 使用者案例或工作專案與 Azure Repos Git 儲存機制分支連接在一起。 您可以將使用者案例或工作直接連結至與其相關聯的程式碼。 

若要將工作專案連接至新分支，請選取工作專案旁的 [ **動作** ] 省略號 (**...**) ，然後在操作功能表上，選取 [新增分支]，然後選取 [ **新增分支**]。  

![1](./media/collaborative-coding-with-git/1-sprint-board-view.png)

在 [ **建立分支** ] 對話方塊中，提供新的分支名稱和基底 Azure Repos Git 儲存機制和分支。 基底存放庫必須與工作專案位於相同的 Azure DevOps 專案中。 基底分支可以是任何現有的分支。 選取 [ **建立分支**]。 

![2](./media/collaborative-coding-with-git/2-create-a-branch.png)

您也可以在 Windows 或 Linux 中，使用下列 Git bash 命令建立新的分支：

```bash
git checkout -b <new branch name> <base branch name>

```
如果您未指定 \<base branch name> ，則會根據建立新的分支 `main` 。 

若要切換至您的工作分支，請執行下列命令： 

```bash
git checkout <working branch name>
```

切換至工作分支之後，您就可以開始開發程式碼或檔成品來完成工作專案。 執行 `git checkout main` 會將您切換回 `main` 分支。

為每個使用者案例工作專案建立 Git 分支是很好的作法。 然後，針對每個工作專案，您可以根據使用者案例分支建立分支。 當您有多位人員針對相同專案使用不同的使用者案例，或在相同使用者案例的不同工作上工作時，組織中對應至使用者 Story-Task 關聯性的分支。 您可以讓每個小組成員在不同的分支上工作，或是在共用分支時于不同的程式碼或其他成品上工作，以將衝突降至最低。 

下圖顯示 TDSP 的建議分支策略。 您可能不需要太多的分支（如下所示），特別是當只有一或兩個人在某個專案上工作時，或只有一個人可以處理使用者案例的所有工作時。 但是，將開發分支與主要分支分開是很好的作法，而且有助於防止發行分支被開發活動中斷。 如需 Git 分支模型的完整說明，請參閱 [成功的 git 分支模型](https://nvie.com/posts/a-successful-git-branching-model/)。

![3](./media/collaborative-coding-with-git/3-git-branches.png)

您也可以將工作項目連結至現有的分支。 在工作專案的 **詳細資料** 頁面上，選取 [ **加入連結**]。 然後選取要連結工作專案的現有分支，然後選取 **[確定]**。 

![4](./media/collaborative-coding-with-git/4-link-to-an-existing-branch.png)

## <a name="work-on-the-branch-and-commit-changes"></a><a name='WorkonaBranchandCommittheChanges-2'></a>處理分支並認可變更 

對工作專案進行變更（例如將 R 腳本檔案新增至本機電腦的 `script` 分支）之後，您可以使用下列 Git bash 命令，將變更從本機分支認可至上游工作分支：

```bash
git status
git add .
git commit -m "added an R script file"
git push origin script
```

![5](./media/collaborative-coding-with-git/5-sprint-push-to-branch.png)

## <a name="create-a-pull-request"></a><a name='CreateapullrequestonVSTS-3'></a>建立提取要求

在一或多個認可和推送之後，當您準備好將目前的工作分支合併至其基底分支時，您可以在 Azure Repos 中建立並提交 *提取要求* 。 

從 Azure DevOps 專案的主頁面，指向左側導覽中的 [**存放庫**  >  **提取要求**]。 然後選取 [ **新增提取要求** ] 按鈕或 [ **建立提取要求** ] 連結。

![6](./media/collaborative-coding-with-git/6-spring-create-pull-request.png)

如有必要，請在 [ **新增提取要求** ] 畫面上，流覽至您想要合併變更的 Git 儲存機制和分支。 新增或變更任何您想要的資訊。 在 [ **審核者**] 下，加入審核者的名稱，然後選取 [ **建立**]。 

![7](./media/collaborative-coding-with-git/7-spring-send-pull-request.png)

## <a name="review-and-merge"></a><a name='ReviewandMerge-4'></a>檢閱及合併

當您建立提取要求之後，您的審核者會收到電子郵件通知，以檢查提取要求。 審核者會測試變更是否正常運作，並盡可能檢查要求者的變更。 審核者可以根據其評量來提出批註、要求變更，以及核准或拒絕提取要求。 

![8](./media/collaborative-coding-with-git/8-add_comments.png)

審核者核准變更之後，您或具有合併許可權的其他人可以將工作分支合併至其基底分支。 選取 [**完成**]，然後在 [**完成提取要求**] 對話方塊中選取 [**完成合併**]。 您可以選擇在合併工作分支之後將其刪除。 

![10](./media/collaborative-coding-with-git/10-spring-complete-pullrequest.png)

確認要求已標示為 **已完成**。 

![11](./media/collaborative-coding-with-git/11-spring-merge-pullrequest.png)

當您回到左側導覽中的 [ **存放庫** ] 時，您可以看到您已在刪除分支之後切換至主要分支 `script` 。

![12](./media/collaborative-coding-with-git/12-spring-branch-deleted.png)

您也可以使用下列 Git bash 命令，將 `script` 工作分支合併至其基底分支，並在合併之後刪除工作分支：

```bash
git checkout main
git merge script
git branch -d script
```

![13](./media/collaborative-coding-with-git/13-spring-branch-deleted-commandline.png)

## <a name="next-steps"></a>後續步驟

[執行資料科學](execute-data-science-tasks.md) 工作顯示如何使用公用程式來完成數個常見的資料科學工作，例如互動式資料探索、資料分析、報告和模型建立。

[範例](walkthroughs.md) 逐步解說會列出特定案例的逐步解說，其中包含連結和縮圖描述。 連結的案例說明如何將雲端和內部部署工具和服務組合成工作流程或管線，以建立智慧型應用程式。 

