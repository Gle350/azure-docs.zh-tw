---
title: 容器支援
titleSuffix: Azure Cognitive Services
description: 瞭解如何建立 Azure 容器實例資源。
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 24f6052c436b73d0075371fa74160d21826e2209
ms.sourcegitcommit: aeba98c7b85ad435b631d40cbe1f9419727d5884
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/04/2021
ms.locfileid: "97865986"
---
## <a name="create-an-azure-container-instance-resource-using-the-azure-portal"></a>使用 Azure 入口網站建立 Azure 容器實例資源

1. 移至容器實例的 [ [建立](https://ms.portal.azure.com/#create/Microsoft.ContainerInstances) ] 頁面。

2. 在 [ **基本** ] 索引標籤上，輸入下列詳細資料：

    |設定|值|
    |--|--|
    |訂用帳戶|選取您的訂用帳戶。|
    |資源群組|選取可用的資源群組，或建立一個新的資源群組（例如） `cognitive-services` 。|
    |容器名稱|輸入名稱，例如 `cognitive-container-instance` 。 名稱必須在較低的 cap 中。|
    |Location|選取要部署的區域。|
    |映像類型|如果您的容器映射儲存在不需要認證的容器登錄中，請選擇 `Public` 。 如果存取容器映射需要認證，請選擇 `Private` 。 請參閱 [容器存放庫和映射](../container-image-tags.md) ，以取得容器映射是否為 `Public` 或 ( 「 `Private` 公開預覽」 ) 的詳細資料。 |
    |映像名稱|輸入認知服務容器位置。 位置是用來做為命令引數的位置 `docker pull` 。 請參閱 [容器存放庫和映射](../container-image-tags.md) 以取得可用的映射名稱及其對應的存放庫。<br><br>映射名稱必須是指定三個部分的完整限定名稱。 首先，容器登錄，接著是存放庫，最後是映射名稱： `<container-registry>/<repository>/<image-name>` 。<br><br>以下範例 `mcr.microsoft.com/azure-cognitive-services/keyphrase` 表示 Azure 認知服務存放庫下 Microsoft Container Registry 中的關鍵片語擷取映射。 另一個範例是， `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text` 代表容器預覽容器登錄的 Microsoft 存放庫中的語音轉換文字映射。 |
    |OS 類型|`Linux`|
    |大小|變更您特定認知服務容器的建議建議大小：<br>2個 CPU 核心<br>4 GB

3. 在 [ **網路** 功能] 索引標籤上，輸入下列詳細資料：

    |設定|值|
    |--|--|
    |連接埠|將 TCP 通訊埠設定為 `5000` 。 公開端口5000上的容器。|

4. 在 [ **Advanced （Advanced** ）] 索引標籤上，為 Azure 容器實例資源的容器帳單設定輸入必要的 **環境變數** ：

    | Key | 值 |
    |--|--|
    |`ApiKey`|從資源的 [ **金鑰和端點** ] 頁面複製。 它是32英數位元字串，沒有空格或連字號 `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx` 。|
    |`Billing`| 從資源的 [ **金鑰和端點** ] 頁面複製的端點 URL。|
    |`Eula`|`accept`|

5. 按一下 [ **審核] 並建立**
6. 通過驗證之後，按一下 [ **建立** ] 以完成建立程式
7. 當資源部署成功時，即已就緒
