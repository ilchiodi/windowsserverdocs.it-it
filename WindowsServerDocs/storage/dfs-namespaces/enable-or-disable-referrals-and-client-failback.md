---
title: Abilitare o disabilitare i riferimenti e il failback del client
description: Questo articolo descrive come abilitare o disabilitare i riferimenti e il failback del client.
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e7dd11b530c61e2536db425d3e85e0fbe458d349
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386216"
---
# <a name="enable-or-disable-referrals-and-client-failback"></a>Abilitare o disabilitare i riferimenti e il failback del client

> Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Un riferimento è un elenco ordinato di server che un computer client riceve da un controller di dominio o un server dello spazio dei nomi quando l'utente accede a una radice dello spazio dei nomi o a una cartella DFS con destinazioni. Dopo aver ricevuto il riferimento, il computer tenta di accedere al primo server dell'elenco. Se il server non è disponibile, il computer client tenta di accedere al server successivo. Se un server non è più disponibile, è possibile configurare i client in modo che eseguano il failback sul server preferito quando diventa disponibile.

Le sezioni seguenti forniscono informazioni su come abilitare o disabilitare i riferimenti o abilitare il failback del client:

## <a name="enable-or-disable-referrals"></a>Abilitazione o disabilitazione dei riferimenti

Disattivando il riferimento di un server dello spazio dei nomi o di una destinazione cartella, è possibile evitare che gli utenti vengano reindirizzati a tale server o destinazione. Tale accorgimento risulta utile se è necessario disconnettere temporaneamente un server per eseguire interventi di manutenzione.

-   Per abilitare o disabilitare i riferimenti a una destinazione di cartella, esegui questa procedura:

    1.  Nell'albero della console Gestione DFS fai clic su una cartella con destinazioni nel nodo **Spazi dei nomi**, quindi fai clic sulla scheda **Destinazioni cartella** nel riquadro **Dettagli**.
    2.  Fai clic con il pulsante destro del mouse sulla destinazione cartella, quindi scegli **Disabilita destinazione cartella** o **Abilita destinazione cartella**.

-   Per abilitare o disabilitare i riferimenti a un server dello spazio dei nomi, esegui questa procedura:

    1.  Nell'albero della console Gestione DFS seleziona lo spazio dei nomi appropriato, quindi fai clic sulla scheda **Server dello spazio dei nomi**.
    2.  Fai clic con il pulsante destro del mouse sul server dello spazio dei nomi appropriato, quindi scegli **Disabilita server dello spazio dei nomi** o **Abilita server dello spazio dei nomi**.


> [!TIP]
> Per abilitare o disabilitare i riferimenti usando Windows PowerShell, usare i cmdlet [set-DfsnRootTarget – state](https://technet.microsoft.com/library/jj884266.aspx) o [set-DfsnServerConfiguration](https://technet.microsoft.com/library/jj884277.aspx) , introdotti in Windows Server 2012.

## <a name="enable-client-failback"></a>Abilitare il failback dei client

Se una destinazione non è più disponibile, è possibile configurare i client in modo che eseguano il failback sulla destinazione dopo che quest'ultima viene ripristinata. Per il corretto funzionamento del failback, è necessario che i computer client soddisfino i requisiti elencati nell'argomento seguente: [Esaminare i requisiti del client per gli spazi dei nomi DFS](https://technet.microsoft.com/library/cc771913(v=ws.11).aspx).


> [!NOTE]
> Per abilitare il failback del client in una radice dello spazio dei nomi tramite Windows PowerShell, usa il cmdlet [Set-DfsnRoot](https://technet.microsoft.com/library/jj884281.aspx). Per abilitare il failback dei client in una cartella DFS, usa il cmdlet [Set-DfsnFolder](https://technet.microsoft.com/library/jj884283.aspx).


## <a name="to-enable-client-failback-for-a-namespace-root"></a>Per abilitare il failback dei client per la directory radice di uno spazio dei nomi

1.  Fare clic sul pulsante **Start**, scegliere **Strumenti di amministrazione**, quindi fare clic su **Gestione DFS**.

2.  Nell'albero della console fare clic con il pulsante destro del mouse su uno spazio dei nomi nel nodo **Spazi dei nomi** e quindi scegliere **Proprietà**.

3.  Nella scheda **Riferimenti** selezionare la casella di controllo **Failback dei client sulle destinazioni preferite**.

Le cartelle con destinazioni ereditano le impostazioni relative al failback dei client dalla directory radice dello spazio dei nomi. Se il failback dei client è disabilitato sulla radice dello spazio dei nomi, puoi eseguire la procedura seguente per abilitarlo su una cartella con destinazioni.

## <a name="to-enable-client-failback-for-a-folder-with-targets"></a>Per abilitare il failback dei client per una cartella con destinazioni

1.  Fare clic sul pulsante **Start**, scegliere **Strumenti di amministrazione**, quindi fare clic su **Gestione DFS**.

2.  Nell'albero della console fare clic con il pulsante destro del mouse su una cartella con destinazioni nel nodo **Spazi dei nomi** e quindi scegliere **Proprietà**.

3.  Nella scheda **Riferimenti** selezionare la casella di controllo **Failback dei client sulle destinazioni preferite**.

## <a name="see-also"></a>Vedere anche 

-   [Ottimizzazione di Spazi dei nomi DFS](tuning-dfs-namespaces.md)
-   [Esaminare i requisiti del client per spazi dei nomi DFS](https://technet.microsoft.com/library/cc771913(v=ws.11).aspx)
-   [Delegare le autorizzazioni di gestione per Spazi dei nomi DFS](delegate-management-permissions-for-dfs-namespaces.md)