---
title: Azure Cosmos DB 中的資料庫交易和開放式並行存取控制
description: 本文描述 Azure Cosmos DB 中的資料庫交易和開放式並行存取控制
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 12/04/2019
ms.reviewer: sngun
ms.openlocfilehash: 96652b2a1eb35668bd8a810b309ab31cec5afdb7
ms.sourcegitcommit: 9514d24118135b6f753d8fc312f4b702a2957780
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/07/2021
ms.locfileid: "97967220"
---
# <a name="transactions-and-optimistic-concurrency-control"></a>交易和開放式並行存取控制
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

資料庫交易提供安全且可預測的程式設計模型來處理並行的資料變更。 傳統的關係資料庫，例如 SQL Server，可讓您使用預存程式和/或觸發程式撰寫商務邏輯，並將它傳送至伺服器，以便直接在資料庫引擎內執行。 使用傳統的關係資料庫時，您必須處理兩種不同的程式設計語言， (非交易式) 應用程式程式設計語言（例如 JavaScript、Python、c #、JAVA 等）和交易式程式設計語言 (例如資料庫原本執行的 T-sql) 。

Azure Cosmos DB 中的資料庫引擎支援與 ACID (不可部分完成性、一致性、隔離性、耐用性) 完全相容的交易與快照集隔離。 容器的 [邏輯分割](partitioning-overview.md) 區範圍內的所有資料庫作業，都是在資料庫引擎內以交易方式執行，該資料分割是由資料分割的複本所裝載。 這些作業包括寫入 (更新邏輯分割區內的一或多個項目) 和讀取作業。 下表說明不同的作業和交易類型：

| **運算**  | **作業類型** | **單一或多項目交易** |
|---------|---------|---------|
| 插入 (不含觸發程序前/觸發程序後) | 寫入 | 單一項目交易 |
| 插入 (含觸發程序前/觸發程序後) | 寫入和讀取 | 多項目交易 |
| 取代 (不含觸發程序前/觸發程序後) | 寫入 | 單一項目交易 |
| 取代 (含觸發程序前/觸發程序後) | 寫入和讀取 | 多項目交易 |
| 更新插入 (不含觸發程序前/觸發程序後) | 寫入 | 單一項目交易 |
| 更新插入 (含觸發程序前/觸發程序後) | 寫入和讀取 | 多項目交易 |
| 刪除 (不含觸發程序前/觸發程序後) | 寫入 | 單一項目交易 |
| 刪除 (含觸發程序前/觸發程序後) | 寫入和讀取 | 多項目交易 |
| 執行預存程序 | 寫入和讀取 | 多項目交易 |
| 系統已啟動執行合併程序 | 寫入 | 多項目交易 |
| 系統已啟動執行根據項目的到期 (TTL) 刪除項目 | 寫入 | 多項目交易 |
| 讀取 | 讀取 | 單一項目交易 |
| 變更摘要 | 讀取 | 多項目交易 |
| 編頁報表 | 讀取 | 多項目交易 |
| 編頁報表 | 讀取 | 多項目交易 |
| 在執行分頁查詢時執行 UDF | 讀取 | 多項目交易 |

## <a name="multi-item-transactions"></a>多項目交易

Azure Cosmos DB 可讓您以 JavaScript 撰寫 [預存程式、前置/後置觸發程式、使用者定義函數 (udf) ](stored-procedures-triggers-udfs.md) 以及合併程式。 Azure Cosmos DB 原生支援在其資料庫引擎內部執行 JavaScript。 您可以在容器中註冊預存程序、觸發程序前/觸發程序後、使用者定義函式 (UDF) 和合併程序，並在之後於 Azure Cosmos 資料庫引擎內以交易方式執行它們。 以 JavaScript 撰寫應用程式邏輯可允許在資料庫交易內，以自然方式表達控制流程、變數範圍、指派，以及整合例外狀況處理基本項目。

JavaScript 型預存程序、觸發程序、UDF，以及合併程序，都包裝在環境 ACID 交易內，且快照集會在邏輯分割區內跨所有項目隔離。 在執行期間，如果 JavaScript 程式擲回例外狀況，會中止整個交易並回復。 產生的程式設計模型非常簡單，但功能強大。 JavaScript 開發人員會取得「持續性」程式設計模型，同時仍然使用其熟悉的語言建構和程式庫基本。

