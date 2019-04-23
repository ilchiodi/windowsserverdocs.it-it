---
title: Replicare destinazioni cartella mediante Replica DFS
description: Questo articolo descrive come replicare destinazioni cartella mediante Replica DFS
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 098ffd1a47891d2355742682a002f80dcfc59650
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878082"
---
# <a name="replicate-folder-targets-using-dfs-replication"></a>Replicare destinazioni cartella mediante Replica DFS

> Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

Puoi usare Replica DFS per sincronizzare il contenuto di destinazioni cartella in modo che gli utenti possano visualizzare gli stessi file indipendentemente dalla destinazione cartella indicata dal computer client.

## <a name="to-replicate-folder-targets-using-dfs-replication"></a>Per replicare destinazioni cartella mediante Replica DFS

1.  Fare clic sul pulsante **Start**, scegliere **Strumenti di amministrazione**, quindi fare clic su **Gestione DFS**.

2.  Nell'albero della console fai clic con il pulsante destro del mouse su una cartella con due o più destinazioni cartella nel nodo **Spazi dei nomi**, quindi scegli **Replica cartella**.

3.  Segui le istruzioni nella Replica guidata cartella.

> [!NOTE]
> Le modifiche di configurazione non vengono applicate immediatamente a tutti i membri ad eccezione di quando si usano i cmdlet [Suspend-DfsReplicationGroup](https://technet.microsoft.com/itpro/powershell/windows/dfsr/suspend-dfsreplicationgroup) e [Sync-DfsReplicationGroup](https://technet.microsoft.com/itpro/powershell/windows/dfsr/sync-dfsreplicationgroup). La nuova configurazione deve essere replicata in tutti i controller di dominio e ogni membro del gruppo di replica deve eseguire il polling del controller di dominio più vicino per ottenere le modifiche. La quantità di tempo necessaria dipende dalla latenza di replica di Active Directory Directory Services (AD DS) e long intervallo di polling (60 minuti) in ogni membro. Per eseguire immediatamente il polling delle modifiche di configurazione, apri una finestra del prompt dei comandi, quindi digita il comando seguente una volta per ogni membro del gruppo di replica: <br /> dfsrdiag.exe PollAD /Member:DOMAIN\Server1
<br />
A questo scopo da una sessione di Windows PowerShell, usare il [Update-DfsrConfigurationFromAD](https://technet.microsoft.com/itpro/powershell/windows/dfsr/update-dfsrconfigurationfromad) cmdlet, che è stata introdotta in Windows Server 2012 R2.

## <a name="see-also"></a>Vedere anche

-   [Distribuzione di spazi dei nomi DFS](deploying-dfs-namespaces.md)
-   [Delegare le autorizzazioni di gestione di spazi dei nomi DFS](delegate-management-permissions-for-dfs-namespaces.md)
-   [Replica DFS](../dfs-replication/dfsr-overview.md)