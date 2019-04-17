---
title: 'Elenco di controllo: Ottimizzare uno spazio dei nomi DFS'
description: Questo articolo descrive come ottimizzare il modo in cui lo spazio dei nomi DFS gestisce i riferimenti ed esegue il polling di AD DS per i dati aggiornati dello spazio dei nomi.
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 827484c2ffcbb2617626129011a8b510473fe669
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="checklist-tune-a-dfs-namespace"></a>Elenco di controllo: ottimizzare uno spazio dei nomi DFS

> Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Dopo la creazione di uno spazio dei nomi e l'aggiunta di cartelle e destinazioni, usa il seguente elenco di controllo per ottimizzare il modo in cui lo spazio dei nomi DFS gestisce i riferimenti ed esegue il polling di Active Directory Domain Services (AD DS) per i dati aggiornati dello spazio dei nomi.

-   Impedire agli utenti di visualizzare le cartelle in uno spazio dei nomi per cui non dispongono delle autorizzazioni di accesso. [Attivare l'enumerazione basata sull'accesso in uno spazio dei nomi](enable-access-based-enumeration-on-a-namespace.md) 
-   Consentire o impedire agli utenti di essere indicati a uno spazio dei nomi o a una destinazione cartella quando accedono a una cartella nello spazio dei nomi. [Abilitare o disabilitare i riferimenti e il failback del client](enable-or-disable-referrals-and-client-failback.md) 
-   Modificare la durata di memorizzazione nella cache di un riferimento da parte dei client prima di richiederne uno nuovo. [Modificare l'intervallo di memorizzazione nella cache dei riferimenti da parte dei client](change-the-amount-of-time-that-clients-cache-referrals.md)
-   Ottimizzare il modo in cui i server dello spazio dei nomi eseguono il polling di AD DS per ottenere i dati dello spazio dei nomi più aggiornati. [Ottimizzare il polling dello spazio dei nomi](optimize-namespace-polling.md)
-   Usare le autorizzazioni ereditate per controllare quali utenti possono visualizzare le cartelle in uno spazio dei nomi per cui è abilitata l'enumerazione basata sull'accesso. [Uso delle autorizzazioni ereditate con enumerazione basata sull'accesso](using-inherited-permissions-with-access-based-enumeration.md)

Inoltre, tramite un miglioramento di Spazi dei nomi DFS noto come priorità di destinazione, puoi specificare la priorità dei server in modo che un server specifico sia sempre il primo o l'ultimo nell'elenco di server (noto come riferimento) che il client riceve quando accede a una cartella con destinazioni nello spazio dei nomi.

-   Specificare in quale ordine gli utenti devono essere indicati alle destinazioni cartella. [Impostare il metodo di ordinamento delle destinazioni nei riferimenti](set-the-ordering-method-for-targets-in-referrals.md)
-   Eseguire l'override dell'ordinamento dei riferimenti per una destinazione cartella o uno spazio dei nomi specifico. [Impostare la priorità di destinazione per eseguire l'override dell'ordinamento dei riferimenti](set-target-priority-to-override-referral-ordering.md)

## <a name="see-also"></a>Vedi anche

-   [Spazi dei nomi](https://technet.microsoft.com/library/cc771914(v=ws.11).aspx)
-   [Elenco di controllo: Distribuire Spazi dei nomi DFS](checklist-deploy-dfs-namespaces.md)
-   [Ottimizzazione di Spazi dei nomi DFS](tuning-dfs-namespaces.md)


