---
title: 外部可用性錯誤碼 - Azure 串流分析
description: 使用外部可用性錯誤碼針對 Azure 串流分析問題進行疑難排解。
ms.author: sidram
author: sidramadoss
ms.topic: troubleshooting
ms.date: 05/07/2020
ms.service: stream-analytics
ms.openlocfilehash: 71625c4b81fc0795c376a89397e98659f4c72ff0
ms.sourcegitcommit: 42a4d0e8fa84609bec0f6c241abe1c20036b9575
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98020548"
---
# <a name="azure-stream-analytics-external-availability-error-codes"></a>Azure 串流分析的外部可用性錯誤碼

您可使用活動記錄和資源記錄，協助對 Azure 串流分析作業中的非預期行為進行偵錯。 本文列出每個外部可用性錯誤碼的描述。 當相依服務無法使用時，就會發生外部可用性錯誤。

## <a name="externalserviceunavailable"></a>ExternalServiceUnavailable

* **原因**︰服務暫時無法使用。
* **建議**：串流分析會繼續嘗試連線到服務。

## <a name="eventhubmessagingerror"></a>EventHubMessagingError

* **原因**︰串流分析在與 EventHub 通訊時發生錯誤。 


## <a name="next-steps"></a>後續步驟

* [對輸入連線進行疑難排解](stream-analytics-troubleshoot-input.md)
* [對 Azure 串流分析輸出進行疑難排解](stream-analytics-troubleshoot-output.md)
* [對 Azure 串流分析查詢進行疑難排解](stream-analytics-troubleshoot-query.md)
* [使用資源記錄對 Azure 串流分析進行疑難排解](stream-analytics-job-diagnostic-logs.md)
* [Azure 串流分析資料錯誤](data-errors.md)
