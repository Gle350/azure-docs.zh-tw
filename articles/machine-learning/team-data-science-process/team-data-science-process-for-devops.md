---
title: 適用於 DevOps 的 Team Data Science Process
description: 對進階分析和認知服務解決方案實作而言特定的開發人員作業 (DevOps) 功能。
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: a84942337b3c8eb5f7509f61f9ba5bcd564d8bb3
ms.sourcegitcommit: ad677fdb81f1a2a83ce72fa4f8a3a871f712599f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/17/2020
ms.locfileid: "97653073"
---
# <a name="team-data-science-process-for-developer-operations"></a>適用於開發人員作業的 Team Data Science Process

本文將探索對「進階分析」和「認知服務」解決方案實作而言特定的「開發人員作業」(DevOps) 功能。 這些訓練教材採用 Team Data 科學程式， (TDSP) 和 Microsoft 與開放原始碼軟體和工具組，可對構想、執行及提供資料科學解決方案有説明。 它會參考涵蓋 DevOps 工具鏈的主題，專屬於資料科學和 AI 專案與解決方案。

## <a name="lesson-path"></a>課程路徑
下表提供以層級為基礎的指導方針，可協助您完成在 Azure 上執行資料科學解決方案的 DevOps 目標。

| 目標 | 主題 | **Resource** | **技術** | **Level** | **先決條件** |
|--|--|--|--|--|--|
| 了解進階分析 | Team Data Science Process 生命週期 | [此技術逐步解說會說明 Team Data Science Process](overview.md) | 資料科學 | 中級 | 一般技術背景、熟悉資料解決方案、熟悉 IT 專案和解決方案實作 |
| 了解進階分析的 Microsoft Azure 平台 | 資訊管理 |
| [本參考提供建立分析資料解決方案之管線的 Azure Data Factory 總覽](../../data-factory/v1/data-factory-introduction.md) | Microsoft Azure Data Factory | 進階者 | 一般技術背景、熟悉資料解決方案、熟悉 IT 專案和解決方案實作 |
|  |
| [本參考涵蓋 Azure 資料目錄的概觀，可用來記載和管理您資料來源上的中繼資料](../../data-catalog/overview.md) | Microsoft Azure 資料目錄 | 中級 | 一般技術背景、熟悉資料解決方案、熟悉關聯式資料庫管理系統 (RDBMS) 和 NoSQL 資料來源 |
|  |
| [本此參考涵蓋 Azure 事件中樞系統的概觀，以及您可用它將資料擷取到您解決方案的方法](../../event-hubs/event-hubs-about.md) | Azure 事件中樞 | 中級 | 一般技術背景、熟悉資料解決方案、熟悉關聯式資料庫管理系統 (RDBMS) 和 NoSQL 資料來源、熟悉物聯網 (IoT) 術語和使用方式 |
|  | 巨量資料存放區 |
| [本參考涵蓋使用 Azure Synapse Analytics 儲存和處理大量資料的總覽](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) | Azure Synapse Analytics | 進階者 | 一般技術背景、熟悉資料解決方案、熟悉關聯式資料庫管理系統 (RDBMS) 和 NoSQL 資料來源、熟悉 HDFS 術語和使用方式 |
|  |  | [本參考會涵蓋使用 Azure Data Lake 在單一位置擷取任何大小、類型和擷取速度的資料，以便進行運作和探究分析的概觀](../../data-lake-store/data-lake-store-overview.md) | Azure Data Lake Store | 中級 | 一般技術背景、熟悉資料解決方案、熟悉 NoSQL 資料來源、熟悉 HDFS |
|  | 機器學習和分析 | [本參考涵蓋機器學習、預測性分析和人工智慧系統的簡介](../classic/index.yml) | Azure Machine Learning | 中級 | 一般技術背景、熟悉資料解決方案、熟悉資料科學詞彙、熟悉 Machine Learning 和人工智慧條款 |
|  |  | [本文提供 Azure HDInsight （Hadoop 技術堆疊的雲端發佈）的簡介。它也涵蓋了什麼是 Hadoop 叢集，以及使用它的時機](../../hdinsight/hadoop/apache-hadoop-introduction.md) | Azure HDInsight | 中級 | 一般技術背景、熟悉資料解決方案、熟悉 NoSQL 資料來源 |
|  |  | [本參考涵蓋 Azure Data Lake Analytics 作業服務的概觀](../../data-lake-analytics/data-lake-analytics-overview.md) | Azure Data Lake Analytics | 中級 | 一般技術背景、熟悉資料解決方案、熟悉 NoSQL 資料來源 |
|  |  | [本概觀涵蓋使用 Azure 串流分析作為可完全受控的事件處理引擎，可設定串流資料的即時分析計算](../../stream-analytics/stream-analytics-introduction.md) | Azure 串流分析 | 中級 | 一般技術背景、熟悉資料解決方案、熟悉結構化與非結構化資料概念 |
|  | 智慧 | [本參考涵蓋可用認知服務 (例如願景、文字和搜尋)，以及如何開始加以使用的概觀](../../cognitive-services/what-are-cognitive-services.md) | 認知服務 | 進階者 | 一般技術背景、熟悉資料解決方案、軟體開發 |
|  |  | [本參考涵蓋 Microsoft Bot Framework 以及如何開始加以使用的簡介](/bot-framework/overview-introduction-bot-framework) | Bot Framework | 進階者 | 一般技術背景、熟悉資料解決方案 |
|  | 視覺效果 | [這個自學的線上課程涵蓋 Power BI 系統，以及如何建立和發行報告](https://powerbi.microsoft.com/guided-learning/powerbi-learning-0-0-what-is-power-bi/) | Microsoft Power BI | 初級 | 一般技術背景、熟悉資料解決方案 |
|  | 方案 | [本資源頁面涵蓋多個應用程式，您可以加以檢閱、測試和實作，以查看從開始到完成的完整解決方案](https://gallery.cortanaintelligence.com/) | Microsoft Azure、Azure Machine Learning、認知服務、Microsoft R、Azure 認知搜尋、Python、Azure Data Factory、Power BI、Azure Document DB、Application Insights、Azure SQL DB、Azure Synapse Analytics、Microsoft SQL Server、Azure Data Lake、認知服務、Bot Framework、Azure Batch、 | 中級 | 一般技術背景、熟悉資料解決方案 |
| 了解及實作 DevOps 程序 | DevOps 基本概念 | [本影片系列說明 DevOps 的基本概念，並協助說明它們如何對應至 DevOps 實務](https://channel9.msdn.com/Series/DevOps-Fundamentals/Introduction-to-DevOps) | DevOps、Microsoft Azure 平台、Azure DevOps | 進階者 | 使用 SDLC、熟悉 Agile 和其他開發架構、熟悉 IT 作業 |
| 將 DevOps 工具鏈用於資料科學 | 設定 | [本參考涵蓋在 Visio 中選擇適當視覺效果來與專案設計通訊的基本概念](https://support.office.com/article/Illustrate-business-processes-with-Visio-flowcharts-DAB16418-1FE6-4DE0-8F26-DBA44A26ED65) | Visio | 中級 | 一般技術背景、熟悉資料解決方案 |
|  |  | [本參考描述 Azure Resource Manager 詞彙，並作為範例、快速入門和其他參考資料的主要根來源](../../azure-resource-manager/management/overview.md) | Azure Resource Manager, Azure PowerShell, Azure CLI | 中級 | 一般技術背景、熟悉資料解決方案 |
|  |  | [本參考會說明適用於 Linux 和 Windows 的 Azure Data Science Virtual Machine](../data-science-virtual-machine/overview.md) | 資料科學虛擬機器 | 進階者 | 熟悉資料科學工作負載、Linux |
|  |  | [本逐步解說會說明使用 Visual Studio 設定 Azure 雲端服務角色 - 密切注意連接字串，特別是針對儲存體帳戶](/visualstudio/azure/vs-azure-tools-configure-roles-for-cloud-service#configure-an-azure-cloud-service) | Visual Studio | 中級 | 軟體開發 |
|  |  | [這一系列將教導您如何使用 Microsoft Project 來排程進階分析專案的時間、資源和目標](https://support.office.com/article/Project-2013-videos-and-tutorials-af7d1e17-5fa7-421f-a452-9bbe2cd7b082) | Microsoft Project | 中級 | 了解專案管理基本概念 |
|  |  | [這個 Microsoft Project 範本會提供進階分析專案的時間、資源和目標追蹤](https://buckwoody.wordpress.com/2017/08/17/a-data-science-microsoft-project-template-you-can-use-in-your-solutions/) | Microsoft Project | 中級 | 了解專案管理基本概念 |
|  |  | [本 Azure 資料目錄教學課程說明企業資料資產的註冊和探索系統](../../data-catalog/data-catalog-get-started.md) | Azure 資料目錄 | 初級 | 熟悉資料來源與結構 |
|  |  | [本 Microsoft 虛擬學術課程說明如何使用 Visual Studio Codespace 和 Microsoft Azure 來設定 Dev-Test](https://mva.microsoft.com/training-courses/dev-test-with-visual-studio-online-and-microsoft-azure-8420?l=P7Ot1TKz_2104984382) | Visual Studio Codespace | 進階者 | 軟體開發、熟悉開發/測試環境 |
|  |  | [本 Microsoft System Center 的管理組件下載包含指導方針文件，可協助您使用 Azure 資產](https://www.microsoft.com/download/details.aspx?id=38414) | System Center | 中級 | 使用 System Center 進行 IT 管理的體驗 |
|  |  | [本文件旨在讓開發人員和作業小組了解 PowerShell 預期狀態設定的優點](/powershell/scripting/dsc/overview/dscforengineers) | PowerShell DSC | 中級 | 使用 PowerShell 程式碼撰寫、企業架構、指令碼的體驗 |
|  | 程式碼 | [此下載也包含有關使用 Visual Studio Codespace 程式碼來建立資料科學和 AI 應用程式的檔](https://code.visualstudio.com/) | Visual Studio Codespace | 中級 | 軟體開發 |
|  |  | [本快速入門網站會教導您有關 DevOps 與 Visual Studio](https://www.visualstudio.com/devops/) | Visual Studio | 初級 | 軟體開發 |
|  |  | [您可以使用 App Service 編輯器直接從 Azure 入口網站撰寫程式碼。深入瞭解此資源與此工具的持續整合](https://github.com/projectkudu/kudu/wiki/App-Service-Editor) | Azure 入口網站 | 高級進階者 | 資料科學背景 - 但無論如何請閱讀 |
|  |  | [此資源說明如何使用 web Azure Machine Learning Studio (傳統) 工具撰寫程式碼及建立預測性分析實驗](../overview-what-is-machine-learning-studio.md#ml-studio-classic-vs-azure-machine-learning-studio) | Azure Machine Learning Studio (傳統)  | 進階者 | 軟體開發 |
|  |  | [本參考包含 Azure 中資料科學虛擬機器所有開發工具的清單和研究連結](../data-science-virtual-machine/overview.md) | 資料科學虛擬機器 | 進階者 | 軟體開發、資料科學 |
|  |  | [在本 Azure 安全性信任中心閱讀並了解每個參考的安全性、隱私權和相容性 - 非常重要](https://azure.microsoft.com/support/trust-center/) | Azure 安全性 | 中級 | 系統架構體驗、安全性開發體驗 |
|  | Build | [本課程會教您如何使用 Visual Studio Codespace 組建來啟用 DevOps 實務](https://mva.microsoft.com/training-courses/enabling-devops-practices-with-visual-studio-online-build-12478?l=ipCj6MuNB_6305094681) | Visual Studio Codespace | 進階者 | 軟體開發、熟悉 SDLC |
|  |  | [本參考會說明使用 Visual Studio 進行編譯和建置](/previous-versions/visualstudio/visual-studio-2015/ide/compiling-and-building-in-visual-studio) | Visual Studio | 中級 | 軟體開發、熟悉 SDLC |
|  |  | [本參考會說明如何協調處理，例如包含 Runbook 的軟體組建](/system-center/orchestrator/automate-runbooks) | System Center | 進階者 | 使用 System Center 協調器的體驗 |
|  | 測試 | [使用此參考來瞭解如何使用 Visual Studio Codespace 進行測試案例管理](http://www.almguide.com/2014/07/visual-studio-online-test-case-management/) | Visual Studio Codespace | 進階者 | 軟體開發、熟悉 SDLC |
|  |  | [使用此 Runbook 的先前參考來使用 System Center 自動化測試](/system-center/orchestrator/automate-runbooks) | System Center | 進階者 | 使用 System Center 協調器的體驗 |
|  |  | [除了測試但開發的過程中，您應該建立安全性。Microsoft SDL Threat Modeling Tool 在所有階段都能提供協助。深入瞭解並下載](https://www.microsoft.com/SDL/adopt/threatmodeling.aspx) | 威脅監視工具 | 進階者 | 熟悉安全性概念、軟體開發 |
|  |  | [這篇文章會說明如何使用 Microsoft Attack Surface Analyzer 來測試您的進階分析解決方案](https://technet.microsoft.com/security/gg749821.aspx) | Attack Surface Analyzer | 進階者 | 熟悉安全性概念、軟體開發 |
|  | 套件 | [本參考說明在 TFS 和 Visual Studio 中使用封裝的概念 Codespace](https://www.visualstudio.com/docs/package/collaborate-with-packages) | Visual Studio Codespace | 進階者 | 軟體開發、熟悉 SDLC |
|  |  | [使用此 Runbook 的先前參考來使用 System Center 自動化封裝](/system-center/orchestrator/automate-runbooks) | System Center | 進階者 | 使用 System Center 協調器的體驗 |
|  |  | [本參考會說明如何建立您解決方案的資料管線，您可儲存為 JSON 範本作為 [套件]](../../data-factory/v1/data-factory-introduction.md) | Azure Data Factory | 中級 | 一般計算背景、資料專案體驗 |
|  |  | [本主題說明 Azure Resource Manager 範本的結構](../../azure-resource-manager/templates/template-syntax.md) | Azure Resource Manager | 中級 | 熟悉 Microsoft Azure 平台 |
|  |  | [DSC 是 PowerShell 中的管理平臺，可讓您使用設定即程式碼來管理 IT 和開發基礎結構，並儲存為套件。本參考是該主題的總覽](/powershell/scripting/dsc/overview/overview) | PowerShell Desired State Configuration | 中級 | 使用 PowerShell 程式碼撰寫、企業架構、指令碼的體驗 |
|  | 版本 | [此標頭參考文件包含適用於 CI/CD 環境的組建、測試及發行等概念](/azure/devops/pipelines/?view=azure-devops) | Visual Studio Codespace | 進階者 | 軟體開發、熟悉 CI/CD 環境、熟悉 SDLC |
|  |  | [使用此 Runbook 的先前參考來使用 System Center 自動化發行管理](/system-center/orchestrator/automate-runbooks) | System Center | 進階者 | 使用 System Center 協調器的體驗 |
|  |  | [這篇文章可協助您判斷將 Web 應用程式、行動應用程式後端或 API 應用程式檔案部署至 Azure App Service的最佳選項，並針對您的慣用選項，引導您參考含適當指示的資源](../../app-service/deploy-local-git.md) | Microsoft Azure 部署 | 中級 | 軟體開發、使用 Microsoft Azure 平台的體驗 |
|  | 監視器 | [本參考會說明 Application Insights，以及您可以如何將它新增至您的進階分析解決方案](../../azure-monitor/app/app-insights-overview.md) | Application Insights | 中級 | 軟體開發、熟悉 Microsoft Azure 平台 |
|  |  | [本主題會說明 Operations Manager 的相關基本概念，適用於管理 Operations Manager 基礎結構的系統管理員，以及監視及支援進階分析解決方案的操作員](/previous-versions/system-center/system-center-2012-R2/hh230741(v=sc.12)) | System Center | 進階者 | 熟悉企業監視、System Center Operations Manager |
|  |  | [此部落格文章會說明如何使用 Azure Data Factory 來監視及管理進階分析管線](https://azure.microsoft.com/blog/azure-data-factory-updates-monitoring-and-management-enhancements/) | Azure Data Factory | 中級 | 熟悉 Azure Data Factory |
|  |  | [這段影片說明如何使用 Azure 監視器記錄監視記錄檔](https://channel9.msdn.com/Shows/Data-Exposed/Enterprise-HDInsight-Monitoring-with-Operations-Management-Suite) | Azure 記錄、PowerShell | 進階者 | 熟悉 Azure 平台 |
| 了解如何透過 Azure 的 DevOps 使用開放原始碼工具 | 開放原始碼 DevOps 工具和 Azure | [本參考頁面包含兩個關於使用 Chef 搭配 Azure 部署的視訊和白皮書](https://www.chef.io/) | Chef | 進階者 | 熟悉 Azure 平台、熟悉 DevOps |
|  |  | [此站台具有工具鏈選取項目路徑](https://azure.microsoft.com/try/devops/) | DevOps、Microsoft Azure 平台、Azure DevOps、開放原始碼軟體 | 進階者 | 使用 SDLC、熟悉 Agile 和其他開發架構、熟悉 IT 作業 |
|  |  | [本教學課程會使用持續整合和部署 CI/CD 管線，將應用程式開發的組建和測試階段自動化](/azure/developer/jenkins/pipeline-with-github-and-docker) | Jenkins | 進階者 | 熟悉 Azure 平台、熟悉 DevOps、熟悉 Jenkins |
|  |  | [這包含使用 Docker 和 Azure，以及實作資料科學應用程式的其他參考概觀](https://www.docker.com/docker-azure#/overview) | Docker | 中級 | 熟悉 Azure 平台、熟悉伺服器作業系統 |
|  |  | [此安裝和說明會說明如何使用 Visual Studio Code 與 Azure 資產](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.OpeninVisualStudioCode) | VSCODE | 中級 | 軟體開發、熟悉 Microsoft Azure 平台 |
|  |  | [此部落格文章會說明如何使用 R Studio 與 Microsoft R](https://www.r-bloggers.com/using-microsoft-r-open-with-rstudio/) | R Studio | 中級 | R 語言體驗 |
|  |  | [此部落格文章會示範如何使用持續整合搭配 Azure 和 GitHub](https://blogs.msdn.microsoft.com/microsoftimagine/2015/09/01/using-continuous-integration-with-azure-github/) | Git、GitHub | 中級 | 軟體開發 |

## <a name="next-steps"></a>後續步驟

[適用于資料科學家的 Team Data 科學流程](team-data-science-process-for-data-scientists.md) 本文提供使用 Azure 來執行資料科學解決方案的指引。
