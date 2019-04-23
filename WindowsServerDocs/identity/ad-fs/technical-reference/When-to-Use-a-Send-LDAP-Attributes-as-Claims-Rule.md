---
ms.assetid: 606f4196-b579-4806-a462-3abd4d93e87c
title: Quando usare una regola Inviare attributi LDAP come attestazioni
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 05a2dd88057b64675bbc3bd30724d1eda0880c44
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860662"
---
>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="when-to-use-a-send-ldap-attributes-as-claims-rule"></a>Quando usare una regola Inviare attributi LDAP come attestazioni
È possibile usare questa regola in Active Directory Federation Services \(ADFS\) quando si desidera rilasciare attestazioni in uscita contenenti effettivo Lightweight Directory Access Protocol \(LDAP\) valori esistenti nell'attributo un attributo di archiviazione e quindi associare un tipo di attestazione con ciascuno degli attributi LDAP. Per altre informazioni sugli archivi di attributi, vedere [il ruolo degli archivi attributi](The-Role-of-Attribute-Stores.md).  
  
Quando si usa questa regola, si rilascia un'attestazione per ogni attributo LDAP specificato e che corrisponde alla logica della regola, come descritto nella tabella seguente.  
  
|Opzione della regola|Logica della regola|  
|---------------|--------------|  
|Mapping degli attributi LDP ai tipi di attestazioni in uscita|Se l'archivio di attributi equivale all'*archivio attributi specificato* e l'attributo LDAP equivale al *valore specificato*, eseguire il mapping del valore dell'attributo LDAP al tipo di *attestazione in uscita specificato* e rilasciare l'attestazione.|  
  
Le sezioni seguenti forniscono un'introduzione di base alle regole attestazioni, oltre a informazioni dettagliate su quando usare la regola Inviare attributi LDAP come attestazioni.  
  
## <a name="about-claim-rules"></a>Informazioni sulle regole attestazioni  
Una regola attestazione rappresenta un'istanza di logica di business che recupera un'attestazione in ingresso, applicare una condizione \(Se x y quindi\) e produrre un'attestazione in uscita in base ai parametri di condizione. L'elenco seguente fornisce suggerimenti importanti sulle regole attestazioni da tenere presenti prima di procedere con la lettura di questo argomento:  
  
-   Nello snap di gestione di ADFS\-attestazione regole possono essere create solo utilizzando modelli di regola attestazione  
  
-   Processo di regole attestazione in ingresso di attestazioni direttamente da un provider di attestazioni \(ad esempio Active Directory o un altro servizio federativo\) o dall'output dell'accettazione di trasformare le regole in un trust di provider di attestazioni.  
  
-   Le regole attestazioni sono elaborate dal motore di rilascio delle attestazioni in ordine cronologico entro un determinato set di regole. Impostando la precedenza sulle regole, è possibile perfezionare o filtrare ulteriormente le attestazioni generate dalle regole precedenti all'interno di un set di regole specificato.  
  
-   Per i modelli di regola attestazioni è sempre necessario specificare un tipo di attestazione in ingresso. Tuttavia, è possibile elaborare più valori di attestazione con lo stesso tipo di attestazione usando una singola regola.  
  
