---
title: File di inclusione
description: File di inclusione
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/20/2019
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 5a1ff10901d10fb274a6fa1418f04e1f20113cb7
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2019
ms.locfileid: "58203600"
---
# <a name="add-sign-in-with-microsoft-to-an-aspnet-web-app"></a>Aggiungere l'accesso con Microsoft a un'app Web ASP.NET

Questa guida illustra come implementare l'accesso con Microsoft usando una soluzione ASP.NET MVC con un'applicazione tradizionale basata su Web browser tramite OpenID Connect.

Al termine di questa guida, l'applicazione sarà in grado di accettare accessi sia di account personali (ad esempio, outlook.com, live.com e altri) sia di account aziendali o di istituti di istruzione di proprietà di aziende od organizzazioni con Azure Active Directory integrato.

> Questa guida richiede Visual Studio 2015 Update 3 o Visual Studio 2017.  Se non lo si ha, è possibile  [scaricare Visual Studio 2017 gratuitamente](https://www.visualstudio.com/downloads/)

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Funzionamento dell'app di esempio generata da questa guida

![Illustra come l'app di esempio generato da questo funzionamento esercitazioni](media/active-directory-develop-guidedsetup-aspnetwebapp-intro/aspnetbrowsergeneral-updated.png)

L'applicazione di esempio creata in questa guida si basa su uno scenario in cui l'utente usa un browser per accedere a un sito Web ASP.NET in cui viene chiesto di eseguire l'autenticazione tramite un pulsante di accesso. In questo scenario, la maggior parte delle operazioni necessarie per il rendering della pagina Web viene eseguita sul lato server.

## <a name="libraries"></a>Librerie

Questa guida usa le librerie seguenti:

|Libreria|DESCRIZIONE|
|---|---|
|[Microsoft.Owin.Security.OpenIdConnect](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|Middleware che consente a un'applicazione di usare OpenID Connect per l'autenticazione|
|[Microsoft.Owin.Security.Cookies](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|Middleware che consente a un'applicazione di mantenere la sessione utente usando i cookie|
|[Microsoft.Owin.Host.SystemWeb](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|Consente l'esecuzione in IIS di applicazioni basate su OWIN tramite la pipeline di richieste ASP.NET|
