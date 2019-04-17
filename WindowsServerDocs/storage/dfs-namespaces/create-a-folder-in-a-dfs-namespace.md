---
title: Creare una cartella in uno spazio dei nomi DFS
description: Questo articolo descrive come creare una cartella in uno spazio dei nomi DFS
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 1ca9fa0b87c6e995f3f0c38abec80fef9068df90
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="create-a-folder-in-a-dfs-namespace"></a>Creare una cartella in uno spazio dei nomi DFS

> Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Puoi usare cartelle per creare ulteriori livelli di gerarchia in uno spazio dei nomi. Puoi inoltre creare cartelle con destinazioni cartella per aggiungere cartelle condivise allo spazio dei nomi. Le cartelle DFS con destinazioni cartella non possono contenere altre cartelle DFS, pertanto se desideri aggiungere un livello di gerarchia allo spazio dei nomi, non aggiungere destinazioni cartella alla cartella.

Usa la procedura seguente per aggiungere una cartella in uno spazio dei nomi mediante Gestione DFS:

## <a name="to-create-a-folder-in-a-dfs-namespace"></a>Per creare una cartella in uno spazio dei nomi

1.  Fai clic sul pulsante **Start**, scegli **Strumenti di amministrazione**, quindi fai clic su **Gestione DFS**.

2.  Nell'albero della console fai clic con il pulsante destro del mouse su uno spazio dei nomi o su una cartella nel nodo **Spazi dei nomi**, quindi scegli **Nuova cartella**.

3.  Nella casella di testo **Nome** digita il nome della nuova cartella.

4.  Per aggiungere una o più destinazioni cartella alla cartella, fai clic su **Aggiungi**, specifica il percorso UNC (Universal Naming Convention) della destinazione cartella, quindi fai clic su **OK**.


> [!TIP]
> Per creare una cartella in uno spazio dei nomi tramite Windows PowerShell, usa il cmdlet [New-DfsnFolder](https://docs.microsoft.com/powershell/module/dfsn/new-dfsnfolder). Il modulo Windows PowerShell per Spazio dei nomi DFS è stato introdotto in Windows Server 2012.


## <a name="see-also"></a>Vedi anche

-   [Distribuzione di Spazi dei nomi DFS](deploying-dfs-namespaces.md)
-   [Delegare le autorizzazioni di gestione per Spazi dei nomi DFS](delegate-management-permissions-for-dfs-namespaces.md)


