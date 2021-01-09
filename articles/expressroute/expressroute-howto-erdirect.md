---
title: Azure ExpressRoute：設定 ExpressRoute Direct
description: 瞭解如何使用 Azure PowerShell 將 Azure ExpressRoute Direct 設定為直接連接至 Microsoft 全球網路。
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: how-to
ms.date: 12/14/2020
ms.author: duau
ms.openlocfilehash: 964af92006aad7b5ce8bdf25a332cbcf9c7ef144
ms.sourcegitcommit: 42a4d0e8fa84609bec0f6c241abe1c20036b9575
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98014513"
---
# <a name="how-to-configure-expressroute-direct"></a>如何設定 ExpressRoute Direct

ExpressRoute Direct 可讓您透過策略性分散在世界各地的對等互連位置，直接連線到 Microsoft 的全球網路。 如需詳細資訊，請參閱[關於 ExpressRoute Direct](expressroute-erdirect-about.md)。

## <a name="before-you-begin"></a>開始之前

使用 ExpressRoute Direct 之前，您必須先註冊您的訂用帳戶。 若要註冊，請將電子郵件與您的訂用帳戶識別碼傳送至 <ExpressRouteDirect@microsoft.com>，其中包含下列詳細資訊：

* 您想要使用 **ExpressRoute Direct** 完成的案例
* 位置喜好設定 - 請參閱[合作夥伴和對等互連位置](expressroute-locations-providers.md)以取得所有位置的完整清單
* 實作的時間軸
* 有任何其他問題嗎

## <a name="create-the-resource"></a><a name="resources"></a>建立資源

1. 登入 Azure 並選取訂用帳戶。 ExpressRoute Direct 資源和 ExpressRoute 線路必須位於相同的訂用帳戶中。

   ```powershell
   Connect-AzAccount 

   Select-AzSubscription -Subscription "<SubscriptionID or SubscriptionName>"
   ```
   
2. 重新註冊您的訂用帳戶至 Microsoft，以存取 expressrouteportslocation 和 expressrouteport Api。

   ```powershell
   Register-AzResourceProvider -ProviderNameSpace "Microsoft.Network"
   ```   
3. 列出支援 ExpressRoute Direct 的所有位置。
  
   ```powershell
   Get-AzExpressRoutePortsLocation
   ```

   **範例輸出**
  
   ```powershell
   Name                : Equinix-Ashburn-DC2
   Id                  : /subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-Ashburn-D
                        C2
   ProvisioningState   : Succeeded
   Address             : 21715 Filigree Court, DC2, Building F, Ashburn, VA 20147
   Contact             : support@equinix.com
   AvailableBandwidths : []

   Name                : Equinix-Dallas-DA3
   Id                  : /subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-Dallas-DA
                        3
   ProvisioningState   : Succeeded
   Address             : 1950 N. Stemmons Freeway, Suite 1039A, DA3, Dallas, TX 75207
   Contact             : support@equinix.com
   AvailableBandwidths : []

   Name                : Equinix-San-Jose-SV1
   Id                  : /subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-San-Jose-
                        SV1
   ProvisioningState   : Succeeded
   Address             : 11 Great Oaks Blvd, SV1, San Jose, CA 95119
   Contact             : support@equinix.com
   AvailableBandwidths : []
   ```
4. 判斷以上所列的位置是否有可用的頻寬

   ```powershell
   Get-AzExpressRoutePortsLocation -LocationName "Equinix-San-Jose-SV1"
   ```

   **範例輸出**

   ```powershell
   Name                : Equinix-San-Jose-SV1
   Id                  : /subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-San-Jose-
                        SV1
   ProvisioningState   : Succeeded
   Address             : 11 Great Oaks Blvd, SV1, San Jose, CA 95119
   Contact             : support@equinix.com
   AvailableBandwidths : [
                          {
                            "OfferName": "100 Gbps",
                            "ValueInGbps": 100
                          }
                        ]
   ```
