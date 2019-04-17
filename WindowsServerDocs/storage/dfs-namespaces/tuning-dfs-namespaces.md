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
ms.openlocfilehash: 4614441fc54913ba5a8b547bbf1ad3e8ce7ee69b
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="tuning-dfs-namespaces"></a>Ottimizzazione di Spazi dei nomi DFS

> Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Dopo la creazione di uno spazio dei nomi e l'aggiunta di cartelle e destinazioni, usa ile sezioni seguenti per ottimizzare il modo in cui lo spazio dei nomi DFS gestisce i riferimenti ed esegue il polling di Active Directory Domain Services (AD DS) per i dati aggiornati dello spazio dei nomi:

-   [Attivare l'enumerazione basata sull'accesso in uno spazio dei nomi](enable-access-based-enumeration-on-a-namespace.md)
-   [Abilitare o disabilitare i riferimenti e il failback del client](enable-or-disable-referrals-and-client-failback.md)
-   [Modificare l'intervallo di memorizzazione nella cache dei riferimenti da parte dei client](change-the-amount-of-time-that-clients-cache-referrals.md)
-   [Impostare il metodo di ordinamento delle destinazioni nei riferimenti](set-the-ordering-method-for-targets-in-referrals.md)
-   [Impostare la priorità di destinazione per eseguire l'override dell'ordinamento dei riferimenti](set-target-priority-to-override-referral-ordering.md)
-   [Ottimizzare il polling dello spazio dei nomi](optimize-namespace-polling.md)
-   [Uso delle autorizzazioni ereditate con enumerazione basata sull'accesso](using-inherited-permissions-with-access-based-enumeration.md)

> [!NOTE]
> Per cercare cartelle o destinazioni cartella, seleziona uno spazio dei nomi, fai clic sulla scheda **Ricerca**, digita la stringa di ricerca nella casella di testo, quindi fai clic su **Cerca**.