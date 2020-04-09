---
ms.assetid: af16e847-47c2-461e-9df1-cc352a322043
title: Quando usare una regola di invio dell'appartenenza a un gruppo come attestazione
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 266f46ef30082541d49bf62d933c551f00fa08da
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853794"
---
# <a name="when-to-use-a-send-group-membership-as-a-claim-rule"></a>Quando usare una regola di invio dell'appartenenza a un gruppo come attestazione
È possibile utilizzare questa regola in Active Directory Federation Services \(AD FS\) quando si desidera emettere un nuovo valore attestazione in uscita solo per gli utenti che sono membri di un gruppo di sicurezza Active Directory specificato. Quando si usa questa regola, si emette una singola attestazione solo per il gruppo specificato e che corrisponde alla logica della regola, come descritto nella tabella seguente.  
  
|Opzione della regola|Logica della regola|  
|---------------|--------------|  
|Valore attestazione in uscita|Se l'appartenenza a un gruppo di un utente è uguale al *gruppo specificato* e il tipo di attestazione in uscita è uguale al *tipo di attestazione*specificato, sostituire il valore del nome del gruppo esistente con il *valore attestazione in uscita specificato* ed emettere l'attestazione.|  
  
Le sezioni seguenti forniscono un'introduzione di base alle regole attestazioni, Forniscono anche informazioni dettagliate su quando usare una regola di invio dell'appartenenza a un gruppo come attestazione.  
  
## <a name="about-claim-rules"></a>Informazioni sulle regole attestazioni  
Una regola attestazione rappresenta un'istanza di logica di business che recupera un'attestazione in ingresso, applicare una condizione \(Se x y quindi\) e produrre un'attestazione in uscita in base ai parametri di condizione. L'elenco seguente fornisce suggerimenti importanti sulle regole attestazioni da tenere presenti prima di procedere con la lettura di questo argomento:  
  
-   Nello snap di gestione di ADFS\-attestazione regole possono essere create solo utilizzando modelli di regola attestazione  
  
-   Processo di regole attestazione in ingresso di attestazioni direttamente da un provider di attestazioni \(ad esempio Active Directory o un altro servizio federativo\) o dall'output dell'accettazione di trasformare le regole in un trust di provider di attestazioni.  
  
-   Le regole attestazioni sono elaborate dal motore di rilascio delle attestazioni in ordine cronologico entro un determinato set di regole. Impostando la precedenza sulle regole, è possibile perfezionare o filtrare ulteriormente le attestazioni generate dalle regole precedenti all'interno di un set di regole specificato.  
  
-   Per i modelli di regola attestazioni è sempre necessario specificare un tipo di attestazione in ingresso. Tuttavia, è possibile elaborare più valori di attestazione con lo stesso tipo di attestazione usando una singola regola.  
  
Per ulteriori informazioni sulle regole attestazione e i set di regole attestazione, vedere [ruolo delle attestazioni regole](The-Role-of-Claim-Rules.md). Per ulteriori informazioni sulle modalità di elaborazione delle regole, vedere [il ruolo del motore di attestazioni](The-Role-of-the-Claims-Engine.md). Per ulteriori informazioni, modalità di elaborazione dei set di regole attestazione, vedere [ruolo delle Pipeline delle attestazioni](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="outgoing-claim-value"></a>Valore attestazione in uscita  
Usando il modello di regola di invio dell'appartenenza a un gruppo come attestazione, è possibile emettere un'attestazione che dipende dal fatto che un utente sia membro di un gruppo specificato.  
  
In altre parole, questo modello di regola rilascia un'attestazione solo se l'utente dispone dell'ID di sicurezza del gruppo \(SID\) corrispondente al gruppo di Active Directory specificato dall'amministratore. Tutti gli utenti che eseguono l'autenticazione in Active Directory Domain Services \(AD DS\) avranno attestazioni SID di gruppo in ingresso per ogni gruppo a cui appartengono. Per impostazione predefinita, le regole di trasformazione accettazione nel trust del provider di attestazioni Active Directory attraversano queste attestazioni SID di gruppo. L'uso di questi SID di gruppo come base per l'emissione delle attestazioni è molto più veloce rispetto alla ricerca dei gruppi dell'utente in servizi di dominio Active Directory.  
  
Quando si usa questa regola, viene inviata solo una singola attestazione, in base al gruppo di Active Directory selezionato. Ad esempio, è possibile usare questo modello di regola per creare una regola che invierà un'attestazione di gruppo con un valore "Admin" se l'utente è membro del gruppo di sicurezza Domain Admins.  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>Configurazione della regola in un trust del provider di attestazioni  
Gli amministratori devono usare questo tipo di regola nelle regole di trasformazione accettazione di un trust del provider di attestazioni solo quando vengono ricevuti SID di gruppo dal provider di attestazioni, scenario molto insolito per tutti i provider di attestazioni ad eccezione di Active Directory o Servizi di dominio Active Directory.  
  
## <a name="how-to-create-this-rule"></a>Come creare la regola  
È possibile creare questa regola usando il linguaggio delle regole attestazioni o usando il modello di regola di invio dell'appartenenza a un gruppo LDAP come attestazione nello snap-in di gestione AD FS\-in. Questo modello di regola offre le opzioni di configurazione seguenti:  
  
-   Specificare un nome della regola attestazione  
  
-   Selezionare il gruppo di un utente usando il selettore oggetti  
  
-   Selezionare un tipo di attestazione in uscita  
  
-   Selezionare un formato ID nome in uscita \(disponibile solo quando si sceglie ID nome dal campo tipo attestazione in uscita\)  
  
-   Specificare un valore attestazione in uscita  
  
Per altre informazioni su come creare questa regola, vedere [creare una regola per inviare l'appartenenza a un gruppo come attestazione](https://technet.microsoft.com/library/ee913569.aspx).  
  
## <a name="using-the-claim-rule-language"></a>Uso del linguaggio delle regole attestazioni  
Se si vogliono emettere attestazioni basate su un SID in ingresso diverso da un SID di gruppo, usare il modello di regola di trasformazione di un'attestazione in ingresso. Se l'amministratore vuole recuperare i nomi per tutti i gruppi di cui l'utente è membro, usare invece il modello di regola di invio di attributi LDAP come attestazioni con l'attributo **tokenGroups**.  
  
### <a name="example-how-to-issue-group-claims-based-on-the-users-group-membership"></a>Esempio: come emettere attestazioni di gruppo in base all'appartenenza a gruppi dell'utente  
La regola seguente emette attestazioni di gruppo per un utente in base a un SID di gruppo in ingresso:  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-21-397933417-626991126-188441444-512", Issuer == "AD AUTHORITY"]  
=> issue(Type = "http://schemas.xmlsoap.org/claims/Group", Value = "administrators", Issuer = c.Issuer, OriginalIssuer = c.OriginalIssuer, ValueType = c.ValueType);  
```  
  
## <a name="additional-references"></a>Altri riferimenti  
[Creare una regola per inviare attributi LDAP come attestazioni](https://technet.microsoft.com/library/dd807115.aspx)  
  

