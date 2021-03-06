---
title: Auto-training di un modello di previsione serie temporale
titleSuffix: Azure Machine Learning service
description: 'Informazioni su come usare il servizio Azure Machine Learning per eseguire il training di una serie temporale: previsione del modello di regressione usando di apprendimento automatico.'
services: machine-learning
author: trevorbye
ms.author: trbye
ms.service: machine-learning
ms.subservice: core
ms.reviewer: trbye
ms.topic: conceptual
ms.date: 03/19/2019
ms.openlocfilehash: e1b584d38c4583e37b7c47535c836d1fa7d428f1
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59357239"
---
# <a name="auto-train-a-time-series-forecast-model"></a>Auto-training di un modello di previsione serie temporale

In questo articolo descrive come eseguire il training di un modello di regressione previsione serie temporale utilizzando automatizzati di machine learning nel servizio Azure Machine Learning. Configurazione di un modello di previsione è simile alla configurazione di un modello di regressione standard Usa l'apprendimento automatico, ma esistono alcuni passaggi di pre-elaborazione e le opzioni di configurazione per l'utilizzo di dati delle serie temporali. Gli esempi seguenti illustrano come a:

* Preparare i dati per la modellizzazione della serie temporale
* Configurare i parametri specifici di serie temporali in un' [ `AutoMLConfig` ](/python/api/azureml-train-automl/azureml.train.automl.automlconfig) oggetto
* Eseguire le stime con i dati di serie temporali

## <a name="prerequisites"></a>Prerequisiti

