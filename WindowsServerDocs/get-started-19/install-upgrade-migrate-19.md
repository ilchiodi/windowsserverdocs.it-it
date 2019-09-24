---
title: Eseguire l'installazione, l'aggiornamento e la migrazione a Windows Server 2019
description: Come eseguire l'installazione pulita, l'aggiornamento sul posto e la migrazione a Windows Server
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a4e99cca754
author: jasongerend
ms.author: jgerend
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: 1c0c6ca10e7ebac16d81fe1393e471a7878fd0ca
ms.sourcegitcommit: ccec91c1d32a978159f9b8bb5e39ead5805c26c4
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/19/2019
ms.locfileid: "71143755"
---
# <a name="install-upgrade-or-migrate-to-windows-server"></a>Eseguire l'installazione, l'aggiornamento e la migrazione a Windows Server

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

> [!IMPORTANT]
> Il supporto esteso per Windows Server 2008 R2 e Windows Server 2008 termina a gennaio 2020. [Scopri le opzioni di aggiornamento](http://aka.ms/upgradecenter). Per scaricare Windows Server 2019, vedere [versioni di valutazione di Windows Server](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019).

È arrivato il momento di passare a una versione più recente di Windows Server? Le opzioni disponibili variano a seconda della versione in uso.

## <a name="clean-install"></a>Installazione pulita

Il modo più semplice per installare Windows Server consiste nell'eseguire un'installazione pulita, in cui è possibile installare in un server vuoto o sovrascrivere un sistema operativo esistente. Si tratta del metodo più semplice, ma prima di tutto devi eseguire il backup dei dati e pianificare la reinstallazione delle applicazioni. Esistono alcuni aspetti da tenere in considerazione, ad esempio i requisiti di sistema, pertanto assicurati di controllare i dettagli per [Windows Server 2019](https://go.microsoft.com/fwlink/?linkid=2006124), [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkID=825558), [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418) e [Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).

## <a name="in-place-upgrade"></a>Aggiornamento sul posto

Se vuoi mantenere lo stesso hardware e tutti i ruoli del server che hai configurato senza rendere flat il server, puoi eseguire un **aggiornamento sul posto**, che consente di passare da una versione meno recente a una versione più recente del sistema operativo, mantenendo intatti impostazioni, ruoli del server e dati. Se ad esempio il server esegue Windows Server 2012 R2, puoi eseguire l'aggiornamento a Windows Server 2016 o Windows Server 2019. Tuttavia, non tutti i sistemi operativi precedenti consentono l'aggiornamento a ogni versione più recente. 

Per istruzioni dettagliate sull'aggiornamento, vedi il [contenuto sugli aggiornamenti di Windows Server](../upgrade/upgrade-overview.md).

## <a name="cluster-os-rolling-upgrade"></a>Aggiornamento in sequenza del sistema operativo del cluster

L'aggiornamento in sequenza del sistema operativo del cluster consente a un amministratore di aggiornare il sistema operativo dei nodi del cluster da Windows Server 2012 R2 a Windows Server 2016 senza arrestare i carichi di lavoro dei file server di scalabilità orizzontale o Hyper-V. Questa funzionalità consente di evitare tempi di inattività che potrebbero influire sui contratti di servizio. La nuova funzionalità è illustrata in modo più dettagliato in [Aggiornamento in sequenza del sistema operativo del cluster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade).

## <a name="migration"></a>Migrazione

La migrazione di Windows Server consiste nello spostamento di un ruolo o di una funzionalità per volta da un computer di origine che esegue Windows Server a un altro computer di destinazione che esegue la stessa versione o una versione più recente di Windows Server. A questo scopo, la migrazione viene definita come spostamento di un ruolo o di una funzionalità e dei rispettivi dati in un altro computer, non come aggiornamento della funzionalità nello stesso computer. 

## <a name="license-conversion"></a>Conversione della licenza

In alcune versioni del sistema operativo puoi convertire una determinata edizione della versione in un'altra edizione della stessa versione con un'unica operazione, usando un semplice comando e il codice di licenza appropriato. Questo processo è definito **conversione della licenza**. Se ad esempio il server esegue Windows Server 2016 Standard, puoi convertirlo in Windows Server 2016 Datacenter. Tieni presente che, anche se puoi passare da Server 2016 Standard a Server 2016 Datacenter, non puoi invertire il processo e passare dall'edizione Datacenter a quella Standard. In alcune versioni di Windows Server, puoi anche eseguire la conversione tra versioni OEM, con contratto multilicenza e versioni definite con lo stesso comando e il codice appropriato.