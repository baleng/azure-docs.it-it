---
title: Esempi di trasformazione di attestazioni booleane per lo schema del framework di gestione delle identità di Azure Active Directory B2C | Microsoft Docs
description: Esempi di trasformazione di attestazioni booleane per lo schema del framework di gestione delle identità di Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: fea0f8c2c2bcab94202916594e66514f2ec87396
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55180127"
---
# <a name="boolean-claims-transformations"></a>Trasformazioni delle attestazioni booleane

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Questo articolo fornisce esempi per l'uso delle trasformazioni di attestazioni booleane dello schema del framework di gestione delle identità di Azure Active Directory (Azure AD) B2C. Per altre informazioni, vedere [ClaimsTransformations](claimstransformations.md).

## <a name="andclaims"></a>AndClaims

Esegue un'operazione And con due inputClaims booleani e imposta l'outputClaim con il risultato dell'operazione.

| Elemento  | TransformationClaimType  | Tipo di dati  | Note |
|-------| ------------------------ | ---------- | ----- |
| InputClaim | inputClaim1 | boolean | Il primo ClaimType da valutare. |
| InputClaim | inputClaim2  | boolean | Il secondo ClaimType da valutare. |
|OutputClaim | outputClaim | boolean | I ClaimTypes generati dopo aver richiamato la trasformazione delle attestazioni (true o false). |

La trasformazione delle attestazioni seguente mostra come usare due ClaimType booleani: `isEmailNotExist` e `isSocialAccount`. L'attestazione di output `presentEmailSelfAsserted` è impostata su `true` se il valore di entrambe le attestazioni di input è `true`. In una fase di orchestrazione è possibile usare una precondizione per preimpostare una pagina autocertificata, solo se l'e-mail di un account di social networking è vuota.

```XML
<ClaimsTransformation Id="CheckWhetherEmailBePresented" TransformationMethod="AndClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="isEmailNotExist" TransformationClaimType="inputClaim1" />
    <InputClaim ClaimTypeReferenceId="isSocialAccount" TransformationClaimType="inputClaim2" />
  </InputClaims>                    
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="presentEmailSelfAsserted" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Esempio

- Attestazioni di input:
    - **inputClaim1**: true
    - **inputClaim2**: false
- Attestazioni di output:
    - **outputClaim**: false


## <a name="assertbooleanclaimisequaltovalue"></a>AssertBooleanClaimIsEqualToValue

Verifica che i valori booleani di due attestazioni siano uguali e genera un'eccezione se non lo sono.

| Elemento | TransformationClaimType  | Tipo di dati  | Note |
| ---- | ------------------------ | ---------- | ----- |
| inputClaim | inputClaim | boolean | Il ClaimType da confermare. |
| InputParameter |valueToCompareTo | boolean | Il valore da confrontare (true o false). |

La trasformazione delle attestazioni **AssertBooleanClaimIsEqualToValue** viene sempre eseguita da un [profilo tecnico di convalida](validation-technical-profile.md) che viene chiamato da un [profilo tecnico autocertificato](self-asserted-technical-profile.md). I metadati del profilo tecnico autocertificato **UserMessageIfClaimsTransformationBooleanValueIsNotEqual** controllano il messaggio di errore che il profilo tecnico presenta all'utente.

![Esecuzione di AssertStringClaimsAreEqual](./media/boolean-transformations/assert-execution.png)

La trasformazione delle attestazioni seguente mostra come controllare il valore di un ClaimType booleano con un valore `true`. Se il valore del ClaimType `accountEnabled` è false, viene generato un messaggio di errore.

```XML
<ClaimsTransformation Id="AssertAccountEnabledIsTrue" TransformationMethod="AssertBooleanClaimIsEqualToValue">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="accountEnabled" TransformationClaimType="inputClaim" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="valueToCompareTo" DataType="boolean" Value="true" />
  </InputParameters>
</ClaimsTransformation>
```


Il profilo tecnico di convalida `login-NonInteractive` chiama la trasformazione delle attestazioni `AssertAccountEnabledIsTrue`.
```XML
<TechnicalProfile Id="login-NonInteractive">
  ...
  <OutputClaimsTransformations>
    <OutputClaimsTransformation ReferenceId="AssertAccountEnabledIsTrue" />
  </OutputClaimsTransformations>
</TechnicalProfile>
```

Il profilo tecnico autocertificato chiama il profilo tecnico **login-NonInteractive** di convalida.

```XML
<TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
  <Metadata>
    <Item Key="UserMessageIfClaimsTransformationBooleanValueIsNotEqual">Custom error message if account is disabled.</Item>
  </Metadata>
  <ValidationTechnicalProfiles>
    <ValidationTechnicalProfile ReferenceId="login-NonInteractive" />
  </ValidationTechnicalProfiles>
</TechnicalProfile>
```

### <a name="example"></a>Esempio

- Attestazioni di input:
    - **inputClaim**: false
    - **valueToCompareTo**: true
- Risultato: errore generato

## <a name="notclaims"></a>NotClaims

Esegue un'operazione Not dell'inputClaim booleano e imposta l'outputClaim con il risultato dell'operazione.

| Elemento | TransformationClaimType | Tipo di dati | Note |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim | boolean | L'attestazione da usare. |
| OutputClaim | outputClaim | boolean | I ClaimType generati dopo che ClaimsTransformation è stato richiamato (true o false). |

Usare questa trasformazione delle attestazioni per eseguire la negazione logica su un'attestazione.

```XML
<ClaimsTransformation Id="CheckWhetherEmailBePresented" TransformationMethod="NotClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="userExists" TransformationClaimType="inputClaim" />
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="userExists" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Esempio

- Attestazioni di input:
    - **inputClaim**: false
- Attestazioni di output:
    - **outputClaim**: true

## <a name="orclaims"></a>OrClaims 

Calcola un'operazione Or di due inputClaims booleani e imposta l'outputClaim con il risultato dell'operazione.

| Elemento | TransformationClaimType | Tipo di dati | Note |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim1 | boolean | Il primo ClaimType da valutare. |
| InputClaim | inputClaim2 | boolean | Il secondo ClaimType da valutare. |
| OutputClaim | outputClaim | boolean | I ClaimType generati dopo aver richiamato la ClaimsTransformation (true o false). |

La trasformazione delle attestazioni seguente mostra come usare `Or` due ClaimType booleani. In una fase di orchestrazione è possibile usare una precondizione per preimpostare una pagina autocertificata, se il valore di una delle attestazioni è `true`.

```XML
<ClaimsTransformation Id="CheckWhetherEmailBePresented" TransformationMethod="OrClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="isLastTOSAcceptedNotExists" TransformationClaimType="inputClaim1" />
    <InputClaim ClaimTypeReferenceId="isLastTOSAcceptedGreaterThanNow" TransformationClaimType="inputClaim2" />
  </InputClaims>                    
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="presentTOSSelfAsserted" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
</ClaimsTransformation>
```

### <a name="example"></a>Esempio

- Attestazioni di input:
    - **inputClaim1**: true
    - **inputClaim2**: false
- Attestazioni di output:
    - **outputClaim**: true

