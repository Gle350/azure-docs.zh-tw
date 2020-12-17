---
title: 內嵌歷程記錄遙測資料
description: 本文說明如何內嵌歷程記錄遙測資料。
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.custom: has-adal-ref
ms.openlocfilehash: 603f14d2076b5b74dde0b92a732f8fe816f6dd10
ms.sourcegitcommit: ad677fdb81f1a2a83ce72fa4f8a3a871f712599f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/17/2020
ms.locfileid: "97656779"
---
# <a name="ingest-historical-telemetry-data"></a>內嵌歷程記錄遙測資料

本文說明如何將歷程記錄感應器資料內嵌至 Azure FarmBeats。

擷取 (IoT) 資源（例如裝置和感應器）的物聯網歷程記錄資料是 FarmBeats 中的常見案例。 您可以建立裝置和感應器的中繼資料，然後將歷程記錄資料內嵌至以標準格式 FarmBeats 的資料。

## <a name="before-you-begin"></a>開始之前

繼續進行本文之前，請確定您已安裝 FarmBeats，並已從 IoT 裝置收集歷程記錄資料。 您也必須啟用合作夥伴存取，如下列步驟所述。

## <a name="enable-partner-access"></a>啟用合作夥伴存取

您必須啟用 Azure FarmBeats 實例的夥伴整合。 此步驟會建立用戶端，該用戶端可以存取您的 Azure FarmBeats 實例作為裝置合作夥伴，並提供後續步驟中所需的下列值：

- API 端點：這是 Datahub URL，例如 HTTPs:// \<datahub> . azurewebsites.net
- 租用戶識別碼
- 用戶端識別碼
- 用戶端密碼
- EventHub 連接字串

請遵循下列步驟：

> [!NOTE]
> 您必須是系統管理員，才能執行下列步驟。

1. 登入 https://portal.azure.com/。

2. **如果您是在 FarmBeats 版本1.2.7 或更新版本上，請略過步驟 a、b 和 c，然後移至步驟3。** 您可以選取 FarmBeats UI 右上角的 [ **設定** ] 圖示來檢查 FarmBeats 版本。

      a.  移至 **Azure Active Directory**  >  **應用程式註冊**

      b. 選取在 FarmBeats 部署過程中建立的 **應用程式註冊** 。 它會有與您的 FarmBeats datahub 相同的名稱。

      c. 選取 [ **公開 API** ] > 選取 [ **新增用戶端應用程式** ]，然後輸入 **04b07795-8ddb-461a-bbee-02f9e1bf7b46** 並檢查 **授權範圍**。 這會提供 Azure CLI (Cloud Shell) 的存取權，以執行下列步驟：

3. 開啟 Cloud Shell。 此選項可在 Azure 入口網站右上角的工具列上取得。

    ![Azure 入口網站工具列](./media/get-drone-imagery-from-drone-partner/navigation-bar-1.png)

4. 確定環境已設定為 **PowerShell**。 依預設，它會設定為 Bash。

    ![PowerShell 工具列設定](./media/get-sensor-data-from-sensor-partner/power-shell-new-1.png)

5. 移至您的主目錄。

    ```azurepowershell-interactive
    cd
    ```

6. 執行下列命令。 這會連接已驗證的帳戶以用於 Azure AD 要求

    ```azurepowershell-interactive
    Connect-AzureAD
    ```

7. 執行下列命令。 這會將腳本下載到您的主目錄。

    ```azurepowershell-interactive 

    wget –q https://aka.ms/farmbeatspartnerscriptv3 -O ./generatePartnerCredentials.ps1

    ```

8. 執行下列指令碼。 腳本會要求租使用者識別碼，您可以從 **Azure Active Directory**  >  **總覽** 頁面取得該識別碼。

    ```azurepowershell-interactive

    ./generatePartnerCredentials.ps1

    ```

9. 遵循畫面上的指示來捕捉 **API 端點**、租使用者 **識別碼**、 **用戶端識別碼**、 **用戶端密碼** 和 **EventHub 連接字串** 的值。


## <a name="create-device-or-sensor-metadata"></a>建立裝置或感應器中繼資料

 現在您已擁有必要的認證，您可以定義裝置和感應器。 若要這樣做，請呼叫 FarmBeats Api 來建立中繼資料。 請務必以您在上一節中建立的用戶端應用程式來呼叫 Api。

 FarmBeats Datahub 具有下列 Api，可讓您建立及管理裝置或感應器中繼資料。

 > [!NOTE]
 > 作為合作夥伴，您只能存取讀取、建立和更新中繼資料; **只有夥伴才能刪除選項。**

- /**DeviceModel**： DeviceModel 對應至裝置的中繼資料，例如製造商和裝置類型，也就是閘道或節點。
- /**Device**：Device 會對應至存在於伺服器陣列上的實體裝置。
- /**SensorModel**： SensorModel 對應至感應器的中繼資料，例如製造商、感應器類型、類比或數位，以及感應器測量，例如環境溫度和壓力。
- /**Sensor**：Sensor 會對應至記錄值的實體感應器。 感應器通常會使用裝置識別碼連線到裝置。

| DeviceModel | 建議 |
|--|--|
| Type (Node、Gateway) | 裝置節點或閘道的類型 |
| 製造商 | 製造商的名稱 |
| ProductCode | 裝置產品代碼，或是型號名稱或號碼。 例如，EnviroMonitor#6800。 |
| 連接埠 | 連接埠名稱和類型 (數位或類比)。 |
| 名稱 | 用於識別資源的名稱。 例如，型號名稱或產品名稱。 |
| 描述 | 提供有意義的型號描述。 |
| 屬性 | 製造商的其他屬性。 |
| **裝置** |  |
| DeviceModelId | 相關裝置型號的識別碼。 |
| HardwareId | 裝置的唯一識別碼，例如 MAC 位址。 |
| ReportingInterval | 報告間隔 (秒)。 |
| Location | 裝置緯度 (-90 到 +90)、經度 (-180 到 180) 和高度 (公尺)。 |
| ParentDeviceId | 此裝置所連線父裝置的識別碼。 例如，連接到閘道的節點。 節點以閘道的形式 parentDeviceId。 |
| 名稱 | 用來識別資源的名稱。 裝置夥伴必須傳送與合作夥伴端上的裝置名稱一致的名稱。 如果夥伴裝置名稱是使用者定義的，則必須將相同的使用者定義名稱傳播至 FarmBeats。 |
| 描述 | 提供有意義的描述。 |
| 屬性 | 製造商的其他屬性。 |
| **SensorModel** |  |
| Type (Analog、Digital) | 感應器的類型，不論是類比或數位。 |
| 製造商 | 感應器的製造商。 |
| ProductCode | 產品代碼，或型號名稱或號碼。 例如，RS-CO2-N01。 |
| SensorMeasures > Name | 感應器量值的名稱。 僅支援小寫。 如需不同深度的度量，請指定深度。 例如，soil_moisture_15cm。 此名稱必須與遙測資料一致。 |
| SensorMeasures > DataType | 遙測資料類型。 目前支援 Double。 |
| SensorMeasures > Type | 感應器遙測資料的度量類型。 系統定義的類型為 AmbientTemperature、CO2、Depth、ElectricalConductivity、LeafWetness、Length、LiquidLevel、Nitrate、O2、PH、Phosphate、PointInTime、Potassium、壓力、RainGauge、RelativeHumidity、Salinity、SoilMoisture、SoilTemperature、SolarRadiation、State、TimeDuration、UVRadiation、UVIndex、WindDirection、WindRun、WindSpeed、Evapotranspiration、、、、、、、、、、 若要新增更多，請參閱 /ExtendedType API。 |
| SensorMeasures > Unit | 感應器遙測資料的單位。 系統定義的單位為 NoUnit、攝氏、華氏、開氏、Rankine、Pascal、水星、PSI、毫米、平方公里、MilesPerHour、MilesPerSecond、KMPerHour、KMPerSecond、MetersPerHour、MetersPerSecond、度數、WattsPerSquareMeter、KiloWattsPerSquareMeter、MilliWattsPerSquareCentiMeter、MilliJoulesPerSquareCentiMeter、VolumetricWaterContent、PartsPerMillion、MicroMol、MicroMolesPerLiter、SiemensPerSquareMeterPerMole、MilliSiemensPerCentiMeter、Centibar、DeciSiemensPerMeter、KiloPascal、VolumetricIonContent、MilliLiter、UnixTimestamp、MicroMolPerMeterSquaredPerSecond、InchesPerHour、/ExtendedType、、、Seconds、、、來新增更多，請參閱 API。 |
| SensorMeasures > AggregationType | 值可以是無、平均、最大值、最小值或 List.standarddeviation。 |
| 名稱 | 用來識別資源的名稱。 例如，型號名稱或產品名稱。 |
| 描述 | 提供有意義的型號描述。 |
| 屬性 | 製造商的其他屬性。 |
| **Sensor** |  |
| HardwareId | 製造商所設定感應器的唯一識別碼。 |
| SensorModelId | 相關感應器型號的識別碼。 |
| Location | 感應器緯度 (-90 到 +90)、經度 (-180 到 180) 和高度 (公尺)。 |
| Port > Name | 感應器所連線裝置上的連接埠名稱和類型。 這必須與裝置模型中所定義的名稱相同。 |
| DeviceID | 感應器所連線裝置的識別碼。 |
| 名稱 | 用於識別資源的名稱。 例如，感應器名稱、產品名稱、型號或產品代碼。 |
| 描述 | 提供有意義的描述。 |
| 屬性 | 製造商的其他屬性。 |

如需物件的詳細資訊，請參閱 [Swagger](https://aka.ms/FarmBeatsDatahubSwagger)。

### <a name="api-request-to-create-metadata"></a>用來建立中繼資料的 API 要求

若要提出 API 要求，請將 HTTP (POST) 方法、API 服務的 URL，以及要查詢的資源 URI、提交資料給、建立或刪除要求。 然後，新增一或多個 HTTP 要求標頭。 API 服務的 URL 是 API 端點，也就是 Datahub URL (HTTPs:// \<yourdatahub> . azurewebsites.net) 。

### <a name="authentication"></a>驗證

FarmBeats Datahub 會使用持有人驗證，這需要在上一節中產生的下列認證：

- 用戶端識別碼
- 用戶端密碼
- 租用戶識別碼

使用這些認證，呼叫端就可以要求存取權杖。 您必須在標頭區段中的後續 API 要求中傳送權杖，如下所示：

```
headers = *{"Authorization": "Bearer " + access_token, …}*
```

下列 Python 程式碼範例提供存取權杖，可用於對 FarmBeats 進行後續的 API 呼叫： 

```python
import requests
import json
import msal

# Your service principal App ID
CLIENT_ID = "<CLIENT_ID>"
# Your service principal password
CLIENT_SECRET = "<CLIENT_SECRET>"
# Tenant ID for your Azure subscription
TENANT_ID = "<TENANT_ID>"

AUTHORITY_HOST = 'https://login.microsoftonline.com'
AUTHORITY = AUTHORITY_HOST + '/' + TENANT_ID

ENDPOINT = "https://<yourfarmbeatswebsitename-api>.azurewebsites.net"
SCOPE = ENDPOINT + "/.default"

context = msal.ConfidentialClientApplication(CLIENT_ID, authority=AUTHORITY, client_credential=CLIENT_SECRET)
token_response = context.acquire_token_for_client(SCOPE)
# We should get an access token here
access_token = token_response.get('access_token')
```

**HTTP 要求標頭**

以下是您對 FarmBeats Datahub 進行 API 呼叫時，必須指定的最常見要求標頭：

- **Content-type**： application/json
- **授權**：持有者 <Access-Token>
- **Accept**： application/json

### <a name="input-payload-to-create-metadata"></a>用來建立中繼資料的輸入承載

DeviceModel


```json
{
  "type": "Node",
  "manufacturer": "string",
  "productCode": "string",
  "ports": [
    {
      "name": "string",
      "type": "Analog"
    }
  ],
  "name": "string",
  "description": "string",
  "properties": {
    "additionalProp1": {},
    "additionalProp2": {},
    "additionalProp3": {}
  }
}
```

裝置

```json
{
  "deviceModelId": "string",
  "hardwareId": "string",
  "farmId": "string",
  "reportingInterval": 0,
  "location": {
    "latitude": 0,
    "longitude": 0,
    "elevation": 0
  },
  "parentDeviceId": "string",
  "name": "string",
  "description": "string",
  "properties": {
    "additionalProp1": {},
    "additionalProp2": {},
    "additionalProp3": {}
  }
}
```

SensorModel

```json
{
  "type": "Analog",
  "manufacturer": "string",
  "productCode": "string",
  "sensorMeasures": [
    {
      "name": "string",
      "dataType": "Double",
      "type": "string",
      "unit": "string",
      "aggregationType": "None",
      "depth": 0,
      "description": "string"
    }
  ],
  "name": "string",
  "description": "string",
  "properties": {
    "additionalProp1": {},
    "additionalProp2": {},
    "additionalProp3": {}
  }
}

```
Sensor

```json
{
  "hardwareId": "string",
  "sensorModelId": "string",
  "location": {
    "latitude": 0,
    "longitude": 0,
    "elevation": 0
  },
  "depth": 0,
  "port": {
    "name": "string",
    "type": "Analog"
  },
  "deviceId": "string",
  "name": "string",
  "description": "string",
  "properties": {
    "additionalProp1": {},
    "additionalProp2": {},
    "additionalProp3": {}
  }
}
```

下列範例要求會建立裝置。 此要求的輸入 JSON 為具有要求主體的承載。

```bash
curl -X POST "https://<datahub>.azurewebsites.net/Device" -H
"accept: application/json" -H  "Content-Type: application/json" -H
"Authorization: Bearer <Access-Token>" -d "{  \"deviceModelId\": \"ID123\",  \"hardwareId\": \"MHDN123\",
\"reportingInterval\": 900,  \"name\": \"Device123\",
\"description\": \"Test Device 123\"}" *
```

以下是 Python 中的範例程式碼。 此範例中使用的存取權杖與驗證期間所收到的相同。

```python
import requests
import json

# Got access token - Calling the Device Model API

headers = {
    "Authorization": "Bearer " + access_token,
    "Content-Type" : "application/json"
    }
payload = '{"type" : "Node", "productCode" : "TestCode", "ports": [{"name": "port1","type": "Analog"}], "name" : "DummyDevice"}'
response = requests.post(ENDPOINT + "/DeviceModel", data=payload, headers=headers)
```

> [!NOTE]
> 這些 API 會傳回所建立每個執行個體的唯一識別碼。 您必須保留識別碼以傳送對應的遙測訊息。

### <a name="send-telemetry"></a>傳送遙測

現在您已在 FarmBeats 中建立裝置和感應器，接下來您可以傳送相關聯的遙測訊息。

### <a name="create-a-telemetry-client"></a>建立遙測用戶端

您必須將遙測資料傳送至 Azure 事件中樞進行處理。 Azure 事件中樞是一項服務，可從連線的裝置和應用程式擷取即時資料 (遙測)。 若要將遙測資料傳送至 FarmBeats，請建立用戶端，以將訊息傳送至 FarmBeats 中的事件中樞。 如需有關傳送遙測的詳細資訊，請參閱 [Azure 事件中樞](../../event-hubs/event-hubs-dotnet-standard-getstarted-send.md)。

### <a name="send-a-telemetry-message-as-the-client"></a>以用戶端的形式傳送遙測訊息

當您建立連線作為事件中樞用戶端之後，您可以將訊息傳送至事件中樞做為 JSON。

以下是以用戶端的形式將遙測傳送至指定事件中樞的範例 Python 程式碼：

```python
import azure
from azure.eventhub import EventHubClient, Sender, EventData, Receiver, Offset
EVENTHUBCONNECTIONSTRING = "<EventHub Connection String provided by customer>"
EVENTHUBNAME = "<EventHub Name provided by customer>"

write_client = EventHubClient.from_connection_string(EVENTHUBCONNECTIONSTRING, eventhub=EVENTHUBNAME, debug=False)
sender = write_client.add_sender(partition="0")
write_client.run()
for i in range(5):
    telemetry = "<Canonical Telemetry message>"
    print("Sending telemetry: " + telemetry)
    sender.send(EventData(telemetry))
write_client.stop()

```

將歷程感應器資料格式轉換為 Azure FarmBeats 瞭解的標準格式。 標準訊息格式如下所示：

```json
{
"deviceid": "<id of the Device created>",
"timestamp": "<timestamp in ISO 8601 format>",
"version" : "1",
"sensors": [
    {
      "id": "<id of the sensor created>",
      "sensordata": [
        {
          "timestamp": "< timestamp in ISO 8601 format >",
          "<sensor measure name (as defined in the Sensor Model)>": <value>
        },
        {
          "timestamp": "<timestamp in ISO 8601 format>",
          "<sensor measure name (as defined in the Sensor Model)>": <value>
        }
      ]
    }
 ]
}
```

新增對應的裝置和感應器之後，請依照上一節所述，取得遙測訊息中的裝置識別碼和感應器識別碼。

以下是遙測訊息的範例：


 ```json
{
  "deviceid": "7f9b4b92-ba45-4a1d-a6ae-c6eda3a5bd12",
  "timestamp": "2019-06-22T06:55:02.7279559Z",
  "version": "1",
  "sensors": [
    {
      "id": "d8e7beb4-72de-4eed-9e00-45e09043a0b3",
      "sensordata": [
        {
          "timestamp": "2019-06-22T06:55:02.7279559Z",
          "hum_max": 15
        },
        {
          "timestamp": "2019-06-22T06:55:02.7279559Z",
          "hum_min": 42
        }
      ]
    },
    {
      "id": "d8e7beb4-72de-4eed-9e00-45e09043a0b3",
      "sensordata": [
        {
          "timestamp": "2019-06-22T06:55:02.7279559Z",
          "hum_max": 20
        },
        {
          "timestamp": "2019-06-22T06:55:02.7279559Z",
          "hum_min": 89
        }
      ]
    }
  ]
}
```

## <a name="troubleshooting"></a>疑難排解

### <a name="cant-view-telemetry-data-after-ingesting-historicalstreaming-data-from-your-sensors"></a>從感應器內嵌歷程/串流資料之後，無法檢視遙測資料

**徵兆**：裝置或感應器已部署，而且您已在 FarmBeats 上建立裝置/感應器，並將遙測內嵌至 EventHub，但無法取得或檢視 FarmBeats 上的遙測資料。

**矯正措施**：

1. 確定您已完成適當的夥伴註冊-您可以移至您的 datahub swagger、流覽至/Partner API、執行 Get 並檢查夥伴是否已註冊，以進行檢查。 如果沒有，請遵循 [此處的步驟](get-sensor-data-from-sensor-partner.md#enable-device-integration-with-farmbeats) 來新增夥伴。

2. 確定您已使用夥伴用戶端認證 (DeviceModel、裝置、SensorModel、感應器) 建立中繼資料。

3. 確定您已使用正確的遙測訊息格式 (，如下所示) ：

```json
{
"deviceid": "<id of the Device created>",
"timestamp": "<timestamp in ISO 8601 format>",
"version" : "1",
"sensors": [
    {
      "id": "<id of the sensor created>",
      "sensordata": [
        {
          "timestamp": "< timestamp in ISO 8601 format >",
          "<sensor measure name (as defined in the Sensor Model)>": <value>
        },
        {
          "timestamp": "<timestamp in ISO 8601 format>",
          "<sensor measure name (as defined in the Sensor Model)>": <value>
        }
      ]
    }
 ]
}
```

## <a name="next-steps"></a>後續步驟

如需以 REST API 為基礎的整合詳細資料的詳細資訊，請參閱 [REST API](rest-api-in-azure-farmbeats.md)。