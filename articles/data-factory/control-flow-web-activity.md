---
title: Azure Data Factory 中的 Web 活動
description: 了解如何使用 Web 活動 (Data Factory 支援的其中一個控制流程活動) 從管線叫用 REST 端點。
services: data-factory
documentationcenter: ''
author: dcstwh
ms.author: weetok
manager: jroth
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 12/19/2018
ms.openlocfilehash: fbe37152f4ff1ce24754bc2d7b968c8e1c76ca10
ms.sourcegitcommit: ea17e3a6219f0f01330cf7610e54f033a394b459
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97387712"
---
# <a name="web-activity-in-azure-data-factory"></a>Azure Data Factory 中的 Web 活動
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]


使用 Web 活動可以從 Data Factory 管線呼叫自訂的 REST 端點。 您可以傳遞資料集和連結服務，以供活動取用和存取。

> [!NOTE]
> Web 活動也支援用於叫用裝載於私人虛擬網路的 URL，方法是利用自我裝載整合執行階段。 整合執行階段應該要能看到 URL 端點。 

> [!NOTE]
> 支援的輸出回應承載大小上限為 4 MB。  

## <a name="syntax"></a>語法

```json
{
   "name":"MyWebActivity",
   "type":"WebActivity",
   "typeProperties":{
      "method":"Post",
      "url":"<URLEndpoint>",
      "connectVia": {
          "referenceName": "<integrationRuntimeName>",
          "type": "IntegrationRuntimeReference"
      }
      "headers":{
         "Content-Type":"application/json"
      },
      "authentication":{
         "type":"ClientCertificate",
         "pfx":"****",
         "password":"****"
      },
      "datasets":[
         {
            "referenceName":"<ConsumedDatasetName>",
            "type":"DatasetReference",
            "parameters":{
               ...
            }
         }
      ],
      "linkedServices":[
         {
            "referenceName":"<ConsumedLinkedServiceName>",
            "type":"LinkedServiceReference"
         }
      ]
   }
}

```

## <a name="type-properties"></a>類型屬性

