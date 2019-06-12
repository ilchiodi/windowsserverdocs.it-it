---
title: Impostare il metodo di ordinamento delle destinazioni nei riferimenti
description: Questo articolo descrive come impostare il metodo di ordinamento per le destinazioni nei riferimenti.
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 52568944a98bed7960b37335b2e3cbbde61479ca
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447199"
---
# <a name="set-the-ordering-method-for-targets-in-referrals"></a>Impostare il metodo di ordinamento delle destinazioni nei riferimenti

> Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Un riferimento è un elenco ordinato di destinazioni che un computer client riceve da un controller di dominio o un server dello spazio dei nomi quando l'utente accede a una radice dello spazio dei nomi oppure a una cartella con destinazioni. Dopo aver ricevuto il riferimento, il client tenta di accedere alla prima destinazione dell'elenco. Se la destinazione non è disponibile, il computer client tenta di accedere alla destinazione successiva.
Le destinazioni nel sito del client vengono sempre elencate per prime in un riferimento. Le destinazioni esterne al sito del client vengono ordinate in base al metodo di ordinamento.

Usa le sezioni seguenti per specificare in quale ordine le destinazioni devono essere indicate ai client e per comprendere i diversi metodi di ordinamento dei riferimenti a destinazioni:

## <a name="to-set-the-ordering-method-for-targets-in-namespace-root-referrals"></a>Per impostare il metodo di ordinamento per le destinazioni nei riferimenti alla radice dello spazio dei nomi

Usa la procedura seguente per impostare il metodo di ordinamento nella radice dello spazio dei nomi:

1.  Fare clic sul pulsante **Start**, scegliere **Strumenti di amministrazione**, quindi fare clic su **Gestione DFS**.

2.  Nell'albero della console fare clic con il pulsante destro del mouse su uno spazio dei nomi nel nodo **Spazi dei nomi** e quindi scegliere **Proprietà**.

3.  Nella scheda **Riferimenti** seleziona un metodo di ordinamento.

> [!NOTE]
> Per usare Windows PowerShell per impostare il metodo di ordinamento per le destinazioni nei riferimenti alla radice dello spazio dei nomi, usa il cmdlet [Set-DfsnRoot](https://technet.microsoft.com/library/jj884281.aspx) con uno dei parametri seguenti:
>    -   **EnableSiteCosting** specifica il metodo di ordinamento **Costo minimo**
>    -   **EnableInsiteReferrals** specifica il metodo di ordinamento **Escludi destinazioni esterne al sito del client**
>    -   L'omissione di un parametro specifica il metodo di ordinamento **Ordine casuale** 

Il modulo DFSN Windows PowerShell è stato introdotto in Windows Server 2012.
   
## <a name="to-set-the-ordering-method-for-targets-in-folder-referrals"></a>Per impostare il metodo di ordinamento delle destinazioni nei riferimenti a cartelle

Le cartelle con destinazioni ereditano il metodo di ordinamento dalla radice dello spazio dei nomi. Puoi ignorare il metodo di ordinamento usando la procedura seguente:

1.  Fare clic sul pulsante **Start**, scegliere **Strumenti di amministrazione**, quindi fare clic su **Gestione DFS**.

2.  Nell'albero della console fare clic con il pulsante destro del mouse su una cartella con destinazioni nel nodo **Spazi dei nomi** e quindi scegliere **Proprietà**.

3.  Nella scheda **Riferimenti** seleziona la casella di controllo **Escludi destinazioni esterne al sito del client**.

> [!NOTE]
> Per usare Windows PowerShell per escludere le destinazioni cartella esterne al sito del client, usa il cmdlet [Set-DfsnFolder –EnableInsiteReferrals](https://technet.microsoft.com/library/jj884283.aspx).

## <a name="target-referral-ordering-methods"></a>Metodi di ordinamento dei riferimenti a destinazioni

I tre metodi di ordinamento sono i seguenti:

-   Ordine casuale
-   Costo minimo
-   Escludi destinazioni esterne al sito del client

## <a name="random-order"></a>Ordine casuale

In questo metodo le destinazioni vengono ordinate come segue:

1.  Nello stesso sito Active Directory Directory Services (AD DS) del client vengono ordinate in ordine casuale nella parte superiore del riferimento.
2.  Destinazioni esterne al sito del client sono elencate in ordine casuale.

Se nessun server di destinazione è disponibile nello stesso sito, il computer client viene indicato a un server di destinazione casuale indipendentemente dal costo delle connessione o dalla distanza della destinazione.

## <a name="lowest-cost"></a>Costo minimo

In questo metodo le destinazioni vengono ordinate come segue:

1.  Destinazioni nello stesso sito del client sono elencate in ordine casuale nella parte superiore del riferimento.
2.  Destinazioni esterne al sito del client sono elencate in ordine crescente di costo. Riferimenti con lo stesso costo vengono raggruppati e le destinazioni sono elencate in ordine casuale all'interno di ogni gruppo.

> [!NOTE]
> I costi di collegamento di sito non vengono visualizzati nello snap-in Gestione DFS. Per visualizzare i costi di collegamento di sito, usa lo snap-in Siti e servizi di Active Directory.

## <a name="exclude-targets-outside-of-the-clients-site"></a>Escludi destinazioni esterne al sito del client

In questo metodo il riferimento contiene solo le destinazioni presenti nello stesso sito del client. Tali destinazioni presenti nello stesso sito sono elencate in ordine casuale. Se nello stesso sito non è presente alcuna destinazione, il client non riceve un riferimento e non può accedere a tale parte dello spazio dei nomi.

> [!NOTE]
> Le destinazioni che hanno la priorità di destinazione impostata su "Prima tra tutte le destinazioni" o "Ultima tra tutte le destinazioni" sono ancora elencate nel riferimento anche se il metodo di ordinamento è impostato su **Escludi destinazioni esterne al sito del client**.

## <a name="see-also"></a>Vedere anche 

-   [Ottimizzazione di Spazi dei nomi DFS](tuning-dfs-namespaces.md)
-   [Delegare le autorizzazioni di gestione per Spazi dei nomi DFS](delegate-management-permissions-for-dfs-namespaces.md)