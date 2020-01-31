---
title: Comprendere e distribuire memoria persistente
description: Informazioni dettagliate sulla memoria persistente e su come configurarla con spazi di archiviazione diretta in Windows Server 2019.
keywords: Spazi di archiviazione diretta, memoria persistente, PMEM, archiviazione, S2D
ms.prod: windows-server
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 1/27/2020
ms.localizationpriority: medium
ms.openlocfilehash: a9070d2e2ab73c7882f4b2ef585ccb01986695bb
ms.sourcegitcommit: 07c9d4ea72528401314e2789e3bc2e688fc96001
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2020
ms.locfileid: "76822314"
---
# <a name="understand-and-deploy-persistent-memory"></a>Comprendere e distribuire memoria persistente

> Si applica a: Windows Server 2019

La memoria persistente (o PMem) è un nuovo tipo di tecnologia di memoria che offre una combinazione univoca di capacità e persistenza di grandi dimensioni. Questo articolo fornisce informazioni di base su PMem e i passaggi per distribuirlo in Windows Server 2019 usando Spazi di archiviazione diretta.

## <a name="background"></a>Informazioni

PMem è un tipo di RAM non volatile (NVDIMM) che mantiene il contenuto tramite cicli di alimentazione. Il contenuto della memoria rimane anche quando l'alimentazione del sistema si interrompe in caso di interruzione imprevista dell'alimentazione, arresto avviato dall'utente, arresto anomalo del sistema e così via. Questa caratteristica univoca significa che è anche possibile usare PMem come risorsa di archiviazione. Questo è il motivo per cui è possibile che gli utenti facciano riferimento a PMem come "memoria della classe di archiviazione".

Per visualizzare alcuni di questi vantaggi, esaminiamo la demo seguente di Microsoft Ignite 2018.

