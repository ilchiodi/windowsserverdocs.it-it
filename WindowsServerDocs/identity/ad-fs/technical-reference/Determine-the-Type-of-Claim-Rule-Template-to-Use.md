---
ms.assetid: 696a29b2-d627-4c9a-a384-9c8aaf50bd23
title: Determinare il tipo di modello di regola attestazione da utilizzare
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 129cd83be4cd8302bd170ba87aad58c50f636006
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="determine-the-type-of-claim-rule-template-to-use"></a>Determinare il tipo di modello di regola attestazione da utilizzare

>Si applica a: Windows Server 2016, Windows Server 2012 R2

Una parte importante della progettazione di un'infrastruttura di Active Directory Federation Services \(AD FS\) consiste nel determinare il set completo di regole attestazioni, e quali corrispondente è necessario utilizzare la creazione di modelli di regole di attestazione, per ogni partner che parteciperà alla federazione con l'organizzazione. Creare regole utilizzando modelli di regola attestazione in snap-in di gestione di ADFS.  
  
Ogni set di regole attestazioni che si configura può solo essere associato a una relazione di trust federativa. Ciò significa che è Impossibile creare un set di regole in una relazione di trust e usarle per altre relazioni di trust nel servizio federativo. Invece è possibile creare facilmente regole di attestazione i modelli di regola più rapidamente per produrre un set di attestazioni concordate tra ogni partner federato e la propria organizzazione.  
  
Per ulteriori informazioni sulle regole e i modelli di regola, vedere [ruolo delle attestazioni regole](The-Role-of-Claim-Rules.md).  
  
Prima di iniziare a determinare i tipi di modelli di regola attestazione, che è consigliabile usare, considerare le domande seguenti:  
  
-   Quali attestazioni verranno fornite dai provider di attestazioni attendibili?  
  
-   Quali attestazioni si considerano attendibili da ogni provider di attestazioni?  
  
-   Quali attestazioni sono richieste dalla relying party che considera attendibile questo servizio federativo?  
  
-   Quali attestazioni si è disposti a divulgare a ogni relying party?  
  
-   Quali utenti devono avere accesso a ogni relying party?  
  
Rispondendo a queste domande, sarà possibile pianificare un solido progettazione di regole attestazioni. Verrà inoltre facilitano la creazione di un'autorizzazione uniforme e strategia di controllo di accesso e rendere più efficiente il team di distribuzione durante l'implementazione.  
  
Nella sezione successiva che informazioni sul tipo di modelli di regola da selezionare per l'ambiente in base l'azienda deve.  
  
## <a name="claim-rule-template-types"></a>Tipi di modello di regola attestazione  
Nella tabella seguente vengono descritti tutti i tipi di modelli di regole attestazioni che è possibile utilizzare per creare regole con la gestione di ADFS snap-in e i vantaggi dell'uso di un modello digitare rispetto a un altro.  
  
|Tipo di modello regola|Descrizione|Vantaggi|Svantaggi|  
|----------------------|---------------|--------------|-----------------|  
|Pass-Through o filtrare un'attestazione in ingresso|Consente di creare una regola di pass-through di tutti i valori di attestazione per un tipo di attestazione selezionato o filtrare le attestazioni in base ai valori di attestazione in modo che consentirà l'ingresso solo determinati valori di attestazione per un tipo di attestazione selezionato.<br /><br />Per ulteriori informazioni, vedere [quando utilizzare un Pass-Through or Filter Claim Rule](When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md).|-Può essere usato per selezionare particolari attestazioni da accettare o emettere senza modifiche|-Attestazione tipo e il valore non può essere modificati.|  
|Trasformare un'attestazione in ingresso|Consente di creare una regola che è possibile selezionare un'attestazione in ingresso e mapparla a un tipo di attestazione diversi o mapparne il valore attestazione per un nuovo valore attestazione.<br /><br />Per ulteriori informazioni, vedere [When to Use a Transform Claim Rule](When-to-Use-a-Transform-Claim-Rule.md).|-Può essere usato per normalizzare i valori o i tipi di attestazione<br />-Può sostituire un suffisso di posta elettronica e\ di un'attestazione in ingresso|-Le sostituzioni di stringa più complesse richiedono una regola personalizzata|  
|Inviare attributi LDAP come attestazioni|Consente di creare una regola di selezione degli attributi da un archivio di attributi LDAP da inviare come attestazioni alla relying party.<br /><br />Per ulteriori informazioni, vedere [quando utilizzare un inviare attributi LDAP come attestazioni regola](When-to-Use-a-Send-LDAP-Attributes-as-Claims-Rule.md).|-Può ottenere le attestazioni da qualsiasi archivio di attributi AD DS\/AD LDS<br />-Possono essere emessi più attestazioni usando una singola regola|-Prestazioni: lente in conseguenza alla ricerca dell'account<br />-Non usare un filtro LDAP personalizzato per l'esecuzione di query|  
|Inviare l'appartenenza al gruppo come attestazione|Consente di creare una regola che è possibile inviare un tipo di attestazione specificato e un valore quando un utente è membro di un gruppo di sicurezza di Active Directory. Verrà inviata solo una singola attestazione tramite questa regola, in base al gruppo selezionato.<br /><br />Per ulteriori informazioni, vedere [When to Use a Send Group Membership come una regola attestazione](When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md).|-Veloce delle prestazioni per il rilascio di attestazioni di gruppo: nessuna ricerca dell'account|-L'utente deve essere un membro di un gruppo locale di Active Directory|  
|Inviare attestazioni mediante una regola personalizzata|Consente di creare una regola personalizzata che fornirà opzioni più avanzate rispetto a un modello di regola standard. Scrivere regole personalizzate con AD FS linguaggio delle regole attestazioni.<br /><br />Per ulteriori informazioni, vedere [When to Use a Custom Claim Rule](When-to-Use-a-Custom-Claim-Rule.md).|-Può essere usato per le attestazioni da un archivio di attributi SQL<br />-Può essere utilizzato per specificare un filtro LDAP personalizzato<br />-Può essere utilizzato per emettere un PPID<br />-Può essere usato con un archivio attributi personalizzato<br />-Consente di aggiungere attestazioni solo al set di attestazioni di input<br />-Può essere utilizzato per inviare attestazioni in base a uno o più attestazioni in ingresso|-Più difficili da configurare \-potrebbe essere necessarie alcune salita tempo per inizialmente acquisire una conoscenza di linguaggio di regola attestazione|  
|Consentire o negare agli utenti in base a un'attestazione in ingresso|Consente di creare una regola per consentire o negare l'accesso da parte degli utenti alla relying party, in base al tipo e il valore di un'attestazione in ingresso.<br /><br />Per ulteriori informazioni, vedere [When to Use an Authorization Claim Rule](When-to-Use-an-Authorization-Claim-Rule.md).|-Semplifica il processo di autorizzazione|-Necessità di tipo solo una attestazione e un valore attestazione essere specificato<br />-Non supporta criteri di ricerca per i valori attestazione|  
|Consentire tutti gli utenti|Consente di creare una regola che consentirà tutti gli utenti di accedere alla relying party.<br /><br />Per ulteriori informazioni, vedere [When to Use an Authorization Claim Rule](When-to-Use-an-Authorization-Claim-Rule.md).|-Semplice da configurare|-Meno sicura rispetto all'uso di consentire o negare agli utenti in base su un modello di attestazione in ingresso|  
  

