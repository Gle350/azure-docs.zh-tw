---
title: 包含檔案
description: 包含檔案
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 12/11/2020
ms.openlocfilehash: 09dd6e9a9d69797c2c33270d1620e861a052efe2
ms.sourcegitcommit: 2ba6303e1ac24287762caea9cd1603848331dd7a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/15/2020
ms.locfileid: "97505115"
---
用來裝載模型的計算目標會影響已部署端點的成本和可用性。 使用此資料表選擇適當的計算目標。

| 計算目標 | 用於 | GPU 支援 | FPGA 支援 | 描述 |
| ----- | ----- | ----- | ----- | ----- |
| [本機&nbsp;Web&nbsp;服務](../articles/machine-learning/how-to-deploy-local-container-notebook-vm.md) | 測試/偵錯 | &nbsp; | &nbsp; | 用於有限的測試和疑難排解。 硬體加速取決於本機系統中的程式庫使用情況。
| [Azure Kubernetes Service (AKS)](../articles/machine-learning/how-to-deploy-azure-kubernetes-service.md) | 即時推斷 |  [是](../articles/machine-learning/how-to-deploy-inferencing-gpus.md) (Web 服務部署) | [是](../articles/machine-learning/how-to-deploy-fpga-web-service.md)   |用於大規模生產環境部署。 提供已部署服務的快速回應時間和自動調整。 Azure Machine Learning SDK 不支援叢集自動調整。 若要變更 AKS 叢集中的節點，請在 Azure 入口網站中使用 AKS 叢集的 UI。 <br/><br/> 在設計工具中支援。 |
| [Azure 容器執行個體](../articles/machine-learning/how-to-deploy-azure-container-instance.md) | 測試或開發 | &nbsp;  | &nbsp; | 用於需要少於 48 GB RAM 的低規模 CPU 型工作負載。 <br/><br/> 在設計工具中支援。 |
| [Azure Machine Learning 計算叢集](../articles/machine-learning/tutorial-pipeline-batch-scoring-classification.md) | 批次&nbsp;推斷 | [是](../articles/machine-learning/tutorial-pipeline-batch-scoring-classification.md) (機器學習管線) | &nbsp;  | 在無伺服器計算上執行批次評分。 支援一般和低優先順序的 VM。 不支援即時推斷。|

> [!NOTE]
> 雖然計算目標 (例如本機、Azure Machine Learning 計算和 Azure Machine Learning 計算叢集) 支援用於訓練和實驗的 GPU，但僅在 AKS 上支援使用 GPU 進行推斷 _當部署為 Web 服務時_。
>
> 僅在 Azure Machine Learning Compute 中支援使用 GPU 進行推斷 _當使用機器學習管線進行評分時_。
> 
> 選擇叢集 SKU 時，請先進行擴大再擴增。從擁有模型所需 RAM 的 150% 的電腦開始，分析結果並找出具有所需效能的電腦。 了解這點之後，請增加電腦數目，以符合您的同時推斷需求。

> [!NOTE]
> * 容器執行個體僅適用於小於 1GB 的小型模型。
> * 針對大型模型的開發/測試使用單一節點 AKS 叢集。