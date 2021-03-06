---
title: Tipi di LiveEvent Servizi multimediali di Azure | Microsoft Docs
description: In questo articolo è inclusa una tabella dettagliata per confrontare i tipi di LiveEvent.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 03/01/2019
ms.author: juliako
ms.openlocfilehash: 9952a7bbac1eb79de0d3425f839e3bd30196844e
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/02/2019
ms.locfileid: "57243891"
---
# <a name="live-event-types-comparison"></a>Confronto tra tipi di eventi live

In Servizi multimediali di Azure, un [evento live](https://docs.microsoft.com/rest/api/media/liveevents) può essere di due tipi: codifica live e pass-through. 

## <a name="types-comparison"></a>Confronto tra tipi 

La tabella seguente mette a confronto le funzionalità dei due tipi di eventi live.

| Funzionalità | Evento live pass-through | Evento live standard |
| --- | --- | --- |
| Input a bitrate singolo codificato in bitrate multipli nel cloud |No  |Sì |
| Risoluzione video massima per feed di contributo |4K (4096 x 2160 a 60 fotogrammi/sec) |1080p (1920 x 1088 a 30 fotogrammi/sec)|
| Livelli massimi consigliati per feed di contributo|Fino a 12|Un audio|
| Livelli massimi nell'output| Uguale all'input|Fino a 6 (vedere il set di impostazioni di sistema riportata di seguito)|
| Larghezza di banda aggregata massima per feed di contributo|60 Mbps|N/D|
| Velocità in bit massima per un singolo livello all'interno del contributo |20 Mbps|20 Mbps|
| Supporto per tracce audio in più lingue|Sì|No |
| Codec video di input supportati |H.264/AVC e H.265/HEVC|H.264/AVC|
| Codec video di output supportati|Uguale all'input|H.264/AVC|
| Profondità bit video di input e output supportata|Fino a 10 bit incluso HDR 10/HLG|8 bit|
| Codec audio di input supportati|AAC-LC, HE-AAC v1, HE-AAC v2|AAC-LC, HE-AAC v1, HE-AAC v2|
| Codec audio di output supportati|Uguale all'input|AAC-LC|
| Risoluzione video massima del video di output|Uguale all'input|720p (a 30 fotogrammi/secondo)|
| Protocolli di input|RTMP, MP4 frammentato (Smooth Streaming)|RTMP, MP4 frammentato (Smooth Streaming)|
| Prezzo|Vedere la [pagina dei prezzi](https://azure.microsoft.com/pricing/details/media-services/) e fare clic sulla scheda "Video live"|Vedere la [pagina dei prezzi](https://azure.microsoft.com/pricing/details/media-services/) e fare clic sulla scheda "Video live"|
| Tempo di esecuzione massimo| 24 ore su 24, 365 giorni all'anno, live linear | Fino a 24 ore|
| Possibilità di trasferire dati dei sottotitoli CEA 608/708 integrati|Sì|Sì|
| Supporto per l'inserimento di slate|No |No |
| Supporto per annunci pubblicitari tramite API| No |No |
| Supporto per annunci pubblicitari tramite messaggi in banda SCTE-35|Sì|Sì|
| Possibilità di recuperare brevi fasi di stallo in feed di contributo|Sì|No (senza dati di input, l'evento live avvierà lo slate dopo almeno 6 secondi)|
| Supporto per GOP di input non uniformi|Sì|No - La durate GOP dell’input deve essere fissa|
| Supporto per input con frequenza dei fotogrammi variabile|Sì|No: l'input deve essere una frequenza di fotogrammi fissa. Sono tollerate lievi variazioni, ad esempio durante scene ad alta velocità. Il feed di contributo non può però diminuire la frequenza dei fotogrammi (ad esempio a 15 fotogrammi/sec).|
| Arresto automatico dell'evento live in caso di perdita del feed di input|No |Dopo 12 ore, se nessun LiveOutput è in esecuzione|

## <a name="system-presets"></a>Set di impostazioni di sistema

Quando si usa la codifica live (evento Live impostato sullo **Standard**), il set di impostazioni di codifica definisce come viene codificato il flusso in ingresso a più velocità in bit o livelli. Attualmente, l'unico valore consentito per il set di impostazioni è *Default720p* (impostazione predefinita).

Con **Default720p** il video sarà codificato nei 6 livelli seguenti.

### <a name="output-video-stream"></a>Flusso video di output

| Velocità in bit | Larghezza | Altezza: | MaxFPS | Profilo | Nome del flusso di output |
| --- | --- | --- | --- | --- | --- |
| 3500 |1280 |720 |30 |Alto |Video_1280x720_3500kbps |
| 2200 |960 |540 |30 |Alto |Video_960x540_2200kbps |
| 1350 |704 |396 |30 |Alto |Video_704x396_1350kbps |
| 850 |512 |288 |30 |Alto |Video_512x288_850kbps |
| 550 |384 |216 |30 |Alto |Video_384x216_550kbps |
| 200 |340 |192 |30 |Alto |Video_340x192_200kbps |

> [!NOTE]
> Se è necessario usare impostazioni codifica live personalizzato, contattare il amshelp@microsoft.com. È necessario specificare la tabella desiderata di risoluzione e velocità in bit. Verificare che vi sia un solo a 720p e al massimo 6 livelli.

### <a name="output-audio-stream"></a>Flusso audio di output

L'audio viene codificato nel formato stereo AAC-LC a 128 kbps, con una frequenza di campionamento di 48 kHz.

## <a name="next-steps"></a>Passaggi successivi

[Panoramica dello streaming live](live-streaming-overview.md)
