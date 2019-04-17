---
ms.assetid: 22f53391-8c6a-4873-a1f4-08b4760ea621
title: Ruolo delle attestazioni
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 98765deaba67ffdc0ee18b6d8ef573e531d739cc
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="the-role-of-claims"></a>Ruolo delle attestazioni
Nel modello di identità basate su claims\ attestazioni svolgono un ruolo fondamentale nel processo di federazione, ma sono il componente chiave che sono determina il risultato di tutti i richieste di autenticazione e autorizzazione basata sul Web. Questo modello consente alle organizzazioni di proiettare in modo sicuro identità digitali, diritti o *attestazioni*attraverso i limiti dell'organizzazione in modo standardizzato e della sicurezza.  
  
## <a name="what-are-claims"></a>Cosa sono le attestazioni?  
Nella sua forma più semplice, le attestazioni sono semplicemente *istruzioni* \ (ad esempio nome, identità, Group \), formulate riguardo a utenti, utilizzati principalmente per autorizzare l'accesso alle applicazioni basate su claims\ si trova in un punto qualsiasi nella rete Internet. Ogni istruzione corrisponde a un *valore* archiviato nell'attestazione.  
  
### <a name="how-claims-are-sourced"></a>Come vengono ottenute le attestazioni  
Il servizio federativo in Active Directory Federation Services \(AD FS\) definisce quali attestazioni vengono scambiate tra partner federati. Tuttavia, prima di eseguire questa operazione deve prima popolare o l'attestazione con un valore recuperato o un valore calcolato di origine. Ogni valore attestazione rappresenta un valore di un utente, gruppo o entità e sia originato in uno dei due modi:  
  
1.  Quando il valore che costituisce l'attestazione viene recuperato da un archivio di attributi, ad esempio, quando un valore di attributo di Sales Department recuperato dalle proprietà di un account utente di Active Directory. Per ulteriori informazioni, vedere [il ruolo di archivi di attributi](The-Role-of-Attribute-Stores.md).  
  
2.  Quando il valore di un'attestazione in ingresso viene trasformato in un altro valore basato sulla logica espressa in una regola. Ad esempio, quando un'attestazione in ingresso con il valore del gruppo Domain Admins viene trasformata in un nuovo valore di Administrators prima che venga inviato come un'attestazione in uscita. Per ulteriori informazioni, vedere [ruolo delle attestazioni regole](The-Role-of-Claim-Rules.md).  
  
Le attestazioni possono includere valori come un indirizzo di posta elettronica e\, \(UPN\) nome entità utente, l'appartenenza al gruppo e altri attributi dell'account.  
  
### <a name="how-claims-flow"></a>Flusso delle attestazioni  
Altre parti si affidano ai valori delle attestazioni per eseguire attività di autorizzazione per le applicazioni basata sul Web che ospitano. Tali parti vengono chiamati *relying party* in snap-in di gestione di ADFS. Il servizio federativo è responsabile di coordinare il trust tra più parti diverse. È progettato per elaborare e convogliare lo scambio attendibile delle attestazioni tra un'organizzazione che inizialmente recupera le attestazioni, detta anche *provider di attestazioni* nella gestione di AD FS snap-in a una relying party. Una relying party Usa quindi queste attestazioni per prendere decisioni di autorizzazione.  
  
Il flusso di attestazioni usando questo processo è noto come il *pipeline delle attestazioni*. Esistono tre passaggi nel flusso di attestazioni attraverso la pipeline delle attestazioni:  
  
1.  Le attestazioni che vengono ricevute dal provider di attestazioni vengono elaborate le regole di trasformazione accettazione nel trust del provider di attestazioni. Queste regole determinano quali attestazioni vengono accettate dal provider di attestazioni.  
  
2.  L'output delle regole di trasformazione accettazione viene usato come input per le regole di autorizzazione di rilascio. Queste regole determinano se l'utente è autorizzato ad accedere alla relying party.  
  
3.  L'output delle regole di trasformazione accettazione viene usato come input per le regole di trasformazione rilascio. Queste regole determinano quali attestazioni verranno inviate alla relying party.  
  
Per ulteriori informazioni, vedere [ruolo delle Pipeline delle attestazioni](The-Role-of-the-Claims-Pipeline.md)  
  
### <a name="how-claims-are-issued"></a>Come vengono emesse le attestazioni  
Quando si scrivono regole attestazione, l'origine delle attestazioni in ingresso per le regole attestazione varia in base se stai scrivendo le regole in un trust del provider di attestazioni o trust della relying party. Quando si scrivono regole attestazione per un trust di provider di attestazioni, le attestazioni in ingresso sono quelle inviate dal provider di attestazioni attendibile al servizio federativo. Quando si scrivono regole per un trust della relying party, le attestazioni in ingresso sono le attestazioni provenienti dalle regole attestazione del trust del provider di attestazioni applicabile. Per ulteriori informazioni sulle attestazioni in ingresso e in uscita, vedere [ruolo delle Pipeline delle attestazioni](The-Role-of-the-Claims-Pipeline.md) e [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md).  
  