Per ulteriori informazioni sulle regole attestazione e i set di regole attestazione, vedere [ruolo delle attestazioni regole](The-Role-of-Claim-Rules.md). Per ulteriori informazioni sulle modalità di elaborazione delle regole, vedere [il ruolo del motore di attestazioni](The-Role-of-the-Claims-Engine.md). Per ulteriori informazioni, modalità di elaborazione dei set di regole attestazione, vedere [ruolo delle Pipeline delle attestazioni](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="mapping-of-ldap-attributes-to-outgoing-claim-types"></a>Mapping degli attributi LDP ai tipi di attestazioni in uscita  
Quando si usa l'invio di attributi LDAP come modello di regola attestazioni, è possibile selezionare gli attributi da un archivio attributi LDAP, ad esempio Active Directory o Active Directory Domain Services \(Active Directory Domain Services\) per inviare i relativi valori come attestazioni alla relying party . In tal modo si esegue praticamente il mapping di attributi LDAP specifici da un archivio di attributi definito dall'utente a un set di attestazioni in uscita che può essere usato per l'autorizzazione.  
  
Usando questo modello, è possibile aggiungere più attributi, che verranno inviati come più attestazioni, da una singola regola. Ad esempio, è possibile usare questo modello di regola per creare una regola che eseguirà la ricerca di valori attributo per gli utenti autenticati dagli attributi **company** e **department** di Active Directory e quindi invierà tali valori come due diverse attestazioni in uscita.  
  
È anche possibile usare questa regola per inviare tutte le appartenenze a gruppi dell'utente. Se si vuole inviare solo le singole appartenenze ai gruppi, usare il modello di regola Invio dell'appartenenza a un gruppo come attestazione. Per altre informazioni, vedere [When to Use a Send Group Membership as a Claim Rule](When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md).  
  
## <a name="how-to-create-this-rule"></a>Come creare la regola  
È possibile creare questa regola utilizzando il linguaggio di regola attestazione o tramite l'invio di attributi LDAP come modello di regola attestazioni in Gestione AD FS blocca\-in. Questo modello di regola offre le opzioni di configurazione seguenti:  
  
-   Specificare un nome della regola attestazione  
  
-   Selezionare un archivio di attributi da cui estrarre gli attributi LDAP  
  
-   Mapping degli attributi LDP ai tipi di attestazioni in uscita  
  
Per altre informazioni su come creare questa regola, vedere [creare una regola per inviare attributi LDAP come attestazioni](https://technet.microsoft.com/library/dd807115.aspx).  
  
## <a name="using-the-claim-rule-language"></a>Uso del linguaggio delle regole attestazioni  
Se la query ad Active Directory, Active Directory Domain Services o Active Directory Lightweight Directory Services \(AD LDS\) deve confrontare rispetto a un attributo LDAP diverso da **samAccountname**, è invece necessario utilizzare una regola personalizzata. Se non è presente alcuna attestazione Nome account Windows nel set di input, è necessario usare anche una regola personalizzata per specificare l'attestazione da usare per l'esecuzione di query di Servizi di dominio Active Directory o AD LDS.  
  
Gli esempi seguenti consentono di comprendere alcuni dei modi in cui è possibile creare una regola personalizzata usando il linguaggio delle regole attestazioni per interrogare ed estrarre i dati in un archivio attributi.  
  
### <a name="example-how-to-query-an-adlds-attribute-store-and-return-a-specified-value"></a>Esempio: Come eseguire le query in un archivio attributi AD LDS e restituire un valore specificato  
I parametri devono essere separati da punto e virgola. Il primo parametro è il filtro LDAP. I parametri successivi sono gli attributi da restituire in tutti gli oggetti corrispondenti.  
  
Nell'esempio seguente viene illustrato come cercare un utente con il **sAMAccountName** attributo ed emettere una e\-attestazione indirizzo di posta elettronica, utilizzando il valore dell'attributo mail dell'utente:  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]  
=> issue(store = "AD LDS", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"), query = "sAMAccountName={0};mail", param = regexreplace(c.Value, "(?<domain>[^\\]+)\\(?<user>.+)", "${user}"));  
  
```  
  
L'esempio seguente mostra come cercare un utente con l'attributo **mail** e le attestazioni issue Title e Display Name usando il valore degli attributi **title** e **displayname** dell'utente:  
  
```  
c:[Type == " http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress ", Issuer == "AD AUTHORITY"]  
=> issue(store = "AD LDS ", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/title","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/displayname"), query = "mail={0};title;displayname", param = c.Value);  
```  
  
L'esempio seguente mostra come cercare un utente con in base a indirizzo e titolo e quindi rilasciare un'attestazione Display Name usando l'attributo **displayname** dell'utente:  
  
```  
c1:[Type == " http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"] && c2:[Type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/title"]  
=> issue(store = "AD LDS ", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/displayname"), query = "(&(mail={0})(title={1}));displayname", param = c1.Value, param = c2.Value);  
```  
  
### <a name="example-how-to-query-an-activedirectory-attribute-store-and-return-a-specified-value"></a>Esempio: Come eseguire le query in un archivio attributi Active Directory e restituire un valore specificato  
La query di Active Directory deve includere il nome dell'utente \(con il nome di dominio\) come il parametro finale in modo da archiviare l'attributo di Active Directory possa eseguire una query il dominio corretto. In caso contrario, sarà supportata la stessa sintassi.  
  
L'esempio seguente mostra come cercare un utente con l'attributo **sAMAccountName** nel relativo dominio e quindi restituire l'attributo **mail**:  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]  
=> issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"), query = "sAMAccountName={0};mail;{1}", param = regexreplace(c.Value, "(?<domain>[^\\]+)\\(?<user>.+)", "${user}"), param = c.Value);  
```  
  
### <a name="example-how-to-query-an-activedirectory-attribute-store-based-on-the-value-of-an-incoming-claim"></a>Esempio: Come eseguire una query di un archivio attributi di Active Directory in base al valore di un'attestazione in ingresso  
  
```  
c:[Type == "http://test/name"]  
  
   => issue(store = "Enterprise AD Attribute Store",  
  
         types = ("http://test/email"),  
  
         query = ";mail;{0}",  
  
         param = c.Value)  
  
```  
  
La query precedente è costituita dalle parti seguenti:  
  
-   Il filtro LDAP: specifica questa parte della query per recuperare gli oggetti per cui si vogliono richiedere gli attributi. Per informazioni generali sulle query LDAP valide, vedere RFC 2254. Quando si esegue una query di un archivio attributi Active Directory e non si specifica un filtro LDAP, l'attributo samAccountName\= {0} presuppone query e l'archivio di attributi di Active Directory prevede un parametro che può alimentare il valore per {0}. In caso contrario, la query genererà un errore. Per un archivio attributi LDAP diverso da Active Directory, non è possibile omettere la parte del filtro LDAP della query, altrimenti la query genererà un errore.  
  
-   Specifica degli attributi, In questa seconda parte della query, specificare gli attributi \(quali sono virgole\-separate, se si usano più valori di attributo\) che si desidera includere gli oggetti filtrati. Il numero di attributi specificato deve corrispondere al numero di tipi di attestazione definiti nella query.  
  
-   Dominio di Active Directory: specificare l'ultima parte della query solo quando l'archivio attributi è Active Directory \(Non è necessario quando si esegue una query altri archivi di attributi.\) Questa parte della query viene usata per specificare l'account utente nel formato dominio\\nome. L'archivio attributi di Active Directory usa la parte del dominio per determinare il controller di dominio appropriato per connettersi ed eseguire la query e richiedere gli attributi.  
  
### <a name="example-how-to-use-two-custom-rules-to-extract-the-manager-e-mail-from-an-attribute-in-activedirectory"></a>Esempio: Come usare due regole personalizzate per estrarre la gestione e\-posta elettronica da un attributo in Active Directory  
Le due regole personalizzate seguenti, se utilizzati insieme nell'ordine riportato di seguito, eseguire una query Active Directory per il **gestore** attributo dell'account utente \(regola 1\) e quindi usare tale attributo per chiedere all'utente account del manager per il **mail** attributo \(regola 2\). Infine, l'attributo **mail** viene rilasciato come attestazione “ManagerEmail”. In sintesi, regola 1 interroga Active Directory e passa il risultato della query alla regola 2, che estrae quindi la gestione e\-i valori di posta elettronica.  
  
Ad esempio, quando queste regole di completamento dell'esecuzione, viene rilasciata un'attestazione che contiene e del gestore della\-indirizzo per un utente nel dominio corp.fabrikam.com di posta elettronica.  
  
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
> Queste regole funzionano solo se il responsabile dell'utente è nello stesso dominio dell'utente \(corp.fabrikam.com in questo esempio\).  
  
## <a name="additional-references"></a>Altri riferimenti  
[Creare una regola per inviare attributi LDAP come attestazioni](https://technet.microsoft.com/library/dd807115.aspx)  
  

