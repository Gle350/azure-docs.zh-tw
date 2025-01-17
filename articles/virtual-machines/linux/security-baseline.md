---
title: 適用于 Linux 虛擬機器的 Azure 安全性基準
description: Linux 虛擬機器安全性基準提供的程式指引和資源，可讓您執行 Azure 安全性基準測試中所指定的安全性建議。
author: msmbaldwin
ms.service: virtual-machines-linux
ms.topic: conceptual
ms.date: 07/13/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: d8893daaf73a15cdc0baf8eeb339e794f6f1da64
ms.sourcegitcommit: 67b44a02af0c8d615b35ec5e57a29d21419d7668
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97913442"
---
# <a name="azure-security-baseline-for-linux-virtual-machines"></a>適用于 Linux 虛擬機器的 Azure 安全性基準

適用于 Linux 虛擬機器的 Azure 安全性基準包含可協助您改善部署安全性狀態的建議。

此服務的基準取自 [Azure 安全性效能評定 1.0 版](../../security/benchmarks/overview.md)，其會提供如何在 Azure 上使用最佳做法指引來保護雲端解決方案的建議。

如需詳細資訊，請參閱 [Azure 安全性基準概觀](../../security/benchmarks/security-baselines-overview.md) (機器翻譯)。

## <a name="network-security"></a>網路安全性

*如需詳細資訊，請參閱 [安全性控制：網路安全性](../../security/benchmarks/security-control-network-security.md)。*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1：保護虛擬網路內的 Azure 資源

**指導** 方針：當您 (VM) 建立 Azure 虛擬機器時，您必須建立虛擬網路 (VNet) 或使用現有的 vnet，並使用子網來設定 VM。 確定所有已部署的子網都已套用網路安全性群組，且該網路存取控制適用于您應用程式的受信任埠和來源。

或者，如果您有適用于集中式防火牆的特定使用案例，也可以使用 Azure 防火牆來滿足這些需求。

* [Azure 中的虛擬網路和虛擬機器](../network-overview.md)

* [如何建立虛擬網路](../../virtual-network/quick-create-portal.md)

* [如何建立具有安全性設定的 NSG](../../virtual-network/tutorial-filter-network-traffic.md)

* [如何部署和設定 Azure 防火牆](../../firewall/tutorial-firewall-deploy-portal.md)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-network-interfaces"></a>1.2：監視和記錄虛擬網路、子網和網路介面的設定和流量

**指導** 方針：使用 Azure 資訊安全中心來識別並遵循網路保護建議，以協助保護 Azure 虛擬機器 (azure 中的 VM) 資源。 啟用 NSG 流量記錄，並將記錄檔傳送至儲存體帳戶，以供 Vm 的流量審核以進行不尋常的活動。

* [如何啟用 NSG 流量記錄](../../network-watcher/network-watcher-nsg-flow-logging-portal.md)

* [瞭解 Azure 資訊安全中心所提供的網路安全性](../../security-center/security-center-network-recommendations.md)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="13-protect-critical-web-applications"></a>1.3：保護重要的 Web 應用程式

**指導** 方針：如果使用虛擬機器 (vm) 來裝載 web 應用程式，請在 vm 的子網上使用網路安全性群組 (NSG) ，以限制允許通訊的網路流量、埠和通訊協定。 將 Nsg 設定為只允許應用程式所需的流量時，請遵循最低許可權的網路方式。

您也可以在重要的 web 應用程式前面 (WAF) 部署 Azure Web 應用程式防火牆，以額外檢查連入流量。 啟用診斷設定以 WAF 記錄，並將其內嵌至儲存體帳戶、事件中樞或 Log Analytics 工作區。

* [使用 Azure 入口網站建立包含 Web 應用程式防火牆的應用程式閘道](../../web-application-firewall/ag/application-gateway-web-application-firewall-portal.md)

* [Azure 中的虛擬網路和虛擬機器](../network-overview.md)

* [網路安全性群組的資訊](../../virtual-network/tutorial-filter-network-traffic.md)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4：拒絕與已知惡意 IP 位址的通訊

**指導** 方針：啟用分散式阻絕服務 (Ddos) 虛擬網路上的標準保護，以防範 ddos 攻擊。 使用 Azure 資訊安全中心整合式威脅情報，您可以監視與已知惡意 IP 位址的通訊。 在每個虛擬網路區段上設定 Azure 防火牆，並啟用威脅情報並設定為惡意網路流量的「警示和拒絕」。

您可以使用 Azure 資訊安全中心的即時網路存取，將 Linux 虛擬機器的風險限制為有限期間內的已核准 IP 位址。 此外，您也可以使用 Azure 資訊安全中心調適型網路強化，根據實際的流量和威脅情報，建議可限制埠和來源 Ip 的 NSG 設定。

* [如何設定 DDoS 保護](../../ddos-protection/manage-ddos-protection.md)

* [如何部署 Azure 防火牆](../../firewall/tutorial-firewall-deploy-portal.md)

* [了解 Azure 資訊安全中心的整合式威脅情報](../../security-center/azure-defender.md)

* [瞭解 Azure 資訊安全中心適應性網路強化](../../security-center/security-center-adaptive-network-hardening.md)

* [瞭解 Azure 資訊安全中心及時的網路存取控制](../../security-center/security-center-just-in-time.md)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="15-record-network-packets"></a>1.5：記錄網路封包

**指導** 方針：您可以將 NSG 流量記錄記錄到儲存體帳戶，以產生 Azure 虛擬機器的流量記錄。 調查異常活動時，您可以啟用網路監看員封包捕獲，讓網路流量可以針對不尋常和非預期的活動進行審核。

* [如何啟用 NSG 流量記錄](../../network-watcher/network-watcher-nsg-flow-logging-portal.md)

* [如何啟用網路監看員](../../network-watcher/network-watcher-create.md)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6：部署以網路為基礎的入侵偵測/入侵防護系統 (IDS/IPS) 

**指導** 方針：結合網路監看員所提供的封包捕捉和開放原始碼的識別碼工具，您可以對各式各樣的威脅執行網路入侵偵測。 此外，您可以在適當的情況下，于虛擬網路區段上部署 Azure 防火牆，並啟用威脅情報並設定為惡意網路流量的「警示和拒絕」。

* [使用網路監看員和開放原始碼工具執行網路入侵偵測](../../network-watcher/network-watcher-intrusion-detection-open-source-tools.md)

* [如何部署 Azure 防火牆](../../firewall/tutorial-firewall-deploy-portal.md)

* [如何使用 Azure 防火牆設定警示](../../firewall/threat-intel.md)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="17-manage-traffic-to-web-applications"></a>1.7：管理 Web 應用程式的流量

**指導** 方針：您可以針對已啟用受信任憑證的 HTTPS/SSL，部署 web 應用程式的 Azure 應用程式閘道。 使用 Azure 應用程式閘道，您可以將接聽程式指派給埠、建立規則，並將資源新增至後端集區（例如 Linux 虛擬機器），以將應用程式 web 流量導向至特定資源。

* [如何部署應用程式閘道](../../application-gateway/quick-create-portal.md)

* [如何將應用程式閘道設定為使用 HTTPS](../../application-gateway/create-ssl-portal.md)

