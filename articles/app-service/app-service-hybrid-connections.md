---
title: 混合式連線
description: 瞭解如何在 Azure App Service 中建立及使用混合式連線，以存取不同網路中的資源。
author: ccompy
ms.assetid: 66774bde-13f5-45d0-9a70-4e9536a4f619
ms.topic: article
ms.date: 06/08/2020
ms.author: ccompy
ms.custom: seodec18, fasttrack-edit
ms.openlocfilehash: 16f6a0660fa9aa20f636ee412f3f337bd5dea9b5
ms.sourcegitcommit: e7179fa4708c3af01f9246b5c99ab87a6f0df11c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/30/2020
ms.locfileid: "97825976"
---
# <a name="azure-app-service-hybrid-connections"></a>Azure App Service 混合式連線

混合式連線既是 Azure 服務，也是 Azure App Service 功能。 作為服務時，它具有 App Service 所利用之用途和功能以外的用途和功能。 若要深入了解混合式連線及其在 App Service 之外的使用方式，請參閱 [Azure 轉送混合式連線][HCService]。

在 App Service 中，您可以使用混合式連線來存取任何網路中的應用程式資源，這些資源可以透過埠443對 Azure 進行輸出呼叫。 混合式連線可讓您從應用程式存取 TCP 端點，而且不會啟用新的方式來存取您的應用程式。 和在 App Service 中使用時相同，每個「混合式連線」都會與單一 TCP 主機和連接埠的組合相互關聯。 這可讓您的應用程式存取任何 OS 上的資源（前提是它是 TCP 端點）。 混合式連線不會知道 (或不在意) 應用程式通訊協定為何，或您要存取什麼資源。 它只會提供網路存取。  

## <a name="how-it-works"></a>運作方式 ##
混合式連線需要部署轉送代理程式，讓它可以同時連線到所需的端點以及 Azure。 轉送代理程式混合式連線管理員 (HCM) ，會透過埠443呼叫 Azure 轉送。 從 web 應用程式網站，App Service 基礎結構也會代表您的應用程式連接到 Azure 轉送。 透過已加入的連接，您的應用程式就能夠存取所需的端點。 連線會使用 TLS 1.2 以求安全性，並使用共用存取簽章 (SAS) 金鑰來進行驗證和授權。    

![混合式連線概要流程圖表][1]

當應用程式提出的 DNS 要求與所設定的「混合式連線」端點相符時，系統就會透過「混合式連線」將輸出 TCP 流量重新導向。  

> [!NOTE]
> 這表示您應該嘗試一律對混合式連線使用 DNS 名稱。 如果端點改為使用 IP 位址，某些用戶端軟體不會執行 DNS 查閱。 
>

### <a name="app-service-hybrid-connection-benefits"></a>App Service 混合式連線優點 ###

「混合式連線」功能有一些優點，包括：

- 應用程式可以安全地存取內部部署系統和服務。
- 此功能不需要可存取網際網路的端點。
- 安裝速度快且過程簡單。 不需要閘道
- 每個「混合式連線」都會與單一的「主機:連接埠」組合對應，有助於提高安全性。
- 通常不需要在防火牆開洞。 連線全都是透過標準的 Web 連接埠輸出。
- 由於這是網路層級的功能，因此不會因為應用程式所使用的語言及端點所使用的技術而受到影響。
- 它可用來讓您從單一應用程式存取多個網路。 
- 適用于 Windows 原生應用程式的 GA 支援，並且在 Linux 應用程式中為預覽狀態。 Windows 容器應用程式不支援此功能。

### <a name="things-you-cannot-do-with-hybrid-connections"></a>混合式連線無法執行的作業 ###

混合式連線無法執行的作業包括：

- 裝載磁碟。
- 使用 UDP。
- 存取使用動態連接埠的 TCP 型服務 (例如「FTP 被動模式」或「延伸被動模式」)。
- 支援 LDAP，因為它需要 UDP。
- 支援 Active Directory，因為您無法將 App Service 背景工作角色加入網域。 

## <a name="add-and-create-hybrid-connections-in-your-app"></a>在您的應用程式中新增和建立混合式連線 ##

