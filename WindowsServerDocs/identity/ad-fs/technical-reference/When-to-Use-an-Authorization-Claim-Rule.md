---
ms.assetid: b734cbcb-342c-4a28-8ab5-b9cd990bb1c2
title: When to Use an Authorization Claim Rule
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 144e382692e8f2a6732f8c7c5b8a1dd6049192cb
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="when-to-use-an-authorization-claim-rule"></a>When to Use an Authorization Claim Rule
È possibile utilizzare questa regola in Active Directory Federation Services \(AD FS\) quando è necessario richiedere un tipo di attestazione in ingresso e quindi applicare un'azione che determinerà se un utente sarà consentito o negato l'accesso in base al valore specificato nella regola. Quando si usa questa regola, si pass-through o si trasformano le attestazioni corrispondenti alla logica della regola seguente, in base all'opzione configurata nella regola:  
  
|Opzione della regola|Logica della regola|  
|---------------|--------------|  
|Consentire tutti gli utenti|Se il tipo di attestazione in ingresso è uguale a *qualsiasi tipo di attestazione* e il valore è uguale a *qualsiasi valore*, rilasciare un'attestazione con valore uguale a *consentire l'accesso*|  
|Consentire l'accesso agli utenti con questa attestazione in ingresso|Se il tipo di attestazione in ingresso è uguale a *tipo di attestazione specificato* e il valore è uguale a *valore attestazione specificato*, rilasciare un'attestazione con valore uguale a *consentire l'accesso*|  
|Nega accesso agli utenti con questa attestazione in ingresso|Se il tipo di attestazione in ingresso è uguale a *tipo di attestazione specificato* e il valore è uguale a *valore attestazione specificato*, rilasciare un'attestazione con valore uguale a *Nega*|  
  
Le sezioni seguenti forniscono un'introduzione di base per le regole di attestazione e fornire ulteriori dettagli su quando usare questa regola.  
  
## <a name="about-claim-rules"></a>Informazioni sulle regole attestazioni  
Una regola attestazioni rappresenta un'istanza della logica di business che recupera un'attestazione in ingresso, applica una condizione \ (se x quindi y\) e genera un'attestazione in uscita in base ai parametri di condizione. L'elenco seguente fornisce importanti suggerimenti che è necessario conoscere sulle regole attestazione prima di procedere con la lettura di questo argomento:  
  
-   Nella gestione di ADFS snap-in, attestazione regole possono essere create solo utilizzando modelli di regola attestazione  
  
-   Processo di regole attestazione in ingresso di attestazioni direttamente da un provider di attestazioni \ (ad esempio Active Directory o un altro Service \ federativo) oppure dall'output dell'accettazione regole in un trust di provider di attestazioni di trasformazione.  
  
-   Le regole attestazioni vengono elaborate dal motore di rilascio delle attestazioni in ordine cronologico entro un determinato set di regole. Impostando la precedenza sulle regole, è possibile perfezionare ulteriormente o filtrare le attestazioni generate dalle regole precedenti all'interno di un determinato set di regole.  
  
-   Modelli di regola attestazione verranno sempre necessario specificare un tipo di attestazione in ingresso. Tuttavia, è possibile elaborare più valori di attestazione con lo stesso tipo di attestazione usando una singola regola.  
  
