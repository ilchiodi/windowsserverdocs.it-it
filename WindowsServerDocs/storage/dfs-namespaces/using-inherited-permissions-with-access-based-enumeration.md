---
title: Uso delle autorizzazioni ereditate con enumerazione basata sull'accesso
description: Questo articolo descrive come usare le autorizzazioni ereditate con l'enumerazione basata sull'accesso
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e8210a6abede3a8ee5317e5b6b2a90bd17013fc4
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="using-inherited-permissions-with-access-based-enumeration"></a>Uso delle autorizzazioni ereditate con enumerazione basata sull'accesso

> Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Per impostazione predefinita, le autorizzazioni usate per una cartella DFS vengono ereditate dal file system locale del server dello spazio dei nomi. Le autorizzazioni sono ereditate dalla radice dell'unità di sistema e concedono le autorizzazioni di lettura del gruppo DOMAIN\\Users. Di conseguenza, anche dopo l'abilitazione dell'enumerazione basata sull'accesso, tutte le cartelle dello spazio dei nomi rimangano visibili a tutti gli utenti del dominio.

## <a name="advantages-and-limitations-of-inherited-permissions"></a>Vantaggi e limitazioni delle autorizzazioni ereditate

L'uso di autorizzazioni ereditate per controllare quali utenti possono visualizzare le cartelle in uno spazio dei nomi DFS comporta due vantaggi principali:

-   Puoi applicare rapidamente le autorizzazioni ereditate a molte cartelle senza la necessità di usare script.
-   Puoi applicare le autorizzazioni ereditate a radici dello spazio dei nomi e a cartelle senza destinazioni.

Nonostante i vantaggi, le autorizzazioni ereditate in Spazi dei nomi DFS presentano limitazioni molti che le rendono inappropriate per la maggior parte degli ambienti:

-   Le modifiche alle autorizzazioni ereditate non vengono replicate ad altri server dello spazio dei nomi. Di conseguenza, l'uso di autorizzazioni ereditate solo su spazi dei nomi autonomi o in ambienti in cui è possibile implementare un sistema di replica di terze parti per mantenere elenchi di controllo di accesso (ACL) sincronizzati su tutti i server dello spazio dei nomi.
-   Gestione DFS e **Dfsutil** non sono in grado di visualizzare o modificare le autorizzazioni ereditate. Per gestire lo spazio dei nomi devi pertanto usare Esplora risorse oppure il comando **Icacls** oltre a Gestione DFS o **Dfsutil**.
-   Se si usano autorizzazioni ereditate, non puoi modificare le autorizzazioni di una cartella con destinazioni, a meno che non usi il comando **Dfsutil**. Spazi dei nomi DFS rimuove automaticamente le autorizzazioni da cartelle con destinazioni impostate mediante altri strumenti o metodi.
-   Se imposti autorizzazioni su una cartella con destinazioni mentre usi autorizzazioni ereditate, l'elenco di controllo di accesso impostato sulla cartella con destinazioni si combina con le autorizzazioni ereditate dall'oggetto padre della cartella nel file system. Per determinare le autorizzazioni corrette, devi esaminare entrambi i set di autorizzazioni.

> [!NOTE]
> Quando usi autorizzazioni ereditate, è più semplice impostare le autorizzazioni sulle radici dello spazio dei nomi e sulle cartelle senza destinazioni. Usa quindi le autorizzazioni ereditate sulle cartelle con destinazioni in modo che ereditino tutte le autorizzazioni dalle cartelle padre.

## <a name="using-inherited-permissions"></a>Uso delle autorizzazioni ereditate

Per limitare gli utenti che possono visualizzare una cartella DFS, devi eseguire una delle operazioni seguenti:

-   **Impostare autorizzazioni esplicite per la cartella, disabilitando l'ereditarietà.** Per impostare le autorizzazioni esplicite in una cartella con destinazioni (collegamento) tramite Gestione DFS o il comando **Dfsutil**, vedi [Attivare l'enumerazione basata sull'accesso in uno spazio dei nomi](enable-access-based-enumeration-on-a-namespace.md).
-   **Modificare le autorizzazioni ereditate nell'elemento padre nel file system locale**. Per modificare le autorizzazioni ereditate da una cartella con destinazioni, se hai già impostato le autorizzazioni esplicite per la cartella passa alle autorizzazioni ereditate da autorizzazioni esplicite, come descritto nella procedura seguente. Usa quindi Esplora risorse o il comando **Icacls** per modificare le autorizzazioni della cartella da cui la cartella con destinazioni eredita le relative autorizzazioni.

> [!NOTE]
> L'enumerazione basata sull'accesso non impedisce agli utenti di ottenere un riferimento a una destinazione cartella se conoscono già il percorso DFS della cartella con destinazioni. Le autorizzazioni impostate usando Esplora risorse o il comando **Icacls** sulle radici dello spazio dei nomi o sulle cartelle controllano se gli utenti possono accedere alla cartella DFS o alla radice dello spazio dei nomi. Tuttavia, non impediscono agli utenti di accedere direttamente a una cartella con destinazioni. Solo le autorizzazioni di condivisione o le autorizzazioni di file system NTFS della cartella condivisa possono impedire agli utenti di accedere alle destinazioni cartella.

## <a name="to-switch-from-explicit-permissions-to-inherited-permissions"></a>Per passare da autorizzazioni esplicite ad autorizzazioni ereditate

1.  Nel nodo **Spazio dei nomi** nell'albero della consolo individua la cartella con destinazioni di cui controllare la visibilità, fai clic con il pulsante destro del mouse sulla cartella, quindi scegli **Proprietà**.

2.  Fai clic sulla scheda **Avanzate**.

3.  Fai clic su **Usa autorizzazioni ereditate dal file system locale**, quindi fai clic su **OK** nella finestra di dialogo **Conferma utilizzo di autorizzazioni ereditate**. Questa operazione rimuove tutte le autorizzazioni impostate in modo esplicito sulla cartella, ripristinando le autorizzazioni NTFS ereditate dal file system locale del server dello spazio dei nomi.

4.  Per modificare le autorizzazioni ereditate per cartelle o radici dello spazio dei nomi in uno spazio dei nomi DFS, usa Esplora risorse o il comando **ICacls**.

## <a name="see-also"></a>Vedi anche

-   [Creare uno spazio dei nomi DFS](create-a-dfs-namespace.md)