---
title: Spazi di archiviazione e Spazi di archiviazione diretta stato operativo
description: Come trovare e comprendere i diversi Stati di integrità e operativi di Spazi di archiviazione diretta e spazi di archiviazione (inclusi i dischi fisici, i pool e i dischi virtuali) e le operazioni da eseguire su di essi.
author: jasongerend
ms.author: jgerend
ms.date: 12/06/2019
ms.topic: article
ms.prod: windows-server
ms.technology: storage-spaces
manager: brianlic
ms.openlocfilehash: 83489cb7a8a44de13b5ba245d7ce1cb5ceabc08e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820974"
---
# <a name="troubleshoot-storage-spaces-and-storage-spaces-direct-health-and-operational-states"></a>Risolvere i problemi relativi a spazi di archiviazione e Spazi di archiviazione diretta stato operativo

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server (canale semestrale), Windows 10, Windows 8.1

Questo argomento descrive lo stato di integrità e operativo dei pool di archiviazione, i dischi virtuali (che si trovano sotto i volumi in spazi di archiviazione) e le unità in [spazi di archiviazione diretta](storage-spaces-direct-overview.md) e [spazi di archiviazione](overview.md). Questi Stati possono risultare utili quando si tenta di risolvere vari problemi, ad esempio perché non è possibile eliminare un disco virtuale a causa di una configurazione di sola lettura. Viene inoltre illustrato il motivo per cui un'unità non può essere aggiunta a un pool (CannotPoolReason).

Spazi di archiviazione ha tre oggetti primari: *dischi fisici* (unità disco rigido, SSD e così via) che vengono aggiunti a un *pool di archiviazione*, virtualizzando l'archiviazione in modo che sia possibile creare *dischi virtuali* dallo spazio disponibile nel pool, come illustrato di seguito. I metadati del pool vengono scritti in ogni unità del pool. I volumi vengono creati sopra i dischi virtuali e archiviano i file, ma in questo caso non parleremo dei volumi.

![I dischi fisici vengono aggiunti a un pool di archiviazione e quindi i dischi virtuali creati dallo spazio del pool](media/storage-spaces-states/storage-spaces-object-model.png)

È possibile visualizzare gli Stati di integrità e operativi in Server Manager o con PowerShell. Di seguito è riportato un esempio di un'ampia gamma di Stati di integrità e operativi in un cluster Spazi di archiviazione diretta in cui manca la maggior parte dei nodi del cluster, facendo clic con il pulsante destro del mouse sulle intestazioni di colonna per aggiungere **lo stato operativo**. Si tratta di un cluster non felice.

![Server Manager Mostra i risultati di due nodi mancanti in un cluster Spazi di archiviazione diretta, ovvero molti dischi fisici mancanti e dischi virtuali in uno stato non integro](media/storage-spaces-states/unhealthy-disks-in-server-manager.png)

## <a name="storage-pool-states"></a>Stati del pool di archiviazione

Ogni pool di archiviazione presenta uno stato di integrità, ovvero **integro**, **avviso**o **sconosciuto**/non **integro**, oltre a uno o più stati operativi.

Per conoscere lo stato di un pool, usare i comandi di PowerShell seguenti:

```PowerShell
Get-StoragePool -IsPrimordial $False | Select-Object HealthStatus, OperationalStatus, ReadOnlyReason
```

Di seguito è riportato un esempio di output che mostra un pool di archiviazione nello stato di integrità sconosciuto con lo stato operativo di sola lettura:

```
FriendlyName                OperationalStatus HealthStatus IsPrimordial IsReadOnly
------------                ----------------- ------------ ------------ ----------
S2D on StorageSpacesDirect1 Read-only         Unknown      False        True
```

Le sezioni seguenti elencano l'integrità e gli stati operativi.

### <a name="pool-health-state-healthy"></a>Stato di integrità del pool: integro

| Stato operativo    | Descrizione |
| ---------            | ---------  |
| OK | Il pool di archiviazione è integro. |

### <a name="pool-health-state-warning"></a>Stato di integrità del pool: avviso

