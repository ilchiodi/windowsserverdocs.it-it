---
title: Consente di delimitare l'allocazione dei volumi in spazi di archiviazione diretta
ms.author: cosmosdarwin
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 03/29/2018
ms.openlocfilehash: c93cbf4ba418f6702cf8747508605952d993508d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889052"
---
# <a name="delimit-the-allocation-of-volumes-in-storage-spaces-direct"></a>Consente di delimitare l'allocazione dei volumi in spazi di archiviazione diretta
> Si applica a: Windows Server Insider Preview build 17093 e versioni successive

Windows Server Insider Preview è stata introdotta un'opzione per delimitare manualmente l'allocazione dei volumi in spazi di archiviazione diretta. Tale operazione in modo che può aumentare notevolmente la tolleranza di errore in determinate condizioni, ma vengono applicate alcune considerazioni sulla gestione di aggiunta e la complessità. Questo argomento viene illustrato come funziona e fornisce esempi di PowerShell.

   > [!IMPORTANT]
   > Questa funzionalità è stata introdotta in Windows Server Insider Preview build 17093 e versioni successive. Non è disponibile in Windows Server 2016. Ti invitiamo i professionisti IT per aggiungere il [programma di Windows Server Insider](https://aka.ms/serverinsider) commenti e suggerimenti su cosa possiamo fare per rendere Windows Server funzionano meglio per l'organizzazione.

## <a name="prerequisites"></a>Prerequisiti

### <a name="green-checkmark-iconmediadelimit-volume-allocationsupportedpng-consider-using-this-option-if"></a>![Icona segno di spunta verde.](media/delimit-volume-allocation/supported.png) È consigliabile usare questa opzione se:

- Il cluster dispone di sei o più server. e
- Il cluster Usa solo [vie](storage-spaces-fault-tolerance.md#mirroring) resilienza

### <a name="red-x-iconmediadelimit-volume-allocationunsupportedpng-do-not-use-this-option-if"></a>![Icona X rossa.](media/delimit-volume-allocation/unsupported.png) Non utilizzare questa opzione se:

- Il cluster con meno di sei server; o
- Il cluster usi [parità](storage-spaces-fault-tolerance.md#parity) oppure [parità con accelerazione mirror](storage-spaces-fault-tolerance.md#mirror-accelerated-parity) resilienza

## <a name="understand"></a>Esaminare con attenzione

### <a name="review-regular-allocation"></a>Revisione: allocazione regolare

Con il mirroring, in modo normale, il volume è suddivisa in molti piccoli "allocazioni memoria" che viene copiati per tre volte e distribuiti uniformemente tra tutte le unità in ogni server del cluster. Per altre informazioni, leggere [questo blog di approfondimento](https://blogs.technet.microsoft.com/filecab/2016/11/21/deep-dive-pool-in-spaces-direct/).

![Diagramma che mostra il volume viene suddivisa in tre gli stack di allocazioni memoria e distribuito uniformemente tra tutti i server.](media/delimit-volume-allocation/regular-allocation.png)

Questa allocazione predefinita Ottimizza parallelo legge e scrive, determinando prestazioni migliori, è molto interessante nella relativa semplicità: ogni server è occupato in modo uniforme, ogni unità è altrettanto completa e tutti i volumi di rimangono online o alla modalità offline contemporaneamente. Ogni volume è garantita per sopravvivere a errori simultanei fino a due, come [questi esempi](storage-spaces-fault-tolerance.md#examples) illustrare.

Tuttavia, con questa allocazione, i volumi non possono sopravvivere tre errori simultanei. Se tre server non riesce in una sola volta, o errori delle unità in tre server in una sola volta, i volumi diventano inaccessibili perché almeno alcune allocazioni di memoria (con una probabilità molto alta) allocati esattamente tre unità o nei server che non è riuscita.

Nell'esempio seguente, i server 1, 3 e 5 eseguire nello stesso momento. Anche se molte allocazioni di memoria disponibile che sopravvivono copie, alcune non si:

![Diagramma che mostra tre di sei server evidenziato in rosso e il volume complessivo è rosso.](media/delimit-volume-allocation/regular-does-not-survive.png)

Il volume viene portata offline e non è accessibile fino a quando non vengono ripristinati i server.

### <a name="new-delimited-allocation"></a>Novità: delimitati allocazione

Con allocazione delimitato da virgole, si specifica un subset di server da usare (minimo tre per vie). Il volume è suddivisa in allocazioni di memoria che vengono copiati tre volte, come in precedenza, ma invece di allocare in ogni server **le allocazioni di memoria vengono allocate solo al sottoinsieme del server è specificare**.

![Diagramma che mostra il volume viene suddivisa in tre gli stack di allocazioni memoria e distribuito solo a tre dei sei server.](media/delimit-volume-allocation/delimited-allocation.png)

#### <a name="advantages"></a>Vantaggi

Con questa allocazione, il volume è probabilmente necessaria superare tre errori simultanei: infatti, alla probabilità di sopravvivenza aumenta da % 0 (con allocazione regolari) al 95% (con allocazione delimitato da virgole) in questo caso. Intuitivamente, questo avviene perché non fa affidamento sui server 4, 5 o 6, in modo non viene interessato dalle loro errori.

Nell'esempio precedente, i server 1, 3 e 5 eseguire nello stesso momento. Poiché delimitato allocazione garantita che server 2 contiene una copia di ogni slab, ogni slab dispone di una copia superstita e il volume rimane online e accessibili:

![Diagramma che mostra tre di sei server evidenziato in rosso, ma il volume complessivo è verde.](media/delimit-volume-allocation/delimited-does-survive.png)

Probabilità di sopravvivenza dipende dal numero di server e altri fattori: consente di visualizzare [analisi](#analysis) per informazioni dettagliate.

#### <a name="disadvantages"></a>Svantaggi

Allocazione delimitato impone alcune considerazioni sulla gestione di aggiunta e la complessità:

1. L'amministratore è responsabile che delimitano l'allocazione di ogni volume per bilanciare l'uso dell'archiviazione su server e incontrarsi elevata probabilità di sopravvivenza, come descritto nel [procedure consigliate](#best-practices) sezione.

2. Con allocazione delimitato da virgole, l'equivalente di riservare **unità di uno capacità per ogni server (con nessun valore massimo)**. Questo è superiore al [pubblicato indicazioni](plan-volumes.md#choosing-the-size-of-volumes) per l'allocazione normali, quali superarne in base alla capacità di quattro unità totali.

3. Se un server ha esito negativo e deve essere sostituito, come descritto in [rimuovere un server con le relative unità](remove-servers.md#remove-a-server-and-its-drives), l'amministratore è responsabile dell'aggiornamento di delimitazione dei volumi interessati aggiungendo il nuovo server e rimuovendo l'one – esempio non riuscito di seguito.

## <a name="usage-in-powershell"></a>Utilizzo in PowerShell

È possibile usare il `New-Volume` cmdlet per creare i volumi in spazi di archiviazione diretta.

Ad esempio, per creare un volume con mirroring, in modo regolare:

```PowerShell
New-Volume -FriendlyName "MyRegularVolume" -Size 100GB
```

### <a name="create-a-volume-and-delimit-its-allocation"></a>Creare un volume e consente di delimitare le allocazione

Per creare un volume con mirroring a tre vie e consente di delimitare le allocazione:

1. Innanzitutto assegnare il server del cluster per la variabile `$Servers`:

    ```PowerShell
    $Servers = Get-StorageFaultDomain -Type StorageScaleUnit | Sort FriendlyName
    ```

   > [!TIP]
   > In spazi di archiviazione diretta, il termine 'Unità di scala di archiviazione' si riferisce all'archiviazione non elaborato collegato a un server, incluse le unità collegato direttamente e direct-attached enclosure esterne con le unità. In questo contesto, è uguale a quello 'server'.

2. Specificare i server da utilizzare con il nuovo `-StorageFaultDomainsToUse` parametro e da indicizzando `$Servers`. Ad esempio, per delimitare l'allocazione per il primo, secondo e terzo server (indici 0, 1 e 2):

    ```PowerShell
    New-Volume -FriendlyName "MyVolume" -Size 100GB -StorageFaultDomainsToUse $Servers[0,1,2]
    ```

### <a name="see-a-delimited-allocation"></a>Vedere un'allocazione delimitato da virgole

Per visualizzare come *volume* viene allocato, utilizzare il `Get-VirtualDiskFootprintBySSU.ps1` lo script in [appendice](#appendix):

```PowerShell
PS C:\> .\Get-VirtualDiskFootprintBySSU.ps1

VirtualDiskFriendlyName TotalFootprint Server1 Server2 Server3 Server4 Server5 Server6
----------------------- -------------- ------- ------- ------- ------- ------- -------
MyVolume                300 GB         100 GB  100 GB  100 GB  0       0       0      
```

Si noti che solo Server1, Server2 e Server3 contiene allocazioni di memoria di *volume*.

### <a name="change-a-delimited-allocation"></a>Modificare un'allocazione delimitato da virgole

Usare le nuove `Add-StorageFaultDomain` e `Remove-StorageFaultDomain` cmdlet per modificare delimitato come l'allocazione.

Ad esempio, per spostare *volume* carico da un unico server:

1. Specificare che il server quarto **può** allocazioni di memoria di archiviare *volume*:

    ```PowerShell
    Get-VirtualDisk MyVolume | Add-StorageFaultDomain -StorageFaultDomains $Servers[3]
    ```

2. Specificare che il primo server **Impossibile** allocazioni di memoria di archiviare *volume*:

    ```PowerShell
    Get-VirtualDisk MyVolume | Remove-StorageFaultDomain -StorageFaultDomains $Servers[0]
    ```

3. Ribilanciare il pool di archiviazione rendere effettiva la modifica:

    ```PowerShell
    Get-StoragePool S2D* | Optimize-StoragePool
    ```

![Diagramma che mostra che le allocazioni di memoria migrare en-masse dai server 1, 2 e 3 per 2, 3 e 4 i server.](media/delimit-volume-allocation/move.gif)

È possibile monitorare lo stato di avanzamento del ribilanciamento con `Get-StorageJob`.

Una volta completata, verificare che *volume* è stato spostato eseguendo `Get-VirtualDiskFootprintBySSU.ps1` nuovamente.

```PowerShell
PS C:\> .\Get-VirtualDiskFootprintBySSU.ps1

VirtualDiskFriendlyName TotalFootprint Server1 Server2 Server3 Server4 Server5 Server6
----------------------- -------------- ------- ------- ------- ------- ------- -------
MyVolume                300 GB         0       100 GB  100 GB  100 GB  0       0      
```

Si noti che non contiene Server1 allocazioni memoria dei *volume* più – invece Server04 viene.

## <a name="best-practices"></a>Procedure consigliate

Ecco le procedure consigliate da seguire quando uso delimitati allocazione di volume:

### <a name="choose-three-servers"></a>Scegliere i tre server

Delimitare ogni volume vie in tre server, non più.

### <a name="balance-storage"></a>Bilanciare al meglio archiviazione

Bilanciare quanto spazio di archiviazione viene allocata a ogni server, per le dimensioni del volume.

### <a name="every-delimited-allocation-unique"></a>Ogni allocazione delimitato univoco

Per ottimizzare la tolleranza di errore, apportare allocazione di ogni volume univoco, vale a dire non condivide *tutti* relativi server con un altro volume (alcuni aspetti comuni sono accettabile). Con i server di N, esistono combinazioni univoche "N scegliere 3", ecco cosa significa per alcune dimensioni comuni per il cluster:

| Numero di server (N) | Numero di univoci delimitati allocazioni (selezione di N 3) |
|-----------------------|-----------------------------------------------------|
| 6                     | 20                                                  |
| 8                     | 56                                                  |
| 12                    | 220                                                 |
| 16                    | 560                                                 |

   > [!TIP]
   > Prendere in considerazione questo utile esaminare [combinatorics scegliere notazione](https://betterexplained.com/articles/easy-permutations-and-combinations/).

Di seguito è riportato un esempio che ottimizza la tolleranza di errore, ogni volume contiene un'allocazione delimitata univoca:

![allocazione univoca](media/delimit-volume-allocation/unique-allocation.png)

Al contrario, nell'esempio seguente, i primi tre volumi usare la stessa allocazione delimitato da virgole (per server 1, 2 e 3) e le ultime tre volumi usano la stessa allocazione delimitato da virgole (per server di 4, 5 e 6). In questo modo non ottimizzare la tolleranza di errore: se tre server hanno esito negativo, più volumi è stato possibile venga portato offline e inaccessibili in una sola volta.

![non univoco-di allocazione](media/delimit-volume-allocation/non-unique-allocation.png)

## <a name="analysis"></a>Analisi

In questa sezione deriva la matematica probabilità che un volume rimane online e accessibile (o allo stesso modo, la frazione previsto di spazio di archiviazione complessivo che rimane online e accessibili) come funzione del numero di errori e le dimensioni del cluster.

   > [!NOTE]
   > Questa sezione è facoltativa la lettura. Se si voglia visualizzare le operazioni matematiche, continuare a leggere. Ma in caso contrario, non bisogna preoccuparsi: [Utilizzo di PowerShell](#usage-in-powershell) e [procedure consigliate](#best-practices) è sufficiente per implementare correttamente l'allocazione delimitato da virgole.

### <a name="up-to-two-failures-is-always-okay"></a>Fino a due tentativi non riusciti è sempre opportuno

Ogni volume con mirroring a tre vie può sopravvivere a errori fino a due allo stesso tempo, come [questi esempi](storage-spaces-fault-tolerance.md#examples) illustrare, indipendentemente dalla propria allocazione. Se due unità hanno esito negativo, esito negativo di due server o uno dei ogni, ogni volume vie rimane online e accessibile, anche con allocazione regolari.

### <a name="more-than-half-the-cluster-failing-is-never-okay"></a>Più della metà l'errore del cluster non è corretto

Al contrario, nel caso estremo più della metà dei server o unità nel cluster non riuscite, in una sola volta [quorum viene perso](understand-quorum.md) e passa alla modalità offline e diventa inaccessibile, indipendentemente dalla propria allocazione di ogni volume con mirroring a tre vie.

### <a name="what-about-in-between"></a>Per quanto riguarda tra?

Se tre o più errori si verificano in una sola volta, ma ad almeno metà delle unità e server sono ancora attivi, i volumi con allocazione delimitato possono rimanere online e accessibili, a seconda del server che presentano errori. Eseguiamo i numeri per determinare quante probabilità ci precisare.

Semplicità, presuppone che i volumi sono in modo indipendente e distribuite in modo identico (IID) secondo le procedure consigliate e tale sufficientemente univoco combinazioni sono disponibili per l'allocazione di ogni volume sia univoco. È la probabilità di ogni volume continua a funzionare anche la frazione previsto di spazio di archiviazione complessivo viene conservata da linearità della previsione. 

Dato **N** server di cui **F** presentano errori, di un volume allocato a **3** di esse passa offline se-e-only-se tutti i **3** sono tra i  **F** con errori. Esistono **(N scegliere F)** modi **F** errori si verificano, dei quali **(F scegliere 3)** comportare il volume sarà offline e diventare inaccessibile. La probabilità che può essere espresso come:

![P_offline = Fc3 / NcF](media/delimit-volume-allocation/probability-volume-offline.png)

In tutti gli altri casi, il volume rimane online e accessibili:

![P_online = 1 – (Fc3 / NcF)](media/delimit-volume-allocation/probability-volume-online.png)

Nelle tabelle seguenti valutano la probabilità per alcune dimensioni comuni per il cluster e un massimo di 5 errori, che rivela che allocazione delimitato aumenta la tolleranza di errore rispetto alle normale allocazione in ogni caso considerato.

### <a name="with-6-servers"></a>Con i 6 server

| Allocazione                           | Probabilità di sopravvivenza agli 1 errori | Probabilità di sopravvivenza agli errori di 2 | Probabilità di sopravvivenza agli errori di 3 | Probabilità di sopravvivenza agli errori di 4 | Probabilità di sopravvivenza agli errori di 5 |
|--------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| Normale, distribuiti in tutti i 6 server | 100%                               | 100%                                | 0%                                  | 0%                                  | 0%                                  |
| Delimitato da 3 solo per i server          | 100%                               | 100%                                | 95.0%                               | 0%                                  | 0%                                  |

   > [!NOTE]
   > Dopo avere più di 3 errori su 6 server totale, il cluster perde il quorum.

### <a name="with-8-servers"></a>Con i 8 server

| Allocazione                           | Probabilità di sopravvivenza agli 1 errori | Probabilità di sopravvivenza agli errori di 2 | Probabilità di sopravvivenza agli errori di 3 | Probabilità di sopravvivenza agli errori di 4 | Probabilità di sopravvivenza agli errori di 5 |
|--------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| Normale, distribuiti in tutti i 8 server | 100%                               | 100%                                | 0%                                  | 0%                                  | 0%                                  |
| Delimitato da 3 solo per i server          | 100%                               | 100%                                | 98.2%                               | 94.3%                               | 0%                                  |

   > [!NOTE]
   > Dopo avere più di 4 errori su 8 server totale, il cluster perde il quorum.

### <a name="with-12-servers"></a>Con 12 server

| Allocazione                            | Probabilità di sopravvivenza agli 1 errori | Probabilità di sopravvivenza agli errori di 2 | Probabilità di sopravvivenza agli errori di 3 | Probabilità di sopravvivenza agli errori di 4 | Probabilità di sopravvivenza agli errori di 5 |
|---------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| Normale, distribuiti in tutti i 12 server | 100%                               | 100%                                | 0%                                  | 0%                                  | 0%                                  |
| Delimitato da 3 solo per i server           | 100%                               | 100%                                | 99.5%                               | 99.2%                               | 98.7%                               |

### <a name="with-16-servers"></a>Con 16 server

| Allocazione                            | Probabilità di sopravvivenza agli 1 errori | Probabilità di sopravvivenza agli errori di 2 | Probabilità di sopravvivenza agli errori di 3 | Probabilità di sopravvivenza agli errori di 4 | Probabilità di sopravvivenza agli errori di 5 |
|---------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| Normale, distribuiti in tutti i 16 server | 100%                               | 100%                                | 0%                                  | 0%                                  | 0%                                  |
| Delimitato da 3 solo per i server           | 100%                               | 100%                                | 99.8%                               | 99.8%                               | 99.8%                               |

## <a name="frequently-asked-questions"></a>Domande frequenti

### <a name="can-i-delimit-some-volumes-but-not-others"></a>È possibile delimitare alcuni volumi, ma non altri?

Sì. È possibile scegliere per il volume per delimitare l'allocazione o meno.

### <a name="does-delimited-allocation-change-how-drive-replacement-works"></a>Allocazione delimitato modifica come sostituzione di un'unità funziona?

No, è analogo a quello di allocazione regolari.

## <a name="see-also"></a>Vedere anche

- [Panoramica di spazi diretti di archiviazione](storage-spaces-direct-overview.md)
- [Tolleranza di errore in spazi di archiviazione diretta](storage-spaces-fault-tolerance.md)

## <a name="appendix"></a>Appendice

Questo script consente di vedere come vengono allocati i volumi.

Per usarlo come descritto in precedenza, copiare e incollare e salvare come `Get-VirtualDiskFootprintBySSU.ps1`.

```PowerShell
Function ConvertTo-PrettyCapacity {
    Param (
        [Parameter(
            Mandatory = $True,
            ValueFromPipeline = $True
            )
        ]
    [Int64]$Bytes,
    [Int64]$RoundTo = 0
    )
    If ($Bytes -Gt 0) {
        $Base = 1024
        $Labels = ("bytes", "KB", "MB", "GB", "TB", "PB", "EB", "ZB", "YB")
        $Order = [Math]::Floor( [Math]::Log($Bytes, $Base) )
        $Rounded = [Math]::Round($Bytes/( [Math]::Pow($Base, $Order) ), $RoundTo)
        [String]($Rounded) + " " + $Labels[$Order]
    }
    Else {
        "0"
    }
    Return
}

Function Get-VirtualDiskFootprintByStorageFaultDomain {

    ################################################
    ### Step 1: Gather Configuration Information ###
    ################################################

    Write-Progress -Activity "Get-VirtualDiskFootprintByStorageFaultDomain" -CurrentOperation "Gathering configuration information..." -Status "Step 1/4" -PercentComplete 00

    $ErrorCannotGetCluster = "Cannot proceed because 'Get-Cluster' failed."
    $ErrorNotS2DEnabled = "Cannot proceed because the cluster is not running Storage Spaces Direct."
    $ErrorCannotGetClusterNode = "Cannot proceed because 'Get-ClusterNode' failed."
    $ErrorClusterNodeDown = "Cannot proceed because one or more cluster nodes is not Up."
    $ErrorCannotGetStoragePool = "Cannot proceed because 'Get-StoragePool' failed."
    $ErrorPhysicalDiskFaultDomainAwareness = "Cannot proceed because the storage pool is set to 'PhysicalDisk' fault domain awareness. This cmdlet only supports 'StorageScaleUnit', 'StorageChassis', or 'StorageRack' fault domain awareness."

    Try  {
        $GetCluster = Get-Cluster -ErrorAction Stop
    }
    Catch {
        throw $ErrorCannotGetCluster
    }

    If ($GetCluster.S2DEnabled -Ne 1) {
        throw $ErrorNotS2DEnabled
    }

    Try  {
        $GetClusterNode = Get-ClusterNode -ErrorAction Stop
    }
    Catch {
        throw $ErrorCannotGetClusterNode
    }

    If ($GetClusterNode | Where State -Ne Up) {
        throw $ErrorClusterNodeDown
    }

    Try {
        $GetStoragePool = Get-StoragePool -IsPrimordial $False -ErrorAction Stop
    }
    Catch {
        throw $ErrorCannotGetStoragePool
    }

    If ($GetStoragePool.FaultDomainAwarenessDefault -Eq "PhysicalDisk") {
        throw $ErrorPhysicalDiskFaultDomainAwareness
    }

    ###########################################################
    ### Step 2: Create SfdList[] and PhysicalDiskToSfdMap{} ###
    ###########################################################

    Write-Progress -Activity "Get-VirtualDiskFootprintByStorageFaultDomain" -CurrentOperation "Analyzing physical disk information..." -Status "Step 2/4" -PercentComplete 25

    $SfdList = Get-StorageFaultDomain -Type ($GetStoragePool.FaultDomainAwarenessDefault) | Sort FriendlyName # StorageScaleUnit, StorageChassis, or StorageRack

    $PhysicalDiskToSfdMap = @{} # Map of PhysicalDisk.UniqueId -> StorageFaultDomain.FriendlyName
    $SfdList | ForEach {
        $StorageFaultDomain = $_
        $_ | Get-StorageFaultDomain -Type PhysicalDisk | ForEach {
            $PhysicalDiskToSfdMap[$_.UniqueId] = $StorageFaultDomain.FriendlyName
        }
    }

    ##################################################################################################
    ### Step 3: Create VirtualDisk.FriendlyName -> { StorageFaultDomain.FriendlyName -> Size } Map ###
    ##################################################################################################

    Write-Progress -Activity "Get-VirtualDiskFootprintByStorageFaultDomain" -CurrentOperation "Analyzing virtual disk information..." -Status "Step 3/4" -PercentComplete 50

    $GetVirtualDisk = Get-VirtualDisk | Sort FriendlyName

    $VirtualDiskMap = @{}

    $GetVirtualDisk | ForEach {
        # Map of PhysicalDisk.UniqueId -> Size for THIS virtual disk
        $PhysicalDiskToSizeMap = @{}
        $_ | Get-PhysicalExtent | ForEach {
            $PhysicalDiskToSizeMap[$_.PhysicalDiskUniqueId] += $_.Size
        }
        # Map of StorageFaultDomain.FriendlyName -> Size for THIS virtual disk
        $SfdToSizeMap = @{}
        $PhysicalDiskToSizeMap.keys | ForEach {
            $SfdToSizeMap[$PhysicalDiskToSfdMap[$_]] += $PhysicalDiskToSizeMap[$_]
        }
        # Store
        $VirtualDiskMap[$_.FriendlyName] = $SfdToSizeMap
    }

    #########################
    ### Step 4: Write-Out ###
    #########################

    Write-Progress -Activity "Get-VirtualDiskFootprintByStorageFaultDomain" -CurrentOperation "Formatting output..." -Status "Step 4/4" -PercentComplete 75

    $Output = $GetVirtualDisk | ForEach {
        $Row = [PsCustomObject]@{}

        $VirtualDiskFriendlyName = $_.FriendlyName
        $Row | Add-Member -MemberType NoteProperty "VirtualDiskFriendlyName" $VirtualDiskFriendlyName

        $TotalFootprint = $_.FootprintOnPool | ConvertTo-PrettyCapacity
        $Row | Add-Member -MemberType NoteProperty "TotalFootprint" $TotalFootprint

        $SfdList | ForEach {
            $Size = $VirtualDiskMap[$VirtualDiskFriendlyName][$_.FriendlyName] | ConvertTo-PrettyCapacity
            $Row | Add-Member -MemberType NoteProperty $_.FriendlyName $Size
        }

        $Row
    }

    # Calculate width, in characters, required to Format-Table
    $RequiredWindowWidth = ("TotalFootprint").length + 1 + ("VirtualDiskFriendlyName").length + 1
    $SfdList | ForEach {
        $RequiredWindowWidth += $_.FriendlyName.Length + 1
    }

    $ActualWindowWidth = (Get-Host).UI.RawUI.WindowSize.Width

    If (!($ActualWindowWidth)) {
        # Cannot get window width, probably ISE, Format-List
        Write-Warning "Could not determine window width. For the best experience, use a Powershell window instead of ISE"
        $Output | Format-Table
    }
    ElseIf ($ActualWindowWidth -Lt $RequiredWindowWidth) {
        # Narrower window, Format-List
        Write-Warning "For the best experience, try making your PowerShell window at least $RequiredWindowWidth characters wide. Current width is $ActualWindowWidth characters."
        $Output | Format-List
    }
    Else {
        # Wider window, Format-Table
        $Output | Format-Table
    }
}

Get-VirtualDiskFootprintByStorageFaultDomain
```
