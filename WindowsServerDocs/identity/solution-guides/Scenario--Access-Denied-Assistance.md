---
ms.assetid: aae907eb-11cf-4a87-a046-8680872ed0b1
title: Assistenza per accesso negato agli scenari
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: a6418f7ac317f060adb72f32e231e1577a5f8b92
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861144"
---
# <a name="scenario-access-denied-assistance"></a>Scenario: Assistenza per accesso negato

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Gli utenti ricevono un messaggio di accesso negato quando cercano di accedere a file e cartelle condivisi su un file server per cui non dispongono di autorizzazioni. Gli amministratori spesso non hanno il contesto appropriato per risolvere il problema di accesso e hanno quindi difficoltà ad intervenire.  
  
## <a name="scenario-description"></a>Descrizione scenario  
L'assistenza per accesso negato è una nuova funzionalità di Windows Server 2012, che fornisce i metodi seguenti per la risoluzione dei problemi relativi all'accesso a file e cartelle:  
  
-   **Auto-assistenza.** Se un utente è in grado di determinare il problema e risolverlo in modo da ottenere l'accesso necessario, l'impatto sulle attività aziendali è contenuto e non è necessario inserire eccezioni speciali nei criteri di accesso centrale. L'assistenza per accesso negato fornisce un messaggio di accesso negato che gli amministratori dei file server possono personalizzare con informazioni specifiche dell'organizzazione. Ad esempio, un amministratore può impostare il messaggio in modo che gli utenti possano richiedere l'accesso direttamente al proprietario dei dati senza coinvolgere l'amministratore del file server.  
  
-   **Assistenza fornita dal proprietario dei dati.** È possibile definire una lista di distribuzione per le cartelle condivise e configurarla in modo che il proprietario di una determinata cartella riceva una notifica tramite posta elettronica quando un utente ha bisogno di accedervi. Se il proprietario dei dati non sa come concedere l'accesso all'utente, può inoltrare le informazioni necessarie all'amministratore del file server.  
  
-   **Assistenza fornita dall'amministratore del file server.** Questo tipo di assistenza è disponibile quando l'utente non è in grado di risolvere il problema e il proprietario dei dati non è in grado di aiutarlo.  Windows Server 2012 fornisce un'interfaccia utente in cui gli amministratori file server possono visualizzare le autorizzazioni valide per un utente su un file o una cartella in modo da semplificare la risoluzione dei problemi di accesso.  
  
L'assistenza per accesso negato in Windows Server 2012 offre agli amministratori file server i dettagli di accesso pertinenti in modo che possano determinare il problema e gli strumenti appropriati in modo da poter apportare modifiche alla configurazione per soddisfare la richiesta di accesso. Ad esempio, l'utente può seguire questa procedura per accedere a un file al quale attualmente non ha accesso:  
  
-   L'utente tenta di leggere un file nella \\cartella condivisa \financeshares, ma il server Visualizza un messaggio di accesso negato.  
  
-    Windows Server 2012 Visualizza all'utente le informazioni di assistenza per accesso negato con un'opzione per richiedere assistenza.  
  
-   Se l'utente richiede l'accesso alla risorsa, il server invia un messaggio di posta elettronica con le informazioni sulla richiesta di accesso al proprietario della cartella.  
  
Per informazioni sulla configurazione dell'assistenza per accesso negato, vedere [Pianificare l'assistenza per accesso negato](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1).  
  
Per informazioni sulla configurazione dell'assistenza per accesso negato, vedere la [procedura &#40;&#41;di distribuzione dell'assistenza per accesso negato](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md).  
  
## <a name="in-this-scenario"></a>In questo scenario  
Questo scenario fa parte dello scenario Controllo dinamico degli accessi. Per altre informazioni su Controllo dinamico degli accessi, vedere:  
  
-   [Controllo dinamico degli accessi: Panoramica dello scenario](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="practical-applications"></a>Applicazioni pratiche  
L'assistenza per accesso negato in Windows Server 2012 contribuisce al controllo dinamico degli accessi offrendo agli utenti la possibilità di richiedere l'accesso a cartelle e file condivisi direttamente da un messaggio di accesso negato.  
  
## <a name="features-included-in-this-scenario"></a><a name="BKMK_NEW"></a>Funzionalità incluse in questo scenario  
Nella tabella che segue sono elencate le funzionalità che fanno parte di questo scenario e viene descritto in che modo lo supportano.  
  
|Caratteristica|Modalità di supporto dello scenario|  
|-----------|---------------------------------|  
|[Panoramica di Gestione risorse file server](https://technet.microsoft.com/library/hh831701.aspx)|L'assistenza per accesso negato può essere configurata mediante la console di Gestione risorse file server nel file server.|  
|[Panoramica di Servizi file e archiviazione](https://technet.microsoft.com/library/hh831487.aspx)|Gestione risorse file server è un servizio ruolo Servizi file e archiviazione e offre una serie di funzionalità che possono essere usate per amministrare i file server nella rete aziendale.|  
  


