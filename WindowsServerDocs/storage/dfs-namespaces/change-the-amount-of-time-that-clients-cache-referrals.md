---
title: Modificare l'intervallo di memorizzazione nella cache dei riferimenti da parte dei client
description: Questo articolo descrive come modificare l'intervallo di tempo di memorizzazione nella cache dei riferimenti da parte dei client
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c6fcf64dc15404ca59e3ce5552b258f782441cfb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402266"
---
# <a name="change-the-amount-of-time-that-clients-cache-referrals"></a>Modificare l'intervallo di memorizzazione nella cache dei riferimenti da parte dei client

> Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Un riferimento è un elenco ordinato di destinazioni che un computer client riceve da un controller di dominio o un server dello spazio dei nomi quando l'utente accede a una directory radice dello spazio dei nomi o a una cartella con destinazioni nello spazio dei nomi. Puoi modificare la durata di memorizzazione nella cache di un riferimento da parte dei client prima di richiederne uno nuovo.

## <a name="to-change-the-amount-of-time-that-clients-cache-namespace-root-referrals"></a>Per modificare l'intervallo di tempo di memorizzazione nella cache dei riferimenti a directory radice dello spazio dei nomi da parte dei client

1.  Fare clic sul pulsante **Start**, scegliere **Strumenti di amministrazione**, quindi fare clic su **Gestione DFS**.

2.  Nell'albero della console fare clic con il pulsante destro del mouse su uno spazio dei nomi nel nodo **Spazi dei nomi** e quindi scegliere **Proprietà**.

3.  Nella scheda **Riferimenti** digitare nella casella di testo **Durata cache (in secondi)** il tempo di memorizzazione nella cache (in secondi) dei riferimenti a directory radice dello spazio dei nomi. L'impostazione predefinita corrisponde a 300 secondi (cinque minuti).

> [!TIP]
> Per modificare la quantità di tempo di memorizzazione nella cache dei riferimenti alla radice dello spazio dei nomi tramite Windows PowerShell, usa il cmdlet [Set-DfsnRoot TimeToLiveSec](https://technet.microsoft.com/library/jj884281.aspx). Questi cmdlet sono stati introdotti in Windows Server 2012.

## <a name="to-change-the-amount-of-time-that-clients-cache-folder-referrals"></a>Per modificare l'intervallo di memorizzazione nella cache dei riferimenti a cartelle da parte dei client

1.  Fai clic sul pulsante **Start**, scegli **Strumenti di amministrazione**, quindi fai clic su **Gestione DFS**.

2.  Nell'albero della console fare clic con il pulsante destro del mouse su una cartella con destinazioni nel nodo **Spazi dei nomi** e quindi scegliere **Proprietà**.

3.  Nella scheda **Riferimenti** digitare nella casella di testo **Durata cache (in secondi)** il tempo di memorizzazione nella cache (in secondi) dei riferimenti a cartelle. L'impostazione predefinita corrisponde a 1800 secondi (30 minuti).

## <a name="see-also"></a>Vedere anche

-   [Ottimizzazione di Spazi dei nomi DFS](tuning-dfs-namespaces.md)
-   [Delegare le autorizzazioni di gestione per Spazi dei nomi DFS](delegate-management-permissions-for-dfs-namespaces.md)


