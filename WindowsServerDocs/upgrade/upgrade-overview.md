---
title: Panoramica sugli aggiornamenti di Windows Server | Microsoft Docs
description: Informazioni sulle informazioni generali sull'aggiornamento di Windows Server, oltre a quanto si può pensare prima di eseguire l'aggiornamento effettivo.
ms.prod: windows server
ms.technology: server-general
ms.topic: upgrade
author: RobHindman
ms.author: robhind
ms.date: 09/10/2019
ms.openlocfilehash: 6f57e52657ca3c80c92d56c54ea87e43aabd1e99
ms.sourcegitcommit: 27f0caf74e88781054250455c3c1adf06deb6234
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/19/2019
ms.locfileid: "71124791"
---
# <a name="overview-about-windows-server-upgrades"></a>Panoramica sugli aggiornamenti di Windows Server

Il processo di aggiornamento a una versione più recente di Windows Server può variare significativamente a seconda del sistema operativo che si sta iniziando e del percorso che si desidera eseguire. Vengono usati i termini seguenti per distinguere tra le diverse azioni, ognuna delle quali potrebbe essere richiesta per una nuova distribuzione di Windows Server.

- **Aggiornamento.** Noto anche come "aggiornamento sul posto". Si passa da una versione precedente del sistema operativo a una versione più recente, rimanendo nello stesso hardware fisico. **Questo è il metodo che verrà illustrata in questa sezione.**

    >[!Important]
    >Gli aggiornamenti sul posto possono anche essere supportati da società di cloud pubblico o privato; per informazioni dettagliate, è tuttavia necessario rivolgersi al provider di servizi cloud. Inoltre, non sarà possibile eseguire un aggiornamento sul posto su qualsiasi server Windows configurato per l' **avvio dal disco rigido virtuale**.

- **Installazione.** Nota anche come "installazione pulita". Si passa da una versione precedente del sistema operativo a una versione più recente, eliminando il sistema operativo precedente.

- **Migrazione.** Si passa da una versione precedente del sistema operativo a una versione più recente del sistema operativo trasferendo a un diverso set di hardware o macchina virtuale.

- **Aggiornamento in sequenza del sistema operativo del cluster.** Si aggiorna il sistema operativo dei nodi del cluster senza arrestare i carichi di lavoro Hyper-V o File server di scalabilità orizzontale. Questa funzionalità consente di evitare tempi di inattività che potrebbero influire sui contratti di servizio. Per ulteriori informazioni, vedere [aggiornamento in sequenza del sistema operativo del cluster](../failover-clustering/cluster-operating-system-rolling-upgrade.md)

- **Conversione delle licenze.** Convertire una particolare edizione della versione in un'altra edizione della stessa versione in un unico passaggio con un semplice comando e il codice di licenza appropriato. Questa operazione viene chiamata "conversione delle licenze". Se ad esempio il server esegue Windows Server 2016 Standard, è possibile convertirlo in Windows Server 2016 Datacenter.

## <a name="which-version-of-windows-server-should-i-upgrade-to"></a>Quale versione di Windows Server è necessario aggiornare a?

Si consiglia di eseguire l'aggiornamento alla versione più recente di Windows Server: Windows Server 2019. L'esecuzione della versione più recente di Windows Server consente di usare le funzionalità più recenti, incluse le funzionalità di sicurezza più recenti, e offre le migliori prestazioni.

Tuttavia, ci rendiamo conto che non è sempre possibile. È possibile usare il diagramma seguente per determinare la versione di Windows Server a cui è possibile eseguire l'aggiornamento, in base alla versione corrente:

![Percorsi di aggiornamento sul posto disponibili](media/upgrade-paths.png)

Windows Server può essere in genere aggiornato con almeno una e talvolta anche due versioni. Ad esempio, Windows Server 2012 R2 e Windows Server 2016 possono essere aggiornati sul posto a Windows Server 2019.

È anche possibile eseguire l'aggiornamento da una versione di valutazione del sistema operativo a una versione definitiva, da una versione definitiva precedente a una versione più recente oppure, in alcuni casi, da un'edizione del sistema operativo con contratto multilicenza a una normale edizione definitiva. Per ulteriori informazioni sulle opzioni di aggiornamento diverse dall'aggiornamento sul posto, vedere [Opzioni di aggiornamento e conversione per Windows Server](../get-started/supported-upgrade-paths.md).
