---
title: 什麼是 Azure Active Directory 監視？ | Microsoft Docs
description: 提供 Azure Active Directory 監視的一般概觀。
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: e2b3d8ce-708a-46e4-b474-123792f35526
ms.service: active-directory
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/18/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 427cf2614f81a086dcb174db06cd636df4876c7e
ms.sourcegitcommit: 8b4b4e060c109a97d58e8f8df6f5d759f1ef12cf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/07/2020
ms.locfileid: "96778490"
---
# <a name="what-is-azure-active-directory-monitoring"></a>什麼是 Azure Active Directory 監視？

透過 Azure Active Directory (Azure AD) 監視，您現在可以將 Azure AD 活動記錄路由傳送至不同的端點。 您可以保留它以供長期使用，或將它與第三方安全性資訊與事件管理 (SIEM) 工具整合，以深入了解您的環境。

目前，您可以將記錄路由傳送至：

- 一個 Azure 儲存體帳戶。
- Azure 事件中樞，因此您可以與 Splunk 和 Sumologic 執行個體整合。
- Azure Log Analytics 工作區，您可在其中分析資料、建立儀表板以及特定事件的警示

**必要角色**：全域管理員

> [!VIDEO https://www.youtube.com/embed/syT-9KNfug8]

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="licensing-and-prerequisites-for-azure-ad-reporting-and-monitoring"></a>Azure AD 報告和監視的授權和必要條件

您需要 Azure AD premium 授權，才能存取 Azure AD 登入記錄。

如需詳細功能與授權資訊，請參閱 [Azure Active Directory 定價指南](https://azure.microsoft.com/pricing/details/active-directory/)。

若要部署 Azure AD 監視和報告，您需要擁有 Azure AD 租用戶的全域管理員或安全性系統管理員權限的使用者。

視記錄資料的最終目的地而定，您需要具備下列其中一項條件：

* 您具有 ListKeys 權限的 Azure 儲存體帳戶。 建議您使用一般儲存體帳戶，而不要使用 Blob 儲存體帳戶。 如需儲存體價格資訊，請參閱 [Azure 儲存體價格計算機](https://azure.microsoft.com/pricing/calculator/?service=storage)。

* Azure 事件中樞命名空間，以便與第三方 SIEM 解決方案整合。

* 用來將記錄傳送至 Azure 監視器記錄的 Azure Log Analytics 工作區。

## <a name="diagnostic-settings-configuration"></a>診斷設定組態

若要設定 Azure AD 活動記錄的監視設定，請先登入 [Azure 入口網站](https://portal.azure.com)，然後選取 **Azure Active Directory**。 在此，您有兩種方式可存取診斷設定組態頁面：

* 從 [監視] 區段選取 [診斷設定]。

    ![診斷設定](./media/overview-monitoring/diagnostic-settings.png)
    
* 選取 [稽核記錄] 或 [登入]，然後選取 [匯出設定]。 

    ![匯出設定](./media/overview-monitoring/export-settings.png)


## <a name="route-logs-to-storage-account"></a>將記錄路由傳送至儲存體帳戶

將記錄路由傳送至 Azure 儲存體帳戶，您可以將它保留超過[保留原則](reference-reports-data-retention.md)中所述的預設保留期。 了解如何[將資料路由傳送至儲存體帳戶](quickstart-azure-monitor-route-logs-to-storage-account.md)。

## <a name="stream-logs-to-event-hub"></a>將記錄串流至事件中樞

將記錄路由傳送至 Azure 事件中樞，可讓您與 Sumologic 和 Splunk 等第三方 SIEM 工具整合。 此整合可讓您結合 Azure AD 活動記錄資料與 SIEM 所管理的其他資料，進而更深入了解您的環境。 了解如何[將記錄串流至事件中樞](tutorial-azure-monitor-stream-logs-to-event-hub.md)。

## <a name="send-logs-to-azure-monitor-logs"></a>將記錄傳送至 Azure 監視器記錄

[Azure 監視器記錄](../../azure-monitor/log-query/log-query-overview.md)解決方案可合併不同來源的監視資料，並提供查詢語言和分析引擎，讓您深入解析應用程式和資源的作業。 將 Azure AD 活動記錄傳送至 Azure 監視器記錄，您即可快速擷取、監視及警示所收集的資料。 了解如何[將資料傳送至 Azure 監視器記錄](howto-integrate-activity-logs-with-log-analytics.md)。

您也可以安裝預先建立的 Azure AD 活動記錄檢視，以監視有關登入和稽核事件的常見案例。 了解如何[安裝與使用適用於 Azure AD 活動記錄的記錄分析檢視](howto-install-use-log-analytics-views.md)。

## <a name="next-steps"></a>後續步驟

* [Azure 監視器中的活動記錄](concept-activity-logs-azure-monitor.md)
* [將記錄串流至事件中樞](tutorial-azure-monitor-stream-logs-to-event-hub.md)
* [將記錄傳送至 Azure 監視器記錄](howto-integrate-activity-logs-with-log-analytics.md)
