---
title: 'Azure 數位 Twins 要求失敗，狀態： 403 (禁止) '
description: 「服務要求失敗的原因和解決方式。 狀態： Azure 數位 Twins 上的 403 (禁止的) 」。
ms.service: digital-twins
author: baanders
ms.author: baanders
ms.topic: troubleshooting
ms.date: 7/20/2020
ms.openlocfilehash: 1517c066fe20d478094f57d85d6e27f355a93601
ms.sourcegitcommit: 8dd8d2caeb38236f79fe5bfc6909cb1a8b609f4a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98049808"
---
# <a name="service-request-failed-status-403-forbidden"></a>服務要求失敗。 狀態： 403 (禁止的) 

本文說明從服務要求至 Azure 數位 Twins 收到403錯誤的原因和解決步驟。 

## <a name="symptoms"></a>徵兆

這項錯誤可能會發生在許多需要驗證的服務要求類型上。 效果是 API 要求失敗，並傳回錯誤狀態 `403 (Forbidden)` 。

## <a name="causes"></a>原因

### <a name="cause-1"></a>原因 #1

最常見的情況是，此錯誤表示您的 Azure 角色型存取控制 (Azure RBAC) 服務的許可權未正確設定。 Azure 數位 Twins 實例的許多動作都要求您在 **嘗試管理的實例上** 擁有 *Azure 數位 Twins 資料擁有* 者角色。 

[!INCLUDE [digital-twins-role-rename-note.md](../../includes/digital-twins-role-rename-note.md)]

### <a name="cause-2"></a>原因 #2

如果您使用用戶端應用程式與向 [應用程式註冊](how-to-create-app-registration.md)進行驗證的 Azure 數位 Twins 進行通訊，則會發生此錯誤，因為您的應用程式註冊沒有針對 Azure 數位 Twins 服務設定的許可權。

應用程式註冊必須已設定 Azure 數位 Twins Api 的存取權限。 然後，當您的用戶端應用程式向應用程式註冊進行驗證時，就會被授與應用程式註冊所設定的許可權。

## <a name="solutions"></a>方案

### <a name="solution-1"></a>方案 #1

第一個解決方案是確認您的 Azure 使用者在您嘗試管理的實例上具有 _**Azure 數位 Twins 資料擁有**_ 者角色。 如果您沒有此角色，請進行設定。

請注意，此角色不同于 .。。
* 此角色先前的名稱在預覽期間， *Azure 數位 Twins 擁有者 (預覽版)* (角色相同，但名稱已變更) 
* 整個 Azure 訂用帳戶的 *擁有* 者角色。 *Azure 數位 Twins 資料擁有* 者是 Azure 數位 Twins 內的角色，其範圍為此個別 Azure 數位 Twins 實例。
* Azure 數位 Twins 中的 *擁有* 者角色。 這些是兩個不同的 Azure 數位 Twins 管理角色，而 *Azure 數位 Twins 資料擁有* 者是應該用於管理的角色。

#### <a name="check-current-setup"></a>檢查目前的設定

[!INCLUDE [digital-twins-setup-verify-role-assignment.md](../../includes/digital-twins-setup-verify-role-assignment.md)]

#### <a name="fix-issues"></a>修正問題 

如果您沒有此角色指派，則您的 **azure 訂** 用帳戶中具有「擁有者」角色的使用者應該執行下列命令，為您的 azure 使用者提供 Azure **數位 Twins 實例** 上的 *azure 數位 Twins 資料擁有* 者角色。 

如果您是訂用帳戶的擁有者，您可以自行執行此命令。 如果不是，請洽詢擁有者，以代表您執行此命令。

```azurecli-interactive
az dt role-assignment create --dt-name <your-Azure-Digital-Twins-instance> --assignee "<your-Azure-AD-email>" --role "Azure Digital Twins Data Owner"
```

如需有關此角色需求和指派程式的詳細資訊，請參閱 how *to：設定實例和驗證 (CLI 或入口網站)* 的「 [*設定使用者的存取權限*」一節](how-to-set-up-instance-CLI.md#set-up-user-access-permissions)。

如果您已有此角色指派 *，且* 您使用 Azure AD 應用程式註冊來驗證用戶端應用程式，如果此解決方案未解決403問題，您可以繼續進行下一個解決方案。

### <a name="solution-2"></a>方案 #2

如果您使用 Azure AD 應用程式註冊來驗證用戶端應用程式，則第二個可能的解決方案是確認應用程式註冊有針對 Azure 數位 Twins 服務設定的許可權。 如果未設定這些設定，請進行設定。

#### <a name="check-current-setup"></a>檢查目前的設定

若要檢查是否已正確設定許可權，請流覽至 Azure 入口網站中的 [Azure AD 應用程式註冊總覽頁面](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) 。 您可以藉由在入口網站的搜尋列中搜尋 *應用程式註冊* ，自行前往此頁面。

切換至 [ *所有應用程式* ] 索引標籤，以查看您的訂用帳戶中已建立的所有應用程式註冊。

您應該會在清單中看到您剛才建立的應用程式註冊。 請選取它以開啟其詳細資料。

:::image type="content" source="media/troubleshoot-error-403/app-registrations.png" alt-text="Azure 入口網站中的應用程式註冊頁面":::

首先，請確認已在註冊時正確設定 Azure 數位 Twins 許可權設定。 若要這樣做，請從功能表列選取 [ *資訊清單* ]，以查看應用程式註冊的資訊清單程式碼。 滾動至程式碼視窗底部，然後在底下尋找這些欄位 `requiredResourceAccess` 。 這些值應該符合以下螢幕擷取畫面中的值：

:::image type="content" source="media/troubleshoot-error-403/verify-manifest.png" alt-text="Azure AD 應用程式註冊的資訊清單入口網站視圖":::

接下來，從功能表列選取 [ *API 許可權* ]，以確認此應用程式註冊包含 Azure 數位 Twins 的讀取/寫入權限。 您應該會看到如下所示的專案：

:::image type="content" source="media/troubleshoot-error-403/verify-api-permissions.png" alt-text="入口網站的 Azure AD 應用程式註冊 API 許可權的入口網站視圖，顯示 Azure 數位 Twins 的「讀取/寫入存取權」":::

#### <a name="fix-issues"></a>修正問題

如果其中任何一項出現的方式不同于所述，請依照如何 [*：建立應用程式註冊*](how-to-create-app-registration.md)中的如何設定應用程式註冊的指示進行。

## <a name="next-steps"></a>下一步

請參閱建立和驗證新的 Azure 數位 Twins 實例的設定步驟：
* [*如何： (CLI 設定實例和驗證)*](how-to-set-up-instance-cli.md)

深入瞭解 Azure 數位 Twins 的安全性和許可權：
* [*概念： Azure 數位 Twins 解決方案的安全性*](concepts-security.md)
