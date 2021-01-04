---
title: Azure 佇列儲存體簡介 - Azure 儲存體
description: 查看 Azure 佇列儲存體的簡介，這是用來儲存大量訊息的服務。 佇列儲存體服務包含 URL 格式、儲存體帳戶、佇列和訊息。
author: mhopkins-msft
ms.author: mhopkins
ms.reviewer: dineshm
ms.date: 03/18/2020
ms.topic: overview
ms.service: storage
ms.subservice: queues
ms.openlocfilehash: 8c5c97fbb72934dd99ec784ccf8e08eec059c31b
ms.sourcegitcommit: d2d1c90ec5218b93abb80b8f3ed49dcf4327f7f4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/16/2020
ms.locfileid: "97590574"
---
# <a name="what-is-azure-queue-storage"></a>什麼是 Azure 佇列儲存體？

Azure 佇列儲存體是用來儲存大量訊息的服務。 使用 HTTP 或 HTTPS 透過境過驗證的呼叫，存取來自世界各地的訊息。 一則佇列訊息的大小可能高達 64 KB。 佇列可以包含數百萬則訊息，最高可達儲存體帳戶的總容量限制。 佇列通常用來建立要以非同步方式處理的待處理項目 (backlog)。

## <a name="queue-storage-concepts"></a>佇列儲存體概念

佇列儲存體包含下列元件：

![顯示儲存體帳戶、佇列和訊息之間關聯性的圖表。](./media/storage-queues-introduction/queue1.png)

- **URL 格式：** 可利用下列 URL 格式來定址佇列：

  `https://<storage account>.queue.core.windows.net/<queue>`

  下列 URL 可定址圖中的佇列：

  `https://myaccount.queue.core.windows.net/images-to-download`

- **儲存體帳戶：** 所有對 Azure 儲存體的存取都是透過儲存體帳戶進行。 如需儲存體帳戶容量的詳細資訊，請參閱[標準儲存體帳戶的延展性和效能目標](../common/scalability-targets-standard-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)。

- **佇列：** 佇列包含一組訊息。 佇列名稱 **必須** 是小寫。 如需為佇列命名的詳細資訊，請參閱 [為佇列和中繼資料命名](/rest/api/storageservices/naming-queues-and-metadata)。

- **訊息：** 大小上限為 64 KB 的訊息 (任何格式)。 在版本 2017-07-29 之前，允許的存留時間上限是七天。 如需版本 2017-07-29 或更新版本，最大存留時間可以是任何正數，或是表示訊息未過期的 -1。 如果省略此參數，則預設存留時間為 7 天。

## <a name="next-steps"></a>後續步驟

- [建立儲存體帳戶](../common/storage-account-create.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
- [以 .NET 開始使用佇列儲存體](storage-dotnet-how-to-use-queues.md)
- [以 Java 開始使用佇列儲存體](storage-java-how-to-use-queue-storage.md)
- [以 Python 開始使用佇列儲存體](storage-python-how-to-use-queue-storage.md)
- [以 Node.js 開始使用佇列儲存體](storage-nodejs-how-to-use-queues.md)
