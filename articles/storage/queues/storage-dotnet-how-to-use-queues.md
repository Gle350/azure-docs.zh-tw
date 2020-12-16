---
title: 以 .NET 開始使用 Azure 佇列儲存體-Azure 儲存體
description: Azure 佇列儲存體在應用程式元件之間提供可靠的非同步通訊。 雲端傳訊可讓您的應用程式元件獨立擴充。
author: mhopkins-msft
ms.author: mhopkins
ms.reviewer: dineshm
ms.date: 10/08/2020
ms.topic: how-to
ms.service: storage
ms.subservice: queues
ms.custom: devx-track-csharp
ms.openlocfilehash: d54b8f15c90aa8f6ffcc04453fee0349e501f47d
ms.sourcegitcommit: d2d1c90ec5218b93abb80b8f3ed49dcf4327f7f4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/16/2020
ms.locfileid: "97585746"
---
# <a name="get-started-with-azure-queue-storage-using-net"></a>透過 .NET 開始使用 Azure 佇列儲存體

[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

## <a name="overview"></a>概觀

Azure 佇列儲存體可提供應用程式元件之間的雲端通訊。 設計應用程式以進行調整時，通常會將應用程式元件分離，讓它們可以獨立調整。 佇列儲存體可提供應用程式元件之間的非同步訊息處理，不論它們是在雲端、桌面、內部部署伺服器或行動裝置上執行。 佇列儲存體也支援管理非同步工作和建立程式工作流程。

### <a name="about-this-tutorial"></a>關於本教學課程

本教學課程說明如何使用 Azure 佇列儲存體撰寫一些常見案例的 .NET 程式碼。 本文說明的案例包括建立和刪除佇列，以及新增、讀取和刪除佇列訊息。

**預估完成時間：** 45 分鐘

### <a name="prerequisites"></a>必要條件

- [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
- [Azure 儲存體帳戶](../common/storage-account-create.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="set-up-your-development-environment"></a>設定開發環境

接下來，在 Visual Studio 中設定您的開發環境，以便您開始嘗試本指南中的程式碼範例。

### <a name="create-a-windows-console-application-project"></a>建立 Windows 主控台應用程式專案

在 Visual Studio 中，建立新的 Windows 主控台應用程式。 下列步驟說明如何在 Visual Studio 2019 中建立主控台應用程式。 這些步驟類似其他 Visual Studio 版本中的步驟。

1. 選取 **檔案**  >  **新增**  >  **專案**
2. 選取 **平臺**  >  **視窗**
3. 選取 **主控台應用程式 ( .NET Framework)**
4. 選取 [**下一步**]
5. 在 [ **專案名稱** ] 欄位中，輸入應用程式的名稱
6. 選取 [建立] 

本教學課程中的所有程式碼範例均可新增至您主控台應用程式的 `Program.cs` 檔案中的 `Main()` 方法。

您可以在任何類型的 .NET 應用程式中使用 Azure 儲存體的用戶端程式庫，包括 Azure 雲端服務或 web 應用程式，以及桌面與行動應用程式。 在本指南中，為求簡化，我們會使用主控台應用程式。

### <a name="use-nuget-to-install-the-required-packages"></a>使用 NuGet 來安裝必要的封裝

# <a name="net-v12"></a>[.NET v12](#tab/dotnet)

您必須參考專案中的下列四個套件，才能完成本教學課程：

- [適用于 .net 的 Azure Core 程式庫](https://www.nuget.org/packages/azure.core/)：此套件提供新式 .NET Azure SDK 用戶端程式庫的共用基本概念、抽象概念和協助程式。
- [Azure：適用于 .net 的一般用戶端程式庫](https://www.nuget.org/packages/azure.storage.common/)：此套件提供其他 Azure 儲存體用戶端程式庫所共用的基礎結構。
- [適用于 .net 的 azure node.js 用戶端程式庫](https://www.nuget.org/packages/azure.storage.queues/)：此封裝可讓您使用 Azure 佇列儲存體來儲存用戶端可能存取的訊息。
- [ 適用于 .net 的System.Configuration.ConfigurationManager 程式庫](https://www.nuget.org/packages/system.configuration.configurationmanager/)：此封裝可讓您存取用戶端應用程式的設定檔。

您可以使用 NuGet 來取得這些套件。 請遵循這些步驟：

1. 在 **方案總管** 中的專案上按一下滑鼠右鍵，然後選擇 [ **管理 NuGet 套件**]。
1. 選取 **[流覽]**
1. 在線上搜尋 `Azure.Storage.Queues` ，然後選取 [ **安裝** ] 以安裝 Azure 儲存體用戶端程式庫及其相依性。 這也會安裝 Azure。 Common 和 Azure Core 程式庫是佇列程式庫的相依性。
1. 在線上搜尋 `System.Configuration.ConfigurationManager` ，然後選取 [ **安裝** ] 來安裝 Configuration Manager。

# <a name="net-v11"></a>[.NET v11](#tab/dotnetv11)

您必須參考專案中的下列三個套件，才能完成本教學課程：

- [適用于 .net 的一般用戶端程式庫](https://www.nuget.org/packages/microsoft.azure.storage.common/)：此套件可讓您以程式設計方式存取儲存體帳戶中的資料資源。
- [適用于 .net 的 Microsoft Azure 佇列儲存體用戶端程式庫](https://www.nuget.org/packages/microsoft.azure.storage.queue/)：此用戶端程式庫可讓您使用 Azure 佇列儲存體來儲存用戶端可能存取的訊息。
- [適用於 .NET 的 Microsoft Azure Configuration Manager 程式庫](https://www.nuget.org/packages/microsoft.azure.configurationmanager/)︰此套件提供一個類別，無論您的應用程式於何處執行，均可用來剖析組態檔中的連接字串。

您可以使用 NuGet 來取得這些套件。 請遵循這些步驟：

1. 在 **方案總管** 中的專案上按一下滑鼠右鍵，然後選擇 [ **管理 NuGet 套件**]。
1. 選取 **[流覽]**
1. 在線上搜尋 `Microsoft.Azure.Storage.Queue` ，然後選取 [ **安裝** ] 以安裝 Azure 儲存體用戶端程式庫及其相依性。 這也會安裝 Microsoft Azure. 通用程式庫，這是佇列程式庫的相依性。
1. 在線上搜尋 `Microsoft.Azure.ConfigurationManager` ，然後選取 [ **安裝** ] 以安裝 Azure Configuration Manager。

---

### <a name="determine-your-target-environment"></a>決定您的目標環境

有兩個環境選項可供您執行本指南中的範例︰

- 您可以對雲端中的 Azure 儲存體帳戶執行您的程式碼。
- 您可以針對 Azurite 儲存體模擬器執行您的程式碼。 Azurite 是在雲端中模擬 Azure 儲存體帳戶的本機環境。 Azurite 是在應用程式開發期間測試和偵測程式碼的免費選項。 模擬器會使用已知的帳戶和金鑰。 如需詳細資訊，請參閱 [使用 Azurite 模擬器進行本機 Azure 儲存體開發和測試](../common/storage-use-azurite.md)。

> [!NOTE]
> 您可以選擇以儲存體模擬器為目標，以避免產生與 Azure 儲存體相關聯的任何費用。 但是，如果您選擇以雲端中的 Azure 儲存體帳戶為目標，則執行本教學課程的成本將會是可忽略的。

## <a name="get-your-storage-connection-string"></a>取得儲存體連接字串

適用于 .NET 的 Azure 儲存體用戶端程式庫支援使用儲存體連接字串來設定端點和認證，以存取儲存體服務。 如需詳細資訊，請參閱[管理儲存體帳戶存取金鑰](../common/storage-account-keys-manage.md)。

### <a name="copy-your-credentials-from-the-azure-portal"></a>從 Azure 入口網站複製您的認證

範例程式碼需要授與對您儲存體帳戶的存取權。 若要進行授權，您必須以連接字串的形式為應用程式提供您的儲存體帳戶認證。 若要檢視您的儲存體帳戶認證：

1. 瀏覽至 [Azure 入口網站](https://portal.azure.com)。
2. 找出您的儲存體帳戶。
3. 在儲存體帳戶概觀的 [設定] 區段中，選取 [存取金鑰]。 您的帳戶存取金鑰隨即出現，此外也會顯示每個金鑰的完整連接字串。
4. 尋找 [金鑰1] 下方的 [連接字串] 值，然後按一下 [複製] 按鈕來複製連接字串。 在下一個步驟中，您會將連接字串值新增至環境變數。

    ![顯示如何從 Azure 入口網站複製連接字串的螢幕擷取畫面](media/storage-dotnet-how-to-use-queues/portal-connection-string.png)

如需有關連接字串的詳細資訊，請參閱[設定 Azure 儲存體的連接字串](../common/storage-configure-connection-string.md)。

> [!NOTE]
> 儲存體帳戶金鑰很類似儲存體帳戶的根密碼。 請務必小心保護您的儲存體帳戶金鑰。 請避免轉發給其他使用者、進行硬式編碼，或將它儲存在其他人可以存取的純文字檔案。 如果您認為金鑰可能遭到破解，請使用 Azure 入口網站重新產生金鑰。

在組態檔中維護儲存體連接字串是最佳方式。 若要設定連接字串，請從 Visual Studio 中的 [方案總管] 開啟 `app.config` 檔案。 加入 `<appSettings>` 此處所示元素的內容。 `connection-string`將取代為您在入口網站中從儲存體帳戶複製的值：

```xml
<configuration>
    <startup>
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.7.2" />
    </startup>
    <appSettings>
        <add key="StorageConnectionString" value="connection-string" />
    </appSettings>
</configuration>
```

例如，組態設定會如下顯示：

```xml
<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=GMuzNHjlB3S9itqZJHHCnRkrokLkcSyW7yK9BRbGp0ENePunLPwBgpxV1Z/pVo9zpem/2xSHXkMqTHHLcx8XRA==EndpointSuffix=core.windows.net" />
```

若要以 Azurite 儲存體模擬器為目標，您可以使用對應到知名帳戶名稱和金鑰的快捷方式。 在此情況下，您的連接字串設定會是︰

```xml
<add key="StorageConnectionString" value="UseDevelopmentStorage=true" />
```

### <a name="add-using-directives"></a>新增 using 指示詞

將下列 `using` 指示詞新增至 `Program.cs` 檔案的開頭處：

# <a name="net-v12"></a>[.NET v12](#tab/dotnet)

:::code language="csharp" source="~/azure-storage-snippets/queues/howto/dotnet/dotnet-v12/QueueBasics.cs" id="snippet_UsingStatements":::

# <a name="net-v11"></a>[.NET v11](#tab/dotnetv11)

```csharp
using System; // Namespace for Console output
using Microsoft.Azure; // Namespace for CloudConfigurationManager
using Microsoft.Azure.Storage; // Namespace for CloudStorageAccount
using Microsoft.Azure.Storage.Queue; // Namespace for Queue storage types
```

---

### <a name="create-the-queue-storage-client"></a>建立佇列儲存體用戶端

# <a name="net-v12"></a>[.NET v12](#tab/dotnet)

[`QueueClient`](/dotnet/api/azure.storage.queues.queueclient)類別可讓您取出儲存在佇列儲存體中的佇列。 以下是建立服務用戶端的其中一種方式：

:::code language="csharp" source="~/azure-storage-snippets/queues/howto/dotnet/dotnet-v12/QueueBasics.cs" id="snippet_CreateClient":::

# <a name="net-v11"></a>[.NET v11](#tab/dotnetv11)

[`CloudQueueClient`](/dotnet/api/microsoft.azure.storage.queue.cloudqueueclient?view=azure-dotnet-legacy&preserve-view=true)類別可讓您取出儲存在佇列儲存體中的佇列。 以下是建立服務用戶端的其中一種方式：

```csharp
// Retrieve storage account from connection string
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
```

---

現在您已準備好撰寫程式碼，從佇列儲存體讀取資料，並將資料寫入佇列儲存體。

## <a name="create-a-queue"></a>建立佇列

此範例說明如何建立佇列：

# <a name="net-v12"></a>[.NET v12](#tab/dotnet)

:::code language="csharp" source="~/azure-storage-snippets/queues/howto/dotnet/dotnet-v12/QueueBasics.cs" id="snippet_CreateQueue":::

# <a name="net-v11"></a>[.NET v11](#tab/dotnetv11)

```csharp
// Retrieve storage account from connection string
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Create the queue if it doesn't already exist
queue.CreateIfNotExists();
```

---

## <a name="insert-a-message-into-a-queue"></a>將訊息插入佇列

# <a name="net-v12"></a>[.NET v12](#tab/dotnet)

若要將訊息插入現有佇列中，請呼叫 [`SendMessage`](/dotnet/api/azure.storage.queues.queueclient.sendmessage) 方法。 訊息可以是 UTF-8 格式的字串 () 或位元組陣列。 下列程式碼會建立佇列 (如果它不存在) 並插入訊息：

:::code language="csharp" source="~/azure-storage-snippets/queues/howto/dotnet/dotnet-v12/QueueBasics.cs" id="snippet_InsertMessage":::

# <a name="net-v11"></a>[.NET v11](#tab/dotnetv11)

若要將訊息插入現有佇列，請先建立新的 [`CloudQueueMessage`](/dotnet/api/microsoft.azure.storage.queue.cloudqueuemessage?view=azure-dotnet-legacy&preserve-view=true) 。 接下來，呼叫 [`AddMessage`](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.addmessage?view=azure-dotnet-legacy&preserve-view=true) 方法。 `CloudQueueMessage`可以從字串 (utf-8 格式) 或位元組陣列建立。 以下程式碼會建立佇列 (（如果不存在）) 並插入訊息 `Hello, World` ：若要將訊息插入現有佇列，請先建立新的 [`CloudQueueMessage`](/dotnet/api/microsoft.azure.storage.queue.cloudqueuemessage?view=azure-dotnet-legacy&preserve-view=true) 。 接下來，呼叫 [`AddMessage`](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.addmessage?view=azure-dotnet-legacy&preserve-view=true) 方法。 `CloudQueueMessage`可以從字串 (utf-8 格式) 或位元組陣列建立。 以下程式碼會建立佇列 (（如果不存在）) 並插入訊息 `Hello, World` ：

```csharp
// Retrieve storage account from connection string
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Create the queue if it doesn't already exist
queue.CreateIfNotExists();

// Create a message and add it to the queue
CloudQueueMessage message = new CloudQueueMessage("Hello, World");
queue.AddMessage(message);
```

---

## <a name="peek-at-the-next-message"></a>查看下一個訊息

# <a name="net-v12"></a>[.NET v12](#tab/dotnet)

您可以藉由呼叫方法來查看佇列中的訊息，而不需要將它們從佇列中移除 [`PeekMessages`](/dotnet/api/azure.storage.queues.queueclient.peekmessages) 。 如果您未傳遞參數的值 `maxMessages` ，則預設值是查看單一訊息。

:::code language="csharp" source="~/azure-storage-snippets/queues/howto/dotnet/dotnet-v12/QueueBasics.cs" id="snippet_PeekMessage":::

# <a name="net-v11"></a>[.NET v11](#tab/dotnetv11)

您可以藉由呼叫方法，在佇列前面查看訊息，而不需要將它從佇列中移除 [`PeekMessage`](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.peekmessage?view=azure-dotnet-legacy&preserve-view=true) 。

```csharp
// Retrieve storage account from connection string
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Peek at the next message
CloudQueueMessage peekedMessage = queue.PeekMessage();

// Display message.
Console.WriteLine(peekedMessage.AsString);
```

---

## <a name="change-the-contents-of-a-queued-message"></a>變更佇列訊息的內容

您可以在佇列中就地變更訊息內容。 如果訊息代表工作作業，則您可以使用此功能來更新工作作業的狀態。 下列程式碼將使用新的內容更新佇列訊息，並將可見度逾時設定延長 60 秒。 這可儲存與訊息相關的工作狀態，並提供用戶端多一分鐘的時間繼續處理訊息。 您可以使用這項技術來追蹤佇列訊息上的多步驟工作流程，而不需要從頭開始（如果處理步驟因為硬體或軟體故障而失敗）。 通常，您也會保留重試計數，如果訊息重試超過 *n* 次，您就會刪除它。 這麼做可防止每次處理時便觸發應用程式錯誤的訊息。

# <a name="net-v12"></a>[.NET v12](#tab/dotnet)

:::code language="csharp" source="~/azure-storage-snippets/queues/howto/dotnet/dotnet-v12/QueueBasics.cs" id="snippet_UpdateMessage":::

# <a name="net-v11"></a>[.NET v11](#tab/dotnetv11)

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Get the message from the queue and update the message contents.
CloudQueueMessage message = queue.GetMessage();
message.SetMessageContent2("Updated contents.", false);
queue.UpdateMessage(message,
    TimeSpan.FromSeconds(60.0),  // Make it invisible for another 60 seconds.
    MessageUpdateFields.Content | MessageUpdateFields.Visibility);
```

---

## <a name="dequeue-the-next-message"></a>將下一個訊息清除佇列

# <a name="net-v12"></a>[.NET v12](#tab/dotnet)

透過兩個步驟，從佇列中清除佇列中的訊息。 當您呼叫時 [`ReceiveMessages`](/dotnet/api/azure.storage.queues.queueclient.receivemessages) ，會取得佇列中的下一則訊息。 從 `ReceiveMessages` 傳回的訊息，對於從此佇列讀取訊息的任何其他程式碼而言，將會是不可見的。 依預設，此訊息會維持 30 秒的不可見狀態。 若要完成從佇列中移除訊息，您還必須呼叫 [`DeleteMessage`](/dotnet/api/azure.storage.queues.queueclient.deletemessage) 。 這個移除訊息的兩步驟程序可確保您的程式碼因為硬體或軟體故障而無法處理訊息時，另一個程式碼的執行個體可以取得相同訊息並再試一次。 您的程式碼會在 `DeleteMessage` 處理完訊息之後立即呼叫。

:::code language="csharp" source="~/azure-storage-snippets/queues/howto/dotnet/dotnet-v12/QueueBasics.cs" id="snippet_DequeueMessage":::

# <a name="net-v11"></a>[.NET v11](#tab/dotnetv11)

您的程式碼可以使用兩個步驟來清除佇列訊息。 當您呼叫時 [`GetMessage`](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.getmessage?view=azure-dotnet-legacy&preserve-view=true) ，會取得佇列中的下一則訊息。 從 `GetMessage` 傳回的訊息，對於從此佇列讀取訊息的任何其他程式碼而言，將會是不可見的。 依預設，此訊息會維持 30 秒的不可見狀態。 若要完成從佇列中移除訊息，您還必須呼叫 [`DeleteMessage`](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.deletemessage?view=azure-dotnet-legacy&preserve-view=true) 。 這個移除訊息的兩步驟程序可確保您的程式碼因為硬體或軟體故障而無法處理訊息時，另一個程式碼的執行個體可以取得相同訊息並再試一次。 您的程式碼會在 `DeleteMessage` 處理完訊息之後立即呼叫。

```csharp
// Retrieve storage account from connection string
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Get the next message
CloudQueueMessage retrievedMessage = queue.GetMessage();

//Process the message in less than 30 seconds, and then delete the message
queue.DeleteMessage(retrievedMessage);
```

---

<!-- docutune:casing "Async-Await pattern" "Async and Await" -->

## <a name="use-the-async-await-pattern-with-common-queue-storage-apis"></a>使用 Async-Await 模式搭配一般佇列儲存體 Api

此範例說明如何搭配使用 Async-Await 模式與一般佇列儲存體 Api。 此範例會呼叫每個指定方法的非同步版本，如 `Async` 每個方法的尾碼所表示。 使用非同步方法時，Async-Await 模式會暫停本機執行，直到呼叫完成為止。 這種行為可讓目前的執行緒執行其他工作，有助於避免發生效能瓶頸並提升應用程式的整體回應。 如需如何在 .NET 中使用 Async-Await 模式的詳細資訊，請參閱 [Async 和 Await (C# 和 Visual Basic)](/previous-versions/hh191443(v=vs.140))

# <a name="net-v12"></a>[.NET v12](#tab/dotnet)

:::code language="csharp" source="~/azure-storage-snippets/queues/howto/dotnet/dotnet-v12/QueueBasics.cs" id="snippet_AsyncQueue":::

# <a name="net-v11"></a>[.NET v11](#tab/dotnetv11)

```csharp
// Create the queue if it doesn't already exist
if(await queue.CreateIfNotExistsAsync())
{
    Console.WriteLine("Queue '{0}' Created", queue.Name);
}
else
{
    Console.WriteLine("Queue '{0}' Exists", queue.Name);
}

// Create a message to put in the queue
CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

// Async enqueue the message
await queue.AddMessageAsync(cloudQueueMessage);
Console.WriteLine("Message added");

// Async dequeue the message
CloudQueueMessage retrievedMessage = await queue.GetMessageAsync();
Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

// Async delete the message
await queue.DeleteMessageAsync(retrievedMessage);
Console.WriteLine("Deleted message");
```

---

## <a name="use-additional-options-for-dequeuing-messages"></a>使用清除佇列訊息的其他選項

自訂從佇列中擷取訊息的方法有兩種。 首先，您可以取得一批訊息 (最多 32 個)。 其次，您可以設定較長或較短的可見度逾時，讓您的程式碼有較長或較短的時間可以完全處理每個訊息。

# <a name="net-v12"></a>[.NET v12](#tab/dotnet)

下列程式碼範例會使用 [`ReceiveMessages`](/dotnet/api/azure.storage.queues.queueclient.receivemessages) 方法，在一個呼叫中取得20個訊息。 接著其會使用 `foreach` 迴圈處理每個訊息。 它也會將可見度逾時設定為每個訊息五分鐘。 請注意，系統會同時為所有訊息啟動五分鐘的時間，因此，從呼叫以來經過五分鐘後 `ReceiveMessages` ，任何尚未刪除的訊息都會重新顯示。

:::code language="csharp" source="~/azure-storage-snippets/queues/howto/dotnet/dotnet-v12/QueueBasics.cs" id="snippet_DequeueMessages":::

# <a name="net-v11"></a>[.NET v11](#tab/dotnetv11)

下列程式碼範例會使用 [`GetMessages`](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.getmessages?view=azure-dotnet-legacy&preserve-view=true) 方法，在一個呼叫中取得20個訊息。 接著其會使用 `foreach` 迴圈處理每個訊息。 它也會將可見度逾時設定為每個訊息五分鐘。 請注意，系統會同時為所有訊息啟動五分鐘的時間，因此，從呼叫以來經過五分鐘後 `GetMessages` ，任何尚未刪除的訊息都會重新顯示。

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

foreach (CloudQueueMessage message in queue.GetMessages(20, TimeSpan.FromMinutes(5)))
{
    // Process all messages in less than 5 minutes, deleting each message after processing.
    queue.DeleteMessage(message);
}
```

---

## <a name="get-the-queue-length"></a>取得佇列長度

# <a name="net-v12"></a>[.NET v12](#tab/dotnet)

您可以取得佇列中的估計訊息數目。 方法會傳回 [`GetProperties`](/dotnet/api/azure.storage.queues.queueclient.getproperties) 佇列屬性，包括訊息計數。 [`ApproximateMessagesCount`](/dotnet/api/azure.storage.queues.models.queueproperties.approximatemessagescount)屬性包含佇列中大約的訊息數目。 此數目不會低於佇列中實際的訊息數目，而會更高。

:::code language="csharp" source="~/azure-storage-snippets/queues/howto/dotnet/dotnet-v12/QueueBasics.cs" id="snippet_GetQueueLength":::

# <a name="net-v11"></a>[.NET v11](#tab/dotnetv11)

您可以取得佇列中的估計訊息數目。 方法會傳回 [`FetchAttributes`](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.fetchattributes?view=azure-dotnet-legacy&preserve-view=true) 佇列屬性，包括訊息計數。 [`ApproximateMessageCount`](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.approximatemessagecount?view=azure-dotnet-legacy&preserve-view=true)屬性會傳回方法所取出的最後一個值 `FetchAttributes` ，而不會呼叫佇列儲存體。

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Fetch the queue attributes.
queue.FetchAttributes();

// Retrieve the cached approximate message count.
int cachedMessageCount = queue.ApproximateMessageCount;

// Display number of messages.
Console.WriteLine("Number of messages in queue: " + cachedMessageCount);
```

---

## <a name="delete-a-queue"></a>刪除佇列

# <a name="net-v12"></a>[.NET v12](#tab/dotnet)

若要刪除佇列及其內含的所有訊息，請 [`Delete`](/dotnet/api/azure.storage.queues.queueclient.delete) 在佇列物件上呼叫方法。

:::code language="csharp" source="~/azure-storage-snippets/queues/howto/dotnet/dotnet-v12/QueueBasics.cs" id="snippet_DeleteQueue":::

# <a name="net-v11"></a>[.NET v11](#tab/dotnetv11)

若要刪除佇列及其內含的所有訊息，請 [`Delete`](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.delete?view=azure-dotnet-legacy&preserve-view=true) 在佇列物件上呼叫方法。

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Delete the queue.
queue.Delete();
```

---

## <a name="next-steps"></a>後續步驟

既然您已瞭解佇列儲存體的基本概念，請遵循下列連結以瞭解更複雜的儲存體工作。

- 如需可用 Api 的完整詳細資料，請參閱佇列儲存體參考檔：
  - [適用于 .NET 的 Azure 儲存體用戶端程式庫參考](/dotnet/api/overview/azure/storage)
  - [Azure 儲存體 REST API 參考](/rest/api/storageservices/)
- 如需了解 Azure 中的其他資料儲存選項，請檢視更多功能指南。
  - [使用 .net 開始使用 Azure 資料表儲存體](../../cosmos-db/tutorial-develop-table-dotnet.md) 來儲存結構化資料。
  - [使用 .net 開始 Azure Blob 儲存體](../blobs/storage-quickstart-blobs-dotnet.md) ，以儲存非結構化資料。
  - [使用 .NET (C#) 連接到 SQL Database ](../../azure-sql/database/connect-query-dotnet-core.md) 以儲存關聯式資料。
- 了解如何使用 [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki)，來簡化您撰寫以使用 Azure 儲存體的程式碼。
