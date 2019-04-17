---
ms.assetid: af16e847-47c2-461e-9df1-cc352a322043
title: Quando utilizzare l'appartenenza a un gruppo di trasmissione come regola attestazione
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 10e027b4de463aad2b05eae40a622aebf8f12a3b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="when-to-use-a-send-group-membership-as-a-claim-rule"></a>Quando utilizzare l'appartenenza a un gruppo di trasmissione come regola attestazione
È possibile utilizzare questa regola in Active Directory Federation Services \(AD FS\) quando si vuole emettere un nuovo valore attestazione in uscita per solo gli utenti che sono membri di un gruppo di sicurezza di Active Directory specificato. Quando si usa questa regola, si emette una singola attestazione solo per il gruppo specificato e che corrisponde alla logica della regola, come descritto nella tabella seguente.  
  
|Opzione della regola|Logica della regola|  
|---------------|--------------|  
|Valore attestazione in uscita|Se l'appartenenza al gruppo dell'utente è uguale al *gruppo specificato* e tipo di attestazione in uscita è uguale a *tipo di attestazione specificato*, quindi sostituire il valore di nome di gruppo esistente con il *in uscita specificato il valore dell'attestazione* ed emettere l'attestazione.|  
  
Le sezioni seguenti forniscono un'introduzione di base per le regole di attestazione. Oltre a informazioni dettagliate su quando usare l'appartenenza al gruppo di invio come una regola attestazione.  
  
## <a name="about-claim-rules"></a>Informazioni sulle regole attestazioni  
Una regola attestazioni rappresenta un'istanza della logica di business che recupera un'attestazione in ingresso, applica una condizione \ (se x quindi y\) e genera un'attestazione in uscita in base ai parametri di condizione. L'elenco seguente fornisce importanti suggerimenti che è necessario conoscere sulle regole attestazione prima di procedere con la lettura di questo argomento:  
  
-   Nella gestione di ADFS snap-in, attestazione regole possono essere create solo utilizzando modelli di regola attestazione  
  
-   Processo di regole attestazione in ingresso di attestazioni direttamente da un provider di attestazioni \ (ad esempio Active Directory o un altro Service \ federativo) oppure dall'output dell'accettazione regole in un trust di provider di attestazioni di trasformazione.  
  
-   Le regole attestazioni vengono elaborate dal motore di rilascio delle attestazioni in ordine cronologico entro un determinato set di regole. Impostando la precedenza sulle regole, è possibile perfezionare ulteriormente o filtrare le attestazioni generate dalle regole precedenti all'interno di un determinato set di regole.  
  
-   Modelli di regola attestazione verranno sempre necessario specificare un tipo di attestazione in ingresso. Tuttavia, è possibile elaborare più valori di attestazione con lo stesso tipo di attestazione usando una singola regola.  
  
Per ulteriori informazioni sulle regole attestazione e set di regole attestazione, vedere [ruolo delle attestazioni regole](The-Role-of-Claim-Rules.md). Per ulteriori informazioni su come vengono elaborate le regole, vedere [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md). Per ulteriori informazioni, modalità di elaborazione dei set di regole attestazione, vedere [ruolo delle Pipeline delle attestazioni](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="outgoing-claim-value"></a>Valore attestazione in uscita  
Utilizza l'appartenenza al gruppo di invio come un modello di regola attestazione, è possibile emettere un'attestazione che dipende dal fatto che un utente è membro di un gruppo specificato.  
  
In altre parole, questo problemi nel modello regola un'attestazione solo quando l'utente ha la sicurezza gruppo ID \(SID\) che corrisponde al gruppo di Active Directory specificato dall'amministratore. Tutti gli utenti autenticati in servizi di dominio Active Directory \(AD DS\) avranno attestazioni per ogni gruppo a cui appartengono SID di gruppo. Per impostazione predefinita, le regole di trasformazione accettazione in Active Directory attestazioni Provider attendibilità attraversano queste attestazioni SID di gruppo. Utilizzo di questi è molto più velocemente rispetto alla ricerca dei gruppi dell'utente di dominio Active Directory come base per il rilascio di attestazioni SID di gruppo.  
  
Quando si usa questa regola, solo una singola attestazione viene inviato, in base al gruppo di Active Directory selezionato. Ad esempio, è possibile utilizzare questo modello di regola per creare una regola che invierà un'attestazione di gruppo con un valore "Admin" se l'utente è membro del gruppo di sicurezza Domain Admins.  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>Configurazione della regola in un trust del provider di attestazioni  
Gli amministratori devono utilizzare questo tipo di regola nelle regole di trasformazione accettazione di un trust di provider di attestazioni solo quando vengono ricevuti SID di gruppo dal provider di attestazioni, scenario molto insolito per tutti i provider di attestazioni, ad eccezione di Active Directory o di dominio Active Directory.  
  
## <a name="how-to-create-this-rule"></a>Come creare questa regola  
Puoi creare questa regola utilizzando il linguaggio di regola attestazione o tramite l'appartenenza al gruppo di invio LDAP come attestazione regola modello di gestione di AD FS snap-in. Questo modello di regola offre le opzioni di configurazione seguenti:  
  
-   Specificare un nome di regola attestazione  
  
-   Selezionare il gruppo un utente tramite il selettore oggetti  
  
-   Selezionare un tipo di attestazione in uscita  
  
-   Selezionare un formato ID nome in uscita \ (che è disponibile solo quando viene scelto ID nome dal field\ tipo di attestazione in uscita)  
  
-   Specificare un valore attestazione in uscita  
  
Per ulteriori informazioni su come creare questa regola, vedere [creare una regola per inviare l'appartenenza al gruppo come attestazione](https://technet.microsoft.com/en-us/library/ee913569.aspx).  
  
## <a name="using-the-claim-rule-language"></a>Utilizzando il linguaggio di regola attestazione  
Se si desidera rilasciare le attestazioni in base a un SID in ingresso diverso da un SID di gruppo, utilizzare la trasformazione di un modello di regola attestazione in ingresso. Se l'amministratore vuole recuperare i nomi di tutti i gruppi che l'utente è membro di, utilizzare inviare attributi LDAP come modello di regola attestazioni invece con le **tokenGroups** attributo.  
  
### <a name="example-how-to-issue-group-claims-based-on-the-users-group-membership"></a>Esempio: Come emettere attestazioni di gruppo in base all'appartenenza a gruppi dell'utente  
La regola seguente emette attestazioni di gruppo per un utente in base a un SID di gruppo in ingresso:  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-21-397933417-626991126-188441444-512", Issuer == "AD AUTHORITY"]  
=> issue(Type = "http://schemas.xmlsoap.org/claims/Group", Value = "administrators", Issuer = c.Issuer, OriginalIssuer = c.OriginalIssuer, ValueType = c.ValueType);  
```  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
[Creare una regola per inviare attributi LDAP come attestazioni](https://technet.microsoft.com/library/dd807115.aspx)  
  

