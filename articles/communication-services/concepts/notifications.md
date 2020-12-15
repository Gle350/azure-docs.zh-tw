---
title: Azure 通訊服務中的通知
titleSuffix: An Azure Communication Services concept document
description: 將通知傳送給內建在 Azure 通訊服務上應用程式的使用者。
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 09/30/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: a52188dc5058dbc74d3b03fba860b98540cd4a41
ms.sourcegitcommit: 4c89d9ea4b834d1963c4818a965eaaaa288194eb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/04/2020
ms.locfileid: "96608497"
---
# <a name="communication-services-notifications"></a>通訊服務通知

[!INCLUDE [Public Preview Notice](../includes/public-preview-include.md)]

Azure 通訊服務聊天和通話用戶端程式庫會建立即時訊息通道，讓訊息以有效率且可靠的方式推送至已連線的用戶端。 這可讓您在應用程式中建置豐富的即時通訊功能，而不需要實作複雜的 HTTP 輪詢邏輯。 不過在行動應用程式上，只有當您的應用程式在前景作用時，此訊號通道才會保持連線狀態。 如果您想要讓使用者在您的應用程式處於背景時接收來電或聊天訊息，您應該使用推播通知。

推播通知可讓您將應用程式的資訊傳送至使用者的行動裝置。 您可以使用推播通知來顯示對話方塊、播放音效，或顯示來電 UI。 Azure 通訊服務提供與 [Azure 事件方格](../../event-grid/overview.md)和 [Azure 通知中樞](../../notification-hubs/notification-hubs-push-notification-overview.md)的整合，讓您將推播通知新增至您的應用程式。

## <a name="trigger-push-notifications-via-azure-event-grid"></a>透過 Azure 事件方格觸發推播通知

Azure 通訊服務與 [Azure 事件方格](https://azure.microsoft.com/services/event-grid/)整合，以可靠、可擴充且安全的方式傳遞即時事件通知。 您可以利用這項整合來建立通知服務，藉由建立可觸發 [Azure Function](../../azure-functions/functions-overview.md) 或 Webhook 的事件方格訂用帳戶，將行動推播通知傳遞給您的使用者。

:::image type="content" source="./media/notifications/acs-events-int.png" alt-text="圖表顯示通訊服務與事件方格的整合方式。":::

深入了解 [Azure 通訊服務中的事件處理](./event-handling.md)。

## <a name="deliver-push-notifications-via-azure-notification-hubs"></a>透過 Azure 通知中樞傳遞推播通知

您可以將 Azure 通知中樞連線到您的通訊服務資源，以便在接到來電時，自動將推播通知傳送至使用者的行動裝置。 您應該使用這些推播通知將您的應用程式從背景喚醒，並且顯示 UI，讓使用者接受或拒絕通話。 

:::image type="content" source="./media/notifications/acs-anh-int.png" alt-text="圖表顯示通訊服務與事件中樞的整合方式。":::

通訊服務會使用 Azure 通知中樞作為傳遞服務，以使用[直接傳送](/rest/api/notificationhubs/direct-send) API 與各種平台特定的推播通知服務進行通訊。 這可讓您重複使用現有的 Azure 通知中樞資源和設定，為您的應用程式提供低延遲、可靠的通話通知。

> [!NOTE]
> 目前只支援呼叫推播通知。

### <a name="notification-hub-provisioning"></a>通知中樞佈建 

若要使用通知中樞將推播通知傳遞至用戶端裝置，請在與您的通訊服務資源相同的訂用帳戶中[建立通知中樞](../../notification-hubs/create-notification-hub-portal.md)。 必須為您要使用的平台通知服務設定 Azure 通知中樞。 若要了解如何在用戶端應用程式中收到通知中樞的推播通知，請參閱[開始使用通知中心](../../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md)，並從靠近頁面頂端的下拉式清單中選取目標用戶端平台。

> [!NOTE]
> 目前支援 APN 和 FCM 平台。  
必須以權杖驗證模式設定 APN 平台。 目前不支援憑證驗證模式。 

設定通知中樞之後，您可以使用 Azure Resource Manager 用戶端或透過 Azure 入口網站提供中樞的連接字串，將其與您的「通訊服務」資源建立關聯。 連接字串應該包含「傳送」權限。 我們建議您特別為您的中樞建立另一個具有僅限「傳送」權限的存取原則。 深入了解[通知中樞安全性和存取原則](../../notification-hubs/notification-hubs-push-notification-security.md)

#### <a name="using-the-azure-resource-manager-client-to-configure-the-notification-hub"></a>使用 Azure Resource Manager 用戶端來設定通知中樞

若要登入 Azure Resource Manager，請執行下列作業，並使用您的認證登入。

```console
armclient login
```

 成功登入之後，請執行下列動作以佈建通知中樞：

```console
armclient POST /subscriptions/<sub_id>/resourceGroups/<resource_group>/providers/Microsoft.Communication/CommunicationServices/<resource_id>/linkNotificationHub?api-version=2020-08-20-preview "{'connectionString': '<connection_string>','resourceId': '<resource_id>'}"
```

#### <a name="using-the-azure-portal-to-configure-the-notification-hub"></a>使用 Azure 入口網站設定通知中樞

在入口網站中，瀏覽至您的 Azure 通訊服務資源。 在通訊服務資源中，從 [通訊服務] 頁面的左側功能表選取 [推播通知]，然後連線您先前佈建的通知中樞。 您必須在這裡提供您的連接字串和資源識別碼：

:::image type="content" source="./media/notifications/acs-anh-portal-int.png" alt-text="顯示 Azure 入口網站中推播通知設定的螢幕擷取畫面。":::

> [!NOTE]
> 如果 Azure 通知中樞連接字串有更新，則必須一併更新通訊服務資源。  
中樞連結方式的任何變更，都會在最長期間 ``10`` 分鐘內反映於資料平面中 (亦即在傳送通知時)。 **如果** 之前已傳送通知，在第一次連結中樞時也適用此原則。

#### <a name="device-registration"></a>裝置註冊 

請參閱[語音通話快速入門](../quickstarts/voice-video-calling/getting-started-with-calling.md)，以了解如何向通訊服務註冊您的裝置控制代碼。

## <a name="next-steps"></a>下一步

* 如需 Azure Event Grid 的簡介，請參閱[什麼是 Event Grid？](../../event-grid/overview.md)
* 若要深入了解 Azure 通知中樞的概念，請參閱 [Azure 通知中樞文件](../../notification-hubs/index.yml)