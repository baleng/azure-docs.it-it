---
title: Anteprima di moreLikeThis in Ricerca di Azure - Ricerca di Azure
description: Documentazione preliminare per la funzionalità moreLikeThis in anteprima esposta nell'API REST di Ricerca di Azure.
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 10/27/2016
ms.author: brjohnst
ms.custom: seodec2018
ms.openlocfilehash: d55a6d883e0dcd5ad4b1c1584b76bae06e6c742a
ms.sourcegitcommit: dd1a9f38c69954f15ff5c166e456fda37ae1cdf2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/07/2019
ms.locfileid: "57569043"
---
# <a name="morelikethis-in-azure-search-preview"></a>Anteprima di moreLikeThis in Ricerca di Azure

`moreLikeThis=[key]` è un parametro query nell'[API di ricerca](https://docs.microsoft.com/rest/api/searchservice/search-documents). Specificando il parametro `moreLikeThis` in una query di ricerca è possibile trovare documenti che sono simili al documento specificato dalla chiave del documento. Quando viene effettuata una richiesta di ricerca con `moreLikeThis`, viene generata una query con i termini di ricerca estratti dal documento specificato che lo descrivono al meglio. La query generata viene quindi usata per eseguire la richiesta di ricerca. Per impostazione predefinita, viene considerato il contenuto di tutti i campi `searchable` a meno che non si usi il parametro `searchFields` per limitare i campi. Il parametro `moreLikeThis` non può essere usato con il parametro di ricerca `search=[string]`.

## <a name="examples"></a>Esempi 

Di seguito è riportato un esempio di una query moreLikeThis. La query trova i documenti i cui campi di descrizione sono più simili al campo del documento di origine come specificato nel parametro `moreLikeThis`.

```
Get /indexes/hotels/docs?moreLikeThis=1002&searchFields=description&api-version=2016-09-01-Preview
```

```
POST /indexes/hotels/docs/search?api-version=2016-09-01-Preview
    {
      "moreLikeThis": "1002",
      "searchFields": "description"
    }
```

## <a name="feature-availability"></a>Disponibilità delle funzionalità

La funzionalità moreLikeThis è attualmente disponibile in anteprima ed è supportata solo nelle versioni API di anteprima, ovvero `2015-02-28-Preview` e `2016-09-01-Preview`. Poiché la versione dell'API è specificata nella richiesta, è possibile combinare API disponibili a livello generale (GA) e di anteprima nella stessa applicazione. Le API disponibili in anteprima non rientrano nel Contratto di servizio e le funzionalità disponibili in anteprima possono subire modifiche, quindi non è consigliabile usarle nelle applicazioni di produzione.
