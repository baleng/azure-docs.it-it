---
title: Eseguire l'autenticazione con Registro Azure Container dal servizio Azure Kubernetes
description: Informazioni su come fornire l'accesso alle immagini nel registro contenitori privato dal servizio Azure Kubernetes usando un'entità servizio di Azure Active Directory.
services: container-service
author: dlepow
ms.service: container-service
ms.topic: article
ms.date: 08/08/2018
ms.author: danlep
ms.openlocfilehash: 1d7e130d619f580aeb82939e19ea5abf680ff039
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/16/2019
ms.locfileid: "56326477"
---
# <a name="authenticate-with-azure-container-registry-from-azure-kubernetes-service"></a>Eseguire l'autenticazione con Registro Azure Container dal servizio Azure Kubernetes

Quando si usa Registro Azure Container con il servizio Azure Kubernetes, è necessario definire un meccanismo di autenticazione. Questo articolo illustra in dettaglio le configurazioni consigliate per l'autenticazione tra questi due servizi di Azure.

Questo articolo presuppone che sia già stato creato un cluster AKS e che si sia in grado di accedere al cluster con il client da riga di comando `kubectl`. 

## <a name="grant-aks-access-to-acr"></a>Concedere al servizio Azure Container l'accesso a Registro Azure Container

Quando si crea un cluster servizio Azure Kubernetes, Azure crea anche un'entità servizio per supportare il funzionamento del cluster con altre risorse di Azure. Questa entità servizio generata automaticamente può essere usata anche per l'autenticazione con un record di controllo di accesso. A tale scopo, è necessario creare un'[assegnazione di ruolo](../role-based-access-control/overview.md#role-assignments) di Azure Active Directory che conceda all'entità servizio del cluster l'accesso al registro contenitori.

Usare lo script seguente per concedere all'entità servizio generata da AKS l'accesso pull a un registro contenitori di Azure. Modificare le variabili `AKS_*` e `ACR_*` per l'ambiente prima di eseguire lo script.

```bash
#!/bin/bash

AKS_RESOURCE_GROUP=myAKSResourceGroup
AKS_CLUSTER_NAME=myAKSCluster
ACR_RESOURCE_GROUP=myACRResourceGroup
ACR_NAME=myACRRegistry

# Get the id of the service principal configured for AKS
CLIENT_ID=$(az aks show --resource-group $AKS_RESOURCE_GROUP --name $AKS_CLUSTER_NAME --query "servicePrincipalProfile.clientId" --output tsv)

# Get the ACR registry resource id
ACR_ID=$(az acr show --name $ACR_NAME --resource-group $ACR_RESOURCE_GROUP --query "id" --output tsv)

# Create role assignment
az role assignment create --assignee $CLIENT_ID --role acrpull --scope $ACR_ID
```

## <a name="access-with-kubernetes-secret"></a>Accedere mediante un segreto Kubernetes

In alcuni casi, potrebbe non essere possibile assegnare i ruoli necessari all'entità servizio Azure Kubernetes generata automaticamente per garantire l'accesso ad ACR. Ad esempio, a causa del modello di sicurezza dell'organizzazione, si potrebbe non disporre delle autorizzazioni sufficienti nella directory di Azure Active Directory per assegnare un ruolo all'entità servizio generata da servizio Azure Kubernetes. L'assegnazione di un ruolo a un'entità servizio richiede l'account di Azure AD per disporre dell'autorizzazione di scrittura al tenant di Azure AD. Se non si dispone dell'autorizzazione, è possibile creare una nuova entità servizio, quindi concedere l'accesso al registro contenitori tramite un segreto Kubernetes per il pull immagine.

Usare lo script seguente per creare una nuova entità servizio (si userà le credenziali per il segreto Kubernetes per il pull delle immagini). Modificare la variabile `ACR_NAME` per l'ambiente prima di eseguire lo script.

```bash
#!/bin/bash

ACR_NAME=myacrinstance
SERVICE_PRINCIPAL_NAME=acr-service-principal

# Populate the ACR login server and resource id.
ACR_LOGIN_SERVER=$(az acr show --name $ACR_NAME --query loginServer --output tsv)
ACR_REGISTRY_ID=$(az acr show --name $ACR_NAME --query id --output tsv)

# Create acrpull role assignment with a scope of the ACR resource.
SP_PASSWD=$(az ad sp create-for-rbac --name http://$SERVICE_PRINCIPAL_NAME --role acrpull --scopes $ACR_REGISTRY_ID --query password --output tsv)

# Get the service principal client id.
CLIENT_ID=$(az ad sp show --id http://$SERVICE_PRINCIPAL_NAME --query appId --output tsv)

# Output used when creating Kubernetes secret.
echo "Service principal ID: $CLIENT_ID"
echo "Service principal password: $SP_PASSWD"
```

Le credenziali dell'entità servizio possono ora essere archiviate in un [segreto per il pull delle immagini][image-pull-secret] Kubernetes a cui il cluster servizio Azure Kubernetes farà riferimento durante l'esecuzione di contenitori.

Usare il comando **kubectl** seguente per creare un segreto Kubernetes. Sostituire `<acr-login-server>` con il nome completo di Registro Azure Container (con formato "acrname.azurecr.io"). Sostituire `<service-principal-ID>` e `<service-principal-password>` con i valori ottenuti dall'esecuzione dello script precedente. Sostituire `<email-address>` con qualsiasi indirizzo di posta elettronica valido.

```bash
kubectl create secret docker-registry acr-auth --docker-server <acr-login-server> --docker-username <service-principal-ID> --docker-password <service-principal-password> --docker-email <email-address>
```

È ora possibile usare il segreto Kubernetes nelle distribuzioni di pod specificandone il nome (in questo caso, "acr-auth") nel parametro `imagePullSecrets`:

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: acr-auth-example
spec:
  template:
    metadata:
      labels:
        app: acr-auth-example
    spec:
      containers:
      - name: acr-auth-example
        image: myacrregistry.azurecr.io/acr-auth-example
      imagePullSecrets:
      - name: acr-auth
```

<!-- LINKS - external -->
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[image-pull-secret]: https://kubernetes.io/docs/concepts/configuration/secret/#using-imagepullsecrets
