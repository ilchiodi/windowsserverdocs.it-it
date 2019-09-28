---
title: Disconnessione di un server Spazi di archiviazione diretti a scopo di manutenzione
ms.prod: windows-server
ms.author: eldenc
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 10/08/2018
Keywords: Spazi di archiviazione diretti, S2D, manutenzione
ms.assetid: 73dd8f9c-dcdb-4b25-8540-1d8707e9a148
ms.localizationpriority: medium
ms.openlocfilehash: 20439a06c255a73f20a297f765e6ed11abfde6f2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402828"
---
# <a name="taking-a-storage-spaces-direct-server-offline-for-maintenance"></a>Disconnessione di un server Spazi di archiviazione diretti a scopo di manutenzione

> Si applica a: Windows Server 2019, Windows Server 2016

In questo argomento vengono fornite indicazioni su come riavviare o arrestare correttamente i server con [Spazi di archiviazione diretti](storage-spaces-direct-overview.md).

Con Spazi di archiviazione diretti, disconnettendo un server (arrestandolo) significa anche disconnettere parti di archiviazione condivise tra tutti i server del cluster. Questa operazione richiede la sospensione del server che si desidera disconnettere, spostando i ruoli ad altri server del cluster e verificando che tutti i dati siano disponibili negli altri server del cluster, in modo che i dati rimangano sicuri e accessibili per l'intera durata della manutenzione.

Utilizzare le procedure seguenti per sospendere correttamente un server in un cluster Spazi di archiviazione diretti prima di disconnetterlo. 

   > [!IMPORTANT]
   > Per installare gli aggiornamenti in un cluster Spazi di archiviazione diretti, utilizzare l'aggiornamento compatibile con cluster, che esegue automaticamente le procedure descritte in questo argomento, non rendendolo necessario quando si installano aggiornamenti. Per altre info, consultare [Aggiornamento compatibile con cluster](https://technet.microsoft.com/library/hh831694.aspx).

## <a name="verifying-its-safe-to-take-the-server-offline"></a>Verificare la sicurezza di disconnettere il server

Prima di disconnettere un server a scopo di manutenzione, verificare che tutti i volumi siano integri.

A tale scopo, aprire una sessione di PowerShell con autorizzazioni di Amministratore, quindi eseguire il comando seguente per visualizzare lo stato dei volumi:

```PowerShell
Get-VirtualDisk 
```

L'output potrebbe essere simile all'esempio seguente:
```
FriendlyName ResiliencySettingName OperationalStatus HealthStatus IsManualAttach Size
------------ --------------------- ----------------- ------------ -------------- ----
MyVolume1    Mirror                OK                Healthy      True           1 TB
MyVolume2    Mirror                OK                Healthy      True           1 TB
MyVolume3    Mirror                OK                Healthy      True           1 TB
```

Verificare che la proprietà **HealthStatus** di ciascun volume (disco virtuale) sia **Integro**.

Per eseguire questa operazione in Gestione cluster di failover, passare ad **Archiviazione** > **Dischi**.

Verificare che la colonna **Stato** di ciascun volume (disco virtuale) sia **Online**.

## <a name="pausing-and-draining-the-server"></a>Sospensione e svuotamento del server

Prima di riavviare o arrestare il server, sospendere e svuotare (rimuovere) eventuali ruoli, ad esempio macchine virtuali in esecuzione su di esso. Questo consente a Spazi di archiviazione diretti anche di scaricare ed eseguire normalmente il commit dei dati per garantire l'arresto trasparente di qualsiasi applicazione in esecuzione nel server in questione.

   > [!IMPORTANT]
   > Sospendere e svuotare sempre i server in cluster prima di riavviarli o arrestarli.

In PowerShell, eseguire il cmdlet seguente (come amministratore) per procedere con la sospensione e lo svuotamento.

```PowerShell
Suspend-ClusterNode -Drain
```

Per eseguire questa operazione in Gestione cluster di failover, passare a **Nodi**, fare clic con il tasto destro del mouse sul nodo, quindi selezionare **Sospendi** > **Svuota ruoli**.

![Sospensione/svuotamento](media/maintain-servers/pause-drain.png)

Tutte le macchine virtuali procederanno alla migrazione in tempo reale ad altri server del cluster. Questa operazione può richiedere alcuni minuti.

   > [!NOTE]
   > Quando si sospende e svuota il nodo del cluster in modo corretto, Windows esegue un controllo di protezione automatico per assicurare che sia sicuro procedere. In presenza di volumi non integri, si interromperà e si riceverà un avviso per cui non è sicuro procedere.

![Controllo di protezione](media/maintain-servers/safety-check.png)

## <a name="shutting-down-the-server"></a>Arresto del server in corso

Quando il server completa lo svuotamento, in Gestione cluster di failover e PowerShell verrà visualizzato **In pausa**.

![In pausa](media/maintain-servers/paused.png)

È ora possibile riavviarlo o arrestarlo in modo sicuro, come sempre (ad esempio, utilizzando i cmdlet PowerShell Restart-Computer o Stop-Computer).

```PowerShell
Get-VirtualDisk 

FriendlyName ResiliencySettingName OperationalStatus HealthStatus IsManualAttach Size
------------ --------------------- ----------------- ------------ -------------- ----
MyVolume1    Mirror                Incomplete        Warning      True           1 TB
MyVolume2    Mirror                Incomplete        Warning      True           1 TB
MyVolume3    Mirror                Incomplete        Warning      True           1 TB
```

Lo stato operativo incompleto o danneggiato è normale quando i nodi sono in fase di arresto o di avvio/arresto del servizio cluster in un nodo e non devono causare problemi. Tutti i volumi restano online e accessibili.

## <a name="resuming-the-server"></a>Ripresa del server