直接在資料庫引擎內執行 JavaScript 的能力，可對容器的項目提供資料庫作業的具效能和交易式執行。 此外，由於 Azure Cosmos 資料庫引擎原生支援 JSON 和 JavaScript，因此應用程式與資料庫的類型系統之間沒有阻抗不相符的問題。

## <a name="optimistic-concurrency-control"></a>樂觀並行控制

開放式並行存取控制可讓您避免遺失更新和刪除。 並行、衝突的作業會受制於擁有項目之邏輯分割區所裝載的資料庫引擎所受到的一般封閉式鎖定限制。 當兩個並行作業嘗試更新邏輯分割區內某個項目的最新版本時，它們其中之一將會成功，另一個則會失敗。 但是，如果嘗試並行更新相同項目的一或兩個作業之前已讀取該項目的較舊版本時，資料庫不會知道兩個衝突作業的其中之一或兩者之前所讀取的值是否確實為項目的最新值。 幸運的是，這種情況可以使用 **開放式並行存取控制來 (OCC)** ，然後讓這兩個作業進入 database engine 內的交易界限。 OCC 會保護您的資料，避免意外遭到其他人覆寫。 它也能防止其他人不小心覆寫您自己所做的變更。

Azure Cosmos DB 的通訊通訊協定層項目的並行更新受制於 OCC。 針對針對 **單一區域寫入** 設定的 Azure Cosmos 帳戶，Azure Cosmos DB 可確保您要更新之專案的用戶端版本 (或刪除) 與 Azure Cosmos 容器中專案的版本相同。 這可確保您的寫入受到保護，而不會被他人的寫入不慎覆寫，反之亦然。 在多使用者環境中，開放式並行存取控制可防止您不小心刪除項目，或更新項目的錯誤版本。 因此，專案會受到保護，以防止惡名昭彰「遺失更新」或「遺失刪除」問題。

在使用 **多重區域寫入** 設定的 Azure Cosmos 帳戶中，如果資料 `_etag` 符合本機區域中的資料，就可以獨立地將資料認可到次要區域。 一旦新的資料在次要區域的本機認可之後，就會合並到中樞或主要區域中。 如果衝突解決原則將新的資料合併到中樞區域，則會使用新的來全域複寫此資料 `_etag` 。 如果衝突解決原則拒絕新的資料，次要區域將會回復成原始資料和 `_etag` 。

Azure Cosmos 容器中儲存的每個項目都有系統定義的 `_etag` 屬性。 `_etag` 的值會自動產生，並在項目每次更新時，由伺服器更新。 `_etag` 可與用戶端提供的 `if-match` 要求標頭搭配使用，以允許伺服器決定是否可有條件地更新專案。 標頭的值 `if-match` 符合伺服器的值 `_etag` ，然後更新專案。 如果 `if-match` 要求標頭的值不再是最新的值，伺服器會拒絕具有「HTTP 412 前置條件失敗」回應訊息的操作。 然後，用戶端可以重新提取專案，以取得伺服器上目前版本的專案，或使用它自己的專案值來覆寫伺服器中的專案版本 `_etag` 。 此外， `_etag` 也可以搭配 `if-none-match` 標頭使用，以判斷是否需要資源的重新擷取。

`_etag`每次更新專案時，專案的值都會變更。 針對取代專案作業， `if-match` 必須明確表示為要求選項的一部分。 如需範例，請參閱 [GitHub](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ItemManagement/Program.cs#L676-L772) 中的範例程式碼。 `_etag` 系統會針對預存程式觸及的所有寫入專案，以隱含的方式檢查值。 如果偵測到任何衝突，預存程式將會回復交易並擲回例外狀況。 透過此方法，在預存程序內的所有寫入都會自動套用，或全部不會自動套用。 這是應用程式重新套用更新並重試原始用戶端要求的信號。

## <a name="next-steps"></a>後續步驟

深入了解下列文章中描述的資料庫交易和開放式並行存取控制：

- [使用 Azure Cosmos 資料庫、容器和項目](account-databases-containers-items.md)
- [一致性層級](consistency-levels.md)
- [衝突類型和解決原則](conflict-resolution-policies.md)
- [使用 TransactionalBatch](transactional-batch.md)
- [預存程序、觸發程序和使用者定義函式](stored-procedures-triggers-udfs.md)