## <a name="what-are-claim-types"></a>Quali sono i tipi di attestazioni?  
Un tipo di attestazione fornisce contesto per il valore dell'attestazione. Viene in genere espresso come un \(URI\) Uniform Resource Identifier. ADFS può supportare qualsiasi tipo di attestazione e viene configurato con i tipi di attestazione nella tabella seguente per impostazione predefinita.  
  
|Nome|Descrizione|URI|  
|--------|---------------|-------|  
|Indirizzo di posta elettronica E\|L'indirizzo di posta elettronica e\ dell'utente|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/EmailAddress|  
|Nome specificato|Il nome dell'utente|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/givenName|  
|Nome|Il nome univoco dell'utente|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/Name|  
|UPN|L'entità utente nome \(UPN\) dell'utente|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/UPN|  
|Nome comune|Il nome comune dell'utente|http:///\/schemas.xmlsoap.org\/claims\/commonName|  
|Indirizzo di posta con AD FS 1. x E\|L'indirizzo di posta elettronica e\ dell'utente quando interagisce con AD FS 1.1 o ad FS 1.0|http:///\/schemas.xmlsoap.org\/claims\/EmailAddress|  
|Gruppo|Un gruppo che l'utente è membro di|http:///\/schemas.xmlsoap.org\/claims\/Group|  
|AD FS 1. x UPN|L'UPN dell'utente quando interagisce con AD FS 1.1 o ad FS 1.0|http:///\/schemas.xmlsoap.org\/claims\/UPN|  
|Ruolo|Un ruolo dell'utente|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/Role|  
|Cognome|Cognome dell'utente|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/surname|  
|PPID|Identificatore privato dell'utente|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/privatepersonalidentifier|  
|Identificatore nome|Identificatore nome SAML dell'utente|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/NameIdentifier|  
|Metodo di autenticazione|Il metodo utilizzato per autenticare l'utente|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/AuthenticationMethod|  
|Nega solo i SID di gruppo|Il solo deny\ SID di gruppo dell'utente|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/denyonlysid|  
|SID primario di sola negazione|SID primario solo deny\ dell'utente|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/denyonlyprimarysid|  
|Nega solo SID gruppo primario di|Il solo deny\ SID gruppo primario dell'utente|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/denyonlyprimarygroupsid|  
|SID di gruppo|Il SID di gruppo dell'utente|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/groupsid|  
|SID gruppo primario di|SID gruppo primario dell'utente|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/primarygroupsid|  
|SID primario di|SID primario dell'utente|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/primarysid|  
|Nome account di Windows|Il nome di account di dominio dell'utente in forma di<domain>\\<user>|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/windowsaccountname|  
  
## <a name="what-are-claim-descriptions"></a>Quali sono le descrizioni di attestazioni?  
Descrizioni di attestazioni rappresentano un elenco dei tipi di attestazioni ADFS supporta e che possono essere pubblicati nei metadati federativi. I tipi di attestazioni menzionati nella tabella precedente sono configurati come descrizioni di attestazioni in snap-in di gestione di ADFS.  
  
La raccolta di descrizioni di attestazioni che verrà pubblicata nei metadati federativi è archiviata nel database di configurazione di ADFS. Queste attestazioni le descrizioni sono usate da vari componenti del servizio federativo.  
  
Ogni descrizione di attestazione include un tipo di attestazione URI, nome, stato di pubblicazione e descrizione. È possibile gestire la raccolta di descrizione di attestazione utilizzando il **descrizioni attestazione** nodo snap-in di gestione di ADFS. È possibile modificare lo stato di pubblicazione di una descrizione di attestazione utilizzando lo snap-in. Sono disponibili le impostazioni seguenti:  
  
-   **Pubblica l'attestazione nei metadati federativi come tipo di attestazione in grado di accettare questo servizio federativo** \(Publish as Accepted\)-indica i tipi di attestazione che verranno accettati da altri provider di attestazioni da questo servizio federativo.  
  
-   **Pubblica l'attestazione nei metadati federativi come tipo di attestazione che questo servizio federativo può inviare** \(Publish as Sent\)-indica i tipi di attestazioni offerte dal servizio federativo. Questi sono i tipi di attestazioni che nel servizio federativo pubblica agli altri come disponibili per l'invio. I tipi di attestazioni effettivi inviati dal provider di attestazioni sono spesso un sottoinsieme di questo elenco.  
  
Per ulteriori informazioni su come impostare lo stato di pubblicazione di un tipo di attestazione, vedere [aggiungere una descrizione di attestazione](https://technet.microsoft.com/library/dd807051.aspx) nella Guida alla distribuzione di ADFS.  
  
### <a name="when-generating-federation-metadata"></a>Durante la generazione di metadati di federazione  
I metadati della federazione includono tutte le descrizioni di attestazioni contrassegnate per la pubblicazione.  
  
### <a name="when-claims-rules-are-processed"></a>Quando vengono elaborate le regole attestazioni  
Se si mantengono informazioni di configurazione relative alle descrizioni di attestazioni, è più semplice configurare regole relative alle attestazioni. Per ulteriori informazioni sulle regole attestazione che può essere utilizzato nell'organizzazione del provider di attestazioni, vedere [ruolo delle attestazioni regole](The-Role-of-Claim-Rules.md).  
  

