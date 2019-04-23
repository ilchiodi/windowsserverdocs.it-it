---
ms.assetid: 934ac796-e2ee-490d-8265-6a818be5ee79
title: Gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni con esigenze particolari
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e9ee275e6fe38005394cd071e9cfe9a7999350e8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855892"
---
# <a name="manage-risk-with-additional-multi-factor-authentication-for-sensitive-applications"></a>Gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni con esigenze particolari

>Si applica a: Windows Server 2012 R2


-   [Configurare l'ambiente lab per AD FS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)

-   [Guida allo scenario: Gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni sensibili](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

-   [Configurare altri metodi di autenticazione per AD FS](../../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md)

## <a name="in-this-guide"></a>Contenuto della guida
In questa guida sono disponibili le informazioni seguenti:

-   [Meccanismi di autenticazione in AD FS](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_1) -descrizione dei meccanismi di autenticazione disponibili in Active Directory Federation Services (ADFS) in Windows Server 2012 R2

-   [Panoramica dello scenario](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_2) -una descrizione di uno scenario in cui si usa Active Directory Federation Services (ADFS) per abilitare multi-factor authentication (MFA) in base all'appartenenza dell'utente.

    > [!NOTE]
    > In ADFS in Windows Server 2012 R2 è possibile abilitare l'autenticazione a più fattori basata sul percorso di rete, identità del dispositivo e utente identità o appartenenza al gruppo.

    Per istruzioni dettagliate per la configurazione e verifica di questo scenario, vedere [Guida allo scenario: Gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni sensibili](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).

## <a name="BKMK_1"></a>Concetti di base-meccanismi di autenticazione in AD FS

### <a name="benefits-of-authentication-mechanisms-in-ad---fs"></a>Vantaggi dei meccanismi di autenticazione in ADFS
Active Directory Federation Services (ADFS) in Windows Server 2012 R2 fornisce agli amministratori IT un set più ampio e flessibile di strumenti per l'autenticazione degli utenti che vogliono accedere alle risorse aziendali. Consente agli amministratori il controllo flessibile sui metodi di autenticazione aggiuntivo e il database primario, offre un'esperienza di gestione completa per la configurazione dell'autenticazione dei criteri (sia tramite l'interfaccia utente di Windows PowerShell) e migliora l'esperienza degli utenti finali che accedono ad applicazioni e servizi protetti da AD FS. Di seguito sono indicati alcuni dei vantaggi della protezione di applicazioni e servizi con AD FS in Windows Server 2012 R2:

-   Criteri di autenticazione globali - una funzionalità di gestione centrale, che un amministratore IT può scegliere quali metodi di autenticazione vengono utilizzati per autenticare gli utenti in base al percorso di rete da cui accedono alle risorse protette. Ciò consente agli amministratori di:

    -   Imporre l'utilizzo di metodi di autenticazione più sicuri per le richieste di accesso dall'Extranet.

    -   Consentire l'autenticazione dei dispositivi per un'autenticazione a due fattori trasparente. Ciò consente di associare l'identità dell'utente al dispositivo registrato utilizzato per accedere alla risorsa, offrendo così una verifica dell'identità composta più sicura prima che si accede alle risorse protette.

        > [!NOTE]
        > Per altre informazioni sull'oggetto dispositivo, Device Registration Service, aggiunta alla rete aziendale e il dispositivo come autenticazione a due fattori trasparente e SSO, vedere [accedere a una rete aziendale da qualsiasi dispositivo per SSO e trasparente secondo fattore di autenticazione Tutte le applicazioni aziendali](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

    -   Impostare il requisito di autenticazione a più fattori per tutti gli accessi extranet o in modo condizionale in base all'identità dell'utente, percorso di rete o un dispositivo che viene usato per accedere alle risorse protette.

-   Maggiore flessibilità nella configurazione dei criteri di autenticazione: è possibile configurare criteri di autenticazione personalizzati per le risorse protette da ADFS di Active Directory con valori aziendali variabili. È ad esempio possibile richiedere l'autenticazione a più fattori per le applicazioni con notevole impatto aziendale.

-   Semplicità d'uso: strumenti di gestione facili e intuitivi, ad esempio lo snap-in con interfaccia grafica MMC Gestione AD FS e i cmdlet di Windows PowerShell consentono agli amministratori IT di configurare i criteri di autenticazione con relativa facilità. È possibile utilizzare Windows PowerShell per creare script delle soluzioni per poterli utilizzare su larga scale e per automatizzare le attività amministrative più ripetitive.

-   Maggiore controllo sulle risorse aziendali: poiché come amministratore è possibile usare AD FS per configurare un criterio di autenticazione che si applica a una risorsa specifica, è necessario maggiore controllo sulle risorse aziendali come protezione. Le applicazioni non possono ignorare i criteri di autenticazione specificati dagli amministratori IT. Per le applicazioni e i servizi con esigenze particolari, è possibile imporre l'autenticazione a più fattori, l'autenticazione del dispositivo e facoltativamente la ripetizione dell'autenticazione per ogni accesso alla risorsa.

-   Supporto per provider di autenticazione a più fattori personalizzati: alle organizzazioni che usano i metodi di autenticazione a più fattori di terze parti, AD FS offre la possibilità di incorporare e usare facilmente questi metodi di autenticazione.

### <a name="authentication-scope"></a>Ambito di autenticazione
In ADFS in Windows Server 2012 R2 è possibile specificare un criterio di autenticazione in ambito globale applicabili a tutte le applicazioni e servizi protetti da AD FS.  È anche possibile impostare i criteri di autenticazione per applicazioni specifiche e i servizi (trust della relying party) protetti da AD FS. L'impostazione di criteri di autenticazione per un'applicazione particolare (per attendibilità del componente) non ha la precedenza sui criteri di autenticazione globali. Se i criteri di autenticazione globali o di attendibilità del componente richiedono l'autenticazione a più fattori, questo tipo di autenticazione verrà attivata quando l'utente tenta l'autenticazione per l'attendibilità del componente in questione.  I criteri di autenticazione globali rappresentano il fallback per le attendibilità del componente (applicazioni e servizi) per le quali non sono configurati criteri di autenticazione specifici.

Un criterio di autenticazione globale si applica a tutte le relying party che sono protetti da AD FS. È possibile configurare le impostazioni seguenti nell'ambito dei criteri di autenticazione globali:

-   Metodi di autenticazione da utilizzare per l'autenticazione primaria

-   Impostazioni e metodi per l'autenticazione a più fattori

-   Abilitazione o meno dell'autenticazione dei dispositivi. Per altre informazioni, vedere [Join to Workplace from Any Device for SSO and Seamless Second Factor Authentication Across Company Applications](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

I criteri di autenticazione per attendibilità del componente sono applicabili nello specifico ai tentativi di accesso a tale attendibilità del componente (applicazione o servizio). È possibile configurare le impostazioni seguenti per i criteri di autenticazione per attendibilità del componente:

-   Se gli utenti devono specificare le credenziali a ogni accesso

-   Le impostazioni di autenticazione a più fattori in base ai dati relativi a utente/gruppo, registrazione del dispositivo e posizione di richiesta dell'accesso

### <a name="primary-and-additional-authentication-methods"></a>Metodi di autenticazione primaria e aggiuntivi
Con AD FS in Windows Server 2012 R2, oltre al meccanismo di autenticazione primaria, gli amministratori possono configurare metodi di autenticazione aggiuntivi. Metodi di autenticazione primaria sono predefiniti e progettati per la convalida delle identità degli utenti. È possibile configurare ulteriori fattori di autenticazione per richiedere che verranno fornite ulteriori informazioni sull'identità dell'utente e garantire quindi un'autenticazione più efficace.

Con l'autenticazione primaria in ADFS in Windows Server 2012 R2, sono disponibili le opzioni seguenti:

-   Per le risorse pubblicate per l'accesso dall'esterno della rete aziendale, viene selezionata per impostazione predefinita l'autenticazione basata su form. È inoltre possibile abilitare l'autenticazione del certificato, in altri termini l'autenticazione basata su smart card o l'autenticazione del certificato client dell'utente utilizzata con Servizi di dominio Active Directory.

-   Per le risorse Intranet è selezionata per impostazione predefinita l'autenticazione di Windows. È inoltre possibile abilitare l'autenticazione basata su form e/o l'autenticazione del certificato.

Con la selezione di più di un metodo di autenticazione si consente agli utenti di scegliere il metodo preferito nella pagina di accesso per l'applicazione o il servizio.

È inoltre possibile abilitare l'autenticazione del dispositivo per l'autenticazione a due fattori trasparente. Ciò consente di associare l'identità dell'utente al dispositivo registrato utilizzato per accedere alla risorsa, offrendo così una verifica dell'identità composta più sicura prima che si accede alle risorse protette.

> [!NOTE]
> Per altre informazioni sull'oggetto dispositivo, Device Registration Service, aggiunta alla rete aziendale e il dispositivo come autenticazione a due fattori trasparente e SSO, vedere [accedere a una rete aziendale da qualsiasi dispositivo per SSO e trasparente secondo fattore di autenticazione Tutte le applicazioni aziendali](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

Se si specifica l'autenticazione di Windows (il metodo predefinito) per le risorse Intranet, le richieste di autenticazione utilizzano questo metodo in modo trasparente nei browser che supportano l'autenticazione di Windows.

> [!NOTE]
> L'autenticazione di Windows non è supportata in tutti i browser. Il meccanismo di autenticazione in ADFS in Windows Server 2012 R2 rileva l'agente utente del browser dell'utente e usa un'impostazione configurabile per determinare se l'agente utente supporta l'autenticazione di Windows. Gli amministratori possono integrare l'elenco di agenti utenti (tramite il comando di Windows PowerShell `Set-AdfsProperties -WIASupportedUserAgents`) per specificare stringhe di agente utente alternative per i browser che supportano l'autenticazione di Windows. Se l'agente utente del client non supporta l'autenticazione di Windows, il metodo di fallback predefinito è l'autenticazione basata su form.

### <a name="configuring-mfa"></a>Configurazione dell'autenticazione a più fattori
Esistono due parti per configurare l'autenticazione a più fattori in ADFS in Windows Server 2012 R2: specificare le condizioni in cui è richiesta l'autenticazione a più fattori e selezione di un metodo di autenticazione aggiuntivo. Per altre informazioni sui metodi di autenticazione aggiuntivi, vedere [configurazione di altri metodi di autenticazione per AD FS](../../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md).

**Impostazioni di autenticazione a più fattori**

Per le impostazioni dell'autenticazione a più fattori sono disponibili le opzioni seguenti (condizioni in cui richiedere l'autenticazione a più fattori):