若要建立「混合式連線」，請移至 [Azure 入口網站][portal]，然後選取您的應用程式。 選取 [**網路**]  >  **設定您的混合式連接端點**。 您可以在這裡看到為您應用程式設定的「混合式連線」。  

![混合式連線清單的螢幕擷取畫面][2]

若要新增混合式連線，請選取 [[+] 新增混合式連線]。  您會看見已建立的混合式連線清單。 若要在應用程式中新增其中的一或多個混合式連線，請按一下您想要的混合式連線，然後選取 [新增選取的混合式連線]。  

![混合式連線入口網站的螢幕擷取畫面][3]

如果您想要建立新的「混合式連線」，請選取 [建立新的混合式連線]。 指定下列項目： 

- 混合式連線名稱。
- 端點主機名稱。
- 端點連接埠。
- 您想要使用的「服務匯流排」命名空間。

![[建立新的混合式連線] 對話方塊螢幕擷取畫面][4]

每個「混合式連線」都會繫結至一個「服務匯流排」命名空間，而每個「服務匯流排」命名空間都位於 Azure 區域中。 請務必嘗試使用與應用程式位於相同區域的「服務匯流排」命名空間，以避免網路所引發的延遲。

如果您想要移除應用程式中的「混合式連線」，請對它按一下滑鼠右鍵，然後選取 [中斷連線]。  

將「混合式連線」新增至應用程式之後，您只要選取它就可以查看詳細資料。 

![混合式連線詳細資料的螢幕擷取畫面][5]

### <a name="create-a-hybrid-connection-in-the-azure-relay-portal"></a>在 Azure 轉送入口網站中建立混合式連線 ###

除了透過入口網站從您應用程式內進行的方式之外，您也可以從「Azure 轉送」入口網站內建立「混合式連線」。 若要讓 App Service 使用混合式連線，必須：

* 要求用戶端授權。
* 擁有一個以「主機:連接埠」組合作為值的中繼資料項目具名端點。

## <a name="hybrid-connections-and-app-service-plans"></a>混合式連線和 App Service 方案 ##

只有在「基本」、「標準」、「進階」及「隔離」定價 SKU 中，才有提供「App Service 混合式連線」。 定價方案會有相關聯的限制。  

| 定價方案 | 方案中可用的混合式連線數目 |
|----|----|
| Basic | 每個方案 5 個 |
| 標準 | 每個方案 25 個 |
| >premiumv2 | 每個應用程式 200 個 |
| 隔離 | 每個應用程式 200 個 |

App Service 方案 UI 會顯示您正在使用的混合式連線數目，以及由哪些應用程式使用。  

![App Service 方案屬性的螢幕擷取畫面][6]

請選取 [混合式連線] 以查看詳細資料。 您可以看到在應用程式檢視中所看到的所有資訊。 您也可以查看在相同的方案中，有多少其他的應用程式使用了該混合式連線。

可用於 App Service 方案的混合式連線端點數目有其上限。 不過，每個使用的混合式連線，則可用於方案中任何數目的應用程式上。 例如，一個在「App Service 方案」中 5 個個別應用程式上使用的「混合式連線」，只算 1 個「混合式連線」。

### <a name="pricing"></a>定價 ###

除了有 App Service 方案 SKU 的需求，還有使用混合式連線的額外成本。 混合式連線所使用的每個接聽程式皆會產生費用。 接聽程式是混合式連線管理員。 如果由兩個混合式連線管理員支援的混合式連線有五個，那就有 10 個接聽程式。 如需詳細資訊，請參閱[服務匯流排價格][sbpricing]。

## <a name="hybrid-connection-manager"></a>混合式連線管理員 ##

「混合式連線」功能需要在裝載「混合式連線」端點的網路中有轉送代理程式。 該轉送代理程式稱為混合式連線管理員 (HCM)。 若要下載 HCM，請在 [Azure 入口網站][portal] 中，從您的應用程式選取 [網路] > [設定您的混合式連接端點]。  

此工具可在 Windows Server 2012 和更新版本上執行。 HCM 以服務形式執行，並向外連線到連接埠 443 上的 Azure 轉送。  

