---
ms.assetid: 22f53391-8c6a-4873-a1f4-08b4760ea621
title: Ruolo delle attestazioni
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 542d7a24e29b52dd3fa0d7ea6a9b2d27fb620d8d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188498"
---
# <a name="the-role-of-claims"></a>Ruolo delle attestazioni
Nelle attestazioni\-modello di identità basato su attestazioni svolgono un ruolo fondamentale nel processo di federazione, ma sono il componente principale per cui il risultato di tutte le applicazioni Web\-vengono determinate in base le richieste di autenticazione e autorizzazione. Questo modello consente alle organizzazioni di proiettare in modo sicuro identità digitali, diritti o *attestazioni* attraverso limiti di sicurezza aziendali in modo standardizzato.  
  
## <a name="what-are-claims"></a>Cosa sono le attestazioni?  
Nella sua forma più semplice, le attestazioni sono semplicemente *istruzioni* \(ad esempio, nome, identità, gruppo\), eseguita sugli utenti, che vengono utilizzati principalmente per autorizzare l'accesso alle attestazioni\-applicazioni basate su che si trovano ovunque su Internet. Ogni istruzione corrisponde a un *valore* archiviato nell'attestazione.  
  
### <a name="how-claims-are-sourced"></a>Come vengono ottenute le attestazioni  
Il servizio federativo in Active Directory Federation Services \(ADFS\) definisce quali attestazioni vengono scambiate tra partner federati. Prima che ciò sia possibile, tuttavia, il servizio deve prima popolare od ottenere l'attestazione tramite un valore recuperato o calcolato. Ogni valore dell'attestazione rappresenta un valore di un utente, gruppo o entità e può essere recuperato in uno di due modi:  
  
1.  Quando il valore che costituisce l'attestazione viene recuperato da un archivio di attributi, ad esempio il valore dell'attributo Sales Department recuperato dalle proprietà di un account utente Active Directory. Per altre informazioni, vedere [Ruolo degli archivi attributi](The-Role-of-Attribute-Stores.md).  
  
2.  Quando il valore di un'attestazione in ingresso viene trasformata in un altro valore basato sulla logica espressa in una regola. Quando ad esempio un'attestazione in ingresso con il valore di Domain Admins viene trasformata in un nuovo valore di Administrators prima di essere inviata come attestazione in uscita. Per altre informazioni, vedere [Ruolo delle regole attestazione](The-Role-of-Claim-Rules.md).  
  
Le attestazioni possono includere valori come una e\-indirizzo, nome dell'entità utente di posta elettronica \(UPN\), appartenenza al gruppo e altri attributi dell'account.  
  
### <a name="how-claims-flow"></a>Flusso delle attestazioni  
Altre parti si affidano ai valori delle attestazioni per eseguire attività di autorizzazione per il Web\-basato su applicazioni che ospitano. Queste parti sono dette *relying party* nello snap di gestione di AD FS\-in. Il servizio federativo è responsabile di coordinare il trust tra più parti diverse. È progettato per elaborare e convogliare lo scambio attendibile delle attestazioni da un'organizzazione che inizialmente le attestazioni, anche detta *provider di attestazioni* nello snap di gestione di AD FS\-in, per una relying party. Una relying party usa quindi queste attestazioni per operare decisioni in merito alle autorizzazioni.  
  
Il flusso di attestazioni che usa questo processo è noto come *pipeline delle attestazioni*. Vi sono tre passaggi nel flusso di attestazioni attraverso la pipeline delle attestazioni:  
  
1.  Le attestazioni ricevute dal provider di attestazioni vengono elaborate dalle regole di trasformazione accettazione nel trust del provider di attestazioni. Queste regole determinano quali attestazioni vengono accettate dal provider di attestazioni.  
  
2.  L'output delle regole di trasformazione accettazione viene usato come input per le regole di autorizzazione rilascio. Tali regole determinano se l'utente è autorizzato ad accedere alla relying party.  
  
3.  L'output delle regole di trasformazione accettazione viene usato come input per le regole di trasformazione rilascio. Tali regole determinano quali attestazioni verranno inviate alla relying party.  
  
Per altre informazioni, vedere [Ruolo delle pipeline delle attestazioni](The-Role-of-the-Claims-Pipeline.md)  
  
### <a name="how-claims-are-issued"></a>Come vengono emesse le attestazioni  
Quando si scrivono regole attestazione, l'origine delle attestazioni in ingresso per le regole attestazione varia in base alla scrittura o meno di regole nel trust del provider di attestazioni o nel trust della relying party. Quando si scrivono regole attestazione per un trust del provider di attestazioni, le attestazioni in ingresso sono quelle inviate dal provider di attestazioni attendibile al servizio federativo. Quando si scrivono regole per un trust della relying party, le attestazioni in ingresso sono quelle provenienti dalle regole attestazione del trust del provider di attestazioni applicabile. Per altre informazioni sulle attestazioni in ingresso e in uscita, vedere [Ruolo delle pipeline delle attestazioni](The-Role-of-the-Claims-Pipeline.md) e [Ruolo del motore delle attestazioni](The-Role-of-the-Claims-Engine.md).  
  
