---
title: Ottimizzazione di Spazi dei nomi DFS
description: Questo articolo descrive come ottimizzare gli spazi dei nomi DFS
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 011512deaeb99ded7d0bfc32a48f19ab3b622475
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386148"
---
# <a name="tuning-dfs-namespaces"></a>Ottimizzazione di Spazi dei nomi DFS

> Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Dopo la creazione di uno spazio dei nomi e l'aggiunta di cartelle e destinazioni, fare riferimento alle sezioni seguenti per ottimizzare o ottimizzare il modo in cui lo spazio dei nomi DFS gestisce i riferimenti e i polling Active Directory Domain Services (AD DS) per i dati aggiornati dello spazio dei nomi:

-   [Abilitare l'enumerazione basata sull'accesso in uno spazio dei nomi](enable-access-based-enumeration-on-a-namespace.md)
-   [Abilitare o disabilitare i riferimenti e il failback del client](enable-or-disable-referrals-and-client-failback.md)
-   [Modificare la quantità di tempo per cui i client memorizzano nella cache i riferimenti](change-the-amount-of-time-that-clients-cache-referrals.md)
-   [Impostare il metodo di ordinamento delle destinazioni nei riferimenti](set-the-ordering-method-for-targets-in-referrals.md)
-   [Impostare la priorità di destinazione per eseguire l'override dell'ordinamento dei riferimenti](set-target-priority-to-override-referral-ordering.md)
-   [Ottimizzare il polling dello spazio dei nomi](optimize-namespace-polling.md)
-   [Utilizzo di autorizzazioni ereditate con enumerazione basata sull'accesso](using-inherited-permissions-with-access-based-enumeration.md)

> [!NOTE]
> Per cercare cartelle o destinazioni cartella, seleziona uno spazio dei nomi, fai clic sulla scheda **Ricerca**, digita la stringa di ricerca nella casella di testo, quindi fai clic su **Cerca**.