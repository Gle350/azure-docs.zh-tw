---
title: 使用 PowerShell 大量複製資料
description: 使用 Azure Data Factory 搭配複製活動，將資料從來源資料存放區大量複製到目的地資料存放區。
services: data-factory
author: linda33wj
ms.author: jingwang
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: tutorial
ms.custom: seo-lt-2019
ms.date: 01/22/2018
ms.openlocfilehash: bf40353a8f29200ab2a33859473dbc504c29bf7d
ms.sourcegitcommit: 63d0621404375d4ac64055f1df4177dfad3d6de6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/15/2020
ms.locfileid: "97510429"
---
# <a name="copy-multiple-tables-in-bulk-by-using-azure-data-factory-using-powershell"></a>使用 PowerShell 以 Azure Data Factory 大量複製多份資料表

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

本教學課程示範如何 **從 Azure SQL Database 將多個資料表複製到 Azure Synapse Analytics**。 您也可以在其他複製案例中套用相同模式。 例如，將資料表從 SQL Server/Oracle 複製到 Azure SQL Database/資料倉儲/Azure Blob，將不同的路徑從 Blob 複製到 Azure SQL Database 資料表。

概括而言，本教學課程包含下列步驟：

> [!div class="checklist"]
> * 建立資料處理站。
> * 建立 Azure SQL Database、Azure Synapse Analytics 和 Azure 儲存體連結服務。
> * 建立 Azure SQL Database 和 Azure Synapse Analytics 資料集。
> * 建立管線來查詢要複製的資料表和其他管線，以執行實際的複製作業。 
> * 啟動管線執行。
> * 監視管線和活動執行。

本教學課程使用 Azure PowerShell。 若要了解如何使用其他工具/SDK 來建立資料處理站，請參閱[快速入門](quickstart-create-data-factory-dot-net.md)。 

## <a name="end-to-end-workflow"></a>端對端工作流程
在此案例中，我們在 Azure SQL Database 中有一些想要複製到 Azure Synapse Analytics 的資料表。 以下是發生在管線中工作流程中步驟的邏輯順序：

![工作流程](media/tutorial-bulk-copy/tutorial-copy-multiple-tables.png)

* 第一個管線會查閱需要複製到接收資料存放區的資料表清單。  或者，您可以維護中繼資料資料表，其中列出要複製到接收資料存放區的所有資料表。 然後，管線會觸發另一個管線，它會逐一查看資料庫中的每個資料表，並執行資料複製作業。
* 第二個管線會執行實際的複製。 它會使用資料表的清單作為參數。 對於清單中的每個資料表，使用[透過 Blob 儲存體和 PolyBase 暫存複製](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-synapse-analytics)，將 Azure SQL Database 中的特定資料表複製到 Azure Synapse Analytics 中的對應資料表，可獲得最佳效能。 在此範例中，第一個管線會將資料表清單傳遞作為參數的值。 

