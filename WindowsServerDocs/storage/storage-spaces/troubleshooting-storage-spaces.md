---
title: Risoluzione spazi di archiviazione
description: Scopri come risolvere i problemi la distribuzione di spazi di archiviazione diretta.
keywords: Spazi di archiviazione
ms.prod: windows-server-threshold
ms.author: ''
ms.technology: storage-spaces
ms.topic: article
author: kaushika-msft
ms.date: 10/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: c7c9573732ff1cf5a998588b1aec81915c227ee2
ms.sourcegitcommit: 5d55c3ebd1ceee7229ea9a9989c69cf93757ed88
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/21/2019
ms.locfileid: "9256934"
---
# Risolvere i problemi di spazi di archiviazione diretta

Usa le informazioni seguenti per risolvere i problemi la distribuzione di spazi di archiviazione diretta.

In generale, inizia con la procedura seguente:

1. Verifica che il marca o modello di unità SSD è ottenere la certificazione per Windows Server 2016 tramite catalogo di Windows Server. Verifica che al fornitore che le unità sono supportate per spazi di archiviazione diretta.
2. Esaminare lo spazio di archiviazione per qualsiasi unità difettoso. Utilizzare il software di gestione archiviazione per controllare lo stato delle unità. Se le unità sono errate, sono compatibili con il tuo fornitore. 
3. Aggiornare l'archiviazione e del firmware delle unità se necessario.
   Assicurati che gli aggiornamenti di Windows più recenti installati in tutti i nodi. Puoi ottenere gli aggiornamenti più recenti per Windows Server 2016 da [https://aka.ms/update2016](https://aka.ms/update2016).
4. Aggiorna firmware e driver scheda di rete.
5. Eseguire la convalida dei cluster e rivedere la sezione dello spazio di archiviazione diretta, assicurarti che le unità che verranno utilizzate per la cache vengono segnalate correttamente e senza errori.

Se si verificano ancora problemi, esamina gli scenari seguenti.

## Le risorse disco virtuale sono in stato di ridondanza No
I nodi di un sistema di spazi di archiviazione diretta riavviare in modo imprevisto a causa di un errore di arresto anomalo del sistema o all'alimentazione. Quindi, uno o più dischi virtuali non può essere online e vedere la descrizione "non è sufficiente le informazioni di ridondanza."
    
|FriendlyName|ResiliencySettingName| OperationalStatus| HealthStatus| IsManualAttach|Dimensioni| PSComputerName|
|------------|---------------------| -----------------| ------------| --------------|-----| --------------|
|Disk4| Mirror| OK|  Healthy| True|  10 TB|  Nodo-01.conto...|
|Disk3         |Mirror                 |OK                          |Healthy       |True            |10 TB | Nodo-01.conto...|
|Disco2         |Mirror                 |Nessuna ridondanza               |Unhealthy     |True            |10 TB | Nodo-01.conto...|
|Disco 1         |Mirror                 |{Nessuna ridondanza, InRiparazione}  |Unhealthy     |True            |10 TB | Nodo-01.conto...| 

Inoltre, dopo un tentativo di portare online il disco virtuale, le informazioni seguenti viene registrate nel log di Cluster (DiskRecoveryAction).  

```
[Verbose] 00002904.00001040::YYYY/MM/DD-12:03:44.891 INFO [RES] Physical Disk <DiskName>: OnlineThread: SuGetSpace returned 0.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 WARN [RES] Physical Disk < DiskName>: Underlying virtual disk is in 'no redundancy' state; its volume(s) may fail to mount.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 ERR [RES] Physical Disk <DiskName>: Failing online due to virtual disk in 'no redundancy' state. If you would like to attempt to online the disk anyway, first set this resource's private property 'DiskRecoveryAction' to 1. We will try to bring the disk online for recovery, but even if successful, its volume(s) or CSV may be unavailable. 
``` 

Lo **Stato operativo ridondanza No** può verificarsi se un disco non è riuscito o se il sistema è in grado di accedere ai dati sul disco virtuale. Questo problema può verificarsi se si verifica un riavvio in un nodo durante la manutenzione sui nodi.
    
Per risolvere questo problema, segui questi passaggi:

1. Rimuovere i dischi virtuali interessati da CSV. Verrà inserirli nel gruppo "Archiviazione disponibile" del cluster e start che mostra come una risorsa di "Disco fisico".

   ```powershell
   Remove-ClusterSharedVolume -name "VdiskName"
   ``` 
2. Nel nodo di proprietario del gruppo di archiviazione disponibili, eseguito il comando seguente in tutti i dischi che sono in uno stato di ridondanza No. Per identificare quale nodo del gruppo di "Archiviazione disponibile" è attivata è possibile eseguire il comando seguente.

   ```powershell
   Get-ClusterGroup
   ```
3. Impostare l'azione di ripristino del disco e quindi avvia i disco o i dischi.
   ```powershell
   Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 1
   Start-ClusterResource -Name "VdiskName"
   ```
4. Ripristino deve avviarsi automaticamente. Attendere la riparazione alla fine. Potrebbe accedere allo stato di sospensione e ricominciare. Per monitorare lo stato di avanzamento: 
    - Esegui **Get-StorageJob** per monitorare lo stato della riparazione e visualizzare quando viene completata.
    - Esegui **Get-VirtualDisk** e verificare che lo spazio restituisce HealthStatus integro.
5. Dopo il ripristino finisce e i dischi virtuali sono integro, modifica i parametri disco virtuale indietro.

   ```powershell
    Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 0
   ```
6. Eseguire i dischi offline e quindi online per avere la DiskRecoveryAction abbia effetto:

   ```powershell
   Stop-ClusterResource "VdiskName"
   Start-ClusterResource "VdiskName"
   ``` 
7. Aggiungere i dischi virtuali interessati nuovamente in formato CSV.

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```

**DiskRecoveryAction** è un commutatore di override che consente di associare il volume di spazio in modalità di lettura e scrittura senza eventuali controlli. La proprietà consente di eseguire operazioni di diagnostica in perché non si connettono un volume. È molto simile alla modalità di manutenzione, ma è possibile richiamarla su una risorsa in uno stato non riuscito. Consente inoltre di accedere ai dati, che possono essere utili in situazioni, ad esempio "No ridondanza", dove puoi ottenere l'accesso a qualsiasi dati puoi e copiarlo. La proprietà DiskRecoveryAction è stato aggiunto a di febbraio 22 2018, aggiornamento, 4077525 KB.
    
    
## Stato disconnesso in un cluster 

Quando Esegui il cmdlet **Get-VirtualDisk** , viene disconnesso OperationalStatus per uno o più dischi virtuali spazi di archiviazione diretta. Tuttavia, il HealthStatus segnalati dal cmdlet **Get-PhysicalDisk** indica che tutti i dischi fisici in uno stato integro.

Di seguito è un esempio dell'output dal cmdlet **Get-VirtualDisk** .

|FriendlyName|  ResiliencySettingName|  OperationalStatus|   HealthStatus|  IsManualAttach|  Dimensioni|   PSComputerName|
|-|-|-|-|-|-|-|
|Disk4|         Mirror|                 OK|                  Healthy|       True|            10 TB|  Nodo-01.conto...|
|Disk3|         Mirror|                 OK|                  Healthy|       True|            10 TB|  Nodo-01.conto...|
|Disco2|         Mirror|                 Scollegati|            Sconosciuto|       True|            10 TB|  Nodo-01.conto...|
|Disco 1|         Mirror|                 Scollegati|            Sconosciuto|       True|            10 TB|  Nodo-01.conto...| 


Inoltre, gli eventi seguenti potrebbero essere registrati sui nodi:

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

Lo **Stato operativo scollegato** può verificarsi se l'area dirty monitoraggio (DRT) log è pieno. Spazi di archiviazione Usa area dirty monitoraggio (DRT) per gli spazi con mirroring per verificare che quando si verifica un errore di alimentazione, gli aggiornamenti in corso per i metadati vengono registrati per assicurarsi che lo spazio di archiviazione può ripristinare o annullare le operazioni per riportare lo spazio di archiviazione in un flessibile e stato coerente quando viene ripristinata l'alimentazione e il sistema torna allo stato attivo. Se il log DRT è completo, il disco virtuale non può essere portato online fino a quando non sono sincronizzato e svuotato i metadati DRT. Questo processo deve essere eseguito un'analisi completa, ciò potrebbe richiedere diverse ore prima che finisca.
    
Per risolvere questo problema, segui questi passaggi:
1. Rimuovere i dischi virtuali interessati da CSV.

   ```powershell
   Remove-ClusterSharedVolume -name "VdiskName"
   ``` 
2. Eseguire i comandi seguenti in tutti i dischi che non sarà disponibile in linea. 

   ```powershell
   Get-ClusterResource -Name "VdiskName" | Set-ClusterParameter DiskRunChkDsk 7
   Start-ClusterResource -Name "VdiskName"
   ``` 
3. Esegui il comando seguente in ogni nodo in cui il volume scollegato è online. 

   ```powershell
   Get-ScheduledTask -TaskName "Data Integrity Scan for Crash Recovery" | Start-ScheduledTask 
   ```
   Questa attività deve essere avviata in tutti i nodi in cui il volume scollegato è online. Ripristino deve avviarsi automaticamente. Attendere la riparazione alla fine. Potrebbe accedere allo stato di sospensione e ricominciare. Per monitorare lo stato di avanzamento: 
   - Esegui **Get-StorageJob** per monitorare lo stato della riparazione e visualizzare quando viene completata.
   -  Esegui **Get-VirtualDisk** e verificare che lo spazio restituisce HealthStatus integro.
    - "Integrità dei dati analisi per ripristino degli arresti anomali" è un'attività che non mostra come un processo di archiviazione e non esiste alcun indicatore di stato. Se l'attività viene visualizzato come in esecuzione, è in esecuzione. Al termine, verrà visualizzato completata.
    
       Inoltre, puoi visualizzare lo stato di un'attività di pianificazione in esecuzione usando il cmdlet seguente: 
       ```powershell
       Get-ScheduledTask | ? State -eq running
       ``` 
4. Appena il "ripristino dei dati integrità analisi per arresto anomalo del sistema" è terminato, finisce il ripristino e i dischi virtuali sono integro, modifica i parametri disco virtuale indietro. 

   ```powershell
   Get-ClusterResource -Name "VdiskName" | Set-ClusterParameter DiskRunChkDsk 0 
   ```
5. Aggiungere i dischi virtuali interessati nuovamente in formato CSV.    

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```  
**Valore DiskRunChkdsk 7** viene usato per connettere il volume di spazio e hanno la partizione in modalità di sola lettura. In questo modo spazi self-individuare e self-heal generando un ripristino. Repair eseguirà automaticamente una volta montati. Inoltre, consente di accedere ai dati, che possono essere utili per ottenere l'accesso a qualsiasi è possibile per copiare i dati. Per alcune condizioni di errore, ad esempio un registro DRT completo, è necessario eseguire analisi integrità dei dati per le attività pianificate di arresto anomalo di ripristino.
    
**Dati Integrity Scan per attività di ripristino di arresto anomalo** viene usato per sincronizzare e cancellare un log di traccia (DRT) area dirty completo. Questa operazione può richiedere diverse ore. "Integrità dei dati analisi per ripristino degli arresti anomali" è un'attività che non mostra come un processo di archiviazione e non esiste alcun indicatore di stato. Se l'attività viene visualizzato come in esecuzione, è in esecuzione. Al termine, mostrerà come completato. Se annulli l'attività o riavviare un nodo durante l'esecuzione di questa attività, sarà necessario ricominciare dall'inizio l'attività.

Per ulteriori informazioni, vedere [risoluzione dei problemi di spazi di archiviazione diretta stati di integrità e operativi](storage-spaces-states.md).
    
## Evento 5120 con STATUS_IO_TIMEOUT c00000b5 

> [!Important]
> Per ridurre la probabilità che verificano questi problemi durante l'applicazione dell'aggiornamento con la correzione, è consigliabile utilizzare la procedura di archiviazione la modalità di manutenzione seguente per installare l' [aggiornamento cumulativo per Windows Server 2016 18 ottobre 2018,](https://support.microsoft.com/help/4462928) o versione successiva Quando i nodi attualmente installato un aggiornamento cumulativo di Windows Server 2016 che è stato rilasciato da [8 maggio 2018](https://support.microsoft.com/help/4103723) a [9 ottobre 2018](https://support.microsoft.com/help/KB4462917).

Potresti ricevere eventi 5120 con STATUS_IO_TIMEOUT c00000b5 dopo il riavvio di un nodo in Windows Server 2016 con aggiornamento cumulativo che sono stati rilasciati da [potrebbe 8 KB 2018 4103723](https://support.microsoft.com/help/4103723) a [9 ottobre 2018 KB 4462917](https://support.microsoft.com/help/4462917) installato.

Quando si riavvia il nodo, 5120 eventi vengono registrati nel registro eventi di sistema e include uno dei seguenti codici di errore:

```
Event Source: Microsoft-Windows-FailoverClustering
Event ID: 5120
Description:    Cluster Shared Volume 'CSVName' ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_IO_TIMEOUT(c00000b5)'. All I/O will temporarily be queued until a path to the volume is reestablished. 

Cluster Shared Volume ‘CSVName’ ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_CONNECTION_DISCONNECTED(c000020c)'. All I/O will temporarily be queued until a path to the volume is reestablished.    
```

Quando viene registrato un evento di 5120, viene generato un dump animato per raccogliere informazioni di debug che possono causare sintomi aggiuntive o hanno un impatto sulle prestazioni. Generazione di dump live crea una breve pausa per consentire l'esecuzione di uno snapshot della memoria per scrivere il file di dump. I sistemi che hanno grandi quantità di memoria e sono sotto stress potrebbe nodi esclusi dalla visualizzazione membri del cluster e anche far sì che il seguente 1135 evento da registrare.

```
Event source: Microsoft-Windows-FailoverClustering
Event ID: 1135  
Description: Cluster node 'NODENAME'was removed from the active failover cluster membership. The Cluster service on this node may have stopped. This could also be due to the node having lost communication with other active nodes in the failover cluster. Run the Validate a Configuration wizard to check your network configuration. If the condition persists, check for hardware or software errors related to the network adapters on this node. Also check for failures in any other network components to which the node is connected such as hubs, switches, or bridges.
```

È stata introdotta una modifica nell'aggiornamento cumulativo per 8 maggio 2018, aggiungere gestisce resiliente SMB per le sessioni di rete SMB interne a un cluster spazi di archiviazione diretta. Questo è stato fatto per migliorare la resilienza a errori di rete temporaneo e migliorare la modalità di gestione di congestione della rete RoCE.

Questi miglioramenti aumentato anche inavvertitamente timeout quando tentano di connessioni SMB riconnettere e attesa del timeout al riavvio di un nodo. Questi problemi possono influire su un sistema che con sovraccarico. Durante l'inattività, pause dei / o di un massimo di 60 secondi anche rilevate mentre il sistema attende per le connessioni di timeout.

Per risolvere questo problema, installare il [18 ottobre 2018, aggiornamento cumulativo per Windows Server 2016](https://support.microsoft.com/help/4462928) o versione successiva.

*Nota* Questo aggiornamento Allinea i timeout CSV con timeout della connessione di SMB per risolvere questo problema. Non implementare le modifiche per disabilitare la generazione di dump live indicata nella sezione per risolvere il problema.
    
### Flusso del processo di arresto:

1. Esegui il cmdlet Get-VirtualDisk e assicurati che il valore HealthStatus sia integro.
2. Svuota il nodo eseguendo il cmdlet seguente:

   ```powershell
   Suspend-ClusterNode -Drain
   ```
3. Inserisci i dischi nel nodo in modalità manutenzione archiviazione eseguendo il cmdlet seguente: 

   ```powershell
   Get-StorageFaultDomain -type StorageScaleUnit | Where-Object {$_.FriendlyName -eq "<NodeName>"} | Enable-StorageMaintenanceMode
   ```
4. Esegui il cmdlet **Get-PhysicalDisk** e assicurati che il valore OperationalStatus sia In modalità di manutenzione.
5. Eseguire il cmdlet **Restart-Computer** per riavviare il nodo.
6. Dopo il riavvio di nodo, rimuovere i dischi nel nodo dalla modalità di manutenzione di archiviazione eseguendo il cmdlet seguente:

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

### Disabilitare i dump live
Per ridurre l'effetto di generazione di dump live nei sistemi che hanno grandi quantità di memoria e sono sotto stress, potresti voler inoltre disabilitare la generazione di dump live. Tre opzioni vengono fornite di seguito.

>[!Caution]
>Questa procedura può evitare la raccolta di informazioni di diagnostica che il supporto Microsoft potrebbero dover analizzare il problema. Potrebbe essere un agente di supporto che richiede all'utente di abilitare nuovamente la generazione di dump animati in base agli scenari di risoluzione dei problemi specifici.

Esistono due metodi per disabilitare i dump live, come descritto di seguito.

#### Metodo 1 (scelta consigliata in questo scenario)
Per disattivare completamente tutti i dump, inclusi i dump live a livello di sistema, segui questi passaggi:

1. Creare la chiave del Registro di sistema seguente: HKLM\System\CurrentControlSet\Control\CrashControl\ForceDumpsDisabled
2. Nella nuova chiave **ForceDumpsDisabled** , creare una proprietà REG_DWORD come GuardedHost e quindi imposta il valore di 0x10000000.
3. Applicare la nuova chiave del Registro di sistema per ogni nodo del cluster.

>[!NOTE]
>È necessario riavviare il computer rendere effettive le modifiche NL.

Dopo che questa chiave del Registro di sistema è impostato, live la creazione di dump non riuscirà e genererà un errore "STATUS_NOT_SUPPORTED".

#### Metodo 2
Per impostazione predefinita, segnalazione errori Windows consentirà un solo LiveDump per ogni tipo di report per ogni 7 giorni e solo 1 LiveDump per ogni computer per ogni 5 giorni. È possibile modificare che impostando le seguenti chiavi del Registro di sistema per consentire solo uno LiveDump sul computer forever.
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v SystemThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v ComponentThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```

*Nota* È necessario riavviare il computer rendere effettive le modifiche.

### Metodo 3
Per disabilitare la generazione di cluster di dump live (ad esempio, quando viene registrato un evento di 5120), Esegui il cmdlet seguente:

```powershell
(Get-Cluster).DumpPolicy = ((Get-Cluster).DumpPolicy -band 0xFFFFFFFFFFFFFFFE)
```
Questo cmdlet ha effetto immediato su tutti i nodi del cluster senza riavviare il computer.
    
## Riduzione delle prestazioni dei / o

Se sono le prestazioni dei / o lenta, controlla se la cache è abilitata nella configurazione di spazi di archiviazione diretta. 

Esistono due modi per controllare: 
     
 
1. Usare il log di cluster. Apri il file di registro del cluster in un editor di testo di scelta e cerca "[=== SBL dischi ===]." Questo sarà un elenco del disco nel nodo che è stato generato il log sul. 

     Esempio di dischi cache Enabled: Si noti che lo stato è CacheDiskStateInitializedAndBound e c'è un GUID presente qui. 

   ```
   [=== SBL Disks ===]
    {26e2e40f-a243-1196-49e3-8522f987df76},3,false,true,1,48,{1ff348f1-d10d-7a1a-d781-4734f4440481},CacheDiskStateInitializedAndBound,1,8087,54,false,false,HGST    ,HUH721010AL4200 ,        7PG3N2ER,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    Cache non abilitato: Qui possiamo vedere non è presente alcun GUID e lo stato è CacheDiskStateNonHybrid. 
    ```
   [=== SBL Disks ===]
    {426f7f04-e975-fc9d-28fd-72a32f811b7d},12,false,true,1,24,{00000000-0000-0000-0000-000000000000},CacheDiskStateNonHybrid,0,0,0,false,false,HGST    ,HUH721010AL4200 ,        7PGXXG6C,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    Cache non abilitato: Quando tutti i dischi sono dello stesso tipo caso non è abilitato per impostazione predefinita. Qui possiamo vedere non è presente alcun GUID e lo stato è CacheDiskStateIneligibleDataPartition. 
    ```
    {d543f90c-798b-d2fe-7f0a-cb226c77eeed},10,false,false,1,20,{00000000-0000-0000-0000-000000000000},CacheDiskStateIneligibleDataPartition,0,0,0,false,false,NVMe    ,INTEL SSDPE7KX02,  PHLF7330004V2P0LGN,0170,{79b4d631-976f-4c94-a783-df950389fd38},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0], 
    ```  
2. Usando Get-PhysicalDisk.xml dal SDDCDiagnosticInfo
    1. Aprire il file XML con "$d = GetPhysicalDisk.XML Import-Clixml"
    2. Esegui "ipmo archiviazione"
    3. Esegui "$d". Si noti che l'utilizzo è selezione automatica, non Journal vedrai output come segue: 

   |FriendlyName|  SerialNumber| MediaType| CanPool| OperationalStatus| HealthStatus| Usage| Dimensioni|
   |-----------|------------|---------| -------| -----------------| ------------| -----| ----|
   |Unità NVMe INTEL SSDPE7KX02| PHLF733000372P0LGN| SSD| False|   OK|                Healthy|      Selezione automatica 1,82 TB|
   |Unità NVMe INTEL SSDPE7KX02 |PHLF7504008J2P0LGN| SSD|  False|    OK|                Healthy| Selezione automatica| 1,82 TB|
   |Unità NVMe INTEL SSDPE7KX02| PHLF7504005F2P0LGN| SSD|  False|  OK|                Healthy| Selezione automatica| 1,82 TB|
   |Unità NVMe INTEL SSDPE7KX02 |PHLF7504002A2P0LGN| SSD| False| OK|    Healthy| Selezione automatica| 1,82 TB|
   |Unità NVMe INTEL SSDPE7KX02| PHLF7504004T2P0LGN |SSD| False|OK|       Healthy| Selezione automatica| 1,82 TB|
   |Unità NVMe INTEL SSDPE7KX02 |PHLF7504002E2P0LGN| SSD| False| OK|      Healthy| Selezione automatica| 1,82 TB|
   |Unità NVMe INTEL SSDPE7KX02 |PHLF7330002Z2P0LGN| SSD| False| OK|      Healthy|Selezione automatica| 1,82 TB|
   |Unità NVMe INTEL SSDPE7KX02 |PHLF733000272P0LGN |SSD| False| OK|  Healthy| Selezione automatica| 1,82 TB|
   |Unità NVMe INTEL SSDPE7KX02 |PHLF7330001J2P0LGN |SSD| False| OK| Healthy| Selezione automatica| 1,82 TB|
   |Unità NVMe INTEL SSDPE7KX02| PHLF733000302P0LGN |SSD| False| OK|Healthy| Selezione automatica| 1,82 TB|
   |Unità NVMe INTEL SSDPE7KX02| PHLF7330004D2P0LGN |SSD| False| OK| Healthy| Selezione automatica |1,82 TB|
    
## Come eliminare un cluster esistente, quindi puoi usare i dischi stesso nuovamente

In un cluster di spazi di archiviazione diretta, una volta disabiliti spazi di archiviazione diretta e Usa il processo di pulizia descritto in [unità pulita](deploy-storage-spaces-direct.md#step-31-clean-drives), pool di archiviazione del cluster rimangono in uno stato Offline e il servizio integrità viene rimosso dal cluster.

Il passaggio successivo consiste nel rimuovere il pool di archiviazione fittizio: 
   ```powershell
   Get-ClusterResource -Name "Cluster Pool 1" | Remove-ClusterResource
   ```

A questo punto, se si esegue **Get-PhysicalDisk** su uno dei nodi, vedrai che sono stati nel pool di tutti i dischi. Ad esempio, in un lab con un cluster di 4 nodi con 4 dischi SAS, 100GB ogni presentato a ogni nodo. In tal caso, dopo che lo spazio di archiviazione diretta è disabilitata, che rimuove il SBL (livello Bus di archiviazione) ma lascia il filtro, se si esegue **Get-PhysicalDisk**, devono segnalare 4 dischi escludendo disco del sistema operativo locale. Al contrario segnalato invece 16. Questo è lo stesso per tutti i nodi del cluster. Quando si esegue un comando di **Get-Disk** , vedrai i dischi collegati localmente numerati come 0, 1, 2 e così via, come illustrato in questo output di esempio:

|Numero| Nome descrittivo| Numero di serie|HealthStatus|OperationalStatus|Dimensioni totali| Stile di partizione|
|-|-|-|-|-|-|-|-|
|0|Virtu msft...  ||Healthy | Online|  127 GB| GPT|
||Virtu msft... ||Healthy| Offline| 100 GB| Crudo|
||Virtu msft... ||Healthy| Offline| 100 GB| Crudo|
||Virtu msft... ||Healthy| Offline| 100 GB| Crudo|
||Virtu msft... ||Healthy| Offline| 100 GB| Crudo|
|1|Virtu msft...||Healthy| Offline| 100 GB| Crudo|
||Virtu msft... ||Healthy| Offline| 100 GB| Crudo|
|2|Virtu msft...||Healthy| Offline| 100 GB| Crudo|
||Virtu msft... ||Healthy| Offline| 100 GB| Crudo|
||Virtu msft... ||Healthy| Offline| 100 GB| Crudo|
||Virtu msft... ||Healthy| Offline| 100 GB| Crudo|
||Virtu msft... ||Healthy| Offline| 100 GB| Crudo|
|4|Virtu msft...||Healthy| Offline| 100 GB| Crudo|
|3|Virtu msft...||Healthy| Offline| 100 GB| Crudo|
||Virtu msft... ||Healthy| Offline| 100 GB| Crudo|
||Virtu msft... ||Healthy| Offline| 100 GB| Crudo|
||Virtu msft... ||Healthy| Offline| 100 GB| Crudo|

    
## Messaggio di errore su "tipo di supporto non supportata" quando si crea un cluster di spazi di archiviazione diretta tramite Enable-ClusterS2D  

Quando Esegui il cmdlet **Enable-ClusterS2D** potresti vedere errori simili al seguente:

![Messaggio di errore scenario 6](media/troubleshooting/scenario-error-message.png)

Per risolvere questo problema, assicurarsi che la scheda HBA sia configurata in modalità HBA. Nessun HBA deve essere configurato in modalità RAID.  
    
## Enable-ClusterStorageSpacesDirect si blocca a 'In attesa finché non SBL dischi vengono esposti' o a 27%

Vedrai le seguenti informazioni nel report convalida:

    Disk <identifier> connected to node <nodename> returned a SCSI Port Association and the corresponding enclosure device could not be found. The hardware is not compatible with Storage Spaces Direct (S2D), contact the hardware vendor to verify support for SCSI Enclosure Services (SES). 
    
  
Il problema è con la scheda espansore SAS HPE che rientri tra i dischi e la scheda HBA. L'espansore SAS crea un ID duplicato tra della prima unità connessi al pulsante di espansione e il pulsante di espansione.  Questo è stato risolto nel [HPE Smart matrice controller SAS espansore Firmware: 4.02](https://support.hpe.com/hpsc/swd/public/detail?sp4ts.oid=7304566&swItemId=MTX_ef8d0bf4006542e194854eea6a&swEnvOid=4184#tab3).
    
## Serie Intel SSD DC P4600 ha un NGUID non univoco
Potresti vedere un problema in cui un dispositivo di serie Intel SSD DC P4600 sembra essere reporting simile di 16 byte NGUID per più spazi dei nomi, ad esempio 0100000001000000E4D25C000014E214 o 0100000001000000E4D25C0000EEE214 nell'esempio seguente.

|UniqueID| ID dispositivo |MediaType| BusType| numero di serie| size|canpool| FriendlyName| OperationalStatus|
|-|-|-|-|-|-|-|-|-
|5000CCA251D12E30| 0| Unità disco rigido| SAS| 7PKR197G|                  10000831348736 |False|HGST| HUH721010AL4200| OK|
|EUI.0100000001000000E4D25C000014E214 |4|SSD| NVMe|   0100_0000_0100_0000_E4D2_5C00_0014_E214.|1600321314816|True| INTEL| SSDPE2KE016T7|  OK|
|EUI.0100000001000000E4D25C000014E214 |5|        SSD|       NVMe|    0100_0000_0100_0000_E4D2_5C00_0014_E214.|  1600321314816|    True| INTEL| SSDPE2KE016T7|  OK|
|EUI.0100000001000000E4D25C0000EEE214| 6|        SSD|       NVMe|    0100_0000_0100_0000_E4D2_5C00_00EE_E214.|  1600321314816|    True| INTEL| SSDPE2KE016T7|  OK|
|EUI.0100000001000000E4D25C0000EEE214| 7|        SSD|       NVMe|    0100_0000_0100_0000_E4D2_5C00_00EE_E214.|  1600321314816|    True| INTEL| SSDPE2KE016T7|  OK|

Per risolvere questo problema, aggiornare il firmware nelle unità Intel la versione più recente.  Versione del firmware QDV101B1 da maggio 2018 è noto per risolvere il problema.

[Maggio 2018 rilasciare dello strumento Intel SSD Data Center](https://downloadmirror.intel.com/27778/eng/Intel_SSD_Data_Center_Tool_3_0_12_Release_Notes_330715-026.pdf) include un aggiornamento del firmware QDV101B1, la serie Intel SSD DC P4600.

    
## "Integro" disco fisico e lo stato operativo è "Rimozione dal Pool" 

In un cluster di Windows Server 2016 spazi di archiviazione diretta, potresti vedere HealthStatus per i dischi fisici di uno o più come "Integro", mentre il OperationalStatus è "(rimozione dal Pool, OK)". 

"Rimuovere dal Pool" è un intent impostato **Remove-PhysicalDisk** viene chiamata ma archiviati in integrità per mantenere lo stato e consentire il recupero se l'operazione di installazione ha esito negativo. È possibile modificare manualmente il OperationalStatus in integro con uno dei metodi seguenti:

- Rimuovere il disco fisico dal pool e quindi aggiungerlo nuovamente.
- Esegui lo [script chiaro PhysicalDiskHealthData.ps1](https://go.microsoft.com/fwlink/?linkid=2034205) per cancellare l'intento. (Disponibile per il download come un. File. TXT. È necessario salvarlo come un. Ps1 file prima di poter eseguire.)

Ecco alcuni esempi che mostrano come eseguire lo script:

- Usa il parametro **SerialNumber** per specificare il disco che è necessario impostare su integro. Puoi ottenere il numero di serie da **WMI MSFT_PhysicalDisk** o **Get-PhysicalDIsk**. (Ci limitiamo a usare 0 per il numero di serie riportato di seguito.)
   
   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -SerialNumber 000000000000000 -Verbose -Force
    ```

- Usa il parametro **UniqueId** per specificare il disco (nuovamente dal **MSFT_PhysicalDisk WMI** o **Get-PhysicalDIsk**).

   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -UniqueId 00000000000000000 -Verbose -Force
   ```

## Copia dei file è lenta

Si potrebbe rilevato un problema usando Esplora File per copiare un disco rigido virtuale di grandi dimensioni per il disco virtuale, la copia di file richiede più tempo del previsto.

Usando File Explorer, Robocopy o Xcopy per copiare un disco rigido virtuale di grandi dimensioni per il disco virtuale non è un metodo consigliato per perché il risultato sarà più lente rispetto alle prestazioni prevista. Il processo di copia non andare tramite lo stack di spazi di archiviazione diretta, che si trova inferiore nello stack di archiviazione e funziona invece di un processo di copia locale.

Se vuoi testare le prestazioni di spazi di archiviazione diretta, ti consigliamo di usare VMFleet e Diskspd per caricare e test di stress i server per ottenere una linea di base e impostare le aspettative delle prestazioni di spazi di archiviazione diretta.

## Gli eventi che vedrebbe sul resto dei nodi durante il riavvio di un nodo è previsto. 

È possibile ignorare questi eventi: 

    Event ID 205: Windows lost communication with physical disk {XXXXXXXXXXXXXXXXXXXX }. This can occur if a cable failed or was disconnected, or if the disk itself failed. 

    Event ID 203: Windows lost communication with physical disk {xxxxxxxxxxxxxxxxxxxxxxxx }. This can occur if a cable failed or was disconnected, or if the disk itself failed. 

Se stai macchine virtuali di Azure, è possibile ignorare questo evento:

    Event ID 32: The driver detected that the device \Device\Harddisk5\DR5 has its write cache enabled. Data corruption may occur. 
    
## Riduzione delle prestazioni o "Comunicazione interrotta," "Errore dei / o", "Disconnesso," o "No ridondanza" errori per le distribuzioni che usano dispositivi Intel P3x00 NVMe

Abbiamo identificato un problema fondamentale che influisce su alcuni utenti spazi di archiviazione diretta che usano hardware in base alla famiglia di Intel P3x00 dei dispositivi NVM Express (NVMe) con le versioni del firmware prima di "Versione di manutenzione 8". 

>[!NOTE]
> Gli OEM singoli possono avere dispositivi che si basano P3x00 Intel alla famiglia di dispositivi NVMe con le stringhe di versione del firmware univoco. Per ulteriori informazioni della versione del firmware più recente, contatta il produttore OEM.

Se usi hardware nella distribuzione in base alla famiglia di dispositivi NVMe P3x00 Intel, ti consigliamo di applicare immediatamente il firmware più recente disponibile (almeno manutenzione versione 8). Questo [articolo di supporto tecnico Microsoft](https://support.microsoft.com/en-us/help/4052341/slow-performance-or-lost-communication-io-error-detached-or-no-redunda) fornisce informazioni aggiuntive su questo problema. 
