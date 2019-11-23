---
ms.assetid: 22f53391-8c6a-4873-a1f4-08b4760ea621
title: Ruolo delle attestazioni
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 851a70bbed606530ca8292f65bc4f776eae77fae
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407347"
---
# <a name="the-role-of-claims"></a>Ruolo delle attestazioni
Nel modello di identità basato sulle attestazioni\-, le attestazioni svolgono un ruolo fondamentale nel processo di federazione, costituiscono il componente chiave in base al quale vengono determinati i risultati di tutte le richieste di autenticazione e autorizzazione basate su Web\-. Questo modello consente alle organizzazioni di proiettare in modo sicuro identità digitali, diritti o *attestazioni* attraverso limiti di sicurezza aziendali in modo standardizzato.  
  
## <a name="what-are-claims"></a>Cosa sono le attestazioni?  
Nella sua forma più semplice, le attestazioni sono semplicemente *istruzioni* \(ad esempio nome, identità, gruppo\), informazioni sugli utenti, che vengono usate principalmente per autorizzare l'accesso alle attestazioni\-le applicazioni basate in qualsiasi posizione in Internet. Ogni istruzione corrisponde a un *valore* archiviato nell'attestazione.  
  
### <a name="how-claims-are-sourced"></a>Come vengono ottenute le attestazioni  
Il Servizio federativo in Active Directory Federation Services \(AD FS\) definisce quali attestazioni vengono scambiate tra partner federati. Prima che ciò sia possibile, tuttavia, il servizio deve prima popolare od ottenere l'attestazione tramite un valore recuperato o calcolato. Ogni valore dell'attestazione rappresenta un valore di un utente, gruppo o entità e può essere recuperato in uno di due modi:  
  
1.  Quando il valore che costituisce l'attestazione viene recuperato da un archivio di attributi, ad esempio il valore dell'attributo Sales Department recuperato dalle proprietà di un account utente Active Directory. Per altre informazioni, vedere [Ruolo degli archivi attributi](The-Role-of-Attribute-Stores.md).  
  
2.  Quando il valore di un'attestazione in ingresso viene trasformata in un altro valore basato sulla logica espressa in una regola. Quando ad esempio un'attestazione in ingresso con il valore di Domain Admins viene trasformata in un nuovo valore di Administrators prima di essere inviata come attestazione in uscita. Per altre informazioni, vedere [Ruolo delle regole attestazione](The-Role-of-Claim-Rules.md).  
  
Le attestazioni possono includere valori come un indirizzo di posta elettronica\-, un nome dell'entità utente \(\)UPN, l'appartenenza a un gruppo e altri attributi dell'account.  
  
### <a name="how-claims-flow"></a>Flusso delle attestazioni  
Altre parti si basano sui valori delle attestazioni per eseguire attività di autorizzazione per le applicazioni basate su Web\-che ospitano. Queste entità sono denominate *relying party* nello snap-in di gestione ad FS\-in. Il Servizio federativo è responsabile del broker di trust tra più parti diverse. È progettato per elaborare e propagare lo scambio attendibile di attestazioni da un'organizzazione che inizialmente genera le attestazioni, dette anche *provider di attestazioni* nello snap-in di gestione ad FS\-in, a un relying party. Una relying party usa quindi queste attestazioni per operare decisioni in merito alle autorizzazioni.  
  
Il flusso di attestazioni che usa questo processo è noto come *pipeline delle attestazioni*. Vi sono tre passaggi nel flusso di attestazioni attraverso la pipeline delle attestazioni:  
  
1.  Le attestazioni ricevute dal provider di attestazioni vengono elaborate dalle regole di trasformazione accettazione nel trust del provider di attestazioni. Queste regole determinano quali attestazioni vengono accettate dal provider di attestazioni.  
  
2.  L'output delle regole di trasformazione accettazione viene usato come input per le regole di autorizzazione rilascio. Tali regole determinano se l'utente è autorizzato ad accedere alla relying party.  
  
3.  L'output delle regole di trasformazione accettazione viene usato come input per le regole di trasformazione rilascio. Tali regole determinano quali attestazioni verranno inviate alla relying party.  
  