* Un'area di lavoro del servizio Azure Machine Learning. Per creare l'area di lavoro, vedere [creare un'area di lavoro del servizio di Azure Machine Learning](setup-create-workspace.md).
* Questo articolo presuppone la conoscenza base di configurazione di un automatizzati esperimento di machine learning. Seguire le [esercitazione](tutorial-auto-train-models.md) oppure [sulle procedure](how-to-configure-auto-train.md) per visualizzare la base automatizzati di machine learning schemi progettuali di esperimento.

## <a name="preparing-data"></a>Preparazione dei dati

La differenza più importante tra un tipo di attività di regressione e la regressione previsione tipo di attività all'interno di apprendimento automatico, incluso una funzionalità dei dati che rappresenta una serie temporale valido. Una serie temporale regolare con una frequenza coerente e ben definita e ha un valore a ogni punto di esempio in un intervallo di tempo continuo. Prendere in considerazione lo snapshot seguente di un file `sample.csv`.

    week_starting,store,sales_quantity,week_of_year
    9/3/2018,A,2000,36
    9/3/2018,B,600,36
    9/10/2018,A,2300,37
    9/10/2018,B,550,37
    9/17/2018,A,2100,38
    9/17/2018,B,650,38
    9/24/2018,A,2400,39
    9/24/2018,B,700,39
    10/1/2018,A,2450,40
    10/1/2018,B,650,40

Questo set di dati è un semplice esempio di dati delle vendite settimanali per una società che ha due archivi diversi, A e B. inoltre, è una funzionalità per `week_of_year` che consentirà il modello rilevare la stagionalità settimanale. Il campo `week_starting` rappresenta una serie temporale pulita con frequenza settimanale e il campo `sales_quantity` è la colonna di destinazione per l'esecuzione di stime. Leggere i dati in un dataframe di Pandas, quindi usare il `to_datetime` funzione per verificare che la serie temporale è un `datetime` tipo.

```python
import pandas as pd
data = pd.read_csv("sample.csv")
data["week_starting"] = pd.to_datetime(data["week_starting"])
```

In questo caso i dati sono già nell'ordine crescente in base al campo ora `week_starting`. Tuttavia, quando si configura un esperimento, assicurarsi che la colonna desiderata viene ordinata in senso crescente per compilare una serie temporale valido. Si supponga che i dati di 1000 record e apportare una divisione deterministica nei dati per creare training set e set di dati di test. Quindi separare il campo di destinazione `sales_quantity` per creare il training di stima e di test imposta.

```python
X_train = data.iloc[:950]
X_test = data.iloc[-50:]

y_train = X_train.pop("sales_quantity").values
y_test = X_test.pop("sales_quantity").values
```

> [!NOTE]
> Durante il training di un modello per la previsione dei valori futuri, assicurarsi che tutte le funzionalità usate nel training possono essere utilizzate durante l'esecuzione di stime per l'orizzonte desiderata. Ad esempio, durante la creazione di una previsione della domanda, tra cui la funzione di prezzo azionario corrente potrebbe altamente aumentare la precisione di training. Tuttavia, se si prevede di previsione con un orizzonte lungo, potrebbe non riuscire a prevedere con precisione futuri valori azionari corrispondente per i futuri punti di serie temporali e potrebbe soffrirne l'accuratezza del modello.

## <a name="configure-experiment"></a>Configurare l'esperimento

Per le attività di previsione, automatizzati di machine learning Usa i passaggi di pre-elaborazione e la stima di specifiche di dati delle serie temporali. Verranno eseguiti i passaggi di pre-elaborazione seguenti:

* Rileva la serie temporale frequenza (ad esempio, oraria, giornaliera, settimanale) di esempio e creare nuovi record per assente punti temporali per rendere la serie continua.
* Attribuire valori mancanti nelle colonne di funzionalità (utilizzando i valori di colonna mediano) e destinazione (tramite il riempimento forward)
* Creare funzionalità in base al livello di dettaglio per attivare effetti fissi tra serie diverse
* Creare funzionalità basate sul tempo per facilitare l'apprendimento di modelli stagionali
* Codificare variabili categoriche per quantità numeriche

Il `AutoMLConfig` oggetto definisce le impostazioni e i dati necessari per un'attività automatizzata di apprendimento automatico. Analogamente a un problema di regressione, si definiscono i parametri standard di training come tipo di attività, il numero di iterazioni, dati di training e numero di cross-convalide. Per le attività di previsione, sono presenti parametri aggiuntivi che devono essere impostati che interessano l'esperimento. Nella tabella seguente descrive ogni parametro e il relativo utilizzo.

| Param | DESCRIZIONE | Obbligatorio |
|-------|-------|-------|
|`time_column_name`|Consente di specificare la colonna Data/ora nei dati di input usati per la compilazione della serie temporale e la frequenza l'inferenza.|✓|
|`grain_column_names`|Nomi definizione di gruppi di serie singola nei dati di input. Se non viene definito livello di dettaglio, si presuppone che il set di dati da una serie temporale.||
|`max_horizon`|Valore massimo desiderato orizzonte di previsione in unità di misura della frequenza di serie temporali.|✓|

Creare le impostazioni di serie temporali come un oggetto dizionario. Impostare il `time_column_name` per il `week_starting` campo nel set di dati. Definire le `grain_column_names` parametro per garantire che **due distinti di gruppi di serie temporali** creati per i dati, una per l'archivio A e B. infine, impostare il `max_horizon` fino a 50 per stimare l'intero test impostata.

```python
time_series_settings = {
    "time_column_name": "week_starting",
    "grain_column_names": ["store"],
    "max_horizon": 50
}
```

A questo punto creare uno standard `AutoMLConfig` dell'oggetto, che specifica il `forecasting` tipo di attività e inviare l'esperimento. Al termine il modello, recuperare l'iterazione di esecuzione ottimale.

```python
from azureml.core.workspace import Workspace
from azureml.core.experiment import Experiment
from azureml.train.automl import AutoMLConfig
import logging

automl_config = AutoMLConfig(task='forecasting',
                             primary_metric='normalized_root_mean_squared_error',
                             iterations=10,
                             X=X_train,
                             y=y_train,
                             n_cross_validations=5,
                             enable_ensembling=False,
                             verbosity=logging.INFO,
                             **time_series_settings)

ws = Workspace.from_config()
experiment = Experiment(ws, "forecasting_example")
local_run = experiment.submit(automl_config, show_output=True)
best_run, fitted_model = local_run.get_output()
```

> [!NOTE]
> Per la procedura (Visualizzatore di concorrenza) la convalida incrociata, dati delle serie temporali possono violare i presupposti statistici della strategia di convalida incrociata di K-riduzione canonico, in modo automatizzato di machine learning implementa una routine di convalida di origine in sequenza per creare riduzioni di convalida incrociata per i dati di serie temporali. Per utilizzare questa procedura, specificare il `n_cross_validations` parametro il `AutoMLConfig` oggetto. È possibile ignorare la convalida e Usa i set di convalida personalizzata con il `X_valid` e `y_valid` parametri.

## <a name="forecasting-with-best-model"></a>Previsioni con il modello migliore

Usare l'iterazione del modello migliore per prevedere i valori per il set di dati di test.

```python
y_predict = fitted_model.predict(X_test)
y_actual = y_test.flatten()
```

Calcolare RMSE (radice errore quadratico medio) tra il `y_test` i valori effettivi e i valori previsti in `y_pred`.

```python
from sklearn.metrics import mean_squared_error
from math import sqrt

rmse = sqrt(mean_squared_error(y_actual, y_predict))
rmse
```

Ora che complessiva è stata determinata accuratezza del modello, il passaggio successivo più realistico consiste nell'usare il modello per stimare valori futuri sconosciuti. È sufficiente fornire un set di dati nello stesso formato del set di test `X_test` , ma con valori DateTime future e la stima risulta set i valori previsti per ogni passaggio di serie temporali. Presupporre gli ultimi record di serie temporali nel set di dati si riferiscono a settimana a partire dalla 12/31/2018. Per prevedere la domanda per la settimana successiva (o tanti punti di previsione, è necessario < = `max_horizon`), creare un singolo record di serie di tempo per ogni archivio per la settimana a partire 01/07/2019.

    week_starting,store,week_of_year
    01/07/2019,A,2
    01/07/2019,A,2

Ripetere i passaggi necessari per caricare i dati futuri in un frame di dati e quindi eseguire `best_run.predict(X_test)` per stimare valori futuri.

> [!NOTE]
> Non è possibile prevedere i valori per il numero di periodi di maggiore di `max_horizon`. Il modello deve essere nuovamente sottoposto a training con un orizzonte di dimensioni maggiori per stimare valori futuri oltre il limite corrente.

## <a name="next-steps"></a>Passaggi successivi

* Seguire le [esercitazione](tutorial-auto-train-models.md) per imparare a creare esperimenti con apprendimento automatico.
* Visualizza i [Azure Machine Learning SDK per Python](https://aka.ms/aml-sdk) documentazione di riferimento.
