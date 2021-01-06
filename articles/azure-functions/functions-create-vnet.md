---
title: 將 Azure Functions 與 Azure 虛擬網路整合
description: 說明如何將函式連線至 Azure 虛擬網路的逐步教學課程
ms.topic: article
ms.date: 4/23/2020
ms.openlocfilehash: efc936111d162d73b1cc5465ae6b677c9006ab32
ms.sourcegitcommit: 2aa52d30e7b733616d6d92633436e499fbe8b069
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97937006"
---
# <a name="tutorial-integrate-functions-with-an-azure-virtual-network"></a>教學課程：將 Functions 與 Azure 虛擬網路整合

本教學課程說明如何使用 Azure Functions 連接到 Azure 虛擬網路中的資源。 您將建立可存取網際網路的函式，以及在虛擬網路中執行 WordPress 的 VM。

> [!div class="checklist"]
> * 在 Premium 方案中建立函數應用程式
> * 將 WordPress 網站部署至虛擬網路中的 VM
> * 將函數應用程式連線到虛擬網路
> * 建立函數 proxy 以存取 WordPress 資源
> * 從虛擬網路內部要求 WordPress 檔案

## <a name="topology"></a>拓撲

下圖顯示您所建立之解決方案的架構：

 ![虛擬網路整合的 UI](./media/functions-create-vnet/topology.png)

在 Premium 方案中執行的函式與 Azure App Service 中的 web apps 具有相同的裝載功能，包括 VNet 整合功能。 若要深入瞭解 VNet 整合，包括疑難排解和 advanced configuration，請參閱 [整合您的應用程式與 Azure 虛擬網路](../app-service/web-sites-integrate-with-vnet.md)。

## <a name="prerequisites"></a>必要條件

