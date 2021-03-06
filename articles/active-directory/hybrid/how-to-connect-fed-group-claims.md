---
title: Configurare le attestazioni di gruppo per le applicazioni con Azure Active Directory | Microsoft Docs
description: Informazioni su come configurare le attestazioni di gruppo per l'uso con Azure AD.
services: active-directory
documentationcenter: ''
ms.reviewer: paulgarn
manager: daveba
ms.component: hybrid
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 02/27/2019
ms.author: billmath
author: billmath
ms.openlocfilehash: 622a3ce0f80bd09bd09fa7ff097f68155318142d
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2019
ms.locfileid: "58080357"
---
# <a name="configure-group-claims-for-applications-with-azure-active-directory-public-preview"></a>Configurare le attestazioni di gruppo per le applicazioni con Azure Active Directory (anteprima pubblica)

Azure Active Directory possono fornire un informazioni di appartenenza al gruppo utenti nei token per l'uso all'interno delle applicazioni.  Sono supportati due modelli principali:

- Gruppi identificati dal proprio identificatore di oggetto (OID) (disponibile a livello generale) di Azure Active Directory
- Gruppi identificati tramite l'attributo SAMAccountName o GroupSID Active Directory (AD) sincronizzata gruppi e utenti (anteprima pubblica)

> [!Note]
> Supporto per l'uso in locale e i nomi di identificatori di sicurezza (SID) è progettato per consentire lo spostamento delle applicazioni esistenti da AD FS.    I gruppi gestiti in Azure AD non contengono gli attributi necessari per generare queste attestazioni.

## <a name="group-claims-for-applications-migrating-from-ad-fs-and-other-idps"></a>Attestazioni di gruppo per le applicazioni eseguono la migrazione da AD FS e altri provider di identità

Molte applicazioni che sono configurate per l'autenticazione con AD FS si basano su informazioni di appartenenza sotto forma di attributi del gruppo di Windows Active Directory.   Questi attributi sono SAMAccountName, che può essere qualificato dal dominio nome gruppo o il SID di gruppo di Windows.  Quando l'applicazione è federata con AD FS, ADFS Usa la funzione TokenGroups per recuperare l'appartenenza ai gruppi dell'utente.

Per trovare il token di un'app riceve da AD FS, possono essere inviate le attestazioni di ruolo e gruppo che contiene il dominio completo SAMAccountName anziché objectID di Azure Active Directory al gruppo.

I formati supportati per le attestazioni di gruppo sono:

- **Azure GroupObjectId Directory attiva** (disponibile per tutti i gruppi)
- **SAMAccountName** (disponibile per i gruppi sincronizzati da Active Directory)
- **NetbiosDomain\samAccountName** (disponibile per i gruppi sincronizzati da Active Directory)
- **DNSDomainName\samAccountName** (disponibile per i gruppi sincronizzati da Active Directory)

> [!NOTE]
> Attributi SAMAccountName e OnPremisesGroupSID disponibili solo per gli oggetti gruppo sincronizzati da Active Directory.   Non sono disponibili i gruppi creati in Azure Active Directory o Office 365.   Le applicazioni che dipendono da attributi di gruppo locali consente di ottenerli sincronizzati solo per i gruppi.

## <a name="options-for-applications-to-consume-group-information"></a>Opzioni per le applicazioni di utilizzare le informazioni sui gruppi

Un modo per le applicazioni ottenere informazioni sul gruppo consiste nel chiamare l'endpoint di gruppi di Graph per recuperare l'appartenenza al gruppo per l'utente autenticato. Questa chiamata assicura che tutti i gruppi di che un utente è membro di sono disponibili anche quando sono coinvolti un numero elevato di gruppi e l'applicazione deve enumerare l'utente è membro di tutti i gruppi.  Enumerazione di gruppo è quindi indipendente delle limitazioni delle dimensioni del token.

Tuttavia, se un'applicazione esistente già prevede di utilizzare le informazioni di gruppo tramite attestazioni nel token ricevuto, Azure Active Directory possono essere configurate con un numero di opzioni di attestazioni diverse a seconda delle esigenze dell'applicazione.  Valutare le opzioni seguenti:

