---
title: 'Elenco di controllo: Distribuire Spazi dei nomi DFS'
description: Questo articolo descrive come configurare e distribuire Spazi dei nomi DFS.
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 7f7cca6b67ff6fa8d81e88323381866315f07f33
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842122"
---
# <a name="checklist-deploy-dfs-namespaces"></a>Elenco di controllo: Distribuire gli spazi dei nomi DFS

> Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Spazi dei nomi di Distributed File System (DFS) e replica DFS è utilizzabile per pubblicare documenti, software e dati line-of-business agli utenti in tutta l'organizzazione. Anche se replica DFS da solo è sufficiente distribuire i dati, è possibile utilizzare spazi dei nomi DFS per configurare lo spazio dei nomi in modo che una cartella è ospitata da più server, ognuno dei quali contiene una copia aggiornata della cartella. In questo modo la disponibilità dei dati aumenta e il carico dei client viene distribuito tra i server.

Durante la ricerca di una cartella nello spazio dei nomi, gli utenti non sono consapevoli del fatto che la cartella è ospitata da più server. Quando un utente apre la cartella, il computer client viene indicato automaticamente a un server del sito. Se nessun server nello stesso sito è disponibile, è possibile configurare lo spazio dei nomi per il client faccia riferimento a un server con la connessione più bassa costo base a quanto definito in Active Directory Directory Services (AD DS).

Per distribuire Spazi dei nomi DFS, esegui queste attività:

-   Esaminare i concetti e i requisiti di Spazi dei nomi DFS.
[Panoramica di spazi dei nomi DFS](dfs-overview.md)
-   [Scegliere un tipo di spazio dei nomi](choose-a-namespace-type.md)
-   [Creare uno spazio dei nomi DFS](create-a-dfs-namespace.md) 
-   Eseguire la migrazione di spazi dei nomi esistenti basati su dominio a spazi dei nomi basati su dominio alla modalità Windows Server 2008. [Eseguire la migrazione di un Namespace basato su dominio in modalità Windows Server 2008](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md) 
-   Aumentare la disponibilità mediante l'aggiunta di server dello spazio dei nomi a uno spazio dei nomi basato su dominio. [Aggiungere server Namespace per un Namespace DFS basato su dominio](add-namespace-servers-to-a-domain-based-dfs-namespace.md)
-   Aggiungere cartelle a uno spazio dei nomi. [Creare una cartella in un Namespace DFS](create-a-folder-in-a-dfs-namespace.md)
-   Aggiungere destinazioni cartella alle cartelle in uno spazio dei nomi. [Aggiungere le destinazioni cartella](add-folder-targets.md)
-   Replicare il contenuto tra le destinazioni cartella mediante Replica DFS (facoltativo). [Replicare le destinazioni cartella tramite la replica DFS](replicate-folder-targets-using-dfs-replication.md)


## <a name="see-also"></a>Vedere anche

-   [Spazi dei nomi](https://technet.microsoft.com/library/cc771914(v=ws.11).aspx)
-   [Elenco di controllo: Ottimizzare un Namespace DFS](checklist-tune-a-dfs-namespace.md)
-   [Replica](https://technet.microsoft.com/library/cc770278(v=ws.11).aspx)


