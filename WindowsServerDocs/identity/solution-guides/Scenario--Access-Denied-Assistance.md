---
ms.assetid: aae907eb-11cf-4a87-a046-8680872ed0b1
title: Scenario assistenza per accesso negato
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d6b8d8a094aa86fd5be78d2eae13bae50df3fd79
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829092"
---
# <a name="scenario-access-denied-assistance"></a>Scenario: Assistenza per accesso negato

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Gli utenti ricevono un messaggio di accesso negato quando cercano di accedere a file e cartelle condivisi su un file server per cui non dispongono di autorizzazioni. Gli amministratori spesso non hanno il contesto appropriato per risolvere il problema di accesso e hanno quindi difficoltà ad intervenire.  
  
## <a name="scenario-description"></a>Descrizione dello scenario  
Accesso negato assistenza è una nuova funzionalità di Windows Server 2012, che offre le modalità seguenti per risolvere i problemi relativi all'accesso a file e cartelle:  
  
-   **Auto-assistenza.** Se un utente è in grado di determinare il problema e risolverlo in modo da ottenere l'accesso necessario, l'impatto sulle attività aziendali è contenuto e non è necessario inserire eccezioni speciali nei criteri di accesso centrale. L'assistenza per accesso negato fornisce un messaggio di accesso negato che gli amministratori dei file server possono personalizzare con informazioni specifiche dell'organizzazione. Ad esempio, un amministratore può impostare il messaggio in modo che gli utenti possano richiedere l'accesso direttamente al proprietario dei dati senza coinvolgere l'amministratore del file server.  
  
-   **Assistenza fornita dal proprietario dei dati.** È possibile definire una lista di distribuzione per le cartelle condivise e configurarla in modo che il proprietario di una determinata cartella riceva una notifica tramite posta elettronica quando un utente ha bisogno di accedervi. Se il proprietario dei dati non sa come concedere l'accesso all'utente, può inoltrare le informazioni necessarie all'amministratore del file server.  
  
-   **Assistenza fornita dall'amministratore del file server.** Questo tipo di assistenza è disponibile quando l'utente non è in grado di risolvere il problema e il proprietario dei dati non è in grado di aiutarlo.  Windows Server 2012 fornisce un'interfaccia utente in cui gli amministratori dei file server possono visualizzare le autorizzazioni valide per un utente su un file o cartella in modo che risulti più semplice risolvere i problemi di accesso.  
  
Assistenza per accesso negato in Windows Server 2012 offre agli amministratori dei file server le informazioni sull'accesso pertinenti in modo che sia possibile stabilire il problema e gli strumenti appropriati, in modo che vengano apportate modifiche alla configurazione per soddisfare la richiesta di accesso. Ad esempio, l'utente può seguire questa procedura per accedere a un file al quale attualmente non ha accesso:  
  
-   L'utente tenta di leggere un file nei \\\financeshares una cartella condivisa, ma il server viene visualizzato un messaggio di accesso negato.  
  
-    Windows Server 2012 consente di visualizzare le informazioni di assistenza per accesso negato all'utente con un'opzione per richiedere assistenza.  
  
-   Se l'utente richiede l'accesso alla risorsa, il server invia un messaggio di posta elettronica con le informazioni sulla richiesta di accesso al proprietario della cartella.  
  
Per informazioni sulla pianificazione della configurazione dell'assistenza per accesso negato, vedere [Plan for Access-Denied Assistance](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1).  
  
È possibile trovare informazioni sulla configurazione di access-denied assistance [assistenza del &#40;passaggi&#41;](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md).  
  
## <a name="in-this-scenario"></a>In questo scenario  
Questo scenario fa parte dello scenario Controllo dinamico degli accessi. Per altre informazioni su Controllo dinamico degli accessi, vedere:  
  
-   [Controllo dinamico degli accessi: Panoramica dello scenario](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="practical-applications"></a>Applicazioni pratiche  
Assistenza per accesso negato in Windows Server 2012 supporta controllo dinamico degli accessi offrendo utenti la possibilità di richiedere l'accesso a cartelle e file condivisi direttamente da un messaggio di accesso negato.  
  
## <a name="BKMK_NEW"></a>Funzionalità incluse in questo scenario  
Nella tabella che segue sono elencate le funzionalità che fanno parte di questo scenario e viene descritto in che modo lo supportano.  
  
|Funzionalità|Modalità di supporto dello scenario|  
|-----------|---------------------------------|  
|[Panoramica di gestione risorse file Server](https://technet.microsoft.com/library/hh831701.aspx)|L'assistenza per accesso negato può essere configurata mediante la console di Gestione risorse file server nel file server.|  
|[Panoramica di servizi di archiviazione e file](https://technet.microsoft.com/library/hh831487.aspx)|Gestione risorse file server è un servizio ruolo Servizi file e archiviazione e offre una serie di funzionalità che possono essere usate per amministrare i file server nella rete aziendale.|  
  


