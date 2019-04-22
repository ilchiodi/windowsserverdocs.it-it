---
title: Risoluzione dei diretto di spazi di archiviazione
description: Informazioni su come risolvere i problemi di distribuzione di spazi di archiviazione diretta.
keywords: Spazi di archiviazione
ms.prod: windows-server-threshold
ms.author: ''
ms.technology: storage-spaces
ms.topic: article
author: kaushika-msft
ms.date: 10/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: ecf3cb5703a90976dce15abbd0c9fdd1d4aa24ec
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812632"
---
# <a name="troubleshoot-storage-spaces-direct"></a>Risolvere i problemi di spazi di archiviazione diretta

Usare le informazioni seguenti per risolvere i problemi di distribuzione di spazi di archiviazione diretta.

In generale, iniziare con la procedura seguente:

1. Confermare che il marca o modello di unità SSD è certificato per Windows Server 2016 tramite il catalogo di Windows Server. Verificare il fornitore che sono supportate le unità per spazi di archiviazione diretta.
2. Controllare lo spazio di archiviazione per tutte le unità difettose. Usare il software di gestione di archiviazione per controllare lo stato delle unità. Se una delle unità sono difettosi, rivolgersi al fornitore. 
3. Aggiornare l'archiviazione e unità del firmware se necessario.
   Verificare che gli ultimi aggiornamenti di Windows sono installati in tutti i nodi. È possibile ottenere gli aggiornamenti più recenti per Windows Server 2016 da [ https://aka.ms/update2016 ](https://aka.ms/update2016).
4. Aggiornare il firmware e driver di schede di rete.
5. Eseguire la convalida del cluster e vedere la sezione dello spazio di archiviazione diretta, assicurarsi che le unità che verranno utilizzate per la cache vengono segnalate correttamente e senza errori.

Se si verificano ancora problemi, esaminare gli scenari seguenti.

## <a name="virtual-disk-resources-are-in-no-redundancy-state"></a>Le risorse del disco virtuale sono in stato di ridondanza n
I nodi di un sistema spazi di archiviazione diretta riavviato in modo imprevisto a causa di un errore di arresto anomalo del sistema o all'alimentazione. Quindi, uno o più dei dischi virtuali che non vengano portate online e noterete che la descrizione "informazioni non sono sufficienti ridondanza".
    
|FriendlyName|ResiliencySettingName| OperationalStatus| HealthStatus| IsManualAttach|Dimensione| PSComputerName|
|------------|---------------------| -----------------| ------------| --------------|-----| --------------|
|Disk4| Mirror| OK|  Integro| True|  10 TB|  Node-01.conto...|
|Disk3         |Mirror                 |OK                          |Integro       |True            |10 TB | Node-01.conto...|
|Disk2         |Mirror                 |Senza ridondanza               |Unhealthy     |True            |10 TB | Node-01.conto...|
|Disk1         |Mirror                 |{No Redundancy, InService}  |Unhealthy     |True            |10 TB | Node-01.conto...| 

Inoltre, dopo un tentativo di portare online il disco virtuale, le informazioni seguenti viene registrate nel registro del Cluster (DiskRecoveryAction).  

```
[Verbose] 00002904.00001040::YYYY/MM/DD-12:03:44.891 INFO [RES] Physical Disk <DiskName>: OnlineThread: SuGetSpace returned 0.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 WARN [RES] Physical Disk < DiskName>: Underlying virtual disk is in 'no redundancy' state; its volume(s) may fail to mount.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 ERR [RES] Physical Disk <DiskName>: Failing online due to virtual disk in 'no redundancy' state. If you would like to attempt to online the disk anyway, first set this resource's private property 'DiskRecoveryAction' to 1. We will try to bring the disk online for recovery, but even if successful, its volume(s) or CSV may be unavailable. 
``` 

Il **stato operativo ridondanza n** può verificarsi se un disco non è riuscita o se il sistema è in grado di accedere ai dati nel disco virtuale. Questo problema può verificarsi se si verifica un riavvio in un nodo durante la manutenzione sui nodi.
    
Per risolvere questo problema, seguire questa procedura:

1. Rimuovere i dischi virtuali interessate da file CSV. Questo verrà inserirle nel gruppo "Archiviazione disponibile" nel cluster e la visualizzazione come un tipo di risorsa del "Disco fisico."

   ```powershell
   Remove-ClusterSharedVolume -name "VdiskName"
   ``` 
2. Nel nodo proprietario del gruppo di risorse di archiviazione disponibili, eseguire il comando seguente in tutti i dischi che si trova in uno stato di ridondanza n. Per identificare quale nodo è il gruppo "Archiviazione disponibile" per l'utente può eseguire il comando seguente.

   ```powershell
   Get-ClusterGroup
   ```
3. Impostare l'azione di ripristino del disco e avviare i dischi.
   ```powershell
   Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 1
   Start-ClusterResource -Name "VdiskName"
   ```
4. Un ripristino dovrebbe essere avviato automaticamente. Attendere il ripristino alla fine. Potrebbe entrare in uno stato sospeso e avviare di nuovo. Per monitorare lo stato di avanzamento: 
    - Eseguire **Get-StorageJob** per monitorare lo stato del ripristino e vedere quando è completata.
    - Eseguire **Get-VirtualDisk** e verificare che lo spazio restituisca HealthStatus integro.
5. Dopo il ripristino al termine della e i dischi virtuali vengono integro, tornare ai parametri di disco virtuale.

   ```powershell
    Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 0
   ```
6. Seguire i dischi in modalità offline e quindi online nuovamente per avere la DiskRecoveryAction effettive:

   ```powershell
   Stop-ClusterResource "VdiskName"
   Start-ClusterResource "VdiskName"
   ``` 
7. Aggiungere i dischi virtuali interessate nuovamente in formato CSV.

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```

**DiskRecoveryAction** è un'opzione di override che consente di collegare il volume di spazio in modalità lettura / scrittura senza alcun controllo. La proprietà consente di eseguire la diagnostica in perché un volume non verrà portato online. È molto simile alla modalità di manutenzione, ma è possibile richiamarlo su una risorsa in uno stato di errore. Consente inoltre di accedere ai dati, che possono essere utili nelle situazioni, ad esempio "N ridondanza", dove è possibile ottenere l'accesso a tutti i dati è possibile e copiarlo. La proprietà DiskRecoveryAction è stato aggiunto nel 22 febbraio 2018, update, 4077525 KB.
    
    
## <a name="detached-status-in-a-cluster"></a>Stato scollegato in un cluster 

Quando si esegue la **Get-VirtualDisk** cmdlet, il OperationalStatus uno o più spazi di archiviazione diretta è scollegare i dischi virtuali. Tuttavia, il HealthStatus segnalati dal **Get-PhysicalDisk** cmdlet indica che tutti i dischi fisici siano in uno stato integro.

Di seguito è riportato un esempio dell'output di **Get-VirtualDisk** cmdlet.

|FriendlyName|  ResiliencySettingName|  OperationalStatus|   HealthStatus|  IsManualAttach|  Dimensione|   PSComputerName|
|-|-|-|-|-|-|-|
|Disk4|         Mirror|                 OK|                  Integro|       True|            10 TB|  Node-01.conto...|
|Disk3|         Mirror|                 OK|                  Integro|       True|            10 TB|  Node-01.conto...|
|Disk2|         Mirror|                 Detached|            Sconosciuta|       True|            10 TB|  Node-01.conto...|
|Disk1|         Mirror|                 Detached|            Sconosciuta|       True|            10 TB|  Node-01.conto...| 


Inoltre, gli eventi seguenti potrebbero essere registrati nei nodi:

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

Il **scollegati stato operativo** può verificarsi se l'area dirty (DRT) di rilevamento registro è pieno. Spazi di archiviazione utilizza area dirty (DRT) di rilevamento per gli spazi con mirroring per garantire che, quando si verifica un'interruzione dell'alimentazione, eventuali aggiornamenti in corso dei metadati vengono registrati per assicurarsi che lo spazio di archiviazione può ripristinare o annullare le operazioni per riportare lo spazio di archiviazione in un flessibili e lo stato coerente quando l'alimentazione viene ripristinata e il sistema torna allo stato attivo. Se il log DRT è pieno, il disco virtuale non possa essere portato online fino a quando la variabile metadata DRT sincronizzata e scaricata. Questo processo richiede l'esecuzione di un'analisi completa, che può richiedere diverse ore.
    
Per risolvere questo problema, seguire questa procedura:
1. Rimuovere i dischi virtuali interessate da file CSV.

   ```powershell
   Remove-ClusterSharedVolume -name "VdiskName"
   ``` 
2. Eseguire i comandi seguenti in tutti i dischi che non viene portata online. 

   ```powershell
   Get-ClusterResource -Name "VdiskName" | Set-ClusterParameter DiskRunChkDsk 7
   Start-ClusterResource -Name "VdiskName"
   ``` 
3. Eseguire il comando seguente in ogni nodo in cui è in linea il volume scollegato. 

   ```powershell
   Get-ScheduledTask -TaskName "Data Integrity Scan for Crash Recovery" | Start-ScheduledTask 
   ```
   Questa attività deve essere avviata in tutti i nodi in cui il volume disconnesso è online. Un ripristino dovrebbe essere avviato automaticamente. Attendere il ripristino alla fine. Potrebbe entrare in uno stato sospeso e avviare di nuovo. Per monitorare lo stato di avanzamento: 
   - Eseguire **Get-StorageJob** per monitorare lo stato del ripristino e vedere quando è completata.
   -  Eseguire **Get-VirtualDisk** e verificare lo spazio restituisce HealthStatus integro.
    - Il "ripristino dei dati integrità analizza per arresto anomalo del sistema" è un'attività venga visualizzato come un processo di archiviazione e non vi è alcun indicatore di stato. Se l'attività viene visualizzato come in esecuzione, è in esecuzione. Al termine, verrà visualizzato completato.
    
       Inoltre, è possibile visualizzare lo stato di un'attività di pianificazione in esecuzione usando il cmdlet seguente: 
       ```powershell
       Get-ScheduledTask | ? State -eq running
       ``` 
4. Appena viene completato il "ripristino dei dati integrità analizza per arresto anomalo del sistema", al termine del ripristino e i dischi virtuali vengono integro, tornare ai parametri di disco virtuale. 

   ```powershell
   Get-ClusterResource -Name "VdiskName" | Set-ClusterParameter DiskRunChkDsk 0 
   ```
5. Aggiungere i dischi virtuali interessate nuovamente in formato CSV.    

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```  
**Valore DiskRunChkdsk 7** viene usato per collegare il volume dello spazio e avere la partizione in modalità sola lettura. In questo modo gli spazi da soli individuare e riparare attivando una correzione. Repair eseguirà automaticamente una volta montata. Consente inoltre di accedere ai dati, che possono essere utili per ottenere l'accesso a tutti i dati è possibile specificare se per copiare. Per alcune condizioni di errore, ad esempio un log DRT completo, è necessario eseguire l'analisi dell'integrità dei dati per l'attività pianificata di arresto anomalo del sistema di ripristino.
    
**Analisi dell'integrità dei dati per attività di ripristino di arresto anomalo** viene usato per sincronizzare e cancellare un log area completo dirty (DRT) di rilevamento. Questa attività può richiedere diverse ore. Il "ripristino dei dati integrità analizza per arresto anomalo del sistema" è un'attività venga visualizzato come un processo di archiviazione e non vi è alcun indicatore di stato. Se l'attività viene visualizzato come in esecuzione, è in esecuzione. Al termine, verrà visualizzato come completata. Se si annulla l'attività o riavvia un nodo durante l'esecuzione di questa attività, sarà necessario ricominciare dall'inizio dell'attività.

Per altre informazioni, vedere [integrità risoluzione dei problemi di spazi di archiviazione diretta e gli stati operativi](storage-spaces-states.md).
    
## <a name="event-5120-with-statusiotimeout-c00000b5"></a>Evento 5120 con STATUS_IO_TIMEOUT c00000b5 

>[! Importante} per ridurre la possibilità di realizzare questi sintomi applicando l'aggiornamento con la correzione, è consigliabile usare la procedura di modalità manutenzione dell'archiviazione seguente per installare il [18 ottobre 2018, aggiornamento cumulativo per Windows Server 2016 ](https://support.microsoft.com/help/4462928) o versione successiva quando i nodi attualmente installato un aggiornamento cumulativo di Windows Server 2016 che è stato rilasciato dal [all'8 maggio 2018](https://support.microsoft.com/help/4103723) al [9 ottobre 2018](https://support.microsoft.com/help/KB4462917).

Si potrebbero ottenere eventi 5120 con STATUS_IO_TIMEOUT c00000b5 dopo il riavvio di un nodo in Windows Server 2016 con aggiornamento cumulativo che sono stati rilasciati dal [all'8 maggio 2018 KB 4103723](https://support.microsoft.com/help/4103723) a [9 ottobre 2018 KB 4462917](https://support.microsoft.com/help/4462917)installato.

Quando si riavvia il nodo, 5120 evento viene registrato nel registro eventi di sistema e include uno dei codici di errore seguenti:

```
Event Source: Microsoft-Windows-FailoverClustering
Event ID: 5120
Description:    Cluster Shared Volume 'CSVName' ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_IO_TIMEOUT(c00000b5)'. All I/O will temporarily be queued until a path to the volume is reestablished. 

Cluster Shared Volume ‘CSVName’ ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_CONNECTION_DISCONNECTED(c000020c)'. All I/O will temporarily be queued until a path to the volume is reestablished.    
```

Quando viene registrato un evento di 5120, viene generato un dump in tempo reale per raccogliere informazioni di debug che potrebbero causare altri sintomi o influire sulla prestazione. Generazione di dump in tempo reale consente di creare una breve pausa per consentire l'esecuzione di uno snapshot di memoria per scrivere il file di dump. I sistemi con grandi quantità di memoria e in condizioni di stress potrebbe essere nodi da eliminare all'esterno dell'appartenenza al cluster e causare anche il seguente 1135 eventi da registrare.

```
Event source: Microsoft-Windows-FailoverClustering
Event ID: 1135  
Description: Cluster node 'NODENAME'was removed from the active failover cluster membership. The Cluster service on this node may have stopped. This could also be due to the node having lost communication with other active nodes in the failover cluster. Run the Validate a Configuration wizard to check your network configuration. If the condition persists, check for hardware or software errors related to the network adapters on this node. Also check for failures in any other network components to which the node is connected such as hubs, switches, or bridges.
```

È stata introdotta una modifica nell'aggiornamento cumulativo di all'8 maggio 2018, per aggiungere gestisce resiliente SMB per le sessioni di rete SMB all'interno del cluster spazi di archiviazione diretta. Ciò permette di migliorare la resilienza agli errori di rete temporanei e migliorare la modalità di gestione traffico di rete intenso RoCE.

Questi miglioramenti aumenta anche inavvertitamente attese a timeout e timeout quando le connessioni SMB tenta di riconnettersi quando un nodo viene riavviato. Questi problemi possono influire su un sistema che è in condizioni di stress. Durante i tempi di inattività non pianificato, le pause dei / o di un massimo di 60 secondi sono anche state osservate quando il sistema è in attesa per le connessioni al timeout.

Per risolvere questo problema, installare il [18 ottobre 2018, aggiornamento cumulativo per Windows Server 2016](https://support.microsoft.com/help/4462928) o versione successiva.

*Nota* questo aggiornamento consente di allineare i valori di timeout CSV con i timeout di connessione di SMB per risolvere il problema. Non implementa le modifiche per disabilitare la generazione di dump in tempo reale indicata nella sezione soluzione alternativa.
    
### <a name="shutdown-process-flow"></a>Flusso del processo di arresto:

1. Eseguire il cmdlet Get-VirtualDisk e assicurarsi che il valore HealthStatus è integro.
2. Svuotare il nodo mediante il cmdlet seguente:

   ```powershell
   Suspend-ClusterNode -Drain
   ```
3. Inserire i dischi in quel nodo in modalità manutenzione dell'archiviazione, eseguire il cmdlet seguente: 

   ```powershell
   Get-StorageFaultDomain -type StorageScaleUnit | Where-Object {$_.FriendlyName -eq "<NodeName>"} | Enable-StorageMaintenanceMode
   ```
4. Eseguire la **Get-PhysicalDisk** cmdlet e assicurarsi che il valore OperationalStatus sia In modalità manutenzione.
5. Eseguire la **Restart-Computer** cmdlet per riavviare il nodo.
6. Dopo il riavvio di nodo, rimuovere i dischi su tale nodo dalla modalità manutenzione dell'archiviazione, eseguire il cmdlet seguente:

   ```powershell
   Get-StorageFaultDomain -type StorageScaleUnit | Where-Object {$_.FriendlyName -eq "<NodeName>"} | Disable-StorageMaintenanceMode
   ```
7. Riprendere il nodo eseguendo il cmdlet seguente:

   ```powershell
   Resume-ClusterNode
   ```
8. Controllare lo stato dei processi di risincronizzazione eseguendo il cmdlet seguente:

   ```powershell
   Get-StorageJob
   ```

### <a name="disabling-live-dumps"></a>La disabilitazione di dump in tempo reale
Per attenuare l'effetto della generazione di dump in tempo reale nei sistemi con grandi quantità di memoria e in condizioni di stress, è possibile inoltre disattivare la generazione di dump in tempo reale. Di seguito vengono fornite tre opzioni.

>[!Caution]
>Questa procedura può impedire la raccolta di informazioni di diagnostica supporto tecnico Microsoft potrebbe essere necessario esaminare il problema. Un agente di supporto potrebbe essere necessario chiedere di abilitare nuovamente la generazione di dump in tempo reale in base agli scenari di risoluzione dei problemi specifici.

Esistono due metodi per disabilitare i dump in tempo reale, come descritto di seguito.

#### <a name="method-1-recommended-in-this-scenario"></a>Metodo 1 (scelta consigliata in questo scenario)
Per disabilitare completamente il dump di tutti, inclusi i dump in tempo reale a livello di sistema, seguire questa procedura:

1. Creare la chiave del Registro di sistema seguente: HKLM\System\CurrentControlSet\Control\CrashControl\ForceDumpsDisabled
2. Sotto la nuova **ForceDumpsDisabled** della chiave, creare una proprietà REG_DWORD come GuardedHost e quindi impostarne il valore su 0x10000000.
3. Applicare la nuova chiave del Registro di sistema per ogni nodo del cluster.

>[!NOTE]
>È necessario riavviare il computer rendere effettiva la modifica NL.

Dopo questa chiave del Registro di sistema è impostato, in tempo reale la creazione di dump avrà esito negativo e generare un errore di "STATUS_NOT_SUPPORTED".

#### <a name="method-2"></a>Method 2
Per impostazione predefinita, la segnalazione errori Windows consentirà LiveDump solo una per ogni tipo di report per ogni 7 giorni e LiveDump solo 1 per ogni macchina per ogni 5 giorni. È possibile modificarlo impostando le seguenti chiavi del Registro di sistema per consentire solo uno LiveDump nel computer per sempre.
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v SystemThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v ComponentThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```

*Nota* è necessario riavviare il computer rendere effettiva la modifica.

### <a name="method-3"></a>Metodo 3
Per disabilitare la generazione di dump in tempo reale (ad esempio, quando viene registrato un evento di 5120) del cluster, eseguire il cmdlet seguente:

```powershell
(Get-Cluster).DumpPolicy = ((Get-Cluster).DumpPolicy -band 0xFFFFFFFFFFFFFFFE)
```
Questo cmdlet ha effetto immediato su tutti i nodi del cluster senza dover riavviare il computer.
    
## <a name="slow-io-performance"></a>Rallentamento delle prestazioni dei / o

Se si riscontrano rallentamento delle prestazioni dei / o, controllare se cache è abilitata nella configurazione di spazi di archiviazione diretta. 

Esistono due modi per controllare: 
     
 
1. Uso del log del cluster. Aprire il registro cluster nell'editor di testo preferito e cercare "[=== dischi SBL ===]." Questo sarà un elenco del disco nel nodo che è stato generato il log. 

     Cache abilitata dischi esempio: Si noti che lo stato è CacheDiskStateInitializedAndBound e vi è un GUID presentano qui. 

   ```
   [=== SBL Disks ===]
    {26e2e40f-a243-1196-49e3-8522f987df76},3,false,true,1,48,{1ff348f1-d10d-7a1a-d781-4734f4440481},CacheDiskStateInitializedAndBound,1,8087,54,false,false,HGST    ,HUH721010AL4200 ,        7PG3N2ER,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    Cache non abilitata: Qui possiamo vedere non è presente alcuna GUID e lo stato è CacheDiskStateNonHybrid. 
    ```
   [=== SBL Disks ===]
    {426f7f04-e975-fc9d-28fd-72a32f811b7d},12,false,true,1,24,{00000000-0000-0000-0000-000000000000},CacheDiskStateNonHybrid,0,0,0,false,false,HGST    ,HUH721010AL4200 ,        7PGXXG6C,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    Cache non abilitata: Quando tutti i dischi sono della custodia del tipo stesso non è abilitato per impostazione predefinita. Qui possiamo vedere non è presente alcuna GUID e lo stato è CacheDiskStateIneligibleDataPartition. 
    ```
    {d543f90c-798b-d2fe-7f0a-cb226c77eeed},10,false,false,1,20,{00000000-0000-0000-0000-000000000000},CacheDiskStateIneligibleDataPartition,0,0,0,false,false,NVMe    ,INTEL SSDPE7KX02,  PHLF7330004V2P0LGN,0170,{79b4d631-976f-4c94-a783-df950389fd38},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0], 
    ```  
2. Usando Get-PhysicalDisk.xml dal SDDCDiagnosticInfo
    1. Aprire il file XML usando "$d = GetPhysicalDisk.XML Import-Clixml"
    2. Eseguire "archiviazione condivisa ipmo"
    3. eseguire "$d". Si noti che l'utilizzo è Auto-Select, non del giornale di registrazione viene visualizzato un output simile al seguente: 

   |FriendlyName|  SerialNumber| MediaType| CanPool| OperationalStatus| HealthStatus| Uso| Dimensione|
   |-----------|------------|---------| -------| -----------------| ------------| -----| ----|
   |NVMe INTEL SSDPE7KX02| PHLF733000372P0LGN| SSD| False|   OK|                Integro|      Auto-Select 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7504008J2P0LGN| SSD|  False|    OK|                Integro| Auto-Select| 1.82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7504005F2P0LGN| SSD|  False|  OK|                Integro| Auto-Select| 1.82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7504002A2P0LGN| SSD| False| OK|    Integro| Auto-Select| 1.82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7504004T2P0LGN |SSD| False|OK|       Integro| Auto-Select| 1.82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7504002E2P0LGN| SSD| False| OK|      Integro| Auto-Select| 1.82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7330002Z2P0LGN| SSD| False| OK|      Integro|Auto-Select| 1.82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF733000272P0LGN |SSD| False| OK|  Integro| Auto-Select| 1.82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7330001J2P0LGN |SSD| False| OK| Integro| Auto-Select| 1.82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF733000302P0LGN |SSD| False| OK|Integro| Auto-Select| 1.82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7330004D2P0LGN |SSD| False| OK| Integro| Auto-Select |1.82 TB|
    
## <a name="how-to-destroy-an-existing-cluster-so-you-can-use-the-same-disks-again"></a>Come eliminare un cluster esistente in modo che è possibile usare nuovamente gli stessi dischi

In un cluster di spazi di archiviazione diretta, una volta disabilitare la funzionalità spazi di archiviazione diretta e usare la procedura di pulizia descritta nel [Pulisci unità](deploy-storage-spaces-direct.md#step-31-clean-drives), il pool di archiviazione in cluster rimane in uno stato Offline e il servizio integrità viene rimosso da grappolo.

Il passaggio successivo consiste nel rimuovere il pool di archiviazione fittizio: 
   ```powershell
   Get-ClusterResource -Name "Cluster Pool 1" | Remove-ClusterResource
   ```

Ora, se si esegue **Get-PhysicalDisk** su uno dei nodi, si noterà che sono stati nel pool di tutti i dischi. Ad esempio, in un ambiente con un cluster di 4 nodi con 4 dischi SAS, 100GB ognuna presentati in ogni nodo. In tal caso, dopo la disabilitazione dello spazio di archiviazione diretta, che rimuove il SBL (livello del Bus di archiviazione), ma lascia invariato il filtro, se si esegue **Get-PhysicalDisk**, dovrebbe segnalare 4 dischi esclusione del disco del sistema operativo locale. In alternativa è segnalato invece 16. Questo è lo stesso per tutti i nodi del cluster. Quando si esegue una **Get-Disk** comando, si noterà i dischi collegati in locale, numerati come 0, 1, 2 e così via, come illustrato in questo esempio di output:

|Numero| Nome descrittivo| Numero di serie|HealthStatus|OperationalStatus|Dimensioni totali| Stile di partizione|
|-|-|-|-|-|-|-|-|
|0|Virtu msft...  ||Integro | Online|  127 GB| GPT|
||Virtu msft... ||Integro| Offline| 100 GB| NON ELABORATI|
||Virtu msft... ||Integro| Offline| 100 GB| NON ELABORATI|
||Virtu msft... ||Integro| Offline| 100 GB| NON ELABORATI|
||Virtu msft... ||Integro| Offline| 100 GB| NON ELABORATI|
|1|Virtu msft...||Integro| Offline| 100 GB| NON ELABORATI|
||Virtu msft... ||Integro| Offline| 100 GB| NON ELABORATI|
|2|Virtu msft...||Integro| Offline| 100 GB| NON ELABORATI|
||Virtu msft... ||Integro| Offline| 100 GB| NON ELABORATI|
||Virtu msft... ||Integro| Offline| 100 GB| NON ELABORATI|
||Virtu msft... ||Integro| Offline| 100 GB| NON ELABORATI|
||Virtu msft... ||Integro| Offline| 100 GB| NON ELABORATI|
|4|Virtu msft...||Integro| Offline| 100 GB| NON ELABORATI|
|3|Virtu msft...||Integro| Offline| 100 GB| NON ELABORATI|
||Virtu msft... ||Integro| Offline| 100 GB| NON ELABORATI|
||Virtu msft... ||Integro| Offline| 100 GB| NON ELABORATI|
||Virtu msft... ||Integro| Offline| 100 GB| NON ELABORATI|

    
## <a name="error-message-about-unsupported-media-type-when-you-create-an-storage-spaces-direct-cluster-using-enable-clusters2d"></a>Messaggio di errore su "tipo di supporto non supportato" quando si crea un cluster di spazi di archiviazione diretta tramite Enable-ClusterS2D  

Si potrebbero verificare errori simili al seguente quando si esegue la **Enable-ClusterS2D** cmdlet:

![Messaggio di errore dello scenario 6](media/troubleshooting/scenario-error-message.png)

Per risolvere questo problema, assicurarsi che la scheda HBA viene configurata in modalità scheda HBA. Nessuna scheda bus host deve essere configurato in modalità RAID.  
    
## <a name="enable-clusterstoragespacesdirect-hangs-at-waiting-until-sbl-disks-are-surfaced-or-at-27"></a>Enable-ClusterStorageSpacesDirect si blocca a 'In attesa fino a SBL dischi vengono replicati' o a 27%

Verranno visualizzate le seguenti informazioni nel report di convalida:

    Disk <identifier> connected to node <nodename> returned a SCSI Port Association and the corresponding enclosure device could not be found. The hardware is not compatible with Storage Spaces Direct (S2D), contact the hardware vendor to verify support for SCSI Enclosure Services (SES). 
    
  
Il problema riguarda la scheda espansore SAS HPE che risiede tra i dischi e la scheda HBA. Espansore SAS consente di creare un ID duplicato tra la prima unità connesse al pulsante di espansione e di espansione stesso.  Questo è stato risolto [HPE Smart matrice controller SAS Expander Firmware: 4.02](https://support.hpe.com/hpsc/swd/public/detail?sp4ts.oid=7304566&swItemId=MTX_ef8d0bf4006542e194854eea6a&swEnvOid=4184#tab3).
    
## <a name="intel-ssd-dc-p4600-series-has-a-non-unique-nguid"></a>Intel SSD DC P4600 serie dispone di un NGUID non univoco
È possibile visualizzare un problema in cui un dispositivo serie Intel SSD DC P4600 sembra essere reporting byte 16 NGUID simile per più spazi dei nomi, ad esempio 0100000001000000E4D25C000014E214 o 0100000001000000E4D25C0000EEE214 nell'esempio seguente.

|uniqueid| ID dispositivo |MediaType| BusType| numero di serie| dimensioni|canpool| FriendlyName| OperationalStatus|
|-|-|-|-|-|-|-|-|-
|5000CCA251D12E30| 0| Unità disco rigido| SAS| 7PKR197G|                  10000831348736 |False|HGST| HUH721010AL4200| OK|
|eui.0100000001000000E4D25C000014E214 |4|SSD| NVMe|   0100_0000_0100_0000_E4D2_5C00_0014_E214.|1600321314816|True| INTEL| SSDPE2KE016T7|  OK|
|eui.0100000001000000E4D25C000014E214 |5|        SSD|       NVMe|    0100_0000_0100_0000_E4D2_5C00_0014_E214.|  1600321314816|    True| INTEL| SSDPE2KE016T7|  OK|
|eui.0100000001000000E4D25C0000EEE214| 6|        SSD|       NVMe|    0100_0000_0100_0000_E4D2_5C00_00EE_E214.|  1600321314816|    True| INTEL| SSDPE2KE016T7|  OK|
|eui.0100000001000000E4D25C0000EEE214| 7|        SSD|       NVMe|    0100_0000_0100_0000_E4D2_5C00_00EE_E214.|  1600321314816|    True| INTEL| SSDPE2KE016T7|  OK|

Per risolvere questo problema, aggiornare il firmware sulle unità Intel la versione più recente.  Versione del firmware QDV101B1 dal mese di maggio 2018 è noto per risolvere il problema.

Il [mese di maggio 2018 versione dello strumento Intel SSD Data Center](https://downloadmirror.intel.com/27778/eng/Intel_SSD_Data_Center_Tool_3_0_12_Release_Notes_330715-026.pdf) include un aggiornamento del firmware, QDV101B1, per la serie di Intel SSD DC P4600.

    
## <a name="physical-disk-healthy-and-operational-status-is-removing-from-pool"></a>"Integro" disco fisico e lo stato operativo è "Rimozione dal Pool" 

In un cluster Windows Server 2016 spazi di archiviazione diretta, si potrebbe vedere il HealthStatus per i dischi fisici a uno o più "Integro", mentre il OperationalStatus è "(rimozione dal Pool, OK)". 

"Rimozione dal Pool" è un intent impostato quando **Remove-PhysicalDisk** viene chiamato ma archiviati in rapporto di stato mantengono lo stato e consentire il recupero se l'operazione di rimozione ha esito negativo. È possibile modificare manualmente il OperationalStatus allo stato Healthy con uno dei metodi seguenti:

- Rimuovere il disco fisico dal pool e quindi aggiungerlo di nuovo.
- Eseguire la [Clear-PhysicalDiskHealthData.ps1 script](https://go.microsoft.com/fwlink/?linkid=2034205) per cancellare lo scopo. (Disponibile per il download come una. File TXT. È necessario salvarla come una. File con estensione ps1 prima di poterlo eseguire.)

Di seguito sono riportati alcuni esempi che illustrano come eseguire lo script:

- Usare la **SerialNumber** parametro per specificare il disco è necessario impostare su integro. È possibile ottenere il numero di serie da **WMI MSFT_PhysicalDisk** oppure **Get-PhysicalDIsk**. (Solo utilizziamo lt;0s&gt per il numero di serie seguente.)
   
   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -SerialNumber 000000000000000 -Verbose -Force
    ```

- Usare la **UniqueId** parametro per specificare il disco (nuovamente dal **MSFT_PhysicalDisk WMI** oppure **Get-PhysicalDIsk**).

   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -UniqueId 00000000000000000 -Verbose -Force
   ```

## <a name="file-copy-is-slow"></a>Copia dei file è lenta

Si potrebbe essere visualizzato un problema usando Esplora File per copiare un disco rigido virtuale di grandi dimensioni al disco virtuale, la copia del file richiede più tempo del previsto.

Uso di Esplora File, Robocopy o Xcopy per copiare un disco rigido virtuale di grandi dimensioni al disco virtuale non è un metodo consigliato per quanto ciò comporterebbe più lento rispetto delle prestazioni previsti. Il processo di copia non passa attraverso lo stack di spazi di archiviazione diretta, che si trova più basso dello stack di archiviazione e invece funziona come un processo di copia locale.

Se si desidera testare le prestazioni di spazi di archiviazione diretta, è consigliabile usare Diskspd e VMFleet carico e test di stress i server per ottenere una linea di base e impostare le aspettative delle prestazioni di spazi di archiviazione diretta.

## <a name="expected-events-that-you-would-see-on-rest-of-the-nodes-during-the-reboot-of-a-node"></a>Gli eventi che si vedrebbe sul resto dei nodi durante il riavvio di un nodo è previsto. 

È possibile ignorare questi eventi: 

    Event ID 205: Windows lost communication with physical disk {XXXXXXXXXXXXXXXXXXXX }. This can occur if a cable failed or was disconnected, or if the disk itself failed. 

    Event ID 203: Windows lost communication with physical disk {xxxxxxxxxxxxxxxxxxxxxxxx }. This can occur if a cable failed or was disconnected, or if the disk itself failed. 

Se si eseguono macchine virtuali di Azure, è possibile ignorare questo evento:

    Event ID 32: The driver detected that the device \Device\Harddisk5\DR5 has its write cache enabled. Data corruption may occur. 
    
## <a name="slow-performance-or-lost-communication-io-error-detached-or-no-redundancy-errors-for-deployments-that-use-intel-p3x00-nvme-devices"></a>Rallentamento delle prestazioni o "Comunicazione perduta," "Errore dei / o", "Scollegato," o "N ridondanza" errori per le distribuzioni che usano dispositivi NVMe P3x00 Intel

Abbiamo identificato un problema grave che interessa alcuni utenti di spazi di archiviazione diretta che usano componenti hardware in base alla famiglia Intel P3x00 di NVM Express (NVMe) nei dispositivi con versioni di firmware prima "Manutenzione versione 8". 

>[!NOTE]
> Singoli OEM potrebbe essere dispositivi basati su della famiglia Intel P3x00 di dispositivi NVMe con le stringhe di versione del firmware univoco. Per altre informazioni dell'ultima versione del firmware, contattare l'OEM.

Se si usa hardware nella distribuzione di base della famiglia Intel P3x00 dei dispositivi NVMe, è consigliabile applicare immediatamente il firmware più recente disponibile (almeno 8 Release di manutenzione). Ciò [articolo del supporto Microsoft](https://support.microsoft.com/en-us/help/4052341/slow-performance-or-lost-communication-io-error-detached-or-no-redunda) fornisce informazioni aggiuntive su questo problema. 
