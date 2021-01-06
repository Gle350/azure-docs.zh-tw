---
title: 條件式存取-需要適用于系統管理員的 MFA-Azure Active Directory
description: 建立自訂條件式存取原則，以要求系統管理員執行多重要素驗證
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: how-to
ms.date: 08/03/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 57826fcff03e79d5617c7eb69aac7d535d3c86f7
ms.sourcegitcommit: 67b44a02af0c8d615b35ec5e57a29d21419d7668
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97915703"
---
# <a name="conditional-access-require-mfa-for-administrators"></a>條件式存取：系統管理員需要 MFA

被指派系統管理許可權的帳戶會成為攻擊者的目標。 在這些帳戶上 (MFA) 需要多重要素驗證，是降低這些帳戶遭到入侵風險的簡單方法。

Microsoft 建議您至少需要下列角色的 MFA：

* 驗證系統管理員
* 計費管理員
* 條件式存取系統管理員
* Exchange 系統管理員
* 全域管理員
* 服務台管理員
* 密碼管理員
* 特殊權限角色管理員
* 安全性系統管理員
* SharePoint 管理員
* 使用者管理員

組織可以選擇在適當的情況下包含或排除角色。

## <a name="user-exclusions"></a>使用者排除

條件式存取原則是功能強大的工具，建議您從原則中排除下列帳戶：

* **緊急存取** 或 **急用** 帳戶，以避免整個租用戶帳戶遭到鎖定。 雖然不太可能發生，但如果所有管理員都遭到鎖定而無法使用租用戶，緊急存取系統管理帳戶就可以用來登入租用戶，以採取存取權復原步驟。
   * 如需詳細資訊，請參閱[在 Azure AD 中管理緊急存取帳戶](../roles/security-emergency-access.md)一文。
* **服務帳戶** 和 **服務主體**，例如 Azure AD Connect 同步處理帳戶。 服務帳戶是未與任何特定使用者繫結的非互動式帳戶。 後端服務通常會使用這些帳戶，以透過程式設計方式存取應用程式，但也可用來登入系統以便進行管理。 請排除這類服務帳戶，因為您無法透過程式設計方式來完成 MFA。 條件式存取不會封鎖服務主體所進行的呼叫。
   * 如果您的組織在指令碼或程式碼中使用這些帳戶，請考慮將其取代為[受控識別](../managed-identities-azure-resources/overview.md)。 您暫時可以在基準原則中排除這些特定帳戶，來解決此問題。

## <a name="create-a-conditional-access-policy"></a>建立條件式存取原則

下列步驟將協助建立條件式存取原則，以要求指派的系統管理角色執行多重要素驗證。

1. 以全域管理員、安全性系統管理員或條件式存取管理員的身分，登入 **Azure 入口網站**。
1. 瀏覽至 [Azure Active Directory] > [安全性] > [條件式存取]。
1. 選取 [新增原則]。
1. 為您的原則命名。 我們建議組織針對其原則的名稱建立有意義的標準。
1. 在 [指派] 底下，選取 [使用者和群組]
   1. 在 [ **包含**] 下，選取 [ **目錄角色] (預覽)** 並至少選擇下列角色：
      * 驗證系統管理員
      * 計費管理員
      * 條件式存取系統管理員
      * Exchange 系統管理員
      * 全域管理員
      * 服務台管理員
      * 密碼管理員
      * 安全性系統管理員
      * SharePoint 管理員
      * 使用者管理員
   
      > [!WARNING]
      > 條件式存取原則不支援使用者將目錄角色指派給範圍直接指向物件的 [管理單位](../roles/admin-units-assign-roles.md) 或目錄角色，例如透過 [自訂角色](../roles/custom-create.md)。

   1. 在 [排除] 底下選取 [使用者和群組]，然後選擇組織的緊急存取或急用帳戶。 
   1. 選取 [完成] 。
1. 在 [雲端應用程式或動作] > [包含] 下，選取 [所有雲端應用程式]，然後選取 [完成]。
1. 在 **[條件**  >  **用戶端應用程式**] 底下，切換 **設定** 為 **[是]** ，然後在 **[選取要套用此原則的用戶端應用程式] 下，選取 [此原則將會** 保留
1. 在 [存取控制] > [授與] 底下選取 [授與存取權] 和 [需要多重要素驗證]，然後選取 [選取]。
1. 確認您的設定，並將 [啟用原則] 設定為 [開啟]。
1. 選取 [建立] 以建立以啟用您的原則。

## <a name="next-steps"></a>後續步驟

[條件式存取一般原則](concept-conditional-access-policy-common.md)

[使用條件式存取報告專用模式判斷影響](howto-conditional-access-insights-reporting.md)

[使用條件式存取 What If 工具模擬登入行為](troubleshoot-conditional-access-what-if.md)
