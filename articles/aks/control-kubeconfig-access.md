---
title: 限制 Azure Kubernetes Service (AKS) 中的 kubeconfig 存取權
description: 了解如何控制叢集管理員和叢集使用者對 Kubernetes 組態檔 (kubeconfig) 的存取
services: container-service
ms.topic: article
ms.date: 05/06/2020
ms.openlocfilehash: 77b9988557106ef460d3b222ef85eb29e08f31c8
ms.sourcegitcommit: b6267bc931ef1a4bd33d67ba76895e14b9d0c661
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/19/2020
ms.locfileid: "97693995"
---
# <a name="use-azure-role-based-access-control-to-define-access-to-the-kubernetes-configuration-file-in-azure-kubernetes-service-aks"></a>使用 Azure 角色型存取控制來定義 Azure Kubernetes Service (AKS 中 Kubernetes 設定檔的存取權) 

您可以使用 `kubectl` 工具與 Kubernetes 叢集互動。 Azure CLI 可讓您使用 `kubectl` 輕鬆地存取連線到 AKS 叢集所需的認證和組態資訊。 若要限制誰可以取得該 Kubernetes 設定 (*kubeconfig*) 資訊，以及限制他們所擁有的許可權，您可以使用 azure 角色型存取控制 (azure RBAC) 。

本文說明如何指派 Azure 角色，以限制誰可以取得 AKS 叢集的設定資訊。

## <a name="before-you-begin"></a>開始之前

此文章假設您目前具有 AKS 叢集。 如果您需要 AKS 叢集，請參閱[使用 Azure CLI][aks-quickstart-cli] 或[使用 Azure 入口網站][aks-quickstart-portal]的 AKS 快速入門。

本文也會要求您執行 Azure CLI 2.0.65 版版或更新版本。 執行 `az --version` 以尋找版本。 如果您需要安裝或升級，請參閱[安裝 Azure CLI][azure-cli-install]。

## <a name="available-cluster-roles-permissions"></a>可用的叢集角色權限

當您使用 `kubectl` 工具與 AKS 叢集互動時，組態檔會用來定義叢集連線資訊。 此設定檔通常會儲存在 *~/.kube/config* 中。您可以在此 *kubeconfig* 檔中定義多個叢集。 您可以使用 [kubectl config use-context][kubectl-config-use-context] 命令在叢集間進行切換。

[az aks get-credentials][az-aks-get-credentials] 命令可讓您取得 AKS 叢集的存取認證，並將其合併至 kubeconfig 檔案。 您可以使用 Azure 角色型存取控制 (Azure RBAC) 來控制這些認證的存取權。 這些 Azure 角色可讓您定義誰可以取得 *kubeconfig* 檔案，以及它們在叢集中的許可權。

兩個內建角色如下：

* **Azure Kubernetes Service 叢集管理員角色**  
  * 允許存取 Microsoft.ContainerService/managedClusters/listClusterAdminCredential/action API 呼叫。 此 API 呼叫會[列出叢集管理員認證][api-cluster-admin]。
  * 下載 *clusterAdmin* 角色的 kubeconfig。
* **Azure Kubernetes Service 叢集使用者角色**
  * 允許存取 Microsoft.ContainerService/managedClusters/listClusterUserCredential/action API 呼叫。 此 API 呼叫會[列出叢集使用者認證][api-cluster-user]。
  * 下載 clusterUser 角色的 kubeconfig。

這些 Azure 角色可套用至 Azure Active Directory (AD) 使用者或群組。

> [!NOTE]
> 在使用 Azure AD 的叢集上，具有 *clusterUser* 角色的使用者會有空白 *kubeconfig* 檔案，以提示登入。 登入之後，使用者會根據其 Azure AD 使用者或群組設定來存取。 具有 *clusterAdmin* 角色的使用者具有系統管理員存取權。
>
> 不使用 Azure AD 的叢集只會使用 *clusterAdmin* 角色。

## <a name="assign-role-permissions-to-a-user-or-group"></a>將角色許可權指派給使用者或群組

若要指派其中一個可用角色，您需要取得 AKS 叢集的資源識別碼，以及 Azure AD 使用者帳戶或群組的識別碼。 下列命令範例：

* 針對 *myResourceGroup* 資源群組中名為 *myAKSCluster* 的叢集，使用 [az aks SHOW][az-aks-show]命令取得叢集資源識別碼。 如有需要，請提供您自己的叢集和資源群組名稱。
* 使用 [az account show][az-account-show] 和 [az ad user show][az-ad-user-show] 命令來取得您的使用者識別碼。
* 最後，使用 [az role assign create][az-role-assignment-create] 命令指派角色。

下列範例會將 *Azure Kubernetes Service Cluster 管理員角色* 指派給個別使用者帳戶：

```azurecli-interactive
# Get the resource ID of your AKS cluster
AKS_CLUSTER=$(az aks show --resource-group myResourceGroup --name myAKSCluster --query id -o tsv)

# Get the account credentials for the logged in user
ACCOUNT_UPN=$(az account show --query user.name -o tsv)
ACCOUNT_ID=$(az ad user show --id $ACCOUNT_UPN --query objectId -o tsv)

# Assign the 'Cluster Admin' role to the user
az role assignment create \
    --assignee $ACCOUNT_ID \
    --scope $AKS_CLUSTER \
    --role "Azure Kubernetes Service Cluster Admin Role"
```

