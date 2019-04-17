---
ms.assetid: a0f7bb11-47a5-47ff-a70c-9e6353382b39
title: Gestire i rischi con il controllo di accesso condizionale
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e2ad7d1467abd6d69077b515b8c69a65f7e70f19
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="manage-risk-with-conditional-access-control"></a>Gestire i rischi con il controllo di accesso condizionale

>Si applica a: Windows Server 2012 R2


-   [Controllo di accesso condizionale concetti chiave in ADFS](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md#BKMK_1)

-   [Gestione dei rischi con il controllo di accesso condizionale](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md#BKMK_2)

## <a name="BKMK_1"></a>Concetti fondamentali - controllo di accesso condizionale in AD FS
La funzione generale di AD FS consiste nel rilasciare un token di accesso che contiene un set di attestazioni. La decisione che riguarda le attestazioni ADFS accetta e quindi rilascia è governata dalle regole attestazione.

Controllo di accesso in ADFS viene implementato con regole attestazione di autorizzazione rilascio usate per rilasciare consentire o negare le attestazioni che determineranno se un utente o un gruppo di utenti potranno accedere alle risorse protette da ADFS di Active Directory o non. Le regole di autorizzazione possono essere impostate solo su trust della relying party.

|Opzione della regola|Logica della regola|
|---------------|--------------|
|Consentire tutti gli utenti|Se il tipo di attestazione in ingresso è uguale a *qualsiasi tipo di attestazione* e il valore è uguale a *qualsiasi valore*, rilasciare un'attestazione con valore uguale a *consentire l'accesso*|
|Consentire l'accesso agli utenti con questa attestazione in ingresso|Se il tipo di attestazione in ingresso è uguale a *tipo di attestazione specificato* e il valore è uguale a *valore attestazione specificato*, rilasciare un'attestazione con valore uguale a *consentire l'accesso*|
|Nega accesso agli utenti con questa attestazione in ingresso|Se il tipo di attestazione in ingresso è uguale a *tipo di attestazione specificato* e il valore è uguale a *valore attestazione specificato*, rilasciare un'attestazione con valore uguale a *Nega*|

Per ulteriori informazioni su queste opzioni delle regole e logica, vedere [When to Use an Authorization Claim Rule](https://technet.microsoft.com/library/ee913560.aspx).

In ADFS in Windows Server 2012 R2, il controllo degli accessi è stato ottimizzato con più fattori, tra cui l'utente, dispositivo, posizione e dati di autenticazione. Questo è reso possibile da un'ampia gamma di tipi di attestazione disponibile per le regole di attestazione di autorizzazione.  In ADFS in Windows Server 2012 R2, in altre parole, è possibile applicare il controllo di accesso condizionale basato su utente identità o appartenenza al gruppo, percorso di rete, dispositivo (se è all'area di lavoro aggiunto, per ulteriori informazioni, vedere [accedere a una rete aziendale da qualsiasi dispositivo per SSO e trasparente secondo fattore di autenticazione tutte le applicazioni aziendali](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)) e lo stato di autenticazione (se è stata eseguita l'autenticazione a più fattori (MFA)).

Controllo di accesso condizionale in ADFS in Windows Server 2012 R2 offre i vantaggi seguenti:

-   Criteri di autorizzazione per applicazione flessibili ed espressivi, in base al quale è possibile consentire o negare l'accesso in base a utente, dispositivo, percorso di rete e stato di autenticazione

-   La creazione di regole di autorizzazione per applicazioni relying party di rilascio

-   Esperienza Rich UI per gli scenari comuni di controllo di accesso condizionale

-   Linguaggio esteso per le attestazioni e Windows PowerShell supporto per gli scenari di controllo di accesso condizionale avanzato

-   Personalizzato (per ogni componente applicazione di terze parti) messaggi di "Accesso negato". Per ulteriori informazioni, vedere [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx). Grazie alla possibilità di personalizzare questi messaggi, è possibile spiegare perché un utente viene negato l'accesso e facilita inoltre la risoluzione correzione self-service in cui è possibile, ad esempio richiedendo agli utenti di rete aziendale aggiunta i dispositivi. Per ulteriori informazioni, vedere [accedere a una rete aziendale da qualsiasi dispositivo per SSO e trasparente secondo fattore di autenticazione tutte le applicazioni aziendali](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

La tabella seguente include tutti i tipi di attestazione disponibili in ADFS in Windows Server 2012 R2 per essere utilizzato per l'implementazione del controllo di accesso condizionale.

|Tipo di attestazione|Descrizione|
|--------------|---------------|
|Indirizzo di posta elettronica|L'indirizzo di posta elettronica dell'utente.|
|Nome specificato|Il nome dell'utente.|
|Nome|Il nome univoco dell'utente,|
|UPN|Il principale nome utente (UPN) dell'utente.|
|Nome comune|Il nome comune dell'utente.|
|AD FS 1. x indirizzo di posta elettronica|L'indirizzo di posta elettronica dell'utente quando interagisce con AD FS 1.1 o AD FS 1.0.|
|Gruppo|L'utente è membro di un gruppo.|
|AD FS 1 x UPN|UPN dell'utente quando interagisce con AD FS 1.1 o AD FS 1.0.|
|Ruolo|Ruolo dell'utente.|
|Cognome|Cognome dell'utente.|
|PPID|Identificatore privato dell'utente.|
|ID nome|Identificatore nome SAML dell'utente.|
|Timestamp autenticazione|Consente di visualizzare la data e che l'utente è stato autenticato.|
|Metodo di autenticazione|Il metodo utilizzato per autenticare l'utente.|
|SID gruppo di sola negazione|Il sola negazione SID di gruppo dell'utente.|
|SID primario di sola negazione|Il SID primario sola negazione dell'utente.|
|Nega solo SID gruppo primario di|Il sola negazione SID gruppo primario dell'utente.|
|SID di gruppo|Il SID di gruppo dell'utente.|
|SID gruppo primario di|Il SID gruppo primario dell'utente.|
|SID primario di|SID primario dell'utente.|
|Nome account di Windows|Nome dell'account di dominio dell'utente nel formato dominio\utente..|
|È utente registrato|L'utente è registrato per l'utilizzo di questo dispositivo.|
|Identificatore dispositivo|Identificatore del dispositivo.|
|Identificatore di registrazione del dispositivo|Identificatore per la registrazione del dispositivo.|
|Nome visualizzato registrazione dispositivo|Nome visualizzato della registrazione del dispositivo.|
|Tipo di dispositivo del sistema operativo|Tipo di sistema operativo del dispositivo.|
|Versione del sistema operativo del dispositivo|Versione del sistema operativo del dispositivo.|
|È un dispositivo gestito|Dispositivo è gestito da un servizio di gestione.|
|Client inoltrato IP|Indirizzo IP dell'utente.|
|Applicazione client|Tipo di applicazione client.|
|Agente utente del client|Tipo di dispositivo client utilizzato per accedere all'applicazione.|
|Client IP|Indirizzo IP del client.|
|Percorso endpoint|Percorso Endpoint assoluto che può essere utilizzato per determinare attivi dai client passivi.|
|Proxy|Nome DNS del proxy server federativo che ha trasmesso la richiesta.|
|Identificatore dell'applicazione|Identificatore per la relying party.|
|Criteri di applicazione|Criteri di applicazione del certificato.|
|Identificatore di chiave dell'autorità|Estensione identificatore di chiave dell'autorità del certificato che ha firmato un certificato emesso.|
|Limitazione di base|Uno dei vincoli base del certificato.|
|Utilizzo chiavi avanzato|Descrive uno degli utilizzi chiavi avanzati del certificato.|
|Autorità di certificazione|Il nome dell'autorità di certificazione che ha emesso il certificato x. 509.|
|Nome dell'autorità di certificazione|Il nome distinto dell'autorità emittente del certificato.|
|Utilizzo delle chiave|Uno degli utilizzi chiavi del certificato.|
|Non dopo|Data nell'ora locale dopo che un certificato non è più valido.|
|Non prima|Il data nell'ora locale in cui un certificato diventa valido.|
|Criteri dei certificati|I criteri in cui è stato emesso il certificato.|
|Chiave pubblica|Chiave pubblica del certificato.|
|Dati non elaborati certificato|I dati non elaborati del certificato.|
|Nome alternativo del soggetto|Uno dei nomi alternativi del certificato.|
|Numero di serie|Il numero di serie del certificato.|
|Algoritmo di firma|L'algoritmo utilizzato per creare la firma di un certificato.|
|Oggetto|Il soggetto del certificato.|
|Identificatore di chiave dell'oggetto|L'identificatore del certificato.|
|Nome del soggetto|Il nome distinto dell'oggetto da un certificato.|
|V2 Nome modello|Il nome del modello di certificato 2 versione utilizzato per emettere o rinnovare un certificato. Questo è un valore specifico di Microsoft.|
|V1 Nome modello|Il nome del modello di certificato versione 1 utilizzato per emettere o rinnovare un certificato. Questo è un valore specifico di Microsoft.|
|Identificazione personale|Identificazione personale del certificato.|
|Versione x. 509|Versione formato x. 509 del certificato.|
|Rete aziendale interna|Consente di indicare se una richiesta proviene dalla rete aziendale.|
|Scadenza delle password|Usato per visualizzare l'ora di scadenza della password.|
|Giorni a scadenza password|Consente di visualizzare il numero di giorni alla scadenza della password.|
|URL aggiornamento Password|Consente di visualizzare l'indirizzo web del servizio di aggiornamento della password.|
|Riferimenti dei metodi di autenticazione|Consente di indicare al metodi di autenticazione utilizzati per autenticare l'utente.|

## <a name="BKMK_2"></a>Gestione dei rischi con il controllo di accesso condizionale
Utilizzando le impostazioni disponibili, esistono molti modi in cui è possibile gestire i rischi con l'implementazione di controllo di accesso condizionale.

### <a name="common-scenarios"></a>Scenari comuni
Ad esempio, un semplice scenario di implementazione del controllo di accesso condizionale in base ai dati di appartenenza al gruppo dell'utente per una particolare applicazione (trust della relying party). In altre parole, è possibile impostare una regola di autorizzazione di rilascio nel server federativo per permettere agli utenti che appartengono a un determinato gruppo in Active Directory accesso al dominio a una specifica applicazione protetta da ADFS.  Passo passo istruzioni dettagliate (tramite l'interfaccia utente e Windows PowerShell) per l'implementazione di questo scenario sono trattate in [Guida allo scenario: gestire i rischi con il controllo di accesso condizionale](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md). Per completare i passaggi descritti in questa procedura dettagliata, è necessario configurare un ambiente di testing e seguire i passaggi descritti in [impostare dell'ambiente di testing per ADFS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

### <a name="advanced-scenarios"></a>Scenari avanzati
Altri esempi di implementazione del controllo di accesso condizionale in ADFS in Windows Server 2012 R2 includono quanto segue:

-   Consentire l'accesso a un'applicazione protetta da ADFS solo se l'identità dell'utente viene convalidata con autenticazione a più fattori

    È possibile utilizzare il codice seguente:

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "PermitAccessWithMFA"
    c:[Type == "https://schemas.microsoft.com/claims/authnmethodsreferences", Value =~ "^(?i)https://schemas\.microsoft\.com/claims/multipleauthn$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

-   Consentire l'accesso a un'applicazione protetta da ADFS solo se la richiesta di accesso proviene da un dispositivo aggiunto all'area di lavoro registrato per l'utente

    È possibile utilizzare il codice seguente:

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "PermitAccessFromRegisteredWorkplaceJoinedDevice"
    c:[Type == "https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", Value =~ "^(?i)true$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

-   Consentire l'accesso a un'applicazione protetta da ADFS solo se la richiesta di accesso proviene da un dispositivo aggiunto all'area di lavoro registrato per un utente la cui identità viene convalidata con autenticazione a più fattori

    È possibile utilizzare il codice seguente

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "RequireMFAOnRegisteredWorkplaceJoinedDevice"
    c1:[Type == "https://schemas.microsoft.com/claims/authnmethodsreferences", Value =~ "^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$"] &&
    c2:[Type == "https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", Value =~ "^(?i)true$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

-   Consentire l'accesso extranet a un'applicazione protetta da ADFS solo se la richiesta di accesso proviene da un utente la cui identità viene convalidata con autenticazione a più fattori.

    È possibile utilizzare il codice seguente:

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "RequireMFAForExtranetAccess"
    c1:[Type == "https://schemas.microsoft.com/claims/authnmethodsreferences", Value =~ "^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$"] &&
    c2:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value =~ "^(?i)false$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

## <a name="see-also"></a>Vedere anche
[Guida allo scenario: Gestire i rischi con il controllo di accesso condizionale](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md)<ph x="2">
</ph>[impostare dell'ambiente di testing per ADFS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



