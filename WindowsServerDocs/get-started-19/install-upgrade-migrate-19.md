---
title: Installare | Eseguire l'aggiornamento | Eseguire la migrazione a Windows Server 2019
description: Modalità di installazione pulita, eseguire l'aggiornamento o la migrazione a Windows Server 2019.
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
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810814"
---
# <a name="install--upgrade--migrate-to-windows-server-2019"></a>Installare | Eseguire l'aggiornamento | Eseguire la migrazione a Windows Server 2019

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

> [!IMPORTANT]
> Il supporto esteso per Windows Server 2008 R2 e Windows Server 2008 termina nel gennaio 2020. [Scopri le opzioni di aggiornamento](http://aka.ms/upgradecenter).

È arrivato il momento di passare a una versione più recente di Windows Server? Le opzioni per accedervi variano a seconda della versione in uso.

## <a name="clean-install"></a>Nuova installazione
Se si desidera spostare da una versione precedente di Windows Server in Windows Server 2019 sullo stesso hardware, è necessario effettuare una **installazione pulita**, in cui si installa il sistema operativo più recente direttamente su quello precedente nello stesso hardware, In questo modo l'eliminazione del sistema operativo precedente. Si tratta del modo più semplice, ma è necessario eseguire il backup dei dati innanzitutto e pianificare di reinstallare le applicazioni. Esistono alcuni aspetti da tenere in considerazione, ad esempio i requisiti di sistema, pertanto assicurarsi di controllare i dettagli per [Windows Server 2019](https://go.microsoft.com/fwlink/?linkid=2006124), [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkID=825558), [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418) , e [Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).

## <a name="in-place-upgrade"></a>Aggiornamento sul posto

Se si desidera mantenere lo stesso hardware e tutti i ruoli del server è stato configurato senza rendere flat il server, è opportuno eseguire un' **Aggiornamena sul posa**, da cui si passa da una versione meno recente una versione più recente del sistema operativo, mantenendo le impostazioni server i ruoli e i dati intatti. Ad esempio, se il server è in esecuzione Windows Server 2012 R2, è possibile aggiornarla a Windows Server 2016 o Windows Server 2019. Tuttavia, non tutti i sistemi operativi precedenti hanno un percorso alla versione più recente. Vedere il diagramma seguente per i percorsi di aggiornamento disponibili:

![Diagramma di percorsi di aggiornamento sul posto di Windows Server](media/upgrade-paths.png)

Per istruzioni dettagliate sull'aggiornamento, visitare il [centro di aggiornamento di Windows Server](http://aka.ms/upgradecenter):

[![Schermata di centro di aggiornamento di Windows Server](media/upgrade-center.png)](http://aka.ms/upgradecenter)

## <a name="cluster-os-rolling-upgrade"></a>Aggiornamento in sequenza del sistema operativo del cluster

Aggiornamento in sequenza del cluster del sistema operativo consente agli amministratori di aggiornare il sistema operativo dei nodi del cluster da Windows Server 2012 R2 e Windows Server 2016 senza interrompere i carichi di lavoro di Scale-Out File Server o Hyper-V. Questa funzionalità consente di evitare tempi di inattività che potrebbero influire sui contratti di servizio. La nuova funzionalità è illustrata in modo più dettagliato in [Aggiornamento in sequenza del sistema operativo del cluster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade).

## <a name="migration"></a>Migrazione

Migrazione di Windows Server è quando si sposta un ruolo o funzionalità alla volta da un computer di origine che esegue Windows Server a un altro computer di destinazione che esegue Windows Server, con la stessa versione o una versione più recente. A tali fini, la migrazione viene definita come lo spostamento di un ruolo o funzionalità e i relativi dati in un altro computer, senza aggiornare la funzionalità nello stesso computer. 

## <a name="license-conversion"></a>Conversione della licenza
La conversione delle licenze in alcune versioni di sistemi operativi consente di convertire una particolare edizione della versione in un'altra edizione della stessa versione con un'unica operazione, usando un semplice comando e il codice di licenza appropriato. Questo processo è definito **conversione della licenza**. Se ad esempio il server esegue Windows Server 2016 Standard, è possibile convertirlo in Windows Server 2016 Datacenter. Tenere presente che, sebbene sia possibile spostare dal Server 2016 Standard in Server 2016 Datacenter, sei non è possibile invertire il processo e passare dal Data Center al piano Standard. In alcune versioni di Windows Server, è anche possibile eseguire la conversione tra versioni OEM, con contratto multilicenza e versioni definite con lo stesso comando e il codice appropriato.


 
 
