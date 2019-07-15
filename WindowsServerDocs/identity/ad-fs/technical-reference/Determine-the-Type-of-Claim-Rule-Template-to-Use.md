---
ms.assetid: 696a29b2-d627-4c9a-a384-9c8aaf50bd23
title: Determinare il tipo di modello di regola attestazioni da usare
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f1d38e23d7f1671f729b03b7e6f8000471d2e9f9
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188654"
---
# <a name="determine-the-type-of-claim-rule-template-to-use"></a>Determinare il tipo di modello di regola attestazioni da usare


Una parte importante della progettazione di un Active Directory Federation Services \(ADFS\) infrastruttura consiste nel determinare il set completo di regole attestazioni, e quali corrispondente di modelli di regole deve usare per creare tali attestazioni, ovvero per ogni partner che prenderà la federazione con l'organizzazione. Creare regole usando i modelli di regola attestazione nello snap di gestione di AD FS\-in.  
  
Ogni set di regole attestazioni che si configura può solo essere associato a una relazione di trust federativa. Ciò significa che non è possibile creare un set di regole in una relazione di trust e usarle per altre relazioni di trust nel Servizio federativo. In alternativa è possibile creare facilmente regole dai modelli di regole attestazione per aiutare a produrre più rapidamente un set di attestazioni concordate tra ogni partner federato e la propria organizzazione.  
  
Per altre informazioni sulle regole e sui modelli di regola, vedere [The Role of Claim Rules](The-Role-of-Claim-Rules.md).  
  
Prima di iniziare a determinare i tipi di modelli di regola attestazioni che è consigliabile usare, considerare le domande seguenti:  
  
-   Quali attestazioni verranno fornite dai provider di attestazioni attendibili?  
  
-   Quali attestazioni di ogni provider di attestazioni si considerano attendibili?  
  
-   Quali attestazioni sono richieste dalla relying party che considera attendibile questo Servizio federativo?  
  
-   Quali attestazioni si è disposti a divulgare a ogni relying party?  
  
-   Quali utenti devono avere accesso a ogni relying party?  
  
Rispondendo a queste domande, sarà possibile pianificare una progettazione di regole attestazioni affidabile. Sarà anche possibile creare una semplice strategia di controllo delle autorizzazioni e degli accessi e aumentare l'efficienza del team di distribuzione durante l'implementazione.  
  
La sezione successiva contiene informazioni sul tipo di modello di regola da selezionare per il proprio ambiente in base alle esigenze aziendali.  
  
## <a name="claim-rule-template-types"></a>Tipi di modello di regola attestazioni  
La tabella seguente descrive tutti i tipi di modelli di regole attestazioni che è possibile usare per creare regole con lo snap di gestione di AD FS\-in e i vantaggi dell'uso di un tipo di modello rispetto a un altro.  
  
|Tipo di modello regola|Descrizione|Vantaggi|Svantaggi|  
|----------------------|---------------|--------------|-----------------|  
|Pass-through o filtro di un'attestazione in ingresso|Consente di creare una regola per il pass-through di tutti i valori attestazione per un tipo di attestazione selezionato oppure per filtrare le attestazioni in base ai relativi valori, in modo che venga eseguito il pass-through solo di alcuni valori per un tipo di attestazione selezionato.<br /><br />Per altre informazioni, vedere [quando utilizzare un Pass Through or Filter Claim Rule](When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md).|-Può essere usato per selezionare particolari attestazioni da accettare o emettere invariato|-Attestazione di tipo e il valore non può essere modificati.|  
|Trasformare un'attestazione in ingresso|Consente di creare una regola che permette di selezionare un'attestazione in ingresso e mapparla a un diverso valore attestazione oppure mapparne il valore attestazione a un nuovo valore attestazione.<br /><br />Per altre informazioni, vedere [quando usare una regola di attestazione di trasformazione](When-to-Use-a-Transform-Claim-Rule.md).|-Può essere utilizzato per normalizzare i tipi di attestazione o valori<br />-È possibile sostituire una e\-suffisso di posta elettronica di un'attestazione in ingresso|-Sostituzioni di stringa più complesse richiedono una regola personalizzata|  
|Inviare attributi LDAP come attestazioni|Consente di creare una regola per la selezione degli attributi da un archivio attributi LDAP da inviare come attestazioni alla relying party.<br /><br />Per altre informazioni, vedere [quando utilizzare un inviare attributi LDAP come attestazioni regola](When-to-Use-a-Send-LDAP-Attributes-as-Claims-Rule.md).|-È possibile ottenere le attestazioni da qualsiasi dominio di Active Directory\/archivio attributi AD LDS<br />-Più attestazioni possono essere eseguite utilizzando una singola regola|-Prestazioni: lente in conseguenza alla ricerca dell'account<br />-Non è possibile usare un filtro LDAP personalizzato per l'esecuzione di query|  
|Inviare l'appartenenza a un gruppo come attestazione|Consente di creare una regola che può inviare un tipo e un valore di attestazione specificati quando un utente è membro di un gruppo di sicurezza di Active Directory. Usando questa regola verrà inviata una singola richiesta in base al gruppo selezionato.<br /><br />Per altre informazioni, vedere [quando usare un gruppo di invio dell'appartenenza a una regola di attestazione](When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md).|-Prestazioni elevate per il rilascio di attestazioni di gruppo, nessuna ricerca dell'account|-L'utente deve essere un membro di un gruppo di Active Directory locale|  
|Inviare attestazioni mediante una regola personalizzata|Consente di creare una regola personalizzata che fornirà opzioni più avanzate rispetto a un modello di regola standard. Scrivere regole personalizzate con AD FS linguaggio delle regole attestazioni.<br /><br />Per altre informazioni, vedere [quando usare una regola attestazioni personalizzata](When-to-Use-a-Custom-Claim-Rule.md).|-Può essere utilizzato per ottenere le attestazioni da un archivio attributi SQL<br />-Può essere utilizzato per specificare un filtro LDAP personalizzato<br />-Può essere utilizzato per emettere un PPID<br />-Può essere utilizzato con un archivio attributi personalizzato<br />-Può essere utilizzato per aggiungere attestazioni solo al set di attestazioni di input<br />-Può essere utilizzato per inviare attestazioni in base a più di un'attestazione in ingresso|-Più difficile da configurare \- può essere necessario un periodo di addestramento per acquisire una conoscenza del linguaggio di regola attestazione iniziale|  
|Consentire o negare l'accesso agli utenti in base a un'attestazione in ingresso|Consente di creare una regola per consentire o negare l'accesso da parte degli utenti alla relying party, in base al tipo e al valore di un'attestazione in ingresso.<br /><br />Per altre informazioni, vedere [quando usare an Authorization Claim Rule](When-to-Use-an-Authorization-Claim-Rule.md).|-Semplifica il processo di autorizzazione|-Richiede che solo un'attestazione di tipo e l'altro valore di attestazione specificare<br />-Non supporta criteri di ricerca per i valori di attestazione|  
|Consentire l'accesso a tutti gli utenti|Consente di creare una regola per permettere a tutti gli utenti di accedere alla relying party.<br /><br />Per altre informazioni, vedere [quando usare an Authorization Claim Rule](When-to-Use-an-Authorization-Claim-Rule.md).|-Semplice da configurare|-Meno sicura rispetto all'uso di Permit or Deny Users Based in un modello di attestazione in ingresso|  
  

