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
ms.openlocfilehash: 5e2d43f75a64a6a7950539c14386c6a037bc3c45
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872642"
---
# <a name="checklist-tune-a-dfs-namespace"></a>Elenco di controllo: Ottimizzazione di uno spazio dei nomi DFS

> Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Dopo la creazione di uno spazio dei nomi e l'aggiunta di cartelle e destinazioni, attenersi alla procedura seguente per ottimizzare il modo lo spazio dei nomi DFS gestisce i riferimenti ed esegue il polling di Active Directory Domain Services (AD DS) per i dati aggiornati dello spazio dei nomi.

-   Impedire agli utenti di visualizzare le cartelle in uno spazio dei nomi per cui non dispongono delle autorizzazioni di accesso. [Attivare l'enumerazione basata sull'accesso in un Namespace](enable-access-based-enumeration-on-a-namespace.md) 
-   Consentire o impedire agli utenti di essere indicati a uno spazio dei nomi o a una destinazione cartella quando accedono a una cartella nello spazio dei nomi. [Abilitare o disabilitare i riferimenti e il Failback dei Client](enable-or-disable-referrals-and-client-failback.md) 
-   Modificare la durata di memorizzazione nella cache di un riferimento da parte dei client prima di richiederne uno nuovo. [Modificare la quantità di tempo che i client memorizzano nella Cache dei riferimenti](change-the-amount-of-time-that-clients-cache-referrals.md)
-   Ottimizzare il modo in cui i server dello spazio dei nomi eseguono il polling di AD DS per ottenere i dati dello spazio dei nomi più aggiornati. [Ottimizzare il Polling di Namespace](optimize-namespace-polling.md)
-   Usare le autorizzazioni ereditate per controllare quali utenti possono visualizzare le cartelle in uno spazio dei nomi per cui è abilitata l'enumerazione basata sull'accesso. [Con l'enumerazione basata sull'accesso le autorizzazioni ereditate](using-inherited-permissions-with-access-based-enumeration.md)

Inoltre, tramite una funzionalità di spazi dei nomi DFS avanzata nota come priorità di destinazione, è possibile specificare la priorità del server in modo che un server specifico è sempre il primo o ultimo nell'elenco dei server (noto come riferimento) che il client riceve quando accede a una cartella con destinazioni nello spazio dei nomi.

-   Specificare in quale ordine gli utenti devono essere indicati alle destinazioni cartella. [Impostare il metodo di ordinamento per le destinazioni in riferimenti](set-the-ordering-method-for-targets-in-referrals.md)
-   Eseguire l'override dell'ordinamento dei riferimenti per una destinazione cartella o uno spazio dei nomi specifico. [Impostare la priorità a Ignora ordinamento riferimenti](set-target-priority-to-override-referral-ordering.md)

## <a name="see-also"></a>Vedere anche

-   [Spazi dei nomi](https://technet.microsoft.com/library/cc771914(v=ws.11).aspx)
-   [Elenco di controllo: Distribuire gli spazi dei nomi DFS](checklist-deploy-dfs-namespaces.md)
-   [Ottimizzazione di spazi dei nomi DFS](tuning-dfs-namespaces.md)


