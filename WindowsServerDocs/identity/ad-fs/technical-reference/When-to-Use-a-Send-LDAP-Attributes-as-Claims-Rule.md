---
ms.assetid: 606f4196-b579-4806-a462-3abd4d93e87c
title: Quando usare una regola Inviare attributi LDAP come attestazioni
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 08382a8eccaafac257165587fe7b6c18a8ecfbbc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407275"
---
# <a name="when-to-use-a-send-ldap-attributes-as-claims-rule"></a>Quando usare una regola Inviare attributi LDAP come attestazioni
È possibile utilizzare questa regola in Active Directory Federation Services \(ad FS\) quando si desidera emettere attestazioni in uscita che contengono valori effettivi dell'attributo \(LDAP\) (Lightweight Directory Access Protocol) esistenti in un archivio di attributi e quindi associare un tipo di attestazione a ognuno degli attributi LDAP. Per ulteriori informazioni sugli archivi di attributi, vedere [il ruolo degli archivi di attributi](The-Role-of-Attribute-Stores.md).  
  
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
Quando si usa il modello di regola Inviare attributi LDAP come attestazioni, è possibile selezionare gli attributi da un archivio attributi LDAP, ad esempio \(Active Directory o\) Active Directory Domain Services Servizi di dominio Active Directory per inviare i relativi valori come attestazioni al relying party . In tal modo si esegue praticamente il mapping di attributi LDAP specifici da un archivio di attributi definito dall'utente a un set di attestazioni in uscita che può essere usato per l'autorizzazione.  
  
Usando questo modello, è possibile aggiungere più attributi, che verranno inviati come più attestazioni, da una singola regola. Ad esempio, è possibile usare questo modello di regola per creare una regola che eseguirà la ricerca di valori attributo per gli utenti autenticati dagli attributi **company** e **department** di Active Directory e quindi invierà tali valori come due diverse attestazioni in uscita.  
  
