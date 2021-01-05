---
title: Azure VMware Solution by CloudSimple-設定防火牆資料表和規則
description: 說明如何設定私用雲端防火牆資料表和規則，以限制子網和 Vlan 的流量。
author: Ajayan1008
ms.author: v-hborys
ms.date: 08/15/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 32dcfb0c6cd26b5f34afad328db8f383fa1c2a6f
ms.sourcegitcommit: d7d5f0da1dda786bda0260cf43bd4716e5bda08b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/05/2021
ms.locfileid: "97895626"
---
# <a name="set-up-firewall-tables-and-rules-for-private-clouds"></a>設定私人雲端的防火牆資料表和規則

防火牆資料表和相關聯的規則可讓您指定要套用至特定子網和 Vlan 之流量的限制。

* 子網可以與一個防火牆資料表相關聯。
* 防火牆資料表可以與多個子網相關聯。

## <a name="add-a-new-firewall-table"></a>新增防火牆資料表

1. [存取 CloudSimple 入口網站](access-cloudsimple-portal.md) ，然後選取側邊功能表上的 [ **網路** ]。
2. 選取 [ **防火牆資料表]**。
3. 選取 [ **建立防火牆資料表**]。

    ![VLAN/子網頁面](media/firewall-tables-page.png)

4. 輸入資料表的名稱。
5. 資料表的預設規則會列出。 按一下 [ **建立新規則** ] 以建立其他規則。 請參閱下列程式以取得詳細資料。
6. 按一下 [ **完成** ] 以儲存防火牆資料表。

> [!IMPORTANT]
> 您可以為每個私人雲端建立最多兩個防火牆資料表。

## <a name="firewall-rules"></a>防火牆規則

防火牆規則會決定防火牆如何處理特定類型的流量。 所選防火牆資料表的 [ **規則** ] 索引標籤會列出所有相關聯的規則。

![防火牆規則資料表](media/firewall-rules-tab.png)

## <a name="create-a-firewall-rule"></a>建立防火牆規則

1. 以下列其中一種方式顯示設定來建立防火牆規則：
    * 建立防火牆資料表時，請按一下 [ **新增規則** ]。
    * 在 [ **網路 > 防火牆資料表]** 頁面上選取特定的防火牆資料表，然後按一下 [ **建立新的防火牆規則**]。
2. 設定規則，如下所示：
    * **名稱**。 指定規則的名稱。
    * **優先權**。 指派規則的優先順序。 較低數位的規則會先執行。
    * **流量類型**。 選取此規則適用于私人雲端、網際網路或 VPN 流量 (無狀態) ，或 (具狀態) 的公用 IP 位址。
    * **通訊協定**。 選取規則所涵蓋的通訊協定， (TCP、UDP 或任何通訊協定) 。
    * **方向**。 選取規則適用于輸入或輸出流量。 您必須為輸入和輸出流量定義個別的規則。
    * **動作**。 如果規則符合 (允許或拒絕) ，請選取要採取的動作。
    * **來源**。 指定規則所涵蓋的來源 (CIDR 區塊、內部或任何來源) 。
    * **來源埠範圍**。 指定受規則規範的埠範圍。
    * **方向**。 選取 [輸入] 或 [輸出]。
    * **目的地**。 指定規則所涵蓋的目的地 (CIDR 區塊、內部或任何來源) 。
    * **來源埠範圍**。 指定受規則規範的埠範圍。

    ![防火牆資料表新增規則](media/firewall-rule-create.png)

3. 按一下 [ **完成** ] 以儲存規則，並將它新增至防火牆資料表的規則清單。

> [!IMPORTANT]
> 每個防火牆資料表最多可以有10個輸入規則和20個輸出規則。 您可以 [聯繫支援人員](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)來增加這些限制。

## <a name="attach-vlanssubnets"></a><a name="attach-vlans-subnet"></a>附加 Vlan/子網

定義防火牆資料表之後，您可以指定受限於資料表中之規則的子網。

1. 在 [**網路**  >  **防火牆資料表]** 頁面上，選取防火牆資料表。
2. 開啟 [ **附加的 vlan/子網** ] 索引標籤。
3. 按一下 [ **附加至 VLAN/子網**]。
4. 選取私人雲端和 VLAN。 會顯示相關聯的子網名稱和 CIDR 區塊。
5. 按一下 [提交]  。
