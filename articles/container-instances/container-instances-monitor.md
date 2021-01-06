---
title: 監視容器實例
description: 如何在 Azure 容器執行個體中依照您的容器，監視計算資源 (例如 CPU 和記憶體) 耗用量。
ms.topic: article
ms.date: 12/17/2020
ms.openlocfilehash: 83a8a5ab2c8c49f4044564c2d899685914103b0b
ms.sourcegitcommit: 67b44a02af0c8d615b35ec5e57a29d21419d7668
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97916065"
---
# <a name="monitor-container-resources-in-azure-container-instances"></a>在 Azure 容器執行個體中監視容器資源

[Azure 監視器][azure-monitoring]可提供您的容器執行個體所用計算資源的見解。 此資源使用量資料可協助您判斷容器群組的最佳資源設定。 Azure 監視器也會提供計量，以便追蹤容器執行個體的網路活動。

本文件詳述如何使用 Azure 入口網站和 Azure CLI，針對容器執行個體蒐集 Azure 監視器計量。

> [!IMPORTANT]
> Azure 容器執行個體中的 Azure 監視器計量目前為預覽狀態，而且[適用一些限制](#preview-limitations)。 若您同意[補充的使用規定][terms-of-use]即可取得預覽。 在公開上市 (GA) 之前，此功能的某些領域可能會變更。

## <a name="preview-limitations"></a>預覽限制

在這個階段， Azure 監視器計量只適用於 Linux 容器。

## <a name="available-metrics"></a>可用的計量

Azure 監視器為 [Azure 容器執行個體提供下列計量][supported-metrics]。 這些計量均適用於容器群組和個別容器。 根據預設，計量會匯總為平均值。

- **CPU 使用率** 以 **millicore** 測量。 
  - 其中一個 millicore 是 CPU 核心的 1/1000，因此 500 millicore 代表 0.5 CPU 核心的使用量。
- **記憶體使用量** （以位元組為單位）
- 每秒 **接收的網路位元組數**
- 每秒 **傳輸的網路位元組數** 

## <a name="get-metrics---azure-portal"></a>取得計量 - Azure 入口網站

建立容器群組後，即可在 Azure 入口網站中使用 Azure 監視資料。 若要查看容器群組的計量，請前往容器群組的 [概觀] 頁面。 您可以在這裡查看為每個可用計量預先建立的圖表。

![雙重圖表][dual-chart]

在包含多個容器的容器群組中，使用 [維度][monitor-dimension] 來依容器顯示度量。 若要使用個別容器計量來建立圖表，請執行下列步驟：

1. 在 [概觀] 頁面中，選取其中一個計量圖表，例如 **CPU**。 
1. 選取 [套用設定] 按鈕，然後選取 [容器名稱]。

![螢幕擷取畫面會顯示已選取 [套用分割] 和 [容器名稱] 的容器實例的計量。][dimension]

## <a name="get-metrics---azure-cli"></a>取得計量 - Azure CLI

使用 Azure CLI 也可以蒐集容器執行個體的計量。 首先，使用下列命令來取得容器群組的識別碼。 以您的資源群組名稱取代 `<resource-group>`，並以您的容器群組名稱取代 `<container-group>`。


```console
CONTAINER_GROUP=$(az container show --resource-group <resource-group> --name <container-group> --query id --output tsv)
```

使用下列命令來取得 **CPU** 使用量計量。

```azurecli
az monitor metrics list --resource $CONTAINER_GROUP --metric CPUUsage --output table
```

```output
Timestamp            Name       Average
-------------------  ---------  ---------
2020-12-17 23:34:00  CPU Usage
. . .
2020-12-18 00:25:00  CPU Usage
2020-12-18 00:26:00  CPU Usage  0.4
2020-12-18 00:27:00  CPU Usage  0.0
```

變更命令中的 `--metric` 參數值，以取得其他[支援的計量][supported-metrics]。 例如，使用下列命令來取得 **記憶體** 使用量計量。 

```azurecli
az monitor metrics list --resource $CONTAINER_GROUP --metric MemoryUsage --output table
```

```output
Timestamp            Name          Average
-------------------  ------------  ----------
2019-04-23 22:59:00  Memory Usage
2019-04-23 23:00:00  Memory Usage
2019-04-23 23:01:00  Memory Usage  0.0
2019-04-23 23:02:00  Memory Usage  8859648.0
2019-04-23 23:03:00  Memory Usage  9181184.0
2019-04-23 23:04:00  Memory Usage  9580544.0
2019-04-23 23:05:00  Memory Usage  10280960.0
2019-04-23 23:06:00  Memory Usage  7815168.0
2019-04-23 23:07:00  Memory Usage  7739392.0
2019-04-23 23:08:00  Memory Usage  8212480.0
2019-04-23 23:09:00  Memory Usage  8159232.0
2019-04-23 23:10:00  Memory Usage  8093696.0
```

對於多容器群組，可以新增 `containerName` 維度以傳回每個容器的計量。

```azurecli
az monitor metrics list --resource $CONTAINER_GROUP --metric MemoryUsage --dimension containerName --output table
```

```output
Timestamp            Name          Containername             Average
-------------------  ------------  --------------------  -----------
2019-04-23 22:59:00  Memory Usage  aci-tutorial-app
2019-04-23 23:00:00  Memory Usage  aci-tutorial-app
2019-04-23 23:01:00  Memory Usage  aci-tutorial-app      0.0
2019-04-23 23:02:00  Memory Usage  aci-tutorial-app      16834560.0
2019-04-23 23:03:00  Memory Usage  aci-tutorial-app      17534976.0
2019-04-23 23:04:00  Memory Usage  aci-tutorial-app      18329600.0
2019-04-23 23:05:00  Memory Usage  aci-tutorial-app      19742720.0
2019-04-23 23:06:00  Memory Usage  aci-tutorial-app      14786560.0
2019-04-23 23:07:00  Memory Usage  aci-tutorial-app      14651392.0
2019-04-23 23:08:00  Memory Usage  aci-tutorial-app      15470592.0
2019-04-23 23:09:00  Memory Usage  aci-tutorial-app      15450112.0
2019-04-23 23:10:00  Memory Usage  aci-tutorial-app      15339520.0
2019-04-23 22:59:00  Memory Usage  aci-tutorial-sidecar
2019-04-23 23:00:00  Memory Usage  aci-tutorial-sidecar
2019-04-23 23:01:00  Memory Usage  aci-tutorial-sidecar  0.0
2019-04-23 23:02:00  Memory Usage  aci-tutorial-sidecar  884736.0
2019-04-23 23:03:00  Memory Usage  aci-tutorial-sidecar  827392.0
2019-04-23 23:04:00  Memory Usage  aci-tutorial-sidecar  831488.0
2019-04-23 23:05:00  Memory Usage  aci-tutorial-sidecar  819200.0
2019-04-23 23:06:00  Memory Usage  aci-tutorial-sidecar  843776.0
2019-04-23 23:07:00  Memory Usage  aci-tutorial-sidecar  827392.0
2019-04-23 23:08:00  Memory Usage  aci-tutorial-sidecar  954368.0
2019-04-23 23:09:00  Memory Usage  aci-tutorial-sidecar  868352.0
2019-04-23 23:10:00  Memory Usage  aci-tutorial-sidecar  847872.0
```

## <a name="next-steps"></a>後續步驟

在 [Azure 監視概觀][azure-monitoring]深入了解 Azure 監視。

了解如何建立[計量警示][metric-alert]，以在 Azure 容器執行個體的計量超出閾值時收到通知。

<!-- IMAGES -->
[cpu-chart]: ./media/container-instances-monitor/cpu-multi.png
[dimension]: ./media/container-instances-monitor/dimension.png
[dual-chart]: ./media/container-instances-monitor/metrics.png
[memory-chart]: ./media/container-instances-monitor/memory-multi.png

<!-- LINKS - External -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[azure-monitoring]: ../azure-monitor/overview.md
[metric-alert]: ..//azure-monitor/platform/alerts-metric.md
[monitor-dimension]: ../azure-monitor/platform/data-platform-metrics.md#multi-dimensional-metrics
[supported-metrics]: ../azure-monitor/platform/metrics-supported.md#microsoftcontainerinstancecontainergroups
