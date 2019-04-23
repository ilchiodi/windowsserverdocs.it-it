---
ms.assetid: 204f5fe9-3611-4da0-b057-a386004b4598
title: Concetti di informazioni sulle chiavi di Active Directory Federation Services
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 27282c6b88b0457af3b4cf031fdadced7b40268c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878132"
---
>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="understanding-key-ad-fs-concepts"></a>Informazioni sui concetti principali relativi ad AD FS
Si consiglia di apprendere i concetti importanti di Active Directory Federation Services e acquisire familiarità con il set di caratteristiche.  
  
> [!TIP]  
> È possibile trovare altri collegamenti alle risorse di AD FS nella pagina della [mappa del contenuto di AD FS](https://social.technet.microsoft.com/wiki/contents/articles/2735.aspx) nel sito Wiki di Microsoft TechNet. Questa pagina è gestita dai membri della community di ADFS ed è controllata regolarmente dal team del prodotto ADFS.  
  
## <a name="ad-fs-terminology-used-in-this-guide"></a>Terminologia di ADFS usata in questa guida  
  
|Termine di ADFS|Definizione|  
|--------------|--------------|  
|Organizzazione partner account|Organizzazione partner federativo rappresentata da un trust del provider di attestazioni nel Servizio federativo. Organizzazione partner account include gli utenti che avranno accesso Web\-applicazioni nel partner risorse.|  
|Server federativo di account|Server federativo nell'organizzazione partner account. Il server federativo di account rilascia token di sicurezza agli utenti in base all'autenticazione utente. Il server autentica l'utente, estrae attributi rilevanti e le informazioni di appartenenza dall'archivio di attributi, include queste informazioni in attestazioni e genera e firma un token di sicurezza \(che contiene le attestazioni\)da restituire all'utente, da utilizzare nella propria organizzazione o da inviare a un'organizzazione partner.|  
|Database di configurazione di ADFS|Database usato per archiviare tutti i dati di configurazione che rappresentano una singola istanza di ADFS o il Servizio federativo. Questi dati di configurazione possono essere archiviati in un database di SQL Server o usando la funzionalità Database interno di Windows incluso in Windows Server 2016, Windows Server 2012 e 2012 R2 e Windows Server 2008 e 2008 R2. </br></br>È possibile creare il database di configurazione di AD FS per SQL Server usando il comando Fsconfig.exe\-strumento della riga e per il Database interno di Windows tramite configurazione guidata Server federativo AD FS.|  
|Provider di attestazioni|Organizzazione che fornisce attestazioni agli utenti. Vedere organizzazione partner account.|  
|Trust del provider di attestazioni|Nello snap di gestione di AD FS\-, attestazioni trust del provider sono oggetti trust creati solitamente nelle organizzazioni partner risorse che rappresentano l'organizzazione nella relazione di trust cui account accederanno alle risorse nella risorsa organizzazione partner. Un oggetto trust del provider di attestazioni è costituito da diversi identificatori, nomi e regole che identificano il partner nel Servizio federativo locale.|  
|Trust del provider di attestazioni locale|Un oggetto trust che rappresenta AD LDS o terze\-LDAP di terze parti\-basato su directory in una farm AD FS. Oggetto costituito da diversi identificatori, nomi e regole che identificano questo LDAP trust del provider di attestazioni locale\-basato su directory in cui il servizio federativo locale.|  
|Metadati federativi|Formato dei dati per la comunicazione di informazioni di configurazione tra un provider di attestazioni e una relying party per facilitare la configurazione corretta dei trust del provider di attestazioni e dei trust della relying party. Il formato dati è definito in Security Assertion Markup Language \(SAML\) 2.0 che viene esteso in WS\-Federation.|  
|Server federativo|Un Server Windows che è stato configurato utilizzando Configurazione guidata Server federativo AD FS di agire nel ruolo server federativo. Un server federativo rilascia token e fa parte del Servizio federativo.|  
|Proxy server federativo|Un Server Windows che è stato configurato usando la configurazione guidata di AD FS Federation Server Proxy per fungere da servizio un proxy intermediario tra un client Internet e un servizio federativo situato dietro un firewall in una rete aziendale.|  
|Server federativo primario|Un Server Windows che è stato configurato nel ruolo server federativo utilizzando Configurazione guidata Server federativo AD FS e dispone di un'operazione di lettura\/scrivere copia del database di configurazione di AD FS. </br></br> Il server federativo primario viene creato quando si utilizza configurazione guidata Server federativo AD FS e selezionare l'opzione per creare un nuovo servizio federativo e rendere il computer del primo server federativo nella farm. Tutti gli altri server federativi della farm devono replicare le modifiche apportate nel server federativo primario a una lettura\-solo una copia del database di configurazione di ADFS archiviato localmente. Il termine "server federativo primario" non si applica quando il database di configurazione di ADFS è archiviato in un database SQL, perché tutti i server federativi possono leggere e scrivere in un database di configurazione archiviato in SQL Server.|  
|Relying party|Organizzazione che riceve ed elabora attestazioni. Vedere organizzazione partner risorse.|  
|Trust della relying party|Nello snap di gestione di AD FS\-, trust della relying party sono oggetti trust creati solitamente in:<br /><br />-Organizzazioni partner account per rappresentare l'organizzazione nella relazione di trust, il cui account accederanno alle risorse nell'organizzazione partner risorse.<br />-Organizzazioni partner risorse che rappresentano la relazione di trust tra il servizio federativo e una sola web\-applicazione basata su.<br /><br />Un oggetto trust della relying party è costituito da diversi identificatori, nomi e regole che identificano il partner o web\-applicazione al servizio federativo locale.|  
|Server federativo di risorsa|Server federativo nell'organizzazione partner risorse. Il server federativo di risorsa rilascia solitamente token di sicurezza agli utenti in base a un token di sicurezza rilasciato da un server federativo di account. Il server riceve il token di sicurezza, verifica la firma, viene applicata logica delle regole attestazioni alle attestazioni estratto dal pacchetto per produrre le attestazioni in uscita necessarie, genera un nuovo token di sicurezza \(con le attestazioni in uscita\) basato sulle informazioni nel token di sicurezza in ingresso e firma il nuovo token da restituire all'utente e, infine, per l'applicazione Web.|  
|Organizzazione partner risorse|Partner federativo rappresentato da un trust della relying party nel Servizio federativo. Il partner risorse rilascia attestazioni\-basato su token di sicurezza che contiene Web pubblicata\-basato su applicazioni che possono accedere gli utenti nel partner account.|  
  
## <a name="overview-of-ad-fs"></a>Panoramica di ADFS  
ADFS è una soluzione di accesso di identità che fornisce ai computer client \(interno o all'esterno della rete\) Internet per proteggere l'accesso SSO facile per\-rivolto verso applicazioni o servizi, anche quando gli account utente e le applicazioni si trovano in organizzazioni o reti completamente diverse.  
  
Quando un'applicazione o in servizio si trova in una rete e un account utente è in un'altra rete, all'utente vengono in genere richieste credenziali secondarie quando prova ad accedere all'applicazione o al servizio. Le credenziali secondarie rappresentano l'identità dell'utente nell'area di autenticazione in cui risiede l'applicazione o il servizio. Sono di solito richieste dal server Web che ospita l'applicazione o il servizio per poter applicare la decisione di autorizzazione appropriata.  
  
Con AD FS, le organizzazioni possono ignorare le richieste delle credenziali secondarie fornendo relazioni di trust \(relazioni di trust federative\) che le organizzazioni possono usare per distribuire un digitale identità e accesso i diritti utente a livello di attendibilità partner. In questo ambiente federativo ogni organizzazione continua a gestire le proprie identità, ma può anche distribuire e accettare in modo sicuro identità di altre organizzazioni.  
  
-   [Il ruolo degli archivi di attributi](The-Role-of-Attribute-Stores.md)  
  
-   [Il ruolo del Database di configurazione AD FS](The-Role-of-the-AD-FS-Configuration-Database.md)  
  
-   [Ruolo delle attestazioni](The-Role-of-Claims.md)  
  
-   [Il ruolo delle regole attestazioni](The-Role-of-Claim-Rules.md)  
  
-   [Il ruolo del motore di attestazioni](The-Role-of-the-Claims-Engine.md)  
  
-   [Il ruolo delle Pipeline delle attestazioni](The-Role-of-the-Claims-Pipeline.md)  
  
-   [Il ruolo del linguaggio di regola attestazione](The-Role-of-the-Claim-Rule-Language.md)  
  
-   [Determinare il tipo di modello di regola attestazione da usare](Determine-the-Type-of-Claim-Rule-Template-to-Use.md)  
  
-   [Come gli URI vengono usati in AD FS](How-URIs-Are-Used-in-AD-FS.md)  
  

