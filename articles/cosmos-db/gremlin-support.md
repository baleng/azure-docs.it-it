---
title: Supporto per Gremlin di Azure Cosmos DB
description: Informazioni sul linguaggio Gremlin di Apache TinkerPop. Informazioni sulle funzionalità e sulle procedure disponibili in Azure Cosmos DB
author: LuisBosquez
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: overview
ms.date: 01/02/2018
ms.author: lbosq
ms.openlocfilehash: c4622293f05be5f4595136a5bbf194116fb2887c
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2019
ms.locfileid: "58081101"
---
# <a name="azure-cosmos-db-gremlin-graph-support"></a>Supporto Gremlin Graph di Azure Cosmos DB
Azure Cosmos DB supporta il linguaggio di attraversamento di grafi [Gremlin](https://tinkerpop.apache.org/docs/current/reference/#graph-traversal-steps) di [Apache Tinkerpop](https://tinkerpop.apache.org), ovvero un'API Gremlin per la creazione di entità di grafi e l'esecuzione di operazioni di query sui grafi. È possibile usare il linguaggio Gremlin per creare le entità dei grafi (vertici e archi), modificare proprietà all'interno di tali entità, eseguire query e attraversamenti ed eliminare entità. 

Azure Cosmos DB offre funzionalità di livello aziendale per database a grafi. Questo include distribuzione globale, scalabilità indipendente di archiviazione e velocità effettiva, latenze stimabili in pochissimi millisecondi, indicizzazione automatica, contratti di servizio e disponibilità di lettura per gli account database che si estendono sue due o più aree Azure. Poiché Azure Cosmos DB supporta TinkerPop/Gremlin, è possibile eseguire facilmente la migrazione di applicazioni scritte usando un altro database a grafi senza dover apportare modifiche al codice. In più, grazie al supporto Gremlin, Azure Cosmos DB si integra facilmente con framework di analisi abilitati per TinkerPop come [Apache Spark GraphX](https://spark.apache.org/graphx/). 

Questo articolo illustra una procedura dettagliata di Gremlin ed enumera le funzionalità e i passaggi di Gremlin supportati dall'API Gremlin.

## <a name="gremlin-by-example"></a>Esempio di Gremlin
Verrà ora usato un grafo di esempio per comprendere come le query possono essere espresse in Gremlin. La figura seguente illustra un'applicazione aziendale che gestisce i dati su utenti, interessi e dispositivi sotto forma di grafo.  

![Database di esempio che mostra persone, dispositivi e interessi](./media/gremlin-support/sample-graph.png) 

Questo grafo presenta i seguenti tipi di vertici (denominati "label" in Gremlin):

- Persone: il grafo include tre persone, Robin, Thomas e Ben
- Interessi: i loro interessi, in questo esempio, sono rappresentati dal gioco del football
- Dispositivi: i dispositivi usati dalle persone
- Sistemi operativi: i sistemi operativi eseguiti nei dispositivi

Si rappresentano le relazioni tra queste entità tramite i seguenti tipi/etichette di archi:

- Conosce: ad esempio, "Thomas conosce Robin"
- Interessato: per rappresentare gli interessi delle persone nel nostro grafo, ad esempio, "Ben è interessato al football"
- RunsOS: il portatile esegue il sistema operativo Windows
- Usa: per rappresentare quale dispositivo viene usato da una persona. Ad esempio, Robin usa un telefono Motorola con numero di serie 77

Verranno ora eseguite alcune operazioni su questo grafo tramite la [console Gremlin](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console). È anche possibile eseguire queste operazioni usando i driver Gremlin nella piattaforma di propria scelta (Java, Node.js, Python o .NET).  Prima di esaminare cosa è supportato in Azure Cosmos DB, verranno esaminati alcuni esempi per acquisire familiarità con la sintassi.

Verrà dapprima esaminato CRUD. L'istruzione Gremlin seguente inserisce il vertice "Thomas" nel grafo:

```
:> g.addV('person').property('id', 'thomas.1').property('firstName', 'Thomas').property('lastName', 'Andersen').property('age', 44)
```

Successivamente, l'istruzione Gremlin seguente inserisce un arco "conosce" tra Thomas e Robin.

```
:> g.V('thomas.1').addE('knows').to(g.V('robin.1'))
```

La query seguente restituisce i vertici "persona" secondo l'ordine decrescente dei relativi nomi:
```
:> g.V().hasLabel('person').order().by('firstName', decr)
```

I grafi sono eccellenti quando è necessario rispondere a domande come "Quali sistemi operativi usano gli amici di Thomas?". È possibile eseguire questo semplice attraversamento Gremlin per ottenere informazioni dal grafo:

```
:> g.V('thomas.1').out('knows').out('uses').out('runsos').group().by('name').by(count())
```
Ora verrà esaminato cosa Azure Cosmos DB offre agli sviluppatori di Gremlin.

## <a name="gremlin-features"></a>Funzionalità di Gremlin
TinkerPop è uno standard che copre un'ampia gamma di tecnologie a grafi. Pertanto, ha una terminologia standard usata per descrivere le funzionalità offerte da un provider di grafi. Azure Cosmos DB offre un database a grafi scrivibile, ad alta concorrenza, persistente, che può essere partizionato tra più server o cluster. 

La tabella seguente elenca le funzionalità di TinkerPop implementate da Azure Cosmos DB: 

| Categoria | Implementazione di Azure Cosmos DB |  Note | 
| --- | --- | --- |
| Funzionalità del grafo | Offre persistenza e accesso simultaneo. Progettata per supportare le transazioni | Metodi di calcolo possono essere implementati tramite il connettore Spark. |
| Funzionalità della variabile | Supporta Boolean, Integer, Byte, Double, Float, Integer, Long, String | Supporta i tipi primitivi, è compatibile con i tipi complessi tramite modello di dati |
| Funzionalità del vertice | Supporta RemoveVertices, MetaProperties, AddVertices, MultiProperties, StringIds, UserSuppliedIds, AddProperty, RemoveProperty  | Supporta la creazione, la modifica e l'eliminazione di vertici |
| Funzionalità della proprietà del vertice | StringIds, UserSuppliedIds, AddProperty, RemoveProperty, BooleanValues, ByteValues, DoubleValues, FloatValues, IntegerValues, LongValues, StringValues | Supporta la creazione, la modifica e l'eliminazione di proprietà di vertici |
| Funzionalità dell'arco | AddEdges, RemoveEdges, StringIds, UserSuppliedIds, AddProperty, RemoveProperty | Supporta la creazione, la modifica e l'eliminazione di archi |
| Funzionalità della proprietà dell'arco | Properties, BooleanValues, ByteValues, DoubleValues, FloatValues, IntegerValues, LongValues, StringValues | Supporta la creazione, la modifica e l'eliminazione di proprietà dell'arco |

## <a name="gremlin-wire-format-graphson"></a>Formato wire Gremlin: GraphSON

Azure Cosmos DB usa il [ formato GraphSON](https://github.com/thinkaurelius/faunus/wiki/GraphSON-Format) per la restituzione di risultati dalle operazioni Gremlin. GraphSON è il formato standard Gremlin per la rappresentazione di vertici, archi e proprietà (singolo e multi-valore) tramite JSON. 

Ad esempio, il frammento di codice seguente mostra una rappresentazione GraphSON di un vertice *restituito al client* da Azure Cosmos DB. 

```json
  {
    "id": "a7111ba7-0ea1-43c9-b6b2-efc5e3aea4c0",
    "label": "person",
    "type": "vertex",
    "outE": {
      "knows": [
        {
          "id": "3ee53a60-c561-4c5e-9a9f-9c7924bc9aef",
          "inV": "04779300-1c8e-489d-9493-50fd1325a658"
        },
        {
          "id": "21984248-ee9e-43a8-a7f6-30642bc14609",
          "inV": "a8e3e741-2ef7-4c01-b7c8-199f8e43e3bc"
        }
      ]
    },
    "properties": {
      "firstName": [
        {
          "value": "Thomas"
        }
      ],
      "lastName": [
        {
          "value": "Andersen"
        }
      ],
      "age": [
        {
          "value": 45
        }
      ]
    }
  }
```

Le proprietà usate da GraphSON per i vertici sono le seguenti:

| Proprietà | DESCRIZIONE |
| --- | --- |
| id | ID del vertice. Deve essere univoco (in combinazione con il valore di _partition se applicabile) |
| label | Etichetta del vertice. Questo è facoltativo e viene usato per descrivere il tipo di entità. |
| type | Usato per distinguere i vertici da documenti non a grafo |
| properties | Contenitore delle proprietà definite dall'utente associate al vertice. Ogni proprietà può avere più valori. |
| _partition (configurabile) | La chiave di partizione del vertice. Può essere usata per scalare orizzontalmente grafi in più server |
| outE | Contiene un elenco degli archi uscenti da un vertice. L'archiviazione delle informazioni di adiacenza con il vertice consente l'esecuzione rapida degli attraversamenti. Gli archi vengono raggruppati in base alle relative etichette. |

E l'arco contiene le informazioni seguenti per spostarsi in altre parti del grafo.

| Proprietà | DESCRIZIONE |
| --- | --- |
| id | ID dell'arco. Deve essere univoco (in combinazione con il valore di _partition se applicabile) |
| label | Etichetta dell'arco. Questa proprietà è facoltativa e viene usata per descrivere il tipo di relazione. |
| inV | Contiene un elenco di vertici per un bordo. L'archiviazione delle informazioni di adiacenza con il bordo consente l'esecuzione rapida degli attraversamenti. I vertici vengono raggruppati in base alle relative etichette. |
| properties | Contenitore delle proprietà definite dall'utente associate all'arco. Ogni proprietà può avere più valori. |

Ogni proprietà può archiviare più valori all'interno di una matrice. 

| Proprietà | DESCRIZIONE |
| --- | --- |
| value | Valore della proprietà

## <a name="gremlin-steps"></a>Step di Gremlin
Verranno ora esaminati gli step di Gremlin supportati da Azure Cosmos DB. Per informazioni complete su Gremlin, vedere [TinkerPop reference](https://tinkerpop.apache.org/docs/current/reference) (Riferimento a TinkerPop).

| step | DESCRIZIONE | Documentazione TinkerPop 3.2 |
| --- | --- | --- |
| `addE` | Aggiunge un arco tra due vertici | [addE step](https://tinkerpop.apache.org/docs/current/reference/#addedge-step) |
| `addV` | Aggiunge un vertice al grafo | [addV step](https://tinkerpop.apache.org/docs/current/reference/#addvertex-step) |
| `and` | Assicura che tutti gli attraversamenti restituiscono un valore | [and step](https://tinkerpop.apache.org/docs/current/reference/#and-step) |
| `as` | Modulatore di step per assegnare una variabile all'output di uno step | [as step](https://tinkerpop.apache.org/docs/current/reference/#as-step) |
| `by` | Modulatore di step usato con `group` e `order` | [by step](https://tinkerpop.apache.org/docs/current/reference/#by-step) |
| `coalesce` | Restituisce il primo attraversamento che restituisce un risultato | [coalesce step](https://tinkerpop.apache.org/docs/current/reference/#coalesce-step) |
| `constant` | Restituisce un valore costante. Usato con `coalesce`| [constant step](https://tinkerpop.apache.org/docs/current/reference/#constant-step) |
| `count` | Restituisce il conteggio risultante dall'attraversamento | [count step](https://tinkerpop.apache.org/docs/current/reference/#count-step) |
| `dedup` | Restituisce i valori con i duplicati rimossi | [dedup step](https://tinkerpop.apache.org/docs/current/reference/#dedup-step) |
| `drop` | Elimina i valori (vertice/arco) | [drop step](https://tinkerpop.apache.org/docs/current/reference/#drop-step) |
| `fold` | Si comporta come una barriera che calcola l'aggregazione di risultati| [fold step](https://tinkerpop.apache.org/docs/current/reference/#fold-step) |
| `group` | Raggruppa i valori in base alle etichette specificate| [group step](https://tinkerpop.apache.org/docs/current/reference/#group-step) |
| `has` | Usato per filtrare proprietà, vertici e archi. Supporta `hasLabel`, `hasId`, `hasNot` e le varianti `has`. | [has step](https://tinkerpop.apache.org/docs/current/reference/#has-step) |
| `inject` | Inserisce i valori in un flusso| [inject step](https://tinkerpop.apache.org/docs/current/reference/#inject-step) |
| `is` | Usato per eseguire un filtro con un'espressione booleana | [is step](https://tinkerpop.apache.org/docs/current/reference/#is-step) |
| `limit` | Usato per limitare il numero di elementi nell'attraversamento| [limit step](https://tinkerpop.apache.org/docs/current/reference/#limit-step) |
| `local` | Esegue il wrapping di una sezione di attraversamento, simile a una sottoquery | [local step](https://tinkerpop.apache.org/docs/current/reference/#local-step) |
| `not` | Usato per produrre la negazione di un filtro | [not step](https://tinkerpop.apache.org/docs/current/reference/#not-step) |
| `optional` | Restituisce il risultato dell'attraversamento specificato se fornisce un risultato, in caso contrario restituisce l'elemento chiamante | [optional step](https://tinkerpop.apache.org/docs/current/reference/#optional-step) |
| `or` | Garantisce che almeno uno degli attraversamenti restituisca un valore | [or step](https://tinkerpop.apache.org/docs/current/reference/#or-step) |
| `order` | Restituisce i risultati nell'ordinamento specificato | [order step](https://tinkerpop.apache.org/docs/current/reference/#order-step) |
| `path` | Restituisce il percorso completo dell'attraversamento | [path step](https://tinkerpop.apache.org/docs/current/reference/#path-step) |
| `project` | Proietta le proprietà come Mappa | [project step](https://tinkerpop.apache.org/docs/current/reference/#project-step) |
| `properties` | Restituisce le proprietà per le etichette specificate | [properties step](https://tinkerpop.apache.org/docs/current/reference/#properties-step) |
| `range` | Filtra per l'intervallo di valori specificato| [range step](https://tinkerpop.apache.org/docs/current/reference/#range-step) |
| `repeat` | Ripete lo step per il numero di volte specificato. Usato per eseguire i cicli | [repeat step](https://tinkerpop.apache.org/docs/current/reference/#repeat-step) |
| `sample` | Usato per campionare i risultati dell'attraversamento | [sample step](https://tinkerpop.apache.org/docs/current/reference/#sample-step) |
| `select` | Usato per proiettare i risultati dell'attraversamento |  [select step](https://tinkerpop.apache.org/docs/current/reference/#select-step) |
| `store` | Usato per le aggregazioni non bloccanti risultanti dall'attraversamento | [store step](https://tinkerpop.apache.org/docs/current/reference/#store-step) |
| `tree` | Aggrega i percorsi da un vertice a una struttura ad albero | [tree step](https://tinkerpop.apache.org/docs/current/reference/#tree-step) |
| `unfold` | Srotola un iteratore come step| [unfold step](https://tinkerpop.apache.org/docs/current/reference/#unfold-step) |
| `union` | Unisce i risultati di più attraversamenti| [union step](https://tinkerpop.apache.org/docs/current/reference/#union-step) |
| `V` | Include gli step necessari per gli attraversamenti tra vertici e archi `V`, `E`, `out`, `in`, `both`, `outE`, `inE`, `bothE`, `outV`, `inV`, `bothV`, e `otherV` per | [vertex steps](https://tinkerpop.apache.org/docs/current/reference/#vertex-steps) |
| `where` | Usato per filtrare i risultati dell'attraversamento. Supporta `eq`, `neq`, `lt`, `lte`, `gt`, `gte` e gli operatori `between`  | [where step](https://tinkerpop.apache.org/docs/current/reference/#where-step) |

Il motore ottimizzato per la scrittura fornito da Azure Cosmos DB supporta l'indicizzazione automatica di tutte le proprietà comprese all'interno di vertici e archi per impostazione predefinita. Pertanto, query con filtri, query di intervallo, ordinamento o aggregazioni in qualsiasi proprietà vengono elaborati dall'indice e serviti in modo efficiente. Per altre informazioni sul funzionamento dell'indicizzazione in Azure Cosmos DB, vedere il documento sull'[indicizzazione senza schema](https://www.vldb.org/pvldb/vol8/p1668-shukla.pdf).

## <a name="next-steps"></a>Passaggi successivi
* Iniziare a creare un'applicazione Graph [tramite SDK](create-graph-dotnet.md) 
* Altre informazioni sul [supporto dei grafi](graph-introduction.md) in Azure Cosmos DB