Quando il pool di archiviazione è nello stato di integrità di **avviso** , significa che il pool è accessibile, ma una o più unità hanno avuto esito negativo o sono mancanti. Di conseguenza, il pool di archiviazione potrebbe avere una resilienza ridotta.

|Stato operativo    |Descrizione|
|---------            |---------  |
|Ridotto|Nel pool di archiviazione sono presenti unità non riuscite o mancanti. Questa condizione si verifica solo con le unità che ospitano i metadati del pool. <br><br>**Azione**: controllare lo stato delle unità e sostituire le unità non riuscite prima che si verifichino altri errori.|

### <a name="pool-health-state-unknown-or-unhealthy"></a>Stato di integrità del pool: sconosciuto o non integro

Quando un pool di archiviazione si trova nello stato di integrità **sconosciuto** o non **integro** , significa che il pool di archiviazione è di sola lettura e non può essere modificato finché il pool non viene restituito agli Stati di **avviso** o di integrità **OK** .

|Stato operativo    |Motivo di sola lettura |Descrizione|
|---------            |---------       |--------   |
|Sola lettura|Incompleto|Questa situazione può verificarsi se il pool di archiviazione perde il proprio [quorum](understand-quorum.md), il che significa che la maggior parte delle unità nel pool non è riuscita o è offline per qualche motivo. Quando un pool perde il quorum, spazi di archiviazione imposta automaticamente la configurazione del pool in sola lettura fino a quando le unità non saranno più disponibili.<br><br>**Azione** <br>1. Riconnettere le unità mancanti e, se si usa Spazi di archiviazione diretta, portare online tutti i server. <br>2. impostare di nuovo il pool su lettura/scrittura aprendo una sessione di PowerShell con autorizzazioni amministrative e digitando:<br><br> <code>Get-StoragePool <PoolName> -IsPrimordial $False \| Set-StoragePool -IsReadOnly $false</code>|
||Condizione|Un amministratore ha impostato il pool di archiviazione in modalità di sola lettura.<br><br>**Azione:** Per impostare un pool di archiviazione in cluster per l'accesso in lettura/scrittura in Gestione cluster di failover, passare a **pool**, fare clic con il pulsante destro del mouse sul pool, quindi scegliere **porta online**.<br><br>Per gli altri server e PC, aprire una sessione di PowerShell con autorizzazioni amministrative e quindi digitare:<br><br><code>Get-StoragePool <PoolName> \| Set-StoragePool -IsReadOnly $false</code><br><br> |
||Starting|È in corso l'avvio di spazi di archiviazione o l'attesa della connessione delle unità nel pool. Si tratta di uno stato temporaneo. Una volta avviato completamente, il pool deve passare a uno stato operativo diverso.<br><br>**Azione:** Se il pool rimane nello stato *iniziale* , verificare che tutte le unità del pool siano connesse correttamente.|

