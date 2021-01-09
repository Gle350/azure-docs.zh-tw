---
title: PCI-DSS v3.2.1 藍圖範例概觀
description: 支付卡產業資料安全標準 v3.2.1 藍圖範例的概觀。 此藍圖範例可協助客戶評定特定控制項。
ms.date: 01/08/2021
ms.topic: sample
ms.openlocfilehash: 409be63f367c1077b3f35ec62c86a36d26aaaab1
ms.sourcegitcommit: 8dd8d2caeb38236f79fe5bfc6909cb1a8b609f4a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98044997"
---
# <a name="overview-of-the-pci-dss-v321-blueprint-sample"></a>PCI-DSS v3.2.1 藍圖範例概觀

PCI-DSS v3.2.1 藍圖範例是一組原則，旨在協助達成 PCI-DSS v3.2.1 合規性。 此藍圖協助客戶透過 PCI-DSS 工作負載治理雲端式環境。
PCI-DSS 藍圖會針對需要此資格認證的任何已部署 Azure 的架，部署一組核心原則。

## <a name="control-mapping"></a>控制項對應

控制項對應區域可針對此方案中包含的原則提供詳細資料，並說明這些原則如何滿足由 PCI-DSS v3.2.1 定義的各種控制項。 將資源指派給架構時，Azure 原則會評估不符合已指派原則的資源。

指派此藍圖後，請在 Azure 原則合規性儀表板中檢閱您的 Azure 環境合規性層級。

## <a name="next-steps"></a>後續步驟

您已檢閱了 PCI-DSS v3.2.1 藍圖範例概觀。 接下來，請瀏覽下列文章，以深入了解控制項對應及部署此範例的方法：

> [!div class="nextstepaction"]
> [PCI-DSS v3.2.1 藍圖 - 控制項對應](./control-mapping.md)
> [PCI-DSS v3.2.1 藍圖 - 部署步驟](./deploy.md)

有關藍圖及其使用方式的其他文件：

- 了解[藍圖生命週期](../../concepts/lifecycle.md)。
- 了解如何使用[靜態與動態參數](../../concepts/parameters.md)。
- 了解如何自訂[藍圖排序順序](../../concepts/sequencing-order.md)。
- 了解如何使用[藍圖資源鎖定](../../concepts/resource-locking.md)。
- 了解如何[更新現有的指派](../../how-to/update-existing-assignments.md)。