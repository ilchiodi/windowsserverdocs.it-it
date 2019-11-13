---
ms.assetid: 934ac796-e2ee-490d-8265-6a818be5ee79
title: Gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni con esigenze particolari
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 79319f54ceb14195dffd56b5a4dfe1b17f048df9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407526"
---
# <a name="manage-risk-with-additional-multi-factor-authentication-for-sensitive-applications"></a>Gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni con esigenze particolari




-   [Configurare l'ambiente lab per AD FS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)

-   [Guida allo scenario: gestire i rischi con ulteriori Multi-Factor Authentication per le applicazioni sensibili](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

-   [Configurare metodi di autenticazione aggiuntivi per AD FS](../../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md)

## <a name="in-this-guide"></a>Contenuto della guida
In questa guida sono disponibili le informazioni seguenti:

-   [Meccanismi di autenticazione in ad FS](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_1) Descrizione dei meccanismi di autenticazione disponibili in Active Directory Federation Services (ad FS) in Windows Server 2012 R2

-   [Panoramica dello scenario](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_2) : Descrizione di uno scenario in cui si usa Active Directory Federation Services (ad FS) per abilitare l'autenticazione a più fattori in base all'appartenenza al gruppo dell'utente.

    > [!NOTE]
    > In AD FS in Windows Server 2012 R2 è possibile abilitare l'autenticazione a più fattori in base al percorso di rete, all'identità del dispositivo e all'identità utente o all'appartenenza a un gruppo.

    Per istruzioni dettagliate per la configurazione e la verifica di questo scenario, vedere Guida alla procedura dettagliata [: gestire i rischi con ulteriori multi-factor authentication per le applicazioni riservate](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).

## <a name="BKMK_1"></a>Concetti chiave: meccanismi di autenticazione in AD FS

### <a name="benefits-of-authentication-mechanisms-in-ad---fs"></a>Vantaggi dei meccanismi di autenticazione in ADFS
Active Directory Federation Services (AD FS) in Windows Server 2012 R2 offre agli amministratori IT un set di strumenti più completo e flessibile per l'autenticazione degli utenti che desiderano accedere alle risorse aziendali. Consente agli amministratori di ottenere un controllo flessibile sui metodi di autenticazione primari e aggiuntivi, offre un'esperienza di gestione completa per la configurazione dei criteri di autenticazione (sia tramite l'interfaccia utente che con Windows PowerShell) e migliora esperienza per gli utenti finali che accedono alle applicazioni e ai servizi protetti da AD FS. Di seguito sono riportati alcuni dei vantaggi della protezione di applicazioni e servizi con AD FS in Windows Server 2012 R2:

-   Criteri di autenticazione globali: una funzionalità di gestione centrale dalla quale un amministratore IT può scegliere i metodi di autenticazione usati per autenticare gli utenti in base al percorso di rete da cui accedono alle risorse protette. Ciò consente agli amministratori di:

    -   Imporre l'utilizzo di metodi di autenticazione più sicuri per le richieste di accesso dall'Extranet.

    -   Consentire l'autenticazione dei dispositivi per un'autenticazione a due fattori trasparente. In questo modo l'identità dell'utente viene collegata al dispositivo registrato usato per accedere alla risorsa, offrendo una verifica dell'identità composta più sicura prima dell'accesso alle risorse protette.

        > [!NOTE]
        > Per altre informazioni sull'oggetto dispositivo, il servizio Registrazione dispositivi, Workplace Join e il dispositivo come autenticazione a due fattori trasparente e SSO, vedere [accedere a una rete aziendale da qualsiasi dispositivo per SSO e autenticazione a due fattori trasparente per tutte le applicazioni aziendali](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

    -   Impostare il requisito di autenticazione a più fattori per l'accesso alla rete Extranet o in modo condizionale in base all'identità dell'utente, al percorso di rete o a un dispositivo usato per accedere alle risorse protette.

-   Maggiore flessibilità nella configurazione dei criteri di autenticazione: è possibile configurare criteri di autenticazione personalizzati per le risorse protette con AD FS con valori aziendali variabili. È ad esempio possibile richiedere l'autenticazione a più fattori per le applicazioni con notevole impatto aziendale.

-   Semplicità d'uso: strumenti di gestione semplici e intuitivi come lo snap-in MMC di gestione AD FS basato su GUI e i cmdlet di Windows PowerShell consentono agli amministratori IT di configurare i criteri di autenticazione con relativa facilità. È possibile utilizzare Windows PowerShell per creare script delle soluzioni per poterli utilizzare su larga scale e per automatizzare le attività amministrative più ripetitive.

-   Maggiore controllo sulle risorse aziendali: dato che un amministratore può usare AD FS per configurare i criteri di autenticazione applicabili a una risorsa specifica, si ha un maggiore controllo sulle modalità di protezione delle risorse aziendali. Le applicazioni non possono ignorare i criteri di autenticazione specificati dagli amministratori IT. Per le applicazioni e i servizi con esigenze particolari, è possibile imporre l'autenticazione a più fattori, l'autenticazione del dispositivo e facoltativamente la ripetizione dell'autenticazione per ogni accesso alla risorsa.

-   Supporto per i provider di autenticazione a più fattori personalizzati: per le organizzazioni che sfruttano i metodi di autenticazione a più fattori di terze parti, AD FS offre la possibilità di incorporare e usare facilmente questi metodi di autenticazione

### <a name="authentication-scope"></a>Ambito di autenticazione
In AD FS in Windows Server 2012 R2 è possibile specificare un criterio di autenticazione in un ambito globale applicabile a tutte le applicazioni e i servizi protetti da AD FS.  È anche possibile impostare criteri di autenticazione per applicazioni e servizi specifici (relying party Trust) protetti da AD FS. L'impostazione di criteri di autenticazione per un'applicazione particolare (per attendibilità del componente) non ha la precedenza sui criteri di autenticazione globali. Se i criteri di autenticazione globali o di attendibilità del componente richiedono l'autenticazione a più fattori, questo tipo di autenticazione verrà attivata quando l'utente tenta l'autenticazione per l'attendibilità del componente in questione.  I criteri di autenticazione globali rappresentano il fallback per le attendibilità del componente (applicazioni e servizi) per le quali non sono configurati criteri di autenticazione specifici.

Un criterio di autenticazione globale si applica a tutte le relying party protette da AD FS. È possibile configurare le impostazioni seguenti nell'ambito dei criteri di autenticazione globali:

-   Metodi di autenticazione da utilizzare per l'autenticazione primaria

-   Impostazioni e metodi per l'autenticazione a più fattori

-   Abilitazione o meno dell'autenticazione dei dispositivi. Per altre informazioni, vedere [Join to Workplace from Any Device for SSO and Seamless Second Factor Authentication Across Company Applications](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

I criteri di autenticazione per attendibilità del componente sono applicabili nello specifico ai tentativi di accesso a tale attendibilità del componente (applicazione o servizio). È possibile configurare le impostazioni seguenti per i criteri di autenticazione per attendibilità del componente:

-   Se gli utenti devono specificare le credenziali a ogni accesso

-   Le impostazioni di autenticazione a più fattori in base ai dati relativi a utente/gruppo, registrazione del dispositivo e posizione di richiesta dell'accesso

### <a name="primary-and-additional-authentication-methods"></a>Metodi di autenticazione primaria e aggiuntivi
Con AD FS in Windows Server 2012 R2, oltre al meccanismo di autenticazione primaria, gli amministratori possono configurare metodi di autenticazione aggiuntivi. I metodi di autenticazione primari sono incorporati e sono progettati per convalidare le identità degli utenti. È possibile configurare altri fattori di autenticazione per richiedere che vengano fornite ulteriori informazioni sull'identità dell'utente e di conseguenza assicurare un'autenticazione più avanzata.

Con l'autenticazione principale in AD FS in Windows Server 2012 R2, sono disponibili le opzioni seguenti:

-   Per le risorse pubblicate per l'accesso dall'esterno della rete aziendale, viene selezionata per impostazione predefinita l'autenticazione basata su form. È inoltre possibile abilitare l'autenticazione del certificato, in altri termini l'autenticazione basata su smart card o l'autenticazione del certificato client dell'utente utilizzata con Servizi di dominio Active Directory.

-   Per le risorse Intranet è selezionata per impostazione predefinita l'autenticazione di Windows. È inoltre possibile abilitare l'autenticazione basata su form e/o l'autenticazione del certificato.

Con la selezione di più di un metodo di autenticazione si consente agli utenti di scegliere il metodo preferito nella pagina di accesso per l'applicazione o il servizio.

È inoltre possibile abilitare l'autenticazione del dispositivo per l'autenticazione a due fattori trasparente. In questo modo l'identità dell'utente viene collegata al dispositivo registrato usato per accedere alla risorsa, offrendo una verifica dell'identità composta più sicura prima dell'accesso alle risorse protette.

> [!NOTE]
> Per altre informazioni sull'oggetto dispositivo, il servizio Registrazione dispositivi, Workplace Join e il dispositivo come autenticazione a due fattori trasparente e SSO, vedere [accedere a una rete aziendale da qualsiasi dispositivo per SSO e autenticazione a due fattori trasparente per tutte le applicazioni aziendali](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

Se si specifica l'autenticazione di Windows (il metodo predefinito) per le risorse Intranet, le richieste di autenticazione utilizzano questo metodo in modo trasparente nei browser che supportano l'autenticazione di Windows.

> [!NOTE]
> L'autenticazione di Windows non è supportata in tutti i browser. Il meccanismo di autenticazione di AD FS in Windows Server 2012 R2 rileva l'agente utente del browser dell'utente e utilizza un'impostazione configurabile per determinare se l'agente utente supporta l'autenticazione di Windows. Gli amministratori possono integrare l'elenco di agenti utenti (tramite il comando di Windows PowerShell `Set-AdfsProperties -WIASupportedUserAgents`) per specificare stringhe di agente utente alternative per i browser che supportano l'autenticazione di Windows. Se l'agente utente del client non supporta l'autenticazione di Windows, il metodo di fallback predefinito è l'autenticazione basata su form.

### <a name="configuring-mfa"></a>Configurazione dell'autenticazione a più fattori
Per configurare l'autenticazione a più fattori in AD FS in Windows Server 2012 R2 sono disponibili due parti: specificare le condizioni in base alle quali è necessaria l'autenticazione a più fattori e selezionare un metodo di autenticazione aggiuntivo. Per ulteriori informazioni sui metodi di autenticazione aggiuntivi, vedere [configurare metodi di autenticazione aggiuntivi per ad FS](../../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md).

**Impostazioni dell'autenticazione a più fattori**

Per le impostazioni dell'autenticazione a più fattori sono disponibili le opzioni seguenti (condizioni in cui richiedere l'autenticazione a più fattori):

-   È possibile richiedere l'autenticazione a più fattori per utenti e gruppi specifici nel dominio di Active Directory a cui appartiene il server federativo.

-   È possibile richiedere l'autenticazione a più fattori per i dispositivi registrati (uniti all'area di lavoro) o non registrati (non uniti all'area di lavoro).

    Windows Server 2012 R2 adotta un approccio incentrato sull'utente ai dispositivi moderni in cui gli oggetti dispositivo rappresentano una relazione tra user@device e una società. Gli oggetti dispositivo sono una nuova classe in Active Directory in Windows Server 2012 R2 che può essere usata per offrire identità composta quando si fornisce l'accesso ad applicazioni e servizi. Un nuovo componente di ADFS, il servizio DRS (Device Registration Service), esegue il provisioning dell'identità di un dispositivo in Active Directory e imposta un certificato nel dispositivo del consumatore, che verrà utilizzato per rappresentare l'identità del dispositivo. È quindi possibile utilizzare questa identità del dispositivo per unire il dispositivo all'area di lavoro, in altre parole per connettere il dispositivo personale ad Active Directory nella rete aziendale. Quando si unisce il dispositivo personale all'area di lavoro, questo diventa un dispositivo noto e consentirà l'autenticazione a due fattori trasparente per le applicazioni e le risorse protette. In altre parole, dopo che un dispositivo è stato aggiunto all'area di lavoro, l'identità dell'utente è associata a questo dispositivo e può essere usata per una verifica dell'identità composta senza problemi prima di accedere a una risorsa protetta.

    Per altre informazioni sull'aggiunta all'area di lavoro e sull'uscita, vedere [accedere a una rete aziendale da qualsiasi dispositivo per SSO e autenticazione a due fattori trasparente per tutte le applicazioni aziendali](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

-   È possibile richiedere l'autenticazione a più fattori quando la richiesta di accesso per le risorse protette proviene dall'Extranet o dall'Intranet.

## <a name="BKMK_2"></a>Panoramica dello scenario
In questo scenario si Abilita l'autenticazione a più fattori in base ai dati di appartenenza a gruppi dell'utente per un'applicazione specifica. In altri termini, verranno configurati i criteri di autenticazione nel server federativo in modo da richiedere l'autenticazione a più fattori quando gli utenti che appartengono a un determinato gruppo accedono a un'applicazione specifica ospitata in un server Web.

Più in dettaglio, nello scenario verranno abilitati criteri di autenticazione per un'applicazione di test basata su attestazioni denominata **claimapp**, in base ai quali l'utente di Active Directory **Robert Hatley** dovrà eseguire l'autenticazione a più fattori dato che appartiene al gruppo di Active Directory **Finance**.

Le istruzioni dettagliate per configurare e verificare questo scenario sono disponibili nella [Guida dettagliata: gestire i rischi con multi-factor authentication aggiuntive per le applicazioni riservate](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md). Per completare i passaggi di questa procedura dettagliata, è necessario configurare un ambiente lab e seguire i passaggi descritti in [configurare l'ambiente lab per ad FS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

Altri scenari di abilitazione dell'autenticazione a più fattori in AD FS includono i seguenti:

-   Abilitare l'autenticazione a più fattori se la richiesta di accesso proviene dall'Extranet. È possibile modificare il codice presentato nella sezione "configurare i criteri di autenticazione a più fattori" della [Guida dettagliata: gestire i rischi con ulteriori multi-factor authentication per le applicazioni riservate](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md) con gli elementi seguenti:

    ```
    'c:[type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn" );'
    ```

-   Abilitare l'autenticazione a più fattori se la richiesta di accesso proviene da un dispositivo non unito all'area di lavoro.  È possibile modificare il codice presentato nella sezione "configurare i criteri di autenticazione a più fattori" della [Guida dettagliata: gestire i rischi con ulteriori multi-factor authentication per le applicazioni riservate](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md) con gli elementi seguenti:

    ```
    'NOT EXISTS([type=="https://schemas.microsoft.com/2012/01/devicecontext/claims/registrationid"]) => issue (type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn");'

    ```

-   Abilitare l'autenticazione a più fattori se l'accesso è eseguito da un utente con un dispositivo unito all'area di lavoro ma non registrato per l'utente. È possibile modificare il codice presentato nella sezione "configurare i criteri di autenticazione a più fattori" della [Guida dettagliata: gestire i rischi con ulteriori multi-factor authentication per le applicazioni riservate](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md) con gli elementi seguenti:

    ```
    'c:[type=="https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", value == "false"] => issue (type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn");'

    ```

## <a name="see-also"></a>Vedi anche
[Guida allo scenario: gestire i rischi con ulteriori multi-factor authentication per le applicazioni sensibili](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)
[configurare l'ambiente lab per ad FS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



