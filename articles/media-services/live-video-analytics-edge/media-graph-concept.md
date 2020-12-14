---
title: Media graph 概念-Azure
description: Media graph 可讓您定義媒體的捕獲來源、處理方式，以及應該傳遞結果的位置。 本文提供 media graph 概念的詳細描述。
ms.topic: conceptual
ms.date: 05/01/2020
ms.openlocfilehash: 6f23e7db8cecb46106a63fdecdb6ba04dbd99682
ms.sourcegitcommit: cc13f3fc9b8d309986409276b48ffb77953f4458
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97401095"
---
# <a name="media-graph"></a>媒體圖表

## <a name="suggested-pre-reading"></a>建議的閱讀準備事項

* [IoT Edge 上的 Live Video Analytics 概觀](overview.md)
* [IoT Edge 上的 Live Video Analytics 術語](terminology.md)

## <a name="overview"></a>概觀

Media graph 可讓您定義媒體的捕獲來源、處理方式，以及應該傳遞結果的位置。 您可以藉由連接元件或節點，以所需的方式來完成這項工作。 下圖提供媒體圖表的圖形表示。  

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/media-graph/media-graph.svg" alt-text="媒體圖表":::

IoT Edge 上的即時影片分析支援不同類型的來源、處理器和接收器。

* **來源節點** 可讓您將媒體捕獲到 media graph。 在概念上，此內容中的媒體可以是音訊串流、影片資料流程、資料流程，或是在單一資料流程中結合了音訊、影片和/或資料的資料流程。
* **處理器節點** 可讓您在 media graph 內處理媒體。
* **接收節點** 可將處理結果傳遞給媒體圖形以外的服務和應用程式。

## <a name="media-graph-topologies-and-instances"></a>Media graph 拓撲和實例 

IoT Edge 上的即時影片分析可讓您透過兩個概念（「圖形拓撲」和「圖形實例」）來管理媒體圖形。 圖形拓撲可讓您使用參數做為值的預留位置，來定義圖形的藍圖。 拓撲會定義媒體圖形中所使用的節點，以及它們在 media graph 內的連線方式。 例如，如果您想要錄製相機的摘要，則您需要具有接收影片之來源節點的圖表，以及寫入影片的接收節點。

當您建立參考拓撲的圖形實例時，會指定拓撲中參數的值。 這可讓您建立多個參考相同拓撲的實例，但針對拓撲中指定的參數使用不同的值。 在上述範例中，您可以使用參數來代表相機的 IP 位址，以及所錄製影片的名稱。 您可以建立具有該拓撲的許多圖形實例，也就是建築物中的每個相機都有一個實例，或許每個都有特定的 IP 位址和特定名稱。

## <a name="media-graph-states"></a>媒體圖形狀態  

圖形拓撲和圖形實例的生命週期會顯示在下列狀態圖中。

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/media-graph/graph-topology-lifecycle.svg" alt-text="圖形拓撲和圖形實例生命週期":::

