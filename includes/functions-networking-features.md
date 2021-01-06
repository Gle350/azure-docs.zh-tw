---
ms.openlocfilehash: 2e92d150851c74a84f785d1f5f0ebe2e5870a54e
ms.sourcegitcommit: 2aa52d30e7b733616d6d92633436e499fbe8b069
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97936750"
---


| 功能 |[取用方案](../articles/azure-functions/consumption-plan.md)|[進階方案](../articles/azure-functions/functions-premium-plan.md)|[專用方案](../articles/azure-functions/dedicated-plan.md)|[ASE](../articles/app-service/environment/intro.md)| [Kubernetes](../articles/azure-functions/functions-kubernetes-keda.md) |
|----------------|-----------|----------------|---------|-----------------------| ---|
|[輸入 IP 限制和私人網站存取](../articles/azure-functions/functions-networking-options.md#inbound-access-restrictions)|✅是|✅是|✅是|✅是|✅是|
|[虛擬網路整合](../articles/azure-functions/functions-networking-options.md#virtual-network-integration)|❌否|✅是 (區域性)|✅是 (區域性和閘道)|✅是| ✅是|
|[虛擬網路觸發程序 (非 HTTP)](../articles/azure-functions/functions-networking-options.md#virtual-network-triggers-non-http)|❌否| ✅是 |✅是|✅是|✅是|
|[混合式連線](../articles/azure-functions/functions-networking-options.md#hybrid-connections) (僅限 Windows)|❌否|✅是|✅是|✅是|✅是|
|[輸出 IP 限制](../articles/azure-functions/functions-networking-options.md#outbound-ip-restrictions)|❌否| ✅是|✅是|✅是|✅是|