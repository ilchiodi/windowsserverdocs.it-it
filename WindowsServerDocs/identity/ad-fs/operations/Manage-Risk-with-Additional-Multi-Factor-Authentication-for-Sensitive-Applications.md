---
ms.assetid: 934ac796-e2ee-490d-8265-6a818be5ee79
title: "Gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni sensibili"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e9ee275e6fe38005394cd071e9cfe9a7999350e8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="manage-risk-with-additional-multi-factor-authentication-for-sensitive-applications"></a>Gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni sensibili

>Si applica a: Windows Server 2012 R2


-   [Configurare l'ambiente di testing per ADFS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)

-   [Guida allo scenario: Gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni sensibili](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

-   [Configurare metodi di autenticazione aggiuntivi per AD FS](../../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md)

## <a name="in-this-guide"></a>In questa Guida
Questa guida fornisce le informazioni seguenti:

-   [Meccanismi di autenticazione in ADFS](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_1) -descrizione dei meccanismi di autenticazione disponibili in Active Directory Federation Services (ADFS) in Windows Server 2012 R2

-   [Panoramica dello scenario](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_2) -una descrizione di uno scenario in cui si utilizza Active Directory Federation Services (ADFS) per abilitare l'autenticazione a più fattori (MFA) in base all'appartenenza a gruppi dell'utente.

    > [!NOTE]
    > In ADFS in Windows Server 2012 R2 è possibile abilitare l'autenticazione a più fattori in base al percorso di rete, l'identità del dispositivo e utente identità o appartenenza al gruppo.

    Per istruzioni dettagliate le istruzioni per la configurazione e verifica di questo scenario, vedere [Guida allo scenario: gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni sensibili](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).

## <a name="BKMK_1"></a>Concetti fondamentali - meccanismi di autenticazione in ADFS

### <a name="benefits-of-authentication-mechanisms-in-ad---fs"></a>Vantaggi dei meccanismi di autenticazione in ADFS
Active Directory Federation Services (ADFS) in Windows Server 2012 R2 offre agli amministratori IT un set più ampio e flessibile di strumenti per l'autenticazione degli utenti che desiderano accedere alle risorse aziendali. Questa agli amministratori il controllo flessibile sui metodi di autenticazione aggiuntivo e primario, offre un'esperienza di gestione completa per la configurazione dell'autenticazione criteri (sia tramite l'interfaccia utente e Windows PowerShell) e migliora l'esperienza degli utenti finali che accedono ad applicazioni e servizi protetti da ADFS. Di seguito sono indicati alcuni dei vantaggi della protezione di applicazioni e servizi con AD FS in Windows Server 2012 R2:

-   Criteri di autenticazione globali - una funzionalità di gestione centralizzata da cui un amministratore IT può scegliere i metodi di autenticazione vengono utilizzati per autenticare gli utenti in base al percorso di rete da cui accedono alle risorse protette. Ciò consente agli amministratori di eseguire le operazioni seguenti:

    -   Imporre l'utilizzo di metodi di autenticazione più sicuri per le richieste di accesso dall'extranet.

    -   Abilitare l'autenticazione del dispositivo per l'autenticazione a due fattori trasparente. Consente di associare l'identità dell'utente al dispositivo registrato utilizzato per accedere alla risorsa, offrendo così una verifica dell'identità composta più sicura prima di accesso alle risorse protette.

        > [!NOTE]
        > Per ulteriori informazioni sull'oggetto dispositivo, Device Registration Service, l'aggiunta alla rete aziendale e il dispositivo come autenticazione a due fattori trasparente e SSO, vedere [accedere a una rete aziendale da qualsiasi dispositivo per SSO e trasparente secondo fattore di autenticazione tutte le applicazioni aziendali](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

    -   Impostare il requisito di autenticazione a più fattori per l'accesso extranet tutti o in modo condizionale in base all'identità dell'utente, percorso di rete o un dispositivo che viene utilizzato per accedere alle risorse protette.

-   Maggiore flessibilità nella configurazione dei criteri di autenticazione: è possibile configurare criteri di autenticazione personalizzati per le risorse protette da ADFS di Active Directory con valori aziendali variabili. Ad esempio, è possibile richiedere l'autenticazione a più fattori per le applicazioni con alto impatto aziendale.

-   Facilità d'uso: strumenti di gestione facili e intuitivi, come lo snap-in con interfaccia grafica MMC Gestione AD FS e i cmdlet di Windows PowerShell consentono agli amministratori IT di configurare criteri di autenticazione con relativa facilità. Con Windows PowerShell, è possibile creare script delle soluzioni per l'uso su larga scala e per automatizzare le attività amministrative più ripetitive.

-   Maggiore controllo sulle risorse aziendali: dato che un amministratore è possibile utilizzare ADFS per configurare un criterio di autenticazione da applicare a una specifica risorsa, si ottiene un maggiore controllo sulle risorse aziendali come sono protezione. Le applicazioni non possono ignorare i criteri di autenticazione specificati dagli amministratori IT. Per le applicazioni e servizi, è possibile abilitare requisito, l'autenticazione del dispositivo e facoltativamente la ripetizione dell'autenticazione a più fattori ogni volta che si accede alla risorsa.

-   Supporto di provider di autenticazione a più fattori personalizzati: alle organizzazioni che utilizzano metodi di autenticazione a più fattori di terze parti, AD FS offre la possibilità di incorporare e usare questi metodi di autenticazione senza problemi.

### <a name="authentication-scope"></a>Ambito di autenticazione
In ADFS in Windows Server 2012 R2 è possibile specificare un criterio di autenticazione in ambito globale applicabili a tutte le applicazioni e servizi protetti da ADFS.  È inoltre possibile impostare criteri di autenticazione per applicazioni specifiche e i servizi (trust della relying party) protetti da ADFS. Se si specifica un criterio di autenticazione per una particolare applicazione (per ogni componente attendibilità) non esegue l'override di criteri di autenticazione globali. Se globale o per ogni componente attendibilità criterio di autenticazione richiede l'autenticazione a più fattori, autenticazione a più fattori verrà attivata quando l'utente tenta l'autenticazione per questo trust della relying party.  I criteri di autenticazione globali sono rappresentano il fallback per i trust della relying party (applicazioni e servizi) che non è configurato un criterio di autenticazione specifici.

Criteri di autenticazione globali si applicano a tutte le relying party che sono protetti da ADFS. È possibile configurare le impostazioni seguenti come parte di criteri di autenticazione globali:

-   Metodi di autenticazione da utilizzare per l'autenticazione principale

-   Le impostazioni e i metodi per l'autenticazione a più fattori

-   Indica se è abilitata l'autenticazione del dispositivo. Per ulteriori informazioni, vedere [accedere a una rete aziendale da qualsiasi dispositivo per SSO e trasparente secondo fattore di autenticazione tutte le applicazioni aziendali](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

Per ogni relying party trust autenticazione criteri vengono applicati in modo specifico per i tentativi di accesso a tale trust della relying party (applicazione o servizio). Come parte del criterio di autenticazione per ogni relying party trust, è possibile configurare le impostazioni seguenti:

-   Indica se gli utenti vengono richiesto di fornire le proprie credenziali ogni volta che al momento dell'accesso

-   Le impostazioni di autenticazione a più fattori in base al utente/di gruppo, la registrazione dei dispositivi e richiesta di accesso ai dati sulla posizione

### <a name="primary-and-additional-authentication-methods"></a>Metodi di autenticazione primaria e aggiuntivi
Con AD FS in Windows Server 2012 R2, oltre al meccanismo di autenticazione primaria, gli amministratori possono configurare metodi di autenticazione aggiuntivi. Metodi di autenticazione primaria sono predefiniti e progettati per la convalida delle identità degli utenti. È possibile configurare ulteriori fattori di autenticazione per richiedere che verranno fornite ulteriori informazioni sull'identità dell'utente e garantire quindi un'autenticazione più avanzata.

Con l'autenticazione primaria in ADFS in Windows Server 2012 R2, sono disponibili le opzioni seguenti:

-   Per le risorse pubblicate per l'accesso dall'esterno della rete aziendale, l'autenticazione basata su form è selezionata per impostazione predefinita. Inoltre, è anche possibile abilitare l'autenticazione del certificato (in altre parole, l'autenticazione basata su smart card o autenticazione del certificato client utente che funziona con AD DS).

-   Per le risorse intranet, l'autenticazione di Windows è selezionata per impostazione predefinita. È inoltre possibile abilitare form e/o l'autenticazione del certificato.

Se si seleziona più di un metodo di autenticazione, consentire agli utenti di scegliere quale metodo di autenticazione con la pagina di accesso per il servizio o applicazione.

È anche possibile abilitare l'autenticazione del dispositivo per l'autenticazione a due fattori trasparente. Consente di associare l'identità dell'utente al dispositivo registrato utilizzato per accedere alla risorsa, offrendo così una verifica dell'identità composta più sicura prima di accesso alle risorse protette.

> [!NOTE]
> Per ulteriori informazioni sull'oggetto dispositivo, Device Registration Service, l'aggiunta alla rete aziendale e il dispositivo come autenticazione a due fattori trasparente e SSO, vedere [accedere a una rete aziendale da qualsiasi dispositivo per SSO e trasparente secondo fattore di autenticazione tutte le applicazioni aziendali](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

Se si specifica il metodo di autenticazione di Windows (opzione predefinita) per le risorse intranet, le richieste di autenticazione utilizzano questo metodo in modo trasparente nei browser che supportano l'autenticazione di Windows.

> [!NOTE]
> Autenticazione di Windows non è supportato in tutti i browser. Il meccanismo di autenticazione in ADFS in Windows Server 2012 R2 rileva l'agente utente del browser dell'utente e utilizza un'impostazione configurabile per determinare se l'agente utente supporta l'autenticazione di Windows. Gli amministratori possono aggiungere all'elenco di agenti utenti (tramite Windows PowerShell `Set-AdfsProperties -WIASupportedUserAgents`comando, per specificare stringhe agente utente alternative per i browser che supportano l'autenticazione di Windows. Se l'agente utente del client non supporta l'autenticazione di Windows, il metodo di fallback predefinito è l'autenticazione basata su form.

### <a name="configuring-mfa"></a>Configurazione di autenticazione a più fattori
Per configurare l'autenticazione a più fattori in ADFS in Windows Server 2012 R2 suddivisa in due parti: specificare le condizioni in cui è richiesta l'autenticazione a più fattori e selezione di un metodo di autenticazione aggiuntivi. Per ulteriori informazioni sui metodi di autenticazione aggiuntivi, vedere [configurare di metodi di autenticazione aggiuntivi per AD FS](../../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md).

**Impostazioni di autenticazione a più fattori**

Le opzioni seguenti sono disponibili per le impostazioni di autenticazione a più fattori (condizioni in cui richiedere l'autenticazione a più fattori):

-   È possibile richiedere l'autenticazione a più fattori per specifici utenti e gruppi nel dominio di Active Directory appartenenti al server federativo.

-   È possibile richiedere l'autenticazione a più fattori per registrare (all'area di lavoro) o annullata la registrazione (non all'area di lavoro) dispositivi.

    Windows Server 2012 R2 adotta un approccio incentrato sull'utente per i dispositivi moderni in cui gli oggetti dispositivi rappresentano una relazione tra user@devicee una società. Gli oggetti dispositivo sono una nuova classe in Active Directory in Windows Server 2012 R2 che può essere usato per offrire identità composte accesso ad applicazioni e servizi. Un nuovo componente di ADFS, il servizio DRS (DRS) - esegue il provisioning identità di un dispositivo in Active Directory e imposta un certificato nel dispositivo del consumatore che verrà utilizzato per rappresentare l'identità del dispositivo. È quindi possibile utilizzare questo dispositivo all'area di lavoro identità unire il dispositivo, in altre parole, per connettere il dispositivo personale ad Active Directory di lavoro. Quando si aggiunge il dispositivo personale all'area di lavoro, diventa un dispositivo noto e fornirà l'autenticazione a due fattori trasparente a risorse protette e applicazioni. In altre parole, una volta un dispositivo all'area di lavoro, l'identità dell'utente viene associata al dispositivo e può essere utilizzato per una verifica dell'identità composta trasparente prima di accede una risorsa protetta.

    Per ulteriori informazioni sull'aggiunta all'area di lavoro e l'uscita, vedere [aggiunta alla rete aziendale da qualsiasi dispositivo per SSO e trasparente secondo fattore di autenticazione tutte le applicazioni aziendali](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

-   È possibile richiedere l'autenticazione a più fattori quando la richiesta di accesso per le risorse protette proviene dall'extranet o intranet.

## <a name="BKMK_2"></a>Panoramica dello scenario
In questo scenario, si abilita l'autenticazione a più fattori in base ai dati di appartenenza al gruppo dell'utente per un'applicazione specifica. In altre parole, si imposterà un criterio di autenticazione nel server federativo in modo da richiedere l'autenticazione a più fattori quando gli utenti che appartengono a un determinato gruppo richiedono l'accesso a un'applicazione specifica ospitata in un server web.

Più specificamente, in questo scenario, si attiva un criterio di autenticazione per un'applicazione di test basato su attestazioni denominata **claimapp**, in cui un utente di Active Directory **Robert Hatley** verrà richiesto di eseguire l'autenticazione a più fattori dato che appartiene a un gruppo di Active Directory **Finance**.

Il passaggio istruzioni dettagliate per configurare e verificare questo scenario sono incluse nei [Guida allo scenario: gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni sensibili](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md). Per completare i passaggi descritti in questa procedura dettagliata, è necessario configurare un ambiente di testing e seguire i passaggi descritti in [impostare dell'ambiente di testing per ADFS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

Altri possibili scenari per abilitare l'autenticazione a più fattori in ADFS includono quanto segue:

-   Abilitare l'autenticazione a più fattori, se la richiesta di accesso proviene dall'extranet. È possibile modificare il codice presentato nella sezione "autenticazione a più fattori configurare i criteri" di [Guida allo scenario: gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni sensibili](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md) con le operazioni seguenti:

    ```
    'c:[type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn" );'
    ```

-   Abilitare l'autenticazione a più fattori, se la richiesta di accesso proviene da un dispositivo aggiunto non aziendale.  È possibile modificare il codice presentato nella sezione "autenticazione a più fattori configurare i criteri" di [Guida allo scenario: gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni sensibili](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md) con le operazioni seguenti:

    ```
    'NOT EXISTS([type=="https://schemas.microsoft.com/2012/01/devicecontext/claims/registrationid"]) => issue (type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn");'

    ```

-   Abilitare l'autenticazione a più fattori, se l'accesso proviene da un utente con un dispositivo all'area di lavoro aggiunti ma non registrato per l'utente. È possibile modificare il codice presentato nella sezione "autenticazione a più fattori configurare i criteri" di [Guida allo scenario: gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni sensibili](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md) con le operazioni seguenti:

    ```
    'c:[type=="https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", value == "false"] => issue (type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn");'

    ```

## <a name="see-also"></a>Vedere anche
[Guida allo scenario: Gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni sensibili](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)<ph x="2">
</ph>[impostare dell'ambiente di testing per ADFS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