Per altre informazioni, vedere [Ruolo delle pipeline delle attestazioni](The-Role-of-the-Claims-Pipeline.md)  
  
### <a name="how-claims-are-issued"></a>Come vengono emesse le attestazioni  
Quando si scrivono regole attestazione, l'origine delle attestazioni in ingresso per le regole attestazione varia in base alla scrittura o meno di regole nel trust del provider di attestazioni o nel trust della relying party. Quando si scrivono regole attestazione per un trust del provider di attestazioni, le attestazioni in ingresso sono quelle inviate dal provider di attestazioni attendibile al servizio federativo. Quando si scrivono regole per un trust della relying party, le attestazioni in ingresso sono quelle provenienti dalle regole attestazione del trust del provider di attestazioni applicabile. Per altre informazioni sulle attestazioni in ingresso e in uscita, vedere [Ruolo delle pipeline delle attestazioni](The-Role-of-the-Claims-Pipeline.md) e [Ruolo del motore delle attestazioni](The-Role-of-the-Claims-Engine.md).  
  
## <a name="what-are-claim-types"></a>Cosa sono i tipi di attestazioni?  
Un tipo di attestazione fornisce contesto per il valore dell'attestazione. Viene in genere espresso come Uniform Resource Identifier \(URI\). AD FS possibile supportare qualsiasi tipo di attestazione ed è configurato con i tipi di attestazione nella tabella seguente per impostazione predefinita.  
  
|Nome|Descrizione|URI|  
|--------|---------------|-------|  
|Indirizzo di posta elettronica\-E|Indirizzo di posta elettronica\-dell'utente|http:\/\/schemas.xmlsoap.org\/WS\/2005\/05\/Identity\/Claims\/EmailAddress|  
|Nome (di battesimo)|Nome (di battesimo) dell'utente|http:\/\/schemas.xmlsoap.org\/WS\/2005\/05\/Identity\/Claims\/DATANAME|  
|Nome|Nome univoco dell'utente|http:\/\/schemas.xmlsoap.org\/WS\/2005\/05\/Identity\/Claims\/nome|  
|UPN|Nome dell'entità utente \(\) UPN dell'utente|http:\/\/schemas.xmlsoap.org\/WS\/2005\/05\/Identity\/Claims\/UPN|  
|Nome comune|Nome comune dell'utente|http:\/\/schemas.xmlsoap.org\/Claims\/CommonName|  
|AD FS 1. x E\-indirizzo di posta elettronica|Il\-l'indirizzo di posta elettronica dell'utente quando si interopera con AD FS 1,1 o ADFS 1,0|http:\/\/schemas.xmlsoap.org\/Claims\/EmailAddress|  
|Gruppo|Gruppo a cui appartiene l'utente|http:\/\/schemas.xmlsoap.org\/Claims\/Group|  
|UPN AD FS 1.x|UPN dell'utente quando interagisce con AD FS 1.1 o AD FS 1.0|http:\/\/schemas.xmlsoap.org\/Claims\/UPN|  
|Ruolo|Ruolo dell'utente|http:\/\/schemas.microsoft.com\/WS\/2008\/06\/Identity\/Claims\/Role|  
|Surname|Cognome dell'utente|http:\/\/schemas.xmlsoap.org\/WS\/2005\/05\/Identity\/Claims\/cognome|  
|PPID|Identificatore privato dell'utente|http:\/\/schemas.xmlsoap.org\/WS\/2005\/05\/Identity\/Claims\/PrivatePersonalIdentifier|  
|Identificatore nome|Identificatore nome SAML dell'utente|http:\/\/schemas.xmlsoap.org\/WS\/2005\/05\/Identity\/Claims\/NameIdentifier|  
|Metodo di autenticazione|Metodo usato per autenticare l'utente|http:\/\/schemas.microsoft.com\/WS\/2008\/06\/Identity\/Claims\/AuthenticationMethod|  
|SID gruppo di sola negazione|SID del gruppo Deny\-only dell'utente|http:\/\/schemas.xmlsoap.org\/WS\/2005\/05\/Identity\/Claims\/DenyOnlySid|  
|SID primario di sola negazione|Nega\-solo SID primario dell'utente|http:\/\/schemas.microsoft.com\/WS\/2008\/06\/Identity\/Claims\/DenyOnlyPrimarySID|  
|SID gruppo primario di sola negazione|Il SID del gruppo di\-solo negato dell'utente|http:\/\/schemas.microsoft.com\/WS\/2008\/06\/Identity\/Claims\/DenyOnlyPrimaryGroupSID|  
|SID gruppo|SID di gruppo dell'utente|http:\/\/schemas.microsoft.com\/WS\/2008\/06\/Identity\/Claims\/GroupSid|  
|SID gruppo primario|SID di gruppo primario dell'utente|http:\/\/schemas.microsoft.com\/WS\/2008\/06\/Identity\/Claims\/PrimaryGroupSID|  
|SID primario|SID primario dell'utente|http:\/\/schemas.microsoft.com\/WS\/2008\/06\/Identity\/Claims\/PrimarySid|  
|Nome account Windows|Nome dell'account di dominio dell'utente sotto forma di \<dominio\>\\\<utente\>|http:\/\/schemas.microsoft.com\/WS\/2008\/06\/Identity\/Claims\/WindowsAccountName|  
  
