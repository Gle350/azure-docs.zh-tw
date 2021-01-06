---
title: 使用延伸模組的部署後設定
description: 瞭解如何使用 Azure Resource Manager 範本 (ARM 範本) 延伸模組，以提供部署後設定。
author: mumian
ms.topic: conceptual
ms.date: 12/14/2018
ms.author: jgao
ms.openlocfilehash: 23d5a9aaa546542218a6f839ab415184ff309928
ms.sourcegitcommit: 2aa52d30e7b733616d6d92633436e499fbe8b069
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97934317"
---
# <a name="provide-post-deployment-configurations-by-using-extensions"></a>使用擴充功能提供部署後的設定

Azure Resource Manager 範本 (ARM 範本) 擴充功能是小型的應用程式，可在 Azure 資源上提供部署後設定和自動化工作。 最常見的是虛擬機器擴充功能。 請參閱[虛擬機器擴充功能和功能 (Windows)](../../virtual-machines/extensions/features-windows.md) 與[虛擬機器擴充功能和功能 (Linux)](../../virtual-machines/extensions/features-linux.md)。

## <a name="extensions"></a>延伸模組

現有的擴充功能有：

- [Microsoft. Compute/virtualMachines/extensions](/azure/templates/microsoft.compute/2018-10-01/virtualmachines/extensions)
- [Microsoft.Compute virtualMachineScaleSets/extensions](/azure/templates/microsoft.compute/2018-10-01/virtualmachinescalesets/extensions) \(英文\)
- [Microsoft.HDInsight clusters/extensions](/azure/templates/microsoft.hdinsight/2018-06-01-preview/clusters) \(英文\)
- [Microsoft.Sql servers/databases/extensions](/azure/templates/microsoft.sql/2014-04-01/servers/databases/extensions) \(英文\)
- [Microsoft.Web/sites/siteextensions](/azure/templates/microsoft.web/2016-08-01/sites/siteextensions) \(英文\)

若要了解可用的擴充功能，請瀏覽至[範本參考](/azure/templates/) \(英文\)。 在 [依標題篩選] 中，輸入 **擴充功能**。

若要了解如何使用這些擴充功能，請參閱：

- [教學課程：使用 ARM 範本部署虛擬機器擴充](template-tutorial-deploy-vm-extensions.md)功能。
- [教學課程：使用 ARM 範本匯入 SQL BACPAC 檔案](template-tutorial-deploy-sql-extensions-bacpac.md)

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [教學課程：使用 ARM 範本部署虛擬機器擴充功能](template-tutorial-deploy-vm-extensions.md)
