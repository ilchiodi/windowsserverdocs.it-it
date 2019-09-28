---
ms.assetid: b734cbcb-342c-4a28-8ab5-b9cd990bb1c2
title: Quando usare una regola attestazioni di autorizzazione
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 49246d9df294b966f0ba38b1d3c1f361ce5f1d5f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407266"
---
# <a name="when-to-use-an-authorization-claim-rule"></a>Quando usare una regola attestazioni di autorizzazione
È possibile utilizzare questa regola in Active Directory Federation Services \(ad FS\) quando è necessario eseguire un tipo di attestazione in ingresso e quindi applicare un'azione che determinerà se a un utente sarà consentito o negato l'accesso in base al valore specificare nella regola. Quando si usa questa regola, si consente l'ingresso o si trasformano le attestazioni corrispondenti alla logica della regola seguente, in base all'opzione configurata nella regola:  
  
|Opzione della regola|Logica della regola|  
|---------------|--------------|  
|Consentire l'accesso a tutti gli utenti|Se il tipo di attestazione in ingresso è uguale a *qualsiasi tipo di attestazione* e il valore è uguale a *qualsiasi valore*, rilasciare un'attestazione con valore uguale a *Consenti*|  
|Consenti accesso agli utenti con questa attestazione in ingresso|Se il tipo di attestazione in ingresso è uguale a *tipo di attestazione specificato* e il valore è uguale a *valore di attestazione specificato*, rilasciare un'attestazione con valore uguale a *Consenti*|  
|Nega accesso agli utenti con questa attestazione in ingresso|Se il tipo di attestazione in ingresso è uguale a *tipo di attestazione specificato* e il valore è uguale a *valore di attestazione specificato*, rilasciare un'attestazione con valore uguale a *Nega*|  
  
Le sezioni seguenti forniscono un'introduzione di base alle regole attestazioni e indicano i casi in cui è necessario usare questa regola.  
  
## <a name="about-claim-rules"></a>Informazioni sulle regole attestazioni  
Una regola attestazione rappresenta un'istanza di logica di business che recupera un'attestazione in ingresso, applicare una condizione \(Se x y quindi\) e produrre un'attestazione in uscita in base ai parametri di condizione. L'elenco seguente fornisce suggerimenti importanti sulle regole attestazioni da tenere presenti prima di procedere con la lettura di questo argomento:  
  
-   Nello snap di gestione di ADFS\-attestazione regole possono essere create solo utilizzando modelli di regola attestazione  
  
-   Processo di regole attestazione in ingresso di attestazioni direttamente da un provider di attestazioni \(ad esempio Active Directory o un altro servizio federativo\) o dall'output dell'accettazione di trasformare le regole in un trust di provider di attestazioni.  
  
-   Le regole attestazioni sono elaborate dal motore di rilascio delle attestazioni in ordine cronologico entro un determinato set di regole. Impostando la precedenza sulle regole, è possibile perfezionare o filtrare ulteriormente le attestazioni generate dalle regole precedenti all'interno di un set di regole specificato.  
  
-   Per i modelli di regola attestazioni è sempre necessario specificare un tipo di attestazione in ingresso. Tuttavia, è possibile elaborare più valori di attestazione con lo stesso tipo di attestazione usando una singola regola.  
  
