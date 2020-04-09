---
ms.assetid: c4d83dd3-2846-4658-8b9c-93901ee69766
title: Distribuzione di server federativi
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 9e66c513c34658c40152cd7b622a1818e7198e47
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855564"
---
# <a name="deploying-federation-servers"></a>Distribuzione di server federativi

Per distribuire i server federativi in Active Directory Federation Services \(AD FS\), completare tutte le attività in [elenco di controllo: configurazione di un server federativo](Checklist--Setting-Up-a-Federation-Server.md).  
  
> [!NOTE]  
> Quando si utilizza questo elenco di controllo, prima di iniziare le procedure per la configurazione dei server, è consigliabile leggere prima i riferimenti alla pianificazione dei server federativi nella [Guida alla progettazione di ad FS in Windows server 2012](https://technet.microsoft.com/library/dd807036.aspx) . Seguendo l'elenco di controllo in questo modo, viene fornita una migliore comprensione del processo di progettazione e distribuzione per i server federativi.  
  
## <a name="about-federation-servers"></a>Informazioni sui server federativi  
I server federativi sono computer che eseguono Windows Server 2008 con il software AD FS installato che sono stati configurati per agire nel ruolo server federativo. I server federativi autenticano o instradano le richieste da account utente in altre organizzazioni e da computer client che possono trovarsi ovunque su Internet.  
  
Il fatto di installare il software AD FS in un computer e di utilizzare la configurazione guidata server federativo di AD FS per configurarlo per il ruolo server federativo rende il computer un server federativo. Consente inoltre di fare in modo che lo snap di gestione di AD FS\-sia disponibile in tale computer nel menu **avvia\\strumenti di amministrazione\\** per poter specificare quanto segue:  
  
-   Il nome host AD FS dove le organizzazioni partner e le applicazioni invieranno richieste di token e risposte  
  
-   Identificatore AD FS che le organizzazioni partner e le applicazioni utilizzeranno per identificare il nome o il percorso univoco dell'organizzazione  
  
-   Token\-certificato di firma che tutti i server federativi di un server farm utilizzeranno per emettere e firmare i token  
  
-   Percorso delle pagine Web personalizzate di ASP.NET per l'accesso client, la disconnessione e l'individuazione del partner account che miglioreranno l'esperienza client  
  
    > [!NOTE]  
    > La maggior parte di queste interfacce utente di base \(impostazioni\) dell'interfaccia utente sono contenute nel file Web. config in ogni server federativo. Il nome host AD FS e i valori dell'identificatore AD FS non sono specificati nel file Web. config.  
  
I server federativi ospitano un motore di rilascio delle attestazioni che rilascia token basati sulle credenziali \(ad esempio, il nome utente e la password\) presentati. Un token di sicurezza è un'unità dati con firma crittografica che esprime una o più attestazioni. Un'attestazione è un'istruzione che un server rende \(ad esempio nome, identità, chiave, gruppo, privilegio o funzionalità\) su un client. Una volta verificate le credenziali nel server federativo \(tramite il processo di accesso utente\), le attestazioni per l'utente vengono raccolte tramite l'analisi degli attributi utente archiviati nell'archivio attributi specificato.  
  
In single Web Federation\-Sign\-on \(SSO\) progetta \(AD FS progettazioni in cui sono incluse due o più organizzazioni\), le attestazioni possono essere modificate dalle regole attestazioni per un relying party specifico. Le attestazioni sono compilate in un token che viene inviato a un server federativo nell'organizzazione partner risorse. Dopo che un server federativo nel partner risorse riceve le attestazioni come attestazioni in ingresso, esegue il motore di rilascio delle attestazioni per eseguire un set di regole attestazioni per filtrare, passare o trasformare tali attestazioni. Le attestazioni vengono quindi compilate in un nuovo token inviato al server Web nel partner risorse.  
  
Nel progetto Web SSO \(un progetto di AD FS in cui è richiesta una sola organizzazione\), è possibile utilizzare un singolo server federativo in modo che i dipendenti possano accedere una sola volta e accedere a più applicazioni.  
  
