---
title: Disconnessione di un server Spazi di archiviazione diretti a scopo di manutenzione
ms.prod: windows-server-threshold
ms.author: eldenc
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 10/08/2018
Keywords: Storage Spaces Direct, S2D, maintenance
ms.assetid: 73dd8f9c-dcdb-4b25-8540-1d8707e9a148
ms.localizationpriority: medium
ms.openlocfilehash: 96ae0ad0d1def12ab68466f0a9ae60d0afcc2c17
ms.sourcegitcommit: e73fbe1046a8bd2bf4f24ccffc11465ad8dfab1d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/07/2019
ms.locfileid: "8992524"
---
# Disconnessione di un server Spazi di archiviazione diretti a scopo di manutenzione

> Si applica a: Windows Server 2019, Windows Server 2016

In questo argomento vengono fornite indicazioni su come riavviare o arrestare correttamente i server con [Spazi di archiviazione diretti](storage-spaces-direct-overview.md).

Con Spazi di archiviazione diretti, disconnettendo un server (arrestandolo) significa anche disconnettere parti di archiviazione condivise tra tutti i server del cluster. Questa operazione richiede la sospensione del server che si desidera disconnettere, spostando i ruoli ad altri server del cluster e verificando che tutti i dati siano disponibili negli altri server del cluster, in modo che i dati rimangano sicuri e accessibili per l'intera durata della manutenzione.

Utilizzare le procedure seguenti per sospendere correttamente un server in un cluster Spazi di archiviazione diretti prima di disconnetterlo. 

   > [!IMPORTANT]
   > Per installare gli aggiornamenti in un cluster Spazi di archiviazione diretti, utilizzare l'aggiornamento compatibile con cluster, che esegue automaticamente le procedure descritte in questo argomento, non rendendolo necessario quando si installano aggiornamenti. Per altre info, consultare [Aggiornamento compatibile con cluster](https://technet.microsoft.com/library/hh831694.aspx).

## Verificare la sicurezza di disconnettere il server

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

## Sospensione e svuotamento del server

Prima di riavviare o arrestare il server, sospendere e svuotare (rimuovere) eventuali ruoli, ad esempio macchine virtuali in esecuzione su di esso. Questo consente a Spazi di archiviazione diretti anche di scaricare ed eseguire normalmente il commit dei dati per garantire l'arresto trasparente di qualsiasi applicazione in esecuzione nel server in questione.

   > [!IMPORTANT]
   > Sospendere e svuotare sempre i server in cluster prima di riavviarli o arrestarli.

In PowerShell, eseguire il cmdlet seguente (come amministratore) per procedere con la sospensione e lo svuotamento.

```PowerShell
Suspend-ClusterNode -Drain
```

Per eseguire questa operazione in Gestione cluster di failover, passare a **Nodi**, fare clic con il tasto destro del mouse sul nodo, quindi selezionare **Sospendi** > **Svuota ruoli**.

![Sospensione/svuotamento](media/maintain-servers/pause-drain.png)

Tutte le macchine virtuali procederanno alla migrazione in tempo reale ad altri server del cluster. Possono essere necessari alcuni minuti.

   > [!NOTE]
   > Quando si sospende e svuota il nodo del cluster in modo corretto, Windows esegue un controllo di protezione automatico per assicurare che sia sicuro procedere. In presenza di volumi non integri, si interromperà e si riceverà un avviso per cui non è sicuro procedere.

![Controllo di protezione](media/maintain-servers/safety-check.png)

## Arresto del server in corso

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

Incompleto o lo stato operativo peggiorata è normale quando i nodi sono in fase di arresto o avvio/arresto del cluster in un nodo di servizio e non deve suscitare. Tutti i volumi restano online e accessibili.

## Ripresa del server

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

## In attesa della risincronizzazione dell'archiviazione

Alla ripresa, il server è necessario risincronizzazione eventuali nuove scritture verificatesi mentre era disponibile. Questo avviene automaticamente. Utilizzando il rilevamento delle modifiche intelligente, non è necessario analizzare o sincronizzare *tutti* i dati, ma solo le modifiche. Questo processo è limitato a ridurre l'impatto sui carichi di lavoro di produzione. A seconda di quanto tempo il server è stato in pausa e della quantità di nuovi dati scritti, potrebbero essere necessari molti minuti per il completamento.

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

## Come aggiornare offline nodi spazi di archiviazione diretta
Utilizzare la procedura seguente per percorso il sistema di spazi di archiviazione diretta rapidamente. Prevede di pianificazione di una finestra di manutenzione e prendendo il sistema verso il basso per l'applicazione di patch. Se è presente un aggiornamento di sicurezza critici che devi applicato rapidamente o magari sarà sufficiente garantire l'applicazione di patch viene completato nella finestra di manutenzione, questo metodo può essere per te. Questo processo viene premuto il cluster di spazi di archiviazione diretta, patch viene e lo introduce tutti fino nuovamente. Lo svantaggio è il tempo di inattività alle risorse ospitate.

1. Pianificare la finestra di manutenzione.
2. Disconnettere i dischi virtuali.
3. Arrestare il cluster per portare offline il pool di archiviazione. Eseguire il cmdlet **Stop-Cluster** o usare Gestione Cluster di Failover per arrestare il cluster.
4. Impostare il servizio cluster su **disabilitato** nei msc su ciascun nodo. Ciò impedisce l'avvio mentre in tutte le patch necessarie il servizio cluster.
5. Applicare l'aggiornamento cumulativo di Windows Server e qualsiasi richiesta aggiornamenti dello Stack di manutenzione per tutti i nodi. (È possibile aggiornare tutti i nodi nello stesso momento, non è necessario attendere dal momento che il cluster è verso il basso).  
6. Riavviare i nodi e garantire che tutti gli elementi sono ottimali.
7. Impostare il servizio cluster su **automatico** su ciascun nodo.
8. Avvia il cluster. Eseguire il cmdlet **Start-Cluster** o usare Gestione Cluster di Failover. 

   Assegnare alcuni minuti.  Assicurati che il pool di archiviazione è integro.
9. Convertire i dischi virtuali online.
10. Monitorare lo stato dei dischi virtuali eseguendo il cmdlet **Get-Volume** e **Get-VirtualDisk** .


## Vedi anche

- [Panoramica di Spazi di archiviazione diretta](storage-spaces-direct-overview.md)
- [Aggiornamento compatibile con cluster](https://technet.microsoft.com/library/hh831694.aspx)
