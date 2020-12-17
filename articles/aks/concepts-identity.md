---
title: 概念 - Azure Kubernetes Service (AKS) 中的存取與身分識別
description: 瞭解 Azure Kubernetes Service (AKS) 中的存取和身分識別，包括 Azure Active Directory 整合、Kubernetes 以角色為基礎的存取控制 (Kubernetes RBAC) ，以及角色和系結。
services: container-service
ms.topic: conceptual
ms.date: 07/07/2020
author: palma21
ms.author: jpalma
ms.openlocfilehash: 3c291d9a9d48b6f75148b673848b8451521bab91
ms.sourcegitcommit: 86acfdc2020e44d121d498f0b1013c4c3903d3f3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/17/2020
ms.locfileid: "97615796"
---
# <a name="access-and-identity-options-for-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) 的存取與身分識別選項

有不同的方式可驗證、控制存取/授權和保護 Kubernetes 叢集。 使用 Kubernetes 角色型存取控制 (Kubernetes RBAC) ，您可以授與使用者、群組和服務帳戶只存取所需資源的存取權。 使用 Azure Kubernetes Service (AKS) ，您可以使用 Azure Active Directory 和 Azure RBAC 進一步增強安全性和許可權結構。 這些方法可協助您保護叢集存取，並僅提供開發人員和操作員所需的最低許可權。

本文介紹可協助您在 AKS 中驗證和指派許可權的核心概念。

## <a name="aks-service-permissions"></a>AKS 服務許可權

建立叢集時，AKS 會建立或修改建立和執行叢集所需的資源，例如 Vm 和 Nic，代表建立叢集的使用者。 此身分識別與叢集的身分識別許可權不同，它是在叢集建立期間所建立。

### <a name="identity-creating-and-operating-the-cluster-permissions"></a>建立和操作叢集許可權的身分識別

建立和操作叢集的身分識別需要下列許可權。

| 權限 | 原因 |
|---|---|
| Microsoft. Compute/diskEncryptionSets/read | 需要讀取磁片加密集識別碼。 |
| Microsoft. Compute/proximityPlacementGroups/write | 更新鄰近放置群組所需。 |
| Microsoft.Network/applicationGateways/read <br/> Microsoft.Network/applicationGateways/write <br/> Microsoft.Network/virtualNetworks/subnets/join/action | 設定應用程式閘道和加入子網所需。 |
| Microsoft.Network/virtualNetworks/subnets/join/action | 使用自訂 VNET 時，必須設定子網的網路安全性群組。|
| Microsoft.Network/publicIPAddresses/join/action <br/> Microsoft.Network/publicIPPrefixes/join/action | 在 Standard Load Balancer 上設定輸出公用 Ip 所需。 |
| OperationalInsights/workspace/sharedkeys/read <br/> Microsoft.OperationalInsights/workspaces/read <br/> Microsoft.OperationsManagement/solutions/write <br/> Microsoft.OperationsManagement/solutions/read <br/> Microsoft.ManagedIdentity/userAssignedIdentities/assign/action | 建立和更新 Log Analytics 工作區和適用于容器的 Azure 監視所需。 |

### <a name="aks-cluster-identity-permissions"></a>AKS 叢集身分識別許可權

AKS 叢集身分識別會使用下列許可權，該身分識別會在建立叢集時建立並與 AKS 叢集建立關聯。 基於下列原因，會使用每個許可權：

