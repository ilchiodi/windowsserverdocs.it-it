---
title: Comprendere e distribuire memoria persistente
description: Informazioni dettagliate su quali memoria persistente è e come configurarlo con spazi di archiviazione diretta in Windows Server 2019.
keywords: Spazi di archiviazione diretta, memoria persistente, pmem, archiviazione, S2D
ms.assetid: ''
ms.prod: ''
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 3/26/2019
ms.localizationpriority: ''
ms.openlocfilehash: ed4b2669ad35a2ce0f818c65f7024ce905d9e4d6
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280039"
---
---
# <a name="understand-and-deploy-persistent-memory"></a>Comprendere e distribuire memoria persistente

>Si applica a: Windows Server 2019

Memoria persistente (o PMem) è un nuovo tipo di tecnologia di memoria che offre una combinazione univoca di grande capacità convenienti e la persistenza. In questo argomento illustra PMem e i passaggi per distribuirlo con Windows Server 2019 con spazi di archiviazione diretta.

## <a name="background"></a>Informazioni

PMem è un tipo di comandi non volatile DRAM (NVDIMM) che ha la velocità di DRAM, ma conserva il contenuto di memoria tramite cicli di alimentazione (il contenuto della memoria rimanga anche quando l'alimentazione del sistema si arresta in caso di un imprevista dell'alimentazione, un arresto avviato dall'utente, un arresto anomalo del sistema, e così via.). Per questo motivo, riprendere da dove era stato interrotto è notevolmente più veloce, perché non è necessario ricaricare il contenuto della RAM. Un'altra caratteristica univoca è PMem byte indirizzabili, ovvero che è possibile usare anche come risorsa di archiviazione (che è il motivo per cui si può sentire PMem cui viene fatto riferimento alla memoria della classe di archiviazione).


Per visualizzare alcuni di questi vantaggi, esaminiamo questa demo di Microsoft Ignite 2018:

[![Microsoft Ignite 2018 Pmem demo](http://img.youtube.com/vi/8WMXkMLJORc/0.jpg)](http://www.youtube.com/watch?v=8WMXkMLJORc)

Qualsiasi sistema di archiviazione fornisce tolleranza d'errore necessariamente crea copie distribuite di scritture, che devono attraversare la rete e comporta amplificazione scrittura back-end. Per questo motivo, i numeri di benchmark di IOPS massimi assoluti in genere si ottengono con operazioni di lettura solo, soprattutto se il sistema di archiviazione è disponibili ottimizzazioni di comune buon senso per leggere dalla copia locale quando possibile, quali spazi di archiviazione diretta viene.

**Grazie al 100% di letture, il cluster offre 13,798,674 IOPS.**

![schermata record IOPS 13,7 m](media/deploy-pmem/iops-record.png)

Se si guarda il video attentamente, si noterà che ulteriormente della thatwhat strabiliante è la latenza: anche a oltre 13,7 IOPS M, il file System in Windows invia report di latenza è costantemente inferiore a 40 µs! (Che è il simbolo per microsecondi, milionesimo di secondo). Si tratta un ordine di grandezza superiore alla velocità quali fornitori all-flash tipici con orgoglio annunciano oggi stesso.

Insieme, spazi di archiviazione diretta in Windows Server 2019 e memoria persistente di controller di dominio di Intel® Optane™ offrono prestazioni all'avanguardia. Questo benchmark uomo leader di settore di oltre 13,7 M IOPS, con una latenza estremamente bassa e prevedibile, è più del doppio nostro precedente benchmark leader di settore di M 6.7 IOPS. Inoltre, questa volta eravamo solo 12 nodi di server, 25% inferiore rispetto a due anni fa.

![Miglioramenti IOPS](media/deploy-pmem/iops-gains.png)

L'hardware usato è stato un cluster 12-server con mirroring a tre vie e delimitato volumi ReFS **12** x Intel® S2600WFT, **GiB 384** memoria, core 2 x 28 "CascadeLake" **1,5 TB**Memoria persistente Intel® Optane™ DC cache **32 TB** NVMe (4 x 8 P4510 TB Intel® DC) come capacità **2** x Mellanox ConnectX-4 25 Gbps

La tabella seguente contiene i numeri delle prestazioni completi: 

| Benchmark                   | Prestazioni         |
|-----------------------------|---------------------|
| Lettura casuale 100% di 4 KB         | 13,8 milioni di operazioni IOPS   |
| 4 K 90/10% in lettura/scrittura casuali | 9.45 milione di IOPS   |
| Lettura sequenziale di 2MB         | 549 GB/s di velocità effettiva |

### <a name="supported-hardware"></a>Hardware supportato

La tabella seguente mostra l'hardware supportati memoria persistente per Windows Server 2016 e Windows Server 2019. Si noti che Intel Optane supporta in modo specifico sia in modalità di memoria in modalità di app con accesso diretto. Windows Server 2019 supporta le operazioni in modalità mista.

| Tecnologia di memoria persistente                                      | Windows Server 2016 | Windows Server 2019 |
|-------------------------------------------------------------------|--------------------------|--------------------------|
| **NVDIMM-N** in modalità di App con accesso diretto                                       | Supportato                | Supportato                |
| **Memoria persistente Optane™ DC Intel** in modalità di App con accesso diretto             | Non supportato            | Supportato                |
| **Memoria persistente Optane™ DC Intel** in modalità a livello di memoria esaurita due (2LM) | Non supportato            | Supportato                |

A questo punto, passiamo la configurazione di memoria persistente.

## <a name="interleave-sets"></a>Interleave set

### <a name="understanding-interleave-sets"></a>Informazioni su quelle con interleave set

È importante ricordare che il dispositivo NVDIMM-N si trova in uno slot DIMM (memoria) standard, inserendo i dati del processore (in questo modo, riducendo la latenza e prestazioni migliori di recupero). Per compilare su questo, un set di interleave è quando due o più NVDIMMs crea un'interfoliazione di N-Way impostate per fornire operazioni di lettura/scrittura strisce per migliorare la produttività. Le configurazioni più comuni sono l'interleave vie 2 o 4 vie.

Set interfogliati possono spesso essere creati nel BIOS della piattaforma per rendere più dispositivi di memoria persistente vengono visualizzati come un singolo disco logico Windows Server. Ogni disco logico di memoria persistente contiene un set interfogliato dei dispositivi fisici tramite l'esecuzione:

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

Noteremo che il disco logico pmem #2 dispositivi fisici di Id20 e Id120, che il disco logico pmem #3 ha dispositivi fisici di Id1020 e Id1120. È anche possibile feed un disco specifico pmem a Get-PmemPhysicalDevice per ottenere tutti i relativi NVDIMMs fisica nell'interleave impostata come indicato di seguito.


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

### <a name="configuring-interleave-sets"></a>Configurazione di interleave set

Per configurare un set di interleave, eseguire il cmdlet di PowerShell seguente:

```PowerShell
Get-PmemUnusedRegion

RegionId TotalSizeInBytes DeviceId
-------- ---------------- --------
       1     270582939648 {20, 120}
       3     270582939648 {1020, 1120}
```

Mostra tutte le aree di memoria persistente non è assegnate a un disco logico di memoria persistente nel sistema.

Per visualizzare tutte le informazioni di dispositivi di memoria persistente nel sistema, compresi tipo di dispositivo, posizione, integrità e lo stato operativo e così via è possibile eseguire il cmdlet seguente nel server locale:

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

Poiché sono presenti area disponibile pmem inutilizzati, è possibile creare nuovi dischi memoria persistente. È possibile creare più dischi persistenti memoria usando le aree inutilizzate da:

```PowerShell
Get-PmemUnusedRegion | New-PmemDisk
Creating new persistent memory disk. This may take a few moments.
```

Dopo che questa operazione viene eseguita, è possibile osservare i risultati eseguendo:

```PowerShell
Get-PmemDisk

DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Healthy      None          True         {20, 120}         0
3          252 GB Healthy      None          True         {1020, 1120}      0
```

Vale la pena notare che potremmo avere eseguiamo **Get-PhysicalDisk | In cui MediaType - Eq SCM** invece di **Get-PmemDisk** per ottenere gli stessi risultati. Il disco appena creato memoria persistente corrisponde 1:1 per unità visualizzate nella PowerShell e Windows Admin Center.

### <a name="using-persistent-memory-for-cache-or-capacity"></a>Utilizzo di memoria persistente per la cache o la capacità

Spazi di archiviazione diretta in Windows Server 2019 supporta l'utilizzo di memoria persistente come una cache o una capacità di unità. Vedere questo [documentazione](understand-the-cache.md) per altri dettagli su come configurare le unità di capacità e della cache.

## <a name="creating-a-dax-volume"></a>Creazione di un Volume DAX

### <a name="understanding-dax"></a>Informazioni su DAX

Esistono due metodi per l'accesso a memoria persistente. Sono:

1. **DirectAccess (DAX)** , che agisce come memoria per ottenere la latenza più bassa. L'app modifica direttamente la memoria persistente, ignorando lo stack. Si noti che questo può essere utilizzato solo con NTFS.
2. **Bloccare l'accesso**, che agisce come archiviazione per la compatibilità dell'applicazione. I dati passano attraverso lo stack in questa configurazione, e può essere usato con NTFS e ReFS.

Di seguito è riportato un esempio:

![Stack DAX](media/deploy-pmem/dax.png)

### <a name="configuring-dax"></a>Configurazione di DAX

È necessario usare i cmdlet di PowerShell per creare un volume DAX in una memoria persistente. Tramite il **- IsDax** switch, è possibile formattare un volume da DAX abilitata.

```PowerShell
Format-Volume -IsDax:$true
```

Il seguente frammento di codice ti aiuterà a creare un volume DAX su un disco persistente di memoria.

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

## <a name="monitoring-health"></a>Monitoraggio dell'integrità

Quando si utilizza memoria persistente, esistono alcune differenze nell'esperienza di monitoraggio:

1. Memoria persistente non è possibile creare contatori delle prestazioni, in modo che non verrà visualizzato se vengono visualizzati nei grafici in Windows Admin Center del disco fisico.
2. Memoria persistente non crea dati 505 Storport, in modo che non si otterranno rilevamento proattivo degli outlier.

A parte ciò, l'esperienza di monitoraggio è lo stesso come qualsiasi altro disco fisico. È possibile eseguire una query per l'integrità di un disco persistente di memoria eseguendo:

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

**HealthStatus** Mostra se il disco di memoria persistente è integro o meno. Il **UnsafeshutdownCount** tiene traccia del numero di arresti che può causare la perdita di dati su questo disco logico. È la somma dei conteggi unsafe arresto di tutti i dispositivi di memoria persistente sottostante di questo disco. È anche possibile usare i comandi seguenti per informazioni sull'integrità di query. Il **OperationalStatus** e **OperationalDetails** vengono fornite ulteriori informazioni sullo stato di integrità.

Per eseguire una query di integrità del dispositivo di memoria persistente:

```PowerShell
Get-PmemPhysicalDevice

DeviceId DeviceType           HealthStatus OperationalStatus PhysicalLocation FirmwareRevision Persistent memory size Volatile memory size
-------- ----------           ------------ ----------------- ---------------- ---------------- ---------------------- --------------------
1020     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_C1     102005310        126 GB                 0 GB
1120     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_F1     102005310        126 GB                 0 GB
120      Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_F1     102005310        126 GB                 0 GB
20       Intel INVDIMM device Unhealthy    {HardwareError}   CPU1_DIMM_C1     102005310        126 GB                 0 GB
```

Ciò indica quale dispositivo di memoria persistente non è integro. Il dispositivo non integri (**DeviceId**) 20 corrisponde a quello nell'esempio precedente. Il **PhysicalLocation** dal BIOS può consentire di identificare quali memoria persistente dispositivo è in uno stato difettoso.

## <a name="replacing-persistent-memory"></a>Sostituzione di memoria persistente

Sopra è stato illustrato come visualizzare lo stato di integrità di memoria persistente. Se è necessario sostituire un modulo guasto, è necessario rieseguire il provisioning del disco di memoria persistente (vedere i passaggi sono descritti in precedenza).

Per risolvere il problema, potrebbe essere necessario usare **Remove-PmemDisk**, che rimuove un disco specifico memoria persistente. È possibile rimuovere tutti i dischi persistenti correnti da:

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

È importante notare che la rimozione di un disco persistente di memoria comporterà la perdita di dati presenti su tale disco.

È un altro cmdlet potrebbe essere necessario **Initialize-PmemPhysicalDevice**, che verrà inizializzato l'area di archiviazione di etichetta nei dispositivi fisici memoria persistente. Utilizzabile per cancellare informazioni di archiviazione danneggiato etichetta nei dispositivi di memoria persistente.

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

È importante notare che questo comando deve essere utilizzato come ultima risorsa correggere memoria persistente problemi correlati. Si verificherà una perdita di dati per la memoria persistente.

## <a name="see-also"></a>Vedere anche

- [Panoramica di spazi diretti di archiviazione](storage-spaces-direct-overview.md)
- [Gestione dell'integrità della memoria (memoria NVDIMM-N)-classe di archiviazione in Windows](storage-class-memory-health.md)
- [Informazioni sulla cache](understand-the-cache.md)