## <a name="what-are-claim-types"></a>Cosa sono i tipi di attestazioni?  
Un tipo di attestazione fornisce contesto per il valore dell'attestazione. Viene in genere espresso come un Uniform Resource Identifier \(URI\). ADFS può supportare qualsiasi tipo di attestazione e viene configurato con i tipi di attestazione nella tabella seguente per impostazione predefinita.  
  
|Nome|Descrizione|URI|  
|--------|---------------|-------|  
|E\-indirizzo di posta elettronica|L'oggetto\-indirizzo dell'utente di posta elettronica|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/emailaddress|  
|Nome (di battesimo)|Nome (di battesimo) dell'utente|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/givenname|  
|Nome|Nome univoco dell'utente|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/name|  
|UPN|Il nome dell'entità utente \(UPN\) dell'utente|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/upn|  
|Nome comune|Nome comune dell'utente|http:\/\/schemas.xmlsoap.org\/claims\/CommonName|  
|AD FS 1.x E\-indirizzo di posta elettronica|L'oggetto\-posta indirizzo dell'utente quando interagisce con AD FS 1.1 o ad FS 1.0|http:\/\/schemas.xmlsoap.org\/claims\/EmailAddress|  
|Raggruppa|Gruppo a cui appartiene l'utente|http:\/\/schemas.xmlsoap.org\/claims\/Group|  
|UPN AD FS 1.x|UPN dell'utente quando interagisce con AD FS 1.1 o AD FS 1.0|http:\/\/schemas.xmlsoap.org\/claims\/UPN|  
|Ruolo|Ruolo dell'utente|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/role|  
|Surname|Cognome dell'utente|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/surname|  
|PPID|Identificatore privato dell'utente|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/privatepersonalidentifier|  
|Identificatore nome|Identificatore nome SAML dell'utente|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/nameidentifier|  
|Metodo di autenticazione|Metodo usato per autenticare l'utente|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/authenticationmethod|  
|SID gruppo di sola negazione|L'istruzione deny\-SID dell'utente solo del gruppo|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/denyonlysid|  
|SID primario di sola negazione|L'istruzione deny\-solo SID primario dell'utente|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/denyonlyprimarysid|  
|SID gruppo primario di sola negazione|L'istruzione deny\-solo SID gruppo primario dell'utente|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/denyonlyprimarygroupsid|  
|SID gruppo|SID di gruppo dell'utente|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/groupsid|  
|SID gruppo primario|SID di gruppo primario dell'utente|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/primarygroupsid|  
|SID primario|SID primario dell'utente|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/primarysid|  
|Nome account Windows|Il nome di account di dominio dell'utente nel formato \<domain\>\\\<utente\>|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/windowsaccountname|  
  
## <a name="what-are-claim-descriptions"></a>Cosa sono le descrizioni di attestazioni?  
Le descrizioni di attestazioni rappresentano un elenco dei tipi di attestazioni che supporta AD FS e che possono essere pubblicati nei metadati della federazione. I tipi di attestazioni menzionati nella tabella precedente vengono configurati come descrizioni di attestazioni nello snap di gestione di AD FS\-in.  
  
La raccolta di descrizioni di attestazioni che verrà pubblicata nei metadati federativi è archiviata nel database di configurazione di ADFS. Tali descrizioni di attestazioni vengono usate da vari componenti del servizio federativo.  
  
Ogni descrizione di attestazione include URI, nome, stato di pubblicazione e descrizione per il tipo di attestazione. È possibile gestire la raccolta di descrizione di attestazione utilizzando il **descrizioni attestazione** nodo nello snap di gestione di AD FS\-in. È possibile modificare lo stato di una descrizione di attestazione utilizzando lo snap-pubblicazione\-in. Sono disponibili le impostazioni seguenti:  
  
-   **Pubblica l'attestazione nei metadati federativi come tipo di attestazione che può accettare questo servizio federativo** \(pubblica come accettato\): indica i tipi di attestazione che verranno accettati da altri provider di attestazioni per questa federazione Servizio.  
  
-   **Pubblica l'attestazione nei metadati federativi come tipo di attestazione che questo servizio federativo può inviare** \(pubblica come inviato\): indica i tipi di attestazione offerti da questo servizio federativo. Questi sono i tipi di attestazioni che il servizio federativo pubblica agli altri come tipi di attestazioni disponibili per l'invio. I tipi di attestazioni effettivi inviati dal provider di attestazioni sono spesso un sottoinsieme di questo elenco.  
  
Per altre informazioni su come impostare lo stato di pubblicazione di un tipo di attestazione, vedere [aggiungere una descrizione di attestazione](https://technet.microsoft.com/library/dd807051.aspx) nella Guida alla distribuzione di AD FS.  
  
### <a name="when-generating-federation-metadata"></a>Durante la generazione dei metadati federativi  
Questi includono tutte le descrizioni di attestazioni contrassegnate per la pubblicazione.  
  
### <a name="when-claims-rules-are-processed"></a>Quando vengono elaborate le regole attestazione  
Se si mantengono informazioni di configurazione relative alle descrizioni di attestazioni, risulterà più facile configurare regole relative alle attestazioni. Per altre informazioni sulle regole attestazione che possono essere usate nell'organizzazione del provider di attestazioni, vedere [Ruolo delle regole attestazione](The-Role-of-Claim-Rules.md).  
  

