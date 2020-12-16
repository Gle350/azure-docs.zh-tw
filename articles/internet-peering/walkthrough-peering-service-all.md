---
title: 對等服務夥伴逐步解說
titleSuffix: Azure
description: 對等服務夥伴逐步解說
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: how-to
ms.date: 12/15/2020
ms.author: prmitiki
ms.openlocfilehash: 292c850f5352c80112e3f0bcc5c868f8128819be
ms.sourcegitcommit: d2d1c90ec5218b93abb80b8f3ed49dcf4327f7f4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/16/2020
ms.locfileid: "97592308"
---
# <a name="peering-service-partner-walkthrough"></a>對等服務夥伴逐步解說

本節說明提供者針對對等互連服務啟用直接對等互連所需遵循的步驟。

## <a name="create-direct-peering-connection-for-peering-service"></a>建立對等互連服務的直接對等互連連線
服務提供者可以藉由建立支援對等互連服務的新直接對等互連來擴展其地理範圍。 若要完成此工作，
1. 如果尚未成為對等互連服務合作夥伴
1. 遵循指示， [使用入口網站建立或修改直接對等互連](howto-direct-portal.md)。 確定它符合高可用性需求。
1. 然後，遵循下列步驟， [使用入口網站在直接對等互連上啟用對等互連服務](howto-peering-service-portal.md)。

## <a name="use-legacy-direct-peering-connection-for-peering-service"></a>使用對等互連服務的舊版直接對等互連連線
如果您有想要用來支援對等互連服務的舊版直接對等互連，
1. 如果尚未成為對等互連服務夥伴，則成為它。
1. 遵循指示， [使用入口網站將舊版直接對等互連轉換為 Azure 資源](howto-legacy-direct-portal.md)。 如有需要，請訂購額外的線路以符合高可用性需求。
1. 然後，遵循下列步驟， [使用入口網站在直接對等互連上啟用對等互連服務](howto-peering-service-portal.md)。

## <a name="next-steps"></a>後續步驟

* 深入瞭解對 [等互連原則](https://peering.azurewebsites.net/peering)。
* 若要瞭解設定與 Microsoft 直接對等互連的步驟，請遵循 [直接對等互連逐步](walkthrough-direct-all.md)解說。
* 若要瞭解設定與 Microsoft 交換對等互連的步驟，請遵循 [Exchange 對等互連逐步](walkthrough-exchange-all.md)解說。