[demo di ![Microsoft Ignite 2018 PMEM](http://img.youtube.com/vi/8WMXkMLJORc/0.jpg)](http://www.youtube.com/watch?v=8WMXkMLJORc)

Qualsiasi sistema di archiviazione che fornisce tolleranza di errore crea necessariamente copie distribuite delle Scritture. Tali operazioni devono attraversare la rete e amplificare il traffico di scrittura back-end. Per questo motivo, i numeri di benchmark di IOPS di dimensioni assolute sono in genere ottenibili misurando solo le letture, soprattutto se il sistema di archiviazione ha ottimizzazioni comuni per leggere la copia locale quando possibile. Spazi di archiviazione diretta è ottimizzato per questa operazione.

**Quando viene misurato usando solo le operazioni di lettura, il cluster recapita 13.798.674 IOPS.**

![schermata del record IOPS da 13,7 m](media/deploy-pmem/iops-record.png)

Se si guarda attentamente il video, si noterà che ciò che è ancora più importante è la latenza. Anche con oltre 13,7 milioni di IOPS, il file system in Windows segnala una latenza costantemente inferiore a 40 μs! Si tratta del simbolo per i microsecondi, un milionesimo di secondo. Questa velocità è un ordine di grandezza più veloce rispetto a quello che i normali fornitori di tutti i flash pubblicizzano oggi.

Insieme, Spazi di archiviazione diretta in Windows Server 2019 e Intel® Optane™ controller di dominio persistente forniscono prestazioni eccezionali. Questo benchmark di HCI leader nel settore di oltre 13,7 milioni di IOPS, accompagnato da una latenza prevedibile e estremamente bassa, è più del doppio del precedente benchmark leader nel settore di 6.7 milioni di IOPS. Questa volta sono necessari solo 12 nodi server&mdash;il 25% meno di due anni fa.

![Guadagni IOPS](media/deploy-pmem/iops-gains.png)

L'hardware di test era un cluster a 12 server configurato per l'utilizzo del mirroring a tre vie e dei volumi ReFS delimitati, **12** x Intel® S2600WFT, **384 GIB** Memory, 2 x 28 core "CascadeLake" **1,5 TB** Intel® Optane™ DC persistent Memory As cache, **32 TB** NVME (4 x 8 TB Intel® DC P4510) As capacity, **2** x Mellanox ConnectX-4 25 Gbps.

La tabella seguente illustra i numeri di prestazioni completi.  

| Riferimento                   | Performance         |
|-----------------------------|---------------------|
| lettura casuale 4K 100%         | 13,8 milioni IOPS   |
| 4K 90/10% di lettura/scrittura casuale | 9.450.000 IOPS   |
| lettura sequenziale 2 MB         | velocità effettiva 549 GB/s |

### <a name="supported-hardware"></a>Hardware supportato

La tabella seguente illustra l'hardware di memoria persistente supportato per Windows Server 2019 e Windows Server 2016.  

| Tecnologia di memoria permanente                                      | Windows Server 2016 | Windows Server 2019 |
|-------------------------------------------------------------------|--------------------------|--------------------------|
| **NVDIMM-N** in modalità persistente                                  | Funzionalità supportata                | Funzionalità supportata                |
| **Memoria persistente Intel Optane™ controller** di dominio in modalità app diretta             | Non Supportato            | Funzionalità supportata                |
| **Memoria persistente Intel Optane™ controller** di dominio in modalità memoria | Funzionalità supportata            | Funzionalità supportata                |

> [!NOTE]  
> Intel Optane supporta sia la *memoria* (volatile) che la modalità *app diretta* (permanente).
   
> [!NOTE]  
> Quando si riavvia un sistema con più moduli Intel® Optane™ PMem in modalità app diretta divisi in più spazi dei nomi, è possibile che si perda l'accesso ad alcuni o a tutti i dischi di archiviazione logica correlati. Questo problema si verifica nelle versioni di Windows Server 2019 precedenti alla versione 1903.
>   
> Questa perdita di accesso si verifica perché non è stato eseguito il training di un modulo PMem o ha esito negativo all'avvio del sistema. In tal caso, tutti gli spazi dei nomi di archiviazione in qualsiasi modulo PMem nel sistema hanno esito negativo, inclusi gli spazi dei nomi che non eseguono fisicamente il mapping al modulo in cui si è verificato l'errore.
>   
> Per ripristinare l'accesso a tutti gli spazi dei nomi, sostituire il modulo che ha avuto esito negativo.
>   
> Se un modulo ha esito negativo in Windows Server 2019 versione 1903 o versioni successive, si perde l'accesso solo agli spazi dei nomi che si mappano fisicamente al modulo interessato. Altri spazi dei nomi non sono interessati.

A questo punto, è possibile approfondire la configurazione della memoria persistente.

## <a name="interleaved-sets"></a>Set con interfoliazione

### <a name="understanding-interleaved-sets"></a>Informazioni sui set con interfoliazione

Tenere presente che un NVDIMM risiede in uno slot standard DIMM (memoria), che consente di avvicinare i dati al processore. Questa configurazione riduce la latenza e migliora le prestazioni di recupero. Per aumentare ulteriormente la velocità effettiva, due o più NVDIMMs creano un set di interfoliazione a n modi per eseguire lo striping delle operazioni di lettura/scrittura. Le configurazioni più comuni sono l'interfoliazione bidirezionale o a quattro vie. Un set con interfoliazione consente inoltre di visualizzare più dispositivi di memoria permanenti come singolo disco logico in Windows Server. È possibile usare il cmdlet **Get-PmemDisk** di Windows PowerShell per esaminare la configurazione di tali dischi logici, come indicato di seguito:

```PowerShell
Get-PmemDisk

DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Healthy      None          True         {20, 120}         0
3          252 GB Healthy      None          True         {1020, 1120}      0
```

Si noterà che il disco PMem logico #2 USA i dispositivi fisici Id20 e Id120 e il disco PMem logico #3 USA i dispositivi fisici Id1020 e Id1120.  

Per recuperare altre informazioni sul set di interfoliazione usato da un'unità logica, eseguire il cmdlet **Get-PmemPhysicalDevice** :

```PowerShell
(Get-PmemDisk)[0] | Get-PmemPhysicalDevice

DeviceId DeviceType           HealthStatus OperationalStatus PhysicalLocation FirmwareRevision Persistent memory size Volatile memory size
-------- ----------           ------------ ----------------- ---------------- ---------------- ---------------------- --------------------
20       Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_C1     102005310        126 GB                 0 GB
120      Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_F1     102005310        126 GB                 0 GB
```

### <a name="configuring-interleaved-sets"></a>Configurazione di set con interfoliazione

Per configurare un set con interfoliazione, iniziare esaminando tutte le aree di memoria permanenti che non sono assegnate a un disco PMem logico nel sistema. A tale scopo, eseguire il cmdlet PowerShell seguente:

```PowerShell
Get-PmemUnusedRegion

RegionId TotalSizeInBytes DeviceId
-------- ---------------- --------
       1     270582939648 {20, 120}
       3     270582939648 {1020, 1120}
```

Per visualizzare tutte le informazioni sul dispositivo PMem nel sistema, inclusi il tipo di dispositivo, la posizione, l'integrità e lo stato operativo e così via, eseguire il cmdlet seguente sul server locale:

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

Poiché è disponibile un'area PMem inutilizzata, è possibile creare nuovi dischi di memoria permanenti. È possibile usare l'area non usata per creare più dischi di memoria permanenti eseguendo i cmdlet seguenti:

```PowerShell
Get-PmemUnusedRegion | New-PmemDisk
Creating new persistent memory disk. This may take a few moments.
```

Al termine di questa operazione, è possibile visualizzare i risultati eseguendo:

```PowerShell
Get-PmemDisk

DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Healthy      None          True         {20, 120}         0
3          252 GB Healthy      None          True         {1020, 1120}      0
```

Vale la pena notare che è possibile eseguire **Get-PhysicalDisk | Dove MediaType-EQ SCM** invece di **Get-PmemDisk** per ottenere gli stessi risultati. Il disco PMem appena creato corrisponde a uno-a-uno con le unità visualizzate in PowerShell e nell'interfaccia di amministrazione di Windows.

### <a name="using-persistent-memory-for-cache-or-capacity"></a>Uso della memoria persistente per la cache o la capacità

Spazi di archiviazione diretta in Windows Server 2019 supporta l'uso di memoria permanente come cache o unità di capacità. Per ulteriori informazioni su come configurare le unità cache e capacità, vedere [informazioni sulla cache in spazi di archiviazione diretta](understand-the-cache.md).

## <a name="creating-a-dax-volume"></a>Creazione di un volume DAX

### <a name="understanding-dax"></a>Informazioni su DAX

Esistono due metodi per accedere alla memoria persistente. e sono:

1. **Accesso diretto (DAX)** , che funziona come la memoria per ottenere la latenza più bassa. L'app modifica direttamente la memoria persistente, ignorando lo stack. Si noti che è possibile utilizzare DAX solo in combinazione con NTFS.
1. **Blocca l'accesso**, che funziona come l'archiviazione per la compatibilità delle app. In questa configurazione, i dati passano attraverso lo stack. Questa configurazione può essere usata in combinazione con NTFS e ReFS.

Nella figura seguente viene illustrato un esempio di configurazione DAX:

![Stack DAX](media/deploy-pmem/dax.png)

### <a name="configuring-dax"></a>Configurazione di DAX

È necessario usare i cmdlet di PowerShell per creare un volume DAX in un disco di memoria permanente. Utilizzando l'opzione **-IsDax** , è possibile formattare un volume in modo che sia abilitato per DAX.

```PowerShell
Format-Volume -IsDax:$true
```

Il frammento di codice seguente consente di creare un volume DAX in un disco di memoria permanente.

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

- La memoria persistente non crea contatori delle prestazioni del disco fisico, quindi non verrà visualizzata nei grafici nell'interfaccia di amministrazione di Windows.
- La memoria persistente non crea dati Storport 505, pertanto non si otterrà il rilevamento proattivo di outlier.

A parte questo, l'esperienza di monitoraggio è identica a quella di qualsiasi altro disco fisico. È possibile eseguire una query per l'integrità di un disco di memoria persistente eseguendo i cmdlet seguenti:

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

**HealthStatus** indica se il disco PMem è integro.  

Il valore **UnsafeshutdownCount** tiene traccia del numero di arresti che possono causare la perdita di dati su questo disco logico. È la somma dei conteggi di arresto non sicuro di tutti i dispositivi PMem sottostanti del disco. Per ulteriori informazioni sullo stato di integrità, utilizzare il cmdlet **Get-PmemPhysicalDevice** per trovare informazioni come **OperationalStatus**.

```PowerShell
Get-PmemPhysicalDevice

DeviceId DeviceType           HealthStatus OperationalStatus PhysicalLocation FirmwareRevision Persistent memory size Volatile memory size
-------- ----------           ------------ ----------------- ---------------- ---------------- ---------------------- --------------------
1020     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_C1     102005310        126 GB                 0 GB
1120     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_F1     102005310        126 GB                 0 GB
120      Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_F1     102005310        126 GB                 0 GB
20       Intel INVDIMM device Unhealthy    {HardwareError}   CPU1_DIMM_C1     102005310        126 GB                 0 GB
```

Questo cmdlet Mostra quale dispositivo di memoria permanente non è integro. Il dispositivo non integro (**DeviceID** 20) corrisponde al caso nell'esempio precedente. Il **PhysicalLocation** nel BIOS consente di identificare quale dispositivo di memoria persistente è in stato di errore.

## <a name="replacing-persistent-memory"></a>Sostituzione della memoria persistente

Questo articolo descrive come visualizzare lo stato di integrità della memoria persistente. Se è necessario sostituire un modulo che ha avuto esito negativo, è necessario eseguire nuovamente il provisioning del disco PMem (vedere i passaggi descritti in precedenza).

Quando si esegue la risoluzione dei problemi, potrebbe essere necessario usare **Remove-PmemDisk**. Questo cmdlet rimuove un disco di memoria persistente specifico. È possibile rimuovere tutti i dischi PMem correnti eseguendo i cmdlet seguenti:

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

> [!IMPORTANT]  
> La rimozione di un disco di memoria persistente causa la perdita di dati su tale disco.

Un altro cmdlet potrebbe essere necessario è **Initialize-PmemPhysicalDevice**. Questo cmdlet Inizializza le aree di archiviazione delle etichette nei dispositivi di memoria permanenti fisici e può cancellare le informazioni di archiviazione delle etichette danneggiate sui dispositivi PMem.

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

> [!IMPORTANT]  
> **Initialize-PmemPhysicalDevice** causa la perdita di dati nella memoria persistente. Utilizzarlo come ultima risorsa per correggere i problemi persistenti correlati alla memoria.

## <a name="see-also"></a>Vedi anche

- [Panoramica di Spazi di archiviazione diretta](storage-spaces-direct-overview.md)
- [Gestione dell'integrità della memoria della classe di archiviazione (NVDIMM-N) in Windows](storage-class-memory-health.md)
- [Informazioni sulla cache](understand-the-cache.md)