5. 根據以上所選的位置建立 ExpressRoute Direct 資源

   ExpressRoute Direct 支援 QinQ 與 Dot1Q 封裝。 如果選取 QinQ，則每個 ExpressRoute 線路都會動態獲得指派的 S-Tag，並將成為整個 ExpressRoute Direct 資源中唯一的。 線路上的每個 C-Tag 必須是線路上唯一的，但不是 ExpressRoute Direct 上唯一的。  

   如果選取 Dot1Q 封裝，您必須管理 C-Tag (VLAN) 在整個 ExpressRoute Direct 資源中的唯一性。  

   > [!IMPORTANT]
   > ExpressRoute Direct 只能是一種封裝類型。 建立 ExpressRoute Direct 之後，就無法變更封裝。
   > 
 
   ```powershell 
   $ERDirect = New-AzExpressRoutePort -Name $Name -ResourceGroupName $ResourceGroupName -PeeringLocation $PeeringLocationName -BandwidthInGbps 100.0 -Encapsulation QinQ | Dot1Q -Location $AzureRegion
   ```

   > [!NOTE]
   > 封裝屬性也可以設定為 Dot1Q。 
   >

   **範例輸出︰**

   ```powershell
   Name                       : Contoso-Direct
   ResourceGroupName          : Contoso-Direct-rg
   Location                   : westcentralus
   Id                         : /subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/exp
                               ressRoutePorts/Contoso-Direct
   Etag                       : W/"<etagnumber> "
   ResourceGuid               : <number>
   ProvisioningState          : Succeeded
   PeeringLocation            : Equinix-Seattle-SE2
   BandwidthInGbps            : 100
   ProvisionedBandwidthInGbps : 0
   Encapsulation              : QinQ
   Mtu                        : 1500
   EtherType                  : 0x8100
   AllocationDate             : Saturday, September 1, 2018
   Links                      : [
                                 {
                                   "Name": "link1",
                                   "Etag": "W/\"<etagnumber>\"",
                                   "Id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.
                               Network/expressRoutePorts/Contoso-Direct/links/link1",
                                   "RouterName": "tst-09xgmr-cis-1",
                                   "InterfaceName": "HundredGigE2/2/2",
                                   "PatchPanelId": "PPID",
                                   "RackId": "RackID",
                                   "ConnectorType": "SC",
                                   "AdminState": "Disabled",
                                   "ProvisioningState": "Succeeded"
                                 },
                                 {
                                   "Name": "link2",
                                   "Etag": "W/\"<etagnumber>\"",
                                   "Id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.
                               Network/expressRoutePorts/Contoso-Direct/links/link2",
                                   "RouterName": "tst-09xgmr-cis-2",
                                   "InterfaceName": "HundredGigE2/2/2",
                                   "PatchPanelId": "PPID",
                                   "RackId": "RackID",
                                   "ConnectorType": "SC",
                                   "AdminState": "Disabled",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]
   Circuits                   : []
   ```

## <a name="generate-the-letter-of-authorization-loa"></a><a name="authorization"></a>產生 (LOA 的授權信件) 

參考最近建立的 ExpressRoute Direct 資源、輸入客戶名稱以寫入 LOA， (選擇性地) 定義要儲存檔的檔案位置。 如果未參考檔案路徑，檔會下載到目前的目錄。

  ```powershell 
   New-AzExpressRoutePortLOA -ExpressRoutePort $ERDirect -CustomerName TestCustomerName -Destination "C:\Users\SampleUser\Downloads" 
   ```
 **範例輸出**

   ```powershell
   Written Letter of Authorization To: C:\Users\SampleUser\Downloads\LOA.pdf
   ```

## <a name="change-admin-state-of-links"></a><a name="state"></a>變更連結的系統管理狀態
   
此程序應用於進行第 1 層測試，確保每個交叉連線都已在每個主要和次要路由器中正確修補。
1. 取得 ExpressRoute Direct 詳細資料。

   ```powershell
   $ERDirect = Get-AzExpressRoutePort -Name $Name -ResourceGroupName $ResourceGroupName
   ```