Quando il server è pronto per iniziare nuovamente l'hosting dei carichi di lavoro, riavviarlo.

In PowerShell, eseguire il cmdlet seguente (come amministratore) per la ripresa.

```PowerShell
Resume-ClusterNode
```

Per spostare nuovamente i ruoli che erano precedentemente in esecuzione su questo server, utilizzare il flag facoltativo **- Failback**.

```PowerShell
Resume-ClusterNode –Failback Immediate
```

Per eseguire questa operazione in Gestione cluster di failover, passare a **Nodi**, fare clic con il tasto destro del mouse sul nodo, quindi selezionare **Ripreni** > **Esegui failback ruoli**.

![Riprendi-Failback](media/maintain-servers/resume-failback.png)

## <a name="waiting-for-storage-to-resync"></a>In attesa della risincronizzazione dell'archiviazione

Quando il server riprende, le nuove scritture che si sono verificate durante il periodo di non disponibilità devono essere risincronizzate. Questo avviene automaticamente. Utilizzando il rilevamento delle modifiche intelligente, non è necessario analizzare o sincronizzare *tutti* i dati, ma solo le modifiche. Questo processo è limitato a ridurre l'impatto sui carichi di lavoro di produzione. A seconda di quanto tempo il server è stato in pausa e della quantità di nuovi dati scritti, potrebbero essere necessari molti minuti per il completamento.

È necessario attendere il completamento della risincronizzazione prima di disconnettere tutti gli altri server del cluster.

In PowerShell, eseguire il cmdlet seguente (come amministratore) per monitorare l'avanzamento.

```PowerShell
Get-StorageJob
```
Ecco un output di esempio che mostra i processi di risincronizzazione (ripristino):
```
Name   IsBackgroundTask ElapsedTime JobState  PercentComplete BytesProcessed BytesTotal
----   ---------------- ----------- --------  --------------- -------------- ----------
Repair True             00:06:23    Running   65              11477975040    17448304640
Repair True             00:06:40    Running   66              15987900416    23890755584
Repair True             00:06:52    Running   68              20104802841    22104819713
```

**BytesTotal** mostra l'archiviazione necessaria da risincronizzare. **PercentComplete** visualizza l'avanzamento.

   > [!WARNING]
   > Non è sicuro disconnettere un altro server fino alla fine di questi processi di ripristino.

Durante questo intervallo di tempo, i volumi continueranno a visualizzare **Avviso**, ed è normale. 

Ad esempio, se si utilizza il cmdlet `Get-VirtualDisk`, è possibile osservare il seguente output:
```
FriendlyName ResiliencySettingName OperationalStatus HealthStatus IsManualAttach Size
------------ --------------------- ----------------- ------------ -------------- ----
MyVolume1    Mirror                InService         Warning      True           1 TB
MyVolume2    Mirror                InService         Warning      True           1 TB
MyVolume3    Mirror                InService         Warning      True           1 TB
```

Al completamento dei processi, verificare che i volumi mostrino nuovamente **Integro** utilizzando il cmdlet `Get-VirtualDisk`. Ecco un output di esempio:

```
FriendlyName ResiliencySettingName OperationalStatus HealthStatus IsManualAttach Size
------------ --------------------- ----------------- ------------ -------------- ----
MyVolume1    Mirror                OK                Healthy      True           1 TB
MyVolume2    Mirror                OK                Healthy      True           1 TB
MyVolume3    Mirror                OK                Healthy      True           1 TB
```

È il momento di sospendere e riavviare gli altri server nel cluster.

## <a name="how-to-update-storage-spaces-direct-nodes-offline"></a>Come aggiornare i nodi Spazi di archiviazione diretta offline
Usare la procedura seguente per eseguire rapidamente il percorso del sistema di Spazi di archiviazione diretta. Comporta la pianificazione di una finestra di manutenzione e l'esecuzione del sistema per l'applicazione di patch. Se è necessario applicare rapidamente un aggiornamento critico alla sicurezza o se è necessario assicurarsi che l'applicazione di patch venga completata nella finestra di manutenzione, questo metodo può essere corretto. Questo processo arresta il cluster di Spazi di archiviazione diretta, lo patch e lo riporta nuovamente. Il compromesso è un tempo di inattività per le risorse ospitate.

1. Pianificare la finestra di manutenzione.
2. Portare offline i dischi virtuali.
3. Arrestare il cluster per portare offline il pool di archiviazione. Eseguire il cmdlet **Stop-cluster** oppure usare Gestione cluster di failover per arrestare il cluster.
4. Impostare il servizio cluster su **disabilitato** in Services. msc in ogni nodo. In questo modo si impedisce l'avvio del servizio cluster durante la correzione.
5. Applicare l'aggiornamento cumulativo di Windows Server ed eventuali aggiornamenti dello stack di manutenzione richiesti a tutti i nodi. (È possibile aggiornare tutti i nodi contemporaneamente, non è necessario attendere che il cluster sia inattivo).  
6. Riavviare i nodi e verificare che tutti gli elementi siano soddisfacenti.
7. Impostare di nuovo il servizio cluster su **automatico** in ogni nodo.
8. Avviare il cluster. Eseguire il cmdlet **Start-cluster** oppure usare Gestione cluster di failover. 

   Concedici pochi minuti.  Verificare che il pool di archiviazione sia integro.
9. Riportare online i dischi virtuali.
10. Monitorare lo stato dei dischi virtuali eseguendo i cmdlet **Get-volume** e **Get-VirtualDisk** .


## <a name="see-also"></a>Vedere anche

- [Panoramica di Spazi di archiviazione diretta](storage-spaces-direct-overview.md)
- [Aggiornamento compatibile con cluster](https://technet.microsoft.com/library/hh831694.aspx)
