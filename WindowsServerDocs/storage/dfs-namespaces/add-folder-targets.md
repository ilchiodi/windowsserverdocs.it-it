---
title: Aggiungere destinazioni cartella
description: In questo argomento viene descritto come aggiungere destinazioni cartella (percorsi UNC)
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: brianlic
ms.technology: storage
ms.topic: article
author: jasongerend
ms-date: 06/05/2017
ms.openlocfilehash: 8cc61189076669d5c24244294b2f0eee2b783517
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831002"
---
# <a name="add-folder-targets"></a>Aggiungere destinazioni cartella

> Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Una destinazione cartella è il percorso UNC (Universal Naming Convention) di una cartella condivisa o un altro spazio dei nomi associato a una cartella in uno spazio dei nomi. L'aggiunta di più destinazioni cartella aumenta la disponibilità della cartella nello spazio dei nomi.

## <a name="to-add-a-folder-target"></a>Per aggiungere una destinazione cartella

Per aggiungere una destinazione cartella mediante Gestione DFS, esegui questa procedura:

1.  Fare clic sul pulsante **Start**, scegliere **Strumenti di amministrazione**, quindi fare clic su **Gestione DFS**.

2.  Nell'albero della console fai clic con il pulsante destro del mouse su una cartella nel nodo **Spazi dei nomi**, quindi scegli **Aggiungi destinazione cartella**.

3.  Digita il percorso della destinazione cartella oppure fai clic su **Sfoglia** per individuarla.

4.  Se la cartella viene replicata tramite Replica DFS, puoi specificare se aggiungere la nuova destinazione cartella al gruppo di replica.

> [!TIP]
> Per aggiungere una destinazione cartella tramite Windows PowerShell, usa il cmdlet [New-DfsnFolderTarget](https://docs.microsoft.com/powershell/module/dfsn/new-dfsnfoldertarget). Il modulo Windows PowerShell per Spazio dei nomi DFS è stato introdotto in Windows Server 2012.

> [!NOTE]
> Le cartelle possono contenere destinazioni cartella o altre cartelle DFS, ma non entrambe, allo stesso livello della gerarchia di cartelle.

## <a name="see-also"></a>Vedere anche

-   [Distribuzione di spazi dei nomi DFS](deploying-dfs-namespaces.md)
-   [Delegare le autorizzazioni di gestione di spazi dei nomi DFS](delegate-management-permissions-for-dfs-namespaces.md)
-   [Replicare le destinazioni cartella tramite la replica DFS](replicate-folder-targets-using-dfs-replication.md)