---
title: 系統需求
description: 列出 Azure 遠端轉譯的系統需求
author: florianborn71
ms.author: flborn
ms.date: 02/03/2020
ms.topic: article
ms.custom: references_regions
ms.openlocfilehash: 4380f14610fb0775c82aa79ec7cda9dc70cf0715
ms.sourcegitcommit: a4533b9d3d4cd6bb6faf92dd91c2c3e1f98ab86a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/22/2020
ms.locfileid: "97722686"
---
# <a name="system-requirements"></a>系統需求

> [!IMPORTANT]
> **Azure 遠端轉譯** 目前處於公開預覽狀態。
> 此預覽版本是在沒有服務等級協定的情況下提供，不建議用於生產工作負載。 可能不支援特定功能，或可能已經限制功能。 如需詳細資訊，請參閱 [Microsoft Azure 預覽版增補使用條款](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)。

本章列出使用 *Azure 遠端轉譯* (ARR) 的最低系統需求。

## <a name="development-pc"></a>開發電腦

* Windows 10 版本1903或更高版本。
* 最新的圖形驅動程式。
* 選擇性：如果您想要使用遠端轉譯內容的本機預覽版 (例如 Unity) ，請 [H265 硬體視頻解碼器](https://www.microsoft.com/p/hevc-video-extensions/9nmzlz57r3t7)。

> [!IMPORTANT]
> Windows update 不一定會提供最新的 GPU 驅動程式，請查看您 GPU 製造商的網站，以取得最新的驅動程式：
>
> * [AMD 驅動程式](https://www.amd.com/en/support)
> * [Intel 驅動程式](https://www.intel.com/content/www/us/en/support/detect.html)
> * [NVIDIA 驅動程式](https://www.nvidia.com/Download/index.aspx)

下表列出支援 H265 硬體影片解碼的 Gpu。

| GPU 製造商 | 支援的模型 |
|-----------|:-----------|
| NVIDIA | 檢查 [此頁面底部](https://developer.nvidia.com/video-encode-decode-gpu-support-matrix)的 **NVDEC 支援矩陣**。 您的 GPU 在 **H. 265 4:2:0 8-bit** 資料行中需要是 YES。 |
| AMD | 具有至少第6版 AMD [整合視頻解碼器](https://en.wikipedia.org/wiki/Unified_Video_Decoder#UVD_6)的 gpu。 |
| Intel | Skylake 和較新的 Cpu |

雖然可能會安裝正確的 H265 編解碼器，但編解碼器 Dll 上的安全性屬性可能會造成編解碼器初始化失敗。 [疑難排解指南](../resources/troubleshoot.md#h265-codec-not-available)會說明如何解決此問題的步驟。 在桌面應用程式（例如 Unity）中使用服務時，才會發生 DLL 問題。

## <a name="devices"></a>裝置

Azure 遠端轉譯目前僅支援 **HoloLens 2** 和 Windows 桌面做為目標裝置。 請參閱 [平臺限制](../reference/limits.md#platform-limitations) 一節。

使用最新的 HEVC 編解碼器是很重要的，因為較新的版本在延遲方面有顯著的改善。 若要檢查您的裝置上已安裝的版本：

1. 啟動 **Microsoft Store**。
1. 按一下右上方的 [ **...]** 按鈕。
1. 選取 [ **下載和更新**]。
1. 在清單中搜尋 **來自裝置製造商的 HEVC 影片擴充** 功能。 如果這個專案未列在 [更新] 下，則會安裝最新的版本。
1. 請確定列出的編解碼器至少有版本 **1.0.21821.0**。
1. 按一下 [ **取得更新** ] 按鈕，並等候安裝完成。

## <a name="network"></a>網路

穩定、低延遲的網路連接對於良好的使用者體驗很重要。

如需 [網路需求](../reference/network-requirements.md)，請參閱專屬章節。

若要針對網路問題進行疑難排解，請參閱 [疑難排解指南](../resources/troubleshoot.md#unstable-holograms)。

### <a name="network-firewall"></a>網路防火牆

### <a name="sdk-version--0176"></a>SDK 版本 >= 0.1.76

遠端轉譯虛擬機器會使用來自下列 IP 範圍的共用 IP 位址：

| 名稱             | 區域         | IP 首碼         |
|------------------|:---------------|:------------------|
| 澳大利亞東部   | australiaeast  | 20.53.44.240/28   |
| 美國東部          | eastus         | 20.62.129.224/28  |
| 美國東部 2        | eastus2        | 20.49.103.240/28  |
| 日本東部       | japaneast      | 20.191.165.112/28 |
| 北歐     | northeurope    | 52.146.133.64/28  |
| 美國中南部 | southcentralus | 20.65.132.80/28   |
| 東南亞   | southeastasia  | 20.195.64.224/28  |
| 英國南部         | uksouth        | 51.143.209.144/28 |
| 西歐      | westeurope     | 20.61.99.112/28   |
| 美國西部 2        | westus2        | 20.51.9.64/28     |

請確定您的防火牆 (在裝置上、在路由器內等等，) 不會封鎖這些 IP 範圍和下列埠範圍：

| 連接埠              | 通訊協定  | 允許    |
|-------------------|---------- |----------|
| 49152-65534       | TCP/UDP | 傳出 |

#### <a name="sdk-version--0176"></a>SDK 版本 < 0.1.76

請確定您的防火牆 (在裝置上、路由器內部等等) 不會封鎖下列連接埠：

| 連接埠              | 通訊協定 | 允許    | 描述 |
|-------------------|----------|----------|-------------|
| 50051             | TCP      | 傳出 |  (HTTP 信號交換的初始連接)  |
| 8266              | UDP      | 傳出 | 資料傳輸 |
| 5000、5433、8443  | TCP      | 傳出 | [ArrInspector 工具](../resources/tools/arr-inspector.md)的必要參數|


## <a name="software"></a>軟體

必須安裝下列軟體：

* **Visual Studio 2019** [ (下載](https://visualstudio.microsoft.com/vs/older-downloads/)的最新版本) 
* [適用於混合實境的 Visual Studio 工具](/windows/mixed-reality/install-the-tools)。 具體而言，必須要安裝下列「工作負載」：
  * **使用 C++ 開發桌面**
  * **通用 Windows 平台 (UWP) 開發**
* **Windows SDK 10.0.18362.0** [ (下載)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* **GIT** [ (下載)](https://git-scm.com/downloads)
* 選擇性：若要從桌上型電腦上的伺服器查看影片串流，您需要 HEVC 的 **影片延伸** 模組 [ (Microsoft Store 連結)](https://www.microsoft.com/p/hevc-video-extensions/9nmzlz57r3t7)。 檢查存放區中的更新，確定已安裝最新版本。

## <a name="unity"></a>Unity

若要使用 Unity 進行開發，請安裝

* Unity 2019.3.1 [(下載)](https://unity3d.com/get-unity/download)
* 在 Unity 中安裝下列模組：
  * **UWP** - 通用 Windows 平台建置支援
  * **IL2CPP** - Windows 建置支援 (IL2CPP)

## <a name="next-steps"></a>後續步驟

* [快速入門：使用 Unity 轉譯模型](../quickstarts/render-model.md)