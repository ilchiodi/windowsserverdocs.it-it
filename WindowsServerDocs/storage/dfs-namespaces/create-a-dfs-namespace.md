---
title: Creare uno spazio dei nomi DFS
description: Questo articolo descrive come creare uno spazio dei nomi DFS.
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: f4d4b86dd1a105576ac4d1749213696b319ba528
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402211"
---
# <a name="create-a-dfs-namespace"></a>Creare uno spazio dei nomi DFS

> Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Per creare un nuovo spazio dei nomi, puoi usare Server Manager per creare lo spazio dei nomi quando installi il servizio ruolo Spazi dei nomi DFS. Puoi anche usare il cmdlet [New-DfsnRoot cmdlet](https://docs.microsoft.com/powershell/module/dfsn/new-dfsnroot) da una sessione di Windows PowerShell. 

Il modulo Windows PowerShell per Spazio dei nomi DFS è stato introdotto in Windows Server 2012. 

In alternativa, puoi usare la procedura seguente per creare uno spazio dei nomi dopo l'installazione del servizio ruolo.

## <a name="to-create-a-namespace"></a>Per creare uno spazio dei nomi

1.  Fare clic sul pulsante **Start**, scegliere **Strumenti di amministrazione**, quindi fare clic su **Gestione DFS**.

2.  Nell'albero della console fai clic con il pulsante destro del mouse sul nodo **Spazi dei nomi**, quindi scegli **Nuovo spazio dei nomi**.

3.  Segui le istruzioni nella **Creazione guidata nuovo spazio dei nomi**.

    Per creare uno spazio dei nomi autonomo in un cluster di failover, specifica il nome di un'istanza di file server in cluster nella pagina **Server dello spazio dei nomi** della **Creazione guidata nuovo spazio dei nomi**.

> [!IMPORTANT]
> Non tentare di creare uno spazio dei nomi basato su dominio usando la modalità Windows Server 2008, a meno che il livello di funzionalità della foresta non sia Windows Server 2003 o versione successiva. Questa operazione può generare uno spazio dei nomi per il quale non è possibile eliminare cartelle DFS, restituendo il seguente messaggio di errore: "Impossibile eliminare la cartella. Impossibile completare la funzione".

## <a name="see-also"></a>Vedere anche

-   [Distribuzione di Spazi dei nomi DFS](deploying-dfs-namespaces.md)
-   [Scegliere un tipo di spazio dei nomi](choose-a-namespace-type.md)
-   [Aggiungere server dello spazio dei nomi a uno spazio dei nomi DFS basato su dominio](add-namespace-servers-to-a-domain-based-dfs-namespace.md)
-   [Delegare le autorizzazioni di gestione per Spazi dei nomi DFS](delegate-management-permissions-for-dfs-namespaces.md).