| 權限 | 原因 |
|---|---|
| Microsoft.Network/loadBalancers/delete <br/> Microsoft.Network/loadBalancers/read <br/> Microsoft.Network/loadBalancers/write | 設定 LoadBalancer 服務的負載平衡器所需。 |
| Microsoft.Network/publicIPAddresses/delete <br/> Microsoft.Network/publicIPAddresses/read <br/> Microsoft.Network/publicIPAddresses/write | 尋找和設定 LoadBalancer 服務的公用 Ip 時需要。 |
| Microsoft.Network/publicIPAddresses/join/action | 設定 LoadBalancer 服務的公用 Ip 所需。 |
| Microsoft.Network/networkSecurityGroups/read <br/> Microsoft.Network/networkSecurityGroups/write | 建立或刪除 LoadBalancer 服務的安全性規則所需。 |
| Microsoft.Compute/disks/delete <br/> Microsoft.Compute/disks/read <br/> Microsoft.Compute/disks/write <br/> Microsoft. 計算/位置/DiskOperations/讀取 | 設定 AzureDisks 時所需。 |
| Microsoft.Storage/storageAccounts/delete <br/> Microsoft.Storage/storageAccounts/listKeys/action <br/> Microsoft.Storage/storageAccounts/read <br/> Microsoft.Storage/storageAccounts/write <br/> Microsoft.Storage/operations/read | 設定 AzureFile 或 AzureDisk 的儲存體帳戶所需。 |
| Microsoft.Network/routeTables/read <br/> Microsoft.Network/routeTables/routes/delete <br/> Microsoft.Network/routeTables/routes/read <br/> Microsoft.Network/routeTables/routes/write <br/> Microsoft.Network/routeTables/write | 設定節點的路由表和路由所需。 |
| Microsoft.Compute/virtualMachines/read | 需要在 VMAS 中尋找虛擬機器的資訊，例如區域、容錯網域、大小和資料磁片。 |
| Microsoft.Compute/virtualMachines/write | 將 AzureDisks 附加至 VMAS 中的虛擬機器所需。 |
| Microsoft.Compute/virtualMachineScaleSets/read <br/> Microsoft.Compute/virtualMachineScaleSets/virtualMachines/read <br/> Microsoft. Compute/virtualMachineScaleSets/virtualmachines/instanceView/read | 需要尋找虛擬機器擴展集中虛擬機器的資訊，例如區域、容錯網域、大小和資料磁片。 |
| Microsoft.Network/networkInterfaces/write | 將 VMAS 中的虛擬機器新增至負載平衡器後端位址集區的必要項。 |
| Microsoft.Compute/virtualMachineScaleSets/write | 必須將虛擬機器擴展集新增至負載平衡器後端位址集區，並將虛擬機器擴展集中的節點相應放大。 |
| Microsoft. Compute/virtualMachineScaleSets/virtualmachines/write | 需要附加 AzureDisks，並將虛擬機器從虛擬機器擴展集新增至負載平衡器。 |
| Microsoft.Network/networkInterfaces/read | 針對 VMAS 中的虛擬機器搜尋內部 Ip 和負載平衡器後端位址集區所需。 |
| Microsoft.Compute/virtualMachineScaleSets/virtualMachines/networkInterfaces/read | 針對虛擬機器擴展集中的虛擬機器，搜尋內部 Ip 和負載平衡器後端位址集區所需。 |
| Microsoft. Compute/virtualMachineScaleSets/virtualMachines/networkInterfaces/ipconfiguration/publicipaddresses/read | 需要在虛擬機器擴展集中尋找虛擬機器的公用 Ip。 |
| Microsoft.Network/virtualNetworks/read <br/> Microsoft.Network/virtualNetworks/subnets/read | 需要確認是否有另一個資源群組中的內部負載平衡器的子網。 |
| Microsoft.Compute/snapshots/delete <br/> Microsoft.Compute/snapshots/read <br/> Microsoft.Compute/snapshots/write | 設定 AzureDisk 的快照集所需。 |
| Microsoft.Compute/locations/vmSizes/read <br/> Microsoft.Compute/locations/operations/read | 尋找用來尋找 AzureDisk 磁片區限制的虛擬機器大小所需。 |

### <a name="additional-cluster-identity-permissions"></a>其他叢集身分識別許可權

使用特定屬性建立叢集時，叢集身分識別需要下列其他許可權。 系統不會自動指派這些許可權，因此您必須在叢集身分識別建立之後，將這些許可權新增至叢集身分識別。

