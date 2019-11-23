---
ms.assetid: a0f7bb11-47a5-47ff-a70c-9e6353382b39
title: Gestire i rischi con il controllo di accesso condizionale
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e73cf77e9590496f0ff3f881fd8ac4556450b5f0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357762"
---
# <a name="manage-risk-with-conditional-access-control"></a>Gestire i rischi con il controllo di accesso condizionale




-   [Concetti chiave: controllo di accesso condizionale in AD FS](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md#BKMK_1)

-   [Gestione dei rischi con il controllo degli accessi condizionali](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md#BKMK_2)

## <a name="BKMK_1"></a>Concetti chiave: controllo di accesso condizionale in AD FS
La funzione complessiva di AD FS consiste nell'emettere un token di accesso che contiene un set di attestazioni. La decisione che riguarda le attestazioni ADFS accetta e quindi invia è disciplinata dalle regole attestazione.

Il controllo di accesso in AD FS viene implementato con regole attestazione di autorizzazione di rilascio usate per rilasciare attestazioni che consentono o negano che determineranno se un utente o un gruppo di utenti sarà autorizzato ad accedere o meno a risorse protette da AD FS. Le regole di autorizzazione possono essere impostate solo per i trust della relying party.

|Opzione della regola|Logica della regola|
|---------------|--------------|
|Consentire l'accesso a tutti gli utenti|Se il tipo di attestazione in ingresso è uguale a *qualsiasi tipo di attestazione* e il valore è uguale a *qualsiasi valore*, rilasciare un'attestazione con valore uguale a *Consenti*|
|Consenti accesso agli utenti con questa attestazione in ingresso|Se il tipo di attestazione in ingresso è uguale a *tipo di attestazione specificato* e il valore è uguale a *valore di attestazione specificato*, rilasciare un'attestazione con valore uguale a *Consenti*|
|Nega accesso agli utenti con questa attestazione in ingresso|Se il tipo di attestazione in ingresso è uguale a *tipo di attestazione specificato* e il valore è uguale a *valore di attestazione specificato*, rilasciare un'attestazione con valore uguale a *Nega*|

Per altre informazioni sulle opzioni e sulla logica di queste regole, vedere [When to Use an Authorization Claim Rule](https://technet.microsoft.com/library/ee913560.aspx).

In AD FS in Windows Server 2012 R2, il controllo di accesso è migliorato con diversi fattori, inclusi i dati relativi a utenti, dispositivi, posizione e autenticazione. Ciò è possibile grazie alla disponibilità di una più ampia gamma di tipi di attestazione per le regole attestazione di autorizzazione.  In altre parole, in AD FS in Windows Server 2012 R2, è possibile applicare il controllo di accesso condizionale in base all'identità dell'utente o all'appartenenza a un gruppo, al percorso di rete, al dispositivo (se è stato aggiunto all'area di lavoro). per altre informazioni, vedere [aggiungere all'area di lavoro da qualsiasi dispositivo per SSO e autenticazione a due fattori trasparente per tutte le applicazioni aziendali](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

Il controllo dell'accesso condizionale in AD FS in Windows Server 2012 R2 offre i vantaggi seguenti:

-   Criteri di autorizzazione per applicazione flessibili ed espressivi, con i quali è possibile concedere o negare l'accesso in base a utente, dispositivo, posizione in rete e stato di autenticazione

-   Creazione di regole di autorizzazione di rilascio per le applicazioni relying party

-   Interfaccia utente completa per gli scenari comuni di controllo di accesso condizionale

-   Linguaggio esteso per le attestazioni e supporto di Windows PowerShell per gli scenari avanzati di controllo di accesso condizionale

-   Messaggi di "accesso negato" personalizzati (per applicazione relying party). Per altre informazioni, vedere [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx). La possibilità di personalizzare questi messaggi consente di spiegare i motivi per cui viene negato l'accesso e facilita inoltre la risoluzione autonoma del problema nei casi in cui è possibile, ad esempio richiedendo agli utenti di unire i loro dispositivi all'area di lavoro. Per altre informazioni, vedere [Join to Workplace from Any Device for SSO and Seamless Second Factor Authentication Across Company Applications](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

La tabella seguente include tutti i tipi di attestazione disponibili in AD FS in Windows Server 2012 R2 da usare per l'implementazione del controllo di accesso condizionale.

|Tipo di attestazione|Descrizione|
|--------------|---------------|
|Indirizzo di posta elettronica|Indirizzo di posta elettronica dell'utente.|
|Nome (di battesimo)|Nome (di battesimo) dell'utente.|
|Nome|Nome univoco dell'utente.|
|UPN|Nome dell'entità utente (UPN) dell'utente.|
|Nome comune|Nome comune dell'utente.|
|Indirizzo di posta elettronica AD FS 1.x|Indirizzo di posta elettronica dell'utente quando interagisce con AD FS 1.1 o AD FS 1.0.|
|Gruppo|Gruppo a cui appartiene l'utente.|
|UPN AD FS 1.x|UPN dell'utente quando interagisce con AD FS 1.1 o AD FS 1.0.|
|Ruolo|Ruolo dell'utente.|
|Surname|Cognome dell'utente.|
|PPID|Identificatore privato dell'utente.|
|ID nome|Identificatore nome SAML dell'utente.|
|Timestamp autenticazione|Utilizzato per visualizzare l'ora e la data in cui l'utente è stato autenticato.|
|Metodo di autenticazione|Metodo utilizzato per autenticare l'utente.|
|SID gruppo di sola negazione|SID di gruppo di sola negazione dell'utente.|
|SID primario di sola negazione|SID primario di sola negazione dell'utente.|
|SID gruppo primario di sola negazione|SID di gruppo primario di sola negazione dell'utente.|
|SID gruppo|SID di gruppo dell'utente.|
|SID gruppo primario|SID di gruppo primario dell'utente.|
|SID primario|SID primario dell'utente.|
|Nome account Windows|Nome account di dominio dell'utente in formato dominio\utente.|
|È un utente registrato|L'utente è registrato per utilizzare il dispositivo.|
|Identificatore dispositivo|Identificatore del dispositivo.|
|Identificatore di registrazione del dispositivo|Identificatore per la registrazione del dispositivo.|
|Nome visualizzato registrazione dispositivo|Nome visualizzato della registrazione del dispositivo.|
|Tipo di sistema operativo del dispositivo|Tipo di sistema operativo del dispositivo.|
|Versione del sistema operativo del dispositivo|Versione del sistema operativo del dispositivo.|
|È un dispositivo gestito|Il dispositivo è gestito da un servizio di gestione.|
|IP client inoltrato|Indirizzo IP dell'utente.|
|Applicazione client|Tipo di applicazione client.|
|Agente utente del client|Tipo di dispositivo utilizzato dal client per accedere all'applicazione.|
|IP client|Indirizzo IP del client.|
|Percorso endpoint|Percorso endpoint assoluto che può essere utilizzato per distinguere i client attivi dai client passivi.|
|Proxy|Nome DNS del proxy server federativo che ha trasmesso la richiesta.|
|Identificatore applicazione|Identificatore componente.|
|Criteri di applicazione|Criteri applicazione del certificato.|
|Identificatore di chiave dell'autorità|Estensione identificatore di chiave dell'autorità del certificato che ha firmato un certificato emesso.|
|Vincoli base|Uno dei vincoli base del certificato.|
|Utilizzo chiavi avanzato|Descrive uno degli utilizzi chiavi avanzati del certificato.|
|Issuer|Nome dell'autorità di certificazione che ha emesso il certificato X.509.|
|Nome dell'autorità di certificazione|Nome distinto dell'autorità di certificazione.|
|Utilizzo chiavi|Uno degli utilizzi chiavi del certificato.|
|Non dopo|Data nell'ora locale dopo la quale un certificato non è più valido.|
|Non prima|Data nell'ora locale in cui un certificato diventa valido.|
|Criteri del certificato|Criteri secondo i quali è stato emesso il certificato.|
|Chiave pubblica|Chiave pubblica del certificato.|
|Dati non elaborati certificato|Dati non elaborati del certificato.|
|Nome alternativo soggetto|Uno dei nomi alternativi del certificato.|
|Numero di serie|Numero di serie di un certificato.|
|Algoritmo di firma|Algoritmo utilizzato per creare la firma di un certificato.|
|Subject|Soggetto del certificato.|
|Identificatore della chiave del soggetto|Identificatore della chiave del soggetto del certificato.|
|Nome soggetto|Nome distinto del soggetto di un certificato.|
|Nome modello V2|Nome del modello di certificato versione 2 utilizzato per emettere o rinnovare un certificato. Il valore è specifico di Microsoft.|
|Nome modello V1|Nome del modello di certificato versione 1 utilizzato per emettere o rinnovare un certificato. Il valore è specifico di Microsoft.|
|Identificazione personale|Identificazione personale del certificato.|
|Versione X.509|Versione formato X.509 di un certificato.|
|All'interno della rete aziendale|Consente di indicare se una richiesta proviene dalla rete aziendale.|
|Ora scadenza password|Consente di visualizzare l'ora di scadenza della password.|
|Giorni a scadenza password|Consente di visualizzare il numero di giorni che mancano alla scadenza della password.|
|URL aggiornamento password|Consente di visualizzare l'indirizzo Web del servizio di aggiornamento password.|
|Riferimenti dei metodi di autenticazione|Consente di indicare tutti i metodi di autenticazione utilizzati per autenticare l'utente.|

## <a name="BKMK_2"></a>Gestione dei rischi con il controllo degli accessi condizionali
Le impostazioni disponibili offrono numerose soluzioni per gestire i rischi grazie all'implementazione del controllo di accesso condizionale.

### <a name="common-scenarios"></a>Scenari comuni
Si supponga, ad esempio, un semplice scenario di implementazione del controllo di accesso condizionale in base ai dati di appartenenza a gruppi dell'utente per una particolare applicazione (relying party Trust). In altre parole, è possibile configurare una regola di autorizzazione di rilascio nel server federativo per consentire agli utenti che appartengono a un determinato gruppo nel dominio di Active Directory di accedere a una particolare applicazione protetta da AD FS.  Le istruzioni dettagliate per l'implementazione di questo scenario tramite l'interfaccia utente e Windows PowerShell sono disponibili in [Walkthrough Guide: Manage Risk with Conditional Access Control](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md). Per completare i passaggi di questa procedura dettagliata, è necessario configurare un ambiente lab e seguire i passaggi descritti in [configurare l'ambiente lab per ad FS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

### <a name="advanced-scenarios"></a>Scenari avanzati
Altri esempi di implementazione del controllo di accesso condizionale in AD FS in Windows Server 2012 R2 includono quanto segue:

-   Consentire l'accesso a un'applicazione protetta da AD FS solo se l'identità dell'utente è stata convalidata con l'autenticazione a più fattori

    È possibile usare il codice seguente:

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "PermitAccessWithMFA"
    c:[Type == "https://schemas.microsoft.com/claims/authnmethodsreferences", Value =~ "^(?i)https://schemas\.microsoft\.com/claims/multipleauthn$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

-   Consentire l'accesso a un'applicazione protetta da AD FS solo se la richiesta di accesso è proveniente da un dispositivo aggiunto all'area di lavoro registrato per l'utente

    È possibile usare il codice seguente:

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "PermitAccessFromRegisteredWorkplaceJoinedDevice"
    c:[Type == "https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", Value =~ "^(?i)true$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

-   Consentire l'accesso a un'applicazione protetta da AD FS solo se la richiesta di accesso è proveniente da un dispositivo aggiunto all'area di lavoro registrato per un utente la cui identità è stata convalidata con l'autenticazione a più fattori

    È possibile usare il codice seguente:

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "RequireMFAOnRegisteredWorkplaceJoinedDevice"
    c1:[Type == "https://schemas.microsoft.com/claims/authnmethodsreferences", Value =~ "^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$"] &&
    c2:[Type == "https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", Value =~ "^(?i)true$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

-   Consentire l'accesso Extranet a un'applicazione protetta da AD FS solo se la richiesta di accesso è proveniente da un utente la cui identità è stata convalidata con l'autenticazione a più fattori.

    È possibile usare il codice seguente:

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "RequireMFAForExtranetAccess"
    c1:[Type == "https://schemas.microsoft.com/claims/authnmethodsreferences", Value =~ "^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$"] &&
    c2:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value =~ "^(?i)false$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

## <a name="see-also"></a>Vedi anche
[Guida allo scenario: gestire i rischi con il controllo degli accessi condizionali](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md)
[configurare l'ambiente lab per ad FS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



