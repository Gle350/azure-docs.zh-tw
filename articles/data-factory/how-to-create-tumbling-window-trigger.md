---
title: 在 Azure Data Factory 中建立輪轉視窗觸發程式
description: 了解如何在 Azure Data Factory 中建立依輪轉視窗執行管線的觸發程序。
services: data-factory
documentationcenter: ''
author: chez-charlie
ms.author: chez
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 10/25/2020
ms.openlocfilehash: 4c40d394e48cb0cd8bc02ef7b37e7ed2b27e13c4
ms.sourcegitcommit: 63d0621404375d4ac64055f1df4177dfad3d6de6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/15/2020
ms.locfileid: "97511547"
---
# <a name="create-a-trigger-that-runs-a-pipeline-on-a-tumbling-window"></a>建立依輪轉視窗執行管線的觸發程序
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

本文章提供建立、啟動、監視輪轉視窗觸發程序的步驟。 如需觸發程序及支援類型的詳細資訊，請參閱[管線執行和觸發程序](concepts-pipeline-execution-triggers.md)。

輪轉視窗觸發程序是可從指定的開始時間定期引發，同時還能保留狀態的一種觸發程序。 輪轉視窗是一系列大小固定、非重疊的連續時間間隔。 輪轉視窗觸發程序與管線會有一對一的關係，並只能參考單一管線。 輪轉視窗觸發程式是排程觸發程式較高的加權替代方式，可提供適用于複雜案例的功能套件， (相依 [于其他輪轉視窗觸發](#tumbling-window-trigger-dependency)程式、重新 [執行失敗的作業](tumbling-window-trigger-dependency.md#monitor-dependencies) ，以及 [設定管線) 的使用者重試](#user-assigned-retries-of-pipelines) 。 若要進一步瞭解排程觸發程式和輪轉視窗觸發程式之間的差異，請前往 [這裡](concepts-pipeline-execution-triggers.md#trigger-type-comparison)。

## <a name="data-factory-ui"></a>Data Factory UI

1. 若要在 Data Factory UI 中建立輪轉視窗觸發程式，請選取 [ **觸發** 程式] 索引標籤，然後選取 [ **新增**]。 
1. 在 [觸發程式設定] 窗格開啟後，選取 [ **輪轉視窗]**，然後定義輪轉視窗觸發程式屬性。 
1. 完成時，選取 [儲存]。

![在 Azure 入口網站中建立輪轉視窗觸發程序](media/how-to-create-tumbling-window-trigger/create-tumbling-window-trigger.png)

## <a name="tumbling-window-trigger-type-properties"></a>輪轉視窗觸發程序類型屬性

輪轉視窗有下列觸發程序類型屬性：

```json
{
    "name": "MyTriggerName",
    "properties": {
        "type": "TumblingWindowTrigger",
        "runtimeState": "<<Started/Stopped/Disabled - readonly>>",
        "typeProperties": {
            "frequency": <<Minute/Hour>>,
            "interval": <<int>>,
            "startTime": "<<datetime>>",
            "endTime": <<datetime – optional>>,
            "delay": <<timespan – optional>>,
            "maxConcurrency": <<int>> (required, max allowed: 50),
            "retryPolicy": {
                "count": <<int - optional, default: 0>>,
                "intervalInSeconds": <<int>>,
            },
            "dependsOn": [
                {
                    "type": "TumblingWindowTriggerDependencyReference",
                    "size": <<timespan – optional>>,
                    "offset": <<timespan – optional>>,
                    "referenceTrigger": {
                        "referenceName": "MyTumblingWindowDependency1",
                        "type": "TriggerReference"
                    }
                },
                {
                    "type": "SelfDependencyTumblingWindowTriggerReference",
                    "size": <<timespan – optional>>,
                    "offset": <<timespan>>
                }
            ]
        },
        "pipeline": {
            "pipelineReference": {
                "type": "PipelineReference",
                "referenceName": "MyPipelineName"
            },
            "parameters": {
                "parameter1": {
                    "type": "Expression",
                    "value": "@{concat('output',formatDateTime(trigger().outputs.windowStartTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                },
                "parameter2": {
                    "type": "Expression",
                    "value": "@{concat('output',formatDateTime(trigger().outputs.windowEndTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                },
                "parameter3": "https://mydemo.azurewebsites.net/api/demoapi"
            }
        }
    }
}
```

下表提供與輪轉視窗觸發程序之週期和排程相關的主要 JSON 元素的概要概觀：

| JSON 元素 | 描述 | 類型 | 允許的值 | 必要 |
|:--- |:--- |:--- |:--- |:--- |
| **type** | 觸發程序的類型。 此類型為固定值 "TumblingWindowTrigger"。 | String | "TumblingWindowTrigger" | 是 |
| **runtimeState** | 觸發程序執行時間的目前狀態。<br/>**注意**：這個元素是 \<readOnly> 。 | String | "Started"、"Stopped"、"Disabled" | 是 |
| **frequency** | 一個字串，代表觸發程序一再執行的頻率單位 (分鐘或小時)。 如果 **startTime** 日期值比 **frequency** 值更細微，計算視窗界限時，會將 **startTime** 日期納入計算。 例如，如果 **frequency** 值是每小時，而 **startTime** 值是 2017-09-01T10:10:10Z，則第一個視窗是 (2017-09-01T10:10:10Z, 2017-09-01T11:10:10Z)。 | String | "minute"、"hour"  | 是 |
| **區間** | 代表 **frequency** 值之間隔的整數值，用來決定觸發程序執行的頻率。 例如，如果 **interval** 為 3，而 **frequency** 為 "hour"，則觸發程序就會每隔 3 小時重複執行一次。 <br/>**注意**：最小視窗間隔為5分鐘。 | 整數 | 正整數。 | 是 |
| **startTime**| 第一次出現，可以是過去。 第一個觸發程序間隔是 (**startTime**, **startTime** + **interval**)。 | Datetime | 日期時間值。 | 是 |
| **endTime**| 最後一次出現，可以是過去。 | Datetime | 日期時間值。 | 是 |
| **延遲** | 視窗延遲開始資料處理所延遲的時間長度。 管線執行會在預期執行時間加上 **delay** 後開始。 **delay** 定義觸發程序要在超過到期時間多久之後，才觸發新的執行。 **delay** 不會改變視窗的 **startTime**。 例如，**delay** 值為 00:10:00 表示延遲 10 分鐘。 | Timespan<br/>(hh:mm:ss)  | 時間範圍值，預設值是 00:00:00。 | 否 |
| **maxConcurrency** | 就緒視窗可引發的同時執行觸發程序數目。 例如，為昨天回填每小時執行，結果會有 24 個視窗。 如果 **maxConcurrency** = 10，只有前 10 個視窗 (00:00-01:00 - 00:09:00-10) 會引發觸發程序事件。 前 10 個觸發的管線執行完成之後，才會引發接下來 10 個視窗 (10:00-11:00 - 19:00 20:00) 的觸發程序執行。 繼續以本範例的 **maxConcurrency** = 10 說明，如果有 10 個就緒視窗，則總共會有 10 個管線執行。 如果只有 1 個就緒視窗，則只有 1 個管線執行。 | 整數 | 1 到 50 之間的整數。 | 是 |
| **retryPolicy: Count** | 到管線執行標示為 [失敗] 前的重試次數。  | 整數 | 整數，預設值為 0 (無重試)。 | 否 |
| **retryPolicy: intervalInSeconds** | 重試嘗試之間的延遲 (以秒指定) | 整數 | 秒數，預設值是 30。 | 否 |
| **dependsOn：類型** | TumblingWindowTriggerReference 的類型。 如果已設定相依性，則為必要項。 | String |  "TumblingWindowTriggerDependencyReference", "SelfDependencyTumblingWindowTriggerReference" | 否 |
| **dependsOn：大小** | 相依性輪轉視窗的大小。 | Timespan<br/>(hh:mm:ss)  | 正的 timespan 值，預設值為子觸發程式的視窗大小  | 否 |
| **dependsOn： offset** | 相依性觸發程式的位移。 | Timespan<br/>(hh:mm:ss) |  在自我相依性中必須為負數的 timespan 值。 如果未指定任何值，則視窗與觸發程式本身相同。 | 自我相依性：是<br/>其他：否  |

> [!NOTE]
> 發行輪轉視窗觸發程式之後，就無法編輯 **間隔** 和 **頻率** 。

### <a name="windowstart-and-windowend-system-variables"></a>WindowStart 和 WindowEnd 系統變數

您可以在您的 **pipeline** (管線) 定義中使用輪轉視窗觸發程序的 **WindowStart** 和 **WindowEnd** 系統變數 (也是查詢的一部分)。 將系統變數當作參數傳遞給 **trigger** (觸發程序) 定義中的管線。 以下範例示範如何將這些變數當作參數傳遞：

```json
{
    "name": "MyTriggerName",
    "properties": {
        "type": "TumblingWindowTrigger",
            ...
        "pipeline": {
            "pipelineReference": {
                "type": "PipelineReference",
                "referenceName": "MyPipelineName"
            },
            "parameters": {
                "MyWindowStart": {
                    "type": "Expression",
                    "value": "@{concat('output',formatDateTime(trigger().outputs.windowStartTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                },
                "MyWindowEnd": {
                    "type": "Expression",
                    "value": "@{concat('output',formatDateTime(trigger().outputs.windowEndTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                }
            }
        }
    }
}
```

若要在管線定義中使用 **WindowStart** 和 **WindowEnd** 系統變數值，分別使用您的 "MyWindowStart" 和 "MyWindowEnd" 參數。

### <a name="execution-order-of-windows-in-a-backfill-scenario"></a>回填案例的視窗執行順序

如果觸發程式的 startTime 是過去的，則根據此公式，M = (CurrentTime-TriggerStartTime) /TumblingWindowSize，觸發程式將會在執行未來的執行之前，以平行方式產生 {M} 回填 (過去) ，接受觸發程式並行處理。 Windows 的執行順序具有決定性，從最舊到最新的間隔。 目前無法修改此行為。

### <a name="existing-triggerresource-elements"></a>現有 TriggerResource 元素

下列幾點適用于現有 **TriggerResource** 元素的更新：

* 觸發程式建立之後，就無法變更 **frequency** 元素的值 (或視窗大小的) ，以及 **interval** 元素。 這是 triggerRun 重新運行和相依性評估正常運作的必要項
* 如果觸發程序的 **endTime** 元素值變更了 (新增或更新)，已經處理的視窗狀態「不會」重設。 觸發程序會採用新的 **endTime** 值。 如果新的 **endTime** 值在已經執行的視窗之前，觸發程序會停止。 反之，觸發程序會在遇到新的 **endTime** 值時停止。

### <a name="user-assigned-retries-of-pipelines"></a>使用者指派的管線重試次數

如果發生管線失敗，輪轉視窗觸發程式可以使用相同的輸入參數自動重試執行參考的管線，而不需要使用者介入。 您可以使用觸發程序定義中的 "retryPolicy" 屬性來指定此屬性。

### <a name="tumbling-window-trigger-dependency"></a>輪轉視窗觸發程式相依性

如果您想要確保只有在 data factory 中的另一個輪轉視窗觸發程式成功執行之後才執行輪轉視窗觸發程式，請 [建立輪轉視窗觸發](tumbling-window-trigger-dependency.md)程式相依性。

### <a name="cancel-tumbling-window-run"></a>取消輪轉視窗執行

您可以取消執行輪轉視窗觸發程式（如果特定視窗處於 _等候_ 中、 _等待_ 相依 _性或執行_ 中狀態）

* 如果 **視窗處於 [** 執行中] 狀態，請取消相關聯的 _管線執行_，並在之後將觸發程式執行標示為 _已取消_ 。
* 如果視窗處於 **等候** 中或 **正在等候** 相依性狀態，您可以從監視中取消視窗：

![從監視頁面取消輪轉視窗觸發程式](media/how-to-create-tumbling-window-trigger/cancel-tumbling-window-trigger.png)

您也可以重新執行已取消的視窗。 重新執行將會取得觸發程式的 _最新_ 發佈定義，而指定視窗的相依性會在重新執行時 _重新評估_

![重新執行先前取消之執行的輪轉視窗觸發程式](media/how-to-create-tumbling-window-trigger/rerun-tumbling-window-trigger.png)

## <a name="sample-for-azure-powershell"></a>Azure PowerShell 的範例

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

本節說明如何使用 Azure PowerShell 來建立、啟動及監視觸發程序。

1. 在 C:\ADFv2QuickStartPSH\ 資料夾中，使用下列內容建立名為 **MyTrigger.json** 的 JSON 檔案：

    > [!IMPORTANT]
    > 在您儲存 JSON 檔案之前，請先將 **startTime** 元素的值設定為目前的 UTC 時間。 將 **endTime** 元素的值設定為目前 UTC 時間的一小時之後。

    ```json
    {
      "name": "PerfTWTrigger",
      "properties": {
        "type": "TumblingWindowTrigger",
        "typeProperties": {
          "frequency": "Minute",
          "interval": "15",
          "startTime": "2017-09-08T05:30:00Z",
          "delay": "00:00:01",
          "retryPolicy": {
            "count": 2,
            "intervalInSeconds": 30
          },
          "maxConcurrency": 50
        },
        "pipeline": {
          "pipelineReference": {
            "type": "PipelineReference",
            "referenceName": "DynamicsToBlobPerfPipeline"
          },
          "parameters": {
            "windowStart": "@trigger().outputs.windowStartTime",
            "windowEnd": "@trigger().outputs.windowEndTime"
          }
        },
        "runtimeState": "Started"
      }
    }
    ```

2. 使用 **>start-azdatafactoryv2trigger** Cmdlet 建立觸發程式：

    ```powershell
    Set-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" -DefinitionFile "C:\ADFv2QuickStartPSH\MyTrigger.json"
    ```
    
3. 使用 **>start-azdatafactoryv2trigger** 指令程式確認觸發程式的狀態已 **停止**：

    ```powershell
    Get-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

4. 使用 **>start-azdatafactoryv2trigger** Cmdlet 來啟動觸發程式：

    ```powershell
    Start-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

5. 使用 **>start-azdatafactoryv2trigger** 指令程式確認觸發程式的狀態已 **啟動**：

    ```powershell
    Get-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

6. 使用 **AzDataFactoryV2TriggerRun** 指令程式取得在 Azure PowerShell 中執行的觸發程式。 若要取得有關觸發程序執行的資訊，請定期執行以下命令。 更新 **TriggerRunStartedAfter** 和 **TriggerRunStartedBefore** 的值，以符合您的觸發程序定義中的值：

    ```powershell
    Get-AzDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "MyTrigger" -TriggerRunStartedAfter "2017-12-08T00:00:00" -TriggerRunStartedBefore "2017-12-08T01:00:00"
    ```
    
若要在 Azure 入口網站中監視觸發程序執行和管線執行，請參閱[監視管線執行](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline)。

## <a name="next-steps"></a>後續步驟

* 如需有關觸發程序的詳細資訊，請參閱[管線執行和觸發程序](concepts-pipeline-execution-triggers.md#trigger-execution)。
* [建立輪轉視窗觸發程序相依性](tumbling-window-trigger-dependency.md)
