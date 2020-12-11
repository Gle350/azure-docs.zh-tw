---
title: 包含檔案
description: 包含檔案
services: virtual-machines-windows
author: cynthn
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 02/11/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: f99aedc21c3b51975649f8944ab53536d365a7d1
ms.sourcegitcommit: 3ea45bbda81be0a869274353e7f6a99e4b83afe2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97096279"
---
## <a name="supported-operating-systems-and-drivers"></a>支援的作業系統和驅動程式

### <a name="nvidia-tesla-cuda-drivers"></a>NVIDIA Tesla (CUDA) 驅動程式

適用于 NC、NCv2、NCv3、NCasT4_v3、ND 和 NDv2 系列 Vm 的 NVIDIA Tesla (CUDA) 驅動程式 (只有下表所列的作業系統才支援 NV 系列) 的選用。 以下是本文章發行時的最新驅動程式下載連結。 如需最新的驅動程式，請瀏覽 [NVIDIA](https://www.nvidia.com/) 網站。

> [!TIP]
> 在 Windows Server VM 上手動安裝 CUDA 驅動程式的替代方案，就是部署 Azure [資料科學虛擬機器](../articles/machine-learning/data-science-virtual-machine/overview.md)映像。 適用於 Windows Server 2016 的 DSVM 版本會預先安裝 NVIDIA CUDA 驅動程式、CUDA 深度類神經網路程式庫和其他工具。


| OS | 驅動程式 |
| -------- |------------- |
| Windows Server 2019 | [451.82](http://us.download.nvidia.com/tesla/451.82/451.82-tesla-desktop-winserver-2019-2016-international.exe) ( .exe)  |
| Windows Server 2016 | [451.82](http://us.download.nvidia.com/tesla/451.82/451.82-tesla-desktop-winserver-2019-2016-international.exe) ( .exe)  |

### <a name="nvidia-grid-drivers"></a>NVIDIA GRID 驅動程式

Microsoft 會針對用來作為虛擬工作站或虛擬應用程式的 NV 和 NVv3 系列 Vm，轉散發 NVIDIA GRID 驅動程式安裝程式。 僅在下表所列的作業系統上安裝 Azure NV 系列 Vm 上的這些方格驅動程式。 這些驅動程式包含在 Azure 的 GRID 虛擬 GPU 軟體的授權中。 您不需要設定 NVIDIA vGPU software 授權伺服器。

Azure 所轉散發的方格驅動程式無法在非 NV 系列 Vm （例如 NCv2、NCv3、ND 和 NDv2 系列 Vm）上運作。 其中一個例外狀況是 NCas_T4_V3 VM 系列，格線驅動程式會在其中啟用類似于 NV 系列的圖形功能。

具有 Nvidia K80 Gpu 的 NC-Series 不支援方格/圖形應用程式。  

請注意，Nvidia 延伸模組一律會安裝最新的驅動程式。 針對相依于較舊版本的客戶，我們提供了先前版本的連結。

若為 Windows Server 2019、Windows Server 2016 和 Windows 10 () 建立2004：
- [方格 11.2 (452.57) ](https://go.microsoft.com/fwlink/?linkid=874181) ( .exe) 
- [方格 11.1 (452.39) ](https://download.microsoft.com/download/9/9/1/99186e1b-d27d-47d5-9957-175c88f4efbe/452.39_grid_win10_64bit_whql.exe) ( .exe)  

若為 Windows Server 2012 R2： 
- [方格 11.0 (451.48) ](https://download.microsoft.com/download/f/7/2/f729e28b-57b8-4141-b577-38d2390973ef/451.48_grid_server2012R2_64bit_international.exe) ( .exe)  
- [方格 10.1 (442.66) ](https://download.microsoft.com/download/4/3/3/4330fd5c-c685-4ca1-abca-3b2fb3c11d2e/442.06_grid_win8_win7_64bit_international_whql.exe) ( .exe)   

如需所有先前的 Nvidia GRID 驅動程式連結的完整清單，請造訪 [GitHub](https://github.com/Azure/azhpc-extensions/blob/master/NvidiaGPU/resources.json)
