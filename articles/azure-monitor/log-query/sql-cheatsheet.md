---
title: 'Confronto tra query SQL e query di log in Monitoraggio di Azure: scheda di riferimento rapido | Microsoft Docs'
description: Informazioni destinate agli utenti che hanno familiarità con SQL per facilitarli nella scrittura di query di log in Monitoraggio di Azure.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/21/2018
ms.author: bwren
ms.openlocfilehash: a0233774deaffe25a8e59f79511a0031b1535ba4
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/18/2019
ms.locfileid: "58011007"
---
# <a name="sql-to-azure-monitor-log-query-cheat-sheet"></a>Confronto tra query SQL e query di log in Monitoraggio di Azure: scheda di riferimento rapido 

La tabella seguente consente agli utenti che hanno familiarità con SQL di apprendere il linguaggio di query Kusto per scrivere query di log in Monitoraggio di Azure. Accanto ai comandi T-SQL per la risoluzione di scenari comuni sono riportati i comandi equivalenti usati nelle query di log di Monitoraggio di Azure.

## <a name="sql-to-azure-monitor"></a>Confronto tra SQL e Monitoraggio di Azure

DESCRIZIONE                             |Query SQL                                                                                          |Query di log in Monitoraggio di Azure
----------------------------------------|---------------------------------------------------------------------------------------------------|----------------------------------------
Selezionare tutti i dati da una tabella            |`SELECT * FROM dependencies`                                                                       |<code>dependencies</code>
Selezionare colonne specifiche da una tabella    |`SELECT name, resultCode FROM dependencies`                                                        |<code>dependencies <br>&#124; project name, resultCode</code>
Selezionare 100 record di una tabella         |`SELECT TOP 100 * FROM dependencies`                                                               |<code>dependencies <br>&#124; take 100</code>
Valutazione null                         |`SELECT * FROM dependencies WHERE resultCode IS NOT NULL`                                          |<code>dependencies <br>&#124; where isnotnull(resultCode)</code>
Confronto di stringhe: uguaglianza             |`SELECT * FROM dependencies WHERE name = "abcde"`                                                  |<code>dependencies <br>&#124; where name == "abcde"</code>
Confronto di stringhe: sottostringa            |`SELECT * FROM dependencies WHERE name like "%bcd%"`                                                   |<code>dependencies <br>&#124; where name contains "bcd"</code>
Confronto di stringhe: wildcard             |`SELECT * FROM dependencies WHERE name like "abc%"`                                                |<code>dependencies <br>&#124; where name startswith "abc"</code>
Confronto di date: ultimo giorno             |`SELECT * FROM dependencies WHERE timestamp > getdate()-1`                                         |<code>dependencies <br>&#124; where timestamp > ago(1d)</code>
Confronto di date: intervallo di date             |`SELECT * FROM dependencies WHERE timestamp BETWEEN '2016-10-01' AND '2016-11-01'`                 |<code>dependencies <br>&#124; where timestamp between (datetime(2016-10-01) .. datetime(2016-10-01))</code>
Confronto booleano                      |`SELECT * FROM dependencies WHERE !(success)`                                                      |<code>dependencies <br>&#124; where success == "False" </code>
Ordina                                    |`SELECT name, timestamp FROM dependencies ORDER BY timestamp asc`                                  |<code>dependencies <br>&#124; order by timestamp asc </code>
Distinzione                                |`SELECT DISTINCT name, type  FROM dependencies`                                                    |<code>dependencies <br>&#124; summarize by name, type </code>
Raggruppamento, aggregazioni                   |`SELECT name, AVG(duration) FROM dependencies GROUP BY name`                                       |<code>dependencies <br>&#124; summarize avg(duration) by name </code>
Alias di colonna, Estendi                  |`SELECT operation_Name as Name, AVG(duration) as AvgD FROM dependencies GROUP BY name`             |<code>dependencies <br>&#124; summarize AvgD=avg(duration) by operation_Name <br>&#124; project Name=operation_Name, AvgD</code>
Primi n record dalla misura                |`SELECT TOP 100 name, COUNT(*) as Count FROM dependencies GROUP BY name ORDER BY Count asc`        |<code>dependencies <br>&#124; summarize Count=count() by name <br>&#124; top 100 by Count asc</code>
Unione                                   |`SELECT * FROM dependencies UNION SELECT * FROM exceptions`                                        |<code>union dependencies, exceptions</code>
Unione: con condizioni                  |`SELECT * FROM dependencies WHERE value > 4 UNION SELECT * FROM exceptions WHERE value < 5`                |<code>dependencies <br>&#124; where value > 4 <br>&#124; union (exceptions <br>&#124; where value < 5)</code>
Join                                    |`SELECT * FROM dependencies JOIN exceptions ON dependencies.operation_Id = exceptions.operation_Id`|<code>dependencies <br>&#124; join (exceptions) on operation_Id == operation_Id</code>


## <a name="next-steps"></a>Passaggi successivi

- Seguire una lezione sulla [scrittura di query di log in Monitoraggio di Azure](get-started-queries.md).