Per ulteriori informazioni sulle regole attestazione e i set di regole attestazione, vedere [ruolo delle attestazioni regole](The-Role-of-Claim-Rules.md). Per ulteriori informazioni sulle modalità di elaborazione delle regole, vedere [il ruolo del motore di attestazioni](The-Role-of-the-Claims-Engine.md). Per ulteriori informazioni, modalità di elaborazione dei set di regole attestazione, vedere [ruolo delle Pipeline delle attestazioni](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="permit-all-users"></a>Consentire l'accesso a tutti gli utenti  
Quando si usa la regola Consentire l'accesso a tutti gli utenti, tutti gli utenti avranno accesso alla relying party. Per limitare ulteriormente l'accesso, è tuttavia possibile usare altre regole di autorizzazione. Se una regola consente a un utente di accedere alla relying party, mentre un'altra regola nega l'accesso, il risultato di quest'ultima sostituisce quello che della regola che lo consente, quindi all'utente viene negato l'accesso.  
  
Agli utenti che hanno ricevuto l'autorizzazione di accesso alla relying parti da parte del Servizio federativo potrebbe comunque essere negato l'accesso dalla stessa relying party.  
  
## <a name="permit-access-to-users-with-this-incoming-claim"></a>Consenti accesso agli utenti con questa attestazione in ingresso  
Quando si usa il modello di regola Consenti o nega utenti in base a un'attestazione in ingresso per creare una regola e si imposta la condizione su Consenti, è possibile consentire l'accesso di un utente specifico al relying party in base al tipo e al valore di un'attestazione in ingresso. Ad esempio, è possibile usare questo modello di regola per creare una regola che consenta solo a determinati utenti di avere un'attestazione basata su gruppo con un valore Domain Admins. Se una regola consente a un utente di accedere alla relying party, mentre un'altra regola nega l'accesso, il risultato di quest'ultima sostituisce quello che della regola che lo consente, quindi all'utente viene negato l'accesso.  
  
Agli utenti che hanno ricevuto l'autorizzazione di accesso alla relying parti da parte del Servizio federativo potrebbe comunque essere negato l'accesso dalla stessa relying party. Se si vuole consentire a tutti gli utenti di accedere alla relying party, usare il modello di regola Consentire l'accesso a tutti gli utenti.  
  
## <a name="deny-access-to-users-with-this-incoming-claim"></a>Nega accesso agli utenti con questa attestazione in ingresso  
Quando si usa il modello di regola Consenti o nega utenti in base a un'attestazione in ingresso per creare una regola e si imposta la condizione su Nega, è possibile negare l'accesso dell'utente al relying party in base al tipo e al valore di un'attestazione in ingresso. Ad esempio, è possibile usare questo modello di regola per creare una regola che neghi a tutti gli utenti di avere un'attestazione basata su gruppo con un valore Domain Admins.  
  
Se si vuole usare la condizione Nega e abilitare comunque l'accesso alla relying party per utenti specifici, è necessario aggiungere in seguito in modo esplicito le regole di autorizzazione con la condizione Consenti per abilitare l'accesso degli utenti alla relying party.  
  
Se a un utente viene negato l'accesso quando il motore di rilascio delle attestazioni elabora il set di regole, l'elaborazione di ulteriori regole viene arrestata e AD FS restituisce un errore "accesso negato" alla richiesta dell'utente.  
  
## <a name="authorizing-users"></a>Autorizzazione degli utenti  
In ad FS, le regole di autorizzazione vengono usate per emettere un'attestazione di autorizzazione o negazione che determinerà se un utente \(o un gruppo di utenti a seconda\) del tipo di attestazione usato\-sarà autorizzato ad accedere alle risorse basate sul Web in una determinata relying entità o meno. Le regole di autorizzazione possono essere impostate solo per i trust della relying party.  
  
### <a name="authorization-rule-sets"></a>Set di regole di autorizzazione  
Esistono diversi set di regole di autorizzazione, a seconda del tipo di operazioni consentite o negate che è necessario configurare. Questi set di regole includono:  
  
-   **Regole di autorizzazione rilascio**: Queste regole determinano se un utente può ricevere attestazioni per una relying party e perciò accedere alla relying party.  
  
-   **Regole di autorizzazione di delega**: determinano se un utente può agire come un altro utente per l'accesso alla relying party. In questo caso, le attestazioni relative all'utente richiedente sono ancora all'interno del token.  
  
-   **Regole di autorizzazione di rappresentazione**: determinano se un utente può rappresentare completamente un altro utente per l'accesso alla relying party. La rappresentazione di un altro utente è una funzionalità molto potente, perché la relying party non riconoscerà che l'utente è rappresentato.  
  
Per altri dettagli sul modo in cui il processo delle regole di autorizzazione si inserisce nella pipeline di rilascio delle autorizzazioni, vedere Ruolo del motore di rilascio delle attestazioni.  
  
### <a name="supported-claim-types"></a>Tipi di attestazione supportati  
AD FS definisce due tipi di attestazione usati per determinare se un utente è consentito o negato. Questi \(URI\) di tipo attestazione Uniform Resource Identifier sono i seguenti:  
  
1.  **Consenti**: http:\/\/schemas.Microsoft.com\/autorizzazione\/perattestazioni\/  
  
2.  **Deny**: http:\/\/\/schemas.Microsoft.com\/Negaattestazioni\/di autorizzazione  
  
## <a name="how-to-create-this-rule"></a>Come creare la regola  
È possibile creare entrambe le regole di autorizzazione usando il linguaggio delle regole attestazioni o il modello di regola **Consenti tutti gli utenti** oppure il modello di regola Consenti **o nega utenti in base a un'attestazione in ingresso** nello snap\--in gestione ad FS. Il modello di regola Consentire l'accesso a tutti gli utenti non include opzioni di configurazione. Il modello di regola Consentire o negare l'accesso agli utenti in base a un'attestazione in ingresso include invece le opzioni di configurazione seguenti:  
  
-   Specificare un nome della regola attestazione  
  
-   Specificare un tipo di attestazione in ingresso  
  
-   Immettere un valore attestazione in ingresso  
  
-   Consenti accesso agli utenti con questa attestazione in ingresso  
  
-   Nega accesso agli utenti con questa attestazione in ingresso  
  
Per altre istruzioni su come creare questo modello, vedere [creare una regola per consentire a tutti gli utenti](https://technet.microsoft.com/library/ee913577.aspx) o [creare una regola per consentire o negare agli utenti in base a un'attestazione in ingresso](https://technet.microsoft.com/library/ee913594.aspx) nella guida alla distribuzione di ad FS.  
  
## <a name="using-the-claim-rule-language"></a>Uso del linguaggio delle regole attestazioni  
Se un'attestazione deve essere inviata solo quando il relativo valore corrisponde a un modello personalizzato, è necessario usare una regola personalizzata. Per altre informazioni, vedere [quando usare una regola attestazioni personalizzata](When-to-Use-a-Custom-Claim-Rule.md).  
  
### <a name="example-of-how-to-create-an-authorization-rule-based-on-multiple-claims"></a>Esempio di creazione di una regola di autorizzazione basata su più attestazioni  
Quando si usa la sintassi del linguaggio delle regole attestazioni per autorizzare le attestazioni, è anche possibile emettere un'attestazione in base alla presenza di più attestazioni nelle attestazioni originali dell'utente. La regola seguente rilascia un'attestazione di autorizzazione solo se l'utente è membro del gruppo Editors e si è autenticato usando l'autenticazione di Windows:  
  
```  
[type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod",   
value == "urn:federation:authentication:windows" ]  
&& [type == "http://schemas.xmlsoap.org/claims/Group ", value == “editors”]   
=> issue(type = "http://schemas.xmlsoap.org/claims/authZ", value = "Granted");  
```  
  
### <a name="example-of-how-to-create-authorization-rules-that-will-delegate-who-can-create-or-remove-federation-server-proxy-trusts"></a>Esempio di creazione di regole di autorizzazione che delegano chi può creare o rimuovere trust di proxy server federativo  
Per consentire a un Servizio federativo di usare un proxy server federativo per reindirizzare le richieste dei client, è necessario stabilire prima un trust tra il Servizio federativo e il computer proxy server federativo. Per impostazione predefinita, un trust proxy viene stabilito quando le credenziali seguenti vengono fornite correttamente nella configurazione guidata del proxy server federativo di AD FS:  
  
-   Account del servizio usato dal Servizio federativo che sarà protetto dal proxy  
  
-   Account di dominio di Active Directory membro del gruppo Administrators locale in tutti i server federativi di una server farm federativa  
  
Per specificare l'utente o gli utenti autorizzati a creare un trust proxy per un determinato Servizio federativo, è possibile usare uno qualsiasi dei metodi di delega seguenti. Questo elenco di metodi è in ordine di priorità, in base alle raccomandazioni del team del prodotto AD FS dei metodi di delega più sicuri e meno problematici. È necessario usare solo uno di questi metodi, in base alle esigenze dell'organizzazione:  
  
1.  Creare un gruppo di sicurezza di dominio \(in Active Directory ad esempio\), FSProxyTrustCreators, aggiungere questo gruppo al gruppo Administrators locale in ogni server federativo della farm e quindi aggiungere solo gli account utente desiderati per delegare questo diritto al nuovo gruppo. Questo è il metodo preferito.  
  
2.  Aggiungere l'account di dominio dell'utente al gruppo Administrator in ogni server federativo della farm.  
  
3.  Se per qualche motivo non è possibile usare uno di questi metodi, si può anche usare una regola di autorizzazione a questo scopo. Anche se non è consigliabile, a causa delle possibili complicazioni che possono verificarsi se la regola non viene scritta correttamente, è possibile usare questa regola di autorizzazione per delegare gli account utente di dominio di Active Directory che possono creare o anche rimuovere i trust tra i proxy server federativi associati a un determinato Servizio federativo.  
  
    Se si sceglie il metodo 3, è possibile usare la sintassi della regola seguente per rilasciare un'attestazione di autorizzazione che consentirà a un utente \(specifico,\) in questo caso, Contoso\\Franky di creare trust per uno o più proxy server federativi nel Servizio federativo. Per applicare questa regola è necessario usare il **set\-** di comandi di Windows PowerShell ADFSProperties AddProxyAuthorizationRules.  
  
    ```  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", issuer=~"^AD AUTHORITY$" value == "contoso\frankm" ] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true")  
  
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-544", Issuer =~ "^AD AUTHORITY$"])   
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^AD AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustManagerSid({0})", param= c.Value );  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/proxytrustid", Issuer =~ "^SELF AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustProvisioned({0})", param=c.Value );  
    ```  
  
    In seguito, per rimuovere l'utente in modo che non possa più creare trust proxy, è possibile ripristinare la regola di autorizzazione per il trust proxy predefinita che rimuoverà il diritto dell'utente alla creazione di trust proxy per il Servizio federativo. È necessario applicare questa regola anche usando il **set\-** di comandi di Windows PowerShell ADFSProperties AddProxyAuthorizationRules.  
  
    ```  
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-544", Issuer =~ "^AD AUTHORITY$"])   
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^AD AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustManagerSid({0})", param= c.Value );  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/proxytrustid", Issuer =~ "^SELF AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustProvisioned({0})", param=c.Value );  
    ```  
  
Per ulteriori informazioni su come utilizzare il linguaggio di regola attestazione, vedere [il ruolo del linguaggio di regola attestazione](The-Role-of-the-Claim-Rule-Language.md).  
  

