---
title: 從 Azure IoT 中樞裝置布建服務取消註冊裝置
description: '如何取消註冊裝置以防止透過 Azure IoT 中樞的裝置布建服務布建 (DPS) '
author: wesmc7777
ms.author: wesmc
ms.date: 04/05/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.openlocfilehash: c75fcd1fd20e41df5018fcaa07fe83051d7e5f1a
ms.sourcegitcommit: 44844a49afe8ed824a6812346f5bad8bc5455030
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/23/2020
ms.locfileid: "97740363"
---
# <a name="how-to-disenroll-a-device-from-azure-iot-hub-device-provisioning-service"></a>如何從 Azure IoT 中樞裝置佈建服務中取消註冊裝置

對於受到廣泛使用的系統 (例如 IoT 解決方案) 來說，適當地管理裝置認證是非常重要的。 適用於這類系統的最佳做法是清楚地規劃當裝置認證 (不論是共用存取簽章 (SAS) 權杖還是 X.509 憑證) 可能外洩時，如何撤銷裝置的存取權。 

裝置布建服務中的註冊可讓您布 [建](about-iot-dps.md#provisioning-process)裝置。 佈建的裝置是已向 IoT 中樞註冊的裝置，這讓裝置可以接收其最初的[裝置對應項](~/articles/iot-hub/iot-hub-devguide-device-twins.md)狀態，並開始回報遙測資料。 本文會說明如何從佈建服務執行個體中取消註冊裝置，使其之後無法再進行佈建。

> [!NOTE] 
> 針對您要撤銷其存取權的裝置，請注意這些裝置的重試原則。 例如，具有無限重試原則的裝置可能會持續嘗試向佈建服務進行註冊。 該情況會耗用服務資源，而且可能會影響效能。

## <a name="disallow-devices-by-using-an-individual-enrollment-entry"></a>使用個別註冊專案不允許裝置

個別註冊適用于單一裝置，而且可以使用 x.509 憑證、TPM 簽署金鑰 (于實際或虛擬 TPM) 中，或使用 SAS 權杖作為證明機制。 若不允許具有個別註冊的裝置，您可以停用或刪除其註冊專案。 

停用裝置的註冊專案以暫時禁止裝置： 

1. 登入 Azure 入口網站，然後在左側功能表中選取 [所有資源]。
2. 在資源清單中，選取您想要禁止裝置的布建服務。
3. 在您的佈建服務中，選取 [管理註冊]，然後選取 [個別註冊] 索引標籤。
4. 為您想要禁止的裝置選取註冊專案。 

    ![選取個別註冊](./media/how-to-revoke-device-access-portal/select-individual-enrollment.png)

5. 在註冊頁面上，捲動到底部，並在 [啟用項目] 切換開關選取 [停用]，然後選取 [儲存]。  

   ![在入口網站中停用個別註冊項目](./media/how-to-revoke-device-access-portal/disable-individual-enrollment.png)

若要藉由刪除裝置的註冊專案來永久禁止裝置：

1. 登入 Azure 入口網站，然後在左側功能表中選取 [所有資源]。
2. 在資源清單中，選取您想要禁止裝置的布建服務。
3. 在您的佈建服務中，選取 [管理註冊]，然後選取 [個別註冊] 索引標籤。
4. 針對您想要禁止的裝置，選取其註冊專案旁的核取方塊。 
5. 選取視窗頂端的 [刪除]，然後選取 [是] 以確認您要移除該註冊。 

   ![在入口網站中刪除個別註冊項目](./media/how-to-revoke-device-access-portal/delete-individual-enrollment.png)


完成此程序之後，您應該會看到您的項目已從個別註冊清單中移除。  

## <a name="disallow-an-x509-intermediate-or-root-ca-certificate-by-using-an-enrollment-group"></a>不允許使用註冊群組來 x.509 中繼或根 CA 憑證

X.509 憑證通常會配置在信任的信任鏈結中。 如果憑證在鏈結中的任何階段發生外洩，信任就會遭到破壞。 憑證必須是不允許的，以防止裝置布建服務在包含該憑證的任何鏈中布建裝置。 若要深入了解 X.509 憑證和其與佈建服務搭配使用的方式，請參閱 [X.509 憑證](./concepts-x509-attestation.md#x509-certificates)。 

註冊群組是裝置的項目，這些裝置會共用由相同中繼或根 CA 所簽署之 X.509 憑證的通用證明機制。 註冊群組項目是使用與中繼或根 CA 關聯的 X.509 憑證所設定。 此項目的設定中也會包含其憑證鏈結中有該憑證之裝置所共用的任何設定值 (例如對應項狀態和 IoT 中樞連線)。 若不允許憑證，您可以停用或刪除其註冊群組。

若要停用憑證的註冊群組以暫時禁止該憑證： 

1. 登入 Azure 入口網站，然後在左側功能表中選取 [所有資源]。
2. 在資源清單中，選取您要禁止簽署憑證的布建服務。
3. 在佈建服務中，選取 [管理註冊]，然後選取 [註冊群組] 索引標籤。
4. 使用您想要禁止的憑證來選取註冊群組。
5. 選取 [啟用項目] 切換開關上的 [停用]，然後選取 [儲存]。  

   ![在入口網站中停用註冊群組項目](./media/how-to-revoke-device-access-portal/disable-enrollment-group.png)

    
若要藉由刪除憑證的註冊群組來永久禁止憑證：

1. 登入 Azure 入口網站，然後在左側功能表中選取 [所有資源]。
2. 在資源清單中，選取您想要禁止裝置的布建服務。
3. 在佈建服務中，選取 [管理註冊]，然後選取 [註冊群組] 索引標籤。
4. 針對您想要禁止的憑證，選取註冊群組旁邊的核取方塊。 
5. 選取視窗頂端的 [刪除]，然後選取 [是] 以確認您要移除該註冊群組。 

   ![在入口網站中刪除註冊群組項目](./media/how-to-revoke-device-access-portal/delete-enrollment-group.png)

完成此程序之後，您應該會看到您的項目已從註冊群組清單中移除。  

> [!NOTE]
> 如果您刪除某個憑證的註冊群組，其憑證鏈結中有該憑證的裝置可能仍可進行註冊，前提是根憑證或另一個在其憑證鏈結中更上層之中繼憑證的已啟用註冊群組存在。

## <a name="disallow-specific-devices-in-an-enrollment-group"></a>不允許註冊群組中的特定裝置

實作 X.509 證明機制的裝置會使用裝置的信任鏈結和私密金鑰來進行驗證。 當裝置連線並向「裝置佈建服務」驗證時，服務會先尋找符合裝置認證的個別註冊。 接著，服務才會搜尋註冊群組，以判斷是否可以佈建該裝置。 如果服務找到裝置的已停用個別註冊，就會防止該裝置進行連線。 即使裝置憑證鏈結中之中繼或根 CA 的已啟用註冊群組存在，服務也會防止連線。 

若不允許註冊群組中的個別裝置，請遵循下列步驟：

1. 登入 Azure 入口網站，然後在左側功能表中選取 [所有資源]。
2. 從資源清單中，選取包含您想要禁止之裝置註冊群組的布建服務。
3. 在您的佈建服務中，選取 [管理註冊]，然後選取 [個別註冊] 索引標籤。
4. 選取頂端的 [新增個別註冊] 按鈕。 
5. 在 [新增註冊] 頁面上，選取 [X.509] 作為裝置的證明 **機制**。

    上傳裝置憑證，並輸入不允許之裝置的裝置識別碼。 針對憑證，請使用裝置上所安裝的已簽署終端實體憑證。 裝置使用已簽署的終端實體憑證進行驗證。

    ![設定不允許裝置的裝置屬性](./media/how-to-revoke-device-access-portal/disable-individual-enrollment-in-enrollment-group-1.png)

6. 捲動到 [新增註冊] 頁面底部，並在 [啟用項目] 切換開關選取 [停用]，然後選取 [儲存]。 

    [![在入口網站中使用已停用的個別註冊專案，從群組註冊停用裝置](./media/how-to-revoke-device-access-portal/disable-individual-enrollment-in-enrollment-group.png)](./media/how-to-revoke-device-access-portal/disable-individual-enrollment-in-enrollment-group.png#lightbox)

成功建立註冊之後，您應該會看到已停用的裝置註冊列在 [個別註冊] 索引標籤上。 

## <a name="next-steps"></a>後續步驟

取消註冊也屬於較大型的取消佈建程序。 取消佈建裝置包括從佈建服務取消註冊，以及從 IoT 中樞取消登錄。 若要深入了解完整程序，請參閱[如何取消佈建先前自動佈建的裝置](how-to-unprovision-devices.md)
