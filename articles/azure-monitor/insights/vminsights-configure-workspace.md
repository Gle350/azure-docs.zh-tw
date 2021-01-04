---
title: 針對適用於 VM 的 Azure 監視器來設定 Log Analytics 工作區
description: 說明如何建立及設定適用於 VM 的 Azure 監視器所使用的 Log Analytics 工作區。
ms.subservice: ''
ms.topic: conceptual
ms.custom: references_regions
author: bwren
ms.author: bwren
ms.date: 12/22/2020
ms.openlocfilehash: 2625da3a397c2cdcf7880fb371d13e63caeb9ab1
ms.sourcegitcommit: 44844a49afe8ed824a6812346f5bad8bc5455030
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/23/2020
ms.locfileid: "97740567"
---
# <a name="configure-log-analytics-workspace-for-azure-monitor-for-vms"></a>針對適用於 VM 的 Azure 監視器來設定 Log Analytics 工作區
適用於 VM 的 Azure 監視器會從 Azure 監視器中的一或多個 Log Analytics 工作區收集其資料。 在將代理程式上架之前，您必須先建立並設定工作區。 本文說明工作區的需求，並針對適用於 VM 的 Azure 監視器進行設定。

## <a name="overview"></a>總覽
依您的需求而定，單一訂用帳戶可以使用任意數目的工作區。 工作區的唯一需求是位於支援的位置，並使用 *VMInsights* 方案進行設定。

設定好工作區之後，您可以使用任何可用的選項，在虛擬機器和虛擬機器擴展集上安裝所需的代理程式，並指定其傳送資料的工作區。 適用於 VM 的 Azure 監視器將會從其訂用帳戶中任何已設定的工作區收集資料。

> [!NOTE]
> 當您使用 Azure 入口網站在單一虛擬機器或虛擬機器擴展集上啟用適用於 VM 的 Azure 監視器時，系統會提供您選取現有工作區或建立新工作區的選項。 *VMInsights* 解決方案將會安裝在此工作區中（如果尚未安裝）。 然後，您可以將此工作區用於其他代理程式。


## <a name="create-log-analytics-workspace"></a>建立 Log Analytics 工作區

>[!NOTE]
>本節所述的資訊也適用于 [服務對應的解決方案](service-map.md)。 

從 **Log analytics 工作區** 功能表存取 Azure 入口網站中的 log analytics 工作區。

