---
title: Predicates 與 PredicateValidations
titleSuffix: Azure AD B2C
description: 使用 Azure Active Directory B2C 中的自訂原則，防止將格式不正確的資料新增至您的 Azure AD B2C 租使用者。
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/30/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 46f04c55b40d4f1bdbbf5fd55eb648d1d3294056
ms.sourcegitcommit: 6172a6ae13d7062a0a5e00ff411fd363b5c38597
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2020
ms.locfileid: "97108411"
---
# <a name="predicates-and-predicatevalidations"></a>Predicates 與 PredicateValidations

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

述詞和 **PredicateValidations** 元素可讓您執行驗證 **程式，以** 確保只會在 Azure Active Directory B2C (Azure AD B2C) 租使用者中輸入正確格式的資料。

下圖顯示元素之間的關聯性：

![顯示述詞和述詞驗證關聯性的圖表](./media/predicates/predicates.png)

## <a name="predicates"></a>述詞

**Predicate** 元素會定義基本驗證來檢查宣告類型的值，並傳回 `true` 或 `false`。 您可以使用指定的 **Method** 元素及與該方法相關聯的一組 **Parameter** 元素來完成驗證。 例如，述詞可以檢查字串宣告值的長度是否介於所指定 Minimum 和 Maximum 參數的範圍內，或者字串宣告值是否包含字元集。 **UserHelpText** 元素會在檢查失敗時，為使用者提供錯誤訊息。 **UserHelpText** 元素的值可以使用 [語言自訂](localization.md)進行當地語系化。

述 **詞元素必須** 緊接在 [BuildingBlocks](buildingblocks.md)元素內的 **ClaimsSchema** 元素之後。

**Predicates** 元素包含下列元素：

| 元素 | 發生次數 | 說明 |
| ------- | ----------- | ----------- |
| Predicate | 1:n | 述詞清單。 |

**Predicate** 元素包含下列屬性：

