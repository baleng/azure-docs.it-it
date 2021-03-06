---
title: Riferimento agli eventi di rischio in Azure Active Directory Identity Protection | Microsoft Docs
description: Riferimento agli eventi di rischio in Azure Active Directory Identity Protection.
services: active-directory
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: mtillman
ms.assetid: 14f7fc83-f4bb-41bf-b6f1-a9bb97717c34
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2018
ms.author: joflore
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: d009022d4ec03990d3ed0321ebcb13c72c674ed8
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/27/2019
ms.locfileid: "58516553"
---
# <a name="azure-active-directory-identity-protection-risk-events-reference"></a>Riferimento agli eventi di rischio in Azure Active Directory Identity Protection

La maggior parte delle violazioni della sicurezza si verifica quando utenti malintenzionati ottengono l'accesso a un ambiente impadronendosi dell'identità di un utente. Trovare le identità compromesse non è un compito facile. Azure Active Directory usa l'euristica e gli algoritmi adattivi di Machine Learning per rilevare azioni sospette correlate agli account dell'utente. Ogni azione sospetta rilevata viene archiviata in un record denominato evento di rischio.


## <a name="anonymous-ip-address"></a>Indirizzo IP anonimo

**Tipo di rilevamento:** Tempo reale  
**Nome precedente:** Accessi da indirizzi IP anonimi


Questo tipo di evento di rischio indica accessi da indirizzi IP anonimi, ad esempio browser Tor o VPN per la navigazione in anonimato.
Questi indirizzi IP sono in genere usati da attori che vogliono nascondere i dati di telemetria relativi agli accessi (indirizzo IP, posizione, dispositivo e così via) con intenzioni potenzialmente dannose.


## <a name="atypical-travel"></a>Trasferimento atipico

**Tipo di rilevamento:** Offline  
**Nome precedente:** Trasferimento impossibile a posizioni atipiche


Questo tipo di evento di rischio identifica due accessi provenienti da posizioni geograficamente distanti, almeno una delle quali può anche essere atipica in base al comportamento passato dell'utente. Oltre ad altri fattori, l'algoritmo di apprendimento automatico prende in considerazione il tempo che intercorre tra i due accessi e il tempo necessario all'utente per spostarsi dalla prima posizione alla seconda, che indica che un altro utente sta usando le stesse credenziali.

L'algoritmo ignora i "falsi positivi" evidenti che contribuiscono alle condizioni di impossibilità del trasferimento, ad esempio le VPN e le posizioni usate regolarmente da altri utenti nell'organizzazione. Il sistema ha un periodo di apprendimento iniziale di 14 giorni o 10 accessi, durante i quali apprende il comportamento di accesso di un nuovo utente.


## <a name="leaked-credentials"></a>Credenziali perse

**Tipo di rilevamento:** Offline  
**Nome precedente:** Utenti con credenziali perse


Questo tipo di evento di rischio indica che le credenziali valide dell'utente sono andate perse.
I criminali informatici, quando compromettono password valide di utenti legittimi, spesso condividono tali credenziali, in genere pubblicandole sul dark Web o su siti pastebin oppure scambiandole o vendendole al mercato nero. Il servizio Credenziali perse di Microsoft acquisisce coppie di nome utente/password monitorando i siti Web pubblici e il dark Web e collaborando con:

- Ricercatori

- Forze dell'ordine

- Team di sicurezza di Microsoft

- Altre fonti attendibili

Quando il servizio acquisisce le credenziali utente dal dark Web, da siti pastebin o dalle origini riportate sopra, queste vengono confrontate con le credenziali valide correnti degli utenti di Azure AD per trovare corrispondenze valide.


## <a name="malware-linked-ip-address"></a>Indirizzo IP collegato a malware

**Tipo di rilevamento:** Offline  
**Nome precedente:** Accessi da dispositivi infetti


Questo tipo di evento di rischio indica accessi da indirizzi IP infettati da malware, che comunicano attivamente con un server bot. Viene determinato mettendo in relazione gli indirizzi IP del dispositivo dell'utente con gli indirizzi IP che sono stati in contatto con un server bot mentre quest'ultimo era attivo.


## <a name="unfamiliar-sign-in-properties"></a>Proprietà di accesso insolite

**Tipo di rilevamento:** Tempo reale  
**Nome precedente:** Accessi da posizioni non note

Questo tipo di evento di rischio prende in considerazione oltre cronologia di accesso (IP, latitudine / longitudine e l'ASN) per la ricerca per accessi anomali. Il sistema archivia le informazioni sulle posizioni usate in precedenza da un utente e le considera posizioni "note". L'evento di rischio viene attivato quando viene eseguito l'accesso da una posizione non inclusa nell'elenco delle posizioni note. Gli utenti appena creati sarà in "modalità di apprendimento" per un periodo di tempo in familiare Accedi le proprietà che gli eventi di rischio verranno spente durante gli algoritmi disponibili informazioni sul comportamento dell'utente. L'apprendimento durata della modalità è dinamica e dipende dal tempo richiede l'algoritmo per raccogliere informazioni sufficienti per valutare i modelli di accesso dell'utente. La durata minima è di cinque giorni. Un utente può tornare in modalità di apprendimento dopo un lungo periodo di inattività. Il sistema ignora anche gli accessi da dispositivi noti e le posizioni geograficamente vicine a una posizione nota. 

È anche possibile eseguire questo rilevamento per l'autenticazione di base (o i protocolli legacy). Poiché questi protocolli non hanno proprietà moderne, ad esempio l'ID client, i dati di telemetria per ridurre i falsi positivi sono limitati. È consigliabile passare all'autenticazione moderna.

