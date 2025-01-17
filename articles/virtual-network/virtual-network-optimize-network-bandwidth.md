---
title: 最佳化 VM 網路輸送量 | Microsoft Docs
description: 將 Microsoft Azure Windows 和 Linux Vm 的網路輸送量優化，包括 Ubuntu、CentOS 和 Red Hat 等主要發行版本。
services: virtual-network
documentationcenter: na
author: steveesp
manager: Gerald DeGrace
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/06/2020
ms.author: steveesp
ms.openlocfilehash: a9db2bcc0b44dfb6146517de8a139f34cd8584af
ms.sourcegitcommit: ad677fdb81f1a2a83ce72fa4f8a3a871f712599f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/17/2020
ms.locfileid: "97654450"
---
# <a name="optimize-network-throughput-for-azure-virtual-machines"></a>最佳化 Azure 虛擬機器的網路輸送量

Azure 虛擬機器 (VM) 有預設網路設定，可進一步針對網路輸送量進行最佳化。 此文章說明如何最佳化 Microsoft Azure Windows 和 Linux VM (包括如 Ubuntu、CentOS 和 Red Hat 等主要發行版本) 的網路輸送量。

## <a name="windows-vm"></a>Windows VM

如果您的 Windows VM 支援[加速網路](create-vm-accelerated-networking-powershell.md)，啟用該功能將會是針對輸送量的最佳設定。 針對所有其他的 Windows VM，相較於不使用接收端調整 (RSS) 的 VM，使用 RSS 的 VM 可達到更高的最大輸送量。 根據預設，Windows VM 中可能會停用 RSS。 若要判斷是否已啟用 RSS，並在停用的情況下將它啟用，請完成下列步驟：

1. 使用 `Get-NetAdapterRss` PowerShell 命令來查看是否已針對網路介面卡啟用 RSS。 在從 `Get-NetAdapterRss` 傳回的下列範例輸出中，RSS 並未啟用。

    ```powershell
    Name                    : Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled                 : False
    ```
2. 若要啟用 RSS，請輸入下列命令：

    ```powershell
    Get-NetAdapter | % {Enable-NetAdapterRss -Name $_.Name}
    ```
    上述命令沒有輸出。 此命令已變更 NIC 設定，導致約一分鐘的暫時性連線中斷。 連線中斷時隨即出現 [正在重新連線] 對話方塊。 第三次嘗試後，連線通常就會恢復。
3. 再次輸入 `Get-NetAdapterRss` 命令以確認 VM 中已啟用 RSS。 如果成功，則會傳回下列範例輸出：

    ```powershell
    Name                    : Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled                  : True
    ```

## <a name="linux-vm"></a>Linux VM

根據預設，Azure Linux VM 中一律會啟用 RSS。 2017 年 10 月之後發行的 Linux 核心包含新的網路最佳化選項，它們可讓 Linux VM 達到更高的網路輸送量。

### <a name="ubuntu-for-new-deployments"></a>新部署的 Ubuntu

Ubuntu Azure 核心最適合用於 Azure 上的網路效能。 若要取得最新的優化，請先安裝 18.04-LTS 的最新支援版本，如下所示：

```json
"Publisher": "Canonical",
"Offer": "UbuntuServer",
"Sku": "18.04-LTS",
"Version": "latest"
```

建立完成之後，請輸入下列命令以取得最新的更新。 這些步驟也適於目前執行 Ubuntu Azure 核心的 VM。

```bash
#run as root or preface with sudo
apt-get -y update
apt-get -y upgrade
apt-get -y dist-upgrade
```

下列可選命令集對於已具備 Azure 核心但因為發生錯誤而無法進行進一步更新的現有 Ubuntu 部署會十分有幫助。

```bash
#optional steps may be helpful in existing deployments with the Azure kernel
#run as root or preface with sudo
apt-get -f install
apt-get --fix-missing install
apt-get clean
apt-get -y update
apt-get -y upgrade
apt-get -y dist-upgrade
```

#### <a name="ubuntu-azure-kernel-upgrade-for-existing-vms"></a>現有 VM 的 Ubuntu Azure 核心升級

藉由升級至 Azure Linux 核心，可獲得顯著的輸送量效能。 若要確認您是否擁有此核心，請檢查您的核心版本。 它應該與範例相同或晚。

```bash
#Azure kernel name ends with "-azure"
uname -r

#sample output on Azure kernel:
#4.13.0-1007-azure
```

若您的 VM 沒有 Azure 核心，其版本號碼通常會以 "4.4" 為開頭。 如果 VM 沒有 Azure 核心，請以根權限執行下列命令：

```bash
#run as root or preface with sudo
apt-get update
apt-get upgrade -y
apt-get dist-upgrade -y
apt-get install "linux-azure"
reboot
```

### <a name="centos"></a>CentOS

為了要使用最新的最佳化項目，最好是指定下列參數，以最新支援的版本建立 VM：

```json
"Publisher": "OpenLogic",
"Offer": "CentOS",
"Sku": "7.7",
"Version": "latest"
```

安裝最新的 Lunix 整合服務 (LIS) 可為全新及現有的 VM 帶來好處。 輸送量最佳化選項從 LIS 4.2.2-2 版開始提供，雖然較新版本包含進一步的改善。 輸入下列命令以安裝最新的 LIS：

```bash
sudo yum update
sudo reboot
sudo yum install microsoft-hyper-v
```

### <a name="red-hat"></a>Red Hat

為了要使用最佳化項目，最好是指定下列參數，以最新支援的版本建立 VM：

```json
"Publisher": "RedHat"
"Offer": "RHEL"
"Sku": "7-RAW"
"Version": "latest"
```

安裝最新的 Lunix 整合服務 (LIS) 可為全新及現有的 VM 帶來好處。 輸送量最佳化選項從 LIS 4.2 版開始提供。 輸入下列命令以下載並安裝 LIS：

```bash
wget https://aka.ms/lis
tar xvf lis
cd LISISO
sudo ./install.sh #or upgrade.sh if prior LIS was previously installed
```

若要深入了解 Linux Integration Services for Hyper-V 4.2 版，請檢視[下載頁面](https://www.microsoft.com/download/details.aspx?id=55106)。

## <a name="next-steps"></a>後續步驟
* 以[接近放置群組](../virtual-machines/windows/co-location.md)的低延遲部署彼此接近的 vm
* 針對您的案例查看[測試 Azure VM 的頻寬/輸送量](virtual-network-bandwidth-testing.md)以取得最佳化的結果。
* 瞭解如何 [將頻寬配置給虛擬機器](virtual-machine-network-throughput.md)
* 深入了解 [Azure 虛擬網路常見問題集 (FAQ)](virtual-networks-faq.md)
