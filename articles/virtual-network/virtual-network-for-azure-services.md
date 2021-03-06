---
title: Rete virtuale per servizi di Azure
titlesuffix: Azure Virtual Network
description: Informazioni sui vantaggi della distribuzione delle risorse in una rete virtuale. Le risorse nelle reti virtuali possono comunicare tra loro e con le risorse locali senza che il traffico attraversi Internet.
services: virtual-network
documentationcenter: na
author: malopMSFT
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: malop;kumud
ms.openlocfilehash: c9b2f7244731be67628776b032e041457900353c
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/03/2019
ms.locfileid: "58886421"
---
# <a name="virtual-network-integration-for-azure-services"></a>Integrazione della rete virtuale per i servizi di Azure

L'integrazione dei servizi di Azure in una rete virtuale di Azure consente di accedere privatamente ai servizi dalle macchine virtuali o da risorse di calcolo nella rete virtuale.
È possibile integrare i servizi di Azure nella rete virtuale con le opzioni seguenti:
- Distribuendo istanze dedicate del servizio in una rete virtuale. I servizi sono accessibili privatamente all'interno della rete virtuale e da reti locali.
- Estendendo una rete virtuale al servizio, tramite endpoint di servizio che consentono la protezione delle singole risorse del servizio nella rete virtuale.

Per integrare più servizi di Azure nella rete virtuale, è possibile combinare uno o più dei criteri precedenti. Ad esempio, è possibile distribuire HDInsight nella rete virtuale e integrare in modo sicuro un account di archiviazione nella subnet di HDInsight tramite gli endpoint di servizio.
 
## <a name="deploy-azure-services-into-virtual-networks"></a>Distribuire servizi di Azure in reti virtuali

Quando si distribuiscono i servizi di Azure in una [rete virtuale](virtual-networks-overview.md), è possibile comunicare con le risorse dei servizi privatamente, tramite indirizzi IP privati.

![Servizi distribuiti in una rete virtuale](./media/virtual-network-for-azure-services/deploy-service-into-vnet.png)

La distribuzione di servizi all'interno di una rete virtuale offre le funzionalità seguenti:

- Le risorse all'interno della rete virtuale possono comunicare tra loro privatamente, tramite indirizzi IP privati, ad esempio trasferendo direttamente i dati tra HDInsight e SQL Server in esecuzione in una macchina virtuale, nella rete virtuale.
- Le risorse locali possono accedere alle risorse in una rete virtuale usando indirizzi IP privati su una [VPN da sito a sito (gateway VPN)](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti) o [ExpressRoute](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Le reti virtuali possono essere [associate tramite peering](virtual-network-peering-overview.md) per consentire alle risorse delle reti virtuali di comunicare tra loro, usando indirizzi IP privati.
- Le istanze dei servizi in una rete virtuale sono completamente gestite dal servizio di Azure, per monitorare l'integrità delle istanze e garantire la scalabilità richiesta in base al carico.
- Le istanze dei servizi vengono distribuite in una subnet in una rete virtuale. L'accesso alla rete in ingresso e in uscita deve essere consentito tramite [gruppi di sicurezza di rete](security-overview.md#network-security-groups) per la subnet, in base alle indicazioni fornite dai servizi.
- Facoltativamente, i servizi potrebbero richiedere una [subnet delegata](virtual-network-manage-subnet.md#add-a-subnet) come identificatore esplicito del fatto che una subnet possa ospitare un servizio specifico. La delega della subnet offre al servizio autorizzazioni esplicite per creare le risorse specifiche del servizio nella subnet.

### <a name="services-that-can-be-deployed-into-a-virtual-network"></a>Servizi distribuibili in una rete virtuale

|Categoria|Service|
|-|-|
| Calcolo | Macchine virtuali: [Linux](../virtual-machines/linux/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json) o [Windows](../virtual-machines/windows/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[set di scalabilità di macchine virtuali](../virtual-machine-scale-sets/virtual-machine-scale-sets-mvss-existing-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Servizio cloud](https://msdn.microsoft.com/library/azure/jj156091): solo rete virtuale (classico)<br/> [Azure Batch](../batch/batch-api-basics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual-network-vnet-and-firewall-configuration)  |
| Rete | [Gateway applicazione - WAF](../application-gateway/application-gateway-ilb-arm.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Gateway VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Firewall di Azure](../firewall/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) <br/>[Appliance virtuali di rete](/windows-server/networking/sdn/manage/use-network-virtual-appliances-on-a-vn) 
|Dati|[RedisCache](../azure-cache-for-redis/cache-how-to-premium-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Istanza gestita di database SQL di Azure](../sql-database/sql-database-managed-instance-connectivity-architecture.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
Analytics | [HDInsight di Azure](../hdinsight/hdinsight-extend-hadoop-virtual-network.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure Databricks](../azure-databricks/what-is-azure-databricks.md?toc=%2fazure%2fvirtual-network%2ftoc.json) |
| Identità | [Servizi di dominio Azure Active Directory](../active-directory-domain-services/active-directory-ds-getting-started-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json) |
| Contenitori | [Servizio Azure Kubernetes](../aks/concepts-network.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Istanza di contenitore di Azure (ACI)](https://www.aka.ms/acivnet)<br/>[Motore del servizio Azure Container](https://github.com/Azure/acs-engine) con il [plug-in](https://github.com/Azure/acs-engine/tree/master/examples/vnet) CNI della Rete virtuale di Azure|
| Web | [Gestione API](../api-management/api-management-using-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Ambiente del servizio app](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>|
| Ospitato | [Modulo di protezione hardware dedicato di Azure](../dedicated-hsm/index.yml?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure NetApp Files](../azure-netapp-files/azure-netapp-files-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>|
|||



## <a name="service-endpoints-for-azure-services"></a>Endpoint di servizio per i servizi di Azure

Alcuni servizi di Azure non possono essere distribuiti nelle reti virtuali. È possibile limitare l'accesso ad alcune delle risorse dei servizi solo a subnet specifiche della rete virtuale, se si vuole, abilitando un endpoint del servizio di rete virtuale.  Vedere altre informazioni sugli [endpoint del servizio di rete virtuale](virtual-network-service-endpoints-overview.md) e sui servizi per i quali è possibile abilitare gli endpoint.
