---
ms.assetid: 204f5fe9-3611-4da0-b057-a386004b4598
title: Informazioni sulle chiavi di Active Directory Federation Services concetti
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 27282c6b88b0457af3b4cf031fdadced7b40268c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="understanding-key-ad-fs-concepts"></a>Informazioni sulla chiave AD FS Concepts
Si consiglia di conoscere i concetti importanti di Active Directory Federation Services e acquisire familiarità con il relativo set di funzionalità.  
  
> [!TIP]  
> È possibile trovare altri collegamenti alle risorse ADFS il [mappa del contenuto di AD FS](https://social.technet.microsoft.com/wiki/contents/articles/2735.aspx) pagina in Microsoft TechNet Wiki. Questa pagina è gestita dai membri della Community di ADFS ed è controllata regolarmente dal Team del prodotto ADFS.  
  
## <a name="ad-fs-terminology-used-in-this-guide"></a>Terminologia di ADFS usata in questa Guida  
  
|Termine di ADFS|Definizione|  
|--------------|--------------|  
|Organizzazione partner account|Un'organizzazione partner federativo rappresentato da un trust del provider di attestazioni nel servizio federativo. L'organizzazione partner account include gli utenti che accederanno alle applicazioni basate sul Web nel partner risorse.|  
|Server federativo di account|Il server federativo nell'organizzazione partner account. Server federativo di account rilascia token di sicurezza agli utenti in base all'autenticazione utente. Il server autentica l'utente, estrae gli attributi rilevanti e informazioni di appartenenza al gruppo fuori l'archivio di attributi, include queste informazioni in attestazioni e genera e firma un token di sicurezza \ (che contiene il claims\) da restituire all'utente, da utilizzare nella propria organizzazione o di essere inviati a un'organizzazione partner.|  
|Database di configurazione di ADFS|Un database utilizzato per archiviare tutti i dati di configurazione che rappresenta una singola istanza di AD FS o un servizio federativo. Questi dati di configurazione possono essere archiviati in un database SQL Server o usando la funzionalità Database interno di Windows incluso in Windows Server 2016, Windows Server 2012 e 2012 R2 e Windows Server 2008 e 2008 R2. </br></br>È possibile creare il database di configurazione di AD FS per SQL Server utilizzando lo strumento della riga di comando Fsconfig.exe o per Database interno di Windows tramite configurazione guidata Server federativo AD FS.|  
|Provider di attestazioni|L'organizzazione che fornisce attestazioni agli utenti. Vedere organizzazione partner account.|  
|Attendibilità provider di attestazioni|Nella gestione di ADFS snap-in, i provider di attestazioni sono oggetti trust creati solitamente nelle organizzazioni partner risorse che rappresentano l'organizzazione nella relazione di trust il cui account accederanno alle risorse nell'organizzazione partner risorse. Un oggetto trust del provider di attestazioni è costituito da diversi identificatori, nomi e regole che identificano il partner nel servizio federativo locale.|  
|Trust del Provider di attestazioni locale|Un oggetto trust che rappresenta AD LDS o directory basate su LDAP\ third\ parti in una farm ADFS. Oggetto è costituito da diversi identificatori, nomi e regole che identificano la directory basate su LDAP\ nel servizio federativo locale trust del provider di attestazioni locale.|  
|Metadati della federazione|Il formato dei dati per comunicare le informazioni di configurazione tra un provider di attestazioni e relying party per facilitare la configurazione corretta del provider di attestazioni e i trust della relying party. Il formato di dati è definito in Security Assertion Markup Language \(SAML\) 2.0 ed è esteso in WS-Federation.|  
|Server federativo|Un Server Windows che è stato configurato con configurazione guidata Server federativo AD FS di agire nel ruolo server federativo. Un server federativo rilascia token e funge da parte di un servizio federativo.|  
|Proxy server federativo|Un Server Windows che è stato configurato con la configurazione guidata di AD FS Federation Server Proxy per fungere da servizio proxy intermediario tra un client Internet e un servizio federativo che si trova dietro un firewall in una rete aziendale.|  
|Server federativo primario|Un Server Windows che è stato configurato nel ruolo del server federativo utilizzando Configurazione guidata Server federativo AD FS e dispone di una copia di lettura/scrittura del database di configurazione di ADFS. </br></br> Il server federativo primario viene creato quando si utilizza configurazione guidata Server federativo AD FS e selezionare l'opzione per creare un nuovo servizio federativo e rendere il computer come primo server federativo nella farm. Tutti gli altri server federativi della farm devono replicare le modifiche apportate nel server federativo primario in una sola copia del database di configurazione di ADFS archiviato localmente. Il termine "server federativo primario" non si applica quando il database di configurazione di ADFS è archiviato in un database SQL, perché tutti i server federativi possono leggere e scrivere in un database di configurazione archiviato in SQL Server.|  
|Relying party|L'organizzazione che riceve ed elabora attestazioni. Vedere organizzazione partner risorse.|  
|Trust della relying party|Nella gestione di ADFS snap-in, trust della relying party sono oggetti trust creati solitamente in:<br /><br />-Le organizzazioni partner account rappresentano l'organizzazione nella relazione di trust il cui account accederanno alle risorse nell'organizzazione partner risorse.<br />-Organizzazioni partner risorse che rappresentano il trust tra il servizio federativo e una singola applicazione basata sul Web.<br /><br />Un oggetto di trust relying party è costituito da diversi identificatori, nomi e regole che identificano il partner o l'applicazione sul Web nel servizio federativo locale.|  
|Server federativo di risorsa|Il server federativo nell'organizzazione partner risorse. Il server federativo di risorsa rilascia solitamente token di sicurezza agli utenti in base a un token di sicurezza emesso da un server federativo di account. Il server riceve il token di sicurezza, verifica la firma, applica la logica di regola attestazione per le attestazioni decompresse per produrre le attestazioni in uscita desiderate, genera un nuovo token di sicurezza \ (con claims\ in uscita) in base alle informazioni nel token di sicurezza in ingresso e firma il nuovo token da restituire all'utente e infine all'applicazione Web.|  
|Organizzazione partner risorse|Partner federativo rappresentato da un trust della relying party nel servizio federativo. Il partner risorse rilascia token di sicurezza basata su claims\ che contiene applicazioni pubblicate basata sul Web che possono accedere gli utenti nel partner account.|  
  
## <a name="overview-of-ad-fs"></a>Panoramica di ADFS  
ADFS è una soluzione di accesso di identità che fornisce ai computer client \ (interno o esterno per l'isolamento di rete) con l'accesso SSO trasparente alle protetti con connessione Internet applicazioni o servizi, anche quando gli account utente e le applicazioni si trovano in organizzazioni o reti completamente diverse.  
  
Quando un'applicazione o servizio in una rete e un account utente in un'altra rete, in genere l'utente viene richiesto delle credenziali secondarie quando prova ad accedere all'applicazione o servizio. Le credenziali secondarie rappresentano l'identità dell'utente nell'area in cui risiede l'applicazione o il servizio di autenticazione. Sono di solito richieste dal server Web che ospita l'applicazione o servizio in modo da poter apportare la decisione di autorizzazione appropriata.  
  
Con AD FS, le organizzazioni possono ignorare le richieste di credenziali secondarie fornendo \(federation trusts\) relazioni trust che le organizzazioni possono usare per la proiezione digitale identità e accesso diritti di un utente a partner attendibili. In questo ambiente federativo ogni organizzazione continua a gestire le proprie identità, ma ogni organizzazione può anche in modo sicuro distribuire e accettare le identità di altre organizzazioni.  
  
-   [Il ruolo degli archivi attributi](The-Role-of-Attribute-Stores.md)  
  
-   [Il ruolo del Database di configurazione di ADFS](The-Role-of-the-AD-FS-Configuration-Database.md)  
  
-   [Ruolo delle attestazioni](The-Role-of-Claims.md)  
  
-   [Il ruolo di regole attestazioni](The-Role-of-Claim-Rules.md)  
  
-   [Il ruolo del motore di attestazioni](The-Role-of-the-Claims-Engine.md)  
  
-   [Il ruolo delle Pipeline delle attestazioni](The-Role-of-the-Claims-Pipeline.md)  
  
-   [Il ruolo del linguaggio di regola attestazione](The-Role-of-the-Claim-Rule-Language.md)  
  
-   [Determinare il tipo di modello di regola attestazione da utilizzare](Determine-the-Type-of-Claim-Rule-Template-to-Use.md)  
  
-   [Come usare gli URI in AD FS](How-URIs-Are-Used-in-AD-FS.md)  
  

