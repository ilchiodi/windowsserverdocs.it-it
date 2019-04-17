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
ms.openlocfilehash: ef13c338d8b13c24a02efb0468f06d5fef803cea
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="replicate-folder-targets-using-dfs-replication"></a>Replicare destinazioni cartella mediante Replica DFS

> Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

Puoi usare Replica DFS per sincronizzare il contenuto di destinazioni cartella in modo che gli utenti possano visualizzare gli stessi file indipendentemente dalla destinazione cartella indicata dal computer client.

## <a name="to-replicate-folder-targets-using-dfs-replication"></a>Per replicare destinazioni cartella mediante Replica DFS

1.  Fai clic sul pulsante **Start**, scegli **Strumenti di amministrazione**, quindi fai clic su **Gestione DFS**.

2.  Nell'albero della console fai clic con il pulsante destro del mouse su una cartella con due o più destinazioni cartella nel nodo **Spazi dei nomi**, quindi scegli **Replica cartella**.

3.  Segui le istruzioni nella Replica guidata cartella.

> [!NOTE]
> Le modifiche di configurazione non vengono applicate immediatamente a tutti i membri ad eccezione di quando si usano i cmdlet [Suspend-DfsReplicationGroup](https://technet.microsoft.com/itpro/powershell/windows/dfsr/suspend-dfsreplicationgroup) e [Sync-DfsReplicationGroup](https://technet.microsoft.com/itpro/powershell/windows/dfsr/sync-dfsreplicationgroup). La nuova configurazione deve essere replicata in tutti i controller di dominio e ogni membro del gruppo di replica deve eseguire il polling del controller di dominio più vicino per ottenere le modifiche. Il tempo richiesto dipende dalla latenza di replica di Active Directory Directory Services (ADDS) e dall'intervallo di polling lungo, ovvero 60 minuti, di ogni membro. Per eseguire immediatamente il polling delle modifiche di configurazione, apri una finestra del prompt dei comandi, quindi digita il comando seguente una volta per ogni membro del gruppo di replica: <br /> dfsrdiag.exe PollAD /Member:DOMAIN\Server1
<br />
A tale scopo, da una sessione di Windows PowerShell, usa il cmdlet [Update-DfsrConfigurationFromAD](https://technet.microsoft.com/itpro/powershell/windows/dfsr/update-dfsrconfigurationfromad), introdotto in Windows Server 2012 R2.

## <a name="see-also"></a>Vedi anche

-   [Distribuzione di Spazi dei nomi DFS](deploying-dfs-namespaces.md)
-   [Delegare le autorizzazioni di gestione per Spazi dei nomi DFS](delegate-management-permissions-for-dfs-namespaces.md)
-   [Replica](https://technet.microsoft.com/library/cc770278(v=ws.11).aspx)