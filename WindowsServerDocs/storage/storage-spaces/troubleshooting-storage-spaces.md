---
title: Risoluzione dei problemi di Spazi di archiviazione diretta
description: Informazioni su come risolvere i problemi relativi alla distribuzione di Spazi di archiviazione diretta.
ms.prod: windows-server
ms.author: ''
ms.technology: storage-spaces
ms.topic: article
author: kaushika-msft
ms.date: 10/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: 429eddf30fddf6bfd035d1f928196a3b66d14646
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820944"
---
# <a name="troubleshoot-storage-spaces-direct"></a>Risoluzione dei problemi Spazi di archiviazione diretta

> Si applica a: Windows Server 2019, Windows Server 2016

Usare le informazioni seguenti per risolvere i problemi relativi alla distribuzione di Spazi di archiviazione diretta.

In generale, iniziare con i passaggi seguenti:

1. Confermare che la marca/modello dell'unità SSD è certificata per Windows Server 2016 e Windows Server 2019 usando il catalogo di Windows Server. Verificare con il fornitore che le unità siano supportate per Spazi di archiviazione diretta.
2. Esaminare lo spazio di archiviazione per individuare eventuali unità difettose. Usare il software di gestione dell'archiviazione per verificare lo stato delle unità. Se una delle unità è difettosa, collaborare con il fornitore. 
3. Aggiornare l'archiviazione e il firmware dell'unità se necessario.
   Verificare che gli aggiornamenti di Windows più recenti siano installati in tutti i nodi. È possibile ottenere gli aggiornamenti più recenti per Windows Server 2016 dalla cronologia degli aggiornamenti di [Windows 10 e Windows server 2016](https://aka.ms/update2016) e per windows server 2019 dalla cronologia degli aggiornamenti di [Windows 10 e Windows Server 2019](https://support.microsoft.com/help/4464619).
4. Aggiornare i driver e il firmware delle schede di rete.
5. Eseguire la convalida del cluster e verificare la sezione spazio di archiviazione diretta, verificare che le unità che verranno usate per la cache vengano segnalate correttamente e che non siano presenti errori.

Se si verificano ancora problemi, rivedere gli scenari seguenti.

## <a name="virtual-disk-resources-are-in-no-redundancy-state"></a>Le risorse del disco virtuale non sono in stato di ridondanza
I nodi di un Spazi di archiviazione diretta riavvio del sistema in modo imprevisto a causa di un arresto anomalo o di alimentazione. Quindi, uno o più dischi virtuali potrebbero non essere online e viene visualizzata la descrizione "informazioni di ridondanza insufficiente".

|FriendlyName|ResiliencySettingName| OperationalStatus| HealthStatus| IsManualAttach|Dimensione| PSComputerName|
|------------|---------------------| -----------------| ------------| --------------|-----| --------------|
|DISK4| Mirror| OK|  Integro| True|  10 TB|  Node-01. conto...|
|Disk3         |Mirror                 |OK                          |Integro       |True            |10 TB | Node-01. conto...|
|Disk2         |Mirror                 |Nessuna ridondanza               |Non integro     |True            |10 TB | Node-01. conto...|
|Disk1         |Mirror                 |{Nessuna ridondanza, inservice}  |Non integro     |True            |10 TB | Node-01. conto...| 

Inoltre, dopo il tentativo di portare online il disco virtuale, le informazioni seguenti vengono registrate nel log del cluster (DiskRecoveryAction).  

```
[Verbose] 00002904.00001040::YYYY/MM/DD-12:03:44.891 INFO [RES] Physical Disk <DiskName>: OnlineThread: SuGetSpace returned 0.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 WARN [RES] Physical Disk < DiskName>: Underlying virtual disk is in 'no redundancy' state; its volume(s) may fail to mount.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 ERR [RES] Physical Disk <DiskName>: Failing online due to virtual disk in 'no redundancy' state. If you would like to attempt to online the disk anyway, first set this resource's private property 'DiskRecoveryAction' to 1. We will try to bring the disk online for recovery, but even if successful, its volume(s) or CSV may be unavailable. 
``` 

Se un disco non è riuscito o se il sistema non è in grado di accedere ai dati nel disco virtuale, è possibile che non si verifichi **alcuna ridondanza operativa** . Questo problema può verificarsi se si verifica un riavvio in un nodo durante la manutenzione dei nodi.

Per risolvere il problema, attenersi alla seguente procedura:

1. Rimuovere i dischi virtuali interessati dal volume condiviso del cluster. In questo modo verranno inseriti nel gruppo "spazio di archiviazione disponibile" nel cluster e l'inizio verrà visualizzato come un ResourceType di "disco fisico".

   ```powershell
   Remove-ClusterSharedVolume -name "VdiskName"
   ``` 
2. Nel nodo proprietario del gruppo di archiviazione disponibile, eseguire il comando seguente in ogni disco che non è in uno stato di ridondanza. Per identificare il nodo in cui si trova il gruppo "available storage", è possibile eseguire il comando seguente.

   ```powershell
   Get-ClusterGroup
   ```
3. Impostare l'azione di ripristino del disco e quindi avviare il disco o i dischi.
   ```powershell
   Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 1
   Start-ClusterResource -Name "VdiskName"
   ```
4. Il ripristino dovrebbe avviarsi automaticamente. Attendere il completamento del ripristino. Potrebbe entrare in uno stato di sospensione e riavviarsi. Per monitorare lo stato di avanzamento: 
    - Eseguire **Get-StorageJob** per monitorare lo stato della riparazione e per vedere quando è stata completata.
    - Eseguire **Get-VirtualDisk** e verificare che lo spazio restituisca un HealthStatus integro.
5. Al termine del ripristino e i dischi virtuali sono integri, modificare di nuovo i parametri del disco virtuale.

   ```powershell
    Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 0
   ```
6. Portare i dischi offline e quindi di nuovo online per rendere effettive le DiskRecoveryAction:

   ```powershell
   Stop-ClusterResource "VdiskName"
   Start-ClusterResource "VdiskName"
   ``` 
7. Aggiungere di nuovo i dischi virtuali interessati a CSV.

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```

**DiskRecoveryAction** è un'opzione di override che consente il fissaggio del volume di spazio in modalità di lettura/scrittura senza controlli. La proprietà consente di diagnosticare i motivi per cui un volume non sarà in linea. È molto simile alla modalità manutenzione, ma è possibile richiamarlo su una risorsa in uno stato di errore. Consente inoltre di accedere ai dati, che possono essere utili in situazioni quali "nessuna ridondanza", dove è possibile ottenere l'accesso a qualsiasi dato che è possibile e copiarlo. La proprietà DiskRecoveryAction è stata aggiunta nel 22 febbraio 2018, aggiornamento, KB 4077525.


## <a name="detached-status-in-a-cluster"></a>Stato scollegato in un cluster 

Quando si esegue il cmdlet **Get-VirtualDisk** , il OperationalStatus per uno o più dischi virtuali spazi di archiviazione diretta viene scollegato. Tuttavia, il HealthStatus segnalato dal cmdlet **Get-PhysicalDisk** indica che tutti i dischi fisici sono in uno stato integro.

Di seguito è riportato un esempio dell'output del cmdlet **Get-VirtualDisk** .

|FriendlyName|  ResiliencySettingName|  OperationalStatus|   HealthStatus|  IsManualAttach|  Dimensione|   PSComputerName|
|-|-|-|-|-|-|-|
|DISK4|         Mirror|                 OK|                  Integro|       True|            10 TB|  Node-01. conto...|
|Disk3|         Mirror|                 OK|                  Integro|       True|            10 TB|  Node-01. conto...|
|Disk2|         Mirror|                 Detached|            Sconosciuto|       True|            10 TB|  Node-01. conto...|
|Disk1|         Mirror|                 Detached|            Sconosciuto|       True|            10 TB|  Node-01. conto...| 


Inoltre, è possibile che nei nodi siano registrati gli eventi seguenti:

```
Log Name: Microsoft-Windows-StorageSpaces-Driver/Operational
Source: Microsoft-Windows-StorageSpaces-Driver 
Event ID: 311 
Level: Error
User: SYSTEM 
Computer: Node#.contoso.local 
Description: Virtual disk {GUID} requires a data integrity scan.  

Data on the disk is out-of-sync and a data integrity scan is required. 

To start the scan, run the following command:   
Get-ScheduledTask -TaskName "Data Integrity Scan for Crash Recovery" | Start-ScheduledTask                

Once you have resolved the condition listed above, you can online the disk by using the following commands in PowerShell:   

Get-VirtualDisk | ?{ $_.ObjectId -Match "{GUID}" } | Get-Disk | Set-Disk -IsReadOnly $false 
Get-VirtualDisk | ?{ $_.ObjectId -Match "{GUID}" } | Get-Disk | Set-Disk -IsOffline  $false
------------------------------------------------------------

Log Name: System
Source: Microsoft-Windows-ReFS 
Event ID: 134
Level: Error 
User: SYSTEM
Computer: Node#.contoso.local 
Description: The file system was unable to write metadata to the media backing volume <VolumeId>. A write failed with status "A device which does not exist was specified." ReFS will take the volume offline. It may be mounted again automatically.
------------------------------------------------------------
Log Name: Microsoft-Windows-ReFS/Operational
Source: Microsoft-Windows-ReFS 
Event ID: 5 
Level: Error 
User: SYSTEM 
Computer: Node#.contoso.local 
Description: ReFS failed to mount the volume. 
Context: 0xffffbb89f53f4180 
Error: A device which does not exist was specified.
Volume GUID:{00000000-0000-0000-0000-000000000000} 
DeviceName: 
Volume Name:
``` 

Lo **stato operativo scollegato** può verificarsi se il log di rilevamento dell'area dirty (DRT) è pieno. Spazi di archiviazione usa il rilevamento dell'area dirty (DRT) per gli spazi con mirroring per assicurarsi che, quando si verifica un errore di alimentazione, vengano registrati tutti gli aggiornamenti in corso per i metadati per assicurarsi che lo spazio di archiviazione possa ripristinare o annullare le operazioni per riportare lo spazio di archiviazione in uno stato flessibile e coerente quando viene ripristinata l'alimentazione e il sistema torna attivo. Se il log DRT è pieno, non è possibile portare online il disco virtuale fino a quando i metadati di DRT non vengono sincronizzati e scaricati. Questo processo richiede l'esecuzione di un'analisi completa, il cui completamento può richiedere diverse ore.

Per risolvere il problema, attenersi alla seguente procedura:
1. Rimuovere i dischi virtuali interessati dal volume condiviso del cluster.

   ```powershell
   Remove-ClusterSharedVolume -name "VdiskName"
   ``` 
2. Eseguire i comandi seguenti in ogni disco non in linea. 

   ```powershell
   Get-ClusterResource -Name "VdiskName" | Set-ClusterParameter DiskRunChkDsk 7
   Start-ClusterResource -Name "VdiskName"
   ``` 
3. Eseguire il comando seguente in ogni nodo in cui il volume scollegato è online. 

   ```powershell
   Get-ScheduledTask -TaskName "Data Integrity Scan for Crash Recovery" | Start-ScheduledTask 
   ```
   Questa attività deve essere avviata in tutti i nodi in cui il volume scollegato è online. Il ripristino dovrebbe avviarsi automaticamente. Attendere il completamento del ripristino. Potrebbe entrare in uno stato di sospensione e riavviarsi. Per monitorare lo stato di avanzamento: 
   - Eseguire **Get-StorageJob** per monitorare lo stato della riparazione e per vedere quando è stata completata.
   - Eseguire **Get-VirtualDisk** e verificare che lo spazio restituisca un HealthStatus integro.
     - L'analisi dell'integrità dei dati per il ripristino dell'arresto anomalo del sistema è un'attività che non viene visualizzata come processo di archiviazione e non è presente alcun indicatore di stato. Se l'attività viene visualizzata come in esecuzione, è in esecuzione. Al termine, verrà visualizzato completato.

       Inoltre, è possibile visualizzare lo stato di un'attività di pianificazione in esecuzione utilizzando il cmdlet seguente: 
       ```powershell
       Get-ScheduledTask | ? State -eq running
       ``` 
4. Non appena viene completata l'analisi dell'integrità dei dati per il ripristino dell'arresto anomalo del sistema, il ripristino termina e i dischi virtuali sono integri, modificare di nuovo i parametri del disco virtuale. 

   ```powershell
   Get-ClusterResource -Name "VdiskName" | Set-ClusterParameter DiskRunChkDsk 0 
   ```

5. Portare i dischi offline e quindi di nuovo online per rendere effettive le DiskRecoveryAction:

   ```powershell
   Stop-ClusterResource "VdiskName"
   Start-ClusterResource "VdiskName"
   ``` 

6. Aggiungere di nuovo i dischi virtuali interessati a CSV.    

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```  
   Il **valore DiskRunChkdsk 7** viene usato per alleghi il volume di spazio e la partizione è in modalità di sola lettura. In questo modo è possibile individuare autonomamente gli spazi e risolverli autonomamente attivando un ripristino. Il ripristino verrà eseguito automaticamente una volta montata. Consente inoltre di accedere ai dati, che possono essere utili per ottenere l'accesso a tutti i dati che è possibile copiare. Per alcune condizioni di errore, ad esempio un log DRT completo, è necessario eseguire l'analisi dell'integrità dei dati per l'attività pianificata di ripristino di emergenza.

L' **analisi dell'integrità dei dati per il ripristino di arresto anomalo del** sistema viene utilizzata per sincronizzare e cancellare un log completo di rilevamento dell'area dirty (DRT). Il completamento di questa attività può richiedere diverse ore. L'analisi dell'integrità dei dati per il ripristino dell'arresto anomalo del sistema è un'attività che non viene visualizzata come processo di archiviazione e non è presente alcun indicatore di stato. Se l'attività viene visualizzata come in esecuzione, è in esecuzione. Al termine, verrà visualizzato come completato. Se si annulla l'attività o si riavvia un nodo durante l'esecuzione di questa attività, sarà necessario ricominciare dall'inizio dell'attività.

Per ulteriori informazioni, vedere la pagina relativa alla [risoluzione dei problemi spazi di archiviazione diretta stato operativo](storage-spaces-states.md).

## <a name="event-5120-with-status_io_timeout-c00000b5"></a>Evento 5120 con STATUS_IO_TIMEOUT c00000b5 

> [!Important]
> **Per Windows Server 2016:** Per ridurre la possibilità di riscontrare questi sintomi durante l'applicazione dell'aggiornamento con la correzione, è consigliabile usare la procedura relativa alla modalità di manutenzione dell'archiviazione riportata di seguito per installare il [18 ottobre 2018, aggiornamento cumulativo per Windows server 2016](https://support.microsoft.com/help/4462928) o versione successiva quando i nodi hanno attualmente installato un aggiornamento cumulativo di windows server 2016 rilasciato dall' [8 maggio 2018](https://support.microsoft.com/help/4103723) al [9 ottobre 2018](https://support.microsoft.com/help/KB4462917).

È possibile ottenere l'evento 5120 con STATUS_IO_TIMEOUT c00000b5 dopo il riavvio di un nodo in Windows Server 2016 con aggiornamento cumulativo rilasciato dall' [8 maggio 2018 kb 4103723](https://support.microsoft.com/help/4103723) al [9 ottobre, 2018 KB 4462917](https://support.microsoft.com/help/4462917) installati.

Quando si riavvia il nodo, l'evento 5120 viene registrato nel registro eventi di sistema e include uno dei codici di errore seguenti:

```
Event Source: Microsoft-Windows-FailoverClustering
Event ID: 5120
Description:    Cluster Shared Volume 'CSVName' ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_IO_TIMEOUT(c00000b5)'. All I/O will temporarily be queued until a path to the volume is reestablished. 

Cluster Shared Volume 'CSVName' ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_CONNECTION_DISCONNECTED(c000020c)'. All I/O will temporarily be queued until a path to the volume is reestablished.    
```

Quando viene registrato un evento 5120, viene generato un dump attivo per raccogliere informazioni di debug che possono causare sintomi aggiuntivi o avere un effetto sulle prestazioni. La generazione del dump attivo crea una breve pausa per consentire l'esecuzione di uno snapshot della memoria per la scrittura del file di dump. I sistemi con una grande quantità di memoria e sottoposti a stress possono causare l'eliminazione dei nodi dall'appartenenza al cluster e la registrazione dell'evento 1135 seguente.

```
Event source: Microsoft-Windows-FailoverClustering
Event ID: 1135  
Description: Cluster node 'NODENAME'was removed from the active failover cluster membership. The Cluster service on this node may have stopped. This could also be due to the node having lost communication with other active nodes in the failover cluster. Run the Validate a Configuration wizard to check your network configuration. If the condition persists, check for hardware or software errors related to the network adapters on this node. Also check for failures in any other network components to which the node is connected such as hubs, switches, or bridges.
```

Una modifica introdotta nell'8 maggio 2018 a Windows Server 2016, che è un aggiornamento cumulativo per aggiungere handle di resilienza SMB per la Spazi di archiviazione diretta sessioni di rete SMB intra-cluster. Questa operazione è stata eseguita per migliorare la resilienza agli errori di rete temporanei e migliorare il modo in cui RoCE gestisce la congestione della rete. Questi miglioramenti hanno anche aumentato inavvertitamente i timeout quando le connessioni SMB tentano la riconnessione e attendono il timeout quando un nodo viene riavviato. Questi problemi possono influire su un sistema sottoposto a stress. Durante i periodi di inattività non pianificati, anche le pause di i/o fino a 60 secondi sono state osservate mentre il sistema attende il timeout delle connessioni. Per risolvere questo problema, installare il [18 ottobre 2018, aggiornamento cumulativo per Windows Server 2016](https://support.microsoft.com/help/4462928) o versione successiva.

*Nota* Questo aggiornamento allinea i timeout CSV con i timeout di connessione SMB per risolvere il problema. Non implementa le modifiche per disabilitare la generazione del dump Live indicata nella sezione relativa alla soluzione alternativa.

### <a name="shutdown-process-flow"></a>Flusso del processo di arresto:

1. Eseguire il cmdlet Get-VirtualDisk e verificare che il valore di HealthStatus sia integro.
2. Svuotare il nodo eseguendo il cmdlet seguente:

   ```powershell
   Suspend-ClusterNode -Drain
   ```
3. Inserire i dischi del nodo in modalità manutenzione archiviazione eseguendo il cmdlet seguente: 

   ```powershell
   Get-StorageFaultDomain -type StorageScaleUnit | Where-Object {$_.FriendlyName -eq "<NodeName>"} | Enable-StorageMaintenanceMode
   ```
4. Eseguire il cmdlet **Get-PhysicalDisk** e verificare che il valore di OperationalStatus sia in modalità manutenzione.
5. Eseguire il cmdlet **Restart-computer** per riavviare il nodo.
6. Dopo il riavvio del nodo, rimuovere i dischi del nodo dalla modalità di manutenzione dell'archiviazione eseguendo il cmdlet seguente:

   ```powershell
   Get-StorageFaultDomain -type StorageScaleUnit | Where-Object {$_.FriendlyName -eq "<NodeName>"} | Disable-StorageMaintenanceMode
   ```
7. Riprendere il nodo eseguendo il cmdlet seguente:

   ```powershell
   Resume-ClusterNode
   ```
8. Verificare lo stato dei processi di risincronizzazione eseguendo il cmdlet seguente:

   ```powershell
   Get-StorageJob
   ```

### <a name="disabling-live-dumps"></a>Disabilitazione di dump dinamici
Per attenuare l'effetto della generazione di dump in tempo reale su sistemi con una grande quantità di memoria e in condizioni di stress, potrebbe essere necessario disabilitare la generazione del dump Live. Di seguito sono disponibili tre opzioni.

>[!Caution]
>Questa procedura può impedire la raccolta di informazioni diagnostiche che supporto tecnico Microsoft potrebbe dover esaminare questo problema. Un agente di supporto potrebbe dover chiedere di riabilitare la generazione del dump in tempo reale in base a specifici scenari di risoluzione dei problemi.

Esistono due metodi per disabilitare i dump Live, come descritto di seguito.

#### <a name="method-1-recommended-in-this-scenario"></a>Metodo 1 (consigliato in questo scenario)
Per disabilitare completamente tutti i dump, inclusi i dump in tempo reale a livello di sistema, seguire questa procedura:

1. Creare la seguente chiave del registro di sistema: HKLM\System\CurrentControlSet\Control\CrashControl\ForceDumpsDisabled
2. Nella nuova chiave **ForceDumpsDisabled** creare una proprietà REG_DWORD come GuardedHost e quindi impostarne il valore su 0x10000000.
3. Applicare la nuova chiave del registro di sistema a ogni nodo del cluster.

>[!NOTE]
>È necessario riavviare il computer per rendere effettive le modifiche apportate a nl '.

Dopo aver impostato la chiave del registro di sistema, la creazione del dump Live avrà esito negativo e genererà un errore "STATUS_NOT_SUPPORTED".

#### <a name="method-2"></a>Method 2
Per impostazione predefinita, Segnalazione errori Windows consentirà un solo LiveDump per ogni tipo di report per 7 giorni e solo 1 LiveDump per ogni computer per 5 giorni. È possibile modificare questa impostazione impostando le seguenti chiavi del registro di sistema in modo da consentire l'uso di un solo LiveDump del computer per sempre.
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v SystemThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v ComponentThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```

*Nota* Per rendere effettive le modifiche, è necessario riavviare il computer.

### <a name="method-3"></a>Metodo 3
Per disabilitare la generazione del cluster di dump Live, ad esempio quando viene registrato un evento 5120, eseguire il cmdlet seguente:

```powershell
(Get-Cluster).DumpPolicy = ((Get-Cluster).DumpPolicy -band 0xFFFFFFFFFFFFFFFE)
```
Questo cmdlet ha un effetto immediato su tutti i nodi del cluster senza riavvio del computer.

## <a name="slow-io-performance"></a>Rallentamento delle prestazioni di i/o

Se vengono visualizzati rallentamenti delle prestazioni di i/o, controllare se la cache è abilitata nella configurazione del Spazi di archiviazione diretta. 

Esistono due modi per verificare: 


1. Utilizzando il log del cluster. Aprire il log del cluster nell'editor di testo desiderato e cercare "[= = = SBL disks = = =]". Si tratta di un elenco del disco nel nodo in cui è stato generato il log. 

     Disco abilitato per la cache esempio: si noti che lo stato è CacheDiskStateInitializedAndBound ed è presente un GUID presente qui. 

   ```
   [=== SBL Disks ===]
    {26e2e40f-a243-1196-49e3-8522f987df76},3,false,true,1,48,{1ff348f1-d10d-7a1a-d781-4734f4440481},CacheDiskStateInitializedAndBound,1,8087,54,false,false,HGST    ,HUH721010AL4200 ,        7PG3N2ER,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    Cache non abilitata: qui è possibile vedere che non è presente alcun GUID e lo stato è CacheDiskStateNonHybrid. 
    ```
   [=== SBL Disks ===]
    {426f7f04-e975-fc9d-28fd-72a32f811b7d},12,false,true,1,24,{00000000-0000-0000-0000-000000000000},CacheDiskStateNonHybrid,0,0,0,false,false,HGST    ,HUH721010AL4200 ,        7PGXXG6C,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    Cache non abilitata: quando tutti i dischi sono dello stesso tipo case non è abilitato per impostazione predefinita. Qui è possibile notare che non è presente alcun GUID e lo stato è CacheDiskStateIneligibleDataPartition. 
    ```
    {d543f90c-798b-d2fe-7f0a-cb226c77eeed},10,false,false,1,20,{00000000-0000-0000-0000-000000000000},CacheDiskStateIneligibleDataPartition,0,0,0,false,false,NVMe    ,INTEL SSDPE7KX02,  PHLF7330004V2P0LGN,0170,{79b4d631-976f-4c94-a783-df950389fd38},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0], 
    ```  
2. Uso di Get-PhysicalDisk. XML da SDDCDiagnosticInfo
    1. Aprire il file XML con "$d = Import-Clixml GetPhysicalDisk. XML"
    2. Eseguire "IPMO storage"
    3. eseguire "$d". Si noti che l'utilizzo è selezione automatica, non Journal. verrà visualizzato un output simile al seguente: 

   |FriendlyName|  SerialNumber| MediaType| CanPool| OperationalStatus| HealthStatus| Utilizzo| Dimensione|
   |-----------|------------|---------| -------| -----------------| ------------| -----| ----|
   |NVMe INTEL SSDPE7KX02| PHLF733000372P0LGN| SSD| False|   OK|                Integro|      Selezionare automaticamente 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7504008J2P0LGN| SSD|  False|    OK|                Integro| Selezione automatica| 1,82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7504005F2P0LGN| SSD|  False|  OK|                Integro| Selezione automatica| 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7504002A2P0LGN| SSD| False| OK|    Integro| Selezione automatica| 1,82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7504004T2P0LGN |SSD| False|OK|       Integro| Selezione automatica| 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7504002E2P0LGN| SSD| False| OK|      Integro| Selezione automatica| 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7330002Z2P0LGN| SSD| False| OK|      Integro|Selezione automatica| 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF733000272P0LGN |SSD| False| OK|  Integro| Selezione automatica| 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7330001J2P0LGN |SSD| False| OK| Integro| Selezione automatica| 1,82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF733000302P0LGN |SSD| False| OK|Integro| Selezione automatica| 1,82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7330004D2P0LGN |SSD| False| OK| Integro| Selezione automatica |1,82 TB|

## <a name="how-to-destroy-an-existing-cluster-so-you-can-use-the-same-disks-again"></a>Come eliminare un cluster esistente per poter usare di nuovo gli stessi dischi

In un cluster di Spazi di archiviazione diretta, dopo aver disattivato Spazi di archiviazione diretta e aver usato il processo di pulizia descritto in [unità pulite](deploy-storage-spaces-direct.md#step-31-clean-drives), il pool di archiviazione in cluster rimane in uno stato offline e il servizio integrità viene rimosso dal cluster.

Il passaggio successivo consiste nel rimuovere il pool di archiviazione fantasma: 
   ```powershell
   Get-ClusterResource -Name "Cluster Pool 1" | Remove-ClusterResource
   ```

A questo punto, se si esegue **Get-PhysicalDisk** su uno qualsiasi dei nodi, verranno visualizzati tutti i dischi presenti nel pool. Ad esempio, in un Lab con un cluster a 4 nodi con 4 dischi SAS, 100 GB ciascuno presentati a ogni nodo. In tal caso, dopo che lo spazio di archiviazione diretto è disabilitato, che rimuove il SBL (livello del bus di archiviazione) ma lascia il filtro, se si esegue **Get-PhysicalDisk**, dovrebbe segnalare 4 dischi, escluso il disco del sistema operativo locale. Invece è stato segnalato 16. Questo è lo stesso per tutti i nodi del cluster. Quando si esegue un comando **Get-disk** , verranno visualizzati i dischi collegati localmente numerati come 0, 1, 2 e così via, come illustrato in questo esempio di output:

|Numero| Soprannome| Numero di serie|HealthStatus|OperationalStatus|Dimensione totale| Stile partizione|
|-|-|-|-|-|-|-|-|
|0|Virtu MSFT...  ||Integro | Online|  127 GB| GPT|
||Virtu MSFT... ||Integro| Offline| 100 GB| RAW|
||Virtu MSFT... ||Integro| Offline| 100 GB| RAW|
||Virtu MSFT... ||Integro| Offline| 100 GB| RAW|
||Virtu MSFT... ||Integro| Offline| 100 GB| RAW|
|1|Virtu MSFT...||Integro| Offline| 100 GB| RAW|
||Virtu MSFT... ||Integro| Offline| 100 GB| RAW|
|2|Virtu MSFT...||Integro| Offline| 100 GB| RAW|
||Virtu MSFT... ||Integro| Offline| 100 GB| RAW|
||Virtu MSFT... ||Integro| Offline| 100 GB| RAW|
||Virtu MSFT... ||Integro| Offline| 100 GB| RAW|
||Virtu MSFT... ||Integro| Offline| 100 GB| RAW|
|4|Virtu MSFT...||Integro| Offline| 100 GB| RAW|
|3|Virtu MSFT...||Integro| Offline| 100 GB| RAW|
||Virtu MSFT... ||Integro| Offline| 100 GB| RAW|
||Virtu MSFT... ||Integro| Offline| 100 GB| RAW|
||Virtu MSFT... ||Integro| Offline| 100 GB| RAW|


## <a name="error-message-about-unsupported-media-type-when-you-create-an-storage-spaces-direct-cluster-using-enable-clusters2d"></a>Messaggio di errore relativo al tipo di supporto non supportato quando si crea un cluster di Spazi di archiviazione diretta usando enable-ClusterS2D  

Quando si esegue il cmdlet **Enable-ClusterS2D** , è possibile che vengano visualizzati errori simili ai seguenti:

![Messaggio di errore dello scenario 6](media/troubleshooting/scenario-error-message.png)

Per risolvere questo problema, assicurarsi che la scheda HBA sia configurata in modalità HBA. Non è necessario configurare HBA in modalità RAID.  

## <a name="enable-clusterstoragespacesdirect-hangs-at-waiting-until-sbl-disks-are-surfaced-or-at-27"></a>Enable-ClusterStorageSpacesDirect si blocca in attesa fino a quando i dischi SBL sono esposti o al 27%

Nel report di convalida vengono visualizzate le informazioni seguenti:

    Disk <identifier> connected to node <nodename> returned a SCSI Port Association and the corresponding enclosure device could not be found. The hardware is not compatible with Storage Spaces Direct (S2D), contact the hardware vendor to verify support for SCSI Enclosure Services (SES). 


Il problema riguarda la scheda espansore SAS HPE che si trova tra i dischi e la scheda HBA. L'espansore SAS crea un ID duplicato tra la prima unità connessa all'espansore e l'espansore stesso.  Questo problema è stato risolto in [HPE Smart Array Controllers SAS Expander firmware: 4,02](https://support.hpe.com/hpsc/swd/public/detail?sp4ts.oid=7304566&swItemId=MTX_ef8d0bf4006542e194854eea6a&swEnvOid=4184#tab3).

## <a name="intel-ssd-dc-p4600-series-has-a-non-unique-nguid"></a>La serie P4600 di Intel SSD DC ha un NGUID non univoco
Potrebbe essere visualizzato un problema per cui un dispositivo Intel SSD DC P4600 Series sembra segnalare un NGUID simile a 16 byte per più spazi dei nomi, ad esempio 0100000001000000E4D25C000014E214 o 0100000001000000E4D25C0000EEE214 nell'esempio riportato di seguito.


|               UniqueId               | DeviceID | MediaType | BusType |               SerialNumber               |      size      | canpool | FriendlyName | OperationalStatus |
|--------------------------------------|----------|-----------|---------|------------------------------------------|----------------|---------|--------------|-------------------|
|           5000CCA251D12E30           |    0     |    Unità disco rigido    |   SAS   |                 7PKR197G                 | 10000831348736 |  False  |     HGST     |  HUH721010AL4200  |
| EUI. 0100000001000000E4D25C000014E214 |    4     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_0014_E214. | 1600321314816  |  True   |    INTEL     |   SSDPE2KE016T7   |
| EUI. 0100000001000000E4D25C000014E214 |    5     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_0014_E214. | 1600321314816  |  True   |    INTEL     |   SSDPE2KE016T7   |
| EUI. 0100000001000000E4D25C0000EEE214 |    6     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_00EE_E214. | 1600321314816  |  True   |    INTEL     |   SSDPE2KE016T7   |
| EUI. 0100000001000000E4D25C0000EEE214 |    7     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_00EE_E214. | 1600321314816  |  True   |    INTEL     |   SSDPE2KE016T7   |

Per risolvere questo problema, aggiornare il firmware delle unità Intel alla versione più recente.  La versione del firmware QDV101B1 del 2018 maggio è nota per risolvere questo problema.

La [versione di maggio 2018 dello strumento Intel SSD Data Center](https://downloadmirror.intel.com/27778/eng/Intel_SSD_Data_Center_Tool_3_0_12_Release_Notes_330715-026.pdf) include un aggiornamento del firmware, QDV101B1, per la serie Intel SSD DC P4600.


## <a name="physical-disk-healthy-and-operational-status-is-removing-from-pool"></a>Il disco fisico "integro" e lo stato operativo è "rimozione dal pool" 

In un cluster Spazi di archiviazione diretta Windows Server 2016, potrebbe essere visualizzato il HealthStatus per uno o più dischi fisici come "integro", mentre OperationalStatus è "(rimozione dal pool, OK)". 

"Rimozione dal pool" è un set di intenti quando **Remove-PhysicalDisk** viene chiamato ma archiviato in integrità per mantenere lo stato e consentire il ripristino in caso di errore dell'operazione di rimozione. È possibile modificare manualmente il OperationalStatus in integro con uno dei metodi seguenti:

- Rimuovere il disco fisico dal pool, quindi aggiungerlo di nuovo.
- Eseguire lo [script Clear-PhysicalDiskHealthData. ps1](https://go.microsoft.com/fwlink/?linkid=2034205) per cancellare lo scopo. (Disponibile per il download come. File TXT. È necessario salvarlo come. File PS1 prima di poterlo eseguire.

Di seguito sono riportati alcuni esempi che illustrano come eseguire lo script:

- Usare il parametro **serialNumber** per specificare il disco che è necessario impostare su integro. È possibile ottenere il numero di serie da **WMI MSFT_PhysicalDisk** o **Get-PhysicalDisk**. (Stiamo usando solo gli zeri per il numero di serie riportato di seguito).

   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -SerialNumber 000000000000000 -Verbose -Force
    ```

- Usare il parametro **UniqueId** per specificare il disco (di nuovo da **WMI MSFT_PhysicalDisk** o **Get-PhysicalDisk**).

   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -UniqueId 00000000000000000 -Verbose -Force
   ```

## <a name="file-copy-is-slow"></a>La copia del file è lenta

Si potrebbe riscontrare un problema con Esplora file per copiare un disco rigido virtuale di grandi dimensioni nel disco virtuale. la copia del file richiede più tempo del previsto.

L'uso di Esplora file, Robocopy o Xcopy per copiare un disco rigido virtuale di grandi dimensioni nel disco virtuale, non è un metodo consigliato perché questa operazione comporterà un rallentamento delle prestazioni previste. Il processo di copia non passa attraverso lo stack di Spazi di archiviazione diretta, che si trova più in basso nello stack di archiviazione e agisce invece come un processo di copia locale.

Se si desidera testare le prestazioni di Spazi di archiviazione diretta, è consigliabile usare VMFleet e Diskspd per caricare e testare i server in modo da ottenere una linea di base e impostare le aspettative per le prestazioni di Spazi di archiviazione diretta.

## <a name="expected-events-that-you-would-see-on-rest-of-the-nodes-during-the-reboot-of-a-node"></a>Eventi previsti che verrebbero visualizzati sul resto dei nodi durante il riavvio di un nodo. 

Ignorare questi eventi è sicuro: 

    Event ID 205: Windows lost communication with physical disk {XXXXXXXXXXXXXXXXXXXX }. This can occur if a cable failed or was disconnected, or if the disk itself failed. 

    Event ID 203: Windows lost communication with physical disk {xxxxxxxxxxxxxxxxxxxxxxxx }. This can occur if a cable failed or was disconnected, or if the disk itself failed. 

Se si eseguono macchine virtuali di Azure, è possibile ignorare questo evento:

    Event ID 32: The driver detected that the device \Device\Harddisk5\DR5 has its write cache enabled. Data corruption may occur. 

## <a name="slow-performance-or-lost-communication-io-error-detached-or-no-redundancy-errors-for-deployments-that-use-intel-p3x00-nvme-devices"></a>Rallentamento delle prestazioni o "perdita di comunicazione", "errore di i/o", "scollegato" o "assenza di ridondanza" per le distribuzioni che usano dispositivi Intel P3x00 NVMe

È stato identificato un problema critico che interessa alcuni Spazi di archiviazione diretta utenti che usano l'hardware basato sulla famiglia Intel P3x00 di dispositivi NVM Express (NVMe) con versioni del firmware prima di "manutenzione versione 8". 

>[!NOTE]
> I singoli OEM possono avere dispositivi basati sulla famiglia Intel P3x00 di dispositivi NVMe con stringhe di versione del firmware univoche. Per ulteriori informazioni sulla versione più recente del firmware, contattare l'OEM.

Se si usa l'hardware nella distribuzione basata sulla famiglia di dispositivi NVMe Intel P3x00, è consigliabile applicare immediatamente il firmware più recente disponibile (almeno la versione di manutenzione 8). Questo [articolo supporto tecnico Microsoft](https://support.microsoft.com/help/4052341/slow-performance-or-lost-communication-io-error-detached-or-no-redunda) fornisce informazioni aggiuntive su questo problema. 
