---
title: Esercitazione - Creare un tenant di Azure Active Directory B2C | Microsoft Docs
description: Informazioni su come preparare la registrazione delle applicazioni creando un tenant di Azure Active Directory B2C con il portale di Azure.
services: B2C
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 01/11/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: b8878eccb079bf78c45ff9c1e4040659d109b1ab
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55152927"
---
# <a name="tutorial-create-an-azure-active-directory-b2c-tenant"></a>Esercitazione: Creare un tenant di Azure Active Directory B2C

Prima che le applicazioni possano interagire con Azure Active Directory (Azure AD) B2C, devono essere registrate in un tenant gestito.

In questo articolo viene spiegato come:

> [!div class="checklist"]
> * Creare un tenant di Azure AD B2C
> * Collegare il tenant alla sottoscrizione

La registrazione di un'applicazione verrà illustrata nell'esercitazione successiva.

Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare.

## <a name="create-an-azure-ad-b2c-tenant"></a>Creare un tenant di Azure AD B2C

1. Accedere al [portale di Azure](https://portal.azure.com/).
2. Assicurarsi di usare la directory che contiene la sottoscrizione facendo clic sul **filtro delle directory e delle sottoscrizioni** nel menu in alto e scegliere la directory che contiene il tenant. Si tratta di una directory diversa rispetto a quella che conterrà il tenant Azure AD B2C.

    ![Passare alla directory della sottoscrizione](./media/tutorial-create-tenant/switch-directory-subscription.png)

3. Scegliere **Crea una risorsa** nell'angolo in alto a sinistra del portale di Azure.
4. Cercare e selezionare **Active Directory B2C** e quindi fare clic su **Crea**.
5. Scegliere **Crea un nuovo tenant Azure AD B2C**, immettere un nome di organizzazione e il nome di dominio iniziale, che viene usato nel nome del tenant, selezionare il paese (non può essere modificato in seguito) e quindi fare clic su **Crea**.

    ![Creare un tenant](./media/tutorial-create-tenant/create-tenant.png)

    In questo esempio il nome del tenant è contoso0926Tenant.onmicrosoft.com

6. Nella pagina **Creare un nuovo tenant B2C o collegarne uno esistente**, selezionare **Collega un tenant Azure AD B2C esistente alla sottoscrizione di Azure**, selezionare il tenant creato, selezionare la sottoscrizione e fare clic su **Crea nuovo**.
7. Immettere un nome per il gruppo di risorse che conterrà il tenant, selezionare la località e quindi fare clic su **Crea**.
8. Per iniziare a usare il nuovo tenant, assicurarsi di usare la directory che contiene il tenant di Azure AD B2C. A tale scopo, fare clic sul **filtro delle directory e delle sottoscrizioni** nel menu in alto e scegliere la directory che lo contiene.

    ![Passare alla directory del tenant](./media/tutorial-create-tenant/switch-directories.png)

## <a name="next-steps"></a>Passaggi successivi

In questo articolo si è appreso come:

> [!div class="checklist"]
> * Creare un tenant di Azure AD B2C
> * Collegare il tenant alla sottoscrizione

> [!div class="nextstepaction"]
> [Registrare le applicazioni](tutorial-register-applications.md)
