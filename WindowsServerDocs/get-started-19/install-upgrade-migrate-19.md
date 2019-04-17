---
title: Installare | Eseguire l'aggiornamento | Eseguire la migrazione a Windows Server 2019
description: Modalità di installazione pulita, aggiornamento sul posto o eseguire la migrazione a Windows Server 2019.
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
ms.openlocfilehash: 58c363fc0a1e336519bc6ec4276651345cc2b5eb
ms.sourcegitcommit: 07ac08dea2b8f2763c2614a999dc7967018aa0b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/07/2018
ms.locfileid: "6121390"
---
# Installare | Eseguire l'aggiornamento | Eseguire la migrazione a Windows Server 2019

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

> [!IMPORTANT]
> Il supporto esteso per Windows Server 2008 R2 e Windows Server 2008 termina nel gennaio 2020. [Informazioni sulle opzioni di aggiornamento](http://aka.ms/upgradecenter).

È arrivato il momento di passare a una versione più recente di Windows Server? Le opzioni per accedervi variano a seconda della versione in uso.

## Nuova installazione
Se si desidera passare da una versione precedente di Windows Server per Windows Server 2019 nello stesso hardware, eseguire **un'installazione pulita**, in cui installare solo il sistema operativo più recente direttamente sul precedente nello stesso hardware, pertanto l'eliminazione di sistema operativo precedente. Si tratta del modo più semplice, ma è necessario eseguire il backup dei dati innanzitutto e pianificare di reinstallare le applicazioni. Esistono alcuni aspetti da tenere in considerazione, ad esempio i requisiti di sistema, quindi assicurati di controllare i dettagli per [Windows Server 2019](https://go.microsoft.com/fwlink/?linkid=2006124), [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkID=825558), [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418)e [Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).

## Aggiornamento sul posto
Se vuoi mantenere lo stesso hardware e tutti i ruoli del server che hai configurato senza appiattire il server, è preferibile eseguire un **aggiornamento sul posto**, da cui si passa da un sistema operativo precedente a una versione più recente, mantenendo le impostazioni, ruoli del server e i dati invariati. Ad esempio, se il server esegue Windows Server 2012 R2, è possibile aggiornarla a Windows Server 2016 o Windows Server 2019. Tuttavia, non tutti i sistemi operativi precedenti hanno un percorso alla versione più recente. Vedi il diagramma seguente per i percorsi di aggiornamento disponibili:

![Diagramma di percorsi di aggiornamento sul posto di Windows Server](media/upgrade-paths.png)

Per indicazioni dettagliate sull'aggiornamento, visita il [Centro di aggiornamento di Windows Server](http://aka.ms/upgradecenter):

<a href="http://aka.ms/upgradecenter"><img src="media/upgrade-center.png" alt="Screenshot of Windows Upgrade Center" title="Il centro di aggiornamento di Windows Server"></a>

## Aggiornamento in sequenza del sistema operativo del cluster
Aggiornamento in sequenza del cluster del sistema operativo consente all'amministratore di aggiornare il sistema operativo dei nodi del cluster da Windows Server 2012 R2 e Windows Server 2016 senza interrompere i carichi di lavoro File Server di scalabilità orizzontale o Hyper-V. Questa funzionalità consente di evitare tempi di inattività che potrebbero influire sui contratti di servizio. La nuova funzionalità è illustrata in modo più dettagliato in [Aggiornamento in sequenza del sistema operativo del cluster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade).

## Migrazione

Migrazione di Windows Server è quando si sposta un ruolo o funzionalità alla volta da un computer di origine che esegue Windows Server in un altro computer di destinazione che esegue Windows Server, lo stesso o una versione più recente. A tali fini, la migrazione viene definita come lo spostamento di un ruolo o funzionalità e i relativi dati in un altro computer, senza aggiornare la funzionalità nello stesso computer. 

## Conversione della licenza
La conversione delle licenze in alcune versioni di sistemi operativi consente di convertire una particolare edizione della versione in un'altra edizione della stessa versione con un'unica operazione, usando un semplice comando e il codice di licenza appropriato. Questo processo è definito **conversione della licenza**. Se ad esempio il server esegue Windows Server 2016 Standard, è possibile convertirlo in Windows Server 2016 Datacenter. In alcune versioni di Windows Server, è anche possibile eseguire la conversione tra versioni OEM, con contratto multilicenza e versioni definite con lo stesso comando e il codice appropriato.


 
 