| 權限 | 原因 |
|---|---|
| Microsoft.Network/networkSecurityGroups/write <br/> Microsoft.Network/networkSecurityGroups/read | 如果使用另一個資源群組中的網路安全性群組，則為必要項。 設定 LoadBalancer 服務的安全性規則所需。 |
| Microsoft.Network/virtualNetworks/subnets/read <br/> Microsoft.Network/virtualNetworks/subnets/join/action | 如果在其他資源群組（例如自訂 VNET）中使用子網，則為必要項。 |
| Microsoft.Network/routeTables/routes/read <br/> Microsoft.Network/routeTables/routes/write | 如果使用與另一個資源群組中的路由表相關聯的子網（例如自訂路由表的自訂 VNET），則為必要項。 需要確認子網是否已存在於其他資源群組中的子網。 |
| Microsoft.Network/virtualNetworks/subnets/read | 如果使用另一個資源群組中的內部負載平衡器，則為必要項。 需要確認資源群組中的內部負載平衡器是否已有子網存在。 |

## <a name="kubernetes-role-based-access-control-kubernetes-rbac"></a>Kubernetes 以角色為基礎的存取控制 (Kubernetes RBAC) 

為了提供使用者可執行之動作的細微篩選，Kubernetes 會使用 Kubernetes 角色型存取控制 (Kubernetes RBAC) 。 此控制機制可讓您指派權限給使用者或使用者群組，以執行像是建立或修改資源，或檢視執行中應用程式工作負載的記錄等動作。 這些權限可以只限於單一命名空間，或授與給整個 AKS 叢集。 透過 Kubernetes RBAC，您可以建立「角色」來定義權限，然後藉由「角色繫結」將這些角色指派給使用者。

如需詳細資訊，請參閱 [使用 KUBERNETES RBAC 授權][kubernetes-rbac]。

### <a name="roles-and-clusterroles"></a>Roles 和 ClusterRoles

使用 Kubernetes RBAC 將權限指派給使用者之前，您必須先將這些權限定義為「角色」。 Kubernetes 角色會「授與」權限。 *拒絕* 許可權沒有任何概念。

Roles (角色) 會用來授與一個命名空間內的權限。 如果您需要將權限授與整個叢集，或授與指定命名空間外部的叢集資源，可以改用 ClusterRoles。

ClusterRole 會以同樣方式將權限授與資源，但這些權限可以套用至整個叢集中的資源，而不是特定命名空間中的資源。

### <a name="rolebindings-and-clusterrolebindings"></a>RoleBindings 和 ClusterRoleBindings

