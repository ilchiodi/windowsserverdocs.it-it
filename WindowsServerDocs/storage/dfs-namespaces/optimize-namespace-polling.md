---
title: Ottimizzare il polling dello spazio dei nomi
description: Questo articolo descrive come ottimizzare il polling dello spazio dei nomi per mantenere la coerenza di uno spazio dei nomi basato su domino tra server dello spazio dei nomi
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: be9a7623089d99a5b9c791b219dbcc64d61466cf
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="optimize-namespace-polling"></a>Ottimizzare il polling dello spazio dei nomi

> Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Per mantenere la coerenza di uno spazio dei nomi basato su dominio tra server dello spazio dei nomi, è necessario che i server eseguano periodicamente il polling ad Active Directory Domain Services (ADDS) in modo da ottenere i dati dello spazio dei nomi più recenti. 

## <a name="to-optimize-namespace-polling"></a>Per ottimizzare il polling dello spazio dei nomi

Per ottimizzare l'esecuzione del polling, esegui questa procedura:

1.  Fai clic sul pulsante **Start**, scegli **Strumenti di amministrazione**, quindi fai clic su **Gestione DFS**.

2.  Nell'albero della console fai clic con il pulsante destro del mouse su uno spazio dei nomi basato su dominio nel nodo **Spazi dei nomi**, quindi scegli **Proprietà**.

3.  Nella scheda **Avanzate** scegli se l'ottimizzazione dello spazio dei nomi deve essere eseguita ai fini della coerenza o della scalabilità.

    -   Scegli **Ottimizza per la coerenza** se il numero di server dello spazio dei nomi che ospitano lo spazio dei nomi è minore o uguale a 16.
    -   Scegli **Ottimizza per la scalabilità** se il numero di server dello spazio dei nomi è maggiore di 16. In tal modo il carico sull'emulatore del controller di dominio primario (PDC) risulterà ridotto, ma aumenterà il tempo necessario per replicare le modifiche apportate allo spazio dei nomi in tutti i server dello spazio dei nomi. Fino a quando le modifiche non vengono replicate in tutti i server, è possibile che lo spazio dei nomi visualizzato non sia coerente.

> [!NOTE]
> Per impostare la modalità di polling dello spazio dei nomi tramite Windows PowerShell, usa il cmdlet [Set-DfsnRoot EnableRootScalability](https://technet.microsoft.com/library/jj884281.aspx), introdotto in Windows Server 2012.

## <a name="see-also"></a>Vedi anche

-   [Ottimizzazione di Spazi dei nomi DFS](tuning-dfs-namespaces.md)
-   [Delegare le autorizzazioni di gestione per Spazi dei nomi DFS](delegate-management-permissions-for-dfs-namespaces.md)