---
title: Azure 應用程式 Insights-Azure Functions 支援的功能
description: 支援 Azure Functions 的 Application Insights 功能
ms.topic: reference
author: TimothyMothra
ms.author: tilee
ms.date: 4/23/2019
ms.reviewer: mbullwin
ms.openlocfilehash: b44279f31aea8fc02130f1c3d7520f42c648bd4c
ms.sourcegitcommit: e15c0bc8c63ab3b696e9e32999ef0abc694c7c41
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/16/2020
ms.locfileid: "97607944"
---
# <a name="application-insights-for-azure-functions-supported-features"></a>適用於 Azure Functions 的 Application Insights 支援的功能

Azure Functions 提供與 Application Insights 的[內建整合](../../azure-functions/functions-monitoring.md)，可透過 ILogger 介面來使用。 以下列出目前支援的功能。 請檢閱 Azure Functions 的[開始使用](../../azure-functions/configure-monitoring.md#enable-application-insights-integration)指南。

如需函數執行階段版本的詳細資訊，請參閱 [這裡](../../azure-functions/functions-versions.md)。

如需 Application Insights 相容版本的詳細資訊，請參閱相依[性。](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Logging.ApplicationInsights/)

## <a name="supported-features"></a>支援的功能

| Azure Functions                   | V1            | V2 & V3 | 
|-----------------------------------|---------------|------------------|
| | | | 
| **自動收集**        |               |                  |
| &bull; 要求                     | 是           | 是              |
| &bull; 例外狀況                   | 是           | 是              |
| &bull; 效能計數器         | 是           | 是              |
| &bull; 相依性                 |               |                  |
| &nbsp;&nbsp;&nbsp;&mdash; HTTP      |               | 是              |
| &nbsp;&nbsp;&nbsp;&mdash; ServiceBus|               | 是              |
| &nbsp;&nbsp;&nbsp;&mdash; EventHub  |               | 是              |
| &nbsp;&nbsp;&nbsp;&mdash; SQL       |               | 是              |
| | | | 
| **支援的功能**              |               |                  |
| &bull; QuickPulse/LiveMetrics       | 是           | 是              | 
| &nbsp;&nbsp;&nbsp;&mdash; 保護控制通道 |               | 是 | 
| &bull; 取樣                     | 是           | 是              | 
| &bull; 活動訊號                   | | 是              | 
| | | |
| **Correlation (相互關聯)**                     |               |                  |
| &bull; ServiceBus                  |               | 是              |
| &bull; EventHub                    |               | 是              |
| | | | 
| **可設定**                  |               |                  |           
| &bull;可完整設定。<br/>如需相關指示，請參閱 [Azure Functions](https://github.com/Microsoft/ApplicationInsights-aspnetcore/issues/759#issuecomment-426687852)。<br/>請參閱所有選項的 [ASP.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Custom-Configuration) 。           |               | 是                 | 

## <a name="performance-counters"></a>效能計數器

只有 Windows 機器才能進行效能計數器的自動收集。

## <a name="live-metrics--secure-control-channel"></a>即時計量和保護控制通道

您指定的自訂篩選條件準則會傳回給 Application Insights SDK 中的即時計量元件。 篩選條件可能會包含機密資訊，例如 customerIDs。 您可以利用祕密 API 金鑰來保護通道安全。 如需指示，請參閱[保護控制通道](./live-stream.md#secure-the-control-channel)。

## <a name="sampling"></a>取樣

Azure Functions 依預設會在其設定中啟用取樣。 如需詳細資訊，請參閱[設定取樣](../../azure-functions/configure-monitoring.md#configure-sampling)。

如果您的專案相依于 Application Insights SDK 以進行手動的遙測追蹤，而您的取樣設定與函數的取樣設定不同，您可能會遇到奇怪的行為。 

我們建議使用與函式相同的設定。 使用函式 **v2**，您可以使用您的函式中的相依性插入來取得相同的設定：

```csharp
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.Extensibility;

public class Function1 
{

    private readonly TelemetryClient telemetryClient;

    public Function1(TelemetryConfiguration configuration)
    {
        this.telemetryClient = new TelemetryClient(configuration);
    }

    [FunctionName("Function1")]
    public async Task<IActionResult> Run(
        [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequest req, ILogger logger)
    {
        this.telemetryClient.TrackTrace("C# HTTP trigger function processed a request.");
    }
}
```
