---
title: 針對已加入舊版混合式 Azure Active Directory 的裝置進行疑難排解
description: 針對已加入混合式 Azure Active Directory 的下層裝置進行疑難排解。
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: troubleshooting
ms.date: 11/21/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jairoc
ms.collection: M365-identity-device-management
ms.openlocfilehash: 057ff064264485a9aea6fc2b31fe57ce37c805ce
ms.sourcegitcommit: d7d5f0da1dda786bda0260cf43bd4716e5bda08b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/05/2021
ms.locfileid: "97895609"
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-down-level-devices"></a>針對已加入混合式 Azure Active Directory 的下層裝置進行疑難排解 

本文章僅適用於下列裝置： 

- Windows 7 
- Windows 8.1 
- Windows Server 2008 R2 
- Windows Server 2012 
- Windows Server 2012 R2 

對於 Windows 10 或 Windows Server 2016，請參閱[針對已加入混合式 Azure Active Directory 的 Windows 10 和 Windows Server 2016 裝置進行疑難排解](troubleshoot-hybrid-join-windows-current.md)。

本文章假設您[設定已加入混合式 Azure Active Directory 的裝置](hybrid-azuread-join-plan.md)來支援下列案例：

- 以裝置為基礎的條件式存取

本文章提供有關如何解決潛在問題的疑難排解指引。  

**您應該知道的事項：** 

