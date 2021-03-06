---
title: Estendere HDInsight con Rete virtuale - Azure
description: Informazioni su come usare Rete virtuale di Azure per la connessione di HDInsight ad altre risorse cloud o risorse nel proprio data center
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/29/2019
ms.openlocfilehash: a2d06cdbcc6ce995c55c858cb7a50a93ef6b3fb1
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/03/2019
ms.locfileid: "58883565"
---
# <a name="extend-azure-hdinsight-using-an-azure-virtual-network"></a>Estendere Azure HDInsight usando Rete virtuale di Azure

Informazioni su come usare HDInsight con [Rete virtuale di Azure](../virtual-network/virtual-networks-overview.md). Il servizio Rete virtuale di Azure consente di implementare gli scenari seguenti:

* Connessione a HDInsight direttamente da una rete locale.

* Connessione di HDInsight ad archivi dati in una rete virtuale di Azure.

* Accesso diretto ai servizi [Apache Hadoop](https://hadoop.apache.org/) che non sono disponibili pubblicamente su Internet, ad esempio le API [Apache Kafka](https://kafka.apache.org/) o l'API Java [Apache HBase](https://hbase.apache.org/).

> [!IMPORTANT]  
> Dopo il 28 febbraio 2019 è verranno eseguito il provisioning di risorse di rete (ad esempio, schede di rete, servizi Location based e così via) per i nuovi cluster creato in una rete virtuale nello stesso gruppo di risorse cluster HDInsight. In precedenza, queste risorse sono state sottoposte a provisioning nel gruppo di risorse della rete virtuale. Non sussiste alcuna modifica per i cluster in esecuzione correnti e i cluster creati senza una rete virtuale.

## <a name="prerequisites-for-code-samples-and-examples"></a>Prerequisiti per gli esempi di codice ed esempi

* La comprensione delle reti TCP/IP. Se non si ha familiarità con le reti TCP/IP, è consigliabile collaborare con qualcuno che ne abbia conoscenza se si intende apportare modifiche alle reti di produzione.
* Se si usa PowerShell, è necessario il [modulo di AZ](https://docs.microsoft.com/powershell/azure/overview).
* Se vogliono usare Azure CLI e non è stato ancora installato, vedere [installare CLI Azure](https://docs.microsoft.com/cli/azure/install-azure-cli).

> [!IMPORTANT]  
> Per istruzioni dettagliate sulla connessione di HDInsight alla rete locale tramite Rete virtuale di Microsoft Azure, vedere il documento [Connettere HDInsight alla rete locale](connect-on-premises-network.md).

## <a name="planning"></a>Pianificazione

Di seguito sono elencate le domande che è necessario porsi quando si intende installare HDInsight in una rete virtuale:

* Occorre installare HDInsight in una rete virtuale esistente? Oppure si intende creare una rete nuova?

    Se si usa una rete virtuale esistente, potrebbe essere necessario modificare la configurazione di rete prima di poter installare HDInsight. Per altre informazioni, vedere la sezione [Aggiungere HDInsight a una rete virtuale esistente](#existingvnet).

* La rete virtuale che contiene HDInsight deve essere connessa a un'altra rete virtuale o alla rete locale?

    Per usare facilmente le risorse tra più reti, potrebbe essere necessario creare un DNS personalizzato e configurare l'inoltro DNS. Per altre informazioni, vedere la sezione [Connessione a più reti](#multinet).

* Si vuole limitare/reindirizzare il traffico in ingresso o in uscita a HDInsight?

    HDInsight deve disporre di una comunicazione senza restrizioni con indirizzi IP specifici nel data center di Azure. Sono presenti anche diverse porte che devono essere abilitate attraverso i firewall per la comunicazione client. Per altre informazioni, vedere la sezione [Controllo del traffico di rete](#networktraffic).

## <a id="existingvnet"></a>Aggiungere HDInsight a una rete virtuale esistente

Seguire la procedura in questa sezione per aggiungere un nuovo cluster HDInsight a una rete virtuale di Azure esistente.

> [!NOTE]  
> È possibile aggiungere un cluster HDInsight esistente in una rete virtuale.

1. Si intende usare un modello di distribuzione classico o di Resource Manager per la rete virtuale?

    HDInsight 3.4 e versione successiva richiedono l'utilizzo di una rete virtuale di Resource Manager. Le versioni precedenti di HDInsight richiedono una rete virtuale classica.

    Se la rete virtuale esistente è un modello classico, è necessario creare una rete virtuale di Resource Manager e successivamente connettere le due reti. [Connessione di reti virtuali classiche a reti virtuali nuove](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

    Dopo che le due reti sono state unite, HDInsight installato nella rete di Resource Manager può interagire con le risorse della rete classica.

2. Si usa il tunneling forzato? Il tunneling forzato è un'impostazione di subnet che spinge il traffico Internet in uscita verso un dispositivo per l'ispezione e la registrazione. HDInsight non supporta il tunneling forzato. Occorre quindi rimuovere questa funzionalità prima di distribuire HDInsight in una subnet esistente oppure creare una nuova subnet per HDInsight senza tunneling forzato.

3. Si usano gruppi di sicurezza di rete, route definite dall'utente o appliance di rete virtuali per limitare il traffico all'interno o all'esterno della rete virtuale?

    In qualità di servizio gestito, HDInsight richiede l'accesso senza restrizioni a vari indirizzi IP nel data center di Azure. Per consentire la comunicazione con questi indirizzi IP, aggiornare tutti i gruppi di sicurezza di rete o le route definite dall'utente esistenti.
    
    HDInsight ospita più servizi, che usano porte diverse. Non bloccare il traffico indirizzato a queste porte. Per un elenco di porte da abilitare attraverso i firewall dell'appliance virtuale, vedere la sezione Sicurezza.
    
    Per trovare la configurazione di sicurezza esistente, usare i comandi di Azure PowerShell o dell'interfaccia della riga di comando di Azure che seguono:

    * Gruppi di sicurezza di rete

        Sostituire `RESOURCEGROUP` con il nome del gruppo di risorse che contiene la rete virtuale e quindi immettere il comando:
    
        ```powershell
        Get-AzNetworkSecurityGroup -ResourceGroupName  "RESOURCEGROUP"
        ```
    
        ```azurecli
        az network nsg list --resource-group RESOURCEGROUP
        ```

        Per altre informazioni, vedere il documento [Risolvere i problemi relativi ai gruppi di sicurezza di rete](../virtual-network/diagnose-network-traffic-filter-problem.md).

        > [!IMPORTANT]  
        > Le regole di gruppo di sicurezza di rete vengono applicate seguendo un ordine basato sulla priorità delle regole. Viene applicata la prima regola che corrisponde al modello di traffico, dopodiché non vengono applicate altre regole per quel traffico. Ordinare le regole dalla più permissiva alla più restrittiva. Per altre informazioni, vedere il documento [Filtrare il traffico di rete con gruppi di sicurezza di rete](../virtual-network/security-overview.md).

    * Route definite dall'utente

        Sostituire `RESOURCEGROUP` con il nome del gruppo di risorse che contiene la rete virtuale e quindi immettere il comando:

        ```powershell
        Get-AzRouteTable -ResourceGroupName "RESOURCEGROUP"
        ```

        ```azurecli
        az network route-table list --resource-group RESOURCEGROUP
        ```

        Per altre informazioni, vedere il documento [Risolvere i problemi relativi alle route](../virtual-network/diagnose-network-routing-problem.md).

4. Creare un cluster HDInsight e selezionare la rete virtuale di Azure durante la configurazione. Per comprendere il processo di creazione del cluster, attenersi alla procedura nei documenti seguenti:

    * [Creare HDInsight nel portale di Azure](hdinsight-hadoop-create-linux-clusters-portal.md)
    * [Creare HDInsight tramite Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
    * [Creare HDInsight utilizzando CLI di Azure classico](hdinsight-hadoop-create-linux-clusters-azure-cli.md)
    * [Creare HDInsight usando un modello di Azure Resource Manager](hdinsight-hadoop-create-linux-clusters-arm-templates.md)

   > [!IMPORTANT]  
   > L'aggiunta di un cluster HDInsight a una rete virtuale è un passaggio di configurazione facoltativo. Assicurarsi di selezionare la rete virtuale quando si configura il cluster.

## <a id="multinet"></a>Connessione a più reti

Il problema più grande con una configurazione con più reti è la risoluzione dei nomi tra le reti.

Azure assicura la risoluzione dei nomi per i servizi di Azure che vengono installati in una rete virtuale. La risoluzione dei nomi integrata consente a HDInsight di connettersi alle risorse seguenti usando un nome di dominio completo (FQDN):

* Qualsiasi risorsa disponibile in Internet, ad esempio microsoft.com o windowsupdate.com.

* Qualsiasi risorsa che si trovi nella stessa rete virtuale di Azure tramite l'utilizzo del __nome DNS interno__ della risorsa. Quando ad esempio si usa la risoluzione dei nomi predefinita, i seguenti sono nomi DNS interni di esempio assegnati a nodi del ruolo di lavoro di HDInsight:

  * wn0-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net
  * wn2-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net

    Entrambi questi nodi possono comunicare direttamente tra loro e con altri nodi in HDInsight, usando i nomi DNS interni.

La risoluzione dei nomi predefinita __non__ consente a HDInsight di risolvere i nomi di risorse in reti che sono aggiunte alla rete virtuale. È ad esempio pratica diffusa unire la rete locale alla rete virtuale. Con la semplice risoluzione dei nomi predefinita, HDInsight non può accedere alle risorse della rete locale in base al nome. È vero anche il contrario. Le risorse nella rete locale non possono accedere alle risorse della rete virtuale in base al nome.

> [!WARNING]  
> È necessario creare il server DNS personalizzato e configurare la rete virtuale per il suo utilizzo prima di creare il cluster HDInsight.

Per abilitare la risoluzione dei nomi tra la rete virtuale e le risorse in reti aggiunte, è necessario eseguire le azioni seguenti:

1. Creare un server DNS personalizzato nella rete virtuale di Azure in cui si intende installare HDInsight.

2. Configurare la rete virtuale per l'utilizzo del server DNS personalizzato.

3. Trovare il suffisso DNS assegnato da Azure per la rete virtuale. Questo valore è simile a `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net`. Per informazioni sulla ricerca del suffisso DNS, vedere la sezione [Esempio: DNS personalizzato](#example-dns).

4. Configurare l'inoltro tra i server DNS. La configurazione dipende dal tipo di rete remota.

   * Se la rete remota è una rete locale, configurare il DNS come indicato di seguito:
        
     * __DNS personalizzato__ (nella rete virtuale):

         * Inoltrare le richieste per il suffisso DNS della rete virtuale al sistema di risoluzione ricorsiva di Azure (168.63.129.16). Azure gestisce le richieste per le risorse nella rete virtuale

         * Inoltrare tutte le altre richieste al server DNS locale. Il server DNS locale gestisce tutte le altre richieste di risoluzione dei nomi, persino le richieste per le risorse Internet quali Microsoft.com.

     * __DNS locale__: inoltrare le richieste per il suffisso DNS di rete virtuale al server DNS personalizzato. Il server DNS personalizzato le inoltra quindi al sistema di risoluzione ricorsiva di Azure.

       Questa configurazione indirizza le richieste di nomi di dominio completi che contengono il suffisso DNS della rete virtuale al server DNS personalizzato. Tutte le altre richieste (anche per gli indirizzi Internet pubblici) vengono gestite dal server DNS locale.

   * Se la rete remota è un'altra rete virtuale di Azure, configurare il DNS come indicato di seguito:

     * __DNS personalizzato__ (in ogni rete virtuale):

         * Le richieste per il suffisso DNS delle reti virtuali vengono inoltrate ai server DNS personalizzati. Il DNS in ogni rete virtuale è responsabile della risoluzione delle risorse all'interno della propria rete.

         * Inoltrare tutte le altre richieste al sistema di risoluzione ricorsiva di Azure. Il sistema di risoluzione ricorsiva è responsabile della risoluzione delle risorse Internet e locali.

       Il server DNS per ogni rete inoltra le richieste all'altro, in base al suffisso DNS. Le altre richieste vengono risolte usando il sistema di risoluzione ricorsiva di Azure.

     Per un esempio di ogni configurazione, vedere la sezione [Esempio: DNS personalizzato](#example-dns).

Per altre informazioni, vedere il documento [Risoluzione dei nomi per le macchine virtuali e le istanze del ruolo](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

## <a name="directly-connect-to-apache-hadoop-services"></a>Connettersi direttamente ai servizi Apache Hadoop

È possibile connettersi al cluster all'indirizzo `https://CLUSTERNAME.azurehdinsight.net`. Questo indirizzo usa un IP pubblico, che potrebbe non essere accessibile se sono stati usati gruppi di sicurezza di rete per limitare il traffico in ingresso da Internet. Quando si distribuisce il cluster in una rete virtuale è inoltre possibile accedervi tramite l'endpoint privato `https://CLUSTERNAME-int.azurehdinsight.net`. Questo endpoint viene risolto in un indirizzo IP privato all'interno della rete virtuale per l'accesso al cluster.

Per connettersi alle pagine Apache Ambari e ad altre pagine Web tramite la rete virtuale, seguire questa procedura:

1. Per individuare i nomi di dominio completi (FQDN) interni dei nodi del cluster HDInsight, usare uno dei modi seguenti:

    Sostituire `RESOURCEGROUP` con il nome del gruppo di risorse che contiene la rete virtuale e quindi immettere il comando:

    ```powershell
    $clusterNICs = Get-AzNetworkInterface -ResourceGroupName "RESOURCEGROUP" | where-object {$_.Name -like "*node*"}

    $nodes = @()
    foreach($nic in $clusterNICs) {
        $node = new-object System.Object
        $node | add-member -MemberType NoteProperty -name "Type" -value $nic.Name.Split('-')[1]
        $node | add-member -MemberType NoteProperty -name "InternalIP" -value $nic.IpConfigurations.PrivateIpAddress
        $node | add-member -MemberType NoteProperty -name "InternalFQDN" -value $nic.DnsSettings.InternalFqdn
        $nodes += $node
    }
    $nodes | sort-object Type
    ```

    ```azurecli
    az network nic list --resource-group RESOURCEGROUP --output table --query "[?contains(name,'node')].{NICname:name,InternalIP:ipConfigurations[0].privateIpAddress,InternalFQDN:dnsSettings.internalFqdn}"
    ```

    Nell'elenco di nodi restituito, trovare i nomi di dominio completi dei nodi head e usarli per connettersi ad Ambari e altri servizi Web. Usare ad esempio `http://<headnode-fqdn>:8080` per accedere ad Ambari.

    > [!IMPORTANT]  
    > Alcuni servizi ospitati nei nodi head sono attivi solo in un nodo alla volta. Se nel tentativo di accedere a un servizio in un nodo head si riceve un messaggio di errore 404, passare al nodo head successivo.

2. Per determinare il nodo e la porta su cui un servizio è disponibile, vedere il documento [Porte usate dai servizi Hadoop su HDInsight](./hdinsight-hadoop-port-settings-for-services.md).

## <a id="networktraffic"></a> Controllo del traffico di rete

Il traffico di rete nelle reti virtuali di Azure può essere controllato usando i modi seguenti:

* i **gruppi di sicurezza di rete** (NSG) consentono di filtrare il traffico di rete in ingresso e in uscita. Per altre informazioni, vedere il documento [Filtrare il traffico di rete con gruppi di sicurezza di rete](../virtual-network/security-overview.md).

    > [!WARNING]  
    > HDInsight non supporta la limitazione del traffico in uscita. Tutto il traffico in uscita deve essere consentito.

* Le **route definite dall'utente** definiscono il flusso del traffico tra le risorse nella rete. Per altre informazioni, vedere il documento [Route definite dall'utente e inoltro IP](../virtual-network/virtual-networks-udr-overview.md).

* Le **appliance virtuali di rete** replicano le funzionalità di dispositivi come i firewall e i router. Per altre informazioni, vedere il documento relativo alle [Appliance di rete](https://azure.microsoft.com/solutions/network-appliances).

Come servizio gestito, HDInsight richiede l'accesso senza restrizioni per l'integrità di HDInsight e servizi di gestione sia per il traffico in ingresso e in uscita dalla rete virtuale. Quando si usano i gruppi di sicurezza di rete e le route definite dall'utente, è necessario assicurarsi che questi servizi possano ancora comunicare con il cluster HDInsight.

### <a id="hdinsight-ip"></a>HDInsight con gruppi di sicurezza di rete e route definite dall'utente

Se si intende usare **gruppi di sicurezza di rete** o **route definite dall'utente** per controllare il traffico di rete, eseguire le azioni seguenti prima di installare HDInsight:

1. Identificare l'area di Azure che si intende usare per HDInsight.

2. Identificare gli indirizzi IP richiesti da HDInsight. Per altre informazioni, vedere la sezione [Indirizzi IP richiesti da HDInsight](#hdinsight-ip).

3. Creare o modificare i gruppi di sicurezza di rete o le route definite dall'utente per la subnet in cui si intende installare HDInsight.

    * __Gruppi di sicurezza di rete__: consentono il traffico __in ingresso__ sulla porta __443__ dagli indirizzi IP. Ciò garantisce che i servizi di gestione di HDInsight possano raggiungere il cluster dall'esterno della rete virtuale.
    * __Route definite dall'utente__: se si pianifica di usare delle route definite dall'utente, creare una route per ogni indirizzo IP e impostare __Tipo hop successivo__ su __Internet__. È anche consigliabile consentire altro traffico in uscita dalla rete virtuale senza restrizioni. Ad esempio, è possibile instradare tutto il traffico per il firewall o rete appliance virtuale di Azure (ospitato in Azure) per il monitoraggio, ma il traffico in uscita non deve essere bloccato.

Per altre informazioni sui gruppi di sicurezza di rete o sulle route definite dall'utente, vedere la documentazione seguente:

* [Gruppo di sicurezza di rete](../virtual-network/security-overview.md)

* [Route definite dall'utente](../virtual-network/virtual-networks-udr-overview.md)

#### <a name="forced-tunneling-to-on-premise"></a>Tunneling forzato a un'istanza locale

Il tunneling forzato è una configurazione di routing definita dall'utente in cui tutto il traffico da una subnet viene spinto verso una rete o un percorso specifico, ad esempio la rete locale. HDInsight viene __non__ supporta il tunneling alle reti locali forzato. Se si usa il Firewall di Azure o un'appliance virtuale di rete ospitato in Azure, è possibile utilizzare route definite dall'utente per instradare il traffico a esso per il monitoraggio e consentire tutto il traffico in uscita.

## <a id="hdinsight-ip"></a> Indirizzi IP richiesti

> [!IMPORTANT]  
> I servizi di gestione e integrità di Azure devono poter comunicare con HDInsight. Se si usano gruppi di sicurezza di rete o route definite dall'utente, consentire il traffico dagli indirizzi IP in modo che tali servizi possano raggiungere HDInsight.
>
> Se non si usano gruppi di sicurezza di rete o route definite dall'utente per controllare il traffico, è possibile ignorare questa sezione.

Se si usano gruppi di sicurezza di rete, è necessario consentire al traffico dai servizi di gestione e integrità di Azure di raggiungere i cluster HDInsight sulla porta 443. È anche necessario consentire il traffico tra macchine virtuali all'interno della subnet. Usare la procedura seguente per trovare gli indirizzi IP che devono essere consentiti:

1. È sempre necessario consentire il traffico dagli indirizzi IP seguenti:

    | Indirizzo IP di origine | Porta di destinazione | Direzione |
    | ---- | ----- | ----- |
    | 168.61.49.99 | 443 | In ingresso |
    | 23.99.5.239 | 443 | In ingresso |
    | 168.61.48.131 | 443 | In ingresso |
    | 138.91.141.162 | 443 | In ingresso |

2. Se il cluster HDInsight si trova in una delle seguenti aree, è necessario consentire il traffico proveniente dagli indirizzi IP elencati per l'area:

    > [!IMPORTANT]  
    > Se si usa un'area di Azure non inclusa nell'elenco, usare solo i quattro IP riportati nel passaggio 1.

    | Paese | Region | Indirizzi IP di origine consentiti | Porta di destinazione consentita | Direzione |
    | ---- | ---- | ---- | ---- | ----- |
    | Asia | Asia orientale | 23.102.235.122</br>52.175.38.134 | 443 | In ingresso |
    | &nbsp; | Asia sud-orientale | 13.76.245.160</br>13.76.136.249 | 443 | In ingresso |
    | Australia | Australia orientale | 104.210.84.115</br>13.75.152.195 | 443 | In ingresso |
    | &nbsp; | Australia sud-orientale | 13.77.2.56</br>13.77.2.94 | 443 | In ingresso |
    | Brasile | Brasile meridionale | 191.235.84.104</br>191.235.87.113 | 443 | In ingresso |
    | Canada | Canada orientale | 52.229.127.96</br>52.229.123.172 | 443 | In ingresso |
    | &nbsp; | Canada centrale | 52.228.37.66</br>52.228.45.222 | 443 | In ingresso |
    | Cina | Cina settentrionale | 42.159.96.170</br>139.217.2.219</br></br>42.159.198.178</br>42.159.234.157 | 443 | In ingresso |
    | &nbsp; | Cina orientale | 42.159.198.178</br>42.159.234.157</br></br>42.159.96.170</br>139.217.2.219 | 443 | In ingresso |
    | &nbsp; | Cina settentrionale 2 | 40.73.37.141</br>40.73.38.172 | 443 | In ingresso |
    | &nbsp; | Cina orientale 2 | 139.217.227.106</br>139.217.228.187 | 443 | In ingresso |
    | Europa | Europa settentrionale | 52.164.210.96</br>13.74.153.132 | 443 | In ingresso |
    | &nbsp; | Europa occidentale| 52.166.243.90</br>52.174.36.244 | 443 | In ingresso |
    | Francia | Francia centrale| 20.188.39.64</br>40.89.157.135 | 443 | In ingresso |
    | Germania | Germania centrale | 51.4.146.68</br>51.4.146.80 | 443 | In ingresso |
    | &nbsp; | Germania nord-orientale | 51.5.150.132</br>51.5.144.101 | 443 | In ingresso |
    | India | India centrale | 52.172.153.209</br>52.172.152.49 | 443 | In ingresso |
    | &nbsp; | India meridionale | 104.211.223.67<br/>104.211.216.210 | 443 | In ingresso |
    | Giappone | Giappone orientale | 13.78.125.90</br>13.78.89.60 | 443 | In ingresso |
    | &nbsp; | Giappone occidentale | 40.74.125.69</br>138.91.29.150 | 443 | In ingresso |
    | Corea del Sud | Corea del Sud centrale | 52.231.39.142</br>52.231.36.209 | 433 | In ingresso |
    | &nbsp; | Corea del Sud meridionale | 52.231.203.16</br>52.231.205.214 | 443 | In ingresso
    | Regno Unito | Regno Unito occidentale | 51.141.13.110</br>51.141.7.20 | 443 | In ingresso |
    | &nbsp; | Regno Unito meridionale | 51.140.47.39</br>51.140.52.16 | 443 | In ingresso |
    | Stati Uniti | Stati Uniti centrali | 13.67.223.215</br>40.86.83.253 | 443 | In ingresso |
    | &nbsp; | Stati Uniti orientali | 13.82.225.233</br>40.71.175.99 | 443 | In ingresso |
    | &nbsp; | Stati Uniti centro-settentrionali | 157.56.8.38</br>157.55.213.99 | 443 | In ingresso |
    | &nbsp; | Stati Uniti centro-occidentali | 52.161.23.15</br>52.161.10.167 | 443 | In ingresso |
    | &nbsp; | Stati Uniti occidentali | 13.64.254.98</br>23.101.196.19 | 443 | In ingresso |
    | &nbsp; | Stati Uniti occidentali 2 | 52.175.211.210</br>52.175.222.222 | 443 | In ingresso |

    Per informazioni sugli indirizzi IP da usare per Azure per enti pubblici, vedere il documento [Azure Government Intelligence + Analytics](https://docs.microsoft.com/azure/azure-government/documentation-government-services-intelligenceandanalytics) (Intelligence e Analisi di Azure per enti pubblici).

3. È necessario consentire l'accesso da __168.63.129.16__. Questo è l'indirizzo del sistema di risoluzione ricorsiva di Azure. Per altre informazioni, vedere il documento [Risoluzione dei nomi per macchine virtuali e istanze del ruolo](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

Per altre informazioni, vedere la sezione [Controllo del traffico di rete](#networktraffic).

Se si usano route definite dall'utente, è necessario specificare una route e consentire il traffico in uscita dalla rete virtuale diretto verso gli indirizzi IP precedenti con l'hop successivo impostato su "Internet".
    
## <a id="hdinsight-ports"></a> Porte richieste

Se si intende usare un **firewall** e accedere al cluster dall'esterno su determinate porte, potrebbe essere necessario consentire il traffico sulle porte necessarie per lo scenario. Per impostazione predefinita, non è necessario definire uno speciale elenco delle porte consentite, purché al traffico di gestione di Azure illustrato nella sezione precedente venga consentito di raggiungere il cluster sulla porta 443.

Per un elenco di porte per servizi specifici, vedere il documento [Porte usate dai servizi Apache Hadoop su HDInsight](hdinsight-hadoop-port-settings-for-services.md).

Per altre informazioni sulle regole del firewall per le appliance virtuali, vedere il documento [Scenario dell'appliance virtuale](../virtual-network/virtual-network-scenario-udr-gw-nva.md).

## <a id="hdinsight-nsg"></a>Ad esempio: gruppi di sicurezza di rete con HDInsight

Gli esempi in questa sezione illustrano come creare regole per i gruppi di sicurezza di rete che consentano a HDInsight di comunicare con i servizi di gestione di Azure. Prima di usare gli esempi, modificare gli indirizzi IP in modo che corrispondano a quelli per l'area di Azure in uso. Queste informazioni sono disponibili nella sezione [HDInsight con gruppi di sicurezza di rete e route definite dall'utente](#hdinsight-ip).

### <a name="azure-resource-management-template"></a>Modello di Resource Manager di Azure

Il modello di Resource Manager seguente crea una rete virtuale che limita il traffico in ingresso, ma consente il traffico dagli indirizzi IP richiesti da HDInsight. Questo modello crea anche un cluster HDInsight nella rete virtuale.

* [Distribuire una rete virtuale di Azure protetta e un cluster Hadoop di HDInsight](https://azure.microsoft.com/resources/templates/101-hdinsight-secure-vnet/)

> [!IMPORTANT]  
> Modificare gli indirizzi IP usati in questo esempio in modo che corrispondano all'area di Azure in uso. Queste informazioni sono disponibili nella sezione [HDInsight con gruppi di sicurezza di rete e route definite dall'utente](#hdinsight-ip).

### <a name="azure-powershell"></a>Azure PowerShell

Usare lo script di PowerShell seguente per creare una rete virtuale che limita il traffico in ingresso e consente il traffico dagli indirizzi IP per l'area Europa settentrionale.

> [!IMPORTANT]  
> Modificare gli indirizzi IP usati in questo esempio in modo che corrispondano all'area di Azure in uso. Queste informazioni sono disponibili nella sezione [HDInsight con gruppi di sicurezza di rete e route definite dall'utente](#hdinsight-ip).

```powershell
$vnetName = "Replace with your virtual network name"
$resourceGroupName = "Replace with the resource group the virtual network is in"
$subnetName = "Replace with the name of the subnet that you plan to use for HDInsight"
# Get the Virtual Network object
$vnet = Get-AzVirtualNetwork `
    -Name $vnetName `
    -ResourceGroupName $resourceGroupName
# Get the region the Virtual network is in.
$location = $vnet.Location
# Get the subnet object
$subnet = $vnet.Subnets | Where-Object Name -eq $subnetName
# Create a Network Security Group.
# And add exemptions for the HDInsight health and management services.
$nsg = New-AzNetworkSecurityGroup `
    -Name "hdisecure" `
    -ResourceGroupName $resourceGroupName `
    -Location $location `
    | Add-AzNetworkSecurityRuleConfig `
        -name "hdirule1" `
        -Description "HDI health and management address 52.164.210.96" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "52.164.210.96" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 300 `
        -Direction Inbound `
    | Add-AzNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 13.74.153.132" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "13.74.153.132" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 301 `
        -Direction Inbound `
    | Add-AzNetworkSecurityRuleConfig `
        -Name "hdirule3" `
        -Description "HDI health and management 168.61.49.99" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "168.61.49.99" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 302 `
        -Direction Inbound `
    | Add-AzNetworkSecurityRuleConfig `
        -Name "hdirule4" `
        -Description "HDI health and management 23.99.5.239" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "23.99.5.239" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 303 `
        -Direction Inbound `
    | Add-AzNetworkSecurityRuleConfig `
        -Name "hdirule5" `
        -Description "HDI health and management 168.61.48.131" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "168.61.48.131" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 304 `
        -Direction Inbound `
    | Add-AzNetworkSecurityRuleConfig `
        -Name "hdirule6" `
        -Description "HDI health and management 138.91.141.162" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "138.91.141.162" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 305 `
        -Direction Inbound `
# Set the changes to the security group
Set-AzNetworkSecurityGroup -NetworkSecurityGroup $nsg
# Apply the NSG to the subnet
Set-AzVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name $subnetName `
    -AddressPrefix $subnet.AddressPrefix `
    -NetworkSecurityGroup $nsg
$vnet | Set-AzVirtualNetwork
```

> [!IMPORTANT]  
> Questo esempio illustra come aggiungere le regole per consentire il traffico in ingresso sugli indirizzi IP richiesti. Non contiene regole per limitare l'accesso in ingresso da altre origini.
>
> L'esempio seguente dimostra come abilitare l'accesso SSH da Internet:
>
> ```powershell
> Add-AzNetworkSecurityRuleConfig -Name "SSH" -Description "SSH" -Protocol "*" -SourcePortRange "*" -DestinationPortRange "22" -SourceAddressPrefix "*" -DestinationAddressPrefix "VirtualNetwork" -Access Allow -Priority 306 -Direction Inbound
> ```

### <a name="azure-cli"></a>Interfaccia della riga di comando di Azure

Seguire questa procedura per creare una rete virtuale che limita il traffico in ingresso, ma consente il traffico dagli indirizzi IP richiesti da HDInsight.

1. Usare il comando seguente per creare un nuovo gruppo di sicurezza di rete denominato `hdisecure`. Sostituire `RESOURCEGROUP` con il gruppo di risorse che contiene la rete virtuale di Azure. Sostituire `LOCATION` con la località (area) che è stato creato il gruppo.

    ```azurecli
    az network nsg create -g RESOURCEGROUP -n hdisecure -l LOCATION
    ```

    Dopo aver creato il gruppo si ricevono informazioni sul nuovo gruppo.

2. Usare le informazioni seguenti per aggiungere regole al nuovo gruppo di sicurezza di rete che ammettono la comunicazione in ingresso sulla porta 443 dal servizio integrità e gestione di Azure HDInsight. Sostituire `RESOURCEGROUP` con il nome del gruppo di risorse che contiene la rete virtuale di Azure.

    > [!IMPORTANT]  
    > Modificare gli indirizzi IP usati in questo esempio in modo che corrispondano all'area di Azure in uso. Queste informazioni sono disponibili nella sezione [HDInsight con gruppi di sicurezza di rete e route definite dall'utente](#hdinsight-ip).

    ```azurecli
    az network nsg rule create -g RESOURCEGROUP --nsg-name hdisecure -n hdirule1 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "52.164.210.96" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 300 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUP --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "13.74.153.132" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 301 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUP --nsg-name hdisecure -n hdirule3 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.49.99" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 302 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUP --nsg-name hdisecure -n hdirule4 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "23.99.5.239" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 303 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUP --nsg-name hdisecure -n hdirule5 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.48.131" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 304 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUP --nsg-name hdisecure -n hdirule6 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "138.91.141.162" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 305 --direction "Inbound"
    ```

3. Per recuperare l'identificatore univoco per questo gruppo di sicurezza di rete, usare il comando seguente:

    ```azurecli
    az network nsg show -g RESOURCEGROUP -n hdisecure --query 'id'
    ```

    Il comando restituisce un valore simile al testo seguente:

        "/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Network/networkSecurityGroups/hdisecure"

    Usare le virgolette doppie intorno `id` nel comando se non si ottengono i risultati previsti.

4. Usare il comando seguente per applicare il gruppo di sicurezza di rete a una subnet. Sostituire il `GUID` e `RESOURCEGROUP` valori con quelli restituiti dal passaggio precedente. Sostituire `VNETNAME` e `SUBNETNAME` con il nome della rete virtuale e il nome di subnet che si desidera creare.

    ```azurecli
    az network vnet subnet update -g RESOURCEGROUP --vnet-name VNETNAME --name SUBNETNAME --set networkSecurityGroup.id="/subscriptions/GUID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Network/networkSecurityGroups/hdisecure"
    ```

    Dopo il completamento del comando, è possibile installare HDInsight nella rete virtuale.

> [!IMPORTANT]  
> Questa procedura consente di accedere solo al servizio di gestione e integrità di HDInsight nel cloud di Azure. Qualsiasi altro accesso al cluster HDInsight dall'esterno della rete virtuale è bloccato. Per abilitare l'accesso dall'esterno della rete virtuale è necessario aggiungere altre regole del gruppo di sicurezza di rete.
>
> L'esempio seguente dimostra come abilitare l'accesso SSH da Internet:
>
> ```azurecli
> az network nsg rule create -g RESOURCEGROUP --nsg-name hdisecure -n hdirule5 --protocol "*" --source-port-range "*" --destination-port-range "22" --source-address-prefix "*" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 306 --direction "Inbound"
> ```

## <a id="example-dns"></a> Esempio: Configurazione del DNS

### <a name="name-resolution-between-a-virtual-network-and-a-connected-on-premises-network"></a>Risoluzione dei nomi tra una rete virtuale e una rete locale connessa

Questo esempio si basa sui presupposti seguenti:

* Si dispone di una rete virtuale di Azure che è connessa a una rete locale tramite un gateway VPN.

* Il server DNS personalizzato nella rete virtuale esegue il sistema operativo Linux o Unix.

* Nel server DNS personalizzato è installato [Bind](https://www.isc.org/downloads/bind/).

Nel server DNS personalizzato nella rete virtuale:

1. Usare Azure PowerShell o l'interfaccia della riga di comando di Azure per individuare il suffisso DNS della rete virtuale:

    Sostituire `RESOURCEGROUP` con il nome del gruppo di risorse che contiene la rete virtuale e quindi immettere il comando:

    ```powershell
    $NICs = Get-AzNetworkInterface -ResourceGroupName "RESOURCEGROUP"
    $NICs[0].DnsSettings.InternalDomainNameSuffix
    ```

    ```azurecli
    az network nic list --resource-group RESOURCEGROUP --query "[0].dnsSettings.internalDomainNameSuffix"
    ```

2. Nel server DNS personalizzato per la rete virtuale, usare il testo seguente come contenuto del file `/etc/bind/named.conf.local`:

    ```
    // Forward requests for the virtual network suffix to Azure recursive resolver
    zone "0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net" {
        type forward;
        forwarders {168.63.129.16;}; # Azure recursive resolver
    };
    ```

    Sostituire il valore `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` con il suffisso DNS della rete virtuale.

    Questa configurazione indirizza tutte le richieste DNS per il suffisso DNS della rete virtuale al sistema di risoluzione ricorsiva di Azure.

2. Nel server DNS personalizzato per la rete virtuale, usare il testo seguente come contenuto del file `/etc/bind/named.conf.options`:

    ```
    // Clients to accept requests from
    // TODO: Add the IP range of the joined network to this list
    acl goodclients {
        10.0.0.0/16; # IP address range of the virtual network
        localhost;
        localnets;
    };

    options {
            directory "/var/cache/bind";

            recursion yes;

            allow-query { goodclients; };

            # All other requests are sent to the following
            forwarders {
                192.168.0.1; # Replace with the IP address of your on-premises DNS server
            };

            dnssec-validation auto;

            auth-nxdomain no;    # conform to RFC1035
            listen-on { any; };
    };
    ```
    
    * Sostituire il valore `10.0.0.0/16` con l'intervallo di indirizzi IP della rete virtuale. Questa voce consente l'indirizzamento delle richieste di risoluzione dei nomi all'interno di questo intervallo.

    * Aggiungere l'intervallo di indirizzi IP della rete locale alla sezione `acl goodclients { ... }`.  Questa voce consente le richieste di risoluzione dei nomi dalle risorse nella rete locale.
    
    * Sostituire il valore `192.168.0.1` con l'indirizzo IP del server DNS locale. Questa voce indirizza tutte le altre richieste DNS al server DNS locale.

3. Per usare la configurazione, riavviare Bind. Ad esempio: `sudo service bind9 restart`.

4. Aggiungere un server d'inoltro condizionale al server DNS locale. Configurare il server d'inoltro condizionale per l'invio di richieste del suffisso DNS del passaggio 1 al server DNS personalizzato.

    > [!NOTE]  
    > Per informazioni dettagliate su come aggiungere un server d'inoltro condizionale, consultare la documentazione del software del DNS.

Dopo avere completato questi passaggi, è possibile connettersi alle risorse in entrambe le reti usando i nomi di dominio completi (FQDN). È ora possibile installare HDInsight nella rete virtuale.

### <a name="name-resolution-between-two-connected-virtual-networks"></a>Risoluzione dei nomi tra due reti virtuali connesse

Questo esempio si basa sui presupposti seguenti:

* Si dispone di due reti virtuali di Azure connesse tramite gateway VPN o peering.

* Il server DNS personalizzato in entrambe le reti esegue il sistema operativo Linux o Unix.

* Nei server DNS personalizzati è installato [Bind](https://www.isc.org/downloads/bind/).

1. Usare Azure PowerShell o l'interfaccia della riga di comando di Azure per individuare il suffisso DNS di entrambe le reti virtuali:

    Sostituire `RESOURCEGROUP` con il nome del gruppo di risorse che contiene la rete virtuale e quindi immettere il comando:

    ```powershell
    $NICs = Get-AzNetworkInterface -ResourceGroupName "RESOURCEGROUP"
    $NICs[0].DnsSettings.InternalDomainNameSuffix
    ```

    ```azurecli
    az network nic list --resource-group RESOURCEGROUP --query "[0].dnsSettings.internalDomainNameSuffix"
    ```

2. Usare il testo seguente come contenuto del file `/etc/bind/named.config.local` nel server DNS personalizzato. Apportare questa modifica al server DNS personalizzato in entrambe le reti virtuali.

    ```
    // Forward requests for the virtual network suffix to Azure recursive resolver
    zone "0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net" {
        type forward;
        forwarders {10.0.0.4;}; # The IP address of the DNS server in the other virtual network
    };
    ```

    Sostituire il valore `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` con il suffisso DNS dell'__altra__ rete virtuale. Questa voce indirizza le richieste per il suffisso DNS della rete remota al DNS personalizzato di quella rete.

3. Nei server DNS personalizzati in entrambe le reti virtuali, usare il testo seguente come contenuto del file `/etc/bind/named.conf.options`:

    ```
    // Clients to accept requests from
    acl goodclients {
        10.1.0.0/16; # The IP address range of one virtual network
        10.0.0.0/16; # The IP address range of the other virtual network
        localhost;
        localnets;
    };

    options {
            directory "/var/cache/bind";

            recursion yes;

            allow-query { goodclients; };

            forwarders {
            168.63.129.16;   # Azure recursive resolver         
            };

            dnssec-validation auto;

            auth-nxdomain no;    # conform to RFC1035
            listen-on { any; };
    };
    ```
    
   Sostituire i valori `10.0.0.0/16` e `10.1.0.0/16` all'interno degli intervalli di indirizzi IP delle reti virtuali. Questa voce consente alle risorse in ogni rete di eseguire richieste dei server DNS.

    Tutte le richieste che non riguardano i suffissi DNS delle reti virtuali (ad esempio microsoft.com) vengono gestite dal sistema di risoluzione ricorsiva di Azure.

4. Per usare la configurazione, riavviare Bind. Ad esempio `sudo service bind9 restart` su entrambi i server DNS.

Dopo avere completato questi passaggi, è possibile connettersi alle risorse nella rete virtuale usando i nomi di dominio completi (FQDN). È ora possibile installare HDInsight nella rete virtuale.

## <a name="next-steps"></a>Passaggi successivi

* Per un esempio completo di configurazione di HDInsight per la connessione a una rete locale, vedere [Connettere HDInsight alla rete locale](./connect-on-premises-network.md).
* Per la configurazione di cluster Apache Hbase nelle reti virtuali di Azure, vedere [Creare cluster Apache HBase in HDInsight nella rete virtuale di Azure](hbase/apache-hbase-provision-vnet.md).
* Per la configurazione della replica geografica di Apache HBase, vedere [Configurare la replica di cluster Apache HBase nelle reti virtuali di Azure](hbase/apache-hbase-replication.md).
* Per altre informazioni sulle reti virtuali di Azure, vedere la [panoramica sulle reti virtuali di Azure](../virtual-network/virtual-networks-overview.md).

* Per altre informazioni sui gruppi di sicurezza di rete, vedere [Gruppi di sicurezza di rete](../virtual-network/security-overview.md).

* Per altre informazioni sulle route definite dall'utente, vedere [Route definite dall'utente e inoltro IP](../virtual-network/virtual-networks-udr-overview.md).
