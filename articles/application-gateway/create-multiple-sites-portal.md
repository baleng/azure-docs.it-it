---
title: Creare un gateway applicazione che ospita più siti Web - Portale di Azure
description: Informazioni su come creare un gateway applicazione che ospita più siti Web usando il portale di Azure.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 2/20/2019
ms.author: victorh
ms.openlocfilehash: 86be94404e7ab492beeebd6a467d23e68e7bce6b
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/18/2019
ms.locfileid: "58080168"
---
# <a name="create-and-configure-an-application-gateway-to-host-multiple-web-sites-using-the-azure-portal"></a>Creare e configurare un gateway applicazione per ospitare più siti Web usando il portale di Azure

È possibile usare il portale di Azure per configurare l'[hosting di più siti Web](multiple-site-overview.md) quando si crea un [gateway applicazione](overview.md). In questo articolo vengono definiti pool di indirizzi back-end con macchine virtuali. e quindi si configurano i listener e le regole in base ai domini di cui si è proprietari per assicurarsi che il traffico Web raggiunga i server appropriati nei pool. Questo articolo presuppone che l'utente sia proprietario di più domini e che usi esempi di *www.contoso.com* e *www.fabrikam.com*.

In questo articolo viene spiegato come:

> [!div class="checklist"]
> * Creare un gateway applicazione
> * Creare macchine virtuali per i server back-end
> * Creare pool back-end con i server back-end
> * Creare i listener back-end
> * Creare le regole di routing
> * Creare un record CNAME nel dominio

![Esempio di routing multisito](./media/create-multiple-sites-portal/scenario.png)

Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare.

## <a name="sign-in-to-azure"></a>Accedere ad Azure

Accedere al portale di Azure all'indirizzo [https://portal.azure.com](https://portal.azure.com)

## <a name="create-an-application-gateway"></a>Creare un gateway applicazione

Per le comunicazioni tra le risorse create è necessaria una rete virtuale. In questo esempio vengono create due subnet: una per il gateway applicazione e l'altra per i server back-end. È possibile creare una rete virtuale durante la creazione del gateway applicazione.

1. Fare clic su **Nuovo** nell'angolo in alto a sinistra nel portale di Azure.
2. Selezionare **Rete** e quindi **Gateway applicazione** nell'elenco In primo piano.
3. Immettere i valori seguenti per il gateway applicazione:

   - *myAppGateway* come nome del gateway applicazione.
   - *myResourceGroupAG* come nuovo gruppo di risorse.

     ![Creare il nuovo gateway applicazione](./media/create-multiple-sites-portal/application-gateway-create.png)

4. Accettare i valori predefiniti per le altre impostazioni e quindi fare clic su **OK**.
5. Fare clic su **Scegliere una rete virtuale**, **Crea nuova** e quindi immettere i valori seguenti per la rete virtuale:

   - *myVNet* come nome della rete virtuale.
   - *10.0.0.0/16* come spazio indirizzi della rete virtuale.
   - *myAGSubnet* come nome della subnet.
   - *10.0.0.0/24* come spazio indirizzi della subnet.

     ![Creare una rete virtuale](./media/create-multiple-sites-portal/application-gateway-vnet.png)

6. Fare clic su **OK** per creare la rete virtuale e la subnet.
7. Fare clic su **Scegliere un indirizzo IP pubblico**, **Crea nuovo** e quindi immettere il nome dell'indirizzo IP pubblico. In questo esempio il nome dell'indirizzo IP pubblico è *myAGPublicIPAddress*. Accettare i valori predefiniti per le altre impostazioni e quindi fare clic su **OK**.
8. Accettare i valori predefiniti per la configurazione del listener, lasciare disabilitato il web application firewall e quindi fare clic su **OK**.
9. Rivedere le impostazioni nella pagina di riepilogo e quindi fare clic su **OK** per creare le risorse di rete e il gateway applicazione. La creazione del gateway applicazione potrebbe richiedere alcuni minuti. Attendere il completamento della distribuzione prima di passare alla sezione successiva.

### <a name="add-a-subnet"></a>Aggiungere una subnet

1. Fare clic su **Tutte le risorse** nel menu a sinistra e quindi su **myVNet** nell'elenco delle risorse.
2. Fare clic su **Subnet** e quindi su **Subnet**.

    ![Creare una subnet](./media/create-multiple-sites-portal/application-gateway-subnet.png)

3. Immettere *myBackendSubnet* come nome della subnet e quindi fare clic su **OK**.

## <a name="create-virtual-machines"></a>Creare macchine virtuali

In questo esempio vengono create due macchine virtuali da usare come server back-end per il gateway applicazione. Viene anche installato IIS nelle macchine virtuali per verificare che il traffico venga instradato correttamente.

1. Fare clic su **Nuovo**.
2. Fare clic su **Calcolo** e quindi selezionare **Windows Server 2016 Datacenter** nell'elenco In primo piano.
3. Immettere i valori seguenti per la macchina virtuale:

    - *contosoVM* come nome della macchina virtuale.
    - *azureuser* come nome utente dell'amministratore.
    - *Azure123456!* come password.
    - Selezionare **Usa esistente** e quindi *myResourceGroupAG*.

