---
ms.assetid: c4d83dd3-2846-4658-8b9c-93901ee69766
title: Distribuzione di server federativi
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 225e6b52ed46eef6f2ccacb5b0d9c6e6d880f475
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847172"
---
# <a name="deploying-federation-servers"></a>Distribuzione di server federativi

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Per distribuire i server federativi in Active Directory Federation Services \(ADFS\), completare le attività in [elenco di controllo: Configurazione di un Server federativo](Checklist--Setting-Up-a-Federation-Server.md).  
  
> [!NOTE]  
> Quando si usa questo elenco di controllo, è consigliabile leggere innanzitutto i riferimenti a federation server planning nel [Guida alla progettazione di AD FS in Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) prima di iniziare le procedure per la configurazione dei server. Seguire l'elenco di controllo in questo modo offre una migliore comprensione del processo di progettazione e distribuzione per i server federativi.  
  
## <a name="about-federation-servers"></a>Sui server federativi  
I server federativi sono computer che eseguono Windows Server 2008 con installato il software ADFS che sono state configurate in modo da agire nel ruolo server federativo. I server federativi autenticano o instradare le richieste dagli account utente in altre organizzazioni e dai computer client che possono essere posizionati ovunque su Internet.  
  
L'azione di installazione del software ADFS in un computer e tramite configurazione guidata Server federativo AD FS per la configurazione per il ruolo server federativo, tale computer risulta un server federativo. Rende inoltre lo snap di gestione di AD FS\-in disponibile in tale computer il **avviare\\strumenti di amministrazione\\**  menu in modo che è possibile specificare quanto segue:  
  
-   Il nome host di AD FS in cui le organizzazioni partner e applicazioni invia le richieste di token e le risposte  
  
-   L'identificatore di AD FS che le applicazioni e le organizzazioni partner verrà utilizzato per identificare il nome univoco o il percorso della propria organizzazione  
  
-   Il token\-certificato che verranno utilizzato da tutti i server federativi in una server farm al problema e firmare i token di firma  
  
-   Il percorso delle pagine Web ASP.NET personalizzate per client di accesso, disconnessione e l'individuazione del partner account che migliorerà l'esperienza client  
  
    > [!NOTE]  
    > La maggior parte di queste funzionalità di base dell'interfaccia utente \(dell'interfaccia utente\) le impostazioni sono contenute nel file Web. config in ogni server federativo. Il nome host di AD FS e i valori di identificatore di AD FS non sono specificati nel file Web. config.  
  
Un motore di rilascio di attestazioni che rilascia i token delle credenziali di ospitare i server federativi \(ad esempio, nome utente e password\) che vengono presentati a esso. Un token di sicurezza è un'unità dati con firma crittografata che esprime una o più attestazioni. Un'attestazione è un'istruzione che rende un server \(ad esempio, nome, identità, chiave, gruppo, con privilegi o funzionalità\) su un client. Dopo aver verificate le credenziali nel server federativo \(attraverso il processo di accesso utente\), le attestazioni dell'utente vengono raccolti attraverso l'esame degli attributi utente che vengono archiviati in archivio di attributi specificato.  
  
Nel singolo Web federata\-Sign\-sul \(SSO\) progettazioni \(ADFS progettazioni in cui sono coinvolti due o più organizzazioni\), le attestazioni possono essere modificate dalle regole attestazione per un componente specifico l'entità. Le attestazioni sono integrate in un token che viene inviato a un server federativo nell'organizzazione partner risorse. Dopo che un server federativo nel partner risorse riceve le attestazioni alle attestazioni in ingresso, esegue il motore di rilascio delle attestazioni per l'esecuzione di un set di regole attestazione per filtrare, pass-through o trasformare le attestazioni. Le attestazioni vengono quindi compilate in un nuovo token che viene inviato al server Web nel partner risorse.  
  
La progettazione di Web SSO \(una progettazione di AD FS in cui è coinvolta sola organizzazione\), un server federativo singolo può essere usato in modo che i dipendenti possono accedere una sola volta e comunque accedere a più applicazioni.  
  
