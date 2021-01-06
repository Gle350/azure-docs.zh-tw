---
title: App Service 方案
description: 瞭解 App Service 方案在 Azure App Service、如何向客戶收費，以及如何根據您的需求進行調整。
keywords: App Service, Azure App Service, 級別, 可調整, 延展性, App Service 方案, App Service 成本
ms.assetid: dea3f41e-cf35-481b-a6bc-33d7fc9d01b1
ms.topic: article
ms.date: 10/01/2020
ms.custom: seodec18
ms.openlocfilehash: a29d81be9b750d89230a180b8a7c786466d99bb8
ms.sourcegitcommit: 2aa52d30e7b733616d6d92633436e499fbe8b069
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97936425"
---
# <a name="azure-app-service-plan-overview"></a>Azure App Service 方案概觀

在 App Service (Web Apps、API Apps 或 Mobile Apps) 中，應用程式一律會在 _App Service 方案_ 中執行。 此外， [Azure Functions](../azure-functions/dedicated-plan.md) 也可以選擇在 _App Service 方案_ 中執行。 App Service 方案會針對要執行的 Web 應用程式定義一組計算資源。 這些計算資源類似于傳統 web 裝載中的 [_伺服器_](https://wikipedia.org/wiki/Server_farm) 陣列。 一或多個應用程式可設定為在相同的計算資源上執行 (或在相同的 App Service 方案中執行)。

當您在特定區域 (例如，西歐) 建立 App Service 方案時，會為該區域的方案建立一組計算資源。 無論您將何種應用程式置入此 App Service 方案，都會在該 App Service 方案定義的計算資源上執行。 每個 App Service 方案都會定義：

- 區域 (美國西部、美國東部等)
- 虛擬機器執行個體的數目
- 虛擬機器執行個體的大小 (小、中、大)
- 定價層 (免費、共用、基本、標準、Premium、>premiumv2、PremiumV3、隔離) 

App Service 方案的 _定價層_ 可決定您獲得哪些 App Service 功能，以及為該方案支付多少費用。 定價層有幾個類別：

- **共用計算**：**免費** 和 **共用**，這兩個基底層會在與其他 App Service 應用程式相同的 Azure VM 上執行應用程式，包括其他客戶的應用程式。 這些層會將 CPU 配額配置到在共用資源上執行的每個應用程式，而且該資源無法向外延展。
- **專用計算**： **基本**、 **標準**、 **Premium**、 **>premiumv2** 和 **PremiumV3** 層會在專用的 Azure vm 上執行應用程式。 只有位於相同 App Service 方案中的應用程式，才會共用相同的計算資源。 層越高，可用於向外延展的 VM 執行個體就越多。
- **隔離**：此層會在專用的 Azure 虛擬網路上執行專用的 azure vm。 它會針對您的應用程式，在計算隔離之上提供網路隔離。 它提供了最大的向外延展能力。

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

每一層也提供 App Service 功能的特定子集。 這些功能包括自訂網域與 TLS/SSL 憑證、自動調整、部署位置、備份、流量管理員整合等等。 層越高，可用的功能就越多。 若要了解每個定價層支援的功能，請參閱[App Service 方案詳細資料](https://azure.microsoft.com/pricing/details/app-service/plans/)。

<a name="new-pricing-tier-premiumv3"></a>

> [!NOTE]
> 新的 **PremiumV3** 定價層可保證具有更快處理器的機器 (每個虛擬 CPU) 的最小 195 [ACU](../virtual-machines/acu.md) 、SSD 儲存體，以及相較于 **標準** 層的四個記憶體/核心比率。 **PremiumV3** 也透過增加的實例計數支援更高的規模，同時仍提供在 **標準** 層中找到的所有先進功能。 現有 **>premiumv2** 層中提供的所有功能都包含在 **PremiumV3** 中。
>
> 類似於其他專用層，此層有三個可用的 VM 大小：
>
> - Small (2 CPU 核心，8 GiB 的記憶體)  
> - 中型 (4 個 CPU 核心，16 GiB 的記憶體)  
> - 大型 (8 個 CPU 核心，32 GiB 的記憶體)   
>
> 如需 **PremiumV3** 定價資訊，請參閱 [App Service 定價](https://azure.microsoft.com/pricing/details/app-service/)。
>
> 若要開始使用新的 **PremiumV3** 定價層，請參閱 [設定適用于 App Service 的 PremiumV3 層](app-service-configure-premium-tier.md)。

## <a name="how-does-my-app-run-and-scale"></a>我的應用程式如何執行及調整縮放？

在 **免費** 和 **共用** 層中，應用程式會在共用的 VM 實例上收到 CPU 分鐘，而且無法相應放大。在其他階層中，應用程式會以下面方式執行和調整。

當您在 App Service 中建立應用程式時，該應用程式會置入 App Service 方案。 當應用程式執行時，會在 App Service 方案中設定的所有 VM 執行個體上執行。 如果有多個應用程式位於相同的 App Service 方案，它們會共用相同的 VM 執行個體。 如果一個應用程式有多個部署位置，所有部署位置也會在相同的 VM 執行個體上執行。 如果您啟用診斷記錄、執行備份，或執行 WebJob，它們也會使用這些 VM 執行個體上的 CPU 週期和記憶體。

如此一來，App Service 方案是 App Service 應用程式的縮放單位。 如果方案設定為執行五個 VM 執行個體，則方案中的所有應用程式會在所有五個執行個體上執行。 如果方案設定為自動調整，則方案中的所有應用程式會根據自動調整設定一起向外延展。

如需有關向外延展應用程式的詳細資訊，請參閱[手動或自動調整執行個體計數](../azure-monitor/platform/autoscale-get-started.md)。

<a name="cost"></a>

## <a name="how-much-does-my-app-service-plan-cost"></a>我的 App Service 方案費用是多少？

本節描述 App Service 應用程式的計費方式。 如需特定區域價格的詳細資訊，請參閱 [App Service 價格](https://azure.microsoft.com/pricing/details/app-service/)。

除了 **免費** 層之外，App Service 方案會對其使用的計算資源收費。

- 在 **共用** 層中，每個應用程式都會收到 cpu 分鐘的配額，因此 _每個應用程式_ 都需支付 cpu 配額的費用。
- 在「**基本**」、「標準 **」、「高階」、「**>premiumv2」、「**標準**」、「高階」、 ****、 **PremiumV3**) 的專用計算 (層中，APP SERVICE 的方案會定義應用程式所調整的 vm 實例數目，因此 App Service 計畫中的 _每個 vm_ 無論有多少個應用程式在 VM 執行個體上執行，這些 VM 執行個體皆採相同收費。 為了避免產生非預期的費用，請參閱[清除 App Service 方案](app-service-plan-manage.md#delete)。
- 在 **隔離** 層中，App Service 環境會定義執行您應用程式的隔離式背景工作角色數目，而 _每個背景工作_ 會收取費用。 此外，執行 App Service 環境本身會有一般的戳記費用。

使用您 (設定自訂網域、TLS/SSL 憑證、部署位置、備份等 ) 的 App Service 功能時，不需付費。 例外狀況為：

- App Service 網域 - 當您在 Azure 中購買一個網域且採每年續訂時，即需要付費。
- App Service 憑證 - 當您在 Azure 中購買一個憑證且採每年續訂時，即需要付費。
- 以 IP 為基礎的 TLS 連線-每個以 IP 為基礎的 TLS 連線都有每小時的費用，但某些 **標準** 層或更高版本提供您一個以 ip 為基礎的 tls 連線，可供免費使用。 以 SNI 為基礎的 TLS 連接是免費的。

> [!NOTE]
> 如果您將 App Service 與另一個 App Service 整合，可能需要考慮來自其他服務的費用。 例如，如果您使用 Azure 流量管理員來調整您的異地應用程式、Azure 流量管理員也會根據您的使用量來收費。 若要預估您在 Azure 中的跨服務成本，請參閱[價格計算器](https://azure.microsoft.com/pricing/calculator/)。 

想要最佳化並節省您的雲端費用嗎？

[!INCLUDE [cost-management-horizontal](../../includes/cost-management-horizontal.md)]

## <a name="what-if-my-app-needs-more-capabilities-or-features"></a>如果我的應用程式需要更多的功能？

可以隨時相應增加和相應減少您的 App Service 方案。 這與更改方案的定價層一樣簡單。 一開始，您可以選擇較低的定價層，當您之後需要更多 App Service 功能時，再相應增加。

例如，您可以開始測試 **免費** App Service 方案中的 Web 應用程式，無須支付任何費用。 當您想要將 [自訂 DNS 名稱](app-service-web-tutorial-custom-domain.md) 加入 Web 應用程式時，只要將您的方案相應增加至 **共用** 層即可。 稍後，當您想要 [建立 TLS](configure-ssl-bindings.md)系結時，請將您的方案調整到 **基本** 層。 當您想要有 [預備環境](deploy-staging-slots.md)時，相應增加至 **標準** 層。 當您需要更多核心、記憶體或儲存體時，可以在同一層中相應增加到較大的 VM 大小。

反向的運作方式也是一樣。 當您覺得不再需要更高層的功能時，可以相應減少到較低層，這樣可以節省費用。

如需有關相應增加 App Service 方案的詳細資訊，請參閱[在 Azure 中相應增加應用程式](manage-scale-up.md)。

如果您的應用程式與其他應用程式處於相同的 App Service 方案中，您可以藉由隔離計算資源以改善應用程式的效能。 您可以透過將應用程式移到不同的 App Service 方案來實現。 如需詳細資訊，請參閱[將應用程式移至另一個 App Service 方案](app-service-plan-manage.md#move)。

## <a name="should-i-put-an-app-in-a-new-plan-or-an-existing-plan"></a>我應該將應用程式置入新的方案或現有方案？

由於您要針對 App Service 方案配置的計算資源付費 (請參閱[我的 App Service 方案成本是多少？](#cost))，您可以將多個應用程式置入一個 App Service 方案來節省費用。 只要現有方案有足夠資源處理負載，即可繼續將應用程式新增至該方案。 不過，請記住，同個 App Service 方案中的應用程式皆會共用相同的計算資源。 若要判斷新的應用程式是否有所需的資源，您必須了解現有 App Service 方案的容量，以及新應用程式的預期負載。 多載 App Service 方案可能會導致新的和現有的應用程式停機。

如果有下列情況，請將您的應用程式隔離至新的 App Service 方案中：

- 應用程式會耗用大量資源。
- 您想要將應用程式與現有方案中的其他應用程式分開調整。
- 應用程式需要不同地理區域中的資源。

這樣可讓您為應用程式配置一組新的資源，更充分掌控您的應用程式。

## <a name="manage-an-app-service-plan"></a>管理 App Service 方案

> [!div class="nextstepaction"]
> [管理 App Service 方案](app-service-plan-manage.md)