---
title: 使用 Azure Migrate 伺服器評量來探索 Hyper-V VM
description: 了解如何使用 Azure Migrate 伺服器評量工具探索內部部署 Hyper-V VM。
author: vineetvikram
ms.author: vivikram
ms.manager: abhemraj
ms.topic: tutorial
ms.date: 09/14/2020
ms.custom: mvc
ms.openlocfilehash: e7b4a1b2e1d737dad0054cbdf08443436ac2c181
ms.sourcegitcommit: e7152996ee917505c7aba707d214b2b520348302
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/20/2020
ms.locfileid: "97705552"
---
# <a name="tutorial-discover-hyper-v-vms-with-server-assessment"></a>教學課程：使用伺服器評量來探索 Hyper-V VM

在遷移至 Azure 的過程中，您會探索內部部署清查和工作負載。 

本教學課程說明如何透過「Azure Migrate：伺服器評量工具」探索內部部署 Hyper-V 虛擬機器 (VM)：(使用輕量型 Azure Migrate 設備)。 您會將設備部署為 Hyper-V VM，以持續探索機器和效能中繼資料。

在本教學課程中，您會了解如何：

> [!div class="checklist"]
> * 設定 Azure 帳戶
> * 準備 Hyper-V 環境以進行探索。
> * 建立 Azure Migrate 專案。
> * 設定 Azure Migrate 設備。
> * 開始連續探索。

> [!NOTE]
> 教學課程顯示嘗試案例的最快路徑，並使用預設選項。  