* [瞭解 Azure web 應用程式閘道的第7層負載平衡](../../application-gateway/overview.md)

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8：將網路安全性規則的複雜性和系統管理負荷降至最低

**指導** 方針：使用虛擬網路服務標籤來定義網路安全性群組的網路存取控制，或針對您的 azure 虛擬機器設定的 azure 防火牆。 建立安全性規則時，您可以使用服務標籤取代特定的 IP 位址。 在規則的適當來源或目的地欄位中指定服務標籤名稱 (例如 ApiManagement)，即可允許或拒絕對應服務的流量。 Microsoft 會管理服務標籤包含的位址前置詞，並隨著位址變更自動更新服務標籤。

* [瞭解和使用服務標記](../../virtual-network/service-tags-overview.md)

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9：維護網路裝置的標準安全性設定

**指導** 方針：使用 Azure 原則來定義和執行 Azure 虛擬機器 (VM) 的標準安全性設定。 您也可以使用 Azure 藍圖，藉由在單一藍圖定義中封裝關鍵環境成品（例如 Azure Resource Manager 範本、角色指派和 Azure 原則指派）來簡化大規模的 Azure VM 部署。 您可以將藍圖套用至訂用帳戶，並透過藍圖版本設定來啟用資源管理。

* [如何設定和管理 Azure 原則](../../governance/policy/tutorials/create-and-manage.md)