開始 [建立圖形拓撲](direct-methods.md#graphtopologyset)。 然後，針對您想要使用此拓撲處理的每個即時影片摘要，您會 [建立一個圖形實例](direct-methods.md#graphinstanceset)。 

圖形實例會處於 `Inactive` (閒置) 狀態。

當您準備好要將即時影片摘要傳送到圖形實例時，您會 [啟用](direct-methods.md#graphinstanceactivate) 它。 圖形實例會短暫進入 transitionary `Activating` 狀態，如果成功則進入 `Active` 狀態。 在 `Active` 狀態中，如果圖形實例收到輸入資料) ，就會處理媒體 (。

> [!NOTE]
>  圖形實例可以是作用中，而不會流經資料 (例如，相機會離線) 。
> 當圖形實例處於作用中狀態時，您的 Azure 訂用帳戶將會向您收費。

如果您有其他即時影片摘要要處理，則可以針對相同的拓撲重複建立和啟用其他圖形實例的程式。

當您完成處理即時影片摘要時，可以 [停用](direct-methods.md#graphinstancedeactivate) 圖形實例。 圖形實例會短暫地經歷 transitionary `Deactivating` 狀態、清除其所擁有的任何資料，然後返回 `Inactive` 狀態。

只有當圖形實例處於狀態時，您才能將其 [刪除](direct-methods.md#graphinstancedelete) `Inactive` 。

當所有參考特定圖形拓撲的圖形實例都已刪除之後，您就可以 [刪除圖形拓撲](direct-methods.md#graphtopologydelete)。


## <a name="sources-processors-and-sinks"></a>來源、處理器和接收器  

IoT Edge 上的即時影片分析支援媒體圖形中的下列節點類型：

### <a name="sources"></a>來源 

#### <a name="rtsp-source"></a>RTSP 來源 

RTSP 來源節點可讓您從 [RTSP](https://tools.ietf.org/html/rfc2326) 伺服器內嵌媒體。 監視和以 IP 為基礎的攝影機會以稱為 RTSP (即時串流通訊協定) 的通訊協定來傳輸其資料，這與其他類型的裝置（例如手機和攝影機）不同。 此通訊協定是用來建立及控制伺服器 (相機) 和用戶端之間的媒體會話。 媒體圖形中的 RTSP 來源節點可作為用戶端，而且可以與 RTSP 伺服器建立會話。 許多裝置，例如大部分的 [IP 攝影機](https://en.wikipedia.org/wiki/IP_camera) 都有內建的 RTSP 伺服器。 [ONVIF](https://www.onvif.org/) 會在其 [設定檔 G、S &](https://www.onvif.org/wp-content/uploads/2019/12/ONVIF_Profile_Feature_overview_v2-3.pdf) 與相容裝置的定義中支援 RTSP。 RTSP 來源節點需要您指定 RTSP URL，以及用來啟用已驗證連線的認證。

#### <a name="iot-hub-message-source"></a>IoT 中樞訊息來源 

如同其他 [IoT Edge 模組](../../iot-edge/iot-edge-glossary.md#iot-edge-module)，IoT Edge 模組上的即時影片分析可透過 [IoT Edge 中樞](../../iot-edge/iot-edge-glossary.md#iot-edge-hub)接收訊息。 您可以從其他模組或在 Edge 裝置上執行的應用程式，或從雲端傳送這些訊息。 這類訊息會 (路由) 傳遞給模組上的 [命名輸入](../../iot-edge/module-composition.md#sink) 。 IoT 中樞訊息來源節點允許這類訊息到達 media graph。 然後，您可以在 media graph 內部使用這些訊息或信號，通常是為了啟動信號閘道 (請參閱以下) 的 [信號閘道](#signal-gate-processor) 。 

例如，您可以有一個 IoT Edge 模組，當大門開啟時，就會產生一則訊息。 來自該模組的訊息可以路由傳送至 IoT Edge hub，然後將它路由傳送到 media graph 的 IoT 中樞訊息來源。 在 media graph 中，IoT 中樞訊息來源可以將事件傳遞至信號閘道處理器，然後將影片從 RTSP 來源記錄到檔案中。 

### <a name="processors"></a>處理器  

#### <a name="motion-detection-processor"></a>動作偵測處理器 

[動作偵測處理器] 節點可讓您偵測即時影片中的動作。 它會檢查傳入的影片畫面，並判斷影片中是否有移動。 如果偵測到動作，它會將影片框架傳遞至下游元件，併發出事件。 當偵測到動作時，[動作偵測處理器] 節點 (與其他節點一起使用) 可用來觸發傳入影片的錄製。

#### <a name="frame-rate-filter-processor"></a>畫面播放速率篩選處理器  

[畫面播放速率篩選處理器] 節點可讓您以指定的速率，從傳入的視頻串流取樣畫面格。 這可讓您減少傳送至低資料流程元件的框架數目 (例如 HTTP 擴充處理器節點) ，以供進一步處理。
>[!WARNING]
> 此處理器已在 IoT Edge 模組上最新版本的即時影片分析中 **淘汰** 。 圖形擴充處理器本身內現在支援畫面播放速率管理。

#### <a name="http-extension-processor"></a>HTTP 擴充處理器

[HTTP 擴充處理器] 節點可讓您將自己的 IoT Edge 模組連接到 media graph。 此節點會取得已解碼的影片畫面作為輸入，並將這類畫面轉送至您的模組所公開的 HTTP REST 端點。 如果需要，此節點可以向 REST 端點進行驗證。 此外，節點也有內建的影像格式器，可在將影片框架轉送至 REST 端點之前進行縮放和編碼。 Scaler 具有要保留、填補或延展的影像外觀比例的選項。 影像編碼器支援 JPEG、PNG 或 BMP 格式。 若要深入 [瞭解處理器，](media-graph-extension-concept.md#http-extension-processor)請參閱。

#### <a name="grpc-extension-processor"></a>gRPC 擴充處理器

GRPC 延伸模組處理器節點會以解碼的影片畫面作為輸入，並將這類畫面轉送至您的模組所公開的 [gRPC](terminology.md#grpc) 端點。 節點支援使用 [共用記憶體](https://en.wikipedia.org/wiki/Shared_memory) 傳送資料，或直接將內容內嵌至 gRPC 訊息的主體。 此外，節點也有內建的影像格式器，可在將影片框架轉送至 gRPC 端點之前進行縮放和編碼。 Scaler 具有要保留、填補或延展的影像外觀比例的選項。 影像編碼器支援 jpeg、png 或 bmp 格式。 若要深入 [瞭解處理器，](media-graph-extension-concept.md#grpc-extension-processor)請參閱。

#### <a name="signal-gate-processor"></a>信號閘道處理器  

[信號閘道處理器] 節點可讓您有條件地將媒體從某個節點轉送到另一個節點。 它也可做為緩衝區，以允許同步處理媒體和事件。 典型的使用案例是在 RTSP 來源節點和資產接收器節點之間插入信號閘道處理器節點，並使用「動作偵測器處理器」節點的輸出來觸發閘道。 使用這類媒體圖形時，您只會在偵測到動作時錄製影片。

### <a name="sinks"></a>接收器  

#### <a name="asset-sink"></a>資產接收  

[資產接收] 節點可讓您將媒體 (影片和/或音訊) 資料寫入 Azure 媒體服務資產。 Media graph 中只能有一個資產接收器節點。 如需資產的詳細資訊及其在錄製和播放媒體的角色，請參閱「 [資產](terminology.md#asset) 」一節。 您也可以查看 [持續的錄影](continuous-video-recording-concept.md) 文章，以取得如何使用此節點屬性的詳細資料。

#### <a name="file-sink"></a>檔案接收  

[檔案接收] 節點可讓您將媒體 (影片和/或音訊) 資料寫入 IoT Edge 裝置本機檔案系統上的位置。 Media graph 中只能有一個 file sink 節點，而且它必須是「信號閘道處理器」節點的下游。 這會將輸出檔案的持續時間限制為 [信號閘道處理器] 節點屬性中指定的值。 為了確保您的 edge 裝置不會用盡磁碟空間，您也可以設定 IoT Edge 模組上的即時影片分析可用來儲存資料的大小上限。  
> [!NOTE]
如果檔案接收已滿，IoT Edge 模組上的即時影片分析將會開始刪除最舊的資料，並以新的資料取代。
#### <a name="iot-hub-message-sink"></a>IoT 中樞訊息接收  

IoT 中樞的 [訊息接收] 節點可讓您將事件發佈至 IoT Edge Hub。 IoT Edge 中樞可以接著將資料路由傳送至 Edge 裝置上的其他模組或應用程式，或將部署資訊清單中指定的每個路由的 IoT 中樞路由至雲端 () 。 IoT 中樞的 [訊息接收] 節點可以接受上游處理器（例如，「動作偵測處理器」節點）或透過 HTTP 擴充處理器節點的外部推斷服務的事件。

## <a name="rules-on-the-use-of-nodes"></a>使用節點的規則

如需有關如何在 media graph 內使用不同節點的其他規則，請參閱 [圖形拓撲的限制](quotas-limitations.md#limitations-on-graph-topologies-at-preview) 。

## <a name="scenarios"></a>案例

使用上面定義的來源、處理器和接收器的組合，您可以針對涉及即時影片分析的各種案例，建立媒體圖表。 範例案例如下：

* [錄製連續影片](continuous-video-recording-concept.md)
* [發生事件時錄製影片](event-based-video-recording-concept.md)
* [不錄製影片但進行 Live Video Analytics](analyze-live-video-concept.md)

## <a name="next-steps"></a>後續步驟

若要瞭解如何在即時影片摘要上執行動作偵測，請參閱 [快速入門：使用您自己的模型來執行即時影片分析](use-your-model-quickstart.md)。
