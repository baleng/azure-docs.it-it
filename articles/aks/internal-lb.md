---
title: Creare un bilanciamento del carico interno nel servizio Azure Kubernetes
description: Informazioni su come creare e usare un servizio di bilanciamento del carico interno per esporre i servizi con il servizio Azure Kubernetes.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 03/04/2019
ms.author: iainfou
ms.openlocfilehash: a26eab83f567a46f613e3bfda95fd99aba2b79c0
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2019
ms.locfileid: "57404315"
---
# <a name="use-an-internal-load-balancer-with-azure-kubernetes-service-aks"></a>Usare un servizio di bilanciamento del carico interno con il servizio Azure Kubernetes

Per limitare l'accesso alle applicazioni nel servizio Azure Kubernetes, è possibile creare e usare un bilanciamento del carico interno. Un bilanciamento del carico interno rende accessibile un servizio Kubernetes solo alle applicazioni in esecuzione nella stessa rete virtuale del cluster Kubernetes. Questo articolo descrive come usare un servizio di bilanciamento del carico interno con il servizio Azure Kubernetes.

> [!NOTE]
> Azure Load Balancer è disponibile in due SKU: *Basic* e *Standard*. servizio Azure Kubernetes supporta attualmente la SKU *Basic*. Se si vuole usare la SKU *Standard*, è possibile usare [aks-engine][aks-engine] upstream. Per altre informazioni, vedere [Confronto tra SKU di Load Balancer][azure-lb-comparison].

## <a name="before-you-begin"></a>Prima di iniziare

