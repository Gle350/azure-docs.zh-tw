---
title: 了解 IoT 隨插即用數位分身
description: 瞭解 IoT 隨插即用如何使用數位 twins
author: prashmo
ms.author: prashmo
ms.date: 12/14/2020
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 99c957e5bf6ffe69c94e109796590f5ab975c3cf
ms.sourcegitcommit: ad677fdb81f1a2a83ce72fa4f8a3a871f712599f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/17/2020
ms.locfileid: "97656881"
---
# <a name="understand-iot-plug-and-play-digital-twins"></a>了解 IoT 隨插即用數位分身

IoT 隨插即用裝置會執行 [數位 Twins 定義語言 ](https://github.com/Azure/opendigitaltwins-dtdl) 所描述的模型， (DTDL) 架構。 模型描述特定裝置所能擁有的一組元件、屬性、命令和遙測訊息。

IoT 隨插即用使用 DTDL 第2版。 如需此版本的詳細資訊，請參閱 GitHub 上的 [數位 Twins 定義語言 (DTDL) -第2版](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md) 規格。

> [!NOTE]
> DTDL 不是 IoT 隨插即用專屬的。 其他 IoT 服務（例如 [Azure 數位 Twins](../digital-twins/overview.md)）使用它來代表整個環境，例如建築物和能源網路。

Azure IoT 服務 Sdk 包含可讓服務與裝置的數位對應項互動的 Api。 例如，服務可以從數位對應項讀取裝置屬性，或使用數位對應項在裝置上呼叫命令。 若要深入瞭解，請參閱 [IoT 中樞數位](concepts-developer-guide-service.md#iot-hub-digital-twin-examples)對應項範例。

本文 IoT 隨插即用裝置範例會實[控溫器](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/Thermostat.json)元件的[溫度控制器模型](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/TemperatureController.json)。

## <a name="device-twins-and-digital-twins"></a>裝置 twins 和數位 twins

以及數位對應項，Azure IoT 中樞也會為每個連線的裝置維護 *裝置* 對應項。 裝置對應項類似于數位對應項，因為它是裝置屬性的標記法。 Azure IoT 服務 Sdk 包含用來與裝置 twins 互動的 Api。

當 IoT 隨插即用裝置第一次連接時，IoT 中樞會初始化數位對應項和裝置對應項。

裝置 twins 是儲存裝置狀態資訊的 JSON 檔，包括中繼資料、設定和條件。 若要深入瞭解，請參閱 [IoT 中樞服務用戶端範例](concepts-developer-guide-service.md#iot-hub-service-client-examples)。 裝置和解決方案產生器都可以繼續使用一組相同的裝置對應項 Api 和 Sdk，利用 IoT 隨插即用慣例來實行裝置和解決方案。

數位對應項 Api 會在高階 DTDL 結構（例如元件、屬性和命令）上運作。 數位對應項 Api 可讓解決方案產生器更輕鬆地建立 IoT 隨插即用的解決方案。

在裝置對應項中，可寫入屬性的狀態會分割成所 *需的屬性* 和 *報告屬性* 區段。 所有唯讀屬性都可以在 [報告屬性] 區段中取得。

在數位對應項中，目前和預期的屬性狀態都有統一的觀點。 給定屬性的同步處理狀態會儲存在對應的 [預設元件] `$metadata` 區段中。

### <a name="device-twin-json-example"></a>裝置對應項 JSON 範例

下列程式碼片段顯示格式化為 JSON 物件的 IoT 隨插即用裝置對應項：

```json
{
  "deviceId": "sample-device",
  "modelId": "dtmi:com:example:TemperatureController;1",
  "version": 15,
  "properties": {
    "desired": {
      "thermostat1": {
        "__t": "c",
        "targetTemperature": 21.8
      },
      "$metadata": {...},
      "$version": 4
    },
    "reported": {
      "serialNumber": "alwinexlepaho8329",
      "thermostat1": {
        "maxTempSinceLastReboot": 25.3,
        "__t": "c",
        "targetTemperature": {
          "value": 21.8,
          "ac": 200,
          "ad": "Successfully executed patch",
        }
      },
      "$metadata": {...},
      "$version": 11
    }
  }
}
```

### <a name="digital-twin-example"></a>數位對應項範例

下列程式碼片段顯示格式化為 JSON 物件的數位對應項：

```json
{
  "$dtId": "sample-device",
  "serialNumber": "alwinexlepaho8329",
  "thermostat1": {
    "maxTempSinceLastReboot": 25.3,
    "targetTemperature": 21.8,
    "$metadata": {
      "targetTemperature": {
        "desiredValue": 21.8,
        "desiredVersion": 4,
        "ackVersion": 4,
        "ackCode": 200,
        "ackDescription": "Successfully executed patch",
        "lastUpdateTime": "2020-07-17T06:11:04.9309159Z"
      },
      "maxTempSinceLastReboot": {
         "lastUpdateTime": "2020-07-17T06:10:31.9609233Z"
      }
    }
  },
  "$metadata": {
    "$model": "dtmi:com:example:TemperatureController;1",
    "serialNumber": {
      "lastUpdateTime": "2020-07-17T06:10:31.9609233Z"
    }
  }
}
```

下表描述數位對應項 JSON 物件中的欄位：

| 欄位名稱 | 描述 |
| --- | --- |
| `$dtId` | 使用者提供的字串，代表裝置數位對應項的識別碼 |
| `{propertyName}` | JSON 中的屬性值 |
| `$metadata.$model` | 參數此數位對應項之特性的模型介面識別碼 |
| `$metadata.{propertyName}.desiredValue` | [僅適用于可寫入的屬性]指定之屬性的所需值 |
| `$metadata.{propertyName}.desiredVersion` | [僅適用于可寫入的屬性]IoT 中樞所維護的所需值版本|
| `$metadata.{propertyName}.ackVersion` | [必要，僅適用于可寫入的屬性]執行數位對應項的裝置所認可的版本，必須大於或等於所需的版本 |
| `$metadata.{propertyName}.ackCode` | [必要，僅適用于可寫入的屬性] `ack` 執行數位對應項的裝置應用程式所傳回的程式碼 |
| `$metadata.{propertyName}.ackDescription` | [選擇性，僅適用于可寫入的屬性] `ack` 執行數位對應項的裝置應用程式所傳回的描述 |
| `$metadata.{propertyName}.lastUpdateTime` | IoT 中樞會維護裝置上次更新屬性的時間戳記。 時間戳記是以 UTC 格式編碼，並以 ISO8601 格式的格式為 YYYY-MM-DD-DDTHH： MM： SS. mmmZ |
| `{componentName}` | JSON 物件，其中包含元件的屬性值和中繼資料。 |
| `{componentName}.{propertyName}` | 元件在 JSON 中的屬性值 |
| `{componentName}.$metadata` | 元件的中繼資料資訊。 |

### <a name="properties"></a>屬性

屬性是代表實體之狀態的資料欄位 (例如許多物件導向程式設計語言中的屬性) 。

#### <a name="read-only-property"></a>唯讀屬性

DTDL 架構：

```json
{
    "@type": "Property",
    "name": "serialNumber",
    "displayName": "Serial Number",
    "description": "Serial number of the device.",
    "schema": "string"
}
```

在此範例中， `alwinexlepaho8329` 是裝置所報告的 `serialNumber` 唯讀屬性目前的值。
下列程式碼片段顯示內容的並列 JSON 標記法 `serialNumber` ：

:::row:::
   :::column span="":::
      **裝置對應項**

```json
"properties": {
  "reported": {
    "serialNumber": "alwinexlepaho8329"
  }
}
```

   :::column-end:::
   :::column span="":::
      **數位分身**

```json
"serialNumber": "alwinexlepaho8329"
```

   :::column-end:::
:::row-end:::

#### <a name="writable-property"></a>可寫入屬性

下列範例顯示預設元件中的可寫入屬性。

DTDL

```json
{
  "@type": "Property",
  "name": "fanSpeed",
  "displayName": "Fan Speed",
  "writable": true,
  "schema": "double"
}
```

:::row:::
   :::column span="":::
      **裝置對應項**

```json
{
  "properties": {
    "desired": {
      "fanSpeed": 2.0,
    },
    "reported": {
      "fanSpeed": {
        "value": 3.0,
        "ac": 200,
        "av": 1,
        "ad": "Successfully executed patch version 1"
      }
    }
  },
}
```

   :::column-end:::
   :::column span="":::
      **數位分身**

```json
{
  "fanSpeed": 3.0,
  "$metadata": {
    "fanSpeed": {
      "desiredValue": 2.0,
      "desiredVersion": 2,
      "ackVersion": 1,
      "ackCode": 200,
      "ackDescription": "Successfully executed patch version 1",
      "lastUpdateTime": "2020-07-17T06:10:31.9609233Z"
    }
  }
}
```

   :::column-end:::
:::row-end:::

在此範例中， `3.0` 是裝置所報告之屬性的目前值 `fanSpeed` 。 `2.0` 是解決方案所設定的所需值。 根層級屬性所需的值和同步處理狀態，是在數位對應項的根層級中設定 `$metadata` 。 當裝置上線時，它可以套用此更新，並回報更新的值。

### <a name="components"></a>元件

元件可讓您將模型介面建立成其他介面的元件。
例如， [控溫器](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/Thermostat.json) 介面可以納入為元件 `thermostat1` 和  `thermostat2` [溫度控制器模型](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/TemperatureController.json) 模型。

在裝置對應項中，標記會識別元件 `{ "__t": "c"}` 。 在數位對應項中， `$metadata` 有標記元件的存在。

在此範例中， `thermostat1` 是具有兩個屬性的元件：

- `maxTempSinceLastReboot` 這是唯讀屬性。
- `targetTemperature` 是已成功由裝置同步處理的可寫入屬性。 這些屬性所需的值和同步處理狀態位於元件的中 `$metadata` 。

下列程式碼片段顯示元件的並列 JSON 標記法 `thermostat1` ：

:::row:::
   :::column span="":::
      **裝置對應項**

```json
"properties": {
  "desired": {
    "thermostat1": {
      "__t": "c",
      "targetTemperature": 21.8
    },
    "$metadata": {
    },
    "$version": 4
  },
  "reported": {
    "thermostat1": {
      "maxTempSinceLastReboot": 25.3,
      "__t": "c",
      "targetTemperature": {
        "value": 21.8,
        "ac": 200,
        "ad": "Successfully executed patch",
        "av": 4
      }
    },
    "$metadata": {
    },
    "$version": 11
  }
}
```

   :::column-end:::
   :::column span="":::
      **數位分身**

```json
"thermostat1": {
  "maxTempSinceLastReboot": 25.3,
  "targetTemperature": 21.8,
  "$metadata": {
    "targetTemperature": {
      "desiredValue": 21.8,
      "desiredVersion": 4,
      "ackVersion": 4,
      "ackCode": 200,
      "ackDescription": "Successfully executed patch",
      "lastUpdateTime": "2020-07-17T06:11:04.9309159Z"
    },
    "maxTempSinceLastReboot": {
       "lastUpdateTime": "2020-07-17T06:10:31.9609233Z"
    }
  }
}
```

   :::column-end:::
:::row-end:::

## <a name="digital-twin-apis"></a>數位對應項 Api

數位對應項 Api 包括 **取得數位** 對應項、 **更新數位** 對應項、叫用 **元件命令** ，以及叫用 **命令** 作業更多管理數位對應項。 您可以直接使用 [REST api](/rest/api/iothub/service/digitaltwin) ，或透過 [服務 SDK](../iot-pnp/libraries-sdks.md)來使用。

## <a name="digital-twin-change-events"></a>數位分身變更事件

當數位分身變更事件啟用時，每當元件或屬性的目前值或所需值變更時，就會觸發事件。 數位對應項變更事件會以 [JSON 修補程式](http://jsonpatch.com/) 格式產生。 如果已啟用對應項變更事件，就會以裝置對應項的格式產生對應的事件。

若要瞭解如何啟用裝置和數位對應項事件的路由，請參閱 [使用 IoT 中樞訊息路由將裝置到雲端訊息傳送至不同的端點](../iot-hub/iot-hub-devguide-messages-d2c.md#non-telemetry-events)。 若要瞭解訊息格式，請參閱 [建立和讀取 IoT 中樞訊息](../iot-hub/iot-hub-devguide-messages-construct.md)。

例如，當由方案設定時，會觸發下列數位對應項變更事件 `targetTemperature` ：

```json
iothub-connection-device-id:sample-device
iothub-enqueuedtime:7/17/2020 6:11:04 AM
iothub-message-source:digitalTwinChangeEvents
correlation-id:275d463fa034
content-type:application/json-patch+json
content-encoding:utf-8
[
  {
    "op": "add",
    "path": "/thermostat1/$metadata/targetTemperature",
    "value": {
      "desiredValue": 21.8,
      "desiredVersion": 4
    }
  }
]
```

當裝置回報上述所需的變更時，就會觸發下列數位對應項變更事件：

```json
iothub-connection-device-id:sample-device
iothub-enqueuedtime:7/17/2020 6:11:05 AM
iothub-message-source:digitalTwinChangeEvents
correlation-id:275d464a2c80
content-type:application/json-patch+json
content-encoding:utf-8
[
  {
    "op": "add",
    "path": "/thermostat1/$metadata/targetTemperature/ackCode",
    "value": 200
  },
  {
    "op": "add",
    "path": "/thermostat1/$metadata/targetTemperature/ackDescription",
    "value": "Successfully executed patch"
  },
  {
    "op": "add",
    "path": "/thermostat1/$metadata/targetTemperature/ackVersion",
    "value": 4
  },
  {
    "op": "add",
    "path": "/thermostat1/$metadata/targetTemperature/lastUpdateTime",
    "value": "2020-07-17T06:11:04.9309159Z"
  },
  {
    "op": "add",
    "path": "/thermostat1/targetTemperature",
    "value": 21.8
  }
]
```

> [!NOTE]
> 當在裝置和數位對應項變更通知中開啟時，對應項變更通知訊息會加倍。

## <a name="next-steps"></a>後續步驟

現在您已瞭解數位 twins，以下是一些額外的資源：

- [如何使用 IoT 隨插即用數位對應項 Api](howto-manage-digital-twin.md)
- [從您的解決方案與裝置互動](quickstart-service.md)
- [IoT 數位對應項 REST API](/rest/api/iothub/service/digitaltwin)
- [Azure IoT 總管](howto-use-iot-explorer.md)