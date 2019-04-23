---
title: Creare uno spazio dei nomi DFS
description: Questo articolo descrive come creare uno spazio dei nomi DFS.
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 4256e124e75be72f94cbd35c182edfe38e92bc90
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847502"
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
> Non tentare di creare un basato su dominio dello spazio dei nomi usando la modalità di Windows Server 2008, a meno che il livello funzionale della foresta sia Windows Server 2003 o versione successiva. In questo modo può comportare uno spazio dei nomi per cui non è possibile eliminare cartelle DFS, generando il messaggio di errore seguente: "Impossibile eliminare la cartella. Impossibile completare la funzione".

## <a name="see-also"></a>Vedere anche

-   [Distribuzione di spazi dei nomi DFS](deploying-dfs-namespaces.md)
-   [Scegliere un tipo di Namespace](choose-a-namespace-type.md)
-   [Aggiungere server Namespace per un Namespace DFS basato su dominio](add-namespace-servers-to-a-domain-based-dfs-namespace.md)
-   [Delegare le autorizzazioni di gestione per Spazi dei nomi DFS](delegate-management-permissions-for-dfs-namespaces.md).