4. Fare clic su **OK**.
5. Selezionare **DS1_V2** come dimensioni per la macchina virtuale e fare clic su **Seleziona**.
6. Assicurarsi che **myVNet** sia selezionato per la rete virtuale e che la subnet sia **myBackendSubnet**. 
7. Fare clic su **Disabilitato** per disabilitare la diagnostica di avvio.
8. Fare clic su **OK**, verificare le impostazioni nella pagina di riepilogo e quindi fare clic su **Crea**.

### <a name="install-iis"></a>Installare IIS

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1. Aprire la shell interattiva e assicurarsi che sia impostata su **PowerShell**.

    ![Installare l'estensione personalizzata](./media/create-multiple-sites-portal/application-gateway-extension.png)

2. Eseguire questo comando per installare IIS nella macchina virtuale: 

    ```azurepowershell-interactive
    $publicSettings = @{ "fileUris" = (,"https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/appgatewayurl.ps1");  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File appgatewayurl.ps1" }
    Set-AzVMExtension `
      -ResourceGroupName myResourceGroupAG `
      -Location eastus `
      -ExtensionName IIS `
      -VMName contosoVM `
      -Publisher Microsoft.Compute `
      -ExtensionType CustomScriptExtension `
      -TypeHandlerVersion 1.4 `
      -Settings $publicSettings
    ```

3. Creare la seconda macchina virtuale e installare IIS seguendo la procedura appena completata. Immettere i nomi dei *fabrikamVM* per il nome e per il valore di VMName in Set-AzVMExtension.

## <a name="create-backend-pools-with-the-virtual-machines"></a>Creare pool back-end con le macchine virtuali

1. Fare clic su **Tutte le risorse** e quindi su **myAppGateway**.
2. Fare clic su **Pool back-end** e quindi su **Aggiungi**.
3. Immettere un nome per *contosoPool* e aggiungere *contosoVM* usando **Aggiungi destinazione**.

    ![Aggiungere i server back-end](./media/create-multiple-sites-portal/application-gateway-multisite-backendpool.png)

4. Fare clic su **OK**.
5. Fare clic su **Pool back-end** e quindi su **Aggiungi**.
6. Creare *fabrikamPool* con *fabrikamVM* seguendo i passaggi appena completati.

## <a name="create-backend-listeners"></a>Creare i listener back-end

1. Fare clic su **Listener** e quindi su **Multisito**.
2. Immettere i valori seguenti per il listener:
    
   - *contosoListener*: per il nome del listener.
   - *www.contoso.com*: sostituire questo esempio di nome host con il proprio nome di dominio.

3. Fare clic su **OK**.
4. Creare un secondo listener usando il nome *fabrikamListener* e usare il secondo nome di dominio. In questo esempio viene usato *www.fabrikam.com*.

![listener multisito](media/create-multiple-sites-portal/be-listeners.png)

## <a name="create-routing-rules"></a>Creare le regole di routing

Le regole vengono elaborate nell'ordine in cui sono elencate e il traffico viene indirizzato usando la prima regola corrispondente indipendentemente dalla specificità. Se ad esempio si dispone di due regole, una che usa un listener di base e una che usa un listener multisito, entrambe sulla stessa porta, la regola con il listener multisito deve essere elencata prima della regola con il listener di base per funzionare come previsto. 

In questo esempio si creano due nuove regole e si elimina la regola predefinita che è stata creata al momento della creazione del gateway applicazione. 

1. Fare clic su **Regole** e quindi su **Base**.
2. Immettere *contosoRule* come nome.
3. Selezionare *contosoListener* come listener.
4. Selezionare *contosoPool* come pool back-end.

    ![Creare una regola basata sul percorso](./media/create-multiple-sites-portal/application-gateway-multisite-rule.png)

5. Fare clic su **OK**.
6. Creare una seconda regola usando i nomi *fabrikamRule*, *fabrikamListener* e *fabrikamPool*.
7. Eliminare la regola predefinita denominata *rule1* facendo clic su di essa e quindi su **Elimina**.

## <a name="create-a-cname-record-in-your-domain"></a>Creare un record CNAME nel dominio

Dopo aver creato il gateway applicazione con l'indirizzo IP pubblico, è possibile ottenere l'indirizzo DNS e usarlo per creare un record CNAME nel dominio. Non è consigliabile usare record A perché l'indirizzo VIP può cambiare quando il gateway applicazione viene riavviato.

1. Fare clic su **Tutte le risorse** e quindi su **myAGPublicIPAddress**.

    ![Registrare l'indirizzo DNS del gateway applicazione](./media/create-multiple-sites-portal/application-gateway-multisite-dns.png)

2. Copiare l'indirizzo DNS e usarlo come valore per un nuovo record CNAME nel dominio.

## <a name="test-the-application-gateway"></a>Testare il gateway applicazione

1. Immettere il nome di dominio nella barra degli indirizzi del browser. Ad esempio, http://www.contoso.com .

    ![Testare il sito contoso nel gateway applicazione](./media/create-multiple-sites-portal/application-gateway-iistest.png)

2. Sostituire l'indirizzo con l'altro dominio come nell'esempio seguente:

    ![Testare il sito fabrikam nel gateway applicazione](./media/create-multiple-sites-portal/application-gateway-iistest2.png)

## <a name="next-steps"></a>Passaggi successivi

[Configurare il servizio app con il gateway applicazione](create-web-app.md)