2. 將 [連結] 設定為 [已啟用]。 重複此步驟，將每個連結設定為 [已啟用]。

   連結 [0] 是主要連接埠，而連結 [1] 是次要連接埠。

   ```powershell
   $ERDirect.Links[0].AdminState = "Enabled"
   Set-AzExpressRoutePort -ExpressRoutePort $ERDirect
   $ERDirect = Get-AzExpressRoutePort -Name $Name -ResourceGroupName $ResourceGroupName
   $ERDirect.Links[1].AdminState = "Enabled"
   Set-AzExpressRoutePort -ExpressRoutePort $ERDirect
   ```
   **範例輸出︰**

   ```powershell
   Name                       : Contoso-Direct
   ResourceGroupName          : Contoso-Direct-rg
   Location                   : westcentralus
   Id                         : /subscriptions/<number>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/exp
                             ressRoutePorts/Contoso-Direct
   Etag                       : W/"<etagnumber> "
   ResourceGuid               : <number>
   ProvisioningState          : Succeeded
   PeeringLocation            : Equinix-Seattle-SE2
   BandwidthInGbps            : 100
   ProvisionedBandwidthInGbps : 0
   Encapsulation              : QinQ
   Mtu                        : 1500
   EtherType                  : 0x8100
   AllocationDate             : Saturday, September 1, 2018
   Links                      : [
                               {
                                 "Name": "link1",
                                 "Etag": "W/\"<etagnumber>\"",
                                 "Id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.
                             Network/expressRoutePorts/Contoso-Direct/links/link1",
                                 "RouterName": "tst-09xgmr-cis-1",
                                 "InterfaceName": "HundredGigE2/2/2",
                                 "PatchPanelId": "PPID",
                                 "RackId": "RackID",
                                 "ConnectorType": "SC",
                                 "AdminState": "Enabled",
                                 "ProvisioningState": "Succeeded"
                               },
                               {
                                 "Name": "link2",
                                 "Etag": "W/\"<etagnumber>\"",
                                 "Id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.
                             Network/expressRoutePorts/Contoso-Direct/links/link2",
                                 "RouterName": "tst-09xgmr-cis-2",
                                 "InterfaceName": "HundredGigE2/2/2",
                                 "PatchPanelId": "PPID",
                                 "RackId": "RackID",
                                 "ConnectorType": "SC",
                                 "AdminState": "Enabled",
                                 "ProvisioningState": "Succeeded"
                               }
                             ]
   Circuits                   : []
   ```

   使用與 `AdminState = "Disabled"` 相同的程序來關閉連接埠。

## <a name="create-a-circuit"></a><a name="circuit"></a>建立線路

根據預設，您可以在 ExpressRoute Direct 資源所在的訂用帳戶中建立 10 個線路。 這項限制可透過支援人員來增加。 您則負責追蹤已佈建和已使用的頻寬。 已佈建的頻寬是 ExpressRoute Direct 資源上所有線路的頻寬總和，而已使用的頻寬則是基礎實體介面的實際使用量。

ExpressRoute Direct 可以使用額外的線路頻寬來支援上述案例。 這些頻寬是 40 Gbps 和 100 Gbps。

**SkuTier** 可以是 Local、Standard 或 Premium。

**SkuFamily** 只能 [metereddata。 ExpressRoute Direct 不支援無限制。

在 ExpressRoute Direct 資源上建立線路。

  ```powershell
  New-AzExpressRouteCircuit -Name $Name -ResourceGroupName $ResourceGroupName -ExpressRoutePort $ERDirect -BandwidthinGbps 100.0  -Location $AzureRegion -SkuTier Premium -SkuFamily MeteredData 
  ```

  其他頻寬包括：5.0、10.0 和 40.0

  **範例輸出︰**

  ```powershell
  Name                             : ExpressRoute-Direct-ckt
  ResourceGroupName                : Contoso-Direct-rg
  Location                         : westcentralus
  Id                               : /subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Netwo
                                   rk/expressRouteCircuits/ExpressRoute-Direct-ckt
  Etag                             : W/"<etagnumber>"
  ProvisioningState                : Succeeded
  Sku                              : {
                                     "Name": "Premium_MeteredData",
                                     "Tier": "Premium",
                                     "Family": "MeteredData"
                                   }
  CircuitProvisioningState         : Enabled
  ServiceProviderProvisioningState : Provisioned
  ServiceProviderNotes             : 
    ServiceProviderProperties        : null
  ExpressRoutePort                 : {
                                     "Id": "/subscriptions/<subscriptionID>n/resourceGroups/Contoso-Direct-rg/providers/Micros
                                   oft.Network/expressRoutePorts/Contoso-Direct"
                                   }
  BandwidthInGbps                  : 10
  Stag                             : 2
  ServiceKey                       : <number>
  Peerings                         : []
  Authorizations                   : []
  AllowClassicOperations           : False
  GatewayManagerEtag     
  ```

## <a name="next-steps"></a>後續步驟

如需 ExpressRoute Direct 的詳細資訊，請參閱 [總覽](expressroute-erdirect-about.md)。
