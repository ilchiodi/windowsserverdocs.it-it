---
ms.assetid: 606f4196-b579-4806-a462-3abd4d93e87c
title: Quando utilizzare un inviare attributi LDAP come attestazioni regola
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 05a2dd88057b64675bbc3bd30724d1eda0880c44
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="when-to-use-a-send-ldap-attributes-as-claims-rule"></a>Quando utilizzare un inviare attributi LDAP come attestazioni regola
È possibile utilizzare questa regola in Active Directory Federation Services \(AD FS\) quando si desidera rilasciare attestazioni in uscita che contengono valori di attributo \(LDAP\) Lightweight Directory Access Protocol effettivi che esiste in un archivio attributi e quindi associare un tipo di attestazione con ciascuno degli attributi LDAP. Per ulteriori informazioni sugli archivi di attributi, vedere [il ruolo degli archivi attributi](The-Role-of-Attribute-Stores.md).  
  
Quando si usa questa regola, si rilascia un'attestazione per ogni attributo LDAP specificato e che corrisponde alla logica della regola, come descritto nella tabella seguente.  
  
|Opzione della regola|Logica della regola|  
|---------------|--------------|  
|Mapping degli attributi LDAP ai tipi di attestazione in uscita|Se l'archivio di attributi equivale *archivio di attributi specificato* e attributo LDAP equivale a *valore specificato*, il valore dell'attributo LDAP per eseguire il mapping di *attestazione in uscita specificato* digitare ed emettere l'attestazione.|  
  
Le sezioni seguenti forniscono un'introduzione di base per le regole di attestazione. Oltre a informazioni dettagliate su quando usare inviare attributi LDAP come attestazioni regola.  
  
## <a name="about-claim-rules"></a>Informazioni sulle regole attestazioni  
Una regola attestazioni rappresenta un'istanza della logica di business che recupera un'attestazione in ingresso, applica una condizione \ (se x quindi y\) e genera un'attestazione in uscita in base ai parametri di condizione. L'elenco seguente fornisce importanti suggerimenti che è necessario conoscere sulle regole attestazione prima di procedere con la lettura di questo argomento:  
  
-   Nella gestione di ADFS snap-in, attestazione regole possono essere create solo utilizzando modelli di regola attestazione  
  
-   Processo di regole attestazione in ingresso di attestazioni direttamente da un provider di attestazioni \ (ad esempio Active Directory o un altro Service \ federativo) oppure dall'output dell'accettazione regole in un trust di provider di attestazioni di trasformazione.  
  
-   Le regole attestazioni vengono elaborate dal motore di rilascio delle attestazioni in ordine cronologico entro un determinato set di regole. Impostando la precedenza sulle regole, è possibile perfezionare ulteriormente o filtrare le attestazioni generate dalle regole precedenti all'interno di un determinato set di regole.  
  
-   Modelli di regola attestazione verranno sempre necessario specificare un tipo di attestazione in ingresso. Tuttavia, è possibile elaborare più valori di attestazione con lo stesso tipo di attestazione usando una singola regola.  
  