- Quando si usa l'appartenenza al gruppo per uno scopo di autorizzazione dell'applicazione (se l'appartenenza al gruppo viene ottenuto dal token o dal grafico), è preferibile usare l'ObjectID del gruppo, ovvero non modificabile e univoco in Azure Active Directory e rese disponibili per tutti i gruppi .
- Se si usa il gruppo di SAMAccountName per l'autorizzazione, usare nomi di dominio completo.  ha meno probabilità di situazioni derivanti esistevano conflitto di nomi. Attributo SAMAccountName in sé può essere univoco all'interno di un dominio Active Directory, ma se più di un dominio di Active Directory è sincronizzato con un tenant di Azure Active Directory prevede la possibilità per più di un gruppo che lo stesso nome.
- È consigliabile usare [i ruoli applicazione](../../active-directory/develop/howto-add-app-roles-in-azure-ad-apps.md) per fornire un livello di riferimento indiretto tra l'appartenenza al gruppo e l'applicazione.   L'applicazione quindi prende decisioni di autorizzazione interno basate su veneridi ruolo nel token.
- Se l'applicazione è configurata per ottenere gli attributi gruppo sincronizzati da Active Directory e un gruppo non contiene gli attributi che non sarà incluso nelle attestazioni.
- Gruppo attestazioni nei token includono gruppi nidificati.   Se un utente è membro del gruppo b indicante e gruppo b indicante è un membro di GroupA, le attestazioni di gruppo per l'utente conterrà GroupA sia gruppo b indicante. Per le organizzazioni con un utilizzo intenso di gruppi nidificati e gli utenti con un numero elevato di appartenenze il numero di gruppi elencate nel token può aumentare le dimensioni del token.   Azure Active Directory limita il numero di gruppi, verrà generato un token per le asserzioni SAML 150 e 200 per JWT.

> Prerequisiti per l'uso degli attributi gruppo sincronizzati da Active Directory:   I gruppi devono essere sincronizzati da Active Directory con Azure AD Connect.

Esistono due passaggi per la configurazione di Azure Active Directory per generare i nomi dei gruppi per gruppi di Active Directory.

1. **Sincronizzare i nomi dei gruppi da Active Directory** prima di Azure Active Directory possono generare i nomi dei gruppi o nel gruppo locale attestazioni SID di gruppo o del ruolo, gli attributi obbligatori devono essere sincronizzati da Active Directory.  È necessario eseguire Azure AD Connect versione 1.2.70 o versione successiva.   Prima della versione 1.2.70 Azure AD Connect sincronizzerà gli oggetti gruppo th da Active Directory, ma non include gli attributi di nome di gruppo necessari per impostazione predefinita.  È consigliabile aggiornare alla versione corrente.

2. **Configurare la registrazione dell'applicazione in Azure Active Directory per includere le attestazioni di gruppo nei token** attestazioni di gruppo possono essere configurato uno nella sezione applicazioni aziendali del portale per un'applicazione della raccolta o Non nella raccolta di SAML SSO, o tramite il manifesto dell'applicazione nella sezione registrazioni per l'applicazione.  Per configurare le attestazioni di gruppo nell'applicazione manifesto vedere "Configurazione di registrazione Active Directory dell'applicazione Azure per gli attributi del gruppo" sotto.

## <a name="configure-group-claims-for-saml-applications-using-sso-configuration"></a>Configurare le attestazioni di gruppo per le applicazioni SAML utilizzando la configurazione di SSO

Per configurare le attestazioni di gruppo per un'applicazione della raccolta o SAML Non nella raccolta, aprire le applicazioni aziendali, fare clic sull'applicazione nell'elenco e selezionare configurazione Single Sign-On.

Selezionare l'icona di modifica accanto a "Gruppi restituite nel token"

![attestazioni dell'interfaccia utente](media/how-to-connect-fed-group-claims/group-claims-ui-1.png)

Usare i pulsanti di opzione per selezionare i gruppi a cui devono essere incluse nel token

![attestazioni dell'interfaccia utente](media/how-to-connect-fed-group-claims/group-claims-ui-2.png)

| Selezione | DESCRIZIONE |
|----------|-------------|
| **Tutti i gruppi** | Genera i gruppi di sicurezza e distribuzione sono elencati.   Determina anche i ruoli della Directory viene assegnato all'utente devono essere create in un'attestazione 'wids' e tutti i ruoli dell'applicazione che all'utente viene assegnata da generare nell'attestazione ruoli. |
| **Gruppi di sicurezza** | Crea i gruppi di sicurezza, che l'utente è membro nell'attestazione relativa ai gruppi |
| **Lista di distribuzione** | Genera liste di distribuzione per di che l'utente è membro |
| **Ruolo della directory** | Se l'utente sono assegnati i ruoli della directory mano che vengono generati come un 'wids' attestazione (attestazione non verrà emesso i gruppi) |

Ad esempio, per emettere l'utente è membro di tutti i gruppi di sicurezza, selezionare i gruppi di sicurezza

![attestazioni dell'interfaccia utente](media/how-to-connect-fed-group-claims/group-claims-ui-3.png)

### <a name="advanced-options"></a>Advanced Options

Il modo in cui vengono generate le attestazioni di gruppo può essere modificato dalle impostazioni in Opzioni avanzate

Personalizzare il nome dell'attestazione gruppo:  Se selezionata, è possibile specificare un tipo di attestazione diversi per le attestazioni di gruppo.   Immettere il tipo di attestazione nel campo nome e lo spazio dei nomi facoltativo per l'attestazione nel campo dello spazio dei nomi.

![attestazioni dell'interfaccia utente](media/how-to-connect-fed-group-claims/group-claims-ui-4.png)

Per creare i gruppi di Active Directory usando attributi invece ObjectID di Azure AD selezionare la casella 'Return gruppi come nomi anziché gli ID' e selezionare il formato dall'elenco a discesa.  Questo strumento sostituisce l'ID di oggetto nelle attestazioni con i valori di stringa che contiene i nomi dei gruppi.   Verranno inclusi solo i gruppi sincronizzati da Active Directory nelle attestazioni.

![attestazioni dell'interfaccia utente](media/how-to-connect-fed-group-claims/group-claims-ui-5.png)

Alcune applicazioni richiedono le informazioni di appartenenza di gruppo venga visualizzato nell'attestazione 'ruolo'. È facoltativamente possibile trasmettere i gruppi dell'utente come ruoli selezionando la casella 'Emettere i gruppi di che un ruolo di attestazioni'.

![attestazioni dell'interfaccia utente](media/how-to-connect-fed-group-claims/group-claims-ui-6.png)

> [!NOTE]
> Se viene utilizzata l'opzione per generare dati di gruppo come ruoli, verranno visualizzato solo i gruppi nell'attestazione ruoli.  Tutti i ruoli applicazione che viene assegnato l'utente non verrà visualizzata nell'attestazione ruoli.

## <a name="configure-the-azure-ad-application-registration-for-group-attributes"></a>Configurare la registrazione di applicazione Azure AD per gli attributi di gruppo

Attestazioni di gruppo possono anche essere configurate nel [attestazioni facoltative](../../active-directory/develop/active-directory-optional-claims.md) sezione il [manifesto dell'applicazione](../../active-directory/develop/reference-app-manifest.md).

1. Nel portale -> Azure Active Directory -> applicazione le registrazioni -> selezionare applicazione -> manifesto

2. Abilitare le attestazioni di appartenenza al gruppo modificando il groupMembershipClaim

   I valori validi sono:

   - "Tutti"
   - "SecurityGroup"
   - "DistributionList"
   - "DirectoryRole"

   Ad esempio: 

   ```json
   "groupMembershipClaims": "SecurityGroup"
   ```

   Per impostazione predefinita che verranno emesse ObjectIDs gruppo nel gruppo valore di attestazione.  Per modificare il valore dell'attestazione per contenere gli attributi di gruppo locale o per modificare il tipo di attestazione ruolo, usare configurazione OptionalClaims come segue:

3. Set di attestazioni facoltative configurazione nome di gruppo.

   Se si desidera gruppi nel token per contenere gli attributi di gruppo AD nella sezione attestazioni facoltative specificano quale attestazione facoltativa di tipo di token deve essere applicato a locale, il nome dell'attestazione facoltativa richiesto ed eventuali proprietà aggiuntive desiderate.  È possibile specificare più tipi di token:

   - idToken per il token ID OIDC
   - per il token di accesso di OAuth/OIDC accessToken
   - Saml2Token per i token SAML.

   > [!NOTE]
   > Il tipo Saml2Token si applica SAML1.1 sia SAML2.0 i token di formato

   Per ogni tipo di token pertinente, modificare l'attestazione relativa ai gruppi per usare la sezione OptionalClaims nel manifesto. Come indicato di seguito è riportato lo schema OptionalClaims:

   ```json
   {
   "name": "groups",
   "source": null,
   "essential": false,
   "additionalProperties": []
   }
   ```

   | Schema di attestazioni facoltative | Valore |
   |----------|-------------|
   | **Nome:** | Deve essere "groups" |
   | **source:** | Non usato. Omettere o specificare null |
   | **essential:** | Non usato. Omettere oppure specificare false |
   | **additionalProperties:** | Elenco di proprietà aggiuntive.  Le opzioni valide sono "sam_account_name", "dns_domain_and_sam_account_name", "netbios_domain_and_sam_account_name", "emit_as_roles" |

   In additionalProperties solo uno dei "sam_account_name", "dns_domain_and_sam_account_name", "netbios_domain_and_sam_account_name" sono obbligatori.  Se è presentano più di uno, viene usato il primo e tutte le altre ignorato.

   Alcune applicazioni richiedono informazioni sul gruppo sull'utente nell'attestazione basata su ruolo.  Per modificare il tipo di attestazione da un gruppo a un'attestazione di ruolo, aggiungere "emit_as_roles" alle proprietà aggiuntive.  I valori del gruppo verranno generati nell'attestazione basata su ruolo.

   > [!NOTE]
   > Se viene usato "emit_as_roles" tutti i ruoli applicazione configurato che l'utente sia assegnata will non visualizzata nell'attestazione basata su ruolo

### <a name="examples"></a>Esempi

Creare i gruppi come i nomi dei gruppi nei token di accesso di OAuth nel formato dnsDomainName\SAMAccountName

```json
"optionalClaims": {
    "accessToken": [{
        "name": "groups",
        "additionalProperties": ["dns_domain_and_sam_account_name"]
    }]
}
 ```

Per generare nomi di gruppo venga restituito nel formato netbiosDomain\samAccountName come i ruoli di attestazione nel token ID di OIDC e SAML:

```json
"optionalClaims": {
    "saml2Token": [{
        "name": "groups",
        "additionalProperties": ["netbios_name_and_sam_account_name", "emit_as_roles"]
    }],

    "idToken": [{
        "name": "groups",
        "additionalProperties": ["netbios_name_and_sam_account_name", "emit_as_roles"]
    }]
 }
 ```

## <a name="next-steps"></a>Passaggi successivi

[Informazioni sull'identità ibrida](whatis-hybrid-identity.md)