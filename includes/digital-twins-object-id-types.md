---
title: File di inclusione
description: File di inclusione
services: digital-twins
author: kingdomofends
ms.service: digital-twins
ms.topic: include
ms.date: 12/20/2018
ms.author: adgera
ms.custom: include file
ms.openlocfilehash: 40ab53c941a7ac619ebb09d381a4ae0450f26e8b
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/27/2018
ms.locfileid: "53795898"
---
`objectIdType` (o **tipo di identificatore di oggetto**) si riferisce al tipo di identità a cui viene assegnato un ruolo. Ad eccezione dei tipi `DeviceId` e `UserDefinedFunctionId`, i tipi di identificatore di oggetto corrispondono alle proprietà di oggetti di Azure Active Directory.

La tabella seguente elenca i tipi di identificatore di oggetto supportati in Gemelli digitali di Azure:

| type | DESCRIZIONE |
| --- | --- |
| UserId | Assegna un ruolo a un utente. |
| deviceId | Assegna un ruolo a un dispositivo. |
| DomainName | Assegna un ruolo a un nome di dominio. Ogni utente con il nome di dominio specificato ha i diritti di accesso del ruolo corrispondente. |
| TenantId | Assegna un ruolo a un tenant. Ogni utente appartenente all'ID tenant di Azure AD specificato ha i diritti di accesso del ruolo corrispondente. |
| ServicePrincipalId | Assegna un ruolo a un ID oggetto entità servizio. |
| UserDefinedFunctionId | Assegna un ruolo a una funzione definita dall'utente. |