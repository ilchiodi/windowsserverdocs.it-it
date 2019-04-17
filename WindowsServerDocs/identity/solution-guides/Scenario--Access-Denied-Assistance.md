---
ms.assetid: aae907eb-11cf-4a87-a046-8680872ed0b1
title: Scenario assistenza per accesso negato
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d6b8d8a094aa86fd5be78d2eae13bae50df3fd79
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="scenario-access-denied-assistance"></a>Scenario: Assistenza per accesso negato

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Gli utenti ricevono un messaggio di accesso negato quando tentano di accedere ai file condivisi e le cartelle in un file server per cui non dispongono di autorizzazioni. Gli amministratori spesso non hanno il contesto appropriato per risolvere il problema di accesso, che rende difficile risolvere il problema.  
  
## <a name="scenario-description"></a>Descrizione dello scenario  
Accesso negato assistenza è una nuova funzionalità di Windows Server 2012, che offre le seguenti modalità di risoluzione dei problemi relativi all'accesso a file e cartelle:  
  
-   **Assistenza automatica.** Se un utente può determinare il problema e risolverlo in modo da ottenere l'accesso richiesto, l'impatto per l'azienda è basso, e non sono necessari eccezioni speciali nei criteri di accesso centrale. Assistenza per accesso negato fornisce un messaggio di accesso negato che gli amministratori dei file server possono personalizzare con informazioni specifiche dell'organizzazione. Ad esempio, un amministratore può impostare il messaggio in modo che gli utenti possono richiedere l'accesso da un proprietario dei dati senza coinvolgere l'amministratore del file server.  
  
-   **Assistenza fornita dal proprietario dei dati.** È possibile definire una lista di distribuzione per le cartelle condivise e configurarla in modo che il proprietario riceve una notifica di posta elettronica quando un utente deve accedere. Se il proprietario dei dati non è in grado di consentire all'utente di ottenere l'accesso, il proprietario può inoltrare queste informazioni per l'amministratore del file server.  
  
-   **Assistenza fornita dall'amministratore del file server.** Questo tipo di assistenza è disponibile quando l'utente non può risolvere il problema e non consente il proprietario dei dati.  Windows Server 2012 offre un'interfaccia utente in cui gli amministratori dei file server possono visualizzare le autorizzazioni effettive per un utente su un file o una cartella in modo che sia più semplice risolvere i problemi di accesso.  
  
Assistenza per accesso negato in Windows Server 2012 offre agli amministratori di file server le informazioni sull'accesso pertinenti, in modo che sia possibile stabilire il problema e gli strumenti appropriati, in modo che è possibile apportare modifiche di configurazione per soddisfare la richiesta di accesso. Ad esempio, un utente può seguire questa procedura per accedere a un file che attualmente non hanno accesso:  
  
-   L'utente tenta di leggere un file nella cartella condivisa \\\financeshares, ma il server Visualizza un messaggio di accesso negato.  
  
-    Windows Server 2012 Visualizza le informazioni di assistenza per accesso negato all'utente con un'opzione per richiedere assistenza.  
  
-   Se l'utente richiede l'accesso alla risorsa, il server invia un messaggio di posta elettronica con le informazioni di richiesta di accesso al proprietario della cartella.  
  
È possibile trovare informazioni sulla configurazione di assistenza per accesso negato in [pianificare l'assistenza per accesso negato per](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1).  
  
È possibile trovare sulla configurazione dell'assistenza per accesso negato in [distribuzione dell'assistenza & #40; passaggi & #41;](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md).  
  
## <a name="in-this-scenario"></a>In questo scenario  
Questo scenario fa parte dello scenario controllo dinamico degli accessi. Per ulteriori informazioni su controllo dinamico degli accessi, vedere:  
  
-   [Controllo dinamico degli accessi: Panoramica dello Scenario](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="practical-applications"></a>Applicazioni pratiche  
Assistenza per accesso negato in Windows Server 2012 supporta controllo dinamico degli accessi offrendo agli utenti la possibilità di richiedere l'accesso a cartelle e file condivisi direttamente da un messaggio di accesso negato.  
  
## <a name="BKMK_NEW"></a>Funzionalità incluse in questo scenario  
Nella tabella seguente sono elencate le funzionalità che fanno parte di questo scenario e descrive la modalità di supporto.  
  
|Funzionalità|Modalità di supporto in questo scenario|  
|-----------|---------------------------------|  
|[Panoramica di gestione risorse file Server](https://technet.microsoft.com/library/hh831701.aspx)|Assistenza per accesso negato può essere configurato tramite la console di gestione risorse File Server nel file server.|  
|[Panoramica di servizi di archiviazione e file](https://technet.microsoft.com/library/hh831487.aspx)|Gestione risorse file Server è un servizio ruolo Servizi File e archiviazione è costituita da un set di funzionalità che possono essere utilizzati per amministrare i file server nella rete.|  
  