屬性 | 描述 | 允許的值 | 必要
-------- | ----------- | -------------- | --------
NAME | Web 活動的名稱 | String | 是
type | 必須設定為 **WebActivity**。 | String | 是
method | 目標端點的 Rest API 方法。 | 字串。 <br/><br/>支援的類型："GET"、"POST"、"PUT" | 是
url | 目標端點和路徑 | 字串 (或含有字串之 resultType 的運算式)。 如果活動未在 1 分鐘內收到來自端點的回應，就會發生逾時並出現錯誤。 | 是
headers | 傳送至要求的標頭。 例如，若要對要求設定語言和類型︰`"headers" : { "Accept-Language": "en-us", "Content-Type": "application/json" }`。 | 字串 (或含有字串之 resultType 的運算式) | 是，Content-type 標頭是必要的。 `"headers":{ "Content-Type":"application/json"}`
body | 代表傳送至端點的承載。  | 字串 (或含有字串之 resultType 的運算式)。 <br/><br/>請在[要求乘載結構描述](#request-payload-schema)一節中查看要求乘載的結構描述。 | POST/PUT 方法的必要項。
驗證 (authentication) | 呼叫端點所使用的驗證方法。 支援的類型為「基本」或 ClientCertificate。 如需詳細資訊，請參閱[驗證](#authentication)一節。 如果不需要驗證，請排除這個屬性。 | 字串 (或含有字串之 resultType 的運算式) | 否
datasets | 傳遞至端點的資料集清單。 | 資料集參考的陣列。 可以是空陣列。 | 是
linkedServices | 傳遞至端點的連結服務清單。 | 連結服務參考的陣列。 可以是空陣列。 | 是
connectVia | 用來連線到資料存放區的[整合執行階段](./concepts-integration-runtime.md)。 如果您的資料存放區位於私人網路) ，您可以使用 Azure integration runtime 或自我裝載整合執行時間 (。 如果未指定此屬性，服務會使用預設的 Azure integration runtime。 | Integration runtime 參考。 | 否 

> [!NOTE]
> Web 活動叫用的 REST 端點必須傳回 JSON 類型的回應。 如果活動未在 1 分鐘內收到來自端點的回應，就會發生逾時並出現錯誤。

下表顯示 JSON 內容的需求：

| 值類型 | 要求本文 | 回應本文 |
|---|---|---|
|JSON 物件 | 支援 | 支援 |
|JSON 陣列 | 支援 <br/>(目前，JSON 陣列因為錯誤的結果無法運作。 正在執行修正。) | 不支援 |
| JSON 值 | 支援 | 不支援 |
| 非 JSON 型別 | 不支援 | 不支援 |
||||

## <a name="authentication"></a>驗證

以下是 web 活動中支援的驗證類型。

### <a name="none"></a>無

如果不需要驗證，請勿包含 authentication 屬性。

### <a name="basic"></a>基本

指定要搭配基本驗證使用的使用者名稱和密碼。

```json
"authentication":{
   "type":"Basic",
   "username":"****",
   "password":"****"
}
```

### <a name="client-certificate"></a>用戶端憑證

指定以 base64 編碼的 PFX 檔案和密碼內容。

```json
"authentication":{
   "type":"ClientCertificate",
   "pfx":"****",
   "password":"****"
}
```

### <a name="managed-identity"></a>受控識別

將使用資料處理站的受控身分識別指定為其要求存取權杖的資源 URI。 若要呼叫 Azure 資源管理 API，請使用 `https://management.azure.com/`。 如需受控識別如何運作的詳細資訊，請參閱 [Azure 資源的受控識別](../active-directory/managed-identities-azure-resources/overview.md)概觀頁面。

```json
"authentication": {
    "type": "MSI",
    "resource": "https://management.azure.com/"
}
```

> [!NOTE]
> 如果您的 data factory 是以 git 存放庫設定，則您必須將認證儲存在 Azure Key Vault 中，才能使用基本或用戶端憑證驗證。 Azure Data Factory 不會在 git 中儲存密碼。

## <a name="request-payload-schema"></a>要求承載結構描述
當您使用 POST/PUT 方法時，主體屬性代表傳送至端點的承載。 您可以將連結服務和資料集傳遞為承載的一部分。 以下是承載的結構描述：

```json
{
    "body": {
        "myMessage": "Sample",
        "datasets": [{
            "name": "MyDataset1",
            "properties": {
                ...
            }
        }],
        "linkedServices": [{
            "name": "MyStorageLinkedService1",
            "properties": {
                ...
            }
        }]
    }
}
```

## <a name="example"></a>範例
在此範例中，管線中的 Web 活動會呼叫 REST 端點。 它會將 Azure SQL 連結服務和 Azure SQL 資料集傳遞至端點。 REST 端點會使用 Azure SQL 連接字串連接到邏輯 SQL server，並傳回 SQL server 實例的名稱。

### <a name="pipeline-definition"></a>管線定義

```json
{
    "name": "<MyWebActivityPipeline>",
    "properties": {
        "activities": [
            {
                "name": "<MyWebActivity>",
                "type": "WebActivity",
                "typeProperties": {
                    "method": "Post",
                    "url": "@pipeline().parameters.url",
                    "headers": {
                        "Content-Type": "application/json"
                    },
                    "authentication": {
                        "type": "ClientCertificate",
                        "pfx": "*****",
                        "password": "*****"
                    },
                    "datasets": [
                        {
                            "referenceName": "MySQLDataset",
                            "type": "DatasetReference",
                            "parameters": {
                                "SqlTableName": "@pipeline().parameters.sqlTableName"
                            }
                        }
                    ],
                    "linkedServices": [
                        {
                            "referenceName": "SqlLinkedService",
                            "type": "LinkedServiceReference"
                        }
                    ]
                }
            }
        ],
        "parameters": {
            "sqlTableName": {
                "type": "String"
            },
            "url": {
                "type": "String"
            }
        }
    }
}

```

### <a name="pipeline-parameter-values"></a>管線參數值

```json
{
    "sqlTableName": "department",
    "url": "https://adftes.azurewebsites.net/api/execute/running"
}

```

### <a name="web-service-endpoint-code"></a>Web 服務端點程式碼

```csharp

[HttpPost]
public HttpResponseMessage Execute(JObject payload)
{
    Trace.TraceInformation("Start Execute");

    JObject result = new JObject();
    result.Add("status", "complete");

    JArray datasets = payload.GetValue("datasets") as JArray;
    result.Add("sinktable", datasets[0]["properties"]["typeProperties"]["tableName"].ToString());

    JArray linkedServices = payload.GetValue("linkedServices") as JArray;
    string connString = linkedServices[0]["properties"]["typeProperties"]["connectionString"].ToString();

    System.Data.SqlClient.SqlConnection sqlConn = new System.Data.SqlClient.SqlConnection(connString);

    result.Add("sinkServer", sqlConn.DataSource);

    Trace.TraceInformation("Stop Execute");

    return this.Request.CreateResponse(HttpStatusCode.OK, result);
}

```

## <a name="next-steps"></a>後續步驟
請參閱 Data Factory 支援的其他控制流程活動：

- [執行管線活動](control-flow-execute-pipeline-activity.md)
- [For Each 活動](control-flow-for-each-activity.md)
- [取得中繼資料活動](control-flow-get-metadata-activity.md)
- [查閱活動](control-flow-lookup-activity.md)
