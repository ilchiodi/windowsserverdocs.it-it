---
title: Installare, aggiornare ed eseguire la migrazione a Windows Server 2019
description: Come eseguire l'installazione pulita, l'aggiornamento sul posto e la migrazione a Windows Server 2019.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a4e99cca754
author: coreyp-at-msft
ms.author: coreyp
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: 1fd955a640832eb161666f74b93d91bb2c3eff11
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/17/2019
ms.locfileid: "66810814"
---
# <a name="install--upgrade--migrate-to-windows-server-2019"></a>Installare, aggiornare ed eseguire la migrazione a Windows Server 2019

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

> [!IMPORTANT]
> Il supporto esteso per Windows Server 2008 R2 e Windows Server 2008 termina a gennaio 2020. [Scopri le opzioni di aggiornamento](http://aka.ms/upgradecenter).

È arrivato il momento di passare a una versione più recente di Windows Server? Le opzioni disponibili variano a seconda della versione in uso.

## <a name="clean-install"></a>Installazione pulita
Se vuoi passare da una versione meno recente di Windows Server a Windows Server 2019 usando lo stesso hardware, puoi eseguire un'**installazione pulita**, durante la quale il sistema operativo più recente viene installato direttamente sull'hardware precedente, eliminando il sistema operativo precedente. Si tratta del modo più semplice, ma devi eseguire preventivamente il backup dei dati e pianificare la reinstallazione delle applicazioni. Esistono alcuni aspetti da tenere in considerazione, ad esempio i requisiti di sistema, pertanto assicurati di controllare i dettagli per [Windows Server 2019](https://go.microsoft.com/fwlink/?linkid=2006124), [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkID=825558), [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418) e [Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).

## <a name="in-place-upgrade"></a>Aggiornamento sul posto

Se vuoi mantenere lo stesso hardware e tutti i ruoli del server che hai configurato senza rendere flat il server, puoi eseguire un **aggiornamento sul posto**, che consente di passare da una versione meno recente a una versione più recente del sistema operativo, mantenendo intatti impostazioni, ruoli del server e dati. Se ad esempio il server esegue Windows Server 2012 R2, puoi eseguire l'aggiornamento a Windows Server 2016 o Windows Server 2019. Tuttavia, non tutti i sistemi operativi precedenti consentono l'aggiornamento a ogni versione più recente. Per i percorsi di aggiornamento disponibili, vedi il diagramma seguente:

![Diagramma dei percorsi di aggiornamento sul posto di Windows Server](media/upgrade-paths.png)

Per istruzioni passo passo sull'aggiornamento, visita [Windows Server Upgrade Center](http://aka.ms/upgradecenter):

[![Screenshot di Windows Server Upgrade Center](media/upgrade-center.png)](http://aka.ms/upgradecenter)

## <a name="cluster-os-rolling-upgrade"></a>Aggiornamento in sequenza del sistema operativo del cluster

L'aggiornamento in sequenza del sistema operativo del cluster consente a un amministratore di aggiornare il sistema operativo dei nodi del cluster da Windows Server 2012 R2 a Windows Server 2016 senza arrestare i carichi di lavoro dei file server di scalabilità orizzontale o Hyper-V. Questa funzionalità consente di evitare tempi di inattività che potrebbero influire sui contratti di servizio. La nuova funzionalità è illustrata in modo più dettagliato in [Aggiornamento in sequenza del sistema operativo del cluster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade).

## <a name="migration"></a>Migrazione

La migrazione di Windows Server consiste nello spostamento di un ruolo o di una funzionalità per volta da un computer di origine che esegue Windows Server a un altro computer di destinazione che esegue la stessa versione o una versione più recente di Windows Server. A tale scopo, la migrazione viene definita come lo spostamento di un ruolo o di una funzionalità e i relativi dati in un altro computer, senza aggiornare la funzionalità nello stesso computer. 

## <a name="license-conversion"></a>Conversione della licenza
In alcune versioni del sistema operativo puoi convertire una particolare edizione della versione in un'altra edizione della stessa versione con un'unica operazione, usando un semplice comando e il codice di licenza appropriato. Questo processo è definito **conversione della licenza**. Se ad esempio il server esegue Windows Server 2016 Standard, puoi convertirlo in Windows Server 2016 Datacenter. Tieni presente che, anche se puoi passare da Server 2016 Standard a Server 2016 Datacenter, non puoi invertire il processo e passare dall'edizione Datacenter a quella Standard. In alcune versioni di Windows Server, puoi anche eseguire la conversione tra versioni OEM, con contratto multilicenza e versioni definite con lo stesso comando e il codice appropriato.


 
 