在安裝 HCM 之後，您可以執行 HybridConnectionManagerUi.exe 來使用工具的 UI。 此檔案位於混合式連線管理員的安裝目錄下。 在 Windows 10 中，您也可以在搜尋方塊中搜尋「混合式連線管理員 UI」即可。  

![混合式連線管理員的螢幕擷取畫面][7]

啟動 HCM UI 時，您會先看到一個表格，當中會列出與這個 HCM 執行個體搭配設定的所有「混合式連線」。 如果您想要進行任何變更，請先向 Azure 進行驗證。 

若要在 HCM 中新增一或多個混合式連線︰

1. 啟動 HCM UI。
2. 選取 [設定另一個混合式連線]。
![設定新的混合式連線螢幕擷取畫面][8]

1. 使用您的 Azure 帳戶登入，以取得您的訂用帳戶可用的混合式連線。 HCM 不會繼續使用您的 Azure 帳戶超過該帳戶。 
1. 選擇訂用帳戶。
1. 選取您要讓 HCM 轉送的「混合式連線」。
![混合式連線的螢幕擷取畫面][9]

1. 選取 [儲存]。

現在可以看到您新增的「混合式連線」。 您也可以選取已設定的混合式連線，以查看詳細資料。

![混合式連線詳細資料的螢幕擷取畫面][10]

若要讓 HCM 能夠支援所設定與其搭配的「混合式連線」，它需要︰

- 對 Azure 的 TCP 存取權 (透過連接埠 443)。
- 對「混合式連線」端點的 TCP 存取權。
- 能夠對端點主機和「服務匯流排」命名空間執行 DNS 查閱。

> [!NOTE]
> 「Azure 轉送」需倚賴「Web 通訊端」來取得連線能力。 此功能只有在 Windows Server 2012 或更新版本上才有提供。 因此，Windows Server 2012 以前的所有版本皆不支援 HCM。
>

### <a name="redundancy"></a>備援性 ###

每個 HCM 都可以支援多個「混合式連線」。 此外，任何指定的「混合式連線」也都可以受到多個 HCM 支援。 預設行為是在任何給定端點所設定的 HCM 之間路由傳送流量。 如果您想要讓來自您網路的「混合式連線」具有高可用性，請在不同電腦上執行多個 HCM。 轉送服務用來將流量散發至 HCM 的負載分配演算法會隨機指派。 

### <a name="manually-add-a-hybrid-connection"></a>手動新增混合式連線 ###

若要想讓訂用帳戶外的人員裝載指定「混合式連線」的 HCM 執行個體，您可以與他們共用「混合式連線」的閘道連接字串。 您可以在 [Azure 入口網站][portal]中的混合式連線屬性中看到閘道連接字串。 若要使用該字串，請在 HCM 中選取 [手動輸入]，然後貼上閘道連接字串。

![手動新增混合式連線][11]

### <a name="upgrade"></a>升級 ###

混合式連線管理員會定期更新，以修正問題或改善效能。 當升級程式發行時，HCM UI 中會出現快顯視窗。 套用升級後，HCM 會發生變更並重新啟動。 

## <a name="adding-a-hybrid-connection-to-your-app-programmatically"></a>以程式設計方式將混合式連線新增至應用程式 ##

混合式連線有 Azure CLI 支援。 提供的命令會在應用程式和 App Service 計畫層級運作。  應用層級命令為：

```azurecli
az webapp hybrid-connection

Group
    az webapp hybrid-connection : Methods that list, add and remove hybrid-connections from webapps.
        This command group is in preview. It may be changed/removed in a future release.
Commands:
    add    : Add a hybrid-connection to a webapp.
    list   : List the hybrid-connections on a webapp.
    remove : Remove a hybrid-connection from a webapp.
```

App Service plan 命令可讓您設定指定的混合式連接將使用的金鑰。 每個混合式連線（主要和次要）都會設定兩個索引鍵。 您可以選擇使用主要或次要金鑰搭配下列命令。 當您想要定期重新產生金鑰時，這可讓您切換的金鑰。 

