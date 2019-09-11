---
title: Comprendere e distribuire memoria persistente
description: Informazioni dettagliate sulla memoria persistente e su come configurarla con spazi di archiviazione diretta in Windows Server 2019.
keywords: Spazi di archiviazione diretta, memoria persistente, PMEM, archiviazione, S2D
ms.assetid: ''
ms.prod: ''
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 3/26/2019
ms.localizationpriority: ''
ms.openlocfilehash: 497fa201c500919fc857d25166d37ce87613d0f0
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872015"
---
---
# <a name="understand-and-deploy-persistent-memory"></a>Comprendere e distribuire memoria persistente

>Si applica a Windows Server 2019

La memoria persistente (o PMem) è un nuovo tipo di tecnologia di memoria che offre una combinazione univoca di capacità e persistenza di grandi dimensioni. Questo argomento fornisce informazioni di base su PMem e i passaggi per distribuirlo con Windows Server 2019 con Spazi di archiviazione diretta.

## <a name="background"></a>Sfondo

PMem è un tipo di DRAM non volatile (NVDIMM) con la velocità della DRAM, ma mantiene il contenuto della memoria attraverso i cicli di alimentazione (il contenuto della memoria rimane anche quando l'alimentazione del sistema si interrompe in caso di interruzione imprevista dell'alimentazione, arresto avviato dall'utente, arresto anomalo del sistema, e così via). Per questo motivo, la ripresa dal punto in cui è stato interrotto è notevolmente più veloce, perché non è necessario ricaricare il contenuto della RAM. Un'altra caratteristica univoca è che PMem è indirizzabile ai byte, il che significa che è possibile usarlo anche come risorsa di archiviazione (per questo motivo è possibile che PMem venga definito memoria della classe di archiviazione).


Per visualizzare alcuni di questi vantaggi, esaminiamo la demo di Microsoft Ignite 2018:

[![Demo di Microsoft Ignite 2018 PMEM](http://img.youtube.com/vi/8WMXkMLJORc/0.jpg)](http://www.youtube.com/watch?v=8WMXkMLJORc)

Qualsiasi sistema di archiviazione che fornisce tolleranza di errore crea necessariamente copie distribuite delle Scritture, che devono attraversare la rete e l'amplificazione di scrittura back-end. Per questo motivo, i numeri di benchmark di IOPS di dimensioni assolute vengono in genere realizzati solo con le letture, soprattutto se il sistema di archiviazione dispone di ottimizzazioni comuni per la lettura dalla copia locale, laddove possibile, che Spazi di archiviazione diretta.

**Con le letture del 100%, il cluster recapita 13.798.674 IOPS.**

![schermata del record IOPS da 13,7 m](media/deploy-pmem/iops-record.png)

Se si guarda attentamente il video, si noterà che questoche è ancora più importante per la rimozione della mascella: anche con oltre 13,7 M di IOPS, il filesystem in Windows segnala una latenza costantemente inferiore a 40 μs! Si tratta del simbolo per i microsecondi, un milionesimo di secondo. Si tratta di un ordine di grandezza più veloce rispetto a quello che i normali fornitori di tutti i flash pubblicizzano oggi.

Insieme, Spazi di archiviazione diretta in Windows Server 2019 e Intel® Optane™ controller di dominio persistente forniscono prestazioni eccezionali. Questo benchmark di HCI leader nel settore di oltre 13,7 milioni di IOPS, con latenza prevedibile ed estremamente bassa, è più del doppio del precedente benchmark leader nel settore di 6.7 milioni di IOPS. Questa volta sono necessari solo 12 nodi server, il 25% meno di due anni fa.

![Guadagni IOPS](media/deploy-pmem/iops-gains.png)

L'hardware utilizzato era un cluster a 12 server che utilizzava il mirroring a tre vie e i volumi ReFS delimitati, **12** x Intel® S2600WFT, **384 GIB** Memory, 2 x 28 core "CASCADELAKE", **1,5 TB** Intel® Optane™ DC persistent Memory As cache, **32 TB** NVMe ( 4 x 8 TB Intel® DC P4510) As Capacity, **2** x Mellanox ConnectX-4 25 Gbps

La tabella seguente presenta i numeri di prestazioni completi: 

| Riferimento                   | Prestazioni         |
|-----------------------------|---------------------|
| Lettura casuale 4K 100%         | 13,8 milioni IOPS   |
| 4K 90/10% di lettura/scrittura casuale | 9.450.000 IOPS   |
| 2MB lettura sequenziale         | Velocità effettiva 549 GB/s |

### <a name="supported-hardware"></a>Hardware supportato

La tabella seguente mostra l'hardware di memoria permanente supportato per Windows Server 2019 e Windows Server 2016. Si noti che Intel Optane supporta specificamente sia la modalità di memoria sia la modalità app-Direct. Windows Server 2019 supporta operazioni in modalità mista.

| Tecnologia di memoria permanente                                      | Windows Server 2016 | Windows Server 2019 |
|-------------------------------------------------------------------|--------------------------|--------------------------|
| **NVDIMM-N** in modalità app-Direct                                       | Supportato                | Supportato                |
| **Intel Optane™ DC Persistent Memory** in modalità app-Direct             | Non supportato            | Supportato                |
| **Memoria persistente Intel Optane™ controller** di dominio in modalità di memoria a due livelli (2LM) | Non supportato            | Supportato                |

A questo punto, è possibile approfondire la configurazione della memoria persistente.

## <a name="interleave-sets"></a>Set di interfoliazione

### <a name="understanding-interleave-sets"></a>Informazioni sui set di interfoliazione

Tenere presente che NVDIMM-N risiede in uno slot standard DIMM (memoria), inserendo i dati più vicini al processore, riducendo così la latenza e il recupero di prestazioni migliori. Per eseguire questa operazione, un set di Interleave è quando due o più NVDIMMs creano un set di interfoliazione a N vie per fornire operazioni di lettura/scrittura a strisce per una maggiore velocità effettiva. Le configurazioni più comuni sono l'interfoliazione bidirezionale o a 4 vie.

I set con interfoliazione possono spesso essere creati in un BIOS della piattaforma per fare in modo che più dispositivi di memoria permanenti vengano visualizzati come un singolo disco logico in Windows Server. Ogni disco logico di memoria persistente contiene un set di dispositivi fisici con interfoliazione eseguendo:

```PowerShell
Get-PmemDisk
```

Di seguito è riportato un esempio di output:

```
DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Healthy      None          True         {20, 120}         0
3          252 GB Healthy      None          True         {1020, 1120}      0
```

Si noterà che il disco PMEM logico #2 dispone di dispositivi fisici di Id20 e Id120 e del disco PMEM logico #3 dispone di dispositivi fisici Id1020 e Id1120. È anche possibile inviare un disco PMEM specifico a Get-PmemPhysicalDevice per ottenere tutti i NVDIMMs fisici nel set di interfoliazione come indicato di seguito.


```PowerShell
(Get-PmemDisk)[0] | Get-PmemPhysicalDevice
```

Di seguito è riportato un esempio di output:

```
DeviceId DeviceType           HealthStatus OperationalStatus PhysicalLocation FirmwareRevision Persistent memory size Volatile memory size
-------- ----------           ------------ ----------------- ---------------- ---------------- ---------------------- --------------------
20       Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_C1     102005310        126 GB                 0 GB
120      Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_F1     102005310        126 GB                 0 GB
```

### <a name="configuring-interleave-sets"></a>Configurazione di set di interfoliazione

Per configurare un set di Interleave, eseguire il cmdlet PowerShell seguente:

```PowerShell
Get-PmemUnusedRegion

RegionId TotalSizeInBytes DeviceId
-------- ---------------- --------
       1     270582939648 {20, 120}
       3     270582939648 {1020, 1120}
```

Mostra tutte le aree di memoria permanenti non assegnate a un disco di memoria persistente logico nel sistema.

Per visualizzare tutte le informazioni sui dispositivi di memoria permanenti nel sistema, inclusi il tipo di dispositivo, la posizione, l'integrità e lo stato operativo e così via, è possibile eseguire il cmdlet seguente sul server locale:

```PowerShell
Get-PmemPhysicalDevice

DeviceId DeviceType           HealthStatus OperationalStatus PhysicalLocation FirmwareRevision Persistent memory size Volatile
                                                                                                                      memory size
-------- ----------           ------------ ----------------- ---------------- ---------------- ---------------------- --------------
1020     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_C1     102005310        126 GB                 0 GB
1120     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_F1     102005310        126 GB                 0 GB
120      Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_F1     102005310        126 GB                 0 GB
20       Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_C1     102005310        126 GB                 0 GB
```

Poiché è disponibile un'area PMEM inutilizzata, è possibile creare nuovi dischi di memoria permanenti. È possibile creare più dischi di memoria permanenti usando le aree inutilizzate:

```PowerShell
Get-PmemUnusedRegion | New-PmemDisk
Creating new persistent memory disk. This may take a few moments.
```

A questo scopo, è possibile visualizzare i risultati eseguendo:

```PowerShell
Get-PmemDisk

DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Healthy      None          True         {20, 120}         0
3          252 GB Healthy      None          True         {1020, 1120}      0
```

Vale la pena notare che è possibile eseguire **Get-PhysicalDisk | Dove MediaType-EQ SCM** invece di **Get-PmemDisk** per ottenere gli stessi risultati. Il disco di memoria persistente appena creato corrisponde a 1:1 alle unità visualizzate in PowerShell e nell'interfaccia di amministrazione di Windows.

### <a name="using-persistent-memory-for-cache-or-capacity"></a>Uso della memoria persistente per la cache o la capacità

Spazi di archiviazione diretta in Windows Server 2019 supporta l'uso di memoria permanente come unità cache o capacità. Per ulteriori informazioni sulla configurazione delle unità cache e capacità, vedere la [documentazione](understand-the-cache.md) .

## <a name="creating-a-dax-volume"></a>Creazione di un volume DAX

### <a name="understanding-dax"></a>Informazioni su DAX

Esistono due metodi per accedere alla memoria persistente. Sono:

1. **Accesso diretto (DAX)** , che funziona come la memoria per ottenere la latenza più bassa. L'app modifica direttamente la memoria persistente, ignorando lo stack. Si noti che questa operazione può essere utilizzata solo con NTFS.
2. **Blocca l'accesso**, che funziona come l'archiviazione per la compatibilità delle app. I dati passano attraverso lo stack in questa configurazione e possono essere usati con NTFS e ReFS.

Di seguito è riportato un esempio:

![Stack DAX](media/deploy-pmem/dax.png)

### <a name="configuring-dax"></a>Configurazione di DAX

È necessario usare i cmdlet di PowerShell per creare un volume DAX in una memoria permanente. Utilizzando l'opzione **-IsDax** , è possibile formattare un volume in modo che sia abilitato DAX.

```PowerShell
Format-Volume -IsDax:$true
```

Il seguente frammento di codice consente di creare un volume DAX in un disco di memoria permanente.

```PowerShell
# Here we use the first pmem disk to create the volume as an example
$disk = (Get-PmemDisk)[0] | Get-PhysicalDisk | Get-Disk
# Initialize the disk to GPT if it is not initialized
If ($disk.partitionstyle -eq "RAW") {$disk | Initialize-Disk -PartitionStyle GPT}
# Create a partition with drive letter 'S' (can use any available drive letter)
$disk | New-Partition -DriveLetter S -UseMaximumSize


   DiskPath: \\?\scmld#ven_8980&dev_097a&subsys_89804151&rev_0018#3&1b1819f6&0&03018089fb63494db728d8418b3cbbf549997891#{53f56307-b6
bf-11d0-94f2-00a0c91efb8b}

PartitionNumber  DriveLetter Offset                                               Size Type
---------------  ----------- ------                                               ---- ----
2                S           16777216                                        251.98 GB Basic

# Format the volume with drive letter 'S' to DAX Volume
Format-Volume -FileSystem NTFS -IsDax:$true -DriveLetter S

DriveLetter FriendlyName FileSystemType DriveType HealthStatus OperationalStatus SizeRemaining      Size
----------- ------------ -------------- --------- ------------ ----------------- -------------      ----
S                        NTFS           Fixed     Healthy      OK                    251.91 GB 251.98 GB

# Verify the volume is DAX enabled
Get-Partition -DriveLetter S | fl


UniqueId             : {00000000-0000-0000-0000-000100000000}SCMLD\VEN_8980&DEV_097A&SUBSYS_89804151&REV_0018\3&1B1819F6&0&03018089F
                       B63494DB728D8418B3CBBF549997891:WIN-8KGI228ULGA
AccessPaths          : {S:\, \\?\Volume{cf468ffa-ae17-4139-a575-717547d4df09}\}
DiskNumber           : 2
DiskPath             : \\?\scmld#ven_8980&dev_097a&subsys_89804151&rev_0018#3&1b1819f6&0&03018089fb63494db728d8418b3cbbf549997891#{5
                       3f56307-b6bf-11d0-94f2-00a0c91efb8b}
DriveLetter          : S
Guid                 : {cf468ffa-ae17-4139-a575-717547d4df09}
IsActive             : False
IsBoot               : False
IsHidden             : False
IsOffline            : False
IsReadOnly           : False
IsShadowCopy         : False
IsDAX                : True                   # <- True: DAX enabled
IsSystem             : False
NoDefaultDriveLetter : False
Offset               : 16777216
OperationalStatus    : Online
PartitionNumber      : 2
Size                 : 251.98 GB
Type                 : Basic
```

## <a name="monitoring-health"></a>Monitoraggio dello stato

Quando si usa la memoria persistente, esistono alcune differenze nell'esperienza di monitoraggio:

1. La memoria persistente non crea contatori delle prestazioni del disco fisico, pertanto non verrà visualizzato se nei grafici nell'interfaccia di amministrazione di Windows.
2. La memoria persistente non crea dati Storport 505, pertanto non si otterrà il rilevamento proattivo di outlier.

A parte questo, l'esperienza di monitoraggio è identica a qualsiasi altro disco fisico. È possibile eseguire una query per lo stato di un disco di memoria persistente eseguendo:

```PowerShell
Get-PmemDisk

DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Unhealthy    None          True         {20, 120}         2
3          252 GB Healthy      None          True         {1020, 1120}      0

Get-PmemDisk | Get-PhysicalDisk | select SerialNumber, HealthStatus, OperationalStatus, OperationalDetails

SerialNumber               HealthStatus OperationalStatus  OperationalDetails
------------               ------------ ------------------ ------------------
802c-01-1602-117cb5fc      Healthy      OK
802c-01-1602-117cb64f      Warning      Predictive Failure {Threshold Exceeded,NVDIMM_N Error}
```

**HealthStatus** indica se il disco di memoria persistente è integro o meno. Il **UnsafeshutdownCount** tiene traccia del numero di arresti che possono causare la perdita di dati su questo disco logico. Si tratta della somma dei conteggi di arresto non sicuro di tutti i dispositivi di memoria permanenti sottostanti del disco. È anche possibile usare i comandi seguenti per eseguire query sulle informazioni sull'integrità. **OperationalStatus** e **OperationalDetails** forniscono ulteriori informazioni sullo stato di integrità.

Per eseguire una query sullo stato del dispositivo di memoria persistente:

```PowerShell
Get-PmemPhysicalDevice

DeviceId DeviceType           HealthStatus OperationalStatus PhysicalLocation FirmwareRevision Persistent memory size Volatile memory size
-------- ----------           ------------ ----------------- ---------------- ---------------- ---------------------- --------------------
1020     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_C1     102005310        126 GB                 0 GB
1120     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_F1     102005310        126 GB                 0 GB
120      Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_F1     102005310        126 GB                 0 GB
20       Intel INVDIMM device Unhealthy    {HardwareError}   CPU1_DIMM_C1     102005310        126 GB                 0 GB
```

Viene visualizzato il dispositivo di memoria permanente non integro. Il dispositivo non integro (**DeviceID**) 20 corrisponde al caso nell'esempio precedente. **PhysicalLocation** dal BIOS consente di identificare quale dispositivo di memoria persistente è in stato di errore.

## <a name="replacing-persistent-memory"></a>Sostituzione della memoria persistente

Sopra è stato descritto come visualizzare lo stato di integrità della memoria persistente. Se è necessario sostituire un modulo che ha avuto esito negativo, sarà necessario eseguire nuovamente il provisioning del disco di memoria persistente (fare riferimento ai passaggi illustrati in precedenza).

Per la risoluzione dei problemi, potrebbe essere necessario usare **Remove-PmemDisk**, che rimuove un disco di memoria persistente specifico. È possibile rimuovere tutti i dischi permanenti correnti per:

```PowerShell
Get-PmemDisk | Remove-PmemDisk

cmdlet Remove-PmemDisk at command pipeline position 1
Supply values for the following parameters:
DiskNumber: 2

This will remove the persistent memory disk(s) from the system and will result in data loss.
Remove the persistent memory disk(s)?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): Y
Removing the persistent memory disk. This may take a few moments.
```

È importante tenere presente che la rimozione di un disco di memoria persistente comporterà la perdita di dati su tale disco.

Un altro cmdlet potrebbe essere necessario è **Initialize-PmemPhysicalDevice**, che consente di inizializzare l'area di archiviazione delle etichette nei dispositivi di memoria permanenti fisici. Questa operazione può essere usata per cancellare le informazioni sull'archiviazione delle etichette danneggiate nei dispositivi di memoria permanenti.

```PowerShell
Get-PmemPhysicalDevice | Initialize-PmemPhysicalDevice

This will initialize the label storage area on the physical persistent memory device(s) and will result in data loss.
Initializes the physical persistent memory device(s)?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): A
Initializing the physical persistent memory device. This may take a few moments.
Initializing the physical persistent memory device. This may take a few moments.
Initializing the physical persistent memory device. This may take a few moments.
Initializing the physical persistent memory device. This may take a few moments.
```

È importante notare che questo comando deve essere usato come ultima risorsa per correggere i problemi persistenti correlati alla memoria. Questa operazione comporterà la perdita di dati nella memoria persistente.

## <a name="see-also"></a>Vedere anche

- [Panoramica di Spazi di archiviazione diretta](storage-spaces-direct-overview.md)
- [Gestione dell'integrità della memoria della classe di archiviazione (NVDIMM-N) in Windows](storage-class-memory-health.md)
- [Informazioni sulla cache](understand-the-cache.md)