一旦將角色定義為可授與權限給資源，您就可以透過 RoleBinding 指派這些 Kubernetes RBAC 權限。 如果您的 AKS 叢集 [與 Azure Active Directory (Azure AD) 整合 ](#azure-active-directory-integration)，系結就是這些 Azure AD 使用者授與的許可權，以在叢集中執行動作，請參閱如何 [使用 Kubernetes 角色型存取控制和 Azure Active Directory 身分識別來控制對叢集資源的存取](azure-ad-rbac.md)。

角色繫結會用來為指定命名空間指派角色。 此方法可讓您以邏輯方式區隔單一 AKS 叢集，讓使用者只能存取獲派命名空間中的應用程式資源。 如果您需要將角色繫結到整個叢集，或繫結到指定命名空間外部的叢集資源，可以改用 ClusterRoleBindings。

ClusterRoleBinding 會以同樣方式將角色繫結到使用者，但這些角色可以套用至整個叢集中的資源，而不是特定命名空間中的資源。 此方法可讓您允許系統管理員或支援工程師存取 AKS 叢集中的所有資源。


> [!NOTE]
> Microsoft/AKS 所採取的任何叢集動作都是在內建 Kubernetes 角色 `aks-service` 和內建角色系結的使用者同意下進行 `aks-service-rolebinding` 。 此角色可讓 AKS 對叢集問題進行疑難排解和診斷，但無法修改許可權，也無法建立角色或角色系結，或其他高許可權動作。 只有在具有即時 (JIT) 存取權的主動式支援票證下，才會啟用角色存取。 深入瞭解 [AKS 支援原則](support-policies.md)。


### <a name="kubernetes-service-accounts"></a>Kubernetes 服務帳戶

Kubernetes 的其中一個主要使用者類型是「服務帳戶」。 服務帳戶存在 Kubernetes API 中，並受其管理。 服務帳戶的認證會儲存為 Kubernetes 祕密，如此便可讓已獲授權的 Pod 用這些認證與 API 伺服器進行通訊。 大部分的 API 要求會為服務帳戶或一般使用者帳戶提供驗證權杖。

一般使用者帳戶可讓人為系統管理員或開發人員提供更傳統的存取權，而不只是服務與處理常式。 Kubernetes 本身不會提供儲存一般使用者帳戶和密碼的身分識別管理解決方案。 而是將外部身分識別解決方案整合到 Kubernetes。 對 AKS 叢集而言，此整合的身分識別解決方案就是 Azure Active Directory。

如需有關 Kubernetes 中身分識別選項的詳細資訊，請參閱 [Kubernetes 驗證][kubernetes-authentication]。

## <a name="azure-active-directory-integration"></a>Azure Active Directory 整合

AKS 叢集的安全性可以透過整合的 Azure Active Directory (AD) 來加強。 以數十年企業身分識別管理技術為基礎，Azure AD 是多租用戶雲端式目錄，也是身分識別管理服務，可將核心目錄服務、應用程式存取管理、身分識別保護結合在單一解決方案中。 透過 Azure AD，您可以將內部部署身分識別整合至 AKS 叢集，以對帳戶管理和安全性提供單一來源。

![整合 Azure Active Directory 與 AKS 叢集](media/concepts-identity/aad-integration.png)

透過與 Azure AD 整合的 AKS 叢集，您可以允許使用者或群組存取命名空間內或叢集上的 Kubernetes 資源。 若要取得 `kubectl` 設定內容，使用者可執行 [az aks get-credentials][az-aks-get-credentials] 命令。 當使用者接著與 AKS 叢集互動時 `kubectl` ，系統會提示他們使用 Azure AD 認證登入。 此方法可對使用者帳戶管理和密碼認證提供單一來源。 使用者只能存取叢集系統管理員所定義的資源。

透過 OpenID Connect 對 AKS 叢集提供 Azure AD 驗證。 OpenID Connect 是以 OAuth 2.0 通訊協定為建置基礎的身分識別層。 如需 OpenID Connect 的詳細資訊，請參閱 [OPEN ID Connect 檔][openid-connect]。 從 Kubernetes 叢集內部， [Webhook 權杖驗證][webhook-token-docs] 用來驗證驗證權杖。 Webhook 權杖驗證已設定並當作 AKS 叢集的一部分管理。

### <a name="webhook-and-api-server"></a>Webhook 和 API 伺服器

![Webhook 和 API 伺服器驗證流程](media/concepts-identity/auth-flow.png)

如上圖所示，API 伺服器會呼叫 AKS webhook 伺服器，並執行下列步驟：

1. Kubectl 會使用 Azure AD 用戶端應用程式，透過 [OAuth 2.0 裝置授權授與流程](../active-directory/develop/v2-oauth2-device-code.md)來登入使用者。
2. Azure AD 提供 access_token、id_token 和 refresh_token。
3. 使用者使用來自 kubeconfig 的 access_token 提出要求 kubectl。
4. Kubectl 會將 access_token 傳送至 API 伺服器。
5. API 伺服器是使用 Auth WebHook 伺服器設定來執行驗證。
6. 驗證 webhook 伺服器藉由檢查 Azure AD 公開簽署金鑰來確認 JSON Web 權杖簽章是否有效。
7. 伺服器應用程式會使用使用者提供的認證，從 MS 圖形 API 查詢登入使用者的群組成員資格。
8. 回應會傳送至 API 伺服器，其具有使用者資訊，例如使用者主體名稱 (UPN) 存取權杖的宣告，以及使用者的群組成員資格（以物件識別碼為基礎）。
9. API 會根據 Kubernetes 角色/RoleBinding 來執行授權決策。
10. 授權之後，API 伺服器會將回應傳回給 kubectl。
11. Kubectl 會將意見反應提供給使用者。
 
**[在這裡](managed-aad.md)瞭解如何整合 AKS 與 AAD。**

## <a name="azure-role-based-access-control-azure-rbac"></a>Azure 角色型存取控制 (Azure RBAC)

Azure RBAC 是建置於 [Azure Resource Manager](../azure-resource-manager/management/overview.md) 上的授權系統，可提供更細緻的 Azure 資源存取管理。

 Azure RBAC 是設計用來處理您 Azure 訂用帳戶內的資源，而 Kubernetes RBAC 是設計來處理 AKS 叢集中的 Kubernetes 資源。 

透過 Azure RBAC，您可以建立「角色定義」來概述要套用的權限。 然後，使用者或群組會透過特定 *範圍* 的 *角色指派*（可能是個別資源、資源群組或跨訂用帳戶）指派此角色定義。

如需詳細資訊，請參閱 [什麼是 AZURE RBAC)  (azure 角色型存取控制？][azure-rbac]

完全操作 AKS 叢集所需的存取層級有兩種： 
1. [存取 Azure 訂用帳戶中的 AKS 資源](#azure-rbac-to-authorize-access-to-the-aks-resource)。 此程式可讓您控制使用 AKS Api 來調整或升級叢集的專案，以及提取您的 kubeconfig。
2. 存取 Kubernetes API。 這項存取是透過[KUBERNETES RBAC](#kubernetes-role-based-access-control-kubernetes-rbac) (傳統) 或[整合 AZURE RBAC 與 AKS 來 Kubernetes 授權](#azure-rbac-for-kubernetes-authorization-preview)來控制。

### <a name="azure-rbac-to-authorize-access-to-the-aks-resource"></a>Azure RBAC 可授權存取 AKS 資源

使用 Azure RBAC，您可以提供使用者 (或身分識別) ，以細微存取權跨一或多個訂用帳戶 AKS 資源。 例如，您可能會有 [Azure Kubernetes Service 參與者角色](../role-based-access-control/built-in-roles.md#azure-kubernetes-service-contributor-role) ，可讓您執行調整和升級叢集等動作。 另一位使用者可能會擁有只提供提取系統管理員 kubeconfig 許可權的 [Azure Kubernetes Service 叢集管理員角色](../role-based-access-control/built-in-roles.md#azure-kubernetes-service-cluster-admin-role) 。

或者，您可以為您的使用者提供一般 [參與者](../role-based-access-control/built-in-roles.md#contributor) 角色，此角色會包含上述許可權，以及 AKS 資源上可能的每個動作，但管理許可權本身除外。

深入瞭解如何使用 Azure RBAC 來保護 kubeconfig 檔案的存取權，以存取 [此處](control-kubeconfig-access.md)的 Kubernetes API。

### <a name="azure-rbac-for-kubernetes-authorization-preview"></a>適用于 Kubernetes 授權的 Azure RBAC (預覽版) 

透過 Azure RBAC 整合，AKS 將會使用 Kubernetes 授權 webhook 伺服器，讓您使用 Azure 角色定義和角色指派來管理 Azure AD 整合式 K8s 叢集資源的許可權和指派。

![適用于 Kubernetes 的 Azure RBAC 授權流程](media/concepts-identity/azure-rbac-k8s-authz-flow.png)

如上圖所示，當使用 Azure RBAC 整合時，所有對 Kubernetes API 的要求都會遵循與 [Azure Active integration 一節](#azure-active-directory-integration)中所述的相同驗證流程。 

但在該情況下，您不需要只依賴 Kubernetes RBAC 進行授權，只要在 AAD 中有提出要求的身分識別，就會將要求授與 Azure 授權。 如果身分識別不存在於 AAD 中（例如 Kubernetes 服務帳戶），則 Azure RBAC 將不會啟動，而且它將會是一般 Kubernetes RBAC。

在此案例中，您可以為使用者提供四個內建角色的其中一個，或建立自訂角色，就像使用 Kubernetes 角色一樣，但在此案例中使用 Azure RBAC 機制和 Api。 

例如，這項功能可讓您不只為使用者授與跨訂用帳戶 AKS 資源的許可權，還可以設定並提供他們在每個控制 Kubernetes API 存取權的叢集中所擁有的角色和許可權。 例如，您可以授與 `Azure Kubernetes Service RBAC Viewer` 訂用帳戶範圍的角色，其收件者將能夠列出並取得所有叢集的所有 Kubernetes 物件，但無法加以修改。


#### <a name="built-in-roles"></a>內建角色

AKS 提供下列四個內建角色。 它們類似于 Kubernetes 的 [內建角色](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#user-facing-roles) ，但有一些差異，例如支援 CRDs。 如需每個內建角色所允許之動作的完整清單，請參閱 [這裡](../role-based-access-control/built-in-roles.md)。

| 角色                                | 描述  |
|-------------------------------------|--------------|
| Azure Kubernetes Service RBAC 檢視器  | 允許唯讀存取，以查看命名空間中的大部分物件。 它不允許查看角色或角色系結。 此角色不允許 `Secrets` 進行查看，因為讀取秘密的內容可讓您存取 `ServiceAccount` 命名空間中的認證，以允許 API 存取命名 `ServiceAccount` 空間中的任何憑證 (形式的許可權擴大)   |
| Azure Kubernetes Service RBAC 寫入器 | 允許對命名空間中大部分物件的讀取/寫入存取。 此角色不允許查看或修改角色或角色系結。 不過，此角色可讓您存取和執行 pod `Secrets` 作為命名空間中的任何 ServiceAccount，因此可以用來取得命名空間中任何 ServiceAccount 的 API 存取層級。 |
| Azure Kubernetes Service RBAC 管理員  | 允許系統管理員存取權，可在命名空間中授與。 允許對命名空間中大部分資源的讀取/寫入存取 (或叢集範圍) ，包括在命名空間內建立角色和角色系結的能力。 此角色不允許寫入資源配額或命名空間本身的存取權。 |
| Azure Kubernetes Service RBAC 叢集管理員  | 允許超級使用者存取在任何資源上執行任何動作。 它可讓您完整控制叢集中的每個資源，以及所有命名空間中的資源。 |

**若要瞭解如何啟用 Azure RBAC 以進行 Kubernetes 授權，請 [參閱這裡](manage-azure-rbac.md)。**

## <a name="summary"></a>摘要

下表摘要說明當 Azure AD 整合啟用時，使用者可以向 Kubernetes 進行驗證的方式。  在所有情況下，使用者的命令順序為：
1. 執行 `az login` 以向 Azure 進行驗證。
1. 執行 `az aks get-credentials` 以將叢集的認證下載至 `.kube/config` 。
1. 執行 `kubectl` 命令 (第一個命令可能會觸發以瀏覽器為基礎的驗證，以驗證叢集，如下表所述) 。

第二個數據行中所指的角色授與是 Azure 入口網站中 [ **存取控制** ] 索引標籤上所顯示的 Azure RBAC 角色授與。 叢集系統管理員 Azure AD 群組會顯示在入口網站中的 [ **設定] 索引** 標籤上， (或 `--aad-admin-group-object-ids` Azure CLI) 中的參數名稱。

| 描述        | 需要角色授與| 叢集系統管理員 Azure AD 群組 (s)  | 使用時機 |
| -------------------|------------|----------------------------|-------------|
| 使用用戶端憑證的舊版管理員登入| **Azure Kubernetes 管理員角色**。 此角色可 `az aks get-credentials` 用於 `--admin` 旗標，這會將 [舊版 (非 Azure AD) 叢集系統管理員憑證](control-kubeconfig-access.md) 下載到使用者 `.kube/config` 。 這是「Azure Kubernetes 管理員角色」唯一的用途。|n/a|如果您被永久封鎖，但無法存取可存取叢集的有效 Azure AD 群組。| 
| 具有手動 (叢集) RoleBindings 的 Azure AD| **Azure Kubernetes 使用者角色**。 「使用者」角色允許在 `az aks get-credentials` 沒有旗標的情況下使用 `--admin` 。  (這是「Azure Kubernetes 使用者角色」的唯一用途。 ) 結果，在啟用 Azure AD 的叢集上，會將 [空白專案](control-kubeconfig-access.md) 下載到 `.kube/config` 中，以在第一次使用瀏覽器式驗證時觸發瀏覽器型驗證 `kubectl` 。| 使用者不在這些群組中。 由於使用者不在任何叢集系統管理員群組中，因此其權利將由叢集系統管理員所設定的任何 RoleBindings 或 ClusterRoleBindings 完全控制。  (Cluster) RoleBindings [提名 Azure AD 的使用者或 Azure AD 群組](azure-ad-rbac.md) 作為其 `subjects` 。 如果未設定這類系結，使用者將無法 excute 任何 `kubectl` 命令。|如果您想要更細緻的存取控制，且您未使用 Azure RBAC 進行 Kubernetes 授權。 請注意，設定系結的使用者必須以這個表格中所列的其中一種方法來登入。|
| 依管理群組成員 Azure AD| 同上|使用者是此處所列其中一個群組的成員。 AKS 會自動產生 ClusterRoleBinding，將所有列出的群組系結至 `cluster-admin` Kubernetes 角色。 因此，這些群組中的使用者可以執行所有 `kubectl` 命令 `cluster-admin` 。|如果您想要為使用者提供完整的系統管理員許可權，而 _不_ 是使用 Azure RBAC 進行 Kubernetes 授權，則為。|
| 使用適用于 Kubernetes 授權的 Azure RBAC Azure AD|兩個角色：首先， **Azure Kubernetes 使用者角色** (如上) 所示。 其次，其中一個「Azure Kubernetes Service **RBAC**...」以上列出的角色，或您自己的自訂替代方案。|啟用適用于 Kubernetes 授權的 Azure RBAC 時，[設定] 索引標籤上的 [管理員角色] 欄位是不相關的。|您正在使用 Azure RBAC 進行 Kubernetes 授權。 這種方法可讓您更精細地控制，而不需要設定 RoleBindings 或 ClusterRoleBindings。|

## <a name="next-steps"></a>後續步驟

- 若要開始使用 Azure AD 和 Kubernetes RBAC，請參閱[整合 Azure Active Directory 與 AKS][aks-aad]。
- 如需相關的最佳作法，請參閱 [AKS 中驗證和授權的最佳做法][operator-best-practices-identity]。
- 若要開始使用 Azure RBAC 進行 Kubernetes 授權，請參閱 [使用 AZURE rbac 來授權 Azure Kubernetes Service (AKS) 叢集內的存取權](manage-azure-rbac.md)。
- 若要開始保護您的 kubeconfig 檔，請參閱限制對叢集 [設定檔的存取](control-kubeconfig-access.md)

如需有關 Kubernetes 及 AKS 核心概念的詳細資訊，請參閱下列文章：

- [Kubernetes / AKS 叢集和工作負載][aks-concepts-clusters-workloads]
- [Kubernetes / AKS 安全性][aks-concepts-security]
- [Kubernetes / AKS 虛擬網路][aks-concepts-network]
- [Kubernetes / AKS 儲存體][aks-concepts-storage]
- [Kubernetes / AKS 調整][aks-concepts-scale]

<!-- LINKS - External -->
[kubernetes-authentication]: https://kubernetes.io/docs/reference/access-authn-authz/authentication
[webhook-token-docs]: https://kubernetes.io/docs/reference/access-authn-authz/authentication/#webhook-token-authentication
[kubernetes-rbac]: https://kubernetes.io/docs/reference/access-authn-authz/rbac/

<!-- LINKS - Internal -->
[openid-connect]: ../active-directory/develop/v2-protocols-oidc.md
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[azure-rbac]: ../role-based-access-control/overview.md
[aks-aad]: managed-aad.md
[aks-concepts-clusters-workloads]: concepts-clusters-workloads.md
[aks-concepts-security]: concepts-security.md
[aks-concepts-scale]: concepts-scale.md
[aks-concepts-storage]: concepts-storage.md
[aks-concepts-network]: concepts-network.md
[operator-best-practices-identity]: operator-best-practices-identity.md
[upgrade-per-cluster]: ../azure-monitor/insights/container-insights-update-metrics.md#upgrade-per-cluster-using-azure-cli