È anche possibile usare questa regola per inviare tutte le appartenenze a gruppi dell'utente. Se si vuole inviare solo le singole appartenenze ai gruppi, usare il modello di regola Invio dell'appartenenza a un gruppo come attestazione. Per altre informazioni, vedere [quando usare un gruppo di invio dell'appartenenza a una regola di attestazione](When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md).  
  
## <a name="how-to-create-this-rule"></a>Come creare la regola  
È possibile creare questa regola usando il linguaggio delle regole attestazioni o usando il modello di regola Inviare attributi LDAP come attestazioni nello snap\--in gestione ad FS. Questo modello di regola offre le opzioni di configurazione seguenti:  
  
-   Specificare un nome della regola attestazione  
  
-   Selezionare un archivio di attributi da cui estrarre gli attributi LDAP  
  
-   Mapping degli attributi LDP ai tipi di attestazioni in uscita  
  
Per altre informazioni su come creare questa regola, vedere [creare una regola per inviare attributi LDAP come attestazioni](https://technet.microsoft.com/library/dd807115.aspx).  
  
## <a name="using-the-claim-rule-language"></a>Uso del linguaggio delle regole attestazioni  
Se la query per Active Directory, servizi di dominio Active Directory \(o\) Active Directory Lightweight Directory Services ad LDS deve essere confrontata con un attributo LDAP diverso da **sAMAccountName**, è necessario usare invece una regola personalizzata. Se non è presente alcuna attestazione Nome account Windows nel set di input, è necessario usare anche una regola personalizzata per specificare l'attestazione da usare per l'esecuzione di query di Servizi di dominio Active Directory o AD LDS.  
  
Gli esempi seguenti consentono di comprendere alcuni dei modi in cui è possibile creare una regola personalizzata usando il linguaggio delle regole attestazioni per interrogare ed estrarre i dati in un archivio attributi.  
  
### <a name="example-how-to-query-an-adlds-attribute-store-and-return-a-specified-value"></a>Esempio: Come eseguire le query in un archivio attributi AD LDS e restituire un valore specificato  
I parametri devono essere separati da punto e virgola. Il primo parametro è il filtro LDAP. I parametri successivi sono gli attributi da restituire in tutti gli oggetti corrispondenti.  
  
Nell'esempio seguente viene illustrato come cercare un utente dall'attributo **sAMAccountName** ed emettere un'\-attestazione di indirizzo di posta elettronica, usando il valore dell'attributo mail dell'utente:  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]  
=> issue(store = "AD LDS", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"), query = "sAMAccountName={0};mail", param = regexreplace(c.Value, "(?<domain>[^\\]+)\\(?<user>.+)", "${user}"));  
  
```  
  
Nell'esempio seguente viene illustrato come cercare un utente tramite l'attributo **mail** e rilasciare le attestazioni title e display name, usando i valori degli attributi **title** e **DisplayName** dell'utente:  
  
```  
c:[Type == " http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress ", Issuer == "AD AUTHORITY"]  
=> issue(store = "AD LDS ", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/title","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/displayname"), query = "mail={0};title;displayname", param = c.Value);  
```  
  
L'esempio seguente mostra come cercare un utente per posta elettronica e titolo e quindi rilasciare un'attestazione del nome visualizzato usando l'attributo **DisplayName** dell'utente:  
  
```  
c1:[Type == " http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"] && c2:[Type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/title"]  
=> issue(store = "AD LDS ", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/displayname"), query = "(&(mail={0})(title={1}));displayname", param = c1.Value, param = c2.Value);  
```  
  
### <a name="example-how-to-query-an-activedirectory-attribute-store-and-return-a-specified-value"></a>Esempio: Come eseguire le query in un archivio attributi Active Directory e restituire un valore specificato  
La query Active Directory deve includere il nome \(dell'utente con il nome\) di dominio come parametro finale in modo che l'archivio attributi Active Directory possa eseguire query nel dominio corretto. In caso contrario, sarà supportata la stessa sintassi.  
  
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
  
-   Il filtro LDAP: specifica questa parte della query per recuperare gli oggetti per cui si vogliono richiedere gli attributi. Per informazioni generali sulle query LDAP valide, vedere RFC 2254. Quando si esegue una query su un archivio attributi Active Directory e non si specifica un filtro LDAP, viene presupposta la query sAMAccountName\= {0} e l'archivio di attributi Active Directory prevede un parametro che può inserire il valore per {0}. In caso contrario, la query genererà un errore. Per un archivio attributi LDAP diverso da Active Directory, non è possibile omettere la parte del filtro LDAP della query, altrimenti la query genererà un errore.  
  
-   Specifica degli attributi: in questa seconda parte della query, si specificano gli \(attributi separati da\-virgole se si usano più valori\) di attributo che si desidera escludere dagli oggetti filtrati. Il numero di attributi specificato deve corrispondere al numero di tipi di attestazione definiti nella query.  
  
-   Dominio di Active Directory: specificare l'ultima parte della query solo quando l'archivio attributi è Active Directory \(Non è necessario quando si esegue una query in altri archivi di attributi.\) Questa parte della query viene usata per specificare l'account utente nel formato nome dominio\\. L'archivio attributi di Active Directory usa la parte del dominio per determinare il controller di dominio appropriato per connettersi ed eseguire la query e richiedere gli attributi.  
  
### <a name="example-how-to-use-two-custom-rules-to-extract-the-manager-e-mail-from-an-attribute-in-activedirectory"></a>Esempio: Come usare due regole personalizzate per estrarre il gestore e\-la posta da un attributo in Active Directory  
Le due regole personalizzate seguenti, quando usate insieme nell'ordine indicato di seguito, eseguono una query Active Directory per l'attributo **Manager** della regola \(dell'account\) utente 1 e quindi usano tale attributo per eseguire una query sull'account utente del gestore per regola dell'attributo \(di posta\)2. Infine, l'attributo **mail** viene rilasciato come attestazione “ManagerEmail”. In sintesi, la regola 1 esegue una query Active Directory e passa il risultato della query alla regola 2, che estrae quindi i\-valori di posta elettronica del Manager.  
  
Ad esempio, al termine dell'esecuzione di queste regole, viene rilasciata un'attestazione contenente\-l'indirizzo di posta elettronica del Manager per un utente nel dominio Corp.fabrikam.com.  
  
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
> Queste regole funzionano solo se il manager dell'utente si trova nello stesso dominio dell'utente \(Corp.fabrikam.com in questo esempio.\)  
  
## <a name="additional-references"></a>Altri riferimenti  
[Creare una regola per inviare attributi LDAP come attestazioni](https://technet.microsoft.com/library/dd807115.aspx)  
  