在本教學課程中，您必須了解 IP 定址和子網路設定。 您可以從 [本文](https://support.microsoft.com/help/164015/understanding-tcp-ip-addressing-and-subnetting-basics)開始，其內容涵蓋了定址和子網路設定的基本概念。 您也可以從線上取得更多文章和影片。

如果您沒有 Azure 訂用帳戶，請在開始前建立[免費帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) 。

## <a name="create-a-function-app-in-a-premium-plan"></a>在 Premium 方案中建立函數應用程式

首先，您會在 [Premium 方案]中建立函數應用程式。 此方案提供無伺服器的調整，同時支援虛擬網路整合。

[!INCLUDE [functions-premium-create](../../includes/functions-premium-create.md)]  

您可以選取右上角的釘選圖示，將函數應用程式釘選到儀表板。 在您建立 VM 之後，釘選可讓您更輕鬆地返回此函數應用程式。

## <a name="create-a-vm-inside-a-virtual-network"></a>在虛擬網路內建立 VM

接下來，建立在虛擬網路內部執行 WordPress 的預先設定 VM， (WordPress LEMP7 Jetware) 的 [最大效能](https://jetware.io/appliances/jetware/wordpress4_lemp7-170526/profile?us=azure) 。 因為 WordPress VM 的成本較低，所以會使用它。 此相同案例適用于虛擬網路中的任何資源，例如 REST Api、App Service 環境及其他 Azure 服務。 

1. 在入口網站的左側流覽窗格中，選擇 [ **+ 建立資源** ]，在 [搜尋] 欄位中輸入 `WordPress LEMP7 Max Performance` ，然後按 enter 鍵。

1. 選擇搜尋結果中的 [ **WORDPRESS LEMP Max Performance** ]。 選取 **WORDPRESS LEMP Max Performance For CentOS** As **software plan** 的軟體方案，然後選取 [ **建立**]。

1. 在 [基本資訊] 索引標籤中，使用影像下方表格中指定的 VM 設定：

    ![用於建立 VM 的 [基本] 索引標籤](./media/functions-create-vnet/create-vm-1.png)

    | 設定      | 建議的值  | 描述      |
    | ------------ | ---------------- | ---------------- |
    | **訂用帳戶** | 您的訂用帳戶 | 建立您資源所在位置的訂用帳戶。 | 
    | **[資源群組](../azure-resource-manager/management/overview.md)**  | myResourceGroup | 選擇 `myResourceGroup` 或您使用函數應用程式建立的資源群組。 針對函數應用程式、WordPress VM 和主控方案使用相同的資源群組，可讓您在完成本教學課程時更輕鬆地清除資源。 |
    | **虛擬機器名稱** | VNET-Wordpress | VM 名稱在資源群組中必須是唯一的 |
    | **[區域](https://azure.microsoft.com/regions/)** | 歐洲 (歐洲) 西歐 | 選擇您附近或接近存取 VM 的函式附近的區域。 |
    | **大小** | B1s | 選擇 [ **變更大小** ]，然後選取 [B1s] 標準映射，其具有 1 vCPU 和 1 GB 的記憶體。 |
    | **驗證類型** | 密碼 | 若要使用密碼驗證，您也必須指定使用者 **名稱**、安全 **密碼**，然後 **確認密碼**。 在本教學課程中，您不需要登入 VM，除非您需要進行疑難排解。 |

1. 選擇 [**網路**] 索引標籤，然後在 [設定虛擬網路] 下方選取 **[新建]。**

1. 在 **建立虛擬網路** 中，使用影像下方表格中的設定：

    ![建立 VM 的 [網路功能] 索引標籤](./media/functions-create-vnet/create-vm-2.png)

    | 設定      | 建議的值  | 描述      |
    | ------------ | ---------------- | ---------------- |
    | **名稱** | myResourceGroup-vnet | 您可以使用為虛擬網路產生的預設名稱。 |
    | **位址範圍** | 10.10.0.0/16 | 在虛擬網路中使用單一位址範圍。 |
    | **子網路名稱** | Tutorial-Net | 子網路的名稱。 |
    | **位址範圍** (子網路) | 10.10.1.0/24   | 子網路大小會定義可新增至子網路的介面數目。 WordPress 網站會使用此子網。  `/24`子網提供254主機位址。 |

1. 選取 [確定] 以建立虛擬網路。

1. 回到 [**網路**] 索引標籤，針對 [**公用 IP**] 選擇 [**無**]。

1. 選擇 [ **管理** ] 索引標籤，然後在 [ **診斷儲存體帳戶**] 中，選擇您使用函數應用程式所建立的儲存體帳戶。

1. 選取 [檢閱 + 建立]。 驗證完成時，選取 [建立]。 VM 建立程序需要幾分鐘完成。 建立的 VM 只能存取虛擬網路。

1. 建立 VM 之後，請選擇 [**移至資源**] 以查看新 VM 的頁面，然後選擇 [**設定**] 下的 [**網路**]。

1. 確認沒有 **公用 IP**。 記下 **私人 IP**，用來從您的函式應用程式連線到 VM。

    ![VM 中的網路設定](./media/functions-create-vnet/vm-networking.png)

您現在已將 WordPress 網站完全部署在您的虛擬網路內。 此網站無法從公用網際網路存取。

## <a name="connect-your-function-app-to-the-virtual-network"></a>將您的函數應用程式連線到虛擬網路

在虛擬網路中的 VM 上執行 WordPress 網站時，您現在可以將函式應用程式連線到該虛擬網路。

1. 在新的函式應用程式中，選取左側功能表中的 [ **網路** 功能]。

1. 在 [VNet 整合] 下，選取 [按一下這裡可設定]。

    :::image type="content" source="./media/functions-create-vnet/networking-0.png" alt-text="選擇函式應用程式中的網路功能":::

1. 在 [ **VNET 整合** ] 頁面上，選取 [ **新增 VNET**]。

    :::image type="content" source="./media/functions-create-vnet/networking-2.png" alt-text="新增 VNet 整合預覽":::

1. 在 [ **網路功能狀態**] 中，使用影像下方表格中的設定：

    ![定義函數應用程式虛擬網路](./media/functions-create-vnet/networking-3.png)

    | 設定      | 建議的值  | 描述      |
    | ------------ | ---------------- | ---------------- |
    | **虛擬網路** | MyResourceGroup-vnet | 此虛擬網路是您稍早建立的虛擬網路。 |
    | **子網路** | 建立新的子網 | 在虛擬網路中建立子網，讓您的函數應用程式使用。 VNet 整合必須設定為使用空的子網。 您的函式所使用的子網與您的 VM 不同，不重要。 虛擬網路會自動在這兩個子網之間路由傳送流量。 |
    | **子網路名稱** | Function-Net | 新子網路的名稱。 |
    | **虛擬網路位址區塊** | 10.10.0.0/16 | 選擇 WordPress 網站所使用的相同位址區塊。 您只能定義一個位址區塊。 |
    | **位址範圍** | 10.10.2.0/24   | 子網大小會限制高階方案函式應用程式可相應放大的實例總數。 此範例會使用 `/24` 具有254可用主機位址的子網。 此子網過度布建，但很容易計算。 |

1. 選取 **[確定]** 以新增子網。 關閉 [ **VNet 整合** 和 **網路功能狀態** ] 頁面，以返回您的函數應用程式頁面。

函數應用程式現在可以存取 WordPress 網站執行所在的虛擬網路。 接下來，您會使用 [Azure Functions Proxy](functions-proxies.md) 從 WordPress 網站傳回檔案。

## <a name="create-a-proxy-to-access-vm-resources"></a>建立 proxy 以存取 VM 資源

啟用 VNet 整合之後，您可以在函數應用程式中建立 proxy，以將要求轉送至虛擬網路中執行的 VM。

1. 在您的函數應用程式中  **，從左側功能表中選取** [proxy]，然後選取 [ **新增**]。 使用映射下表中的 proxy 設定：

    :::image type="content" source="./media/functions-create-vnet/create-proxy.png" alt-text="定義 Proxy 設定":::

    | 設定  | 建議的值  | 描述      |
    | -------- | ---------------- | ---------------- |
    | **名稱** | PlAnT | 名稱可以是任何值。 它是用來識別 proxy。 |
    | **路由範本** | /plant | 對應至 VM 資源的路由。 |
    | **後端 URL** | HTTP://<YOUR_VM_IP>/wp-content/themes/twentyseventeen/assets/images/header.jpg | 將取代 `<YOUR_VM_IP>` 為您稍早建立的 WORDPRESS VM IP 位址。 此對應會從網站傳回單一檔案。 |

1. 選取 [ **建立** ] 以將 proxy 加入至您的函數應用程式。

## <a name="try-it-out"></a>試做

1. 在您的瀏覽器中，嘗試存取用來作為 **後端 url** 的 url。 如預期般，要求超時。因為您的 WordPress 網站只連接到您的虛擬網路，而不是連線到網際網路，所以會發生超時。

1. 從新的 proxy 複製 [ **PROXY URL** ] 值，並將它貼到瀏覽器的網址列。 傳回的映射來自于虛擬網路內執行的 WordPress 網站。

    ![從 WordPress 網站傳回的植物影像檔](./media/functions-create-vnet/plant.png)

您的函式應用程式會連線到網際網路和您的虛擬網路。 Proxy 會透過公用網際網路接收要求，然後做為簡單的 HTTP proxy，以將該要求轉送到已連線的虛擬網路。 然後 proxy 會透過網際網路將回應轉送回您。

[!INCLUDE [clean-up-section-portal](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>後續步驟

在本教學課程中，WordPress 網站可做為在函數應用程式中使用 proxy 呼叫的 API。 此案例可提供良好的教學課程，因為它很容易設定和視覺化。 您可以使用在虛擬網路內部署的任何其他 API。 您也可以建立具有程式碼的函式，以呼叫虛擬網路內部署的 Api。 更實際的案例是使用資料用戶端 Api 來呼叫部署于虛擬網路中 SQL Server 實例的函式。

在高階方案中執行的函式會在 >premiumv2 方案上共用與 web 應用程式相同的基礎 App Service 基礎結構。 [Azure App Service 中 web 應用程式](../app-service/overview.md)的所有檔都適用于您的高階方案功能。

> [!div class="nextstepaction"]
> [深入了解 Azure Functions 中的網路功能選項](./functions-networking-options.md)

[進階方案]: functions-premium-plan.md
