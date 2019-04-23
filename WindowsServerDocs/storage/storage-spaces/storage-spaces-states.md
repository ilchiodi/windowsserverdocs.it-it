---
title: Lo stato diretto di spazi di archiviazione e stati operativi
description: Come trovare e comprendere l'integrità diversi e stati operativi di spazi di archiviazione diretta e spazi di archiviazione (inclusi i dischi fisici, pool e i dischi virtuali) e operazioni da eseguire per risolverli.
keywords: Spazi di archiviazione, scollegati, disco virtuale, disco fisico, danneggiati
author: jasongerend
ms.author: jgerend
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage-spaces
manager: brianlic
ms.openlocfilehash: 5090a68270438bd9a06c7d50f9d4abca066d31e6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849142"
---
# <a name="troubleshoot-storage-spaces-direct-health-and-operational-states"></a>Risolvere i problemi di integrità di spazi di archiviazione diretta e gli stati operativi

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server (canale semestrale), Windows 10, Windows 8.1

In questo argomento descrive l'integrità e gli stati operativi di pool di archiviazione, i dischi virtuali (che si trovano sotto i volumi in spazi di archiviazione) e unità in [spazi di archiviazione diretta](storage-spaces-direct-overview.md) e [spazi di archiviazione](overview.md). Questi stati possono essere estremamente utile quando si tenta di risolvere i problemi vari, ad esempio perché non è possibile eliminare un disco virtuale a causa di una configurazione di sola lettura. Viene inoltre descritto il motivo per cui non è possibile aggiungere un'unità a un pool (il CannotPoolReason).

Spazi di archiviazione dispone di tre oggetti primari - *dischi fisici* (dischi rigidi, unità SSD, e così via) che vengono aggiunte a un *pool di archiviazione*, virtualizzare l'archiviazione in modo che sia possibile creare *idischivirtuali* da liberare spazio nel pool, come illustrato di seguito. I metadati del pool vengono scritti in ogni disco nel pool. I volumi vengono creati all'interno i dischi virtuali e archiviano i file, ma non dobbiamo parlare qui i volumi.

![I dischi fisici vengono aggiunti a un pool di archiviazione e quindi i dischi virtuali creati dallo spazio del pool](media/storage-spaces-states/storage-spaces-object-model.png)

È possibile visualizzare l'integrità e gli stati operativi in Server Manager o con PowerShell. Di seguito è riportato un esempio di un'ampia gamma di integrità (prevalentemente negativo) e stati operativi in un cluster di spazi di archiviazione diretta in cui Manca la maggior parte dei relativi nodi cluster (pulsante destro del mouse per aggiungere le intestazioni di colonna **lo stato operativo**). Non è un buon cluster.

![Server Manager, che mostra i risultati di due nodi mancanti in un cluster di spazi di archiviazione diretta - un numero elevato di dischi virtuali in uno stato non integro e i dischi fisici mancanti](media/storage-spaces-states/unhealthy-disks-in-server-manager.png)

## <a name="storage-pool-states"></a>Stati di pool di archiviazione

