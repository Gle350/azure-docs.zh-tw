---
title: 驗證和授權
description: 瞭解 Azure App Service 和 Azure Functions 內建的驗證和授權支援，以及它如何協助保護您的應用程式免于未經授權的存取。
ms.assetid: b7151b57-09e5-4c77-a10c-375a262f17e5
ms.topic: article
ms.date: 07/08/2020
ms.reviewer: mahender
ms.custom: seodec18, fasttrack-edit, has-adal-ref
ms.openlocfilehash: 1b95b1e96dc26fb72338518fc969c69b035d5f68
ms.sourcegitcommit: 5db975ced62cd095be587d99da01949222fc69a3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97095231"
---
# <a name="authentication-and-authorization-in-azure-app-service-and-azure-functions"></a>Azure App Service 和 Azure Functions 中的驗證和授權

Azure App Service 提供內建的驗證和授權支援，因此您在 Web 應用程式、RESTful API 和行動裝置後端以及 [Azure Functions](../azure-functions/functions-overview.md) 中幾乎不需要寫入或完全無需寫入程式碼，即可登入使用者及存取資料。 本文說明 App Service 如何協助您簡化應用程式的驗證和授權。

若要有安全的驗證和授權，必須對安全性有深入了解，包括同盟、加密、[JSON Web 權杖 (JWT)](https://wikipedia.org/wiki/JSON_Web_Token) 管理、[授與類型](https://oauth.net/2/grant-types/)等等。 App Service 會提供這些公用程式，以便您可以將更多的時間和精力花在為客戶提供商務價值上。

> [!IMPORTANT]
> 您不需要使用這項功能來進行驗證和授權。 您可以在您選擇的 web 架構中使用配套的安全性功能，也可以撰寫自己的公用程式。 不過，請記住， [Chrome 80 會對 SameSite for cookie 進行重大變更](https://www.chromestatus.com/feature/5088147346030592) ， (發行日期大約是2020年3月) ，而自訂遠端驗證或其他依賴跨網站 cookie 張貼的案例可能會在用戶端 Chrome 瀏覽器更新時中斷。 因應措施很複雜，因為它需要針對不同的瀏覽器支援不同的 SameSite 行為。 
>
> App Service 所裝載的 ASP.NET Core 2.1 和更新版本已針對此重大變更進行修補，並且適當地處理 Chrome 80 和較舊的瀏覽器。 此外，ASP.NET Framework 4.7.2 的相同修補程式已部署在2020年1月的 App Service 實例上。 如需詳細資訊，請參閱 [Azure App Service SameSite cookie 更新](https://azure.microsoft.com/updates/app-service-samesite-cookie-update/)。
>

> [!NOTE]
> 驗證/授權功能有時也稱為「簡單驗證」。

> [!NOTE]
> 啟用此功能將會自動將 **所有** 不安全的 HTTP 要求重新導向至 HTTPs，不論 App Service 的設定設定是否 [強制使用 HTTPs](configure-ssl-bindings.md#enforce-https)。 如有需要，您可以透過 `requireHttps` [驗證設定設定檔](app-service-authentication-how-to.md#configuration-file-reference)中的設定來停用此功能，但您必須小心確保不會透過非安全的 HTTP 連線來傳輸任何安全性權杖。

如需原生行動應用程式的專屬資訊，請參閱 [Azure App Service 的行動應用程式使用者驗證和授權](/previous-versions/azure/app-service-mobile/app-service-mobile-auth)。

## <a name="how-it-works"></a>運作方式

### <a name="on-windows"></a>在 Windows 上

驗證和授權模組會在與應用程式程式碼相同的沙箱中執行。 此模組啟用時，每個連入的 HTTP 要求會先通過此模組，再由應用程式程式碼處理。

![此架構圖表顯示在網站沙箱中，與識別提供者互動的進程所攔截到的要求，然後允許流量傳送至部署的網站](media/app-service-authentication-overview/architecture.png)

此模組會處理應用程式的幾件事：

- 向指定提供者驗證使用者
- 驗證、儲存及重新整理權杖
- 管理已驗證的工作階段
- 將身分識別資訊插入要求標頭中

此模組會與應用程式程式碼分開執行，並且會使用應用程式設定加以設定。 不需要任何 SDK、特定語言或對應用程式程式碼進行任何變更。 

### <a name="on-containers"></a>在容器上

驗證和授權模組會在個別的容器中執行，並與您的應用程式程式碼隔離。 使用所謂的 [大使模式](/azure/architecture/patterns/ambassador)，它會與連入流量互動，以在 Windows 上執行類似的功能。 因為它不是以同進程執行，所以無法與特定語言架構進行直接整合;不過，您的應用程式所需的相關資訊會透過使用要求標頭來傳遞，如下所述。

### <a name="userapplication-claims"></a>使用者/應用程式宣告

針對所有語言架構，App Service 會將內送權杖中的宣告 (，使其從已驗證的使用者或用戶端應用程式中，藉由將其插入要求標頭中，來) 可供您的程式碼使用。 在 ASP.NET 4.6 應用程式中，App Service 會使用已驗證的使用者宣告填入 [ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current)，因此您可以遵循標準的 .NET 程式碼模式，包括 `[Authorize]` 屬性。 同樣地，在 PHP 應用程式中，App Service 會填入 `_SERVER['REMOTE_USER']` 變數。 對於 JAVA 應用程式， [可從 Tomcat servlet 存取](configure-language-java.md#authenticate-users-easy-auth)宣告。

若為 [Azure Functions](../azure-functions/functions-overview.md)， `ClaimsPrincipal.Current` 則不會針對 .net 程式碼填入，但您仍然可以在要求標頭中找到使用者宣告，或 `ClaimsPrincipal` 從要求內容或甚至透過系結參數取得物件。 如需詳細資訊，請參閱 [使用用戶端](../azure-functions/functions-bindings-http-webhook-trigger.md#working-with-client-identities) 身分識別。

如需詳細資訊，請參閱[存取使用者宣告](app-service-authentication-how-to.md#access-user-claims)。

> [!NOTE]
> 目前，ASP.NET Core 目前不支援使用驗證/授權功能來擴展目前的使用者。 不過，有一些 [協力廠商的開放原始碼中介軟體元件](https://github.com/MaximRouiller/MaximeRouiller.Azure.AppService.EasyAuth) ，可協助填滿此間隙。
>

### <a name="token-store"></a>權杖存放區

App Service 會提供內建的權杖存放區，也就是與 Web 應用程式、API 或原生行動應用程式相關聯的權杖存放庫。 當您使用任何提供者啟用驗證時，此權杖存放區就會立即供應用程式使用。 如果應用程式程式碼需要代表使用者存取這些提供者的資料，例如： 

- 張貼至已驗證使用者的 Facebook 時間軸
- 使用 Microsoft Graph API 來讀取使用者的公司資料

您通常必須撰寫程式碼，才能在應用程式中收集、儲存及重新整理這些權杖。 使用權杖存放區時，您只有在需要權杖時才會[取出權杖](app-service-authentication-how-to.md#retrieve-tokens-in-app-code)，並在權杖失效時才會[告知 App Service 加以重新整理](app-service-authentication-how-to.md#refresh-identity-provider-tokens)。 

識別碼權杖、存取權杖和重新整理權杖會針對已驗證的會話進行快取，而且只能由相關聯的使用者存取。  

如果您不需要在應用程式中使用權杖，可以在應用程式的 [ **驗證/授權** ] 頁面中停用權杖存放區。

### <a name="logging-and-tracing"></a>記錄和追蹤

如果您[啟用應用程式記錄](troubleshoot-diagnostic-logs.md)，則會直接在記錄檔中看到驗證和授權追蹤。 如果您看到未預期的驗證錯誤，您可以查看現有的應用程式記錄檔，以輕鬆地找到所有詳細資料。 如果您啟用[失敗要求追蹤](troubleshoot-diagnostic-logs.md)，就可以看到驗證和授權模組可能在失敗要求中所扮演的確切角色。 在追蹤記錄中，尋找名為 `EasyAuthModule_32/64` 的模組參考。 

## <a name="identity-providers"></a>識別提供者

App Service 使用[同盟身分識別](https://en.wikipedia.org/wiki/Federated_identity)，由第三方識別提供者為您管理使用者身分識別和驗證流程。 預設可用的識別提供者有五個： 

| 提供者 | 登入端點 |
| - | - |
| [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) | `/.auth/login/aad` |
| [Microsoft 帳戶](../active-directory/develop/v2-overview.md) | `/.auth/login/microsoftaccount` |
| [Facebook](https://developers.facebook.com/docs/facebook-login) | `/.auth/login/facebook` |
| [Google](https://developers.google.com/identity/choose-auth) | `/.auth/login/google` |
| [Twitter](https://developer.twitter.com/en/docs/basics/authentication) | `/.auth/login/twitter` |
| 任何 [OpenID Connect](https://openid.net/connect/) 提供者 (預覽)  | `/.auth/login/<providerName>` |

當您利用上述其中一個提供者啟用驗證和授權時，其登入端點即可用來驗證使用者，以及用來驗證提供者的驗證權杖。 您可以輕鬆地為使用者提供任何數目的上述登入選項。

有 [舊版][custom-auth] 的擴充性路徑可與其他身分識別提供者或自訂驗證解決方案整合，但不建議這麼做。 相反地，請考慮使用 OpenID Connect 支援。

## <a name="authentication-flow"></a>驗證流程

所有提供者的驗證流程皆相同，但會根據您是否要使用提供者的 SDK 登入而有所不同：

- 不使用提供者 SDK：應用程式會將同盟登入委派給 App Service。 瀏覽器應用程式通常是這種情況，可以向使用者顯示提供者的登入頁面。 伺服器程式碼會管理登入程序，因此也稱為「伺服器導向流程」或「伺服器流程」。 此案例適用於瀏覽器應用程式。 它也適用於使用 Mobile Apps 用戶端 SDK 將使用者登入的原生應用程式，因為 SDK 會開啟 Web 檢視，使用 App Service 驗證將使用者登入。 
- 使用提供者 SDK：應用程式會以手動方式將使用者登入提供者，然後將驗證權杖提交給 App Service 進行驗證。 無瀏覽器應用程式通常是這種情況，無法向使用者顯示提供者的登入頁面。 應用程式程式碼會管理登入程序，因此也稱為「用戶端導向流程」或「用戶端流程」。 此案例適用於 REST API、[Azure Functions](../azure-functions/functions-overview.md)、JavaScript 瀏覽器用戶端，以及在登入程序中需要更多彈性的瀏覽器應用程式。 它也適用於使用提供者 SDK 將使用者登入的原生行動應用程式。

> [!NOTE]
> 從 App Service 中受信任的瀏覽器應用程式呼叫 App Service 或 [Azure Functions](../azure-functions/functions-overview.md) 中的另一個 REST API，可以使用伺服器導向的流程進行驗證。 如需詳細資訊，請參閱[自訂 App Service 中的驗證與授權](app-service-authentication-how-to.md)。
>

下表顯示驗證流程的步驟。

| 步驟 | 不使用提供者 SDK | 使用提供者 SDK |
| - | - | - |
| 1. 登入使用者 | 將用戶端重新導向至 `/.auth/login/<provider>`。 | 用戶端程式碼會直接使用提供者的 SDK 將使用者登入，並接收驗證權杖。 如需詳細資訊，請參閱提供者的文件。 |
| 2. 驗證後 | 提供者會將用戶端重新導向至 `/.auth/login/<provider>/callback`。 | 用戶端程式代碼會將 [來自提供者的 token 張貼](app-service-authentication-how-to.md#validate-tokens-from-providers) 至以 `/.auth/login/<provider>` 進行驗證。 |
| 3. 建立已驗證的會話 | App Service 會將已驗證的 Cookie 新增至回應。 | App Service 會將自己的驗證權杖傳回至用戶端程式碼。 |
| 4. 為已驗證的內容提供服務 | 用戶端會在後續要求中包含驗證 Cookie (瀏覽器會自動處理)。 | 用戶端程式碼會在 `X-ZUMO-AUTH` 標頭中顯示驗證權杖 (Mobile Apps 用戶端 SDK 會自動處理)。 |

對於用戶端瀏覽器，App Service 可以自動將所有未經驗證的使用者導向至 `/.auth/login/<provider>`。 您也可以向使用者顯示一或多個 `/.auth/login/<provider>` 連結，讓其使用選擇的提供者登入您的應用程式。

<a name="authorization"></a>

## <a name="authorization-behavior"></a>授權行為

在 [Azure 入口網站](https://portal.azure.com)中，當連入要求未通過驗證時，您可以使用數個行為來設定 App Service 授權。

![顯示「當要求未經驗證時所採取的動作」下拉式清單的螢幕擷取畫面](media/app-service-authentication-overview/authorization-flow.png)

下列標題會說明可用選項。

### <a name="allow-anonymous-requests-no-action"></a>允許匿名要求 (沒有動作) 

此選項會將未經驗證的流量授權延遲至您的應用程式程式碼。 對於已驗證的要求，App Service 也會在 HTTP 標頭中一起傳送驗證資訊。 

此選項會提供更大的彈性來處理匿名要求。 例如，它可讓您向使用者[顯示多個登入提供者](app-service-authentication-how-to.md#use-multiple-sign-in-providers)。 不過，您必須撰寫程式碼。 

### <a name="allow-only-authenticated-requests"></a>僅允許已驗證的要求

此選項會 **使用 \<provider> 登入**。 App Service 會將所有匿名要求重新導向至您所選提供者的 `/.auth/login/<provider>`。 如果匿名要求來自原生行動應用程式，則傳回的回應是 `HTTP 401 Unauthorized`。

使用此選項時，您不需要在應用程式中撰寫任何驗證程式碼。 您可以藉由檢查使用者的宣告來處理更精細的授權 (例如特定角色授權，請參閱[存取使用者宣告](app-service-authentication-how-to.md#access-user-claims))。

> [!CAUTION]
> 以這種方式限制存取會套用至對您應用程式的所有呼叫，這對需要公開可用的首頁的應用程式可能不需要，如同許多單頁應用程式一樣。

> [!NOTE]
> 根據預設，Azure AD 租使用者中的任何使用者都可以從 Azure AD 要求您應用程式的權杖。 如果您想要將應用程式的存取許可權制為一組已定義的使用者，您可以 [在 Azure AD 中設定應用程式](../active-directory/develop/howto-restrict-your-app-to-a-set-of-users.md) 。

## <a name="more-resources"></a>其他資源

* [教學課程：在存取 Azure 儲存體和 Microsoft Graph 的 web 應用程式中驗證和授權使用者](scenario-secure-app-authentication-app-service.md)
* [教學課程：在 Azure App Service 中端對端驗證和授權使用者 (Windows)](tutorial-auth-aad.md)  
* [教學課程：在適用於 Linux 的 Azure App Service 中端對端驗證和授權使用者](./tutorial-auth-aad.md?pivots=platform-linux%3fpivots%3dplatform-linux)  
* [自訂 App Service 中的驗證與授權](app-service-authentication-how-to.md)
* [Azure AppService EasyAuth (協力廠商) 的 .NET Core 整合 ](https://github.com/MaximRouiller/MaximeRouiller.Azure.AppService.EasyAuth)
* [使用 .NET Core (協力廠商) 取得 Azure App Service 驗證 ](https://github.com/kirkone/KK.AspNetCore.EasyAuthAuthentication)

提供者專屬的使用說明指南：

* [如何設定 App 使用 Azure Active Directory 登入][AAD]
* [如何設定 App 使用 Facebook 登入][Facebook]
* [如何設定 App 以使用 Google 登入][Google]
* [如何設定 App 使用 Microsoft 帳戶登入][MSA]
* [如何設定 App 以使用 Twitter 登入][Twitter]
* [如何設定您的應用程式以使用 OpenID Connect 提供者登入 (預覽) ][OIDC]
* [如何設定您的應用程式以使用 Apple (preview) 的登入 ][Apple]

[AAD]: configure-authentication-provider-aad.md
[Facebook]: configure-authentication-provider-facebook.md
[Google]: configure-authentication-provider-google.md
[MSA]: configure-authentication-provider-microsoft.md
[Twitter]: configure-authentication-provider-twitter.md
[OIDC]: configure-authentication-provider-openid-connect.md
[Apple]: configure-authentication-provider-apple.md

[custom-auth]: /previous-versions/azure/app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk#custom-auth

[ADAL-Android]: /previous-versions/azure/app-service-mobile/app-service-mobile-android-how-to-use-client-library#adal
[ADAL-iOS]: /previous-versions/azure/app-service-mobile/app-service-mobile-ios-how-to-use-client-library#adal
[ADAL-dotnet]: /previous-versions/azure/app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library#adal
