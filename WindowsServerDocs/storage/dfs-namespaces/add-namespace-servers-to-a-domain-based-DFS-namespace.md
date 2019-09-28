---
title: Aggiungere server dello spazio dei nomi a uno spazio dei nomi DFS basato su dominio
description: Questo articolo descrive come specificare i server dello spazio dei nomi aggiuntivi per ospitare uno spazio dei nomi mediante Gestione DFS.
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: fa28ec06448b67724ce8e3f29894fc70f6a6dae9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386253"
---
# <a name="add-namespace-servers-to-a-domain-based-dfs-namespace"></a>Aggiungere server dello spazio dei nomi a uno spazio dei nomi DFS basato su dominio

> Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Puoi aumentare la disponibilità di uno spazio dei nomi basati su dominio specificando i server dello spazio dei nomi aggiuntivi per ospitare lo spazio dei nomi.

## <a name="to-add-a-namespace-server-to-a-domain-based-namespace"></a>Per aggiungere un server dello spazio dei nomi a uno spazio dei nomi basato su dominio

Per aggiungere un server dello spazio dei nomi a uno spazio di nomi basato su dominio usando Gestione DFS, esegui questa procedura:

1.  Fare clic sul pulsante **Start**, scegliere **Strumenti di amministrazione**, quindi fare clic su **Gestione DFS**.

2.  Nell'albero della console fai clic con il pulsante destro del mouse su uno spazio dei nomi basato su dominio nel nodo **Spazi dei nomi**, quindi scegli **Aggiungi server dello spazio dei nomi**.

3.  Immetti il percorso di un altro server oppure fai clic su **Sfoglia** per individuare un server.

> [!NOTE]
> Questa procedura non è applicabile per spazi dei nomi autonomi in quanto supportano solo un unico server dello spazio dei nomi. Per aumentare la disponibilità di uno spazio dei nomi autonomo, specifica un cluster di failover come server dello spazio dei nomi nella Creazione guidata nuovo spazio dei nomi.


> [!TIP]
> Per aggiungere un server dello spazio dei nomi tramite Windows PowerShell, usa il cmdlet [New-DfsnRootTarget](https://docs.microsoft.com/powershell/module/dfsn/new-dfsnroottarget). Il modulo Windows PowerShell per Spazio dei nomi DFS è stato introdotto in Windows Server 2012.

## <a name="see-also"></a>Vedere anche

-   [Distribuzione di Spazi dei nomi DFS](deploying-dfs-namespaces.md)
-   [Esaminare i requisiti del server per spazi dei nomi DFS](https://technet.microsoft.com/library/cc753448(v=ws.11).aspx)
-   [Creare uno spazio dei nomi DFS](create-a-dfs-namespace.md)
-   [Delegare le autorizzazioni di gestione per Spazi dei nomi DFS](delegate-management-permissions-for-dfs-namespaces.md)

