---
title: 使用 Azure 訂用帳戶對應建立資源集區
titleSuffix: Azure VMware Solution by CloudSimple
description: 說明如何透過 Azure 訂用帳戶對應建立私人雲端的資源集區
author: Ajayan1008
ms.author: v-hborys
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 7be4a4c601d3f33972c1e52596ef623116dcadd4
ms.sourcegitcommit: d7d5f0da1dda786bda0260cf43bd4716e5bda08b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/05/2021
ms.locfileid: "97897071"
---
# <a name="create-resource-pools-for-your-private-cloud-with-azure-subscription-mapping"></a>使用 Azure 訂用帳戶對應建立私人雲端的資源集區
Azure 訂用帳戶對應可讓您從可用的 vSphere 資源集區建立私人雲端的資源集區。 在 CloudSimple 入口網站中，您可以查看和管理私人雲端的 Azure 訂用帳戶。

> [!NOTE]
> 對應資源集區也會對應任何子資源集區。 如果已對應任何子資源集區，則無法對應父資源集區。

1. [存取 CloudSimple 入口網站](access-cloudsimple-portal.md)。
2. 開啟 [ **資源** ] 頁面，然後選取 [Azure 訂用帳戶 **對應**]。  
3. 按一下 [ **編輯 Azure 訂** 用帳戶對應]。  
4. 若要對應可用的資源集區，請在左側選取它們，然後按一下向右箭號。 
5. 若要移除對應，請在右側選取它們，然後按一下向左箭號。 

    ![Azure 訂用帳戶](media/resources-azure-mapping.png)

6. 按一下 [確定]  。