- 舊版 Windows 裝置的混合式 Azure AD 加入運作方式與在 Windows 10 中的運作方式略有不同。 許多客戶並不了解他們需要設定的是 AD FS (適用於同盟網域) 還是「無縫 SSO」(適用於受控網域)。
- 針對具有同盟網域的客戶，如果「服務連接點」(SCP) 已設定為指向受控網域名稱 (例如 contoso.onmicrosoft.com，而不是 contoso.com)，則舊版 Windows 裝置的混合式 Azure AD 加入將無法運作。
- 當多位網域使用者登入已加入舊版混合式 Azure AD 的裝置時，相同的實體裝置會在 Azure AD 中出現多次。  舉例來說：如果 *jdoe* 和 *jharnett* 登入裝置，在 [使用者] 資訊索引標籤中就會為每位使用者建立個別的註冊 (DeviceID)。 
- 在使用者資訊索引標籤上，也可能因重新安裝作業系統或手動重新註冊，而導致一個裝置有多個項目。
- 您可以將裝置的初始註冊/加入設定為在登入或鎖定/解除鎖定時嘗試執行。 工作排程器工作可能會觸發 5 分鐘的延遲。 
- 如果是 Windows 7 SP1 或 Windows Server 2008 R2 SP1，請確定已安裝 [KB4284842](https://support.microsoft.com/help/4284842)。 此更新可避免未來的驗證因客戶在變更密碼之後遺失存取受保護金鑰的權限而失敗。
- 混合式 Azure AD 聯結可能會在使用者的 UPN 變更之後失敗，並中斷無縫 SSO 驗證程式。 在聯結程式進行期間，您可能會看到它仍將舊的 UPN 傳送給 Azure AD，除非已清除瀏覽器會話 cookie 或使用者明確登出，並移除舊的 UPN。

## <a name="step-1-retrieve-the-registration-status"></a>步驟 1：擷取註冊狀態 

**確認註冊狀態：**  

1. 使用已執行混合式 Azure AD 加入的使用者帳戶登入。
1. 開啟命令提示字元 
1. 輸入 `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe" /i`

此命令會顯示一個對話方塊，當中會提供有關加入狀態的詳細資料。

:::image type="content" source="./media/troubleshoot-hybrid-join-windows-legacy/01.png" alt-text="[Workplace Join for Windows] 對話方塊的螢幕擷取畫面。包含電子郵件地址的文字，指出特定裝置已加入工作場所。" border="false":::

## <a name="step-2-evaluate-the-hybrid-azure-ad-join-status"></a>步驟 2：評估混合式 Azure AD 加入狀態 

如果裝置未加入混合式 Azure AD，您可以按一下 [加入] 按鈕來嘗試執行混合式 Azure AD 加入。 如果執行混合式 Azure AD 加入的嘗試失敗，將會顯示有關該失敗的詳細資料。

**最常見的問題包括：**

- AD FS 或 Azure AD 設定不正確或網路問題

    :::image type="content" source="./media/troubleshoot-hybrid-join-windows-legacy/02.png" alt-text="[Workplace Join for Windows] 對話方塊的螢幕擷取畫面。文字報告帳戶驗證期間發生錯誤。" border="false":::
    
   - Autoworkplace.exe 無法以無訊息方式使用 Azure AD 或 AD FS 進行驗證。 造成此情況的原因可能是 AD FS (適用於同盟網域) 遺漏或設定不正確、「Azure AD 無縫單一登入」(適用於受控網域) 遺漏或設定不正確，或網路問題。 
   - 這可能是因為使用者已啟用/設定了多重要素驗證 (MFA) ，而且未在 AD FS 伺服器設定 WIAORMULTIAUTHN。 
   - 另一個可能原因是主領域探索 (HRD) 頁面正在等候使用者互動，而導致 **autoworkplace.exe** 無法以無訊息方式要求權杖。
   - 可能是用戶端上 IE 的內部網路區域中遺漏 AD FS 和 Azure AD URL。
   - 網路連線能力問題可能會導致 **autoworkplace.exe** 無法連線至 AD FS 或 Azure AD URL。 
   - **Autoworkplace.exe** 需要用戶端直接從用戶端到組織的內部部署 AD 網域控制站，這表示只有當用戶端連線到組織的內部網路時，混合式 Azure AD 加入才會成功。
   - 貴組織使用的是 Azure AD 無縫單一登入，`https://autologon.microsoftazuread-sso.com` 或 `https://aadg.windows.net.nsatc.net` 不在裝置的 IE 內部網路設定中，並且未針對內部網路區域啟用 [允許透過指令碼更新至狀態列]。
- 您不是以網域使用者身分登入

   :::image type="content" source="./media/troubleshoot-hybrid-join-windows-legacy/03.png" alt-text="[Workplace Join for Windows] 對話方塊的螢幕擷取畫面。文字報告帳戶驗證期間發生錯誤。" border="false":::

   這種情況可能是由幾種不同的原因所致︰

   - 登入的使用者不是網域使用者 (例如本機使用者)。 對下層裝置的混合式 Azure AD 聯結僅支援網域使用者。
   - 用戶端無法連線至網域控制站。    
- 已達到配額限制

    :::image type="content" source="./media/troubleshoot-hybrid-join-windows-legacy/04.png" alt-text="[Workplace Join for Windows] 對話方塊的螢幕擷取畫面。文字會回報錯誤，因為使用者已達到已加入裝置的最大數目。" border="false":::

- 服務沒有回應 

    :::image type="content" source="./media/troubleshoot-hybrid-join-windows-legacy/05.png" alt-text="[Workplace Join for Windows] 對話方塊的螢幕擷取畫面。文字報告因為伺服器沒有回應而發生錯誤。" border="false":::

您也可以在事件記錄檔的 [應用程式及服務記錄檔] > [Microsoft-Workplace Join] 底下找到狀態資訊
  
**混合式 Azure AD 加入失敗的最常見原因包括：** 

- 您的電腦未連線至組織的內部網路，或不在可連線到內部部署 AD 網域控制站的 VPN 上。
- 您是使用本機電腦帳戶來登入您的電腦。 
- 服務組態問題： 
   - AD FS 伺服器未設定為支援 **WIAORMULTIAUTHN**。 
   - 您電腦的樹系沒有「服務連接點」物件指向 Azure AD 中已驗證的網域名稱 
   - 或者，如果您的網域是受控網域，則是未設定「無縫 SSO」或其無法運作。
   - 使用者已達到裝置限制。 

## <a name="next-steps"></a>後續步驟

如有問題，請參閱[裝置管理常見問題集](faq.md)  