[![記錄 Anlytics 工作區](media/vminsights-configure-workspace/log-analytics-workspaces.png)](media/vminsights-configure-workspace/log-analytics-workspaces.png#lightbox)

您可以使用下列任何一種方法來建立新的 Log Analytics 工作區。 請參閱 [設計 Azure 監視器記錄部署](../platform/design-logs-deployment.md) ，以取得決定您應在環境中使用的工作區數目，以及如何設計其存取策略的指引。


* [Azure 入口網站](../../azure-monitor/learn/quick-create-workspace.md)
* [Azure CLI](../../azure-monitor/learn/quick-create-workspace-cli.md)
* [PowerShell](../platform/powershell-workspace-configuration.md)
* [Azure Resource Manager](../samples/resource-manager-workspace.md)

## <a name="supported-regions"></a>支援區域
適用於 VM 的 Azure 監視器支援 Log analytics 所支援的任何區域中的 Log Analytics 工作區，但下列 [情況](https://azure.microsoft.com/global-infrastructure/services/?products=monitor&regions=all) 除外：

- 德國中西部
- 南韓中部

>[!NOTE]
>您可以監視任何區域中的 Azure Vm。 Vm 本身不限於 Log Analytics 工作區支援的區域。

## <a name="azure-role-based-access-control"></a>Azure 角色型存取控制
若要啟用和存取適用於 VM 的 Azure 監視器中的功能，您必須在工作區中擁有 [Log Analytics 參與者角色](../platform/manage-access.md#manage-access-using-azure-permissions) 。 若要查看效能、健康情況和對應資料，您必須擁有 Azure VM 的「 [監視讀者」角色](../platform/roles-permissions-security.md#built-in-monitoring-roles) 。 如需有關如何控制 Log Analytics 工作區存取的詳細資訊，請參閱[管理工作區](../platform/manage-access.md)。

## <a name="add-vminsights-solution-to-workspace"></a>將 VMInsights 解決方案新增至工作區
在 Log Analytics 工作區可搭配適用於 VM 的 Azure 監視器使用之前，必須先安裝 *VMInsights* 解決方案。 下列各節將說明設定工作區的方法。

> [!NOTE]
> 當您將 *VMInsights* 解決方案新增至工作區時，所有連線到該工作區的現有虛擬機器都將開始傳送資料至 InsightsMetrics。 在您將 Dependency Agent 新增至連線至工作區的現有虛擬機器之前，將不會收集其他資料類型的資料。

### <a name="azure-portal"></a>Azure 入口網站
有三個選項可供您使用 Azure 入口網站設定現有的工作區。 各項目分述如下。

若要設定單一工作區，請移至 [ **Azure 監視器**] 功能表中的 [**虛擬機器**] 選項、選取 **其他上線選項**，然後 **設定工作區**。 選取訂用帳戶和工作區，然後按一下 [ **設定**]。

[![設定工作區](media/vminsights-enable-at-scale-policy/configure-workspace.png)](media/vminsights-enable-at-scale-policy/configure-workspace.png#lightbox)

若要設定多個工作區，請在 Azure 入口網站的 [**監視**] 功能表的 [**虛擬機器**] 功能表中，選取 [**工作區** 設定] 索引標籤 設定篩選值以顯示現有工作區的清單。 選取要啟用的每個工作區旁的核取方塊，然後按一下 [ **設定選取**]。

[![工作區設定](media/vminsights-enable-at-scale-policy/workspace-configuration.png)](media/vminsights-enable-at-scale-policy/workspace-configuration.png#lightbox)


當您使用 Azure 入口網站在單一虛擬機器或虛擬機器擴展集上啟用適用於 VM 的 Azure 監視器時，系統會提供您選取現有工作區或建立新工作區的選項。 *VMInsights* 解決方案將會安裝在此工作區中（如果尚未安裝）。 然後，您可以將此工作區用於其他代理程式。

[![在入口網站中啟用單一 VM](media/vminsights-enable-single-vm/enable-vminsights-vm-portal.png)](media/vminsights-enable-single-vm/enable-vminsights-vm-portal.png#lightbox)


### <a name="resource-manager-template"></a>Resource Manager 範本
您可以 [從我們的 GitHub 存放庫下載](https://aka.ms/VmInsightsARMTemplates)的封存檔案 ( .zip) 中提供適用於 VM 的 Azure 監視器的 Azure Resource Manager 範本。 這包括一個稱為 **ConfigureWorkspace** 的範本，可設定適用於 VM 的 Azure 監視器的 Log Analytics 工作區。 您可以使用任何標準方法來部署此範本，包括範例 PowerShell 和下列 CLI 命令： 

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli
az deployment group create --name ConfigureWorkspace --resource-group my-resource-group --template-file CreateWorkspace.json  --parameters workspaceResourceId='/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/my-resource-group/providers/microsoft.operationalinsights/workspaces/my-workspace' workspaceLocation='eastus'

```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```powershell
New-AzResourceGroupDeployment -Name ConfigureWorkspace -ResourceGroupName my-resource-group -TemplateFile ConfigureWorkspace.json -workspaceResourceId /subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/my-resource-group/providers/microsoft.operationalinsights/workspaces/my-workspace -location eastus
```

---



## <a name="next-steps"></a>後續步驟
- 請參閱將代理程式上 [架到適用於 VM 的 Azure 監視器](vminsights-enable-overview.md) ，以將代理程式連線到適用於 VM 的 Azure 監視器。
- 請參閱 [Azure 監視器 (Preview 中的目標監視解決方案) ](solution-targeting.md) ，以限制從方案傳送至工作區的資料量。
