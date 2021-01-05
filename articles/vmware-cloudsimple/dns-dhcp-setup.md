---
title: Azure VMware Solution by CloudSimple-設定私用雲端的工作負載 DNS 和 DHCP
description: 說明如何針對在 CloudSimple 私用雲端環境中執行的應用程式和工作負載設定 DNS 和 DHCP
author: Ajayan1008
ms.author: v-hborys
ms.date: 08/16/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: cdcb3cd7afa660909fad416ca455c041dc50321e
ms.sourcegitcommit: d7d5f0da1dda786bda0260cf43bd4716e5bda08b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/05/2021
ms.locfileid: "97896987"
---
# <a name="set-up-dns-and-dhcp-applications-and-workloads-in-your-cloudsimple-private-cloud"></a>設定 CloudSimple 私人雲端中的 DNS 和 DHCP 應用程式和工作負載

在私用雲端環境中執行的應用程式和工作負載需要名稱解析和 DHCP 服務，以便進行查閱和 IP 位址指派。  必須要有適當的 DHCP 和 DNS 基礎結構，才能提供這些服務。  您可以設定虛擬機器，以在您的私人雲端環境中提供這些服務。  

## <a name="prerequisites"></a>必要條件

* 已設定 VLAN 的分散式埠群組
* 將設定路由傳送至內部部署或以網際網路為基礎的 DNS 伺服器
* 用來建立虛擬機器的虛擬機器範本或 ISO

## <a name="linux-based-dns-server-setup"></a>以 Linux 為基礎的 DNS 伺服器設定

Linux 提供各種套件來設定 DNS 伺服器。  以下是 [來自 DigitalOcean 的範例設定](https://www.digitalocean.com/community/tutorials/how-to-configure-bind-as-a-private-network-dns-server-on-ubuntu-18-04) ，其中包含設定開放原始碼系結 DNS 伺服器的指示。

## <a name="windows-based-setup"></a>以 Windows 為基礎的安裝程式

這些 Microsoft 主題說明如何將 Windows 伺服器設定為 DNS 伺服器和 DHCP 伺服器。

* [作為 DNS 伺服器的 Windows Server](/windows-server/networking/dns/dns-top)
* [Windows Server 做為 DHCP 伺服器](/windows-server/networking/technologies/dhcp/dhcp-top)
