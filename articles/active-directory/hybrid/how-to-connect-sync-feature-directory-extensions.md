---
title: Azure AD Connect 同步：目錄擴充 | Microsoft Docs
description: 本主題說明 Azure AD Connect 中的目錄擴充功能。
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 995ee876-4415-4bb0-a258-cca3cbb02193
ms.service: active-directory
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/12/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 25d4152783129fa1c5950d6cf6287332bf90d32a
ms.sourcegitcommit: 8f0803d3336d8c47654e119f1edd747180fe67aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/07/2021
ms.locfileid: "97976872"
---
# <a name="azure-ad-connect-sync-directory-extensions"></a>Azure AD Connect 同步處理：目錄擴充
您可以使用目錄擴充功能，從內部部署 Active Directory 利用自己的屬性擴充 Azure Active Directory (Azure AD) 中的結構描述。 此功能可讓您建置 LOB 應用程式，方法是取用您在內部部署中持續進行管理的屬性。 這些屬性可以透過 [擴充](/graph/extensibility-overview
)功能來取用。 您可以使用 [Microsoft Graph Explorer](https://developer.microsoft.com/graph/graph-explorer)來查看可用的屬性。 您也可以使用這項功能，在 Azure AD 中建立動態群組。

目前，沒有 Microsoft 365 的工作負載會使用這些屬性。

## <a name="customize-which-attributes-to-synchronize-with-azure-ad"></a>自訂要與 Azure AD 同步處理的屬性

您可以在安裝精靈的自訂設定路徑中，設定您想要同步處理的其他屬性。

> [!NOTE]
> 在 Azure AD Connect 1.2.65.0 之前的版本中， **可用屬性** 的 [搜尋] 方塊會區分大小寫。

![結構描述擴充功能精靈](./media/how-to-connect-sync-feature-directory-extensions/extension2.png)  

 安裝會顯示下列屬性，這些都是有效的候選項目：

* 使用者和群組物件類型
* 單一值屬性︰字串、布林值、整數、二進位檔
* 多值屬性︰字串、二進位檔


>[!NOTE]
> 在 Azure AD Connect 同步處理多重值 Active Directory 屬性 Azure AD 為多重值屬性擴充功能之後，就可以將屬性包含在 SAML 宣告中。 但是，您無法透過 API 呼叫來使用此資料。

屬性清單是從 Azure AD Connect 安裝期間建立的結構描述快取讀取。 如果您已使用其他屬性擴充 Active Directory 結構描述，就必須[重新整理結構描述](how-to-connect-installation-wizard.md#refresh-directory-schema)，才可看見這些新屬性。

Azure AD 中的物件最多可有 100 個目錄擴充功能的屬性。 長度上限是 250 個字元。 如果屬性值較長，同步處理引擎就會加以截斷。

## <a name="configuration-changes-in-azure-ad-made-by-the-wizard"></a>由 wizard 進行的 Azure AD 設定變更

在 Azure AD Connect 安裝期間，會註冊可提供這些屬性的應用程式。 您可以在 Azure 入口網站中看到這個應用程式。 它的名稱一律是 **Tenant Schema Extension App**。

![結構描述擴充功能應用程式](./media/how-to-connect-sync-feature-directory-extensions/extension3new.png)

請確定選取 **所有應用程式** 以查看此應用程式。

屬性的前面會加 **上 \_ {ApplicationId} \_**。 ApplicationId 的 Azure AD 租使用者中的所有屬性都有相同的值。 本主題的所有其他案例都需要此值。

## <a name="viewing-attributes-using-the-microsoft-graph-api"></a>使用 Microsoft Graph API 來查看屬性

您現在可以使用 [Microsoft Graph Explorer](https://developer.microsoft.com/graph/graph-explorer#)，透過 Microsoft Graph API 取得這些屬性。

>[!NOTE]
> 在 Microsoft Graph API 中，您必須要求傳回屬性。 明確地選取屬性，如下所示： `https://graph.microsoft.com/beta/users/abbie.spencer@fabrikamonline.com?$select=extension_9d98ed114c4840d298fad781915f27e4_employeeID,extension_9d98ed114c4840d298fad781915f27e4_division` 。
>
> 如需詳細資訊，請參閱 [Microsoft Graph：使用查詢參數](/graph/query-parameters#select-parameter)。

>[!NOTE]
> 不支援將屬性值從 AADConnect 同步至不是由 AADConnect 建立的延伸模組屬性。 這樣做可能會產生效能問題和非預期的結果。 同步處理只支援以上所示的擴充屬性建立。

## <a name="use-the-attributes-in-dynamic-groups"></a>使用動態群組中的屬性

其中一個比較實用的案例是在動態安全性或 Microsoft 365 群組中使用這些屬性。

1. 在 Azure AD 中建立新的群組。 請為它提供一個良好的名稱，並確定 **成員資格類型** 為 **動態使用者**。

   ![具有新群組的螢幕擷取畫面](./media/how-to-connect-sync-feature-directory-extensions/dynamicgroup1.png)

2. 選取以 **新增動態查詢**。 如果您查看屬性，就不會看到這些擴充屬性。 您必須先新增這些專案。 按一下 [ **取得自訂延伸模組屬性**]，輸入應用程式識別碼，然後按一下 [重新整理 **屬性**]。

   ![已新增目錄擴充功能的螢幕擷取畫面](./media/how-to-connect-sync-feature-directory-extensions/dynamicgroup2.png) 

3. 開啟屬性下拉式清單，並注意您加入的屬性現在是可見的。

   ![UI 中顯示新屬性的螢幕擷取畫面](./media/how-to-connect-sync-feature-directory-extensions/dynamicgroup3.png)

   完成運算式以符合您的需求。 在我們的範例中，規則設定為 **(user.extension_9d98ed114c4840d298fad781915f27e4_division-eq "Sales and marketing" )**。

4. 建立群組之後，請 Azure AD 一些時間來填入成員，然後再檢查成員。

   ![動態群組中具有成員的螢幕擷取畫面](./media/how-to-connect-sync-feature-directory-extensions/dynamicgroup4.png)  

## <a name="next-steps"></a>後續步驟
深入了解 [Azure AD Connect 同步](how-to-connect-sync-whatis.md) 組態。

深入了解 [整合內部部署身分識別與 Azure Active Directory](whatis-hybrid-identity.md)。