Per ulteriori informazioni sulle regole attestazione e set di regole attestazione, vedere [ruolo delle attestazioni regole](The-Role-of-Claim-Rules.md). Per ulteriori informazioni su come vengono elaborate le regole, vedere [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md). Per ulteriori informazioni, modalità di elaborazione dei set di regole attestazione, vedere [ruolo delle Pipeline delle attestazioni](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="mapping-of-ldap-attributes-to-outgoing-claim-types"></a>Mapping degli attributi LDAP ai tipi di attestazione in uscita  
Quando si utilizza inviare attributi LDAP come modello di regola attestazioni, è possibile selezionare gli attributi da un archivio di attributi LDAP, ad esempio \(AD DS\) Active Directory o servizi di dominio Active Directory per inviare i relativi valori come attestazioni alla relying party. Esegue praticamente il mapping attributi LDAP specifici da un archivio di attributi definiti a un set di attestazioni in uscita che può essere utilizzato per l'autorizzazione.  
  
Utilizzando questo modello, è possibile aggiungere più attributi, che verranno inviati come più attestazioni, da una singola regola. Ad esempio, è possibile utilizzare questo modello di regola per creare una regola che eseguirà la ricerca di valori di attributo per gli utenti autenticati dal **società** e **reparto** attributi di Active Directory e quindi inviare questi valori come due diverse attestazioni in uscita.  
  
È inoltre possibile utilizzare questa regola per inviare l'appartenenza al gruppo dell'utente. Se si desidera inviare solo le singole appartenenze, utilizzare l'appartenenza al gruppo di invio come un modello di regola attestazione. Per ulteriori informazioni, vedere [When to Use a Send Group Membership come una regola attestazione](When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md).  
  
## <a name="how-to-create-this-rule"></a>Come creare questa regola  
È possibile creare questa regola utilizzando il linguaggio di regola attestazione o utilizzando inviare attributi LDAP come attestazioni regola modello di gestione di AD FS snap-in. Questo modello di regola offre le opzioni di configurazione seguenti:  
  
-   Specificare un nome di regola attestazione  
  
-   Selezionare un archivio di attributi da cui estrarre gli attributi LDAP  
  
-   Mapping degli attributi LDAP ai tipi di attestazione in uscita  
  
Per ulteriori informazioni su come creare questa regola, vedere [creare una regola per inviare attributi LDAP come attestazioni](https://technet.microsoft.com/library/dd807115.aspx).  
  
## <a name="using-the-claim-rule-language"></a>Utilizzando il linguaggio di regola attestazione  
Se la query di Active Directory, di dominio Active Directory o Active Directory Lightweight Directory Services \(AD LDS\) deve eseguire il confronto con un attributo LDAP diverso da **samAccountname**, è invece necessario utilizzare una regola personalizzata. Se è presente alcuna attestazione nome Account Windows nel set di input, è inoltre necessario utilizzare una regola personalizzata per specificare l'attestazione da usare per le query di dominio Active Directory o AD LDS.  
  
Gli esempi seguenti vengono forniti per aiutarti a comprendere alcuni dei modi in cui che è possibile creare una regola personalizzata usando il linguaggio di regola attestazioni per interrogare ed estrarre i dati in un archivio attributi.  
  
### <a name="example-how-to-query-an-ad-lds-attribute-store-and-return-a-specified-value"></a>Esempio: Come eseguire ricerche in un archivio di attributi AD LDS e restituire un valore specificato  
Parametri devono essere separati da un punto e virgola. Il primo parametro è il filtro LDAP. I parametri successivi sono gli attributi da restituire in tutti gli oggetti corrispondenti.  
  
L'esempio seguente mostra come cercare un utente con il **sAMAccountName** attributo e rilasciare un'attestazione di indirizzo di posta elettronica e\, usando il valore dell'attributo mail dell'utente:  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]  
=> issue(store = "AD LDS", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"), query = "sAMAccountName={0};mail", param = regexreplace(c.Value, "(?<domain>[^\\]+)\\(?<user>.+)", "${user}"));  
  
```  
  
L'esempio seguente mostra come cercare un utente con il **posta** attributo e rilasciare le attestazioni Title e Display Name usando i valori dell'utente **titolo** e **displayname** attributi:  
  
```  
c:[Type == " http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress ", Issuer == "AD AUTHORITY"]  
=> issue(store = "AD LDS ", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/title","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/displayname"), query = "mail={0};title;displayname", param = c.Value);  
```  
  
L'esempio seguente mostra come cercare un utente da posta e il titolo e quindi rilasciare un'attestazione Display Name usando l'utente **displayname** attributo:  
  
```  
c1:[Type == " http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"] && c2:[Type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/title"]  
=> issue(store = "AD LDS ", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/displayname"), query = "(&(mail={0})(title={1}));displayname", param = c1.Value, param = c2.Value);  
```  
  
### <a name="example-how-to-query-an-active-directory-attribute-store-and-return-a-specified-value"></a>Esempio: Come eseguire ricerche in un archivio di attributi di Active Directory e restituire un valore specificato  
La query di Active Directory deve includere il nome dell'utente \ (con name\ il dominio) come parametro finale in modo che l'archivio di attributi di Active Directory può eseguire query nel dominio appropriato. In caso contrario, è supportata la stessa sintassi.  
  
L'esempio seguente mostra come cercare un utente con il **sAMAccountName** dell'attributo nel proprio dominio e quindi restituire il **posta** attributo:  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]  
=> issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"), query = "sAMAccountName={0};mail;{1}", param = regexreplace(c.Value, "(?<domain>[^\\]+)\\(?<user>.+)", "${user}"), param = c.Value);  
```  
  
### <a name="example-how-to-query-an-active-directory-attribute-store-based-on-the-value-of-an-incoming-claim"></a>Esempio: Come eseguire una query di un archivio di attributi di Active Directory in base al valore di un'attestazione in ingresso  
  
```  
c:[Type == "http://test/name"]  
  
   => issue(store = "Enterprise AD Attribute Store",  
  
         types = ("http://test/email"),  
  
         query = ";mail;{0}",  
  
         param = c.Value)  
  
```  
  
La query precedente è costituita da tre parti seguenti:  
  
-   Il filtro LDAP, si specifica questa parte della query per recuperare gli oggetti per cui si desidera richiedere gli attributi. Per informazioni generali sulle query LDAP valide, vedere RFC 2254. Quando si esegue una query di un archivio di attributi di Active Directory e non si specifica un filtro LDAP, il samAccountName\ = {0} presuppone query e l'archivio di attributi di Active Directory prevede un parametro che può alimentare il valore per {0}. In caso contrario, la query genererà un errore. Per un archivio di attributi LDAP diverso da Active Directory, è possibile omettere la parte del filtro LDAP della query o la query genererà un errore.  
  
-   Specifica degli attributi: In questa seconda parte della query, si specificano gli attributi \ (che sono delimitati da virgole se si utilizzano più attributi values\) che desidera che gli oggetti filtrati. Il numero di attributi specificato deve corrispondere al numero di tipi di attestazione definiti nella query.  
  
-   Dominio di Active Directory, specificare l'ultima parte della query solo quando l'archivio di attributi è Active Directory. \ (Non è necessario quando si esegue una query altri archivi di attributi. \) questa parte della query viene utilizzata per specificare l'account utente in domain\\name il modulo. L'archivio di attributi di Active Directory utilizza la parte del dominio per determinare il controller di dominio appropriato per connettersi a e di eseguire la query e richiedere gli attributi.  
  
### <a name="example-how-to-use-two-custom-rules-to-extract-the-manager-e-mail-from-an-attribute-in-active-directory"></a>Esempio: Come usare due regole personalizzate per estrarre il manager e\ di posta elettronica da un attributo in Active Directory  
Le seguenti due regole personalizzate, quando usate insieme nell'ordine illustrato di seguito, eseguire una query Active Directory per il **manager** attributo dell'utente \(Rule 1\) dell'account e quindi usare tale attributo per l'account utente del manager per eseguire una query di **posta** \(Rule 2\) dell'attributo. Infine, il **posta** attributo viene rilasciato come attestazione "ManagerEmail". In breve, regola 1 interroga Active Directory e passa il risultato della query alla regola 2, che estrae quindi il servizio di gestione valori e\ di posta elettronica.  
  
Ad esempio, quando termina l'esecuzione di queste regole, viene rilasciata un'attestazione contenente l'indirizzo di posta elettronica e\ del gestore per un utente nel dominio corp.fabrikam.com.  
  
**Regola 1**  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]  
=> add(store = "Active Directory", types = ("http://schemas.xmlsoap.org/claims/ManagerDistinguishedName"), query = "sAMAccountName=  
{0};mail,userPrincipalName,extensionAttribute5,manager,department,extensionAttribute2,cn;{1}", param = regexreplace(c.Value, "(?  
<domain>[^\\]+)\\(?<user>.+)", "${user}"), param = c.Value);  
```  
  
**Regola 2**  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]  
&& c1:[Type == "http://schemas.xmlsoap.org/claims/ManagerDistinguishedName"]  
=> issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/claims/ManagerEmail"), query = "distinguishedName={0};mail;{1}", param = c1.Value,   
param = regexreplace(c1.Value, ".*DC=(?<domain>.+),DC=corp,DC=fabrikam,DC=com", "${domain}\username"));  
```  
  
> [!NOTE]  
> Queste regole funzionano solo se il manager dell'utente è nello stesso dominio dell'utente \ (corp.fabrikam.com in questo example\).  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
[Creare una regola per inviare attributi LDAP come attestazioni](https://technet.microsoft.com/library/dd807115.aspx)  
  