-   È possibile richiedere l'autenticazione a più fattori per utenti e gruppi specifici nel dominio di Active Directory a cui appartiene il server federativo.

-   È possibile richiedere l'autenticazione a più fattori per i dispositivi registrati (uniti all'area di lavoro) o non registrati (non uniti all'area di lavoro).

    Windows Server 2012 R2 adotta un approccio incentrato sull'utente per i dispositivi moderni in cui gli oggetti dispositivi rappresentano una relazione tra user@device e una società. Gli oggetti dispositivo sono una nuova classe in Active Directory in Windows Server 2012 R2 che può essere utilizzato per offrire identità composte accesso ad applicazioni e servizi. Un nuovo componente di ADFS, il servizio DRS (Device Registration Service), esegue il provisioning dell'identità di un dispositivo in Active Directory e imposta un certificato nel dispositivo del consumatore, che verrà utilizzato per rappresentare l'identità del dispositivo. È quindi possibile utilizzare questa identità del dispositivo per unire il dispositivo all'area di lavoro, in altre parole per connettere il dispositivo personale ad Active Directory nella rete aziendale. Quando si unisce il dispositivo personale all'area di lavoro, questo diventa un dispositivo noto e consentirà l'autenticazione a due fattori trasparente per le applicazioni e le risorse protette. In altre parole, una volta un dispositivo all'area di lavoro, l'identità dell'utente è associata al dispositivo e può essere utilizzato per una verifica dell'identità composta trasparente prima dell'accesso a una risorsa protetta.

    Per altre informazioni su aggiunta alla rete aziendale e uscita, vedere [Join a una rete aziendale da qualsiasi dispositivo per SSO e trasparente secondo fattore di autenticazione tra le applicazioni aziendali](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

-   È possibile richiedere l'autenticazione a più fattori quando la richiesta di accesso per le risorse protette proviene dall'Extranet o dall'Intranet.

## <a name="BKMK_2"></a>Panoramica dello scenario
In questo scenario, si abilita MFA basato sui dati di appartenenza al gruppo dell'utente per un'applicazione specifica. In altri termini, verranno configurati i criteri di autenticazione nel server federativo in modo da richiedere l'autenticazione a più fattori quando gli utenti che appartengono a un determinato gruppo accedono a un'applicazione specifica ospitata in un server Web.

Più in dettaglio, nello scenario verranno abilitati criteri di autenticazione per un'applicazione di test basata su attestazioni denominata **claimapp**, in base ai quali l'utente di Active Directory **Robert Hatley** dovrà eseguire l'autenticazione a più fattori dato che appartiene al gruppo di Active Directory **Finance**.

Il passaggio a istruzioni dettagliate per configurare e verificare questo scenario vengono fornite [Guida allo scenario: Gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni sensibili](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md). Per completare la procedura descritta in questa procedura dettagliata, è necessario configurare un ambiente lab e seguire i passaggi descritti in [configurare l'ambiente lab per AD FS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

Altri possibili scenari per abilitare l'autenticazione a più fattori in ADFS includono quanto segue:

-   Abilitare l'autenticazione a più fattori se la richiesta di accesso proviene dall'Extranet. È possibile modificare il codice presentato nella sezione "Configurare MFA i criteri" di [Guida allo scenario: Gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni sensibili](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md) con quanto segue:

    ```
    'c:[type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn" );'
    ```

-   Abilitare l'autenticazione a più fattori se la richiesta di accesso proviene da un dispositivo non unito all'area di lavoro.  È possibile modificare il codice presentato nella sezione "Configurare MFA i criteri" di [Guida allo scenario: Gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni sensibili](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md) con quanto segue:

    ```
    'NOT EXISTS([type=="https://schemas.microsoft.com/2012/01/devicecontext/claims/registrationid"]) => issue (type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn");'

    ```

-   Abilitare l'autenticazione a più fattori se l'accesso è eseguito da un utente con un dispositivo unito all'area di lavoro ma non registrato per l'utente. È possibile modificare il codice presentato nella sezione "Configurare MFA i criteri" di [Guida allo scenario: Gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni sensibili](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md) con quanto segue:

    ```
    'c:[type=="https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", value == "false"] => issue (type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn");'

    ```

## <a name="see-also"></a>Vedere anche
[Guida allo scenario: Gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni sensibili](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)
[configurare l'ambiente lab per AD FS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



