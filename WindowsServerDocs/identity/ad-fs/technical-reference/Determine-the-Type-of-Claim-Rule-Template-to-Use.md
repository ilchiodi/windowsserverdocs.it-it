---
ms.assetid: 696a29b2-d627-4c9a-a384-9c8aaf50bd23
title: Determinare il tipo di modello di regola attestazioni da usare
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 8cbe858a9bd11710378f533f843aa06099036b21
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860234"
---
# <a name="determine-the-type-of-claim-rule-template-to-use"></a>Determinare il tipo di modello di regola attestazioni da usare


Una parte importante della progettazione di un Active Directory Federation Services \(AD FS infrastruttura\) consiste nel determinare il set completo di regole attestazioni, e quali modelli di regole attestazioni corrispondenti è necessario usare per crearli, per ogni partner che parteciperà alla Federazione con l'organizzazione. È possibile creare regole usando i modelli di regola attestazioni nello snap-in di gestione AD FS\-in.  
  
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
Nella tabella seguente vengono descritti tutti i tipi di modelli di regola attestazioni che è possibile utilizzare per creare regole mediante lo snap-in di gestione AD FS\-in e i vantaggi derivanti dall'utilizzo di un tipo di modello rispetto a un altro.  
  
|Tipo di modello regola|Descrizione|Vantaggi|Svantaggi|  
|----------------------|---------------|--------------|-----------------|  
|Pass Through or Filter an Incoming Claim|Consente di creare una regola per il pass-through di tutti i valori attestazione per un tipo di attestazione selezionato oppure per filtrare le attestazioni in base ai relativi valori, in modo che venga eseguito il pass-through solo di alcuni valori per un tipo di attestazione selezionato.<p>Per altre informazioni, vedere [When to Use a Pass Through or Filter Claim Rule](When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md).|-Può essere usato per selezionare attestazioni specifiche da accettare o emettere senza modifiche|-Impossibile modificare il tipo di attestazione e il valore|  
|Trasformare un'attestazione in ingresso|Consente di creare una regola che permette di selezionare un'attestazione in ingresso e mapparla a un diverso valore attestazione oppure mapparne il valore attestazione a un nuovo valore attestazione.<p>Per altre informazioni, vedere [When to Use a Transform Claim Rule](When-to-Use-a-Transform-Claim-Rule.md).|-Può essere usato per normalizzare i tipi di attestazione o i valori<br />-Può sostituire un suffisso di posta elettronica\-di un'attestazione in ingresso|-Le sostituzioni di stringa più complesse richiedono una regola personalizzata|  
|Inviare attributi LDAP come attestazioni|Consente di creare una regola per la selezione degli attributi da un archivio attributi LDAP da inviare come attestazioni alla relying party.<p>Per altre informazioni, vedere [When to Use a Send LDAP Attributes as Claims Rule](When-to-Use-a-Send-LDAP-Attributes-as-Claims-Rule.md).|-È possibile originare attestazioni da qualsiasi\/di servizi di dominio Active Directory AD LDS archivio attributi<br />-È possibile emettere più attestazioni usando una singola regola|-Prestazioni-lento in seguito alla ricerca dell'account<br />-Non è possibile usare un filtro LDAP personalizzato per le query|  
|Invia appartenenza a gruppo come attestazione|Consente di creare una regola che può inviare un tipo e un valore di attestazione specificati quando un utente è membro di un gruppo di sicurezza di Active Directory. Usando questa regola verrà inviata una singola richiesta in base al gruppo selezionato.<p>Per altre informazioni, vedere [When to Use a Send Group Membership as a Claim Rule](When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md).|-Prestazioni veloci per il rilascio di attestazioni di gruppo-nessuna ricerca account|-L'utente deve essere membro di un gruppo di Active Directory locale|  
|Send Claims Using a Custom Rule|Consente di creare una regola personalizzata che fornirà opzioni più avanzate rispetto a un modello di regola standard. È possibile scrivere regole personalizzate con il linguaggio delle regole attestazioni AD FS.<p>Per altre informazioni, vedere [When to Use a Custom Claim Rule](When-to-Use-a-Custom-Claim-Rule.md).|-Può essere usato per l'origine di attestazioni da un archivio attributi SQL<br />-Può essere usato per specificare un filtro LDAP personalizzato<br />-Può essere usato per emettere un PPID<br />-Può essere usato con un archivio attributi personalizzato<br />-Può essere usato per aggiungere attestazioni solo al set di attestazioni di input<br />-Può essere usato per inviare attestazioni in base a più di un'attestazione in ingresso|-È più difficile configurare \- potrebbe essere necessario un tempo di preparazione per acquisire familiarità con il linguaggio delle regole attestazioni|  
|Consentire o negare l'accesso agli utenti in base a un'attestazione in ingresso|Consente di creare una regola per consentire o negare l'accesso da parte degli utenti alla relying party, in base al tipo e al valore di un'attestazione in ingresso.<p>Per altre informazioni, vedere [When to Use an Authorization Claim Rule](When-to-Use-an-Authorization-Claim-Rule.md).|-Semplifica il processo di autorizzazione|-Richiede la specifica di un solo tipo di attestazione e un valore di attestazione<br />-Non supporta i criteri di ricerca per i valori attestazione|  
|Consentire l'accesso a tutti gli utenti|Consente di creare una regola per permettere a tutti gli utenti di accedere alla relying party.<p>Per altre informazioni, vedere [When to Use an Authorization Claim Rule](When-to-Use-an-Authorization-Claim-Rule.md).|-Semplice da configurare|-Meno sicuro rispetto all'uso del modello Consenti o nega utenti in base a un'attestazione in ingresso|  
  