```azurecli
az appservice hybrid-connection --help

Group
    az appservice hybrid-connection : A method that sets the key a hybrid-connection uses.
        This command group is in preview. It may be changed/removed in a future release.
Commands:
    set-key : Set the key that all apps in an appservice plan use to connect to the hybrid-
                connections in that appservice plan.
```

## <a name="secure-your-hybrid-connections"></a>保護混合式連接 ##

您可以將現有的混合式連線新增至具有基礎 Azure 服務匯流排轉送的足夠許可權之任何使用者的其他 App Service Web Apps。 這表示，如果您必須防止其他人重複使用相同的混合式連線 (例如，當目標資源是沒有任何額外安全性措施可防止未經授權存取) 的服務時，您必須鎖定 Azure 服務匯流排轉送的存取權。

擁有轉送存取權的任何人都可以在 `Reader` 嘗試將其新增至 Azure 入口網站中的 Web 應用程式時， _看到_ 混合式連線，但因為缺少取得用來建立轉送連線之連接字串的許可權，所以將無法 _新增_ 該連接。 為了成功新增混合式連線，它們必須具有 `listKeys` () 的許可權 `Microsoft.Relay/namespaces/hybridConnections/authorizationRules/listKeys/action` 。 在 `Contributor` 轉送上包含此許可權的角色或任何其他角色，可讓使用者使用混合式連線，並將其新增至自己的 Web Apps。

## <a name="troubleshooting"></a>疑難排解 ##

「已連線」狀態意謂著該「混合式連線」至少有一個已設定的 HCM，並且能夠連線至 Azure。 如果「混合式連線」的狀態並未顯示「已連線」，則表示在所有可存取 Azure 的 HCM 上皆未設定該「混合式連線」。

用戶端無法連線至其端點的主要原因是，端點在指定時所使用的是 IP 位址而非 DNS 名稱。 如果您的應用程式無法連線到所要的端點，且您使用的是 IP 位址，請改為使用對 HCM 執行所在之主機有效的 DNS 名稱。 也請檢查 HCM 執行所在的主機已正確解析 DNS 名稱。 請確認 HCM 執行所在的主機可連線到「混合式連線」端點。  

在 App Service 中，您可以從 [Advanced Tools (Kudu]) 主控台叫用 **tcpping** 命令列工具。 這個工具可以指出您是否能夠存取 TCP 端點，但不會指出您是否能夠存取「混合式連線」端點。 當您在主控台中對「混合式連線」端點使用此工具時，您只能確認該端點使用「主機:連接埠」組合。  

如果您的端點有命令列用戶端，您可以從應用程式主控台測試連線能力。 例如，您可以使用捲曲測試對 web 伺服器端點的存取。


<!--Image references-->
[1]: ./media/app-service-hybrid-connections/hybridconn-connectiondiagram.png
[2]: ./media/app-service-hybrid-connections/hybridconn-portal.png
[3]: ./media/app-service-hybrid-connections/hybridconn-addhc.png
[4]: ./media/app-service-hybrid-connections/hybridconn-createhc.png
[5]: ./media/app-service-hybrid-connections/hybridconn-properties.png
[6]: ./media/app-service-hybrid-connections/hybridconn-aspproperties.png
[7]: ./media/app-service-hybrid-connections/hybridconn-hcm.png
[8]: ./media/app-service-hybrid-connections/hybridconn-hcmadd.png
[9]: ./media/app-service-hybrid-connections/hybridconn-hcmadded.png
[10]: ./media/app-service-hybrid-connections/hybridconn-hcmdetails.png
[11]: ./media/app-service-hybrid-connections/hybridconn-manual.png
[12]: ./media/app-service-hybrid-connections/hybridconn-bt.png

<!--Links-->
[HCService]: /azure/service-bus-relay/relay-hybrid-connections-protocol/
[portal]: https://portal.azure.com/
[oldhc]: /azure/biztalk-services/integration-hybrid-connection-overview/
[sbpricing]: https://azure.microsoft.com/pricing/details/service-bus/
[armclient]: https://github.com/projectkudu/ARMClient/
