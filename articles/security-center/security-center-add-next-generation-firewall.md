---
title: Aggiungere un firewall di nuova generazione nel Centro sicurezza di Azure | Microsoft Docs
description: Questo argomento illustra come implementare i consigli **Aggiungi un firewall di nuova generazione** e **Indirizza il traffico solo tramite il firewall di nuova generazione** del Centro sicurezza di Azure.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 48b99015-4db8-4ce8-85e4-b544c0fa203e
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: 731102037b596091b80fbdfa02a8ff3c111b556e
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2019
ms.locfileid: "56105519"
---
# <a name="add-a-next-generation-firewall-in-azure-security-center"></a>Aggiungere un firewall di nuova generazione in Centro sicurezza di Azure
È possibile che il Centro sicurezza di Azure consigli di aggiungere un firewall di nuova generazione di un partner Microsoft per aumentare le impostazioni di sicurezza. In questo documento è riportato un esempio di come eseguire questa operazione.

> [!NOTE]
> Il documento introduce il servizio usando una distribuzione di esempio.  Questa non è una guida dettagliata.
>
>

## <a name="implement-the-recommendation"></a>Implementare la raccomandazione
1. Nel pannello **Raccomandazioni** selezionare **Aggiungi un firewall di nuova generazione**.
   ![Aggiungi un firewall di nuova generazione][1]
2. Nel pannello **Aggiungi un firewall di nuova generazione** selezionare un endpoint.
   ![Selezionare un endpoint][2]
3. Viene visualizzato un secondo pannello **Aggiungi un firewall di nuova generazione**. È possibile scegliere di usare una soluzione esistente eventualmente disponibile o di crearne una nuova. In questo esempio non sono disponibili soluzioni esistenti ed è necessario creare un firewall di nuova generazione.
   ![Creare un firewall di nuova generazione][3]
4. Per creare un firewall di nuova generazione, selezionare una soluzione dall'elenco di partner integrati. In questo esempio si selezionerà **Check Point**.
   ![Selezionare un firewall di nuova generazione][4]
5. Viene visualizzato il pannello **Check Point** , dove sono disponibili informazioni sulla soluzione del partner. Selezionare **Crea** nel pannello informativo.
   ![Pannello di informazioni sul firewall][5]
6. Viene visualizzato il pannello **Crea macchina virtuale**. Qui è possibile immettere le informazioni necessarie per creare una macchina virtuale (VM) che esegua il firewall di nuova generazione. Seguire i passaggi e fornire le informazioni necessarie sul firewall di nuova generazione. Selezionare OK per applicare.
   ![Creare la macchina virtuale per eseguire il firewall di nuova generazione][6]

## <a name="route-traffic-through-ngfw-only"></a>Route traffic through NGFW only (Indirizza il traffico solo tramite il firewall di nuova generazione)
Tornare al pannello **Raccomandazioni** . Dopo l'aggiunta di un firewall di nuova generazione tramite il Centro sicurezza, è stata generata la nuova voce **Indirizza il traffico solo tramite il firewall di nuova generazione**. Questa raccomandazione viene creata solo se il firewall di nuova generazione è installato tramite il Centro sicurezza PC. Se si dispone di endpoint con connessione Internet, il Centro sicurezza consiglia di configurare le regole dei gruppi di sicurezza di rete che indirizzano il traffico in ingresso alla macchina virtuale tramite il firewall di nuova generazione.

1. Nel pannello **Raccomandazioni** selezionare **Indirizza il traffico solo tramite il firewall di nuova generazione**.
   ![Route traffic through NGFW only][7]
2. Verrà visualizzato il pannello **Indirizza il traffico solo tramite il firewall di nuova generazione** in cui sono elencate le macchine virtuali su cui è possibile instradare il traffico. Selezionare una VM dall'elenco.
   ![Selezionare una macchina virtuale][8]
3. Viene visualizzato un pannello per la VM selezionata con le regole in ingresso correlate. Una descrizione fornisce altre informazioni sui possibili passaggi successivi. Selezionare **Modifica le regole in ingresso** per procedere con la modifica di una regola in ingresso. È possibile che **Origine** non sia impostato su **Qualsiasi** per gli endpoint con connessione Internet collegati al firewall di nuova generazione. Per altre informazioni sulle proprietà della regola in ingresso, vedere le [regole di sicurezza](../virtual-network/security-overview.md#security-rules).
   ![Configurare le regole per limitare l'accesso][9]
    ![Modificare la regola in ingresso][10]

## <a name="see-also"></a>Vedere anche 
Questo documento illustra come implementare la raccomandazione "Aggiungi un firewall di nuova generazione" del Centro sicurezza. Per ulteriori informazioni sui firewall di nuova generazione e la soluzione del partner Check Point, vedere gli argomenti seguenti:

* [Firewall di nuova generazione](https://en.wikipedia.org/wiki/Next-Generation_Firewall)
* [Check Point vSEC](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/checkpoint.vsec)

Per altre informazioni sul Centro sicurezza, vedere gli argomenti seguenti:

* [Impostazione dei criteri di sicurezza nel Centro sicurezza di Azure](tutorial-security-policy.md) : informazioni su come configurare i criteri di sicurezza.
* [Gestione delle raccomandazioni di sicurezza nel Centro sicurezza di Azure](security-center-recommendations.md) : informazioni sul modo in cui le raccomandazioni semplificano la protezione delle risorse di Azure.
* [Monitoraggio dell'integrità della sicurezza nel Centro sicurezza di Azure](security-center-monitoring.md) : informazioni su come monitorare l'integrità delle risorse di Azure.
* [Gestione e risposta agli avvisi di sicurezza nel Centro sicurezza di Azure](security-center-managing-and-responding-alerts.md) : informazioni su come gestire e rispondere agli avvisi di sicurezza.
* [Monitoraggio delle soluzioni dei partner con il Centro sicurezza di Azure](security-center-partner-solutions.md) : informazioni su come monitorare lo stato integrità delle soluzioni dei partner.
* [Domande frequenti sul Centro sicurezza di Azure](security-center-faq.md) : domande frequenti sull'uso del servizio.
* [Blog sulla sicurezza di Azure](https://blogs.msdn.com/b/azuresecurity/) : post di blog sulla sicurezza e sulla conformità di Azure.

<!--Image references-->
[1]: ./media/security-center-add-next-gen-firewall/add-next-gen-firewall.png
[2]: ./media/security-center-add-next-gen-firewall/select-an-endpoint.png
[3]: ./media/security-center-add-next-gen-firewall/create-new-next-gen-firewall.png
[4]: ./media/security-center-add-next-gen-firewall/select-next-gen-firewall.png
[5]: ./media/security-center-add-next-gen-firewall/firewall-solution-info-blade.png
[6]: ./media/security-center-add-next-gen-firewall/create-virtual-machine.png
[7]: ./media/security-center-add-next-gen-firewall/route-traffic-through-ngfw.png
[8]: ./media/security-center-add-next-gen-firewall/select-vm.png
[9]: ./media/security-center-add-next-gen-firewall/configure-rules-to-limit-access.png
[10]: ./media/security-center-add-next-gen-firewall/edit-inbound-rule.png
