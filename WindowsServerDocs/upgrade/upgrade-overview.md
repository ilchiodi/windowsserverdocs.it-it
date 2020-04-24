---
title: Panoramica sugli aggiornamenti di Windows Server | Microsoft Docs
description: Informazioni generali sull'aggiornamento di Windows Server e indicazioni su cosa tenere in considerazione prima di eseguire l'aggiornamento.
ms.prod: windows-server
ms.technology: server-general
ms.topic: upgrade
author: RobHindman
ms.author: robhind
ms.date: 09/10/2019
ms.openlocfilehash: 1ac4cbe8b9bda4ac2de2c7ad7ec27b1534c0de72
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80854234"
---
# <a name="overview-about-windows-server-upgrades"></a>Panoramica sugli aggiornamenti di Windows Server

Il processo di aggiornamento a una nuova versione di Windows Server può variare notevolmente a seconda del sistema operativo di partenza e del percorso scelto. Per distinguere le diverse azioni, ognuna delle quali potrebbe essere richiesta per una nuova distribuzione di Windows Server, usiamo i termini seguenti.

- **Aggiornamento.** Noto anche come "aggiornamento sul posto". Passi da una versione precedente del sistema operativo a una versione più recente, rimanendo nello stesso hardware fisico. **Questo è il metodo che illustreremo in questa sezione.**

    >[!Important]
    >Gli aggiornamenti sul posto possono anche essere supportati da società di cloud pubblico o privato. Per informazioni dettagliate, rivolgiti al provider di servizi cloud. Inoltre, non potrai eseguire un aggiornamento sul posto su qualsiasi server Windows configurato per **l'avvio da VHD**.

- **Installazione.** Nota anche come "installazione pulita". Passi da una versione precedente del sistema operativo a una versione più recente, eliminando il sistema operativo precedente.

- **Migrazione.** Passi da una versione precedente del sistema operativo a una versione più recente, mediante il trasferimento a un diverso set di hardware o macchina virtuale.

- **Aggiornamento in sequenza del sistema operativo del cluster.** Aggiorni il sistema operativo dei nodi del cluster senza interrompere i carichi di lavoro di File server di scalabilità orizzontale o Hyper-V. Questa funzionalità consente di evitare tempi di inattività che potrebbero influire sui contratti di servizio. Per altre informazioni, vedi [Aggiornamento in sequenza del sistema operativo del cluster](../failover-clustering/cluster-operating-system-rolling-upgrade.md).

- **Conversione della licenza.** Converti una determinata edizione della versione in un'altra edizione della stessa versione con un'unica operazione, usando un semplice comando e il codice di licenza appropriato. Questo processo è definito come "conversione delle licenze". Ad esempio, se il server esegue Windows Server 2016 Standard, puoi convertirlo in Windows Server 2016 Datacenter.

## <a name="which-version-of-windows-server-should-i-upgrade-to"></a>A quale versione di Windows Server dovrei eseguire l'aggiornamento?

Consigliamo di eseguire l'aggiornamento alla versione più recente di Windows Server: Windows Server 2019. L'esecuzione della versione più recente di Windows Server consente di usare le funzionalità più recenti, incluse le funzionalità di sicurezza più recenti, e offre le migliori prestazioni.

Tuttavia, ci rendiamo conto che non è sempre possibile. Puoi usare il diagramma seguente per determinare la versione di Windows Server a cui puoi eseguire l'aggiornamento, in base alla versione in uso:

![Percorsi di aggiornamento sul posto disponibili](media/upgrade-paths.png)

Windows Server può essere in genere aggiornato ad almeno una, e a volte due, versioni. Ad esempio, Windows Server 2012 R2 e Windows Server 2016 possono essere entrambi aggiornati sul posto a Windows Server 2019.

Puoi anche effettuare l'aggiornamento da una versione di valutazione del sistema operativo a una versione definitiva, da una versione definitiva precedente a una più recente oppure, in alcuni casi, da un'edizione del sistema operativo con contratto multilicenza a una normale edizione definitiva. Per altre informazioni su opzioni di aggiornamento diverse dall'aggiornamento sul posto, vedi [Opzioni di aggiornamento e conversione per Windows Server](../get-started/supported-upgrade-paths.md).
""'