---
title: Aggiungere server dello spazio dei nomi a uno spazio dei nomi DFS basato su dominio
description: Questo articolo descrive come specificare i server dello spazio dei nomi aggiuntivi per ospitare uno spazio dei nomi mediante Gestione DFS.
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: bb3b98e1ea687b68bbb87d0da413f9624d336370
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853332"
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
> Per aggiungere un server dello spazio dei nomi tramite Windows PowerShell, usa il cmdlet [New-DfsnRootTarget](https://docs.microsoft.com/powershell/module/dfsn/set-dfsnroottarget). Il modulo Windows PowerShell per Spazio dei nomi DFS è stato introdotto in Windows Server 2012.

## <a name="see-also"></a>Vedere anche

-   [Distribuzione di spazi dei nomi DFS](deploying-dfs-namespaces.md)
-   [Esaminare i requisiti del Server di spazi dei nomi DFS](https://technet.microsoft.com/library/cc753448(v=ws.11).aspx)
-   [Creare un Namespace DFS](create-a-dfs-namespace.md)
-   [Delegare le autorizzazioni di gestione di spazi dei nomi DFS](delegate-management-permissions-for-dfs-namespaces.md)
