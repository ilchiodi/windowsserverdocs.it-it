---
ms.assetid: 204f5fe9-3611-4da0-b057-a386004b4598
title: Informazioni sui concetti chiave Active Directory Federation Services
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d212c2f820204bae8e1e55dc1ebc44200e266a18
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407317"
---
# <a name="understanding-key-ad-fs-concepts"></a>Informazioni sui concetti principali relativi ad AD FS
È consigliabile conoscere i concetti importanti per Active Directory Federation Services e acquisire familiarità con il relativo set di funzionalità.  
  
> [!TIP]  
> È possibile trovare altri collegamenti alle risorse di AD FS nella pagina della [mappa del contenuto di AD FS](https://social.technet.microsoft.com/wiki/contents/articles/2735.aspx) nel sito Wiki di Microsoft TechNet. Questa pagina è gestita dai membri della community di ADFS ed è controllata regolarmente dal team del prodotto ADFS.  
  
## <a name="ad-fs-terminology-used-in-this-guide"></a>Terminologia di ADFS usata in questa guida  
  
|Termine di ADFS|Definizione|  
|--------------|--------------|  
|Organizzazione partner account|Organizzazione partner federativo rappresentata da un trust del provider di attestazioni nel Servizio federativo. Organizzazione partner account include gli utenti che avranno accesso Web\-applicazioni nel partner risorse.|  
|Server federativo di account|Server federativo nell'organizzazione partner account. Il server federativo di account rilascia token di sicurezza agli utenti in base all'autenticazione utente. Il server autentica l'utente, estrae gli attributi rilevanti e le informazioni sull'appartenenza a gruppi dall'archivio attributi, inserisce tali informazioni in attestazioni e genera e firma un token di sicurezza \(which contiene le attestazioni @ no__t-1 da restituire. all'utente, da usare nella propria organizzazione o da inviare a un'organizzazione partner.|  
|Database di configurazione di ADFS|Database usato per archiviare tutti i dati di configurazione che rappresentano una singola istanza di ADFS o il Servizio federativo. Questi dati di configurazione possono essere archiviati in un database di SQL Server o usando la funzionalità database interno di Windows incluso in Windows Server 2016, Windows Server 2012 e 2012 R2 e Windows Server 2008 e 2008 R2. </br></br>È possibile creare il database di configurazione AD FS per SQL Server utilizzando lo strumento Fsconfig. exe Command @ no__t-0line e per il database interno di Windows utilizzando la configurazione guidata server federativo di AD FS.|  
|Provider di attestazioni|Organizzazione che fornisce attestazioni agli utenti. Vedere organizzazione partner account.|  
|Trust del provider di attestazioni|Nello snap-in di gestione AD FS @ no__t-0cm, i trust del provider di attestazioni sono oggetti trust creati in genere nelle organizzazioni partner risorse per rappresentare l'organizzazione nella relazione di trust i cui account accederanno alle risorse nel partner risorse organizzazione. Un oggetto trust del provider di attestazioni è costituito da diversi identificatori, nomi e regole che identificano il partner nel Servizio federativo locale.|  
|Trust del provider di attestazioni locale|Oggetto trust che rappresenta AD LDS o terza directory @ no__t-0party LDAP @ no__t-1Basato in una farm di AD FS. Un oggetto trust del provider di attestazioni locale è costituito da diversi identificatori, nomi e regole che identificano la directory LDAP @ no__t-0based per la Servizio federativo locale.|  
|Metadati federativi|Formato dei dati per la comunicazione di informazioni di configurazione tra un provider di attestazioni e una relying party per facilitare la configurazione corretta dei trust del provider di attestazioni e dei trust della relying party. Il formato dei dati è definito in Security Assertion Markup Language \(SAML @ no__t-1 2,0 ed è esteso in WS @ no__t-2Federation.|  
|Server federativo|Un server Windows che è stato configurato utilizzando la configurazione guidata del server federativo di AD FS per agire nel ruolo server federativo. Un server federativo rilascia token e fa parte del Servizio federativo.|  
|Proxy server federativo|Un server Windows che è stato configurato utilizzando la configurazione guidata del proxy server federativo di AD FS per fungere da servizio proxy intermediario tra un client Internet e un Servizio federativo protetto da un firewall in una rete aziendale.|  
|Server federativo primario|Un server Windows che è stato configurato nel ruolo server federativo utilizzando la configurazione guidata server federativo di AD FS e che include una copia @ no__t-0write del database di configurazione AD FS. </br></br> Il server federativo primario viene creato quando si usa la configurazione guidata server federativo di AD FS e si seleziona l'opzione per creare un nuovo Servizio federativo e si rende il computer il primo server federativo nella farm. Tutti gli altri server federativi della farm devono replicare le modifiche apportate nel server federativo primario a una copia Read @ no__t-0only del database di configurazione AD FS archiviato localmente. Il termine "server federativo primario" non si applica quando il database di configurazione di ADFS è archiviato in un database SQL, perché tutti i server federativi possono leggere e scrivere in un database di configurazione archiviato in SQL Server.|  
|Relying party|Organizzazione che riceve ed elabora attestazioni. Vedere organizzazione partner risorse.|  
|Trust della relying party|Nello snap-in di gestione AD FS @ no__t-0cm, i trust relying party sono oggetti trust creati in genere in:<br /><br />-Organizzazioni partner account che rappresentano l'organizzazione nella relazione di trust i cui account accederanno alle risorse nell'organizzazione partner risorse.<br />-Organizzazioni partner risorse per rappresentare il trust tra il Servizio federativo e una singola applicazione Web @ no__t-0based.<br /><br />Un oggetto relying party trust è costituito da diversi identificatori, nomi e regole che identificano il partner o Web @ no__t-0application per la Servizio federativo locale.|  
|Server federativo di risorsa|Server federativo nell'organizzazione partner risorse. Il server federativo di risorsa rilascia solitamente token di sicurezza agli utenti in base a un token di sicurezza rilasciato da un server federativo di account. Il server riceve il token di sicurezza, verifica la firma, applica la logica della regola attestazioni alle attestazioni non incluse nel pacchetto per produrre le attestazioni in uscita desiderate, genera un nuovo token di sicurezza \(with le attestazioni in uscita @ no__t-1 in base alle informazioni contenute in ingresso token di sicurezza e firma il nuovo token per tornare all'utente e infine all'applicazione Web.|  
|Organizzazione partner risorse|Partner federativo rappresentato da un trust della relying party nel Servizio federativo. Il partner risorse rilascia i token di sicurezza @ no__t-0based che contengono le applicazioni Web @ no__t-1Basato pubblicate a cui possono accedere gli utenti nel partner account.|  
  
## <a name="overview-of-ad-fs"></a>Panoramica di ADFS  
AD FS è una soluzione di accesso alle identità che fornisce ai computer client \(internal o esterni alla rete @ no__t-1 con accesso SSO facile ai servizi o alle applicazioni internet protette @ no__t-2facing, anche quando gli account utente e le applicazioni sono si trova in reti o organizzazioni completamente diverse.  
  
Quando un'applicazione o in servizio si trova in una rete e un account utente è in un'altra rete, all'utente vengono in genere richieste credenziali secondarie quando prova ad accedere all'applicazione o al servizio. Le credenziali secondarie rappresentano l'identità dell'utente nell'area di autenticazione in cui risiede l'applicazione o il servizio. Sono di solito richieste dal server Web che ospita l'applicazione o il servizio per poter applicare la decisione di autorizzazione appropriata.  
  
Con AD FS, le organizzazioni possono ignorare le richieste di credenziali secondarie fornendo relazioni di trust \(federation Trusts @ no__t-1 che queste organizzazioni possono usare per proiettare l'identità digitale e i diritti di accesso di un utente a partner attendibili. In questo ambiente federativo ogni organizzazione continua a gestire le proprie identità, ma può anche distribuire e accettare in modo sicuro identità di altre organizzazioni.  
  
-   [Ruolo degli archivi di attributi](The-Role-of-Attribute-Stores.md)  
  
-   [Ruolo del database di configurazione di AD FS](The-Role-of-the-AD-FS-Configuration-Database.md)  
  
-   [Ruolo delle attestazioni](The-Role-of-Claims.md)  
  
-   [Ruolo delle regole delle attestazioni](The-Role-of-Claim-Rules.md)  
  
-   [Ruolo del motore delle attestazioni](The-Role-of-the-Claims-Engine.md)  
  
-   [Ruolo della pipeline delle attestazioni](The-Role-of-the-Claims-Pipeline.md)  
  
-   [Ruolo del linguaggio delle regole delle attestazioni](The-Role-of-the-Claim-Rule-Language.md)  
  
-   [Determinare il tipo di modello di regola attestazioni da usare](Determine-the-Type-of-Claim-Rule-Template-to-Use.md)  
  
-   [Modalità d'uso degli URI in AD FS](How-URIs-Are-Used-in-AD-FS.md)  
  

