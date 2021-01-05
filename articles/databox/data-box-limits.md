---
title: Azure 資料箱限制 | Microsoft Docs
description: 描述 Microsoft Azure 資料箱元件和連線的系統限制和建議大小。
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 01/05/2021
ms.author: alkohli
ms.openlocfilehash: 97d8da86565db73aa9a3866f39f793aaf0905470
ms.sourcegitcommit: 5e762a9d26e179d14eb19a28872fb673bf306fa7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/05/2021
ms.locfileid: "97900156"
---
# <a name="azure-data-box-limits"></a>Azure 資料箱限制

當您部署和操作 Microsoft Azure 資料箱時，請考慮這些限制。 下表描述資料箱的這些限制。

## <a name="data-box-service-limits"></a>資料箱服務限制

[!INCLUDE [data-box-service-limits](../../includes/data-box-service-limits.md)]

## <a name="data-box-limits"></a>資料箱限制

- 資料箱最多可以儲存500000000個檔案，以進行匯入和匯出。
- 資料箱支援最多512個容器或雲端中的共用。 使用者共用內的最上層目錄會成為雲端中的容器或 Azure 檔案共用。 
- 由於 ReFS 元資料空間耗用量，資料箱使用量容量可能小於 80 TB。
- 資料箱一次最多支援10個用戶端連接。

## <a name="azure-storage-limits"></a>Azure 儲存體限制

[!INCLUDE [data-box-storage-limits](../../includes/data-box-storage-limits.md)]

## <a name="data-upload-caveats"></a>資料上傳注意事項


### <a name="for-import-order"></a>針對匯入順序

匯入順序的資料箱警告包括：

[!INCLUDE [data-box-data-upload-caveats](../../includes/data-box-data-upload-caveats.md)]

### <a name="for-export-order"></a>針對匯出順序

匯出順序的資料箱警告包括：

- 資料箱是以 Windows 為基礎的裝置，不支援區分大小寫的檔案名。 例如，在 Azure 中，您可能會有兩個不同的檔案，且名稱在大小寫上是不同的。 請勿使用資料箱來匯出這類檔案，因為裝置上將會覆寫檔案。
- 如果您在輸入檔中有重複的標記，或是參考相同資料的標記，則資料箱匯出可能會略過或覆寫檔案。 Azure 入口網站顯示的檔案數目和資料大小，可能會與裝置上實際的資料大小不同。 
- 資料箱會透過 SMB 將資料匯出至以 Windows 為基礎的系統，並且受限於檔案和資料夾的 SMB 限制。 不會匯出具有不支援之名稱的檔案和資料夾。
- 從前置詞到容器有1:1 的對應。
- 檔案名的大小上限為1024個字元。 不會匯出超過此長度的檔案名。
- *Xml* 檔案中的重複前置詞 (在建立順序時上傳) 會匯出。 不會忽略重複的首碼。
- 分頁 blob 和容器名稱會區分大小寫。 如果大小寫不相符，就會找不到 blob 和/或容器。
 

## <a name="azure-storage-account-size-limits"></a>Azure 儲存體帳戶大小限制

[!INCLUDE [data-box-storage-account-size-limits](../../includes/data-box-storage-account-size-limits.md)]

## <a name="azure-object-size-limits"></a>Azure 物件大小限制

[!INCLUDE [data-box-object-size-limits](../../includes/data-box-object-size-limits.md)]

## <a name="azure-block-blob-page-blob-and-file-naming-conventions"></a>Azure 區塊 Blob、分頁 Blob 和檔案命名慣例

[!INCLUDE [data-box-naming-conventions](../../includes/data-box-naming-conventions.md)]
