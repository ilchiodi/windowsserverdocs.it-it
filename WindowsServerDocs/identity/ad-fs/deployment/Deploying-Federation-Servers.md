---
ms.assetid: c4d83dd3-2846-4658-8b9c-93901ee69766
title: Distribuzione di server federativi
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 225e6b52ed46eef6f2ccacb5b0d9c6e6d880f475
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="deploying-federation-servers"></a>Distribuzione di server federativi

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Per distribuire i server federativi in Active Directory Federation Services \(AD FS\), completare tutte le operazioni in [Checklist: Setting Up a Federation Server](Checklist--Setting-Up-a-Federation-Server.md).  
  
> [!NOTE]  
> Quando si usa questo elenco di controllo, è consigliabile prima leggere i riferimenti per la pianificazione di server federativo nel [Guida alla progettazione di AD FS in Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) prima di iniziare le procedure per configurare i server. In questo modo l'elenco di controllo seguente offre una migliore comprensione del processo di progettazione e distribuzione per i server federativi.  
  
## <a name="about-federation-servers"></a>Sui server federativi  
I server federativi sono computer che esegue Windows Server 2008 con il software ADFS installato che sono stati configurati per agire nel ruolo del server federativo. I server federativi autenticare o indirizzare le richieste dagli account utente in altre organizzazioni e dai computer client che possono essere posizionate ovunque su Internet.  
  
L'atto di installazione del software ADFS in un computer e di configurazione guidata Server federativo AD FS per configurarlo per il ruolo server federativo, tale computer risulta un server federativo. Rende inoltre la gestione di ADFS snap-in disponibili in tale computer il **Start\\Administrative Tools\\** menu in modo che è possibile specificare quanto segue:  
  
-   Il nome host AD FS in cui le organizzazioni partner e applicazioni invia le richieste di token e risposte  
  
-   L'identificatore di ADFS che le applicazioni e le organizzazioni partner verrà utilizzato per identificare il nome univoco o il percorso dell'organizzazione  
  
-   Il certificato di firma di token\ che tutti i server federativi in una server farm utilizzerà per i token problema di accesso  
  
-   Il percorso del personalizzato ASP.NET pagine Web per l'accesso, disconnessione e individuazione del partner account che verrà migliorano l'esperienza client  
  
    > [!NOTE]  
    > La maggior parte di queste impostazioni di \(UI\) dei componenti di base dell'interfaccia utente sono contenute nel file Web. config in ogni server federativo. Il nome host AD FS e i valori di identificatore di AD FS non sono specificati nel file Web. config.  
  
I server federativi ospitano un motore di rilascio delle attestazioni che rilascia token in base alle credenziali \ (ad esempio nome utente e password\) che vengono presentati a esso. Un token di sicurezza è un'unità dati firmati crittograficamente che esprime una o più attestazioni. Un'attestazione è un'istruzione di un server \ (ad esempio nome, identità, chiave, gruppo, con privilegi o capability\) su un client. Dopo aver verificato le credenziali nel server federativo \ (tramite il process\ di accesso utente), vengono raccolte le attestazioni per l'utente attraverso l'esame degli attributi utente archiviati nell'archivio di attributi specificato.  
  
In \(SSO\) Single\-Sign\-On Web federata progettazioni \ (progettazioni di ADFS in cui due o più organizzazioni sono involved\), le attestazioni possono essere modificate dalle regole attestazione per una relying party specifica. Le attestazioni sono integrate in un token che viene inviato a un server federativo nell'organizzazione partner risorse. Dopo che un server federativo nel partner risorse riceve le attestazioni come attestazioni in ingresso, viene eseguito il motore di rilascio delle attestazioni per eseguire un set di regole attestazione per filtrare, pass-through o trasformare le attestazioni. Le attestazioni vengono quindi incorporate in un nuovo token che viene inviato al server Web nel partner risorse.  
  
Nella progettazione Web SSO \ (un progetto di ADFS in cui solo un'organizzazione è involved\), un server federativo singolo può essere utilizzato in modo che i dipendenti possono accedere una sola volta e comunque accedere a più applicazioni.  
  
