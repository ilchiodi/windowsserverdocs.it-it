---
title: Creare una cartella in uno spazio dei nomi DFS
description: Questo articolo descrive come creare una cartella in uno spazio dei nomi DFS
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 7389b825afe5ccae3059f50ffdedac72ecd5ac9a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402250"
---
# <a name="create-a-folder-in-a-dfs-namespace"></a>Creare una cartella in uno spazio dei nomi DFS

> Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Puoi usare cartelle per creare ulteriori livelli di gerarchia in uno spazio dei nomi. Puoi inoltre creare cartelle con destinazioni cartella per aggiungere cartelle condivise allo spazio dei nomi. Le cartelle DFS con destinazioni cartella non possono contenere altre cartelle DFS, pertanto se desideri aggiungere un livello di gerarchia allo spazio dei nomi, non aggiungere destinazioni cartella alla cartella.

Usa la procedura seguente per aggiungere una cartella in uno spazio dei nomi mediante Gestione DFS:

## <a name="to-create-a-folder-in-a-dfs-namespace"></a>Per creare una cartella in uno spazio dei nomi

1.  Fare clic sul pulsante **Start**, scegliere **Strumenti di amministrazione**, quindi fare clic su **Gestione DFS**.

2.  Nell'albero della console fai clic con il pulsante destro del mouse su uno spazio dei nomi o su una cartella nel nodo **Spazi dei nomi**, quindi scegli **Nuova cartella**.

3.  Nella casella di testo **Nome** digita il nome della nuova cartella.

4.  Per aggiungere una o più destinazioni cartella alla cartella, fai clic su **Aggiungi**, specifica il percorso UNC (Universal Naming Convention) della destinazione cartella, quindi fai clic su **OK**.


> [!TIP]
> Per creare una cartella in uno spazio dei nomi tramite Windows PowerShell, usa il cmdlet [New-DfsnFolder](https://docs.microsoft.com/powershell/module/dfsn/new-dfsnfolder). Il modulo Windows PowerShell per Spazio dei nomi DFS è stato introdotto in Windows Server 2012.


## <a name="see-also"></a>Vedere anche

-   [Distribuzione di Spazi dei nomi DFS](deploying-dfs-namespaces.md)
-   [Delegare le autorizzazioni di gestione per Spazi dei nomi DFS](delegate-management-permissions-for-dfs-namespaces.md)


