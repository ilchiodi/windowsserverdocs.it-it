---
ms.assetid: af16e847-47c2-461e-9df1-cc352a322043
title: Quando usare una regola di invio dell'appartenenza a un gruppo come attestazione
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: dffd886ffd0bedd429918f72408b2d13d9fa1bdf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859172"
---
>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="when-to-use-a-send-group-membership-as-a-claim-rule"></a>Quando usare una regola di invio dell'appartenenza a un gruppo come attestazione
È possibile usare questa regola in Active Directory Federation Services \(ADFS\) quando si vuole emettere un nuovo valore attestazione in uscita per solo gli utenti che sono membri di un gruppo di sicurezza di Active Directory specificato. Quando si usa questa regola, si emette una singola attestazione solo per il gruppo specificato e che corrisponde alla logica della regola, come descritto nella tabella seguente.  
  
|Opzione della regola|Logica della regola|  
|---------------|--------------|  
|Valore attestazione in uscita|Se l'appartenenza al gruppo di un utente corrisponde al *gruppo specificato* e il tipo di attestazione in uscita corrisponde al *tipo di attestazione specificato*, allora sostituire il valore del nome del gruppo esistente con il *valore attestazione in uscita specificato* ed emettere l'attestazione.|  
  
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
  
In altre parole, questo modello di regola emette un'attestazione solo quando l'utente ha l'ID del gruppo di sicurezza \(SID\) che corrisponde al gruppo Active Directory specificato dall'amministratore. Tutti gli utenti autenticati in servizi di dominio Active Directory \(Active Directory Domain Services\) avranno attestazioni SID per ogni gruppo cui appartiene di gruppo. Per impostazione predefinita, le regole di trasformazione accettazione nel trust del provider di attestazioni Active Directory attraversano queste attestazioni SID di gruppo. L'uso di questi SID di gruppo come base per l'emissione delle attestazioni è molto più veloce rispetto alla ricerca dei gruppi dell'utente in Servizi di dominio Active Directory.  
  
Quando si usa questa regola, viene inviata solo una singola attestazione, in base al gruppo di Active Directory selezionato. Ad esempio, è possibile usare questo modello di regola per creare una regola che invierà un'attestazione di gruppo con un valore "Admin" se l'utente è membro del gruppo di sicurezza Domain Admins.  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>Configurazione della regola in un trust del provider di attestazioni  
Gli amministratori devono usare questo tipo di regola nelle regole di trasformazione accettazione di un trust del provider di attestazioni solo quando vengono ricevuti SID di gruppo dal provider di attestazioni, scenario molto insolito per tutti i provider di attestazioni ad eccezione di Active Directory o Servizi di dominio Active Directory.  
  
## <a name="how-to-create-this-rule"></a>Come creare la regola  
Creare questa regola utilizzando il linguaggio di regola attestazione o tramite l'appartenenza al gruppo LDAP di invio come un modello di regola attestazione in Gestione AD FS snap\-in. Questo modello di regola offre le opzioni di configurazione seguenti:  
  
-   Specificare un nome della regola attestazione  
  
-   Selezionare un gruppo di un utente tramite il selettore oggetti  
  
-   Selezionare un tipo di attestazione in uscita  
  
-   Selezionare un formato ID nome in uscita \(che è disponibile solo quando viene scelto ID nome del campo di tipo attestazione in uscita\)  
  
-   Specificare un valore attestazione in uscita  
  
Per altre informazioni su come creare questa regola, vedere [creare una regola per inviare l'appartenenza al gruppo come attestazione](https://technet.microsoft.com/library/ee913569.aspx).  
  
## <a name="using-the-claim-rule-language"></a>Uso del linguaggio delle regole attestazioni  
Se si vogliono emettere attestazioni basate su un SID in ingresso diverso da un SID di gruppo, usare il modello di regola di trasformazione di un'attestazione in ingresso. Se l'amministratore vuole recuperare i nomi per tutti i gruppi di cui l'utente è membro, usare invece il modello di regola di invio di attributi LDAP come attestazioni con l'attributo **tokenGroups**.  
  
### <a name="example-how-to-issue-group-claims-based-on-the-users-group-membership"></a>Esempio: Come emettere attestazioni di gruppo basate sull'appartenenza dell'utente a un gruppo  
La regola seguente emette attestazioni di gruppo per un utente in base a un SID di gruppo in ingresso:  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-21-397933417-626991126-188441444-512", Issuer == "AD AUTHORITY"]  
=> issue(Type = "http://schemas.xmlsoap.org/claims/Group", Value = "administrators", Issuer = c.Issuer, OriginalIssuer = c.OriginalIssuer, ValueType = c.ValueType);  
```  
  
## <a name="additional-references"></a>Altri riferimenti  
[Creare una regola per inviare attributi LDAP come attestazioni](https://technet.microsoft.com/library/dd807115.aspx)  
  