| 屬性 | 必要 | 說明 |
| --------- | -------- | ----------- |
| 識別碼 | 是 | 要用於述詞的識別碼。 其他元素可以在原則中使用這個識別碼。 |
| 方法 | 是 | 要用於驗證的方法類型。 可能的值：[IsLengthRange](#islengthrange)、[MatchesRegex](#matchesregex)、[IncludesCharacters](#includescharacters) 或 [IsDateRange](#isdaterange)。  |
| HelpText | 否 | 檢查失敗時提供給使用者的錯誤訊息。 此字串可以使用[語言自訂](localization.md)進行當地語系化。 |

**Predicate** 元素包含下列元素：

| 元素 | 發生次數 | 說明 |
| ------- | ----------- | ----------- |
| UserHelpText | 0:1 | 如果檢查失敗， (已淘汰) 使用者的錯誤訊息。 |
| 參數 | 1:1 | 適用於字串驗證方法類型的參數。 |

**Parameters** 元素包含下列元素：

| 元素 | 發生次數 | 說明 |
| ------- | ----------- | ----------- |
| 參數 | 1:n | 適用於字串驗證方法類型的參數。 |

**Parameter** 元素包含下列屬性：

| 元素 | 發生次數 | 說明 |
| ------- | ----------- | ----------- |
| 識別碼 | 1:1 | 參數的識別碼。 |

### <a name="predicate-methods"></a>述詞方法

#### <a name="islengthrange"></a>IsLengthRange

IsLengthRange 方法會檢查字串宣告值的長度是否在指定的最小和最大參數範圍內。 述詞元素支援下列參數：

| 參數 | 必要 | 說明 |
| ------- | ----------- | ----------- |
| 最大值 | 是 | 可以輸入的最大字元數。 |
| 最小值 | 是 | 必須輸入的最小字元數。 |


下列範例顯示具有參數的 IsLengthRange 方法 `Minimum` ，並 `Maximum` 指定字串的長度範圍：

```xml
<Predicate Id="IsLengthBetween8And64" Method="IsLengthRange" HelpText="The password must be between 8 and 64 characters.">
  <Parameters>
    <Parameter Id="Minimum">8</Parameter>
    <Parameter Id="Maximum">64</Parameter>
  </Parameters>
</Predicate>
```

#### <a name="matchesregex"></a>MatchesRegex

MatchesRegex 方法會檢查字串宣告值是否符合正則運算式。 述詞元素支援下列參數：

| 參數 | 必要 | 說明 |
| ------- | ----------- | ----------- |
| RegularExpression | 是 | 要比對的規則運算式模式。 |

下列範例顯示 `MatchesRegex` 方法，以及可指定規則運算式的 `RegularExpression` 參數：

```xml
<Predicate Id="PIN" Method="MatchesRegex" HelpText="The password must be numbers only.">
  <Parameters>
    <Parameter Id="RegularExpression">^[0-9]+$</Parameter>
  </Parameters>
</Predicate>
```

#### <a name="includescharacters"></a>>includescharacters

>includescharacters 方法會檢查字串宣告值是否包含字元集。 述詞元素支援下列參數：

| 參數 | 必要 | 說明 |
| ------- | ----------- | ----------- |
| CharacterSet | 是 | 可輸入的字元組。 例如，小寫字元  `a-z` 、大寫字元 `A-Z` 、數位 `0-9` 或符號清單，例如 `@#$%^&amp;*\-_+=[]{}|\\:',?/~"();!` 。 |

下列範例顯示 `IncludesCharacters` 方法，以及可指定字元集的 `CharacterSet` 參數：

```xml
<Predicate Id="Lowercase" Method="IncludesCharacters" HelpText="a lowercase letter">
  <Parameters>
    <Parameter Id="CharacterSet">a-z</Parameter>
  </Parameters>
</Predicate>
```

#### <a name="isdaterange"></a>>isdaterange

>isdaterange 方法會檢查日期宣告值是否介於指定的最小和最大參數範圍之間。 述詞元素支援下列參數：

| 參數 | 必要 | 說明 |
| ------- | ----------- | ----------- |
| 最大值 | 是 | 可以輸入的最大可能日期。 日期的格式遵循 `yyyy-mm-dd` 慣例或 `Today` 。 |
| 最小值 | 是 | 可以輸入的最小可能日期。 日期的格式遵循 `yyyy-mm-dd` 慣例或 `Today` 。|

下列範例顯示 `IsDateRange` 方法，以及可使用 `yyyy-mm-dd` 和 `Today` 格式來指定日期範圍的 `Minimum` 和 `Maximum` 參數。

```xml
<Predicate Id="DateRange" Method="IsDateRange" HelpText="The date must be between 1970-01-01 and today.">
  <Parameters>
    <Parameter Id="Minimum">1970-01-01</Parameter>
    <Parameter Id="Maximum">Today</Parameter>
  </Parameters>
</Predicate>
```

## <a name="predicatevalidations"></a>PredicateValidations

當述詞定義驗證以針對宣告類型進行檢查時，**PredicateValidations** 會群組一組述詞，以形成可套用至宣告類型的使用者輸入驗證。 每個 **PredicateValidation** 元素均包含一組 **PredicateGroup** 元素，其中包含一組指向 **Predicate** 的 **PredicateReference** 元素。 若要通過驗證，宣告的值應該在所有的 **PredicateGroup** 下方，使用它們的 **PredicateReference** 元素組來傳遞任何述詞的所有測試。

**PredicateValidations** 元素必須緊接在 [BuildingBlocks](buildingblocks.md)元素內的述 **詞元素之後**。

```xml
<PredicateValidations>
  <PredicateValidation Id="">
    <PredicateGroups>
      <PredicateGroup Id="">
        <UserHelpText></UserHelpText>
        <PredicateReferences MatchAtLeast="">
          <PredicateReference Id="" />
          ...
        </PredicateReferences>
      </PredicateGroup>
      ...
    </PredicateGroups>
  </PredicateValidation>
...
</PredicateValidations>
```

**PredicateValidations** 元素包含下列元素：

| 元素 | 發生次數 | 說明 |
| ------- | ----------- | ----------- |
| PredicateValidation | 1:n | 述詞驗證清單。 |

**PredicateValidation** 元素包含下列屬性：

| 屬性 | 必要 | 說明 |
| --------- | -------- | ----------- |
| 識別碼 | 是 | 要用於述詞驗證的識別碼。 **ClaimType** 元素可以在原則中使用這個識別碼。 |

**PredicateValidation** 元素包含下列元素：

| 元素 | 發生次數 | 說明 |
| ------- | ----------- | ----------- |
| PredicateGroups | 1:n | 述詞群組清單。 |

**PredicateGroups** 元素包含下列元素：

| 元素 | 發生次數 | 說明 |
| ------- | ----------- | ----------- |
| PredicateGroup | 1:n | 述詞清單。 |

**PredicateGroup** 元素包含下列屬性：

| 屬性 | 必要 | 說明 |
| --------- | -------- | ----------- |
| 識別碼 | 是 | 要用於述詞群組的識別碼。  |

**PredicateGroup** 元素包含下列元素：

| 元素 | 發生次數 | 說明 |
| ------- | ----------- | ----------- |
| UserHelpText | 0:1 |  述詞的說明，有助於使用者了解他們應輸入的值。 |
| PredicateReferences | 1:n | 述詞參考清單。 |

**PredicateReferences** 元素包含下列屬性：

| 屬性 | 必要 | 說明 |
| --------- | -------- | ----------- |
| MatchAtLeast | 否 | 指定值至少必須符合許多述詞定義，以用於要接受的輸入。 如果未指定，則值必須符合所有述詞定義。 |

**PredicateReferences** 元素包含下列元素：

| 元素 | 發生次數 | 說明 |
| ------- | ----------- | ----------- |
| PredicateReference | 1:n | 對述詞的參考。 |

**PredicateReference** 元素包含下列屬性：

| 屬性 | 必要 | 說明 |
| --------- | -------- | ----------- |
| 識別碼 | 是 | 要用於述詞驗證的識別碼。  |


## <a name="configure-password-complexity"></a>設定密碼複雜度

利用 **Predicates** 和 **PredicateValidationsInput**，您可以控制使用者在建立帳戶時所提供密碼的複雜度需求。 根據預設，Azure AD B2C 會使用強式密碼。 Azure AD B2C 也支援組態選項，可控制客戶可以使用的密碼複雜度。 您可以使用這些述詞元素來定義密碼複雜度：

- 使用 `IsLengthRange` 方法的 **IsLengthBetween8And64**，驗證密碼長度必須介於 8 到 64 個字元之間。
- 使用 `IncludesCharacters` 方法的 **Lowercase**，驗證密碼包含小寫字母。
- 使用 `IncludesCharacters` 方法的 **Uppercase**，驗證密碼包含大寫字母。
- 使用 `IncludesCharacters` 方法的 **Number**，驗證密碼包含數字。
- 使用方法的 **符號** `IncludesCharacters` ，驗證密碼包含數個符號字元的其中一個。
- 使用 `MatchesRegex` 方法的 **PIN**，驗證密碼只包含數字。
- 使用 `MatchesRegex` 方法的 **AllowedAADCharacters**，驗證提供了只對密碼無效的字元。
- 使用 `MatchesRegex` 方法的 **DisallowedWhitespace**，驗證密碼不是以空白字元開始或結尾。

```xml
<Predicates>
  <Predicate Id="IsLengthBetween8And64" Method="IsLengthRange" HelpText="The password must be between 8 and 64 characters.">
    <Parameters>
      <Parameter Id="Minimum">8</Parameter>
      <Parameter Id="Maximum">64</Parameter>
    </Parameters>
  </Predicate>

  <Predicate Id="Lowercase" Method="IncludesCharacters" HelpText="a lowercase letter">
    <Parameters>
      <Parameter Id="CharacterSet">a-z</Parameter>
    </Parameters>
  </Predicate>

  <Predicate Id="Uppercase" Method="IncludesCharacters" HelpText="an uppercase letter">
    <Parameters>
      <Parameter Id="CharacterSet">A-Z</Parameter>
    </Parameters>
  </Predicate>

  <Predicate Id="Number" Method="IncludesCharacters" HelpText="a digit">
    <Parameters>
      <Parameter Id="CharacterSet">0-9</Parameter>
    </Parameters>
  </Predicate>

  <Predicate Id="Symbol" Method="IncludesCharacters" HelpText="a symbol">
    <Parameters>
      <Parameter Id="CharacterSet">@#$%^&amp;*\-_+=[]{}|\\:',.?/`~"();!</Parameter>
    </Parameters>
  </Predicate>

  <Predicate Id="PIN" Method="MatchesRegex" HelpText="The password must be numbers only.">
    <Parameters>
      <Parameter Id="RegularExpression">^[0-9]+$</Parameter>
    </Parameters>
  </Predicate>

  <Predicate Id="AllowedAADCharacters" Method="MatchesRegex" HelpText="An invalid character was provided.">
    <Parameters>
      <Parameter Id="RegularExpression">(^([0-9A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~"();! ]|(\.(?!@)))+$)|(^$)</Parameter>
    </Parameters>
  </Predicate>

  <Predicate Id="DisallowedWhitespace" Method="MatchesRegex" HelpText="The password must not begin or end with a whitespace character.">
    <Parameters>
      <Parameter Id="RegularExpression">(^\S.*\S$)|(^\S+$)|(^$)</Parameter>
    </Parameters>
  </Predicate>
```

當您定義基本驗證之後，可將它們組合在一起，然後建立一組您可在原則中使用的密碼原則：

- **SimplePassword** 會驗證 DisallowedWhitespace、AllowedAADCharacters 及 IsLengthBetween8And64
- **StrongPassword** 會驗證 DisallowedWhitespace、AllowedAADCharacters、IsLengthBetween8And64。 最後一個群組 `CharacterClasses` 會搭配設定為 3 的 `MatchAtLeast` 來執行一組額外的述詞。 使用者密碼長度必須介於 8 到 16 個字元之間，並具備下列其中三個字元：小寫、大寫、數字或符號。
- **CustomPassword** 只會驗證 DisallowedWhitespace、AllowedAADCharacters。 因此，只要字元有效，使用者就能提供任意長度的任何密碼。

```xml
<PredicateValidations>
  <PredicateValidation Id="SimplePassword">
    <PredicateGroups>
      <PredicateGroup Id="DisallowedWhitespaceGroup">
        <PredicateReferences>
          <PredicateReference Id="DisallowedWhitespace" />
        </PredicateReferences>
      </PredicateGroup>
      <PredicateGroup Id="AllowedAADCharactersGroup">
        <PredicateReferences>
          <PredicateReference Id="AllowedAADCharacters" />
        </PredicateReferences>
      </PredicateGroup>
      <PredicateGroup Id="LengthGroup">
        <PredicateReferences>
          <PredicateReference Id="IsLengthBetween8And64" />
        </PredicateReferences>
      </PredicateGroup>
    </PredicateGroups>
  </PredicateValidation>

  <PredicateValidation Id="StrongPassword">
    <PredicateGroups>
      <PredicateGroup Id="DisallowedWhitespaceGroup">
        <PredicateReferences>
          <PredicateReference Id="DisallowedWhitespace" />
       </PredicateReferences>
      </PredicateGroup>
      <PredicateGroup Id="AllowedAADCharactersGroup">
        <PredicateReferences>
          <PredicateReference Id="AllowedAADCharacters" />
        </PredicateReferences>
      </PredicateGroup>
      <PredicateGroup Id="LengthGroup">
        <PredicateReferences>
          <PredicateReference Id="IsLengthBetween8And64" />
        </PredicateReferences>
      </PredicateGroup>
      <PredicateGroup Id="CharacterClasses">
        <UserHelpText>The password must have at least 3 of the following:</UserHelpText>
        <PredicateReferences MatchAtLeast="3">
          <PredicateReference Id="Lowercase" />
          <PredicateReference Id="Uppercase" />
          <PredicateReference Id="Number" />
          <PredicateReference Id="Symbol" />
        </PredicateReferences>
      </PredicateGroup>
    </PredicateGroups>
  </PredicateValidation>

  <PredicateValidation Id="CustomPassword">
    <PredicateGroups>
      <PredicateGroup Id="DisallowedWhitespaceGroup">
        <PredicateReferences>
          <PredicateReference Id="DisallowedWhitespace" />
        </PredicateReferences>
      </PredicateGroup>
      <PredicateGroup Id="AllowedAADCharactersGroup">
        <PredicateReferences>
          <PredicateReference Id="AllowedAADCharacters" />
        </PredicateReferences>
      </PredicateGroup>
    </PredicateGroups>
  </PredicateValidation>
</PredicateValidations>
```

在您的宣告類型中，新增 **PredicateValidationReference** 元素，並指定識別碼作為其中一個述詞驗證，例如 SimplePassword、StrongPassword 或 CustomPassword。

```xml
<ClaimType Id="password">
  <DisplayName>Password</DisplayName>
  <DataType>string</DataType>
  <AdminHelpText>Enter password</AdminHelpText>
  <UserHelpText>Enter password</UserHelpText>
  <UserInputType>Password</UserInputType>
  <PredicateValidationReference Id="StrongPassword" />
</ClaimType>
```

以下顯示當 Azure AD B2C 顯示錯誤訊息時組織元素的方式：

![述詞和 PredicateGroup 密碼複雜度範例的圖表](./media/predicates/predicates-pass.png)

## <a name="configure-a-date-range"></a>設定日期範圍

利用 **Predicates** 和 **PredicateValidations** 元素，您可以使用 `DateTimeDropdown` 來控制 **UserInputType** 的最小和最大日期值。 若要這樣做，請使用 `IsDateRange` 方法來建立 **Predicate**，並提供 Minimum 和 Maximum 參數。

```xml
<Predicates>
  <Predicate Id="DateRange" Method="IsDateRange" HelpText="The date must be between 01-01-1980 and today.">
    <Parameters>
      <Parameter Id="Minimum">1980-01-01</Parameter>
      <Parameter Id="Maximum">Today</Parameter>
    </Parameters>
  </Predicate>
</Predicates>
```

使用對 `DateRange` 述詞的參考來新增 **PredicateValidation**。

```xml
<PredicateValidations>
  <PredicateValidation Id="CustomDateRange">
    <PredicateGroups>
      <PredicateGroup Id="DateRangeGroup">
        <PredicateReferences>
          <PredicateReference Id="DateRange" />
        </PredicateReferences>
      </PredicateGroup>
    </PredicateGroups>
  </PredicateValidation>
</PredicateValidations>
```

在您的宣告類型中，新增 **PredicateValidationReference** 元素，並將識別碼指定為 `CustomDateRange`。

```xml
<ClaimType Id="dateOfBirth">
  <DisplayName>Date of Birth</DisplayName>
  <DataType>date</DataType>
  <AdminHelpText>The user's date of birth.</AdminHelpText>
  <UserHelpText>Your date of birth.</UserHelpText>
  <UserInputType>DateTimeDropdown</UserInputType>
  <PredicateValidationReference Id="CustomDateRange" />
</ClaimType>
 ```

## <a name="next-steps"></a>後續步驟

- 瞭解如何 [在 Azure Active Directory B2C 中使用自訂原則](password-complexity.md) ，使用述詞驗證來設定密碼複雜度。