---
ms.assetid: c4d83dd3-2846-4658-8b9c-93901ee69766
title: Distribuzione di server federativi
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 785dcad4ac8e03cc59730fb533e30a001569dd63
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359626"
---
# <a name="deploying-federation-servers"></a>Distribuzione di server federativi

Per distribuire i server federativi in Active Directory Federation Services \(AD FS @ no__t-1, completare tutte le attività in [Checklist: Configurazione di un server federativo @ no__t-0.  
  
> [!NOTE]  
> Quando si utilizza questo elenco di controllo, prima di iniziare le procedure per la configurazione dei server, è consigliabile leggere prima i riferimenti alla pianificazione dei server federativi nella [Guida alla progettazione di ad FS in Windows server 2012](https://technet.microsoft.com/library/dd807036.aspx) . Seguendo l'elenco di controllo in questo modo, viene fornita una migliore comprensione del processo di progettazione e distribuzione per i server federativi.  
  
## <a name="about-federation-servers"></a>Informazioni sui server federativi  
I server federativi sono computer che eseguono Windows Server 2008 con il software AD FS installato che sono stati configurati per agire nel ruolo server federativo. I server federativi autenticano o instradano le richieste da account utente in altre organizzazioni e da computer client che possono trovarsi ovunque su Internet.  
  
Il fatto di installare il software AD FS in un computer e di utilizzare la configurazione guidata server federativo di AD FS per configurarlo per il ruolo server federativo rende il computer un server federativo. Rende inoltre disponibile lo snap-in di gestione AD FS @ no__t-0cm nel computer nel menu **Start @ no__t-2Administrative Tools @ no__t-3** , in modo da poter specificare quanto segue:  
  
-   Il nome host AD FS dove le organizzazioni partner e le applicazioni invieranno richieste di token e risposte  
  
-   Identificatore AD FS che le organizzazioni partner e le applicazioni utilizzeranno per identificare il nome o il percorso univoco dell'organizzazione  
  
-   Il token @ no__t-0signing certificate che tutti i server federativi in un server farm utilizzeranno per emettere e firmare i token  
  
-   Percorso delle pagine Web personalizzate di ASP.NET per l'accesso client, la disconnessione e l'individuazione del partner account che miglioreranno l'esperienza client  
  
    > [!NOTE]  
    > La maggior parte delle impostazioni di base dell'interfaccia utente \(UI @ no__t-1 sono contenute nel file Web. config in ogni server federativo. Il nome host AD FS e i valori dell'identificatore AD FS non sono specificati nel file Web. config.  
  
I server federativi ospitano un motore di rilascio delle attestazioni che rilascia token basati sulle credenziali @no__t esempio 0per, nome utente e password @ no__t-1 presentati. Un token di sicurezza è un'unità dati con firma crittografica che esprime una o più attestazioni. Un'attestazione è un'istruzione che un server rende @no__t 0per esempio, nome, identità, chiave, gruppo, privilegio o funzionalità @ no__t-1 su un client. Una volta verificate le credenziali nel server federativo \(through il processo di accesso utente @ no__t-1, le attestazioni per l'utente vengono raccolte tramite l'analisi degli attributi utente archiviati nell'archivio attributi specificato.  
  
In federated Web single @ no__t-0Sign @ no__t-1Sulla Barra \(SSO @ no__t-3 progetta \(AD FS progetta in cui due o più organizzazioni sono incluse in @ no__t-5, le attestazioni possono essere modificate dalle regole attestazioni per un relying party specifico. Le attestazioni sono compilate in un token che viene inviato a un server federativo nell'organizzazione partner risorse. Dopo che un server federativo nel partner risorse riceve le attestazioni come attestazioni in ingresso, esegue il motore di rilascio delle attestazioni per eseguire un set di regole attestazioni per filtrare, passare o trasformare tali attestazioni. Le attestazioni vengono quindi compilate in un nuovo token inviato al server Web nel partner risorse.  
  
Nella progettazione di Web SSO \(AN AD FS progettazione in cui è partecipata una sola organizzazione @ no__t-1, è possibile utilizzare un singolo server federativo in modo che i dipendenti possano accedere una sola volta e accedere a più applicazioni.  
  
