---
title: 'Elenco di controllo: Distribuire Spazi dei nomi DFS'
description: Questo articolo descrive come configurare e distribuire Spazi dei nomi DFS.
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: ab38c41c32ec88285a69fb94e62abc1453ddc3d1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402273"
---
# <a name="checklist-deploy-dfs-namespaces"></a>Elenco di controllo: Distribuire spazi dei nomi DFS

> Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Gli spazi dei nomi e i Replica DFS di file system distribuito (DFS) possono essere utilizzati per pubblicare documenti, software e dati line-of-business agli utenti in un'organizzazione. Sebbene solo Replica DFS sia sufficiente per distribuire i dati, è possibile utilizzare spazi dei nomi DFS per configurare lo spazio dei nomi in modo che una cartella sia ospitata da più server, ognuno dei quali include una copia aggiornata della cartella. In questo modo la disponibilità dei dati aumenta e il carico dei client viene distribuito tra i server.

Durante la ricerca di una cartella nello spazio dei nomi, gli utenti non sono consapevoli del fatto che la cartella è ospitata da più server. Quando un utente apre la cartella, il computer client viene indicato automaticamente a un server del sito. Se non sono disponibili server dello stesso sito, è possibile configurare lo spazio dei nomi in modo che faccia riferimento al client a un server con il minor costo di connessione definito in Active Directory Directory Services (AD DS).

Per distribuire Spazi dei nomi DFS, esegui queste attività:

-   Esaminare i concetti e i requisiti di Spazi dei nomi DFS.
[Panoramica degli spazi dei nomi DFS](dfs-overview.md)
-   [Scegliere un tipo di spazio dei nomi](choose-a-namespace-type.md)
-   [Creare uno spazio dei nomi DFS](create-a-dfs-namespace.md) 
-   Eseguire la migrazione di spazi dei nomi esistenti basati su dominio a spazi dei nomi basati su dominio alla modalità Windows Server 2008. [Eseguire la migrazione di uno spazio dei nomi basato su dominio alla modalità Windows Server 2008](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md) 
-   Aumentare la disponibilità mediante l'aggiunta di server dello spazio dei nomi a uno spazio dei nomi basato su dominio. [Aggiungere server dello spazio dei nomi a uno spazio dei nomi DFS basato su dominio](add-namespace-servers-to-a-domain-based-dfs-namespace.md)
-   Aggiungere cartelle a uno spazio dei nomi. [Creare una cartella in uno spazio dei nomi DFS](create-a-folder-in-a-dfs-namespace.md)
-   Aggiungere destinazioni cartella alle cartelle in uno spazio dei nomi. [Aggiungere destinazioni cartella](add-folder-targets.md)
-   Replicare il contenuto tra le destinazioni cartella mediante Replica DFS (facoltativo). [Replicare destinazioni di cartelle utilizzando Replica DFS](replicate-folder-targets-using-dfs-replication.md)


## <a name="see-also"></a>Vedere anche

-   [Namespaces](https://technet.microsoft.com/library/cc771914(v=ws.11).aspx)
-   [Elenco di controllo: Ottimizzare uno spazio dei nomi DFS](checklist-tune-a-dfs-namespace.md)
-   [Replica](https://technet.microsoft.com/library/cc770278(v=ws.11).aspx)


