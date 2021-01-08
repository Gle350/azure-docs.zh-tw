---
title: 清除 Azure 串流分析作業
description: 本文將為您說明刪除 Azure 串流分析作業的不同方法。
author: enkrumah
ms.author: ebnkruma
ms.service: stream-analytics
ms.topic: how-to
ms.date: 06/21/2019
ms.custom: seodec18
ms.openlocfilehash: 31812ac805bd3465b1b735842b45adb372286d66
ms.sourcegitcommit: 42a4d0e8fa84609bec0f6c241abe1c20036b9575
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98016060"
---
# <a name="stop-or-delete-your-azure-stream-analytics-job"></a>停止或刪除您的 Azure 串流分析作業

您可以透過 Azure 入口網站、Azure PowerShell、Azure SDK for .Net 或 REST API 輕鬆地停止或刪除 azure 串流分析作業。 一旦刪除串流分析工作，就無法復原。

>[!NOTE] 
>當您停止串流分析作業時，資料只會保留在輸入和輸出儲存體中，例如事件中樞或 Azure SQL Database。 如果您需要移除 Azure 中的資料，請務必遵循串流分析作業的輸入和輸出資源的移除流程。

## <a name="stop-a-job-in-azure-portal"></a>在 Azure 入口網站中停止作業

當您停止工作時，會取消布建資源，並停止處理事件。 與此工作相關的費用也會停止。 不過，您的所有設定都會保留下來，您可以稍後再重新開機作業 

1. 登入 [Azure 入口網站](https://portal.azure.com)。 

2. 找到執行中的串流分析作業並加以選取。

3. 在 [串流分析作業] 頁面上，選取 [停止] 以停止作業。 

   ![停止 Azure 串流分析作業](./media/stream-analytics-clean-up-your-job/stop-stream-analytics-job.png)


## <a name="delete-a-job-in-azure-portal"></a>在 Azure 入口網站中刪除作業

>[!WARNING] 
>一旦刪除串流分析工作，就無法復原。

1. 登入 Azure 入口網站。 

2. 找到現有的串流分析作業並加以選取。

3. 在 [串流分析作業] 頁面上，選取 [刪除] 以刪除作業。 

   ![刪除 Azure 串流分析作業](./media/stream-analytics-clean-up-your-job/delete-stream-analytics-job.png)


## <a name="stop-or-delete-a-job-using-powershell"></a>使用 PowerShell 停止或刪除作業

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

若要使用 PowerShell 停止作業，請使用 [AzStreamAnalyticsJob](/powershell/module/az.streamanalytics/stop-azstreamanalyticsjob) Cmdlet。 若要使用 PowerShell 來刪除作業，請使用 [AzStreamAnalyticsJob](/powershell/module/az.streamanalytics/Remove-azStreamAnalyticsJob) Cmdlet。

## <a name="stop-or-delete-a-job-using-azure-sdk-for-net"></a>使用適用於 .NET 的 Azure SDK 停止或刪除作業

若要使用適用於 .NET 的 Azure SDK 停止作業，請使用 [StreamingJobsOperationsExtensions.BeginStop](/dotnet/api/microsoft.azure.management.streamanalytics.streamingjobsoperationsextensions.beginstop) 方法。 若要使用適用於 .NET 的 Azure SDK 刪除作業，請使用 [StreamingJobsOperationsExtensions.BeginDelete](/dotnet/api/microsoft.azure.management.streamanalytics.streamingjobsoperationsextensions.begindelete) 方法。

## <a name="stop-or-delete-a-job-using-rest-api"></a>使用 REST API 停止或刪除作業

若要使用 REST API 停止作業，請參閱 [Stop](/rest/api/streamanalytics/2016-03-01/streamingjobs/stop) 方法。 若要使用 REST API 刪除作業，請參閱 [Delete](/rest/api/streamanalytics/2016-03-01/streamingjobs/delete) 方法。