Vedere anche [modifica di un pool di archiviazione con una configurazione](https://social.technet.microsoft.com/wiki/contents/articles/14861.modifying-a-storage-pool-that-has-a-read-only-configuration.aspx)di sola lettura.

## <a name="virtual-disk-states"></a>Stati del disco virtuale

In spazi di archiviazione, i volumi vengono posizionati su dischi virtuali (spazi di archiviazione) che sono incisi nello spazio libero in un pool. Ogni disco virtuale presenta uno stato di integrità, **integro**, **avviso**, non **integro**o **sconosciuto** , oltre a uno o più stati operativi.

Per scoprire quali sono i dischi virtuali di stato, usare i comandi di PowerShell seguenti:

```PowerShell
Get-VirtualDisk | Select-Object FriendlyName,HealthStatus, OperationalStatus, DetachedReason
```

Di seguito è riportato un esempio di output che mostra un disco virtuale scollegato e un disco virtuale danneggiato/incompleto:

```
FriendlyName HealthStatus OperationalStatus      DetachedReason
------------ ------------ -----------------      --------------
Volume1      Unknown      Detached               By Policy
Volume2      Warning      {Degraded, Incomplete} None
```

Le sezioni seguenti elencano l'integrità e gli stati operativi.

### <a name="virtual-disk-health-state-healthy"></a>Stato di integrità disco virtuale: integro

|Stato operativo    |Descrizione|
|---------            |---------          |
|OK    |Il disco virtuale è integro.|
|Non ottimali    |I dati non vengono scritti uniformemente tra le unità. <br><br>**Azione**: ottimizzare l'utilizzo dell'unità nel pool di archiviazione eseguendo il cmdlet [optimize-StoragePool](https://technet.microsoft.com/itpro/powershell/windows/storage/optimize-storagepool) .|

### <a name="virtual-disk-health-state-warning"></a>Stato di integrità disco virtuale: avviso

Quando il disco virtuale si trova in uno stato di **avviso** , significa che una o più copie dei dati non sono disponibili, ma gli spazi di archiviazione possono comunque leggere almeno una copia dei dati.

|Stato operativo    |Descrizione|
|---------            |---------          |
|In servizio            |Windows sta ripristinando il disco virtuale, ad esempio dopo l'aggiunta o la rimozione di un'unità. Al termine della riparazione, il disco virtuale dovrebbe tornare allo stato di integrità OK.|
|Incompleto           |La resilienza del disco virtuale è ridotta perché una o più unità hanno avuto esito negativo o sono mancanti. Tuttavia, le unità mancanti contengono copie aggiornate dei dati.<br><br> **Azione**: <br>1. Riconnettere le unità mancanti, sostituire eventuali unità non riuscite e, se si usa Spazi di archiviazione diretta, portare online tutti i server offline. <br>2. se non si usa Spazi di archiviazione diretta, ripristinare il disco virtuale usando il cmdlet [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) .<br> Spazi di archiviazione diretta avvia automaticamente un ripristino se necessario dopo la riconnessione o la sostituzione di un'unità.|
|Ridotto             |La resilienza del disco virtuale è ridotta perché una o più unità hanno avuto esito negativo o sono mancanti ed è presente una copia obsoleta dei dati in queste unità. <br><br>**Azione**: <br> 1. Riconnettere le unità mancanti, sostituire eventuali unità non riuscite e, se si usa Spazi di archiviazione diretta, portare online tutti i server offline. <br> 2. se non si usa Spazi di archiviazione diretta, ripristinare il disco virtuale usando il cmdlet [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) . <br>Spazi di archiviazione diretta avvia automaticamente un ripristino se necessario dopo la riconnessione o la sostituzione di un'unità.|

### <a name="virtual-disk-health-state-unhealthy"></a>Stato di integrità disco virtuale: non integro

Quando un disco virtuale si trova in uno stato di integrità non **integro** , alcuni o tutti i dati nel disco virtuale sono attualmente inaccessibili.

|Stato operativo    |Descrizione|
|---------            |--------   |
|Nessuna ridondanza|Il disco virtuale ha perso i dati perché troppe unità non sono riuscite.<br><br>**Azione**: sostituire le unità non riuscite e quindi ripristinare i dati dal backup.|

### <a name="virtual-disk-health-state-informationunknown"></a>Stato di integrità disco virtuale: informazioni/sconosciuto

Il disco virtuale può anche trovarsi nello stato di integrità **delle informazioni** , come illustrato nell'elemento del pannello di controllo spazi di archiviazione, o stato di integrità **sconosciuto** (come illustrato in PowerShell) se un amministratore ha portato il disco virtuale offline o il disco virtuale è stato scollegato.

|Stato operativo    |Motivo scollegato |Descrizione|
|---------            |---------       |--------   |
|Detached             |Per criterio            |Un amministratore ha portato il disco virtuale offline oppure ha impostato il disco virtuale per richiedere l'allegato manuale. in questo caso sarà necessario collegare manualmente il disco virtuale ogni volta che viene riavviato Windows.<br><br>**Azione**: riportare online il disco virtuale. A tale scopo, quando il disco virtuale si trova in un pool di archiviazione in cluster, in Gestione cluster di failover selezionare **pool** di > di **archiviazione** > **dischi virtuali**, selezionare il disco virtuale che mostra lo stato **offline** , quindi selezionare **porta online**. <br><br>Per riportare online un disco virtuale quando non è presente in un cluster, aprire una sessione di PowerShell come amministratore e quindi provare a usare il comando seguente:<br><br> <code>Get-VirtualDisk \| Where-Object -Filter { $_.OperationalStatus -eq "Detached" } \| Connect-VirtualDisk</code><br><br>Per alleghi automaticamente tutti i dischi virtuali non in cluster dopo il riavvio di Windows, aprire una sessione di PowerShell come amministratore e quindi usare il comando seguente:<br><br> <code>Get-VirtualDisk \| Set-VirtualDisk -ismanualattach $false</code>|
|            |Dischi di maggioranza non integri |Troppe unità usate dal disco virtuale non riuscite, mancanti o con dati non aggiornati.   <br><br>**Azione**: <br> 1. Riconnettere le unità mancanti e, se si usa Spazi di archiviazione diretta, portare online tutti i server offline. <br> 2. dopo che tutte le unità e i server sono online, sostituire le unità non riuscite. Per informazioni dettagliate, vedere [servizio integrità](../../failover-clustering/health-service-overview.md) . <br>Spazi di archiviazione diretta avvia automaticamente un ripristino se necessario dopo la riconnessione o la sostituzione di un'unità.<br>3. se non si usa Spazi di archiviazione diretta, ripristinare il disco virtuale usando il cmdlet [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) .  <br><br>Se non sono stati rilevati più dischi di quante copie dei dati e il disco virtuale non è stato ripristinato tra errori, tutti i dati nel disco virtuale andranno persi definitivamente. In questo caso sfortunato, eliminare il disco virtuale, creare un nuovo disco virtuale e quindi eseguire il ripristino da un backup.|
|                     |Incompleto |Sono presenti unità insufficienti per leggere il disco virtuale.    <br><br>**Azione**: <br> 1. Riconnettere le unità mancanti e, se si usa Spazi di archiviazione diretta, portare online tutti i server offline. <br> 2. dopo che tutte le unità e i server sono online, sostituire le unità non riuscite. Per informazioni dettagliate, vedere [servizio integrità](../../failover-clustering/health-service-overview.md) . <br>Spazi di archiviazione diretta avvia automaticamente un ripristino se necessario dopo la riconnessione o la sostituzione di un'unità.<br>3. se non si usa Spazi di archiviazione diretta, ripristinare il disco virtuale usando il cmdlet [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) .<br><br>Se non sono stati rilevati più dischi di quante copie dei dati e il disco virtuale non è stato ripristinato tra errori, tutti i dati nel disco virtuale andranno persi definitivamente. In questo caso sfortunato, eliminare il disco virtuale, creare un nuovo disco virtuale e quindi eseguire il ripristino da un backup.|
| |Timeout|Il fissaggio del disco virtuale ha richiesto troppo tempo <br><br> **Azione:** Questa operazione non deve essere eseguita spesso, quindi è possibile provare a verificare se la condizione passa nel tempo. In alternativa, è possibile provare a disconnettere il disco virtuale con il cmdlet Disconnect [-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/disconnect-virtualdisk?view=win10-ps) e quindi usare il cmdlet [Connect-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/connect-virtualdisk?view=win10-ps) per riconnetterlo. |

## <a name="drive-physical-disk-states"></a>Stati unità (disco fisico)

Le sezioni seguenti descrivono gli Stati di integrità in cui può trovarsi un'unità. Le unità in un pool sono rappresentate in PowerShell come oggetti *disco fisico* .

### <a name="drive-health-state-healthy"></a>Stato di integrità unità: integro

|Stato operativo    |Descrizione|
|---------            |---------          |
|OK|L'unità è integra.|
|In servizio|L'unità esegue alcune operazioni interne di manutenzione. Al termine dell'azione, l'unità dovrebbe tornare allo stato di integrità *OK* .|

### <a name="drive-health-state-warning"></a>Stato di integrità dell'unità: avviso

Un'unità nello stato di avviso può leggere e scrivere dati correttamente, ma presenta un problema.

|Stato operativo    |Descrizione|
|---------            |---------          |
|Comunicazione persa|Unità mancante. Se si usa Spazi di archiviazione diretta, il problema potrebbe essere dovuto al fatto che un server è inattivo.<br><br>**Azione**: se si usa spazi di archiviazione diretta, riportare tutti i server online. Se il problema persiste, riconnettere l'unità, sostituirla o provare a ottenere informazioni di diagnostica dettagliate su questa unità attenendosi alla procedura descritta nella sezione risoluzione dei problemi con Segnalazione errori Windows > si è [verificato un timeout del disco fisico](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-timed-out).|
|Rimozione dal pool|Lo spazio di archiviazione è in corso di rimozione dell'unità dal pool di archiviazione. <br><br> Si tratta di uno stato temporaneo. Una volta completata la rimozione, se l'unità è ancora collegata al sistema, l'unità passa a un altro stato operativo, in genere OK, in un pool primordiale.|
|Avvio della modalità di manutenzione|Spazi di archiviazione è in corso di inserimento dell'unità in modalità manutenzione dopo che un amministratore ha attivato la modalità manutenzione per l'unità. Si tratta di uno stato temporaneo. l'unità dovrebbe presto trovarsi nello stato *modalità manutenzione* .|
|In modalità manutenzione|Un amministratore ha inserito l'unità in modalità di manutenzione, bloccando le letture e le Scritture dall'unità. Questa operazione viene in genere eseguita prima di aggiornare il firmware dell'unità o quando si verificano errori.<br><br>**Azione**: per disattivare la modalità di manutenzione dell'unità, usare il cmdlet [Disable-StorageMaintenanceMode](https://technet.microsoft.com/itpro/powershell/windows/storage/disable-storagemaintenancemode) .|
|Arresto della modalità di manutenzione|La modalità di manutenzione è stata disattivata da un amministratore e spazi di archiviazione è in corso per riportare online l'unità. Si tratta di uno stato temporaneo. l'unità dovrebbe presto trovarsi in un altro stato, idealmente *integro*.|
|Errore predittivo|L'unità ha segnalato che si è verificato un errore.<br><br>**Azione**: sostituire l'unità.|
|Errore IO|Si è verificato un errore temporaneo durante l'accesso all'unità.<br><br>**Azione**: <br>1. se l'unità non esegue la transizione allo stato **OK** , è possibile provare a usare il cmdlet [Reset-PhysicalDisk](https://docs.microsoft.com/powershell/module/storage/reset-physicaldisk) per eliminare l'unità. <br> 2. usare [Repair-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/repair-virtualdisk) per ripristinare la resilienza dei dischi virtuali interessati. <br>3. se il problema si verifica, sostituire l'unità.|
|Errore temporaneo|Si è verificato un errore temporaneo con l'unità. Ciò significa in genere che l'unità non risponde, ma potrebbe anche significare che la partizione di protezione degli spazi di archiviazione è stata rimossa in modo non appropriato dall'unità. <br><br>**Azione**: <br>1. se l'unità non esegue la transizione allo stato **OK** , è possibile provare a usare il cmdlet [Reset-PhysicalDisk](https://docs.microsoft.com/powershell/module/storage/reset-physicaldisk) per eliminare l'unità. <br> 2. usare [Repair-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/repair-virtualdisk) per ripristinare la resilienza dei dischi virtuali interessati. <br>3. se il problema si verifica, sostituire l'unità oppure provare a ottenere informazioni di diagnostica dettagliate su questa unità attenendosi alla procedura descritta nella sezione relativa alla risoluzione dei problemi utilizzando Segnalazione errori Windows > [disco fisico non è](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online)stato in grado di portare in linea.|
|Latenza anormale|L'unità viene eseguita lentamente, misurata dal Servizio integrità in Spazi di archiviazione diretta.<br><br>**Azione**: se il problema persiste, sostituire l'unità in modo che non riduca le prestazioni di spazi di archiviazione nel suo insieme.

### <a name="drive-health-state-unhealthy"></a>Stato di integrità dell'unità: non integro

Non è attualmente possibile scrivere o accedere a un'unità nello stato non integro.

|Stato operativo    |Descrizione|
|---------            |---------          |
|Non utilizzabile|Questa unità non può essere usata da spazi di archiviazione. Per altre informazioni, vedere [spazi di archiviazione diretta requisiti hardware](storage-spaces-direct-hardware-requirements.md); Se non si usa Spazi di archiviazione diretta, vedere [Panoramica di spazi di archiviazione](https://technet.microsoft.com/library/hh831739(v=ws.11).aspx#Requirements).|
|Dividi|L'unità è stata separata dal pool.<br><br>**Azione**: ripristinare l'unità, cancellando tutti i dati dall'unità e aggiungendoli di nuovo al pool come unità vuota. A tale scopo, aprire una sessione di PowerShell come amministratore, eseguire il cmdlet [Reset-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk) , quindi eseguire [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk). <br><br>Per ottenere informazioni di diagnostica dettagliate su questa unità, seguire i passaggi descritti in risoluzione dei problemi utilizzando Segnalazione errori Windows > [disco fisico non è stato possibile portare online](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online).|
|Metadati non aggiornati|Negli spazi di archiviazione sono stati trovati metadati obsoleti sull'unità.<br><br>**Azione**: dovrebbe essere uno stato temporaneo. Se l'unità non esegue la transizione a OK, è possibile eseguire [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) per avviare un'operazione di ripristino sui dischi virtuali interessati. Se il problema persiste, è possibile reimpostare l'unità con il cmdlet [Reset-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk) , cancellare tutti i dati dall'unità, quindi eseguire [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk).|
|Metadati non riconosciuti|Negli spazi di archiviazione sono stati trovati metadati non riconosciuti sull'unità, che in genere significa che l'unità contiene metadati di un pool diverso.<br><br>**Azione**: per eliminare l'unità e aggiungerla al pool corrente, reimpostare l'unità. Per reimpostare l'unità, aprire una sessione di PowerShell come amministratore, eseguire il cmdlet [Reset-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk) , quindi eseguire [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk).|
|Supporti non riusciti|L'unità non è riuscita e non verrà più usata da spazi di archiviazione.<br><br>**Azione**: sostituire l'unità. <br><br>Per ottenere informazioni di diagnostica dettagliate su questa unità, seguire i passaggi descritti in risoluzione dei problemi utilizzando Segnalazione errori Windows > [disco fisico non è stato possibile portare online](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online).|
|Errore hardware del dispositivo|Si è verificato un errore hardware in questa unità. <br><br>**Azione**: sostituire l'unità.|
|Aggiornamento del firmware.|Windows sta aggiornando il firmware sull'unità. Si tratta di uno stato temporaneo che in genere dura meno di un minuto e durante il quale altre unità del pool gestiscono tutte le operazioni di lettura e scrittura. Per altre informazioni, vedere [aggiornare il firmware dell'unità](../update-firmware.md).|
|Starting|L'unità viene preparata per l'operazione. Si tratta di uno stato temporaneo, al termine del quale l'unità dovrebbe passare a uno stato operativo diverso.|

## <a name="reasons-a-drive-cant-be-pooled"></a>Motivi per cui non è possibile raggruppare un'unità

Alcune unità non sono pronte per essere incluse in un pool di archiviazione. È possibile scoprire perché un'unità non è idonea per il pool osservando la proprietà `CannotPoolReason` di un disco fisico. Ecco un esempio di script di PowerShell per visualizzare la proprietà CannotPoolReason:

```PowerShell
Get-PhysicalDisk | Format-Table FriendlyName,MediaType,Size,CanPool,CannotPoolReason
```

Di seguito è riportato un esempio di output:

```
FriendlyName          MediaType          Size CanPool CannotPoolReason
------------          ---------          ---- ------- ----------------
ATA MZ7LM120HCFD00D3  SSD        120034123776   False Insufficient Capacity
Msft Virtual Disk     SSD         10737418240    True
Generic Physical Disk SSD        119990648832   False In a Pool
```

Nella tabella seguente vengono fornite informazioni dettagliate su ogni motivo.

|Motivo|Descrizione|
|---|---|
|In un pool|L'unità appartiene già a un pool di archiviazione. <br><br>Le unità possono appartenere solo a un singolo pool di archiviazione alla volta. Per usare questa unità in un altro pool di archiviazione, rimuovere prima l'unità dal pool esistente, che indica agli spazi di archiviazione di spostare i dati nell'unità in altre unità del pool. In alternativa, reimpostare l'unità se l'unità è stata disconnessa dal pool senza notificare gli spazi di archiviazione. <br><br>Per rimuovere in modo sicuro un'unità da un pool di archiviazione, usare [Remove-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/remove-physicaldisk)oppure passare a Server Manager > **Servizi File e archiviazione** > **pool di archiviazione**, > **dischi fisici**, fare clic con il pulsante destro del mouse sull'unità e scegliere **Rimuovi disco**.<br><br>Per reimpostare un'unità, usare [Reset-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk).|
|Non integro|L'unità non è in uno stato integro e potrebbe essere necessario sostituirla.|
|Supporto rimovibile|L'unità è classificata come unità rimovibile. <br><br>Spazi di archiviazione non supporta supporti riconosciuti da Windows come rimovibili, ad esempio le unità Blu-ray. Sebbene molte unità fisse si trovino in slot rimovibili, in generale, i supporti *classificati* da Windows come rimovibili non sono idonei per l'uso con spazi di archiviazione.|
|Utilizzato dal cluster|L'unità è attualmente utilizzata da un cluster di failover.|
|Offline|L'unità è offline. <br><br>Per portare online tutte le unità offline e impostare la lettura/scrittura, aprire una sessione di PowerShell come amministratore e usare gli script seguenti:<br><br><code>Get-Disk \| Where-Object -Property OperationalStatus -EQ "Offline" \| Set-Disk -IsOffline $false</code><br><br><code>Get-Disk \| Where-Object -Property IsReadOnly -EQ $true \| Set-Disk -IsReadOnly $false</code>|
|Capacità insufficiente|Questa situazione si verifica in genere quando ci sono partizioni che occupano lo spazio disponibile nell'unità. <br><br>**Azione**: eliminare tutti i volumi nell'unità, cancellando tutti i dati nell'unità. A tale scopo, è possibile usare il cmdlet di PowerShell [Clear-Disk](https://docs.microsoft.com/powershell/module/storage/clear-disk?view=win10-ps) .|
|Verifica in corso|Il [servizio integrità](../../failover-clustering/health-service-overview.md#supported-components-document) controlla se l'unità o il firmware dell'unità è approvato per l'uso da parte dell'amministratore del server.|
|Verifica non riuscita|Il [servizio integrità](../../failover-clustering/health-service-overview.md#supported-components-document) non è riuscito a verificare se l'unità o il firmware dell'unità è approvato per l'uso da parte dell'amministratore del server.|
|Firmware non conforme|Il firmware dell'unità fisica non è incluso nell'elenco delle revisioni del firmware approvate specificate dall'amministratore del server tramite il [servizio integrità](../../failover-clustering/health-service-overview.md#supported-components-document). |
|Hardware non conforme|L'unità non è inclusa nell'elenco dei modelli di archiviazione approvati specificati dall'amministratore del server tramite il [servizio integrità](../../failover-clustering/health-service-overview.md#supported-components-document).|

## <a name="see-also"></a>Vedere anche

- [Spazi di archiviazione diretta](storage-spaces-direct-overview.md)
- [Requisiti hardware Spazi di archiviazione diretta](storage-spaces-direct-hardware-requirements.md)
- [Informazioni sul quorum di cluster e pool](understand-quorum.md)