* [適用于網路的 Azure 原則範例](../../governance/policy/samples/built-in-policies.md#network)

* [如何建立 Azure 藍圖](../../governance/blueprints/create-blueprint-portal.md)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="110-document-traffic-configuration-rules"></a>1.10：文件流量設定規則

**指導** 方針：您可以使用 Nsg 的標記，以及針對 Azure 虛擬機器設定的網路安全性和流量流程相關的其他資源。 針對個別的 NSG 規則，請使用 [描述] 欄位來指定允許流量進出網路之任何規則的商務需求和/或持續時間。

* [如何建立和使用標籤](../../azure-resource-manager/management/tag-resources.md)

* [如何建立虛擬網路](../../virtual-network/quick-create-portal.md)

* [如何建立具有安全性設定的 NSG](../../virtual-network/tutorial-filter-network-traffic.md)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11：使用自動化工具來監視網路資源設定並偵測變更

**指導** 方針：使用 Azure 活動記錄監視與您虛擬機器相關之網路資源設定的變更。 在 Azure 監視器中建立警示，在發生重大網路設定或資源的變更時，將會觸發這些警示。

使用 Azure 原則來驗證 (和/或補救與 Linux 虛擬機器相關之網路資源的) 設定。

* [如何檢視及擷取 Azure 活動記錄事件](../../azure-monitor/platform/activity-log.md#view-the-activity-log)

* [如何在 Azure 監視器中建立警示](../../azure-monitor/platform/alerts-activity-log.md)

* [如何設定和管理 Azure 原則](../../governance/policy/tutorials/create-and-manage.md)

* [適用于網路的 Azure 原則範例](../../governance/policy/samples/built-in-policies.md#network)

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

## <a name="logging-and-monitoring"></a>記錄和監視

*如需詳細資訊，請參閱 [安全性控制：記錄和監視](../../security/benchmarks/security-control-logging-monitoring.md)。*

### <a name="21-use-approved-time-synchronization-sources"></a>2.1：使用已核准的時間同步處理來源

**指導** 方針： Microsoft 會維護 Azure 資源的時間來源，不過，您可以選擇管理 Linux 虛擬機器的時間同步處理設定。

* [如何設定 Azure 計算資源的時間同步處理](./time-sync.md)

**Azure 資訊安全中心監視**：無法使用

**責任**：共用

### <a name="22-configure-central-security-log-management"></a>2.2：設定中央安全性記錄管理

**指導** 方針： Azure 資訊安全中心提供 Linux 虛擬機器的安全性事件記錄檔監視。

* [Azure 資訊安全中心的資料收集](../../security-center/security-center-enable-data-collection.md)

* [若要捕獲 Syslog 資料進行監視，您必須啟用 Log Analytics 擴充功能](../../azure-monitor/learn/quick-collect-azurevm.md#enable-the-log-analytics-vm-extension)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3：啟用 Azure 資源的稽核記錄

**指導** 方針：活動記錄可以用來在虛擬機器資源上審核作業和執行的動作。 活動記錄包含 (PUT、POST、DELETE) 資源的所有寫入作業，但讀取作業 (取得) 。 當您進行疑難排解時，可以使用活動記錄來尋找錯誤，或是監視組織中的使用者修改資源的方式。

藉由在虛擬機器上部署診斷擴充功能， (VM) 來啟用來賓 OS 診斷資料的收集。 您可以使用診斷延伸模組，從 Azure 虛擬機器收集診斷資料，例如應用程式記錄檔或效能計數器。

若要讓您的虛擬機器支援的應用程式和服務具有更高的可見度，您可以同時啟用適用於 VM 的 Azure 監視器和 Application insights。 使用 Application Insights，您可以監視您的應用程式並捕捉遙測資料（例如 HTTP 要求、例外狀況等等），讓您可以將 Vm 與應用程式之間的問題相互關聯。

此外，請啟用 Azure 監視器，以存取您的 audit and 活動記錄，其中包括事件來源、日期、使用者、時間戳記、來源位址、目的地位址和其他有用的元素。

* [如何使用 Azure 監視器收集平臺記錄和計量](../../azure-monitor/platform/diagnostic-settings.md)

* [Log analytics 代理程式總覽](../../azure-monitor/platform/log-analytics-agent.md)

* [適用于 Linux 的 Log analytics 虛擬機器擴充功能](../extensions/oms-linux.md)

* [查看和取出 Azure 活動記錄事件](../../azure-monitor/platform/activity-log.md#view-the-activity-log)

* [Application Insights 概觀](../../azure-monitor/app/app-insights-overview.md)

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

### <a name="24-collect-security-logs-from-operating-systems"></a>2.4：從作業系統收集安全性記錄

**指導** 方針：使用 Azure 資訊安全中心為 Azure 虛擬機器提供安全性事件記錄檔監視。 由於安全性事件記錄檔所產生的資料量，因此預設不會儲存。

如果您的組織想要保留虛擬機器的安全性事件記錄檔資料，則可以將它儲存在 Log Analytics 工作區中，位於 Azure 資訊安全中心中設定的所需資料收集層。

* [Azure 資訊安全中心的資料收集](../../security-center/security-center-enable-data-collection.md)

* [若要捕獲 Syslog 資料進行監視，您必須啟用 Log Analytics 擴充功能](../../azure-monitor/learn/quick-collect-azurevm.md#enable-the-log-analytics-vm-extension)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="25-configure-security-log-storage-retention"></a>2.5：設定安全性記錄儲存體保留期

**指導** 方針：確定用於儲存虛擬機器記錄的任何儲存體帳戶或 log Analytics 工作區都已根據您組織的合規性法規設定記錄保留期限。

* [如何在 Azure 中監視虛擬機器](../../azure-monitor/insights/monitor-vm-azure.md)

* [如何設定 Log Analytics 工作區保留期限](../../azure-monitor/platform/manage-cost-storage.md)

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

### <a name="26-monitor-and-review-logs"></a>2.6：監視和審核記錄

**指導** 方針：啟用 log analytics 代理程式（也稱為 MICROSOFT MONITORING AGENT (MMA) 或 OMS Linux 代理程式），並將其設定為將記錄傳送至 Log Analytics 工作區。 Linux 代理程式會將收集的資料從不同的來源傳送至 Azure 監視器中的 Log Analytics 工作區，以及監視解決方案中定義的任何唯一記錄或計量。

分析和監視記錄中的異常行為，並定期查看結果。 使用 Azure 監視器來檢查記錄檔資料，並對其執行查詢。

或者，您也可以啟用和內部資料來 Azure Sentinel 或協力廠商 SIEM，以監視和檢查您的記錄。

* [Log analytics 代理程式總覽](../../azure-monitor/platform/log-analytics-agent.md)

* [適用于 Linux 的 Log analytics 虛擬機器擴充功能](../extensions/oms-linux.md)

* [如何使 Azure Sentinel 上線](../../sentinel/quickstart-onboard.md)

* [了解 Log Analytics 工作區](../../azure-monitor/log-query/log-analytics-tutorial.md)

* [如何在 Azure 監視器中執行自訂查詢](../../azure-monitor/log-query/get-started-queries.md)

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

### <a name="27-enable-alerts-for-anomalous-activities"></a>2.7：啟用異常活動的警示

**指導** 方針：使用 Azure 資訊安全中心設定 Log Analytics 工作區，以在 Azure 虛擬機器的安全性記錄和事件中找到的異常活動進行監視和警示。

或者，您也可以啟用和內部資料來 Azure Sentinel 或協力廠商 SIEM，以設定異常活動的警示。

* [如何使 Azure Sentinel 上線](../../sentinel/quickstart-onboard.md)

* [如何在 Azure 資訊安全中心中管理警示](../../security-center/security-center-managing-and-responding-alerts.md)

* [如何對 log analytics 記錄資料發出警示](../../azure-monitor/learn/tutorial-response.md)

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

### <a name="28-centralize-anti-malware-logging"></a>2.8：集中化反惡意程式碼記錄

**指導** 方針：在 Linux OS 內部，您將需要協力廠商工具來偵測反惡意程式碼弱點。

* [將 Linux 伺服器上架到 Azure 安全性中心的指示](../../security-center/quickstart-onboard-machines.md)

* [下列連結提供 Microsoft 建議的安全性指導方針，可作為所選弱點軟體的準則清單](../security-recommendations.md)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="29-enable-dns-query-logging"></a>2.9：啟用 DNS 查詢記錄

**指導** 方針：根據您組織的需求，針對 DNS 記錄解決方案的 Azure Marketplace 來執行協力廠商解決方案。

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="210-enable-command-line-audit-logging"></a>2.10：啟用命令列稽核記錄

**指導** 方針：您可以針對每個節點手動設定主控台記錄，並使用 syslog 來儲存資料。 此外，使用 Azure 監視器的 Log Analytics 工作區來檢查記錄，並從 Azure 虛擬機器對 syslog 資料執行查詢。

* [如何在 Azure 監視器中執行自訂查詢](../../azure-monitor/log-query/get-started-queries.md)

* [Azure 監視器中的 Syslog 資料來源](../../azure-monitor/platform/data-sources-syslog.md)

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

## <a name="identity-and-access-control"></a>身分識別與存取控制

*如需詳細資訊，請參閱 [安全性控制：身分識別與存取控制](../../security/benchmarks/security-control-identity-access-control.md)。*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1：維護系統管理帳戶的詳細目錄

**指導** 方針：雖然 Azure Active Directory 是管理使用者存取權的建議方法，但 Azure 虛擬機器可能具有本機帳戶。 本機和網域帳戶都應該以最少的使用量進行審核及管理。 此外，利用 Azure Privileged Identity Management 來存取虛擬機器資源所用的系統管理帳戶。

* [本機帳戶的資訊可于](../../active-directory/devices/assign-local-admin.md#manage-the-device-administrator-role)

* [特殊許可權身分識別管理員的資訊](../../active-directory/privileged-identity-management/pim-deployment-plan.md)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="32-change-default-passwords-where-applicable"></a>3.2：在適用的情況下變更預設密碼

**指導** 方針： Linux 虛擬機器和 Azure Active Directory 沒有預設密碼的概念。 客戶負責協力廠商應用程式，以及可能使用預設密碼的 marketplace 服務。

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

### <a name="33-use-dedicated-administrative-accounts"></a>3.3：使用專用的系統管理帳戶

**指導** 方針：使用可存取您虛擬機器的專用系統管理帳戶來建立標準作業程式。 使用 Azure 資訊安全中心身分識別和存取管理來監視系統管理帳戶的數目。 用來存取 Azure 虛擬機器資源的任何系統管理員帳戶也可以由 Azure Privileged Identity Management (PIM) 來管理。 Azure Privileged Identity Management 提供數個選項，例如及時提高許可權、在假設角色之前需要 Multi-Factor Authentication，以及委派選項，讓許可權僅適用于特定的時間範圍，而且需要核准者。

* [瞭解 Azure 資訊安全中心身分識別和存取權](../../security-center/security-center-identity-access.md)

* [特殊許可權身分識別管理員的資訊](../../active-directory/privileged-identity-management/pim-deployment-plan.md)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="34-use-azure-active-directory-single-sign-on-sso"></a>3.4：使用 Azure Active Directory 單一登入 (SSO) 

**指導** 方針：可能的話，客戶可以搭配 AZURE ACTIVE DIRECTORY 使用 SSO，而不是為每個服務設定個別的獨立認證。 使用 Azure 資訊安全中心身分識別和存取管理建議。

* [瞭解 Azure AD 的 SSO](../../active-directory/manage-apps/what-is-single-sign-on.md)

* [如何在 Azure 資訊安全中心監視身分識別和存取](../../security-center/security-center-identity-access.md)

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5：所有以 Azure Active Directory 為基礎的存取都使用多重要素驗證

**指引**：啟用 Azure AD MFA，並遵循 Azure 資訊安全中心的身分識別與存取管理建議。

* [如何在 Azure 中啟用 MFA](../../active-directory/authentication/howto-mfa-getstarted.md)

* [如何在 Azure 資訊安全中心監視身分識別和存取](../../security-center/security-center-identity-access.md)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="36-use-secure-azure-managed-workstations-for-administrative-tasks"></a>3.6：使用安全、受 Azure 管理的工作站進行系統管理工作

**指引**：使用已設定 MFA 的特殊權限存取 PAW (特殊權限存取工作站) 登入和設定 Azure 資源。

* [瞭解特殊權限存取工作站](/windows-server/identity/securing-privileged-access/privileged-access-workstations)

* [如何在 Azure 中啟用 MFA](../../active-directory/authentication/howto-mfa-getstarted.md)

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3.7：來自系統管理帳戶的可疑活動記錄和警示

**指導** 方針：當環境中發生可疑或不安全的活動時，請使用 AZURE AD PRIVILEGED IDENTITY MANAGEMENT (PIM) 來產生記錄和警示。 使用 Azure AD 風險偵測來檢視風險性使用者行為的相關警示和報告。 客戶可以選擇性地將 Azure 資訊安全中心風險偵測警示內嵌至 Azure 監視器，並使用動作群組設定自訂警示/通知。

* [如何部署 Privileged Identity Management (PIM)](../../active-directory/privileged-identity-management/pim-deployment-plan.md)

* [瞭解 Azure 資訊安全中心風險偵測 (可疑活動) ](../../active-directory/identity-protection/overview-identity-protection.md)

* [如何將 Azure 活動記錄整合到 Azure 監視器中](../../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

* [如何設定自訂警示和通知的動作群組](../../azure-monitor/platform/action-groups.md)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8：僅從核准的位置管理 Azure 資源

**指導** 方針：使用 Azure Active Directory 條件式存取原則和命名位置，只允許來自特定 IP 位址範圍或國家/地區的邏輯群組進行存取。

* [如何在 Azure 中設定具名位置](../../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

### <a name="39-use-azure-active-directory"></a>3.9：使用 Azure Active Directory

**指導** 方針：使用 Azure Active Directory (Azure AD) 作為中央驗證和授權系統。 Azure AD 會對待用資料和傳輸中資料使用增強式加密，以保護資料安全。 Azure AD 也會對使用者認證進行 Salt 處理、雜湊處理並安全儲存資料。 您可以使用受控識別來驗證任何支援 Azure AD authentication 的服務，包括 Key Vault，而不需要您程式碼中的任何認證。 您在虛擬機器上執行的程式碼可以使用其受控識別來要求支援 Azure AD authentication 之服務的存取權杖。

* [如何建立及設定 Azure AD 執行個體](../../active-directory-domain-services/tutorial-create-instance.md)

* [Azure 資源受控識別概觀](../../active-directory/managed-identities-azure-resources/overview.md)

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10：定期檢閱並協調使用者存取

**指引**：Azure AD 會提供記錄來協助探索過時的帳戶。 此外，使用 Azure Active Directory 身分識別存取評論，有效率地管理群組成員資格、企業應用程式的存取權，以及角色指派。 您可以定期審核使用者的存取權，以確保只有適當的使用者可以繼續存取。 使用 Azure 虛擬機器時，您將需要檢查本機安全性群組和使用者，以確定沒有任何未預期的帳戶可能會危害系統。

* [如何使用 Azure 身分識別存取權檢閱](../../active-directory/governance/access-reviews-overview.md)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3.11：監視嘗試存取已停用的認證

**指導** 方針：設定 Azure Active Directory 的診斷設定，以將審核記錄和登入記錄傳送至 Log Analytics 工作區。 此外，您也可以使用 Azure 監視器來檢查記錄，並對 Azure 虛擬機器的驗證 syslog 資料執行查詢。

* [了解 Log Analytics 工作區](../../azure-monitor/log-query/log-analytics-tutorial.md)

* [如何將 Azure 活動記錄整合到 Azure 監視器中](../../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

* [如何在 Azure 監視器中執行自訂查詢](../../azure-monitor/log-query/get-started-queries.md)

* [Azure 監視器中的 Syslog 資料來源](../../azure-monitor/platform/data-sources-syslog.md)

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

### <a name="312-alert-on-account-sign-in-behavior-deviation"></a>3.12：帳戶登入行為偏差的警示

**指導** 方針：使用 Azure Active Directory 的風險和身分識別保護功能，對偵測到的儲存體帳戶資源相關的可疑動作設定自動回應。 您應該透過 Azure Sentinel 啟用自動回應，以執行您組織的安全性回應。

* [如何檢視有風險的 Azure AD 登入](../../active-directory/identity-protection/overview-identity-protection.md)

* [如何設定和啟用身分識別保護風險原則](../../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

* [如何使 Azure Sentinel 上線](../../sentinel/quickstart-onboard.md)

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13：在支援案例期間為 Microsoft 提供相關客戶資料的存取權

**指導** 方針：如果協力廠商需要存取客戶資料 (例如) 支援要求時，請使用 Azure 虛擬機器的客戶加密箱來檢查和核准或拒絕客戶資料存取要求。

* [適用於 Microsoft Azure 的客戶加密箱](../../security/fundamentals/customer-lockbox-overview.md)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

## <a name="data-protection"></a>資料保護

*如需詳細資訊，請參閱 [安全性控制：資料保護](../../security/benchmarks/security-control-data-protection.md)。*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1：維護敏感性資訊的詳細目錄

**指導** 方針：使用標籤協助追蹤儲存或處理敏感性資訊的 Azure 虛擬機器。

* [如何建立和使用標籤](../../azure-resource-manager/management/tag-resources.md)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2：隔離儲存或處理敏感性資訊的系統

**指引**：針對開發、測試和生產，實作不同的訂用帳戶及/或管理群組。 資源應以虛擬網路/子網分隔、適當標記，並在網路安全性群組內受到保護， (NSG) 或 Azure 防火牆。 針對儲存或處理敏感性資料的虛擬機器，請在不使用時將原則和程式 (的) 將其關閉。

* [如何建立額外的 Azure 訂閱](../../cost-management-billing/manage/create-subscription.md)

* [如何建立管理群組](../../governance/management-groups/create-management-group-portal.md)

* [如何建立和使用標籤](../../azure-resource-manager/management/tag-resources.md)

* [如何建立虛擬網路](../../virtual-network/quick-create-portal.md)

* [如何建立具有安全性設定的 NSG](../../virtual-network/tutorial-filter-network-traffic.md)

* [如何部署 Azure 防火牆](../../firewall/tutorial-firewall-deploy-portal.md)

* [如何使用 Azure 防火牆設定警示或警示和拒絕](../../firewall/threat-intel.md)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3：監視並封鎖未經授權的敏感性資訊傳輸

**指導** 方針：在網路周邊執行協力廠商解決方案，以監視未經授權的機密資訊傳輸，並封鎖這類傳輸，同時警示資訊安全專業人員。

針對 Microsoft 所管理的基礎平臺，Microsoft 會將所有客戶內容視為機密，以防止客戶資料遺失和公開。 為了確保 Azure 中的客戶資料安全無虞，Microsoft 已實作並維護一套強大的資料保護控制項和功能。

* [瞭解 Azure 中的客戶資料保護](../../security/fundamentals/protection-customer-data.md)

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4：加密傳輸中的所有敏感性資訊

**指導** 方針：在執行 Linux 的虛擬機器 (VM) 之間傳輸至、進出虛擬機器的資料，會以數種方式加密，這取決於連接到 RDP 或 SSH 會話中的 VM 時的連線本質。

Microsoft 在雲端服務與客戶之間移動時，會使用傳輸層安全性 (TLS) 通訊協定來保護資料。

* [VM 中的傳輸中加密](../../security/fundamentals/encryption-overview.md#in-transit-encryption-in-vms)

**Azure 資訊安全中心監視**：無法使用

**責任**：共用

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5：使用作用中探索工具來識別敏感性資料

**指導** 方針：使用協力廠商主動式探索工具來識別組織技術系統儲存、處理或傳輸的所有機密資訊，包括位於現場或遠端服務提供者的機密資訊，以及更新組織的機密資訊清查。

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6：使用 Azure RBAC 來控制資源的存取權

**指導** 方針：使用 azure 角色型存取控制 (azure RBAC) ，您可以將小組內的職責區隔，並只授與 VM 上執行其工作所需的使用者存取權。 您不需為每個人授與 VM 的權限，而是只允許執行特定的動作。 您可以使用 Azure CLI 或 Azure PowerShell，在 Azure 入口網站中設定 VM 的存取控制。

* [Azure RBAC](../../role-based-access-control/overview.md)

* [Azure 內建角色](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor)

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7：使用主機型資料外洩防護來強制執行存取控制

**指導** 方針：執行協力廠商工具（例如自動化的主機型資料遺失防護解決方案），以強制執行存取控制以降低資料缺口的風險。

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8：加密待用的敏感性資訊

**指導** 方針： LINUX 虛擬機器 (VM) 上的虛擬磁片會使用伺服器端加密或 Azure 磁片加密 (ADE) 在待用時加密。 Azure 磁碟加密利用 Linux 的 DM-Crypt 功能，利用來賓 VM 內客戶管理的金鑰來加密受控磁片。 使用客戶管理的金鑰進行伺服器端加密，可讓您藉由加密儲存庫服務中的資料，對您的 VM 使用任何作業系統類型和映像，而改善 ADE 的效能。

* [Azure 受控磁片的伺服器端加密](../disk-encryption.md)

* [適用於 Linux VM 的 Azure 磁碟加密](./disk-encryption-overview.md)

**Azure 資訊安全中心監視**：是

**責任**：共用

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9：針對重要 Azure 資源的變更留下記錄和發出警示

**指導** 方針：使用 Azure 監視器搭配 Azure 活動記錄來建立虛擬機器和相關資源發生變更時的警示。

* [如何建立 Azure 活動記錄事件的警示](../../azure-monitor/platform/alerts-activity-log.md)

* [如何建立 Azure 活動記錄事件的警示](../../azure-monitor/platform/alerts-activity-log.md)

* [Azure 儲存體分析記錄](../../storage/common/storage-analytics-logging.md)

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

## <a name="vulnerability-management"></a>弱點管理

*如需詳細資訊，請參閱 [安全性控制：弱點管理](../../security/benchmarks/security-control-vulnerability-management.md)。*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1：執行自動化弱點掃描工具

**指導** 方針：在 Linux OS 內部，您將需要協力廠商工具來偵測反惡意程式碼弱點。

* [將 Linux 伺服器上架到 Azure 安全性中心的指示](../../security-center/quickstart-onboard-machines.md)

* [Microsoft 建議的安全性指導方針](../security-recommendations.md)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5.2：部署自動化的作業系統修補程式管理解決方案

**指導** 方針：使用 Azure 更新管理解決方案來管理虛擬機器的更新和修補程式。 更新管理依賴本機設定的更新存放庫來修補支援的系統。

* [Azure 中的更新管理解決方案](../../automation/update-management/overview.md)

* [管理 Vm 的更新和修補程式](../../automation/update-management/manage-updates-for-vm.md)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="53-deploy-automated-patch-management-solution-for-third-party-software-titles"></a>5.3：為協力廠商軟體專案部署自動化的修補程式管理解決方案

**指導** 方針：您可以使用協力廠商修補程式管理解決方案。 您可以使用 Azure 更新管理解決方案來管理虛擬機器的更新和修補程式。 更新管理依賴本機設定的更新存放庫來修補支援的系統。

* [Azure 中的更新管理解決方案](../../automation/update-management/overview.md)

* [管理 Vm 的更新和修補程式](../../automation/update-management/manage-updates-for-vm.md)

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4：比較連續性弱點掃描

**指導** 方針：依一致的間隔匯出掃描結果並比較結果，以確認已補救弱點。 使用 Azure 資訊安全中心建議的弱點管理建議時，客戶可能會進入所選解決方案的入口網站以查看歷史掃描資料。

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5：使用風險評等程序來排定所發現弱點的補救優先順序

**指導** 方針：使用 (Azure 資訊安全中心提供的安全分數) 的預設風險評等。

* [瞭解 Azure 資訊安全中心安全分數](../../security-center/secure-score-security-controls.md)

**Azure 資訊安全中心監視**：是

**責任**：客戶

## <a name="inventory-and-asset-management"></a>清查和資產管理

*如需詳細資訊，請參閱 [安全性控制：清查和資產管理](../../security/benchmarks/security-control-inventory-asset-management.md)。*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1：使用自動化資產探索解決方案

**指導** 方針：使用 Azure Resource Graph 來查詢及探索所有資源 (包括訂用帳戶內) 的虛擬機器。 確保租用戶中有適當的 (讀取) 權限，且能列舉所有 Azure 訂用帳戶以及訂用帳戶中的資源。

* [如何使用 Azure Graph 建立查詢](../../governance/resource-graph/first-query-portal.md)

* [如何檢視您的 Azure 訂用帳戶](/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)

* [了解 Azure RBAC](../../role-based-access-control/overview.md)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="62-maintain-asset-metadata"></a>6.2：維護資產中繼資料

**指引**：將標記套用至 Azure 資源，以根據分類法以邏輯方式組織資料。

* [如何建立和使用標籤](../../azure-resource-manager/management/tag-resources.md)

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

### <a name="63-delete-unauthorized-azure-resources"></a>6.3：刪除未經授權的 Azure 資源

**指導** 方針：使用標記、管理群組和個別訂用帳戶（如果適用）來組織和追蹤虛擬機器和相關資源。 請定期調節清查，並確保會及時刪除訂用帳戶中未經授權的資源。

* [如何建立額外的 Azure 訂閱](../../cost-management-billing/manage/create-subscription.md)

* [如何建立管理群組](../../governance/management-groups/create-management-group-portal.md)

* [如何建立和使用標籤](../../azure-resource-manager/management/tag-resources.md)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6.4：定義和維護已核准 Azure 資源的清查

**指導** 方針：您應為您的計算資源建立核准的 Azure 資源和已核准軟體的清查。 您也可以使用自動調整應用程式控制項，這是一項 Azure 資訊安全中心的功能，可協助您定義一組可在已設定的電腦上執行的應用程式。 這項功能適用于 Azure 和非 Azure Windows (所有版本、傳統或 Azure Resource Manager) 和 Linux 機器。

         

* [如何啟用適應性應用程式控制](../../security-center/security-center-adaptive-application.md)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5：監視未經核准的 Azure 資源

**指引**：使用下列內建原則定義，利用 Azure 原則對可在客戶訂閱中建立的資源類型施加限制：
- 不允許的資源類型
- 允許的資源類型

此外，使用 Azure Resource Graph 來查詢/探索其訂用帳戶內的資源。 這有助於以高安全性為基礎的環境，例如具有儲存體帳戶的環境。

* [如何設定和管理 Azure 原則](../../governance/policy/tutorials/create-and-manage.md)

* [如何使用 Azure Graph 建立查詢](../../governance/resource-graph/first-query-portal.md)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6：監視計算資源內未經核准的軟體應用程式

**指導** 方針： Azure 自動化可在部署、作業和解除委任工作負載和資源時，提供完整的控制權。 利用 Azure 虛擬機器清查來自動收集虛擬機器上所有軟體的相關資訊。 注意：您可以從 Azure 入口網站取得軟體名稱、版本、發行者和重新整理時間。 若要取得計量和其他資訊的存取權，客戶必須啟用來賓層級的診斷，並可將 syslog 資訊傳送至指定的儲存體帳戶。

除了使用變更追蹤監視軟體應用程式之外，Azure 資訊安全中心中的適應性應用程式控制使用機器學習服務來分析在電腦上執行的應用程式，並從這項智慧建立允許清單。 這項功能可大幅簡化設定和維護應用程式允許清單原則的程式，讓您可以避免在您的環境中使用不必要的軟體。 您可以設定 audit 模式或強制執行模式。 Audit 模式只會審核受保護 Vm 上的活動。 強制模式會強制執行規則，並確定不允許執行的應用程式會遭到封鎖。

* [Linux 虛擬機器的診斷擴充功能](../extensions/diagnostics-linux.md?toc=/azure/azure-monitor/toc.json)

* [Azure 自動化簡介](../../automation/automation-intro.md)

* [如何啟用 Azure VM 清查](../../automation/automation-tutorial-installed-software.md)

* [瞭解適應性應用程式控制](../../security-center/security-center-adaptive-application.md)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7：移除未經核准的 Azure 資源和軟體應用程式

**指導** 方針： Azure 自動化可在部署、作業和解除委任工作負載和資源時，提供完整的控制權。 您可以使用變更追蹤來識別安裝在虛擬機器上的所有軟體。 您可以執行自己的處理常式，或使用 Azure 自動化狀態設定來移除未經授權的軟體。

* [Azure 自動化簡介](../../automation/automation-intro.md)

* [瞭解變更追蹤](../../automation/change-tracking/overview.md)

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

### <a name="68-use-only-approved-applications"></a>6.8：僅使用已核准的應用程式

**指導** 方針：使用 Azure 資訊安全中心的自動調整應用程式控制，以確保只有授權的軟體會執行，而且所有未經授權的軟體都會在 Azure 虛擬機器上遭到封鎖而無法執行。

* [如何使用 Azure 資訊安全中心適應性應用程式控制](../../security-center/security-center-adaptive-application.md)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="69-use-only-approved-azure-services"></a>6.9：僅使用已核准的 Azure 服務

**指引**：使用下列內建原則定義，利用 Azure 原則對可在客戶訂閱中建立的資源類型施加限制：
- 不允許的資源類型
- 允許的資源類型

* [如何設定和管理 Azure 原則](../../governance/policy/tutorials/create-and-manage.md)

* [如何使用 Azure 原則拒絕特定的資源類型](../../governance/policy/samples/index.md)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="610-maintain-an-inventory-of-approved-software-titles"></a>6.10：維護已核准軟體標題的清查

**指導** 方針：自我調整應用程式控制是 Azure 資訊安全中心的智慧型、自動化、端對端解決方案，可協助您控制哪些應用程式可以在 azure 和非 azure 電腦上執行 (Windows 和 Linux) 。 如果這不符合您組織的需求，請執行協力廠商解決方案。

* [如何使用 Azure 資訊安全中心適應性應用程式控制](../../security-center/security-center-adaptive-application.md)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6.11：限制使用者與 Azure Resource Manager 互動的能力

**指引**：使用 Azure 條件式存取，藉由對「Microsoft Azure 管理」應用程式設定「封鎖存取」，以限制使用者與 Azure Resource Manager 互動的能力。

* [如何設定條件式存取以封鎖 Azure Resource Manager 的存取](../../role-based-access-control/conditional-access-azure-management.md)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6.12：限制使用者在計算資源內執行指令碼的能力

**指導** 方針：根據腳本的類型而定，您可以使用作業系統專屬設定或協力廠商資源，以限制使用者在 Azure 計算資源內執行腳本的能力。 您也可以運用 Azure 資訊安全中心的自我調整應用程式控制，以確保只有授權的軟體會執行，而且所有未經授權的軟體都會在 Azure 虛擬機器上遭到封鎖而無法執行。

* [如何使用 Azure 資訊安全中心適應性應用程式控制](../../security-center/security-center-adaptive-application.md)

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13：以實體或邏輯方式隔離高風險的應用程式

**指導** 方針：在 azure 環境中部署的高風險應用程式可能會使用虛擬網路、子網、訂用帳戶、管理群組進行隔離，並使用 Azure 防火牆、Web 應用程式防火牆 (WAF) 或網路安全性群組 (NSG) 來充分保護這些應用程式。

* [Azure 中的虛擬網路和虛擬機器](../network-overview.md)

* [Azure 防火牆概觀](../../firewall/overview.md)

* [Web 應用程式防火牆概觀](../../web-application-firewall/overview.md)

* [網路安全性概觀](../../virtual-network/network-security-groups-overview.md)

* [Azure 虛擬網路總覽](../../virtual-network/virtual-networks-overview.md)

* [使用 Azure 管理群組來組織資源](../../governance/management-groups/overview.md)

* [訂用帳戶決策指南](/azure/cloud-adoption-framework/decision-guides/subscriptions/)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

## <a name="secure-configuration"></a>安全設定

*如需詳細資訊，請參閱 [安全性控制：安全設定](../../security/benchmarks/security-control-secure-configuration.md)。*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1：為所有 Azure 資源建立安全設定

**指導** 方針：使用 Azure 原則或 Azure 資訊安全中心來維護所有 Azure 資源的安全性設定。 此外，Azure Resource Manager 能夠在 JavaScript 物件標記法 (的 JSON) 中匯出範本，您應該檢查這些設定，以確保設定符合/超過公司的安全性需求。

* [如何設定和管理 Azure 原則](../../governance/policy/tutorials/create-and-manage.md)

* [如何下載 VM 範本的資訊](../windows/download-template.md)

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

### <a name="72-establish-secure-operating-system-configurations"></a>7.2：建立安全的作業系統設定

**指導** 方針：使用 Azure 資訊安全中心建議 [補救您虛擬機器上安全性設定中的弱點]，以維護所有計算資源上的安全性設定。

* [如何監視 Azure 資訊安全中心建議](../../security-center/security-center-recommendations.md)

* [如何修復 Azure 資訊安全中心建議](../../security-center/security-center-remediate-recommendations.md)

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3：維護安全的 Azure 資源設定

**指導** 方針：使用 Azure Resource Manager 範本和 Azure 原則，安全地設定與虛擬機器相關聯的 Azure 資源。 Azure Resource Manager 範本是以 JSON 為基礎的檔案，用來部署虛擬機器以及 Azure 資源，而且必須維護自訂範本。 Microsoft 會在基底範本上執行維護。 使用 Azure 原則 [拒絕] 和 [在不存在時部署]，對您的 Azure 資源強制使用安全設定。

* [建立 Azure Resource Manager 範本的資訊](../windows/ps-template.md)

* [如何設定和管理 Azure 原則](../../governance/policy/tutorials/create-and-manage.md)

* [了解 Azure 原則效果](../../governance/policy/concepts/effects.md)

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

### <a name="74-maintain-secure-operating-system-configurations"></a>7.4：維護安全的作業系統設定

**指導** 方針：有數個選項可維護 Azure 虛擬機器的安全設定， (VM) 以進行部署：

1 Azure Resource Manager 範本：這些是以 JSON 為基礎的檔案，用來從 Azure 入口網站部署 VM，而且必須維護自訂範本。 Microsoft 會在基底範本上執行維護。

2-自訂虛擬硬碟 (VHD) ：在某些情況下，可能需要使用自訂 VHD 檔案，例如在處理無法透過其他方式管理的複雜環境時。

3-Azure 自動化狀態設定：一旦部署基底作業系統之後，就可以使用它來更細微地控制設定，並透過自動化架構強制執行。

在大部分的情況下，與 Azure 自動化 Desired State Configuration 結合的 Microsoft 基底 VM 範本，可協助滿足和維護安全性需求。

* [如何下載 VM 範本的資訊](../windows/download-template.md)

* [建立 ARM 範本的資訊](../windows/ps-template.md)

* [如何將自訂 VM VHD 上傳至 Azure](/azure-stack/operator/azure-stack-add-vm-image?view=azs-1910&preserve-view=true)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5：安全地儲存 Azure 資源的設定

**指導** 方針：使用 Azure DevOps/存放庫來安全地儲存和管理您的程式碼，例如自訂 Azure 原則定義、Azure Resource Manager 範本、Desired State Configuration 腳本和其他程式碼。 若要存取您在 Azure DevOps 中管理的資源，例如程式碼、組建和工作追蹤，您必須擁有這些特定資源的許可權。 大部分的許可權都是由內建安全性群組授與，如許可權和存取權中所述。 您可以授與或拒絕特定使用者、內建安全性群組或 Azure Active Directory (Azure AD) （如果與 Azure DevOps 整合）中定義的群組，或與 TFS 整合時的 Active Directory。

* [如何在 Azure DevOps 中儲存程式碼](/azure/devops/repos/git/gitworkflow?view=azure-devops)

* [關於 Azure DevOps 中的許可權和群組](/azure/devops/organizations/security/about-permissions)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="76-securely-store-custom-operating-system-images"></a>7.6：安全地儲存自訂作業系統映像

**指導** 方針：如果使用自訂映射 (例如虛擬硬碟) ，請使用 azure 角色型存取控制 (azure RBAC) ，以確保只有獲得授權的使用者可以存取映射。

* [了解 Azure RBAC](../../role-based-access-control/rbac-and-directory-admin-roles.md)

* [如何設定 Azure RBAC](../../role-based-access-control/quickstart-assign-role-user-portal.md)

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7：部署適用于 Azure 資源的設定管理工具

**指導** 方針：利用 Azure 原則來警示、審核和強制執行虛擬機器的系統組態。 此外，開發流程和管線以管理原則例外狀況。

* [如何設定和管理 Azure 原則](../../governance/policy/tutorials/create-and-manage.md)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="78-deploy-configuration-management-tools-for-operating-systems"></a>7.8：部署作業系統的設定管理工具

**指導** 方針： Azure 自動化狀態設定是在任何雲端或內部部署資料中心內 DESIRED STATE CONFIGURATION (DSC) 節點的設定管理服務。 它可讓您從中央、安全的位置快速且輕鬆地延展性到數千部電腦。 您可以輕鬆地上架機器、指派它們宣告式組態和檢視顯示每個電腦的符合性報告 (達您指定的所需狀態)。

* [將機器上架交由 Azure Automation State Configuration 管理](../../automation/automation-dsc-onboarding.md)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9：執行 Azure 資源的自動化設定監視

**指導** 方針：利用 Azure 資訊安全中心來執行 Azure 虛擬機器的基準掃描。 自動化設定的其他方法包括使用 Azure 自動化狀態設定。

* [如何修復 Azure 資訊安全中心中的建議](../../security-center/security-center-remediate-recommendations.md)

* [開始使用 Azure Automation State Configuration](../../automation/automation-dsc-getting-started.md)

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10：為作業系統實作自動化的設定監視

**指導** 方針： Azure 自動化狀態設定是在任何雲端或內部部署資料中心內 DESIRED STATE CONFIGURATION (DSC) 節點的設定管理服務。 它可讓您從中央、安全的位置快速且輕鬆地延展性到數千部電腦。 您可以輕鬆地上架機器、指派它們宣告式組態和檢視顯示每個電腦的符合性報告 (達您指定的所需狀態)。

* [將機器上架交由 Azure Automation State Configuration 管理](../../automation/automation-dsc-onboarding.md)

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

### <a name="711-manage-azure-secrets-securely"></a>7.11：安全地管理 Azure 秘密

**指導** 方針：使用受控服務識別搭配 Azure Key Vault，以簡化和保護雲端應用程式的秘密管理。

* [如何與 Azure 受控識別整合](../../azure-app-configuration/howto-integrate-azure-managed-service-identity.md)

* [如何建立 Key Vault](../../key-vault/general/quick-create-portal.md)

* [如何驗證 Key Vault](../../key-vault/general/authentication.md)

* [如何指派 Key Vault 存取原則](../../key-vault/general/assign-access-policy-portal.md)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="712-manage-identities-securely-and-automatically"></a>7.12：安全且自動地管理身分識別

**指導** 方針：使用受控識別，在 Azure AD 中為 Azure 服務提供自動管理的身分識別。 受控識別可供對支援 Azure AD 驗證的任何服務進行驗證 (包括 Key Vault)，不需要程式碼中的任何認證。

* [如何設定受控識別](../../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md)

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13：消除非預期的認證公開

**指引**：實作認證掃描器來識別程式碼中的認證。 認證掃描器也有助於將探索到的認證移至更安全的位置，例如 Azure Key Vault。

* [如何設定認證掃描器](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

## <a name="malware-defense"></a>惡意程式碼防禦

*如需詳細資訊，請參閱 [安全性控制：惡意程式碼防禦](../../security/benchmarks/security-control-malware-defense.md)。*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8.1：使用集中管理的反惡意程式碼軟體

**指導** 方針：在 Azure Linux 虛擬機器中，您將需要適用于反惡意程式碼防護的協力廠商工具。

* [如何設定雲端服務和虛擬機器的 Microsoft Antimalware](../security-recommendations.md)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2：預先掃描要上傳至非計算 Azure 資源的檔案

**指導** 方針：不適用 Azure 虛擬機器，因為它是計算資源。

**Azure 資訊安全中心監視**：不適用

**責任**：不適用

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8.3：確保更新反惡意程式碼軟體和簽章

**指導** 方針：在 Azure Linux 虛擬機器中，您將需要適用于反惡意程式碼防護的協力廠商工具。

* [如何設定雲端服務和虛擬機器的 Microsoft Antimalware](../security-recommendations.md)

**Azure 資訊安全中心監視**：是

**責任**：客戶

## <a name="data-recovery"></a>資料復原

*如需詳細資訊，請參閱 [安全性控制：資料復原](../../security/benchmarks/security-control-data-recovery.md)。*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1：確定定期自動備份

**指導** 方針：啟用 Azure 備份並設定 Azure 虛擬機器 (VM) ，以及自動備份所需的頻率和保留期限。

* [Azure VM 備份的總覽](../../backup/backup-azure-vms-introduction.md)

* [從 VM 設定備份 Azure VM](../../backup/backup-azure-vms-first-look-arm.md)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2：執行完整的系統備份並備份任何客戶管理的金鑰

**指導** 方針：使用 POWERSHELL 或 REST Api 建立 Azure 虛擬機器的快照集，或連接到這些實例的受控磁片。 在 Azure Key Vault 中備份任何客戶管理的金鑰。

啟用 Azure 備份，並將 Azure 虛擬機器的目標設為 (VM) ，以及所需的頻率和保留期限。 這包括完整的系統狀態備份。 如果您使用 Azure 磁片加密，Azure VM 備份會自動處理客戶管理金鑰的備份。

* [使用加密的 Azure Vm 上的備份](../../backup/backup-azure-vms-encryption.md)

* [Azure VM 備份總覽](../../backup/backup-azure-vms-introduction.md)

* [瞭解自動備份的運作方式](../../backup/backup-azure-vms-encryption.md)

* [如何在 Azure 中備份金鑰保存庫金鑰](/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3：驗證所有備份，包括客戶管理的金鑰

**指導** 方針：確保能夠定期對 Azure 備份內的內容進行資料還原。 如有必要，請測試將內容還原至隔離的虛擬網路或訂用帳戶。 客戶可測試已備份之客戶管理金鑰的還原。

如果您使用 Azure 磁片加密，您可以使用磁片加密金鑰來還原 Azure VM。 使用磁片加密時，您可以使用磁片加密金鑰來還原 Azure VM。

* [使用加密的 Azure Vm 上的備份](../../backup/backup-azure-vms-encryption.md)

* [如何從 Azure 虛擬機器備份復原檔案](../../backup/backup-azure-restore-files-from-vm.md)

* [如何在 Azure 中還原金鑰保存庫金鑰](/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0)

* [如何備份和還原已加密的 VM](../../backup/backup-azure-vms-encryption.md)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4：確保備份和客戶管理的金鑰的保護

**指導** 方針：當您使用 Azure 備份備份 Azure vm 時，vm 會以儲存體服務加密 (SSE) 進行待用加密。 Azure 備份也可以使用 Azure 磁碟加密來備份已加密的 Azure Vm。 Azure 磁碟加密也會與 (Kek) 的 Azure Key Vault 金鑰加密金鑰整合。 在 Key Vault 中啟用虛刪除，以防止金鑰遭到意外或惡意刪除。 

* [Vm 的虛刪除](../../backup/soft-delete-virtual-machines.md)

* [Azure Key Vault 虛刪除概觀](../../key-vault/general/soft-delete-overview.md)

**Azure 資訊安全中心監視**：是

**責任**：客戶

## <a name="incident-response"></a>事件回應

*如需詳細資訊，請參閱 [安全性控制：事件回應](../../security/benchmarks/security-control-incident-response.md)。*

### <a name="101-create-an-incident-response-guide"></a>10.1：建立事件回應指南

**指引**：為組織製作事件回應指南。 請確定有書面的事件回應計畫，其中定義人員的所有角色，以及從偵測到事件後檢討的事件處理/管理階段。

* [建立自有安全性事件回應程序的指引](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

* [Microsoft 安全性回應中心的事件剖析](https://msrc-blog.microsoft.com/2019/06/27/inside-the-msrc-anatomy-of-a-ssirp-incident/)

* [客戶也可以利用 NIST 的電腦安全性性事件處理指南來協助建立自己的事件回應計畫](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2：建立事件評分和優先順序程序

**指引**：資訊安全中心會指派每個警示的嚴重性，以協助設定應優先調查哪些警示。 嚴重性會依據資訊安全中心對用於發出警示的發現或分析其信心程度，以及信賴等級具有活動背後會導致警示的惡意意圖。 此外，使用標記清楚地標示訂用帳戶 (例如， 生產、非生產) 並建立命名系統，以清楚地識別及分類 Azure 資源，尤其是處理敏感性資料的資源。 您需負責根據發生事件的 Azure 資源和環境的重要性，設定警示的補救優先順序。

* [Azure 資訊安全中心的安全性警示](../../security-center/security-center-alerts-overview.md)

* [使用標記來組織 Azure 資源](../../azure-resource-manager/management/tag-resources.md)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="103-test-security-response-procedures"></a>10.3：測試安全性回應程序

**指引**：進行練習以定期測試系統的事件回應功能，進而協助保護您的 Azure 資源。

* [視需要找出弱式點和間隙和修訂計畫。請參閱 NIST 的發行集：適用于 IT 方案和功能的測試、訓練和練習程式指南](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4：提供安全性事件連絡人詳細資料，並設定安全性事件的警示通知

**指引**：如果 Microsoft 安全性回應中心 (MSRC) 發現您的資料遭到非法或未經授權的對象存取，Microsoft 將使用安全性事件連絡資訊來連絡您。 事後檢討事件，確保問題已解決。

* [如何設定 Azure 資訊安全中心的安全性連絡人](../../security-center/security-center-provide-security-contact-details.md)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5：將安全性警示併入事件回應系統

**指引**：使用「連續匯出」功能來匯出 Azure 資訊安全中心警示和建議，協助找出 Azure 資源的風險。 「連續匯出」可讓您以手動或持續不斷的方式來匯出警示和建議。 您可使用 Azure 資訊安全中心的資料連接器，將警示串流至 Azure Sentinel。

* [如何設定連續匯出](../../security-center/continuous-export.md)

* [如何將警示串流至 Azure Sentinel](../../sentinel/connect-azure-security-center.md)

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

### <a name="106-automate-the-response-to-security-alerts"></a>10.6：自動回應安全性警示

**指導** 方針：使用 Azure 資訊安全中心中的工作流程自動化功能，透過「Logic Apps」安全性警示和建議來自動觸發回應，以保護您的 Azure 資源。

* [如何設定工作流程自動化和 Logic Apps](../../security-center/workflow-automation.md)

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

## <a name="penetration-tests-and-red-team-exercises"></a>滲透測試和 Red Team 練習

*如需詳細資訊，請參閱 [安全性控制：滲透測試和 Red Team 練習](../../security/benchmarks/security-control-penetration-tests-red-team-exercises.md)。*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11.1：進行 Azure 資源的定期滲透測試，並確保修復所有重要的安全性結果

**指導** 方針：遵循 Microsoft 的 Engagement 規則，以確保您的滲透測試不違反 Microsoft 原則。 針對受 Microsoft 管理的雲端基礎結構、服務和應用程式，使用 Microsoft 的策略和執行的 Red 小組和即時網站滲透測試。

* [滲透測試運作規則](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

* [Microsoft Cloud Red Teaming](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure 資訊安全中心監視**：不適用

**責任**：共用

## <a name="next-steps"></a>後續步驟

- 請參閱 [Azure 安全性效能評定](../../security/benchmarks/overview.md)
- 深入了解 [Azure 資訊安全性基準](../../security/benchmarks/security-baselines-overview.md)
