---
title: 'Generare ed esportare i certificati per le connessioni da punto a sito: Linux: Interfaccia della riga di comando: Azure | Microsoft Docs'
description: Creare un certificato radice autofirmato, esportare la chiave pubblica e generare certificati client usando l'interfaccia della riga di comando Linux (strongSwan).
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 01/31/2019
ms.author: cherylmc
ms.openlocfilehash: b673be47d4951adab08f04efc56410095f549356
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2019
ms.locfileid: "55564792"
---
# <a name="generate-and-export-certificates"></a>Generare ed esportare i certificati

Le connessioni da punto a sito usano certificati per l'autenticazione. Questo articolo illustra come creare un certificato radice autofirmato e generare i certificati usando l'interfaccia della riga di comando Linux e strongSwan. Se si cercano diverse istruzioni di certificato, vedere gli articoli [Powershell](vpn-gateway-certificates-point-to-site.md) oppure [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md). Per informazioni su come installare strongSwan usando l'interfaccia utente grafica anziché l'interfaccia della riga di comando, vedere i passaggi dell'articolo [Configurazione del client](point-to-site-vpn-client-configuration-azure-cert.md#install).

## <a name="generate-and-export"></a>Generare ed esportare
[!INCLUDE [strongSwan certificates](../../includes/vpn-gateway-strongswan-certificates-include.md)]

## <a name="next-steps"></a>Passaggi successivi

Continuare con la configurazione da punto a sito [Creare e installare i file di configurazione del client VPN](point-to-site-vpn-client-configuration-azure-cert.md#linuxinstallcli).
