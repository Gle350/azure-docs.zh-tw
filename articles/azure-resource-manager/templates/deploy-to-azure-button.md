---
title: 部署至 Azure 按鈕
description: 使用按鈕，從 GitHub 存放庫部署 Azure Resource Manager 範本。
ms.topic: conceptual
ms.date: 11/10/2020
ms.openlocfilehash: abe59f377474540e9209691df8b1d1a7b806c26d
ms.sourcegitcommit: e46f9981626751f129926a2dae327a729228216e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98028738"
---
# <a name="use-a-deployment-button-to-deploy-templates-from-github-repository"></a>使用部署按鈕從 GitHub 存放庫部署範本

本文說明如何使用 [ **部署至 Azure** ] 按鈕，從 GitHub 存放庫部署範本。 您可以直接將按鈕新增至 GitHub 儲存機制中的 _README.md_ 檔案。 或者，您可以將按鈕加入至參考存放庫的網頁。

部署範圍是由範本架構所決定。 如需詳細資訊，請參閱：

- [資源群組](deploy-to-resource-group.md)
- [訂閱](deploy-to-subscription.md)
- [管理群組](deploy-to-management-group.md)
- [租戶](deploy-to-tenant.md)

## <a name="use-common-image"></a>使用通用映射

若要將按鈕加入至您的網頁或存放庫，請使用下圖：

```markdown
![Deploy to Azure](https://aka.ms/deploytoazurebutton)
```

```html
<img src="https://aka.ms/deploytoazurebutton"/>
```

影像會顯示為：

![部署至 Azure 按鈕](https://aka.ms/deploytoazurebutton)

## <a name="create-url-for-deploying-template"></a>建立用來部署範本的 URL

若要建立範本的 URL，請從您存放庫中範本的原始 URL 開始。 若要查看原始 URL，請選取 [ **原始**]。

:::image type="content" source="./media/deploy-to-azure-button/select-raw.png" alt-text="選取原始":::

URL 的格式為：

```html
https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
```

然後，將 URL 轉換成 URL 編碼的值。 您可以使用線上編碼器或執行命令。 下列 PowerShell 範例示範如何對值進行 URL 編碼。

```powershell
$url = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json"
[uri]::EscapeDataString($url)
```

URL 編碼時，範例 URL 具有下列值。

```html
https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-storage-account-create%2Fazuredeploy.json
```

每個連結的開頭都是相同的基底 URL：

```html
https://portal.azure.com/#create/Microsoft.Template/uri/
```

將 URL 編碼的範本連結新增至基底 URL 的結尾。

```html
https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-storage-account-create%2Fazuredeploy.json
```

您有連結的完整 URL。

一般來說，您會將範本裝載于公用存放庫中。 如果您使用私人存放庫，則必須包含權杖以存取範本的原始內容。 GitHub 產生的權杖只會在短時間後有效。 您需要經常更新連結。

如果您使用 [Git 搭配 Azure Repos](/azure/devops/repos/git/) 而不是使用 GitHub 存放庫，您仍可使用 [ **部署至 Azure** ] 按鈕。 請確定您的存放庫是公用的。 使用 [Items](/rest/api/azure/devops/git/items/get) 作業來取得範本。 您的要求應該採用下列格式：

```http
https://dev.azure.com/{organization-name}/{project-name}/_apis/git/repositories/{repository-name}/items?scopePath={url-encoded-path}&api-version=6.0
```

將此要求 URL 編碼。

## <a name="create-deploy-to-azure-button"></a>[建立部署至 Azure] 按鈕

最後，將連結和影像放在一起。

若要在 GitHub 存放庫或網頁的 _README.md_ 檔案中新增具有 Markdown 的按鈕，請使用：

```markdown
[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-storage-account-create%2Fazuredeploy.json)
```

若是 HTML，請使用：

```html
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-storage-account-create%2Fazuredeploy.json" target="_blank">
  <img src="https://aka.ms/deploytoazurebutton"/>
</a>
```

若為 Git 與 Azure 存放庫，按鈕的格式如下：

```markdown
[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fdev.azure.com%2Forgname%2Fprojectname%2F_apis%2Fgit%2Frepositories%2Freponame%2Fitems%3FscopePath%3D%2freponame%2fazuredeploy.json%26api-version%3D6.0)
```

## <a name="deploy-the-template"></a>部署範本

若要測試完整的解決方案，請選取下列按鈕：

[![部署至 Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-storage-account-create%2Fazuredeploy.json)

入口網站會顯示一個窗格，可讓您輕鬆地提供參數值。 這些參數會預先填入範本中的預設值。

![使用入口網站進行部署](./media/deploy-to-azure-button/portal.png)

## <a name="next-steps"></a>後續步驟

- 若要深入了解範本，請參閱[瞭解 ARM 範本的結構和語法](template-syntax.md)。