如果您沒有 Azure 訂用帳戶，請在開始前建立[免費帳戶](https://azure.microsoft.com/pricing/free-trial/)。

## <a name="prerequisites"></a>必要條件

在開始本教學課程之前，請先確認您符合下列必要條件。


**需求** | **詳細資料**
--- | ---
**Hyper-V 主機** | VM 所在的 Hyper-V 主機可以是獨立主機，或位於叢集中。<br/><br/> 主機必須執行 Windows Server 2019、Windows Server 2016 或 Windows Server 2012 R2。<br/><br/> 確認 WinRM 連接埠 5985 上允許輸入連線 (HTTP)，讓設備可使用通用訊息模型 (CIM) 工作階段連線以提取 VM 中繼資料和效能資料。
**設備部署** | Hyper-V 主機需要用來為設備配置 VM 的資源：<br/><br/> - Windows Server 2016<br/><br/> \- 16 GB 的 RAM<br/><br/> - 八個 vCPU<br/><br/> - 約 80 GB 的磁碟儲存體。<br/><br/> - 外部虛擬交換器。<br/><br/> - VM 的網際網路存取 (直接或透過 Proxy)。
**VM** | VM 可在任何 Windows 或 Linux 作業系統中執行。 

開始之前，您可以先[檢閱設備在探索期間收集的資料](migrate-appliance.md#collected-data---hyper-v)。

## <a name="prepare-an-azure-user-account"></a>準備 Azure 使用者帳戶

若要建立 Azure Migrate 專案並註冊 Azure Migrate 設備，您需要具有下列權限的帳戶：
- Azure 訂用帳戶的參與者或擁有者權限。
- 註冊 Azure Active Directory 應用程式的權限。

如果您剛建立免費的 Azure 帳戶，您就是訂用帳戶的擁有者。 如果您不是訂用帳戶擁有者，請與擁有者合作以指派權限，如下所示：


1. 在 Azure 入口網站中搜尋「訂用帳戶」，然後在 [服務] 底下選取 [訂用帳戶]。

    ![搜尋 Azure 訂用帳戶的搜尋方塊](./media/tutorial-discover-hyper-v/search-subscription.png)

2. 在 [訂用帳戶] 頁面中，選取您要在其中建立 Azure Migrate 專案的訂用帳戶。 
3. 在訂用帳戶中，選取 [存取控制 (IAM)] > [檢查存取權]。
4. 在 [檢查存取權] 中，搜尋相關的使用者帳戶。
5. 在 [新增角色指派] 中，按一下 [新增]。

    ![搜尋使用者帳戶以檢查存取權並指派角色](./media/tutorial-discover-hyper-v/azure-account-access.png)

6. 在 [新增角色指派] 中，選取 [參與者] 或 [擁有者] 角色，然後選取帳戶 (在我們的範例中為 azmigrateuser)。 然後按一下 [儲存]  。

    ![開啟 [新增角色指派] 頁面，將角色指派給帳戶](./media/tutorial-discover-hyper-v/assign-role.png)

7. 在入口網站中搜尋使用者，然後在 [服務] 底下選取 [使用者]。
8. 在 [使用者設定] 中，確認 Azure AD 使用者可以註冊應用程式 (預設為 [是])。

    ![在 [使用者設定] 中確認使用者可以註冊 Active Directory 應用程式](./media/tutorial-discover-hyper-v/register-apps.png)

9. 或者，租用戶/全域管理員可為帳戶指派 **應用程式開發人員** 角色，以允許註冊 AAD 應用程式。 [深入了解](../active-directory/fundamentals/active-directory-users-assign-role-azure-portal.md)。

## <a name="prepare-hyper-v-hosts"></a>準備 Hyper-V 主機

在 Hyper-V 主機上設定具有管理員存取權的帳戶。 設備會使用此帳戶進行探索。

- 選項 1：準備對 Hyper-V 機器具有管理員存取權的帳戶。
- 選項 2：準備本機管理員帳戶或網域管理員帳戶，並將帳戶新增至下列群組：遠端管理使用者、Hyper-V 管理員和效能監視器使用者。


## <a name="set-up-a-project"></a>設定專案

設定新的 Azure Migrate 專案。

1. 在 Azure 入口網站 > [所有服務] 中，搜尋 **Azure Migrate**。
2. 在 [服務] 下，選取 [Azure Migrate]。
3. 在 [概觀] 中，選取 [建立專案]。
5. 在 [建立專案] 中，選取您的 Azure 訂用帳戶和資源群組。 如果您還沒有資源群組，請加以建立。
6. 在 [專案詳細資料] 中，指定專案名稱以及要在其中建立專案的地理位置。 請檢閱[公用](migrate-support-matrix.md#supported-geographies-public-cloud)和[政府雲端](migrate-support-matrix.md#supported-geographies-azure-government)支援的地理位置。

   ![專案名稱和區域的方塊](./media/tutorial-discover-hyper-v/new-project.png)

7. 選取 [建立]。
8. 等候幾分鐘讓 Azure Migrate 專案完成部署。

**Azure Migrate：伺服器評量** 工具依預設會新增至新專案中。

![顯示依預設新增伺服器評量工具的頁面](./media/tutorial-discover-hyper-v/added-tool.png)


## <a name="set-up-the-appliance"></a>設定設備

本教學課程會在 Hyper-V VM 上設定設備，如下所示：

- 提供設備名稱，並在入口網站中產生 Azure Migrate 專案金鑰。
- 從 Azure 入口網站下載壓縮的 Hyper-V VHD。
- 建立設備，並確認其可以連線至 Azure Migrate 伺服器評估。
- 進行設備的第一次設定，並使用 Azure Migrate 專案金鑰將其註冊至 Azure Migrate 專案。
> [!NOTE]
> 如果您因故無法使用範本設定設備，可以使用 PowerShell 指令碼進行設定。 [深入了解](deploy-appliance-script.md#set-up-the-appliance-for-hyper-v)。


### <a name="generate-the-azure-migrate-project-key"></a>產生 Azure Migrate 專案金鑰

1. 在 [移轉目標] > [伺服器] >  **[Azure Migrate：伺服器評估]** 中，選取 [探索]。
2. 在 [探索機器] > [機器是否已虛擬化?] 中，選取 [是，使用 Hyper-V]。
3. 在 **1：產生 Azure Migrate 專案金鑰** 中，為您要為探索之 Hyper-V VM 設定的 Azure Migrate 設備命名。名稱應使用英數位元，且長度不超過 14 個字元。
1. 按一下 [產生金鑰] ，開始建立必要的 Azure 資源。 在建立資源期間，請勿關閉探索的電腦頁面。
1. 成功建立 Azure 資源之後，系統會產生 **Azure Migrate 專案金鑰**。
1. 複製金鑰，您在設定期間需要此金鑰才能完成設備的註冊。

### <a name="download-the-vhd"></a>下載 VHD

在 **2：下載 Azure Migrate 設備** 中，選取 .VHD 檔案，然後按一下 [下載]。 


### <a name="verify-security"></a>確認安全性

請先確認 ZIP 檔案安全無虞再進行部署。

1. 在存放下載檔案的目標電腦上，開啟系統管理員命令視窗。

2. 執行下列 PowerShell 命令以產生 ZIP 檔的雜湊
    - ```C:\>Get-FileHash -Path <file_location> -Algorithm [Hashing Algorithm]```
    - 使用方式範例：```C:\>Get-FileHash -Path ./AzureMigrateAppliance_v3.20.09.25.zip -Algorithm SHA256```

3.  確認最新的設備版本與雜湊值：

    - 對於 Azure 公用雲端：

        **案例** | **下載** | **SHA256**
        --- | --- | ---
        Hyper-V (8.91 GB) | [最新版本](https://go.microsoft.com/fwlink/?linkid=2140422) |  40aa037987771794428b1c6ebee2614b092e6d69ac56d48a2bbc75eeef86c99a

    - 對於 Azure Government：

        **案例** _ | _ *下載** | **SHA256**
        --- | --- | ---
        Hyper-V (85.8 MB) | [最新版本](https://go.microsoft.com/fwlink/?linkid=2140424) |  cfed44bb52c9ab3024a628dc7a5d0df8c624f156ec1ecc3507116bae330b257f

### <a name="create-the-appliance-vm"></a>建立設備 VM

匯入所下載的檔案，並建立 VM。

1. 在會裝載設備 VM 的 Hyper-V 主機上，將壓縮的 VHD 檔案解壓縮至其中的資料夾。 會解壓縮出三個資料夾。
2. 開啟 Hyper-V 管理員。 在 [動作] 中，按一下 [匯入虛擬機器]。
2. 在 [匯入虛擬機器精靈] > [開始之前]，按 [下一步]。
3. 在 [尋找資料夾] 中，指定已解壓縮的 VHD 所在的資料夾。 然後按一下 [下一步]  。
1. 在 [選取虛擬機器] 中，按 [下一步]。
2. 在 [選擇匯入類型] 中，按一下 [複製虛擬機器 (建立新的唯一識別碼)]。 然後按一下 [下一步]  。
3. 在 [選擇目的地] 中，保留預設設定。 按 [下一步]  。
4. 在 [儲存資料夾] 中，保留預設設定。 按 [下一步]  。
5. 在 [選擇網路] 中，指定 VM 會使用的虛擬交換器。 此交換器必須能夠連線到網際網路，以將資料傳送至 Azure。
6. 在 [摘要] 中檢閱設定。 然後按一下 [ **完成**]。
7. 在 [Hyper-V 管理員] > [虛擬機器] 中，啟動 VM。


### <a name="verify-appliance-access-to-azure"></a>確認設備是否能存取 Azure

確定設備 VM 可以連線至[公用](migrate-appliance.md#public-cloud-urls)和[政府](migrate-appliance.md#government-cloud-urls)雲端的 Azure URL。

### <a name="configure-the-appliance"></a>設定設備

第一次設定設備。

> [!NOTE]
> 如果您使用 [PowerShell 指令碼](deploy-appliance-script.md)來設定設備 (而非使用下載的 VHD)，則此程序中的前兩個步驟將與之無關。

1. 在 [Hyper-V 管理員] > [虛擬機器] 中，以滑鼠右鍵按一下 [VM] > [連線]。
2. 提供設備的語言、時區和密碼。
3. 在任何可連線至 VM 的機器上開啟瀏覽器，並開啟設備 Web 應用程式的 URL：**https://設備名稱或 IP 位址:44368**。

   或者，您也可以按一下應用程式捷徑，從設備桌面開啟應用程式。
1. 接受 **授權條款**，並閱讀第三方資訊。
1. 在 [Web 應用程式] > [設定必要條件] 中，執行下列動作：
    - **連線能力**：應用程式會確認 VM 是否能夠存取網際網路。 如果 VM 使用 Proxy：
      - 按一下 [設定 Proxy] 以指定 Proxy 位址 (格式為 http://ProxyIPAddress 或 http://ProxyFQDN) ) 和接聽連接埠。
      - 如果 Proxy 需要驗證，請指定認證。
      - 僅支援 HTTP Proxy。
      - 如果您已新增 Proxy 詳細資料，或已停用 Proxy 和/或驗證，請按一下 [儲存] 以再次觸發連線檢查。
    - **時間同步**：系統會確認時間。 設備上的時間應該與網際網路時間同步，VM 探索才能正常運作。
    - **安裝更新**：Azure Migrate Server 評估會檢查設備是否已安裝最新的更新。檢查完成之後，您可以按一下 [檢視設備服務] 以查看設備上執行之元件的狀態和版本。

### <a name="register-the-appliance-with-azure-migrate"></a>向 Azure Migrate 註冊設備

1. 貼上從入口網站複製的 **Azure Migrate 專案金鑰**。 如果沒有金鑰，請移至 **伺服器評估 > 探索 > 管理現有的設備**，選取在金鑰產生時提供的設備名稱，並複製對應的金鑰。
1. 您需要裝置程式碼才能向 Azure 驗證。 按一下 [登入] 會以如下所示的裝置程式碼開啟強制回應。

    ![顯示裝置程式碼的強制回應](./media/tutorial-discover-vmware/device-code.png)

1. 按一下 [複製程式碼和登入] 以複製裝置程式碼，然後在新的瀏覽器索引標籤中開啟 Azure 登入提示。如果未出現，請確定您已在瀏覽器中停用快顯封鎖程式。
1. 在新的索引標籤上，使用您的 Azure 使用者名稱和密碼登入並貼上裝置程式碼。
   
   不支援使用 PIN 登入。
3. 如果不小心在尚未完成登入流程時關閉了登入索引標籤，則需要重新整理設備組態管理員的瀏覽器索引標籤，才能再次啟用登入按鈕。
1. 成功登入之後，請回到設備組態管理員的上一個索引標籤。
4. 如果用於記錄的 Azure 使用者針對在金鑰產生期間建立的 Azure 資源帳戶具有正確的權限，就會起始設備註冊。
1. 成功註冊設備之後，您可以按一下 [檢視詳細資料]查看註冊詳細資料。



### <a name="delegate-credentials-for-smb-vhds"></a>委派 SMB VHD 的認證

如果您要在 SMB 上執行 VHD，就必須將認證從設備委派到 Hyper-V 主機。 若要從設備執行此動作：

1. 在設備 VM 上，執行此命令。 HyperVHost1/HyperVHost2 是主機名稱範例。

    ```
    Enable-WSManCredSSP -Role Client -DelegateComputer HyperVHost1.contoso.com, HyperVHost2.contoso.com, HyperVHost1, HyperVHost2 -Force
    ```

2. 或者，也可以在設備上的本機群組原則編輯器中執行此動作：
    - 在 [本機電腦原則] > [電腦設定] 中，按一下 [系統管理範本] > [系統] > [認證委派]。
    - 按兩下 [允許委派全新認證]，然後選取 [已啟用]。
    - 在 [選項] 中，按一下 [顯示]，然後以 **wsman/** 作為前置詞將您想要探索的每一部 Hyper-V 主機新增至清單中。
    - 在 [認證委派] 中，按兩下 [允許委派僅具有 NTLM 伺服器驗證的全新認證]。 再次以 **wsman/** 作為前置詞將您想要探索的每一部 Hyper-V 主機新增至清單中。

## <a name="start-continuous-discovery"></a>開始連續探索

從設備連線至 Hyper-V 主機或叢集，然後開始探索 VM。

1. 在 [步驟 1：選取復原點]**提供 Hyper-V 主機認證** 中，按一下 [新增認證] 以指定認證的自訂名稱，並新增 Hyper-V 主機/叢集的 **使用者名稱** 和 **密碼**，設備將使用此資訊探索  VM。 按一下 [ **儲存**]。
1. 如果想要一次新增多個認證，請按一下 [新增更多] 以儲存並新增更多認證。 Hyper-V VM 探索支援使用多個認證。
1. 在 **步驟 2：提供 Hyper-V 主機/叢集詳細資料** 中，按一下 [新增探索來源] 來指定 Hyper-V 主機/叢集 **IP 位址/FQDN** 和認證的自訂名稱，以連線到主機/叢集。
1. 您可以一次 **新增單一項目**，或 **新增多個項目**。 另外也可透過 **匯入 CSV** 提供 Hyper-V 主機/叢集詳細資料。


    - 如果您選擇 **新增單一項目**，需要指定認證的自訂名稱和 Hyper-V 主機/叢集的 **IP 位址/FQDN**，然後按一下 [儲存]。
    - 如果您選擇 **新增多個項目**  _(預設選項)_ ，可以在文字方塊中指定Hyper-V 主機/叢集的 **IP 位址/FQDN** 和認證的自訂名稱，一次新增多筆記錄。**確認** 新增的記錄，然後按一下 [儲存]。
    - 如果您選擇 **匯入 CSV**，可以下載 CSV 範本檔案，並在檔案中填入 Hyper-V 主機/叢集的 **IP 位址/FQDN** 和憑證的自訂名稱。 然後將檔案匯入設備，**確認** 檔案中的記錄，然後按一下 [儲存]。

1. 按一下儲存時，設備會嘗試驗證已新增的 Hyper-V 主機/叢集連線，並在資料表中顯示每部主機/叢集的 **驗證狀態**。
    - 對於成功驗證的主機/叢集，您可以按一下其 IP 位址/FQDN 以檢視更多詳細資料。
    - 如果主機驗證失敗，請按一下資料表狀態欄中的 [驗證失敗] 以檢閱錯誤。 修正問題，然後再次驗證。
    - 若要移除主機或叢集，請按一下 [刪除]。
    - 您無法從叢集中移除特定主機。 您只能移除整個叢集。
    - 即使叢集中的特定主機有問題，您還是可以新增叢集。
1. 您可以在啟動探索之前，隨時 **重新驗證** 主機/叢集的連線功能是否正常。
1. 按一下 [開始探索]，以開始探索成功驗證的 VM 主機/叢集。 成功起始探索之後，您可以在資料表中檢查每部主機/叢集的探索狀態。

這會開始探索。 每部主機大約會需要 2 分鐘，才能讓探索到之伺服器的中繼資料出現在 Azure 入口網站中。

## <a name="verify-vms-in-the-portal"></a>在入口網站中確認 VM

探索完成後，便可以確認 VM 是否出現在入口網站中。

1. 開啟 Azure Migrate 儀表板。
2. 在 Azure Migrate - 伺服器 >  **Azure Migrate：伺服器評量** 頁面中，按一下圖示以顯示 探索到的伺服器 計數。

## <a name="next-steps"></a>後續步驟

- [評估 Hyper-V VM](tutorial-assess-hyper-v.md) 以移轉至 Azure VM。
- [檢閱設備在探索期間收集的資料](migrate-appliance.md#collected-data---hyper-v)。