如果您沒有 Azure 訂用帳戶，請在開始前建立[免費帳戶](https://azure.microsoft.com/free/)。

## <a name="prerequisites"></a>必要條件

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

* **Azure PowerShell**(英文)。 遵循[如何安裝並設定 Azure PowerShell](/powershell/azure/install-Az-ps) 中的指示。
* **Azure 儲存體帳戶**。 Azure 儲存體帳戶會在大量複製作業中用做暫存 Blob 儲存體。 
* **Azure SQL Database**。 此資料庫包含來源資料。 
* **Azure Synapse Analytics**。 此資料倉儲保存從 SQL Database 複製的資料。 

### <a name="prepare-sql-database-and-azure-synapse-analytics"></a>準備 SQL Database 和 Azure Synapse Analytics

**準備來源 Azure SQL Database**：

遵循[在 Azure SQL Database 中建立資料庫](../azure-sql/database/single-database-create-quickstart.md)一文，在 SQL Database 中建立具有 Adventure Works LT 範例資料的資料庫。 本教學課程會將此範例資料庫中的所有資料表複製到 Azure Synapse Analytics。

**準備接收 Azure Synapse Analytics**：

1. 如果您沒有 Azure Synapse Analytics 工作區，請參閱[開始使用 Azure Synapse Analytics](..\synapse-analytics\get-started.md) 一文，以取得其建立步驟。

2. 在 Azure Synapse Analytics 中建立對應的資料表結構描述。 在稍後步驟中，您可以使用 Azure Data Factory 來移轉/複製資料。

## <a name="azure-services-to-access-sql-server"></a>Azure 服務存取 SQL Server

對於 SQL Database 和 Azure Synapse Analytics，均應允許 Azure 服務存取 SQL Server。 確保伺服器的 [允許存取 Azure 服務] 設定已 [開啟]。 此設定可允許 Data Factory 服務從您的 Azure SQL Database 讀取資料，並將資料寫入至 Azure Synapse Analytics。 若要確認並開啟此設定，請執行下列步驟：

1. 按一下左邊的 [所有服務] 中樞，然後按一下 [SQL Server]。
2. 選取您的伺服器，然後按一下 [設定] 下的 [防火牆]。
3. 在 [防火牆設定] 頁面中，對 [允許存取 Azure 服務] 按一下 [開啟]。

## <a name="create-a-data-factory"></a>建立 Data Factory

1. 啟動 **PowerShell**。 將 Azure PowerShell 維持在開啟狀態，直到本教學課程結束為止。 如果您關閉並重新開啟，則需要再次執行這些命令。

    執行下列命令，並輸入您用來登入 Azure 入口網站的使用者名稱和密碼：
        
    ```powershell
    Connect-AzAccount
    ```
    執行下列命令以檢視此帳戶的所有訂用帳戶：

    ```powershell
    Get-AzSubscription
    ```
    執行下列命令以選取您要使用的訂用帳戶。 以您的 Azure 訂用帳戶識別碼取代 **SubscriptionId**：

    ```powershell
    Select-AzSubscription -SubscriptionId "<SubscriptionId>"
    ```
2. 執行 **Set-AzDataFactoryV2** Cmdlet 來建立資料處理站。 執行命令之前，以您自己的值取代預留位置。 

    ```powershell
    $resourceGroupName = "<your resource group to create the factory>"
    $dataFactoryName = "<specify the name of data factory to create. It must be globally unique.>"
    Set-AzDataFactoryV2 -ResourceGroupName $resourceGroupName -Location "East US" -Name $dataFactoryName
    ```

    請注意下列幾點：

    * Azure Data Factory 的名稱在全域必須是唯一的。 如果發生下列錯誤，請變更名稱，並再試一次。

        ```
        The specified Data Factory name 'ADFv2QuickStartDataFactory' is already in use. Data Factory names must be globally unique.
        ```

    * 若要建立 Data Factory 執行個體，您必須是 Azure 訂用帳戶的參與者或系統管理員。
    * 如需目前可使用 Data Factory 的 Azure 區域清單，請在下列頁面上選取您感興趣的區域，然後展開 [分析] 以找出 [Data Factory]：[依區域提供的產品](https://azure.microsoft.com/global-infrastructure/services/)。 資料處理站所使用的資料存放區 (Azure 儲存體、Azure SQL Database 等) 和計算 (HDInsight 等) 可位於其他區域。

## <a name="create-linked-services"></a>建立連結的服務

在本教學課程中，您已建立三個連結服務，分別是來源、接收和暫存 Blob，其包含與您的資料存放區的連線：

### <a name="create-the-source-azure-sql-database-linked-service"></a>建立來源 Azure SQL Database 連結服務

1. 在 **C:\ADFv2TutorialBulkCopy** 資料夾中，使用下列內容建立名為 **AzureSqlDatabaseLinkedService.json** 的 JSON 檔案：(如果 ADFv2TutorialBulkCopy 資料夾不存在，請加以建立。)

    > [!IMPORTANT]
    > 儲存檔案之前，以您的 Azure SQL Database 的值取代 &lt;servername&gt;、&lt;databasename&gt;、&lt;username&gt;@&lt;servername&gt; 和 &lt;password&gt;。

    ```json
    {
        "name": "AzureSqlDatabaseLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "typeProperties": {
                "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        }
    }
    ```

2. 在 **Azure PowerShell** 中，切換至 **ADFv2TutorialBulkCopy** 資料夾。

3. 執行 **Set-AzDataFactoryV2LinkedService** Cmdlet 來建立連結服務：**AzureSqlDatabaseLinkedService**。 

    ```powershell
    Set-AzDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "AzureSqlDatabaseLinkedService" -File ".\AzureSqlDatabaseLinkedService.json"
    ```

    以下是範例輸出：

    ```console
    LinkedServiceName : AzureSqlDatabaseLinkedService
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureSqlDatabaseLinkedService
    ```

### <a name="create-the-sink-azure-synapse-analytics-linked-service"></a>建立接收 Azure Synapse Analytics 連結服務

1. 在 **C:\ADFv2TutorialBulkCopy** 資料夾中，使用下列內容建立名為 **AzureSqlDWLinkedService.json** 的 JSON 檔案：

    > [!IMPORTANT]
    > 儲存檔案之前，以您的 Azure SQL Database 的值取代 &lt;servername&gt;、&lt;databasename&gt;、&lt;username&gt;@&lt;servername&gt; 和 &lt;password&gt;。

    ```json
    {
        "name": "AzureSqlDWLinkedService",
        "properties": {
            "type": "AzureSqlDW",
            "typeProperties": {
                "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        }
    }
    ```

2. 若要建立連結服務：**AzureSqlDWLinkedService**，請執行 **Set-AzDataFactoryV2LinkedService** Cmdlet。

    ```powershell
    Set-AzDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "AzureSqlDWLinkedService" -File ".\AzureSqlDWLinkedService.json"
    ```

    以下是範例輸出：

    ```console
    LinkedServiceName : AzureSqlDWLinkedService
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureSqlDWLinkedService
    ```

### <a name="create-the-staging-azure-storage-linked-service"></a>建立暫存 Azure 儲存體連結服務

在本教學課程中，您會使用 Azure Blob 儲存體作為暫時的暫存區域，讓 PolyBase 獲得更好的複製效能。

1. 在 **C:\ADFv2TutorialBulkCopy** 資料夾中，使用下列內容建立名為 **AzureStorageLinkedService.json** 的 JSON 檔案：

    > [!IMPORTANT]
    > 儲存檔案前，以 Azure 儲存體帳戶的名稱和金鑰取代 &lt;accountName&gt; 和 &lt;accountKey&gt;。

    ```json
    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountName>;AccountKey=<accountKey>"
            }
        }
    }
    ```

2. 若要建立連結服務：**AzureStorageLinkedService**，請執行 **Set-AzDataFactoryV2LinkedService** Cmdlet。

    ```powershell
    Set-AzDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "AzureStorageLinkedService" -File ".\AzureStorageLinkedService.json"
    ```

    以下是範例輸出：

    ```console
    LinkedServiceName : AzureStorageLinkedService
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureStorageLinkedService
    ```

## <a name="create-datasets"></a>建立資料集

在本教學課程中，您會建立來源和接收資料集，其指定儲存資料的位置：

### <a name="create-a-dataset-for-source-sql-database"></a>建立來源 SQL Database 的資料集

1. 在 **C:\ADFv2TutorialBulkCopy** 資料夾中，使用下列內容建立名為 **AzureSqlDatabaseDataset.json** 的 JSON 檔案。 "tableName" 虛擬的，因為稍後您會在複製活動中使用 SQL 查詢來取出資料。

    ```json
    {
        "name": "AzureSqlDatabaseDataset",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": {
                "referenceName": "AzureSqlDatabaseLinkedService",
                "type": "LinkedServiceReference"
            },
            "typeProperties": {
                "tableName": "dummy"
            }
        }
    }
    ```

2. 若要建立資料集：**AzureSqlDatabaseDataset**，請執行 **Set-AzDataFactoryV2Dataset** Cmdlet。

    ```powershell
    Set-AzDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "AzureSqlDatabaseDataset" -File ".\AzureSqlDatabaseDataset.json"
    ```

    以下是範例輸出：

    ```console
    DatasetName       : AzureSqlDatabaseDataset
    ResourceGroupName : <resourceGroupname>
    DataFactoryName   : <dataFactoryName>
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureSqlTableDataset
    ```

### <a name="create-a-dataset-for-sink-azure-synapse-analytics"></a>建立接收 Azure Synapse Analytics 的資料集

1. 在 **C:\ADFv2TutorialBulkCopy** 資料夾中 建立名為 **AzureSqlDWDataset.json** 的 JSON 檔案，加上下列內容："tableName" 已設定作為參數，稍後參考此資料集的複製活動會傳遞實際值至該資料集。

    ```json
    {
        "name": "AzureSqlDWDataset",
        "properties": {
            "type": "AzureSqlDWTable",
            "linkedServiceName": {
                "referenceName": "AzureSqlDWLinkedService",
                "type": "LinkedServiceReference"
            },
            "typeProperties": {
                "tableName": {
                    "value": "@{dataset().DWTableName}",
                    "type": "Expression"
                }
            },
            "parameters":{
                "DWTableName":{
                    "type":"String"
                }
            }
        }
    }
    ```

2. 若要建立資料集：**AzureSqlDWDataset**，請執行 **Set-AzDataFactoryV2Dataset** Cmdlet。

    ```powershell
    Set-AzDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "AzureSqlDWDataset" -File ".\AzureSqlDWDataset.json"
    ```

    以下是範例輸出：

    ```console
    DatasetName       : AzureSqlDWDataset
    ResourceGroupName : <resourceGroupname>
    DataFactoryName   : <dataFactoryName>
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureSqlDwTableDataset
    ```

## <a name="create-pipelines"></a>建立管線

在本教學課程中，您將建立兩個管線：

### <a name="create-the-pipeline-iterateandcopysqltables"></a>建立管線 "IterateAndCopySQLTables"

這個管線會使用資料表的清單作為參數。 對於清單中每的個資料表，其會使用暫存的複製和 PolyBase，將 Azure SQL Database 資料表中的資料複製到 Azure Synapse Analytics。

1. 在 **C:\ADFv2TutorialBulkCopy** 資料夾中，使用下列內容建立名為 **IterateAndCopySQLTables.json** 的 JSON 檔案：

    ```json
    {
        "name": "IterateAndCopySQLTables",
        "properties": {
            "activities": [
                {
                    "name": "IterateSQLTables",
                    "type": "ForEach",
                    "typeProperties": {
                        "isSequential": "false",
                        "items": {
                            "value": "@pipeline().parameters.tableList",
                            "type": "Expression"
                        },
                        "activities": [
                            {
                                "name": "CopyData",
                                "description": "Copy data from Azure SQL Database to Azure Synapse Analytics",
                                "type": "Copy",
                                "inputs": [
                                    {
                                        "referenceName": "AzureSqlDatabaseDataset",
                                        "type": "DatasetReference"
                                    }
                                ],
                                "outputs": [
                                    {
                                        "referenceName": "AzureSqlDWDataset",
                                        "type": "DatasetReference",
                                        "parameters": {
                                            "DWTableName": "[@{item().TABLE_SCHEMA}].[@{item().TABLE_NAME}]"
                                        }
                                    }
                                ],
                                "typeProperties": {
                                    "source": {
                                        "type": "SqlSource",
                                        "sqlReaderQuery": "SELECT * FROM [@{item().TABLE_SCHEMA}].[@{item().TABLE_NAME}]"
                                    },
                                    "sink": {
                                        "type": "SqlDWSink",
                                        "preCopyScript": "TRUNCATE TABLE [@{item().TABLE_SCHEMA}].[@{item().TABLE_NAME}]",
                                        "allowPolyBase": true
                                    },
                                    "enableStaging": true,
                                    "stagingSettings": {
                                        "linkedServiceName": {
                                            "referenceName": "AzureStorageLinkedService",
                                            "type": "LinkedServiceReference"
                                        }
                                    }
                                }
                            }
                        ]
                    }
                }
            ],
            "parameters": {
                "tableList": {
                    "type": "Object"
                }
            }
        }
    }
    ```

2. 若要建立管線：**IterateAndCopySQLTables**，請執行 **Set-AzDataFactoryV2Pipeline** Cmdlet。

    ```powershell
    Set-AzDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "IterateAndCopySQLTables" -File ".\IterateAndCopySQLTables.json"
    ```

    以下是範例輸出：

    ```console
    PipelineName      : IterateAndCopySQLTables
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    Activities        : {IterateSQLTables}
    Parameters        : {[tableList, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification]}
    ```

### <a name="create-the-pipeline-gettablelistandtriggercopydata"></a>建立管線 "GetTableListAndTriggerCopyData"

這個管線會執行兩個步驟：

* 請查閱 Azure SQL Database 系統資料表，以取得要複製的資料表清單。
* 觸發管線 "IterateAndCopySQLTables" 以進行實際的資料複製。

1. 在 **C:\ADFv2TutorialBulkCopy** 資料夾中，使用下列內容建立名為 **GetTableListAndTriggerCopyData.json** 的 JSON 檔案：

    ```json
    {
        "name":"GetTableListAndTriggerCopyData",
        "properties":{
            "activities":[
                { 
                    "name": "LookupTableList",
                    "description": "Retrieve the table list from Azure SQL dataabse",
                    "type": "Lookup",
                    "typeProperties": {
                        "source": {
                            "type": "SqlSource",
                            "sqlReaderQuery": "SELECT TABLE_SCHEMA, TABLE_NAME FROM information_schema.TABLES WHERE TABLE_TYPE = 'BASE TABLE' and TABLE_SCHEMA = 'SalesLT' and TABLE_NAME <> 'ProductModel'"
                        },
                        "dataset": {
                            "referenceName": "AzureSqlDatabaseDataset",
                            "type": "DatasetReference"
                        },
                        "firstRowOnly": false
                    }
                },
                {
                    "name": "TriggerCopy",
                    "type": "ExecutePipeline",
                    "typeProperties": {
                        "parameters": {
                            "tableList": {
                                "value": "@activity('LookupTableList').output.value",
                                "type": "Expression"
                            }
                        },
                        "pipeline": {
                            "referenceName": "IterateAndCopySQLTables",
                            "type": "PipelineReference"
                        },
                        "waitOnCompletion": true
                    },
                    "dependsOn": [
                        {
                            "activity": "LookupTableList",
                            "dependencyConditions": [
                                "Succeeded"
                            ]
                        }
                    ]
                }
            ]
        }
    }
    ```

2. 若要建立管線：**GetTableListAndTriggerCopyData**，請執行 **Set-AzDataFactoryV2Pipeline** Cmdlet。

    ```powershell
    Set-AzDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "GetTableListAndTriggerCopyData" -File ".\GetTableListAndTriggerCopyData.json"
    ```

    以下是範例輸出：

    ```console
    PipelineName      : GetTableListAndTriggerCopyData
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    Activities        : {LookupTableList, TriggerCopy}
    Parameters        :
    ```

## <a name="start-and-monitor-pipeline-run"></a>啟動及監視管線執行

1. 啟動主要 "GetTableListAndTriggerCopyData" 管線的管線執行，並擷取管線執行識別碼，方便後續監視。 在底下，它會觸發管線 "IterateAndCopySQLTables" 的執行，如 ExecutePipeline 活動中所指定。

    ```powershell
    $runId = Invoke-AzDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineName 'GetTableListAndTriggerCopyData'
    ```

2.  執行下列程式碼，持續檢查管線 **GetTableListAndTriggerCopyData** 的執行狀態，並列印出最終的管線執行和活動執行結果。

    ```powershell
    while ($True) {
        $run = Get-AzDataFactoryV2PipelineRun -ResourceGroupName $resourceGroupName -DataFactoryName $DataFactoryName -PipelineRunId $runId

        if ($run) {
            if ($run.Status -ne 'InProgress') {
                Write-Host "Pipeline run finished. The status is: " $run.Status -foregroundcolor "Yellow"
                Write-Host "Pipeline run details:" -foregroundcolor "Yellow"
                $run
                break
            }
            Write-Host  "Pipeline is running...status: InProgress" -foregroundcolor "Yellow"
        }

        Start-Sleep -Seconds 15
    }

    $result = Get-AzDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $runId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)
    Write-Host "Activity run details:" -foregroundcolor "Yellow"
    $result
    ```

    執行範例的輸出如下：

    ```console
    Pipeline run details:
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    RunId             : 0000000000-00000-0000-0000-000000000000
    PipelineName      : GetTableListAndTriggerCopyData
    LastUpdated       : 9/18/2017 4:08:15 PM
    Parameters        : {}
    RunStart          : 9/18/2017 4:06:44 PM
    RunEnd            : 9/18/2017 4:08:15 PM
    DurationInMs      : 90637
    Status            : Succeeded
    Message           : 

    Activity run details:
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    ActivityName      : LookupTableList
    PipelineRunId     : 0000000000-00000-0000-0000-000000000000
    PipelineName      : GetTableListAndTriggerCopyData
    Input             : {source, dataset, firstRowOnly}
    Output            : {count, value, effectiveIntegrationRuntime}
    LinkedServiceName : 
    ActivityRunStart  : 9/18/2017 4:06:46 PM
    ActivityRunEnd    : 9/18/2017 4:07:09 PM
    DurationInMs      : 22995
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}

    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    ActivityName      : TriggerCopy
    PipelineRunId     : 0000000000-00000-0000-0000-000000000000
    PipelineName      : GetTableListAndTriggerCopyData
    Input             : {pipeline, parameters, waitOnCompletion}
    Output            : {pipelineRunId}
    LinkedServiceName : 
    ActivityRunStart  : 9/18/2017 4:07:11 PM
    ActivityRunEnd    : 9/18/2017 4:08:14 PM
    DurationInMs      : 62581
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    ```

3. 您可以取得管線 "**IterateAndCopySQLTables**" 的執行識別碼，並檢查詳細的活動執行結果，如下所示。

    ```powershell
    Write-Host "Pipeline 'IterateAndCopySQLTables' run result:" -foregroundcolor "Yellow"
    ($result | Where-Object {$_.ActivityName -eq "TriggerCopy"}).Output.ToString()
    ```

    執行範例的輸出如下：

    ```json
    {
        "pipelineRunId": "7514d165-14bf-41fb-b5fb-789bea6c9e58"
    }
    ```

    ```powershell
    $result2 = Get-AzDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId <copy above run ID> -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)
    $result2
    ```

3. 連線至接收 Azure Synapse Analytics，並確認已從 Azure SQL Database 正確複製資料。

## <a name="next-steps"></a>後續步驟
在本教學課程中，您已執行下列步驟： 

> [!div class="checklist"]
> * 建立資料處理站。
> * 建立 Azure SQL Database、Azure Synapse Analytics 和 Azure 儲存體連結服務。
> * 建立 Azure SQL Database 和 Azure Synapse Analytics 資料集。
> * 建立管線來查詢要複製的資料表和其他管線，以執行實際的複製作業。 
> * 啟動管線執行。
> * 監視管線和活動執行。

進入下列教學課程，以了解如何將資料累加從來源複製到目的地：
> [!div class="nextstepaction"]
>[以累加方式複製資料](tutorial-incremental-copy-powershell.md)
