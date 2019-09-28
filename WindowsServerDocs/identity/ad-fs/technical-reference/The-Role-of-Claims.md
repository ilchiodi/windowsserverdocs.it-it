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
Nel modello di identità Claims @ no__t-0based, le attestazioni svolgono un ruolo fondamentale nel processo di federazione, costituiscono il componente chiave in base al quale vengono determinati i risultati di tutte le richieste di autenticazione e autorizzazione Web @ no__t-1Basato. Questo modello consente alle organizzazioni di proiettare in modo sicuro identità digitali, diritti o *attestazioni* attraverso limiti di sicurezza aziendali in modo standardizzato.  
  
## <a name="what-are-claims"></a>Cosa sono le attestazioni?  
Nella sua forma più semplice, le attestazioni sono semplicemente *istruzioni* \(per esempio, nome, identità, gruppo @ no__t-2, fatto sugli utenti, che vengono usate principalmente per autorizzare l'accesso alle attestazioni @ no__t-3based in qualsiasi posizione in Internet. Ogni istruzione corrisponde a un *valore* archiviato nell'attestazione.  
  
### <a name="how-claims-are-sourced"></a>Come vengono ottenute le attestazioni  
Il Servizio federativo in Active Directory Federation Services \(AD FS @ no__t-1 definisce quali attestazioni vengono scambiate tra partner federati. Prima che ciò sia possibile, tuttavia, il servizio deve prima popolare od ottenere l'attestazione tramite un valore recuperato o calcolato. Ogni valore dell'attestazione rappresenta un valore di un utente, gruppo o entità e può essere recuperato in uno di due modi:  
  
1.  Quando il valore che costituisce l'attestazione viene recuperato da un archivio di attributi, ad esempio il valore dell'attributo Sales Department recuperato dalle proprietà di un account utente Active Directory. Per altre informazioni, vedere [Ruolo degli archivi attributi](The-Role-of-Attribute-Stores.md).  
  
2.  Quando il valore di un'attestazione in ingresso viene trasformata in un altro valore basato sulla logica espressa in una regola. Quando ad esempio un'attestazione in ingresso con il valore di Domain Admins viene trasformata in un nuovo valore di Administrators prima di essere inviata come attestazione in uscita. Per altre informazioni, vedere [Ruolo delle regole attestazione](The-Role-of-Claim-Rules.md).  
  
Le attestazioni possono includere valori quali un indirizzo e @ no__t-0mail, il nome dell'entità utente \(UPN @ no__t-2, l'appartenenza al gruppo e altri attributi dell'account.  
  
### <a name="how-claims-flow"></a>Flusso delle attestazioni  
Altre parti si basano sui valori delle attestazioni per eseguire attività di autorizzazione per le applicazioni Web @ no__t-0based che ospitano. Queste entità sono denominate *relying party* nello snap-in di gestione ad FS @ no__t-1in. Il Servizio federativo è responsabile del broker di trust tra più parti diverse. È progettato per elaborare e propagare lo scambio attendibile di attestazioni da un'organizzazione che inizialmente genera le attestazioni, dette anche *provider di attestazioni* nello snap-in di gestione ad FS @ no__t-1in, a una relying party. Una relying party usa quindi queste attestazioni per operare decisioni in merito alle autorizzazioni.  
  
Il flusso di attestazioni che usa questo processo è noto come *pipeline delle attestazioni*. Vi sono tre passaggi nel flusso di attestazioni attraverso la pipeline delle attestazioni:  
  
1.  Le attestazioni ricevute dal provider di attestazioni vengono elaborate dalle regole di trasformazione accettazione nel trust del provider di attestazioni. Queste regole determinano quali attestazioni vengono accettate dal provider di attestazioni.  
  
2.  L'output delle regole di trasformazione accettazione viene usato come input per le regole di autorizzazione rilascio. Tali regole determinano se l'utente è autorizzato ad accedere alla relying party.  
  
3.  L'output delle regole di trasformazione accettazione viene usato come input per le regole di trasformazione rilascio. Tali regole determinano quali attestazioni verranno inviate alla relying party.  
  
Per altre informazioni, vedere [Ruolo delle pipeline delle attestazioni](The-Role-of-the-Claims-Pipeline.md)  
  
### <a name="how-claims-are-issued"></a>Come vengono emesse le attestazioni  
Quando si scrivono regole attestazione, l'origine delle attestazioni in ingresso per le regole attestazione varia in base alla scrittura o meno di regole nel trust del provider di attestazioni o nel trust della relying party. Quando si scrivono regole attestazione per un trust del provider di attestazioni, le attestazioni in ingresso sono quelle inviate dal provider di attestazioni attendibile al servizio federativo. Quando si scrivono regole per un trust della relying party, le attestazioni in ingresso sono quelle provenienti dalle regole attestazione del trust del provider di attestazioni applicabile. Per altre informazioni sulle attestazioni in ingresso e in uscita, vedere [Ruolo delle pipeline delle attestazioni](The-Role-of-the-Claims-Pipeline.md) e [Ruolo del motore delle attestazioni](The-Role-of-the-Claims-Engine.md).  
  
## <a name="what-are-claim-types"></a>Cosa sono i tipi di attestazioni?  
Un tipo di attestazione fornisce contesto per il valore dell'attestazione. Viene in genere espresso come Uniform Resource Identifier \(URI @ no__t-1. AD FS possibile supportare qualsiasi tipo di attestazione ed è configurato con i tipi di attestazione nella tabella seguente per impostazione predefinita.  
  
|Nome|Descrizione|URI|  
|--------|---------------|-------|  
|E @ no__t-indirizzo 0Mail|Indirizzo e @ no__t-0mail dell'utente|http: @no__t -0\/schemas.xmlsoap.org @ no__t-2WS @ no__t-32005 @ no__t-405 @ no__t-5identity @ no__t-6claims @ no__t-7emailaddress|  
|Nome (di battesimo)|Nome (di battesimo) dell'utente|http: @no__t -0\/schemas.xmlsoap.org @ no__t-2WS @ no__t-32005 @ no__t-405 @ no__t-5identity @ no__t-6claims @ no__t-7givenname|  
|Nome|Nome univoco dell'utente|http: @no__t -0\/schemas.xmlsoap.org @ no__t-2WS @ no__t-32005 @ no__t-405 @ no__t-5identity @ no__t-6claims @ no__t-7name|  
|UPN|Nome dell'entità utente \(UPN @ no__t-1 dell'utente|http: @no__t -0\/schemas.xmlsoap.org @ no__t-2WS @ no__t-32005 @ no__t-405 @ no__t-5identity @ no__t-6claims @ no__t-7upn|  
|Nome comune|Nome comune dell'utente|http: @no__t -0\/schemas.xmlsoap.org @ no__t-2claims @ no__t-3CommonName|  
|AD FS 1. x E @ no__t-indirizzo 0Mail|Indirizzo e @ no__t-0mail dell'utente per l'interoperabilità con AD FS 1,1 o ADFS 1,0|http: @no__t -0\/schemas.xmlsoap.org @ no__t-2claims @ no__t-3EmailAddress|  
|Group|Gruppo a cui appartiene l'utente|http: @no__t -0\/schemas.xmlsoap.org @ no__t-2claims @ no__t-3Group|  
|UPN AD FS 1.x|UPN dell'utente quando interagisce con AD FS 1.1 o AD FS 1.0|http: @no__t -0\/schemas.xmlsoap.org @ no__t-2claims @ no__t-3UPN|  
|Role|Ruolo dell'utente|http: @no__t -0\/schemas.microsoft.com @ no__t-2WS @ no__t-32008 @ no__t-406 @ no__t-5identity @ no__t-6claims @ no__t-7role|  
|Surname|Cognome dell'utente|http: @no__t -0\/schemas.xmlsoap.org @ no__t-2WS @ no__t-32005 @ no__t-405 @ no__t-5identity @ no__t-6claims @ no__t-7surname|  
|PPID|Identificatore privato dell'utente|http: @no__t -0\/schemas.xmlsoap.org @ no__t-2WS @ no__t-32005 @ no__t-405 @ no__t-5identity @ no__t-6claims @ no__t-7privatepersonalidentifier|  
|Identificatore nome|Identificatore nome SAML dell'utente|http: @no__t -0\/schemas.xmlsoap.org @ no__t-2WS @ no__t-32005 @ no__t-405 @ no__t-5identity @ no__t-6claims @ no__t-7nameidentifier|  
|Metodo di autenticazione|Metodo usato per autenticare l'utente|http: @no__t -0\/schemas.microsoft.com @ no__t-2WS @ no__t-32008 @ no__t-406 @ no__t-5identity @ no__t-6claims @ no__t-7authenticationmethod|  
|SID gruppo di sola negazione|SID del gruppo Deny @ no__t-0only dell'utente|http: @no__t -0\/schemas.xmlsoap.org @ no__t-2WS @ no__t-32005 @ no__t-405 @ no__t-5identity @ no__t-6claims @ no__t-7denyonlysid|  
|SID primario di sola negazione|Il SID primario dell'utente Deny @ no__t-0only|http: @no__t -0\/schemas.microsoft.com @ no__t-2WS @ no__t-32008 @ no__t-406 @ no__t-5identity @ no__t-6claims @ no__t-7denyonlyprimarysid|  
|SID gruppo primario di sola negazione|SID del gruppo primario Deny @ no__t-0only dell'utente|http: @no__t -0\/schemas.microsoft.com @ no__t-2WS @ no__t-32008 @ no__t-406 @ no__t-5identity @ no__t-6claims @ no__t-7denyonlyprimarygroupsid|  
|SID gruppo|SID di gruppo dell'utente|http: @no__t -0\/schemas.microsoft.com @ no__t-2WS @ no__t-32008 @ no__t-406 @ no__t-5identity @ no__t-6claims @ no__t-7groupsid|  
|SID gruppo primario|SID di gruppo primario dell'utente|http: @no__t -0\/schemas.microsoft.com @ no__t-2WS @ no__t-32008 @ no__t-406 @ no__t-5identity @ no__t-6claims @ no__t-7primarygroupsid|  
|SID primario|SID primario dell'utente|http: @no__t -0\/schemas.microsoft.com @ no__t-2WS @ no__t-32008 @ no__t-406 @ no__t-5identity @ no__t-6claims @ no__t-7primarysid|  
|Nome account Windows|Nome dell'account di dominio dell'utente nel formato \<domain @ no__t-1 @ no__t-2 @ no__t-3User @ no__t-4|http: @no__t -0\/schemas.microsoft.com @ no__t-2WS @ no__t-32008 @ no__t-406 @ no__t-5identity @ no__t-6claims @ no__t-7windowsaccountname|  
  
## <a name="what-are-claim-descriptions"></a>Cosa sono le descrizioni di attestazioni?  
Le descrizioni delle attestazioni rappresentano un elenco di tipi di attestazioni che AD FS supporta e che possono essere pubblicati nei metadati federativi. I tipi di attestazione indicati nella tabella precedente sono configurati come descrizioni delle attestazioni nello snap-in di gestione AD FS @ no__t-0cm.  
  
La raccolta di descrizioni di attestazioni che verrà pubblicata nei metadati federativi è archiviata nel database di configurazione di ADFS. Tali descrizioni di attestazioni vengono usate da vari componenti del servizio federativo.  
  
Ogni descrizione di attestazione include URI, nome, stato di pubblicazione e descrizione per il tipo di attestazione. È possibile gestire la raccolta di descrizioni delle attestazioni usando il nodo **descrizioni attestazioni** nello snap-in di gestione ad FS @ no__t-1in. È possibile modificare lo stato di pubblicazione di una descrizione di attestazione usando lo snap @ no__t-0cm. Sono disponibili le impostazioni seguenti:  
  
-   **Pubblicare questa attestazione nei metadati federativi come tipo di attestazione che questo servizio federativo può accettare** \(Publish come accettato @ no__t-2 — indica i tipi di attestazione che verranno accettati da altri provider di attestazioni da questa servizio federativo.  
  
-   **Pubblicare questa attestazione nei metadati federativi come tipo di attestazione che questo servizio federativo può inviare** \(Publish come inviato @ no__t-2 — indica i tipi di attestazione offerti da questo servizio federativo. Questi sono i tipi di attestazioni che il servizio federativo pubblica agli altri come tipi di attestazioni disponibili per l'invio. I tipi di attestazioni effettivi inviati dal provider di attestazioni sono spesso un sottoinsieme di questo elenco.  
  
Per ulteriori informazioni su come impostare lo stato di pubblicazione di un tipo di attestazione, vedere [aggiungere una descrizione di attestazione](https://technet.microsoft.com/library/dd807051.aspx) nella Guida alla distribuzione di ad FS.  
  
### <a name="when-generating-federation-metadata"></a>Durante la generazione dei metadati federativi  
Questi includono tutte le descrizioni di attestazioni contrassegnate per la pubblicazione.  
  
### <a name="when-claims-rules-are-processed"></a>Quando vengono elaborate le regole attestazione  
Se si mantengono informazioni di configurazione relative alle descrizioni di attestazioni, risulterà più facile configurare regole relative alle attestazioni. Per altre informazioni sulle regole attestazione che possono essere usate nell'organizzazione del provider di attestazioni, vedere [Ruolo delle regole attestazione](The-Role-of-Claim-Rules.md).  
  