Ogni pool di archiviazione ha uno stato di integrità - **integro**, **avviso**, o **sconosciuto**/**Unhealthy**, anche con uno o più stati operativi.

Per scoprire quali lo stato è in un pool, usare i comandi PowerShell seguenti:

```PowerShell
Get-StoragePool -IsPrimordial $False | Select-Object HealthStatus, OperationalStatus, ReadOnlyReason
```

Ecco un esempio di output che mostra un pool di archiviazione con lo stato operativo di sola lettura lo stato di integrità sconosciuto:

```
FriendlyName                OperationalStatus HealthStatus IsPrimordial IsReadOnly
------------                ----------------- ------------ ------------ ----------
S2D on StorageSpacesDirect1 Read-only         Unknown      False        True
```

Le sezioni seguenti elencano l'integrità e gli stati operativi.

### <a name="pool-health-state-healthy"></a>Stato di integrità del pool: Integro

|stato operativo    |Descrizione|
|---------            |---------  |
|OK|Il pool di archiviazione sia integro.|

### <a name="pool-health-state-warning"></a>Stato di integrità del pool: Avviso

Quando il pool di archiviazione è il **avviso** lo stato di integrità, significa che il pool è accessibile, ma una o più unità non è riuscita o non sono presenti. Di conseguenza, il pool di archiviazione potrebbe avere ridotto la resilienza.

|stato operativo    |Descrizione|
|---------            |---------  |
|Ridotto|Esistono unità mancante o non riuscita nel pool di archiviazione. Questa condizione si verifica solo con le unità che ospita i metadati del pool. <br><br>**Azione**: Controllare lo stato delle unità e sostituire tutte le unità non riuscite prima che si verificano altri errori.|

### <a name="pool-health-state-unknown-or-unhealthy"></a>Stato di integrità del pool: Sconosciuto o non è integro

Quando un pool di archiviazione è nel **sconosciuto** o **Unhealthy** lo stato di integrità, significa che il pool di archiviazione è di sola lettura e non può essere modificato fino a quando non viene restituito al pool di **avviso**oppure **OK** gli stati di integrità.

|stato operativo    |Motivo di sola lettura |Descrizione|
|---------            |---------       |--------   |
|Sola lettura|Incompleto|Ciò può verificarsi se perde il pool di archiviazione relativi [quorum](understand-quorum.md), che significa che la maggior parte dei dischi nel pool hanno avuto esito negativo o sono offline per qualche motivo. Quando si perde il quorum per un pool, spazi di archiviazione imposta automaticamente la configurazione del pool in sola lettura fino a quando non abbastanza rigidi tornato disponibile.<br><br>**Azione:** <br>1. Ristabilire la connessione delle unità mancante e se si usa spazi di archiviazione diretta, portare online tutti i server. <br>2. Impostare il pool torna in lettura / scrittura aprendo una sessione di PowerShell con autorizzazioni amministrative e quindi digitando:<br><br> <code>Get-StoragePool <PoolName> -IsPrimordial $False \| Set-StoragePool -IsReadOnly $false</code>|
||Condizione|Un amministratore di imposta il pool di archiviazione di sola lettura.<br><br>**Azione:** Per impostare un pool di archiviazione in cluster per l'accesso in Gestione Cluster di Failover di lettura / scrittura, passare a **pool**, fare clic sul pool e quindi selezionare **portare Online**.<br><br>Per altri server e i PC, aprire una sessione di PowerShell con autorizzazioni amministrative e quindi digitare:<br><br><code>Get-StoragePool <PoolName> \| Set-StoragePool -IsReadOnly $false</code><br><br> |
||Starting|Avvio o in attesa per le unità siano connessi nel pool di spazi di archiviazione. Deve trattarsi di un stato temporaneo. Una volta completamente avviata, il pool deve passare in uno stato operativo diverso.<br><br>**Azione:** Se il pool resta nel *Starting* nello stato, assicurarsi che tutte le unità del pool siano collegate correttamente.|

Vedere anche [modifica un Pool di archiviazione che ha una configurazione di sola lettura](https://social.technet.microsoft.com/wiki/contents/articles/14861.modifying-a-storage-pool-that-has-a-read-only-configuration.aspx).

## <a name="virtual-disk-states"></a>Stati di disco virtuale

In spazi di archiviazione, i volumi vengono inseriti nei dischi virtuali (spazi di archiviazione) che vengono ottenuti dal spazio libero in un pool. Ogni disco virtuale presenta uno stato di integrità - **integro**, **avviso**, **Unhealthy**, oppure **sconosciuto** , nonché di uno o più stati operativi.

Per scoprire quali dischi virtuali dello stato sono in, utilizzare i comandi di PowerShell seguenti:

```PowerShell
Get-VirtualDisk | Select-Object FriendlyName,HealthStatus, OperationalStatus, DetachedReason
```

Ecco un esempio di output che mostra un disco virtuale disconnesso e un disco virtuale incompleto o danneggiato:

```
FriendlyName HealthStatus OperationalStatus      DetachedReason
------------ ------------ -----------------      --------------
Volume1      Unknown      Detached               By Policy
Volume2      Warning      {Degraded, Incomplete} None
```

Le sezioni seguenti elencano l'integrità e gli stati operativi.

### <a name="virtual-disk-health-state-healthy"></a>Stato di integrità del disco virtuale: Integro

|stato operativo    |Descrizione|
|---------            |---------          |
|OK    |Il disco virtuale è integro.|
|Non ottimale    |I dati non viene scritto in modo uniforme tra le unità. <br><br>**Azione**: Ottimizzare l'uso del disco nel pool di archiviazione eseguendo il [Optimize-StoragePool](https://technet.microsoft.com/itpro/powershell/windows/storage/optimize-storagepool) cmdlet.|

### <a name="virtual-disk-health-state-warning"></a>Stato di integrità del disco virtuale: Avviso

Quando il disco virtuale è in un **avviso** lo stato di integrità, significa che non sono disponibili uno o più copie dei dati, ma gli spazi di archiviazione può leggere almeno una copia dei dati.

|stato operativo    |Descrizione|
|---------            |---------          |
|Nel servizio            |Windows è riparare il disco virtuale, ad esempio dopo l'aggiunta o rimozione di un'unità. Una volta completato il ripristino, il disco virtuale deve restituire lo stato di integrità OK.|
|Incompleto           |La resilienza del disco virtuale viene ridotto perché una o più unità mancano o non è riuscita. Tuttavia, le unità mancante contengono copie aggiornate dei dati.<br><br> **Azione**: <br>1. Riconnettersi a qualsiasi unità mancanti e sostituire tutte le unità non riuscite se si usa spazi di archiviazione diretta, portare online tutti i server che sono offline. <br>2. Se non si usa spazi di archiviazione diretta, quindi ripristinare il disco virtuale usando il [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) cmdlet.<br> Spazi di archiviazione diretta avvia automaticamente una correzione se necessaria dopo la riconnessione o la sostituzione di un'unità.|
|Ridotto             |La resilienza del disco virtuale viene ridotto perché una o più unità mancano o non è riuscita e non esistono copie obsolete dei dati su tali unità. <br><br>**Azione**: <br> 1. Riconnettersi a qualsiasi unità mancanti e sostituire tutte le unità non riuscite se si usa spazi di archiviazione diretta, portare online tutti i server che sono offline. <br> 2. Se non si usa spazi di archiviazione diretta, quindi ripristinare il disco virtuale usando il [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) cmdlet. <br>Spazi di archiviazione diretta avvia automaticamente una correzione se necessaria dopo la riconnessione o la sostituzione di un'unità.|

### <a name="virtual-disk-health-state-unhealthy"></a>Stato di integrità del disco virtuale: Unhealthy

Quando un disco virtuale è in un **Unhealthy** lo stato di integrità, alcuni o tutti i dati nel disco virtuale è attualmente inaccessibili.

|stato operativo    |Descrizione|
|---------            |--------   |
|Senza ridondanza|Il disco virtuale ha perso dati poiché un numero eccessivo di unità non è riuscita.<br><br>**Azione**: Sostituire le unità non riuscite e quindi ripristinare i dati dal backup.|

### <a name="virtual-disk-health-state-informationunknown"></a>Stato di integrità del disco virtuale: Informazioni/sconosciuti

Il disco virtuale può trovarsi inoltre nel **le informazioni** lo stato di integrità (come illustrato nell'elemento del Pannello di controllo di spazi di archiviazione) o **sconosciuto** integrità stato (come illustrato in PowerShell) se un amministratore ha richiesto il disco virtuale non in linea o il disco virtuale è diventano detached.

|stato operativo    |Motivo scollegato |Descrizione|
|---------            |---------       |--------   |
|Detached             |Dai criteri            |Un amministratore hanno portato offline il disco virtuale o imposta il disco virtuale in modo da richiedere allegato manuale, nel qual caso sarà necessario collegare manualmente il disco virtuale ogni riavvio di Windows.,<br><br>**Azione**: Portare online il disco virtuale. A tale scopo, quando il disco virtuale è in un pool di archiviazione in cluster, selezionare in Gestione Cluster di Failover **memorizzazione** > **pool** > **dischi virtuali** , selezionare il disco virtuale che mostra le **Offline** lo stato e quindi selezionare **portare Online**. <br><br>Per portare online un disco virtuale quando non è in un cluster, aprire una sessione di PowerShell come amministratore e quindi provare a usare il comando seguente:<br><br> <code>Get-VirtualDisk \| Where-Object -Filter { $_.OperationalStatus -eq "Detached" } \| Connect-VirtualDisk</code><br><br>Per connettere automaticamente tutti i dischi virtuali non cluster dopo il riavvio di Windows, aprire una sessione di PowerShell come amministratore e quindi usare il comando seguente:<br><br> <code>Get-VirtualDisk \| Set-VirtualDisk -ismanualattach $false</code>|
|            |Dischi di maggioranza dei nodi non integri |Troppi dischi usati da questo disco virtuale non è riuscita, non sono presenti o include dati non aggiornati.   <br><br>**Azione**: <br> 1. Ristabilire la connessione delle unità mancante e se si usa spazi di archiviazione diretta, portare online tutti i server che sono offline. <br> 2. Dopo che tutte le unità e i server siano online, sostituire tutte le unità non riuscite. Visualizzare [servizio integrità](../../failover-clustering/health-service-overview.md) per informazioni dettagliate. <br>Spazi di archiviazione diretta avvia automaticamente una correzione se necessaria dopo la riconnessione o la sostituzione di un'unità.<br>3. Se non si usa spazi di archiviazione diretta, quindi ripristinare il disco virtuale usando il [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) cmdlet.  <br><br>Se più dischi non è riuscita a hai copie dei dati e il disco virtuale non è stati riparati errori interni, tutti i dati nel disco virtuale viene definitivamente perduto. In questo caso sfortunato, eliminare il disco virtuale, creare un nuovo disco virtuale e quindi ripristinare da un backup.|
|                     |Incompleto |Non sono sufficienti le unità sono presenti per leggere il disco virtuale.    <br><br>**Azione**: <br> 1. Ristabilire la connessione delle unità mancante e se si usa spazi di archiviazione diretta, portare online tutti i server che sono offline. <br> 2. Dopo che tutte le unità e i server siano online, sostituire tutte le unità non riuscite. Visualizzare [servizio integrità](../../failover-clustering/health-service-overview.md) per informazioni dettagliate. <br>Spazi di archiviazione diretta avvia automaticamente una correzione se necessaria dopo la riconnessione o la sostituzione di un'unità.<br>3. Se non si usa spazi di archiviazione diretta, quindi ripristinare il disco virtuale usando il [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) cmdlet.<br><br>Se più dischi non è riuscita a hai copie dei dati e il disco virtuale non è stati riparati errori interni, tutti i dati nel disco virtuale viene definitivamente perduto. In questo caso sfortunato, eliminare il disco virtuale, creare un nuovo disco virtuale e quindi ripristinare da un backup.|
| |Timeout|Collegare il disco virtuale ha impiegato troppo tempo <br><br> **Azione:** Questa situazione non dovrebbe verificarsi spesso, in modo che è possibile provare a vedere se la condizione viene soddisfatta nel tempo. Oppure è possibile provare a disconnettere il disco virtuale con il [Disconnect-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/disconnect-virtualdisk?view=win10-ps) cmdlet, quindi utilizzando il [Connect-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/connect-virtualdisk?view=win10-ps) per riconnettersi. |

## <a name="drive-physical-disk-states"></a>Stati dell'unità (disco fisico)

Le sezioni seguenti descrivono gli stati di integrità in cui può essere un'unità. Le unità in un pool sono rappresentate in PowerShell come *disco fisico* oggetti.

### <a name="drive-health-state-healthy"></a>Stato di integrità unità: Integro

|stato operativo    |Descrizione|
|---------            |---------          |
|OK|L'unità è integra.|
|Nel servizio|L'unità sta eseguendo alcune operazioni di manutenzione interna. Una volta completata l'azione, l'unità deve restituire per il *OK* lo stato di integrità.|

### <a name="drive-health-state-warning"></a>Stato di integrità unità: Avviso

Un'unità, è possibile stato avviso leggere e scrivere i dati correttamente ma si è verificato un problema.

|stato operativo    |Descrizione|
|---------            |---------          |
|Comunicazione perduta|L'unità è manca. Se si usa spazi di archiviazione diretta, ciò potrebbe essere che un server è inattivo.<br><br>**Azione**: Se si usa spazi di archiviazione diretta, portare online tutti i server. Se non viene risolto, riconnettere l'unità, sostituirla o provare a ottenere informazioni di diagnostica dettagliate relative a tale unità seguendo i passaggi di risoluzione dei problemi con segnalazione errori Windows > [timeout disco fisico](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-timed-out).|
|Rimozione dal pool|Spazi di archiviazione è in corso la rimozione dal relativo pool di archiviazione. <br><br> Questo è un stato temporaneo. Al termine della rimozione, se l'unità è ancora collegato al sistema, le unità passano allo stato operativo di un altro (in genere OK) in un pool originale.|
|Inizio modalità manutenzione|Spazi di archiviazione è in corso di inserire l'unità in modalità di manutenzione dopo che un amministratore di inserire l'unità in modalità di manutenzione. Questo è un stato temporaneo: l'unità deve presto disponibile nel *In modalità di manutenzione* dello stato.|
|In modalità di manutenzione|Un amministratore inserita l'unità in modalità manutenzione, l'arresto di lettura e scrittura dall'unità. Questa operazione viene in genere eseguita prima di aggiornare il firmware dell'unità, o durante il test degli errori.<br><br>**Azione**: Per sfruttare l'unità dalla modalità manutenzione, usare il [Disable-StorageMaintenanceMode](https://technet.microsoft.com/itpro/powershell/windows/storage/disable-storagemaintenancemode) cmdlet.|
|Arrestare la modalità di manutenzione|Un amministratore ha richiesto l'unità dalla modalità manutenzione e spazi di archiviazione è in corso di riportare online l'unità. Questo è un stato temporaneo: l'unità deve essere presto in un altro stato - idealmente *integro*.|
|Errore prevedibile|L'unità ha segnalato che è vicino al failover.<br><br>**Azione**: Sostituire l'unità.|
|Errore dei / o|Si è verificato un errore temporaneo durante l'accesso dell'unità.<br><br>**Azione**: <br>1. Se l'unità non eseguire la transizione al **OK** lo stato, è possibile provare a utilizzare il [Reset-PhysicalDisk](https://docs.microsoft.com/powershell/module/storage/reset-physicaldisk) cmdlet per l'unità di cancellazione dati. <br> 2. Uso [Repair-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/repair-virtualdisk) per ripristinare la resilienza dei dischi virtuali interessate. <br>3. Se si continua a verificarsi, sostituire l'unità.|
|Errore temporaneo|Si è verificato un errore temporaneo con l'unità. Ciò significa in genere l'unità stato non risponda, ma può anche indicare che gli spazi di archiviazione protettivo partizione in modo non appropriato è stato rimosso dall'unità. <br><br>**Azione**: <br>1. Se l'unità non eseguire la transizione al **OK** lo stato, è possibile provare a utilizzare il [Reset-PhysicalDisk](https://docs.microsoft.com/powershell/module/storage/reset-physicaldisk) cmdlet per l'unità di cancellazione dati. <br> 2. Uso [Repair-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/repair-virtualdisk) per ripristinare la resilienza dei dischi virtuali interessate. <br>3. Se si continua a verificarsi, sostituire l'unità o provare a ottenere informazioni di diagnostica dettagliate relative a tale unità seguendo i passaggi di risoluzione dei problemi con segnalazione errori Windows > [disco fisico non è stato possibile portare in linea](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online).|
|Latenza anomala|L'unità esegue lentamente, misurato dal servizio integrità in spazi di archiviazione diretta.<br><br>**Azione**: Se si continua a verificarsi, sostituire l'unità in modo da non riduce le prestazioni di spazi di archiviazione nel suo complesso.

### <a name="drive-health-state-unhealthy"></a>Stato di integrità unità: Unhealthy

Un'unità in stato non integro non può attualmente essere scritto o si accede.

|stato operativo    |Descrizione|
|---------            |---------          |
|Non è utilizzabile|Questa unità non può essere utilizzata da spazi di archiviazione. Per altre informazioni, vedere [requisiti hardware di spazi di archiviazione diretta](storage-spaces-direct-hardware-requirements.md); se non si usa spazi di archiviazione diretta, vedere [Panoramica di spazi di archiviazione](https://technet.microsoft.com/library/hh831739(v=ws.11).aspx#Requirements).|
|Split|L'unità è diventano separato dal pool.<br><br>**Azione**: Reimpostare l'unità, la cancellazione di tutti i dati dall'unità e aggiungerlo al pool come un'unità vuota. A tale scopo, aprire una sessione di PowerShell come amministratore, eseguire la [Reset-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk) cmdlet, quindi eseguire [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk). <br><br>Per ottenere informazioni di diagnostica dettagliate relative a tale unità, seguire i passaggi di risoluzione dei problemi con segnalazione errori Windows > [disco fisico non è stato possibile portare in linea](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online).|
|Metadati obsoleti|Spazi di archiviazione trovato metadati obsoleti nell'unità.<br><br>**Azione**: Deve trattarsi di un stato temporaneo. Se l'unità non eseguire la transizione a OK, è possibile eseguire [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) inizi un'operazione di ripristino su dischi virtuali interessate. Se non viene risolto il problema, è possibile ripristinare l'unità con il [Reset-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk) cmdlet di cancellazione di tutti i dati dall'unità, quindi eseguire [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk).|
|Metadati non riconosciuti|Spazi di archiviazione trovato metadati non riconosciuto nell'unità, questo in genere significa che l'unità sia i metadati da un pool diverso su di esso.<br><br>**Azione**: Per cancellare l'unità e aggiungerlo al pool corrente, reimpostare l'unità. Per reimpostare l'unità, aprire una sessione di PowerShell come amministratore, eseguire la [Reset-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk) cmdlet, quindi eseguire [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk).|
|Supporto con errori|L'unità non è riuscita e non sarà più essere usato da spazi di archiviazione.<br><br>**Azione**: Sostituire l'unità. <br><br>Per ottenere informazioni di diagnostica dettagliate relative a tale unità, seguire i passaggi di risoluzione dei problemi con segnalazione errori Windows > [disco fisico non è stato possibile portare in linea](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online).|
|Errore hardware del dispositivo|Si è verificato un errore hardware in questa unità. <br><br>**Azione**: Sostituire l'unità.|
|Aggiornamento del firmware.|Windows è l'aggiornamento del firmware nell'unità. Questo è un stato temporaneo che in genere dura meno di un minuto e durante il quale ora che altre unità nel pool di gestire tutte le letture e scritture. Per altre informazioni, vedi [aggiornare il firmware dell'unità](../update-firmware.md).|
|Starting|L'unità si sta preparando per l'operazione. Deve trattarsi di un stato temporaneo - al termine dell'operazione, che l'unità deve eseguire la transizione a uno stato operativo diverso.|

## <a name="reasons-a-drive-cant-be-pooled"></a>Motivi di che un'unità non è possibile raggruppare in pool

Alcune unità non appena sono pronte essere in un pool di archiviazione. È possibile scoprire il motivo per cui un'unità non è idonea per il pool, esaminando il `CannotPoolReason` proprietà di un disco fisico. Ecco un esempio di script di PowerShell per visualizzare la proprietà CannotPoolReason:

```PowerShell
Get-PhysicalDisk | Format-Table FriendlyName,MediaType,Size,CanPool,CannotPoolReason
```

Ecco un esempio di output:

```
FriendlyName          MediaType          Size CanPool CannotPoolReason
------------          ---------          ---- ------- ----------------
ATA MZ7LM120HCFD00D3  SSD        120034123776   False Insufficient Capacity
Msft Virtual Disk     SSD         10737418240    True
Generic Physical Disk SSD        119990648832   False In a Pool
```

Nella tabella seguente fornisce altri dettagli su ciascuno dei motivi.

|Motivo|Descrizione|
|---|---|
|In un pool|L'unità appartiene già a un pool di archiviazione. <br><br>Le unità possono appartenere a solo un unico pool di archiviazione alla volta. Per usare questa unità in un altro pool di archiviazione, prima di tutto rimuovere l'unità dal relativo pool esistente, che indica a spostare i dati nell'unità in altre unità nel pool di spazi di archiviazione. O reimpostare l'unità se l'unità è stato disconnesso dal relativo pool senza avvisare gli spazi di archiviazione. <br><br>Per rimuovere in modo sicuro un'unità da un pool di archiviazione, usare [Remove-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/remove-physicaldisk), oppure passare a Server Manager > **servizi File e archiviazione** > **i pool di archiviazione**, > **Dischi fisici**, fare doppio clic su unità e quindi selezionare **Rimuovi disco**.<br><br>Per ripristinare un'unità, usare [Reset-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk).|
|Non è integro|L'unità non è in uno stato integro e potrebbe essere necessario sostituire.|
|Supporto rimovibile|L'unità viene classificato come un'unità rimovibile. <br><br>Spazi di archiviazione non supporta multimediali che sono riconosciuti da Windows come rimovibile, ad esempio le unità disco Blu-Ray. Sebbene molti corretti le unità sono negli slot di archivi rimovibili, in generale, supporti *classificati* da Windows come rimovibile non sono adatti per l'uso con spazi di archiviazione.|
|In uso dal cluster|L'unità viene attualmente usata da un Cluster di Failover.|
|Offline|L'unità è offline. <br><br>Per portare offline tutti i dischi online e impostato su lettura/scrittura, aprire una sessione di PowerShell come amministratore e usare gli script seguenti:<br><br><code>Get-Disk \| Where-Object -Property OperationalStatus -EQ "Offline" \| Set-Disk -IsOffline $false</code><br><br><code>Get-Disk \| Where-Object -Property IsReadOnly -EQ $true \| Set-Disk -IsReadOnly $false</code>|
|Capacità insufficiente|Ciò si verifica in genere quando sono presenti partizioni occupare lo spazio disponibile sull'unità. <br><br>**Azione**: Eliminare i volumi nell'unità cancellando tutti i dati nell'unità. Un modo per farlo consiste nell'usare la [Clear – Disk](https://docs.microsoft.com/powershell/module/storage/clear-disk?view=win10-ps) cmdlet di PowerShell.|
|Verifica in corso.|Il [servizio integrità](../../failover-clustering/health-service-overview.md#supported-components-document) è un controllo per verificare se l'unità o firmware l'unità è approvato per l'uso, dall'amministratore del server.|
|Verifica non riuscita|Il [servizio integrità](../../failover-clustering/health-service-overview.md#supported-components-document) non è stato possibile verificare se l'unità o firmware l'unità è approvato per l'uso, dall'amministratore del server.|
|Firmware non conforme|Il firmware del disco fisico non è nell'elenco delle revisioni del firmware approvati dall'amministratore del server specificato usando il [servizio integrità](../../failover-clustering/health-service-overview.md#supported-components-document). |
|Hardware non conforme|L'unità non è nell'elenco dei modelli di archiviazione approvati specificata dall'amministratore del server utilizzando il [servizio integrità](../../failover-clustering/health-service-overview.md#supported-components-document).|

## <a name="see-also"></a>Vedere anche

- [Spazi di archiviazione diretta](storage-spaces-direct-overview.md)
- [Requisiti hardware diretto di spazi di archiviazione](storage-spaces-direct-hardware-requirements.md)
- [Quorum del cluster e pool di comprensione](understand-quorum.md)