Per ulteriori informazioni sulle regole attestazione e set di regole attestazione, vedere [ruolo delle attestazioni regole](The-Role-of-Claim-Rules.md). Per ulteriori informazioni su come vengono elaborate le regole, vedere [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md). Per ulteriori informazioni, modalità di elaborazione dei set di regole attestazione, vedere [ruolo delle Pipeline delle attestazioni](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="permit-all-users"></a>Consentire tutti gli utenti  
Quando si usa il modello di regola consentire tutti gli utenti, tutti gli utenti avranno accesso alla relying party. Tuttavia, è possibile utilizzare le regole di autorizzazione aggiuntive per limitare ulteriormente l'accesso. Se una regola consente a un utente di accedere alla relying party e un'altra regola Nega l'accesso alla relying party, il risultato di negazione sostituisce il risultato di autorizzazione e l'utente viene negato l'accesso.  
  
Gli utenti sono consentiti l'accesso alla relying party dal servizio federativo potrebbero comunque essere negato l'accesso dalla relying party.  
  
## <a name="permit-access-to-users-with-this-incoming-claim"></a>Consentire l'accesso agli utenti con questa attestazione in ingresso  
Quando si utilizza consentire o negare agli utenti in base in un modello di regola attestazione in ingresso per creare una regola e impostare la condizione su Consenti, è possibile consentire l'accesso dell'utente specifico per la relying party in base al tipo e il valore di un'attestazione in ingresso. Ad esempio, è possibile utilizzare questo modello di regola per creare una regola che consenta solo gli utenti che dispongono di un gruppo di un'attestazione con un valore del gruppo Domain Admins. Se una regola consente a un utente di accedere alla relying party e un'altra regola Nega l'accesso alla relying party, il risultato di negazione sostituisce il risultato di autorizzazione e l'utente viene negato l'accesso.  
  
Gli utenti autorizzati ad accedere alla relying party dal servizio federativo potrebbero comunque essere negato l'accesso dalla relying party. Se si desidera consentire tutti gli utenti di accedere alla relying party, usare il modello di regola consentire tutti gli utenti.  
  
## <a name="deny-access-to-users-with-this-incoming-claim"></a>Nega accesso agli utenti con questa attestazione in ingresso  
Quando si utilizza il Permit or Deny Users Based in un modello di regola attestazione in ingresso per creare una regola e impostare la condizione su Nega, è possibile negare accesso l'alla relying party in base al tipo e il valore di un'attestazione in ingresso. Ad esempio, è possibile utilizzare questo modello di regola per creare una regola che neghi a tutti gli utenti che dispongono di un gruppo di un'attestazione con un valore di Domain Users.  
  
Se si desidera utilizzare la condizione Nega, ma anche abilitare l'accesso alla relying party per utenti specifici, è necessario aggiungere in seguito in modo esplicito le regole di autorizzazione con la condizione Consenti per abilitare l'accesso degli utenti alla relying party.  
  
Se un utente viene negato l'accesso quando il motore di rilascio delle attestazioni elabora il set di regole, arresta l'elaborazione delle regole ulteriore e ADFS restituisce un errore "Accesso negato" alla richiesta dell'utente.  
  
## <a name="authorizing-users"></a>Autorizzare gli utenti  
In ADFS, le regole di autorizzazione vengono usate per rilasciare consentire o negare l'attestazione che determinerà se un utente o un gruppo di utenti \ (a seconda used\ il tipo di attestazione) potranno accedere alle risorse basata sul Web in una determinata relying party o non. Le regole di autorizzazione possono essere impostate solo su trust della relying party.  
  
### <a name="authorization-rule-sets"></a>Set di regole di autorizzazione  
Set di regole di autorizzazione diverso esiste in base al tipo di consentire o negare operazioni che è necessario configurare. Questi set di regole includono:  
  
-   **Regole di autorizzazione rilascio**: queste regole determinano se un utente può ricevere attestazioni per una relying party e, pertanto, accedere alla relying party.  
  
-   **Regole di autorizzazione di delega**: queste regole determinano se un utente può agire come un altro utente per la relying party. Quando un utente viene utilizzato come un altro utente, le attestazioni relative all'utente richiedente sono ancora posizionate nel token.  
  
-   **Regole di autorizzazione di rappresentazione**: queste regole determinano se un utente può rappresentare completamente un altro utente per la relying party. La rappresentazione di un altro utente è una funzionalità molto potente, perché la relying party non riconoscerà che l'utente è rappresentato.  
  
Per ulteriori informazioni su come il processo di regola di autorizzazione si inserisce nella pipeline di rilascio delle attestazioni, vedere ruolo del motore di rilascio delle attestazioni.  
  
### <a name="supported-claim-types"></a>Tipi di attestazione supportati  
AD FSdefines due tipi che vengono utilizzati per determinare se un utente è consentito o negato l'attestazione. Questi attestazione Uniform Resource Identifier \(URIs\) sono i seguenti:  
  
1.  **Consenti**: http:///\/schemas.microsoft.com\/authorization\/claims\/permit  
  
2.  **Nega**: http:///\/schemas.microsoft.com\/authorization\/claims\/deny  
  
## <a name="how-to-create-this-rule"></a>Come creare questa regola  
È possibile creare entrambe le regole di autorizzazione usando il linguaggio delle regole attestazione oppure il **consentire tutti gli utenti** modello di regola o **consentire o negare agli utenti in base a un'attestazione in ingresso** modello di regola nello snap-in di gestione di ADFS. Il modello di regola consentire tutti gli utenti non include opzioni di configurazione. Tuttavia, consentire o negare agli utenti in base a un modello di regola attestazione in ingresso offre le opzioni di configurazione seguenti:  
  
-   Specificare un nome di regola attestazione  
  
-   Specificare un tipo di attestazione in ingresso  
  
-   Digitare un valore attestazione in ingresso  
  
-   Consentire l'accesso agli utenti con questa attestazione in ingresso  
  
-   Nega accesso agli utenti con questa attestazione in ingresso  
  
Per ulteriori istruzioni su come creare questo modello, vedere [creare una regola per consentire tutti gli utenti](https://technet.microsoft.com/library/ee913577.aspx) o [creare una regola per consentire o negare agli utenti in base un'attestazione in ingresso](https://technet.microsoft.com/library/ee913594.aspx) nella Guida alla distribuzione di ADFS.  
  
## <a name="using-the-claim-rule-language"></a>Utilizzando il linguaggio di regola attestazione  
Se un'attestazione deve essere inviata solo quando il valore dell'attestazione corrisponde a un modello personalizzato, è necessario utilizzare una regola personalizzata. Per ulteriori informazioni, vedere [When to Use a Custom Claim Rule](When-to-Use-a-Custom-Claim-Rule.md).  
  
### <a name="example-of-how-to-create-an-authorization-rule-based-on-multiple-claims"></a>Esempio di come creare una regola di autorizzazione basata su più attestazioni  
Quando si usa la sintassi di linguaggio delle regole attestazioni per autorizzare le attestazioni, anche un'attestazione può essere rilasciata in base alla presenza di più attestazioni nelle attestazioni originali dell'utente. La regola seguente rilascia un'attestazione di autorizzazione solo se l'utente è un membro del gruppo Editors e si è autenticato usando l'autenticazione Windows:  
  
```  
[type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod",   
value == "urn:federation:authentication:windows" ]  
&& [type == "http://schemas.xmlsoap.org/claims/Group ", value == “editors”]   
=> issue(type = "http://schemas.xmlsoap.org/claims/authZ", value = "Granted");  
```  
  
### <a name="example-of-how-to-create-authorization-rules-that-will-delegate-who-can-create-or-remove-federation-server-proxy-trusts"></a>Regole di esempio di come creare l'autorizzazione che delegano chi può creare o rimuovere trust di proxy server federativo  
Prima di un servizio federativo è possibile utilizzare un proxy server federativo per reindirizzare le richieste dei client, è necessario prima stabilire una relazione di trust tra il servizio federativo e il computer proxy server federativo. Per impostazione predefinita, viene stabilita una relazione di trust proxy quando uno delle credenziali seguenti vengono fornite correttamente nella configurazione guidata di AD FS Federation Server Proxy:  
  
-   L'account del servizio usato dal servizio federativo, che sarà protetto dal proxy  
  
-   Un account di dominio Active Directory che è un membro del gruppo Administrators locale su tutti i server federativi in una server farm federativa  
  
Quando si desidera specificare l'utente o gli utenti possono creare un trust proxy per un determinato servizio federativo, è possibile utilizzare uno dei seguenti metodi di delega. Questo elenco di metodi è in ordine di priorità, in base alle raccomandazioni del team del prodotto AD FS dei metodi di delega più sicuri e meno problematici. È necessario usare solo uno di questi metodi, a seconda delle esigenze dell'organizzazione:  
  
1.  Creare un gruppo di sicurezza di dominio in Active Directory \ (ad esempio, FSProxyTrustCreators\), aggiungere questo gruppo al gruppo Administrators locale in ogni server federativo nella farm e quindi aggiungere solo gli account utente a cui si desidera delegare questo diritto al nuovo gruppo. Questo è il metodo preferito.  
  
2.  Aggiungere l'account di dominio al gruppo Administrators in ogni server federativo nella farm.  
  
3.  Se per qualche motivo non è possibile utilizzare uno di questi metodi, è anche possibile creare una regola di autorizzazione a questo scopo. Sebbene non sia consigliabile, a causa delle possibili complicazioni che possono verificarsi se questa regola non viene scritta correttamente, è possibile utilizzare una regola di autorizzazione per delegare quali Active Directory gli account utente di dominio possono inoltre creare o anche rimuovere i trust tra tutti i server federativi che sono associati a un determinato servizio federativo.  
  
    Se si sceglie il metodo 3, è possibile utilizzare la sintassi della regola seguente per rilasciare un'attestazione di autorizzazione che consentirà agli utenti specificati \ (in questo caso, contoso\\frankm\) per creare trust per uno o più proxy server federativi per il servizio federativo. È necessario applicare questa regola utilizzando il comando Windows PowerShell **set-ADFSProperties AddProxyAuthorizationRules**.  
  
    ```  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", issuer=~"^AD AUTHORITY$" value == "contoso\frankm" ] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true")  
  
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-544", Issuer =~ "^AD AUTHORITY$"])   
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^AD AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustManagerSid({0})", param= c.Value );  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/proxytrustid", Issuer =~ "^SELF AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustProvisioned({0})", param=c.Value );  
    ```  
  
    In un secondo momento, se si desidera rimuovere l'utente in modo che l'utente non possa più creare trust proxy, è possibile ripristinare la regola di autorizzazione trust proxy predefinita per rimuovere il diritto dell'utente creare trust proxy per il servizio federativo. È anche necessario applicare questa regola utilizzando il comando Windows PowerShell **set-ADFSProperties AddProxyAuthorizationRules**.  
  
    ```  
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-544", Issuer =~ "^AD AUTHORITY$"])   
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^AD AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustManagerSid({0})", param= c.Value );  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/proxytrustid", Issuer =~ "^SELF AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustProvisioned({0})", param=c.Value );  
    ```  
  
Per ulteriori informazioni su come usare il linguaggio di regola attestazione, vedere [il ruolo del linguaggio di regola attestazione di](The-Role-of-the-Claim-Rule-Language.md).  
  