## <a name="what-are-claim-descriptions"></a>Cosa sono le descrizioni di attestazioni?  
Le descrizioni delle attestazioni rappresentano un elenco di tipi di attestazioni che AD FS supporta e che possono essere pubblicati nei metadati federativi. I tipi di attestazione indicati nella tabella precedente sono configurati come descrizioni delle attestazioni nello snap-in di gestione AD FS\-in.  
  
La raccolta di descrizioni di attestazioni che verrà pubblicata nei metadati federativi è archiviata nel database di configurazione di ADFS. Tali descrizioni di attestazioni vengono usate da vari componenti del servizio federativo.  
  
Ogni descrizione di attestazione include URI, nome, stato di pubblicazione e descrizione per il tipo di attestazione. È possibile gestire la raccolta di descrizioni delle attestazioni usando il nodo **descrizioni attestazioni** nello snap-in di gestione ad FS\-in. È possibile modificare lo stato di pubblicazione di una descrizione di attestazione utilizzando il\-di snap in. Sono disponibili le impostazioni seguenti:  
  
-   **Pubblicare questa attestazione nei metadati federativi come tipo di attestazione che questo servizio federativo può accettare** \(pubblicare come accettato\): indica i tipi di attestazione che verranno accettati da altri provider di attestazioni da questo servizio federativo.  
  
-   **Pubblicare questa attestazione nei metadati federativi come tipo di attestazione che questo servizio federativo può inviare** \(pubblicare come inviato\): indica i tipi di attestazione offerti da questo servizio federativo. Questi sono i tipi di attestazioni che il servizio federativo pubblica agli altri come tipi di attestazioni disponibili per l'invio. I tipi di attestazioni effettivi inviati dal provider di attestazioni sono spesso un sottoinsieme di questo elenco.  
  
Per ulteriori informazioni su come impostare lo stato di pubblicazione di un tipo di attestazione, vedere [aggiungere una descrizione di attestazione](https://technet.microsoft.com/library/dd807051.aspx) nella Guida alla distribuzione di ad FS.  
  
### <a name="when-generating-federation-metadata"></a>Durante la generazione dei metadati federativi  
Questi includono tutte le descrizioni di attestazioni contrassegnate per la pubblicazione.  
  
### <a name="when-claims-rules-are-processed"></a>Quando vengono elaborate le regole attestazione  
Se si mantengono informazioni di configurazione relative alle descrizioni di attestazioni, risulterà più facile configurare regole relative alle attestazioni. Per altre informazioni sulle regole attestazione che possono essere usate nell'organizzazione del provider di attestazioni, vedere [Ruolo delle regole attestazione](The-Role-of-Claim-Rules.md).  
  

