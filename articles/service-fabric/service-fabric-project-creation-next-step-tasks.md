---
title: Service Fabric 專案建立後續步驟
description: 了解您剛才在 Visual Studio 中建立的應用程式專案。  了解如何使用教學課程建置服務，並深入了解為 Service Fabric 開發服務。
ms.topic: conceptual
ms.date: 12/21/2020
ms.custom: contperf-fy21q2
ms.openlocfilehash: 59c8eb0737d2cef1c4b1df34d673b74944fef4e1
ms.sourcegitcommit: 6cca6698e98e61c1eea2afea681442bd306487a4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/24/2020
ms.locfileid: "97760430"
---
# <a name="your-service-fabric-application-and-next-steps"></a>您的 Service Fabric 應用程式和後續步驟
您的 Azure Service Fabric 應用程式已經建立。 本文包含許多資源、您可能感興趣的一些資訊，以及可能的 [後續步驟](#next-steps)。

新的使用者可能會發現教學課程、逐步解說 [和範例](#get-started-with-tutorials-walk-throughs-and-samples) 很有用。 檢查已 [建立應用程式專案的結構](#the-application-project)時，也會很有用。 也包含 Service Fabric 的程式 [設計模型](#learn-more-about-the-programming-models)、 [服務通訊](#learn-about-service-communication)、 [應用程式安全性](#learn-about-configuring-application-security)和 [應用程式生命週期](#learn-about-the-application-lifecycle)的說明。

更有經驗的使用者可能會發現 Service Fabric 的 [最佳做法](#learn-about-best-practices) 章節，可説明您瞭解如何利用最效力的平臺和結構應用程式。

如有問題或意見反應，或想要回報問題的人，請參閱 [對應的章節](#have-questions-or-feedback--need-to-report-an-issue)。

## <a name="get-started-with-tutorials-walk-throughs-and-samples"></a>開始使用教學課程、逐步解說和範例
準備開始使用了嗎？  

逐步查看 .NET 應用程式教學課程。 了解如何使用 ASP.NET Core 前端和具狀態後端來[建置應用程式](service-fabric-tutorial-create-dotnet-app.md)、[部署應用程式](service-fabric-tutorial-deploy-app-to-party-cluster.md)至叢集、[設定 CI/CD](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)，以及[設定監視和診斷](service-fabric-tutorial-monitoring-aspnet.md)。

或者，嘗試其中一個下列逐步解說，然後建立您的第一個...
- [Windows 上的 C# Reliable Services 服務](service-fabric-reliable-services-quick-start.md) 
- [Windows 上的 C# Reliable Actors 服務](service-fabric-reliable-actors-get-started.md) 
- [Windows 上可供來賓執行的服務](quickstart-guest-app.md) 
- [Windows 容器應用程式](service-fabric-get-started-containers.md) 

您可以試用我們的[範例應用程式](/samples/browse/?products=azure)。

## <a name="the-application-project"></a>應用程式專案
每個新應用程式都包含一個應用程式專案。 視所選擇的服務類型而定，可能會有一個或兩個額外的專案。

應用程式專案包含：

* 一組對構成應用程式之服務的參考。
* 三個發行設定檔 (1-Node Local、5-Node Local、Cloud)，可用來維護喜好設定，以搭配不同的環境使用，例如叢集端點的相關喜好設定，以及是否預設要執行升級部署。
* 三個應用程式參數檔案 (同上)，可用來維護環境特定應用程式組態，例如為服務建立之分割區的數目。 了解如何[針對多種環境設定您的應用程式](service-fabric-manage-multiple-environment-app-configuration.md)。
* 部署指令碼，可用來從命令列部署您的應用程式，或透過自動化連續整合和部署管線中部署您的應用程式。 深入了解[使用 PowerShell 部署應用程式](service-fabric-deploy-remove-applications.md)。
* 應用程式資訊清單，描述應用程式。 您可以在 ApplicationPackageRoot 資料夾下方找到資訊清單。 深入了解[應用程式及服務資訊清單](service-fabric-application-model.md)。

## <a name="learn-more-about-the-programming-models"></a>深入了解有關程式設計模型
Service Fabric 提供多種撰寫和管理服務的方式。  以下的概觀和概念性資訊是關於[無狀態與具狀態的 Reliable Services](service-fabric-reliable-services-introduction.md)、[Reliable Actors](service-fabric-reliable-actors-introduction.md)、[容器](service-fabric-containers-overview.md)、[可供來賓執行](service-fabric-guest-executables-introduction.md)和[無狀態與具狀態的 ASP.NET Core 服務](service-fabric-reliable-services-communication-aspnetcore.md)。

## <a name="learn-about-service-communication"></a>深入了解服務通訊
Service Fabric 應用程式是由不同的服務組成，每一個服務用來執行特定的工作。 這些服務可能會彼此通訊，且連接到服務並與服務通訊的叢集外可能會有用戶端應用程式。 了解如何在 Service Fabric 中[設定您的服務之間的通訊](service-fabric-connect-and-communicate-with-services.md)。 

## <a name="learn-about-configuring-application-security"></a>了解有關設定應用程式安全性
您可以保護在叢集中以不同使用者帳戶執行的應用程式。 在以該使用者帳戶部署時，Service Fabric 也會協助保護應用程式所使用的資源，例如檔案、目錄和憑證。 如此一來，即使在共用主控環境中，執行中的應用程式就不會彼此干擾。  了解如何[設定應用程式的安全性原則](service-fabric-application-runas-security.md)。

應用程式可能包含機密資訊，例如儲存體連接字串、密碼或其他不會以純文字處理的值。 了解如何[管理您的應用程式中的祕密](service-fabric-application-secret-management.md)。

## <a name="learn-about-the-application-lifecycle"></a>了解應用程式生命週期
如同其他平台，Service Fabric 應用程式通常會經歷下列階段：設計、開發、測試、部署、升級、維護和移除。 [本文提供 api](service-fabric-application-lifecycle.md) 的總覽，以及這些 api 在 Service Fabric 應用程式生命週期的各個階段中，如何由不同的角色使用。

## <a name="learn-about-best-practices"></a>深入瞭解最佳做法
Service Fabric 有許多描述 [最佳作法](./service-fabric-best-practices-overview.md)的文章。 您可以利用這項資訊來協助確保您的叢集和應用程式的執行效能更好。
涵蓋的主題包括：
* [安全性](./service-fabric-best-practices-security.md)
* [網路功能](./service-fabric-best-practices-networking.md)
* [計算規劃和調整](./service-fabric-best-practices-capacity-scaling.md)
* [基礎結構即程式碼](./service-fabric-best-practices-infrastructure-as-code.md)
* [監視和診斷](./service-fabric-best-practices-monitoring.md)
* [應用程式設計](./service-fabric-best-practices-applications.md)

此外也包含 [生產環境就緒檢查清單](./service-fabric-production-readiness-checklist.md) ，可將所有最佳作法資訊整合成容易使用的格式。

## <a name="have-questions-or-feedback--need-to-report-an-issue"></a>有任何問題或意見嗎？  需要回報問題？
閱讀[常見問題](service-fabric-common-questions.md)並尋找有關 Service Fabric 可以執行的工作和其使用方式的答案。

[疑難排解指南](https://github.com/Azure/Service-Fabric-Troubleshooting-Guides) 可用來協助診斷和解決 Service Fabric 叢集中的常見問題。

[支援選項](service-fabric-support.md)列出 StackOverflow 和 MSDN 上用於詢問問題的論壇，以及用於報告問題、取得支援及提交產品意見反應的選項。


## <a name="next-steps"></a>後續步驟
- [在 Azure 中建立 Windows 叢集](service-fabric-tutorial-create-vnet-and-windows-cluster.md)。
- 使用 [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) 將叢集視覺化，包括已部署的應用程式和實體配置。
- [進行服務版本設定和升級](service-fabric-application-upgrade-tutorial.md)