> [!IMPORTANT]
> 在某些情況下，帳戶中的 *user.name* 與 *userPrincipalName* 不同，例如 Azure AD 來賓使用者：
>
> ```output
> $ az account show --query user.name -o tsv
> user@contoso.com
> $ az ad user list --query "[?contains(otherMails,'user@contoso.com')].{UPN:userPrincipalName}" -o tsv
> user_contoso.com#EXT#@contoso.onmicrosoft.com
> ```
>
> 在此情況下，請將 *ACCOUNT_UPN* 的值設定為 Azure AD 使用者的 *userPrincipalName* 。 例如，如果您的帳戶 *user.name* 是 *使用者 \@ contoso.com*：
> 
> ```azurecli-interactive
> ACCOUNT_UPN=$(az ad user list --query "[?contains(otherMails,'user@contoso.com')].{UPN:userPrincipalName}" -o tsv)
> ```

> [!TIP]
> 如果您想要將許可權指派給 Azure AD 群組，請 `--assignee` 使用 *群組* 的物件識別碼（而非 *使用者*）來更新上述範例中所示的參數。 若要取得群組的物件識別碼，請使用 [az ad group show][az-ad-group-show] 命令。 下列範例會取得名為 *appdev* 之 Azure AD 群組的物件識別碼： `az ad group show --group appdev --query objectId -o tsv`

如有需要，您可以將上述指派變更為「叢集使用者角色」。

下列範例輸出顯示已成功建立角色指派：

```
{
  "canDelegate": null,
  "id": "/subscriptions/<guid>/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myAKSCluster/providers/Microsoft.Authorization/roleAssignments/b2712174-5a41-4ecb-82c5-12b8ad43d4fb",
  "name": "b2712174-5a41-4ecb-82c5-12b8ad43d4fb",
  "principalId": "946016dd-9362-4183-b17d-4c416d1f8f61",
  "resourceGroup": "myResourceGroup",
  "roleDefinitionId": "/subscriptions/<guid>/providers/Microsoft.Authorization/roleDefinitions/0ab01a8-8aac-4efd-b8c2-3ee1fb270be8",
  "scope": "/subscriptions/<guid>/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myAKSCluster",
  "type": "Microsoft.Authorization/roleAssignments"
}
```

## <a name="get-and-verify-the-configuration-information"></a>取得及驗證組態資訊

在指派 Azure 角色的情況下，使用 [az aks 取得認證][az-aks-get-credentials] 命令來取得 aks 叢集的 *kubeconfig* 定義。 下列範例會取得 --admin 認證，如果使用者已獲派「叢集管理員角色」，則認證可正常運作：

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster --admin
```

接著，您可以使用 [kubectl config view][kubectl-config-view] 命令，確認叢集的「內容」會顯示已套用管理員組態資訊：

```
$ kubectl config view

apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://myaksclust-myresourcegroup-19da35-4839be06.hcp.eastus.azmk8s.io:443
  name: myAKSCluster
contexts:
- context:
    cluster: myAKSCluster
    user: clusterAdmin_myResourceGroup_myAKSCluster
  name: myAKSCluster-admin
current-context: myAKSCluster-admin
kind: Config
preferences: {}
users:
- name: clusterAdmin_myResourceGroup_myAKSCluster
  user:
    client-certificate-data: REDACTED
    client-key-data: REDACTED
    token: e9f2f819a4496538b02cefff94e61d35
```

## <a name="remove-role-permissions"></a>移除角色權限

若要移除角色指派，請使用 [az role assignment delete][az-role-assignment-delete] 命令。 指定帳戶識別碼和叢集資源識別碼，如先前的命令所取得。 如果您將角色指派給群組而不是使用者，請指定適當的群組物件識別碼，而非參數的帳戶物件識別碼 `--assignee` ：

```azurecli-interactive
az role assignment delete --assignee $ACCOUNT_ID --scope $AKS_CLUSTER
```

## <a name="next-steps"></a>後續步驟

若要加強 AKS 叢集存取的安全性，請[整合 Azure Active Directory 驗證][aad-integration]。

<!-- LINKS - external -->
[kubectl-config-use-context]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#config
[kubectl-config-view]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#config

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[azure-rbac]: ../role-based-access-control/overview.md
[api-cluster-admin]: /rest/api/aks/managedclusters/listclusteradmincredentials
[api-cluster-user]: /rest/api/aks/managedclusters/listclusterusercredentials
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-account-show]: /cli/azure/account#az-account-show
[az-ad-user-show]: /cli/azure/ad/user#az-ad-user-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-role-assignment-delete]: /cli/azure/role/assignment#az-role-assignment-delete
[aad-integration]: ./azure-ad-integration-cli.md
[az-ad-group-show]: /cli/azure/ad/group#az-ad-group-show