Questo articolo presuppone che si disponga di un cluster AKS esistente. Se è necessario un cluster servizio Azure Kubernetes, vedere la Guida introduttiva su servizio Azure Kubernetes [Uso dell'interfaccia della riga di comando di Azure][aks-quickstart-cli] oppure [Uso del portale di Azure][aks-quickstart-portal].

Anche necessario la CLI di Azure versione 2.0.59 o versione successiva installato e configurato. Eseguire  `az --version` per trovare la versione. Se è necessario eseguire l'installazione o l'aggiornamento, vedere  [Installare l'interfaccia della riga di comando di Azure][install-azure-cli].

## <a name="create-an-internal-load-balancer"></a>Creare un bilanciamento del carico interno

Per creare un bilanciamento del carico interno, creare un manifesto del servizio denominato `internal-lb.yaml` con il tipo di servizio *LoadBalancer* e l'annotazione *azure-load-balancer-internal* come illustrato nell'esempio seguente:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: internal-app
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: internal-app
```

Distribuire il servizio di bilanciamento di carico interno usando il [kubectl applicare] kubectl-applicare] e specificare il nome del manifesto del YAML:

```console
kubectl apply -f internal-lb.yaml
```

Un servizio di bilanciamento del carico di Azure viene creata nel gruppo di risorse di nodo e connesse alla stessa rete virtuale del cluster AKS.

Quando si visualizzano i dettagli del servizio, l'indirizzo IP del bilanciamento del carico interno viene visualizzato nella colonna *EXTERNAL-IP*. In questo contesto *esterno* in relazione all'interfaccia esterna del servizio di bilanciamento del carico, non è che riceva un indirizzo IP pubblico, esterno. Potrebbero essere richiesti un paio di minuti affinché l'indirizzo IP diventi un effettivo indirizzo IP interno dallo stato *\<in sospeso\>*, come mostrato nell'esempio seguente:

```
$ kubectl get service internal-app

NAME           TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
internal-app   LoadBalancer   10.0.248.59   10.240.0.7    80:30555/TCP   2m
```

## <a name="specify-an-ip-address"></a>Specificare un indirizzo IP

Se si vuole usare un indirizzo IP specifico con il bilanciamento del carico interno, aggiungere la proprietà *loadBalancerIP* nel manifesto YAML del bilanciamento del carico. L'indirizzo IP specificato deve trovarsi nella stessa subnet del cluster del servizio Azure Container e non deve essere già assegnato a una risorsa.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: internal-app
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
spec:
  type: LoadBalancer
  loadBalancerIP: 10.240.0.25
  ports:
  - port: 80
  selector:
    app: internal-app
```

Quando distribuito e si visualizzano i dettagli di servizio, l'indirizzo IP nel *EXTERNAL-IP* colonna riflette l'indirizzo IP specificato:

```
$ kubectl get service internal-app

NAME           TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
internal-app   LoadBalancer   10.0.184.168   10.240.0.25   80:30225/TCP   4m
```

## <a name="use-private-networks"></a>Usare le reti private

Quando si crea il cluster servizio Azure Kubernetes è possibile specificare impostazioni di rete avanzate. Questo approccio consente di distribuire il cluster in una rete virtuale di Azure esistente e nelle subnet. Uno scenario consiste nella distribuzione del cluster servizio Azure Kubernetes in una rete privata connessa all'ambiente locale e nell'esecuzione dei servizi accessibili solo internamente. Per altre informazioni, vedere Configurare proprie subnet della rete virtuale con [Kubenet] [ use-kubenet] oppure [Azure CNI][advanced-networking].

Non sono necessarie modifiche dei passaggi precedenti per distribuire un servizio di bilanciamento del carico interno in un cluster servizio Azure Kubernetes che usa una rete privata. Il servizio di bilanciamento del carico viene creato nello stesso gruppo di risorse del cluster servizio Azure Kubernetes ma è connesso alla rete virtuale e alla subnet private, come illustrato nell'esempio seguente:

```
$ kubectl get service internal-app

NAME           TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
internal-app   LoadBalancer   10.1.15.188   10.0.0.35     80:31669/TCP   1m
```

> [!NOTE]
> Può essere necessario concedere all'entità servizio per il cluster servizio Azure Kubernetes il ruolo *Collaboratore di rete* per il gruppo di risorse in cui vengono distribuite le risorse della rete virtuale di Azure. Visualizzare l'entità servizio con [az servizio Azure Kubernetes show][az-aks-show], ad esempio `az aks show --resource-group myResourceGroup --name myAKSCluster --query "servicePrincipalProfile.clientId"`. Per creare un'assegnazione di ruolo, usare il comando [az role assignment create][az-role-assignment-create].

## <a name="specify-a-different-subnet"></a>Specificare una subnet diversa

Per specificare una subnet per il bilanciamento del carico, aggiungere l'annotazione *azure-load-balancer-internal-subnet* al servizio. La subnet specificata deve essere nella stessa rete virtuale del cluster servizio Azure Kubernetes. Quando viene distribuito, l'indirizzo *EXTERNAL-IP* del servizio di bilanciamento del carico fa parte della subnet specificata.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: internal-app
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
    service.beta.kubernetes.io/azure-load-balancer-internal-subnet: "apps-subnet"
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: internal-app
```

## <a name="delete-the-load-balancer"></a>Eliminare il servizio di bilanciamento del carico

Quando vengono eliminati tutti i servizi che usano il servizio di bilanciamento del carico interno, viene eliminato anche tale servizio.

È anche possibile eliminare direttamente un servizio come per qualsiasi risorsa di Kubernetes, ad esempio `kubectl delete service internal-app`, operazione che comporta anche l'eliminazione del bilanciamento del carico sottostante.

## <a name="next-steps"></a>Passaggi successivi

Altre informazioni sui servizi Kubernetes sono disponibili nella [documentazione relativa ai servizi Kubernetes][kubernetes-services].

<!-- LINKS - External -->
[kubernetes-services]: https://kubernetes.io/docs/concepts/services-networking/service/
[aks-engine]: https://github.com/Azure/aks-engine

<!-- LINKS - Internal -->
[advanced-networking]: configure-azure-cni.md
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[azure-lb-comparison]: ../load-balancer/load-balancer-overview.md#skus
[use-kubenet]: configure-kubenet.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli