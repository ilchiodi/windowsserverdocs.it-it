---
title: Ottimizzazione di Spazi dei nomi DFS
description: Questo articolo descrive come ottimizzare gli spazi dei nomi DFS
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c11bbf65c3baebebe1e5143a5e694ca752500aca
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844632"
---
# <a name="tuning-dfs-namespaces"></a>Ottimizzazione di Spazi dei nomi DFS

> Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Dopo la creazione di uno spazio dei nomi e l'aggiunta di cartelle e destinazioni, vedere le sezioni seguenti per ottimizzare il modo DFS Namespace gestisce i riferimenti e sondaggi Active Directory Domain Services (AD DS) per i dati aggiornati dello spazio dei nomi:

-   [Attivare l'enumerazione basata sull'accesso in un Namespace](enable-access-based-enumeration-on-a-namespace.md)
-   [Abilitare o disabilitare i riferimenti e il Failback dei Client](enable-or-disable-referrals-and-client-failback.md)
-   [Modificare la quantità di tempo che i client memorizzano nella Cache dei riferimenti](change-the-amount-of-time-that-clients-cache-referrals.md)
-   [Impostare il metodo di ordinamento per le destinazioni in riferimenti](set-the-ordering-method-for-targets-in-referrals.md)
-   [Impostare la priorità a Ignora ordinamento riferimenti](set-target-priority-to-override-referral-ordering.md)
-   [Ottimizzare il Polling di Namespace](optimize-namespace-polling.md)
-   [Con l'enumerazione basata sull'accesso le autorizzazioni ereditate](using-inherited-permissions-with-access-based-enumeration.md)

> [!NOTE]
> Per cercare cartelle o destinazioni cartella, seleziona uno spazio dei nomi, fai clic sulla scheda **Ricerca**, digita la stringa di ricerca nella casella di testo, quindi fai clic su **Cerca**.