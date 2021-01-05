---
title: 補救 Azure 資訊安全中心中的建議 |Microsoft Docs
description: 本文說明如何回應 Azure 資訊安全中心中的建議，以保護您的資源並滿足安全性原則。
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 8be947cc-cc86-421d-87a6-b1e23077fd50
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/08/2020
ms.author: memildin
ms.openlocfilehash: dabd7e9e2c3c74225efc4611c7ad3523a6c76ba5
ms.sourcegitcommit: 02ed9acd4390b86c8432cad29075e2204f6b1bc3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/29/2020
ms.locfileid: "97807985"
---
# <a name="remediate-recommendations-in-azure-security-center"></a>Azure 資訊安全中心的補救建議

建議提供有關如何更妥善保護您資源的建議。 您可以遵循建議中提供的補救步驟來執行建議。

## <a name="remediation-steps"></a>補救步驟 <a name="remediation-steps"></a>

在查看所有建議之後，請先決定要補救哪一個建議。 建議您將安全性控制的優先順序設為最高的潛能，以提高您的安全分數。

1. 從清單中選取建議。

1. 遵循 **補救步驟** 一節中的指示。 每個建議都有自己的一組指示。 下列螢幕擷取畫面顯示將應用程式設定為只允許透過 HTTPS 流量的補救步驟。

    :::image type="content" source="./media/security-center-remediate-recommendations/security-center-remediate-recommendation.png" alt-text="建議的手動補救步驟" lightbox="./media/security-center-remediate-recommendations/security-center-remediate-recommendation.png":::

1. 完成之後，就會出現通知，告知您問題是否已解決。

## <a name="quick-fix-remediation"></a>快速修正補救

為了簡化補救並提高環境的安全性 (並提高您的安全分數) ，許多建議都包含一個快速修正選項。

快速修正可協助您在多個資源上快速補救建議。

> [!TIP]
> 快速修正解決方案只適用于特定的建議。 若要尋找具有可用快速修正的建議，請使用 **回應動作** 篩選以取得建議清單：
> 
> :::image type="content" source="media/security-center-remediate-recommendations/quick-fix-filter.png" alt-text="使用建議清單上方的篩選準則來尋找具有快速修正選項的建議":::

若要執行快速修正解決方案：

1. 從有 **快速修正** 的建議清單中！ 標籤，選取建議。

    [![選取 [快速修正]！](media/security-center-remediate-recommendations/security-center-quick-fix-select.png)](media/security-center-remediate-recommendations/security-center-quick-fix-select.png#lightbox)

1. 從 [ **狀況不良的資源** ] 索引標籤中，選取您要執行建議的資源，然後選取 [ **修復**]。

    > [!NOTE]
    > 某些列出的資源可能已停用，因為您沒有適當的許可權可以修改它們。

1. 在確認方塊中，閱讀補救詳細資料和含意。

    ![快速修正](./media/security-center-remediate-recommendations/security-center-quick-fix-view.png)

    > [!NOTE]
    > 在按一下 [**補救**] 之後開啟的 [**補救資源**] 視窗的灰色方塊中會列出這些含意。 它們會列出繼續進行快速修正補救時所發生的變更。

1. 如有必要，請插入相關的參數，並核准補救。

    > [!NOTE]
    > 補救完成後可能需要幾分鐘的時間，才能在 [ **狀況良好的資源** ] 索引標籤中查看資源。若要查看補救動作，請檢查 [活動記錄](#activity-log)。

1. 完成之後，會出現通知，通知您是否已成功補救。

## <a name="quick-fix-remediation-logging-in-the-activity-log"></a>在活動記錄中快速修正補救記錄 <a name="activity-log"></a>

補救作業會使用範本部署或 REST 修補程式 API 呼叫，將設定套用至資源。 這些作業會記錄在 [Azure 活動記錄](../azure-resource-manager/management/view-activity-logs.md)中。


## <a name="next-steps"></a>後續步驟

在本檔中，您已瞭解如何修復安全中心的建議。 若要深入了解資訊安全中心，請參閱下列頁面：

* [在 Azure 資訊安全中心中設定安全性原則](tutorial-security-policy.md) -瞭解如何為您的 Azure 訂用帳戶和資源群組設定安全性原則。
* [Azure 資訊安全中心的安全性健全狀況監視](security-center-monitoring.md) - 了解如何監視 Azure 資源的健全狀況。
