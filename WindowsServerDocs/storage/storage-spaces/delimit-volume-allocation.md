---
title: Delimitare l'allocazione di volumi in Spazi di archiviazione diretta
ms.author: cosmosdarwin
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 03/29/2018
ms.openlocfilehash: 19e5a38ca406878b7dbc5a187b0057e97e4fe2d1
ms.sourcegitcommit: 74107a32efe1e53b36c938166600739a79dd0f51
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2020
ms.locfileid: "76918300"
---
# <a name="delimit-the-allocation-of-volumes-in-storage-spaces-direct"></a>Delimitare l'allocazione di volumi in Spazi di archiviazione diretta
> Si applica a: Windows Server 2019

Windows Server 2019 introduce un'opzione che consente di delimitare manualmente l'allocazione di volumi in Spazi di archiviazione diretta. Questa operazione può aumentare significativamente la tolleranza di errore in determinate condizioni, ma impone alcune considerazioni e complessità di gestione aggiuntive. Questo argomento spiega come funziona e fornisce esempi in PowerShell.

   > [!IMPORTANT]
   > Questa funzionalità è una novità di Windows Server 2019. Non è disponibile in Windows Server 2016. 

## <a name="prerequisites"></a>Prerequisiti

### <a name="green-checkmark-iconmediadelimit-volume-allocationsupportedpng-consider-using-this-option-if"></a>![Icona segno di spunta verde.](media/delimit-volume-allocation/supported.png) Provare a usare questa opzione se:

- Il cluster ha sei o più server; e
- Il cluster usa solo la resilienza con [mirroring a tre vie](storage-spaces-fault-tolerance.md#mirroring)

### <a name="red-x-iconmediadelimit-volume-allocationunsupportedpng-do-not-use-this-option-if"></a>![Icona X rossa.](media/delimit-volume-allocation/unsupported.png) Non usare questa opzione se:

- Il cluster ha meno di sei server; o
- Il [cluster usa la](storage-spaces-fault-tolerance.md#parity) resilienza di parità o con [accelerazione speculare](storage-spaces-fault-tolerance.md#mirror-accelerated-parity)

## <a name="understand"></a>Nozioni di base

### <a name="review-regular-allocation"></a>Revisione: allocazione regolare

Con il normale mirroring a tre vie, il volume è suddiviso in molte piccole "solette" copiate tre volte e distribuite in modo uniforme in ogni unità in ogni server del cluster. Per altri dettagli, leggi [questo Blog di approfondimento](https://blogs.technet.microsoft.com/filecab/2016/11/21/deep-dive-pool-in-spaces-direct/).

![Diagramma che mostra il volume suddiviso in tre stack di lastre e distribuito in modo uniforme in ogni server.](media/delimit-volume-allocation/regular-allocation.png)

Questa allocazione predefinita ottimizza le letture e le Scritture parallele, ottenendo prestazioni migliori ed è accattivante nella sua semplicità: ogni server è ugualmente occupato, ogni unità è ugualmente piena e tutti i volumi vengono mantenuti online oppure passano in modalità offline. Ogni volume garantisce che sopravviva fino a due errori simultanei, come illustrato in [questi esempi](storage-spaces-fault-tolerance.md#examples) .

Con questa allocazione, tuttavia, i volumi non possono sopravvivere a tre errori simultanei. Se si verifica un errore in una sola volta in tre server o se le unità in tre server hanno un errore in una sola volta, i volumi diventano inaccessibili perché almeno alcune piastre erano (con probabilità molto elevata) allocate alle tre unità o ai server che hanno avuto esito negativo.

Nell'esempio seguente, i server 1, 3 e 5 hanno esito negativo nello stesso momento. Anche se molte lastre hanno superstiti copie, alcune non:

![Diagramma che mostra tre di sei server evidenziati in rosso e il volume complessivo è rosso.](media/delimit-volume-allocation/regular-does-not-survive.png)

Il volume passa alla modalità offline e diventa inaccessibile fino al ripristino dei server.

### <a name="new-delimited-allocation"></a>Nuovo: allocazione delimitata

Con l'allocazione delimitata è possibile specificare un subset di server da usare (minimo quattro). Il volume è diviso in lastre che vengono copiate tre volte, ad esempio prima, ma anziché allocare in ogni server, **le solette vengono allocate solo al subset di server specificato**.

Se, ad esempio, si dispone di un cluster a 8 nodi (nodi da 1 a 8), è possibile specificare un volume da posizionare solo sui dischi nei nodi 1, 2, 3, 4.
#### <a name="advantages"></a>Vantaggi

Con l'allocazione di esempio, è probabile che il volume sopravviva a tre errori simultanei. Se i nodi 1, 2 e 6 si arrestano, solo 2 dei nodi che contengono le 3 copie dei dati per il volume sono inattivi e il volume rimane in linea.

La probabilità di sopravvivenza dipende dal numero di server e da altri fattori. per ulteriori informazioni, vedere l' [analisi](#analysis) .

#### <a name="disadvantages"></a>Svantaggi

L'allocazione delimitata impone alcune considerazioni e complessità di gestione aggiuntive:

1. L'amministratore è responsabile della delimitazione dell'allocazione di ogni volume per bilanciare l'utilizzo dello spazio di archiviazione tra i server e sostenere un'elevata probabilità di sopravvivenza, come descritto nella sezione [procedure consigliate](#best-practices) .

2. Con allocazione delimitata, riservare l'equivalente di **un'unità di capacità per ogni server (senza valore massimo)** . Questo è più della [raccomandazione pubblicata](plan-volumes.md#choosing-the-size-of-volumes) per l'allocazione regolare, che Zadok a quattro unità di capacità totali.

3. Se un server ha esito negativo e deve essere sostituito, come descritto in [rimuovere un server e le relative unità](remove-servers.md#remove-a-server-and-its-drives), l'amministratore è responsabile dell'aggiornamento del delimitatore dei volumi interessati mediante l'aggiunta del nuovo server e la rimozione dell'esempio non riuscito: di seguito.

## <a name="usage-in-powershell"></a>Utilizzo in PowerShell

È possibile utilizzare il cmdlet `New-Volume` per creare volumi in Spazi di archiviazione diretta.

Ad esempio, per creare un normale volume mirror a tre vie:

```PowerShell
New-Volume -FriendlyName "MyRegularVolume" -Size 100GB
```

### <a name="create-a-volume-and-delimit-its-allocation"></a>Creare un volume e delimitare l'allocazione

Per creare un volume mirror a tre vie e delimitare l'allocazione:

1. Assegnare prima i server del cluster alla variabile `$Servers`:

    ```PowerShell
    $Servers = Get-StorageFaultDomain -Type StorageScaleUnit | Sort FriendlyName
    ```

   > [!TIP]
   > In Spazi di archiviazione diretta, il termine "unità di scala di archiviazione" si riferisce a tutti i dati di archiviazione non elaborati collegati a un server, incluse le unità collegate direttamente e le enclosure esterne collegate con le unità. In questo contesto, equivale a "Server".

2. Specificare i server da utilizzare con il nuovo parametro di `-StorageFaultDomainsToUse` e indicizzando in `$Servers`. Ad esempio, per delimitare l'allocazione al primo, secondo, terzo e quarto server (indici 0, 1, 2 e 3):

    ```PowerShell
    New-Volume -FriendlyName "MyVolume" -Size 100GB -StorageFaultDomainsToUse $Servers[0,1,2,3]
    ```

### <a name="see-a-delimited-allocation"></a>Vedere un'allocazione delimitata

Per visualizzare la modalità di allocazione di *volume* , usare lo script `Get-VirtualDiskFootprintBySSU.ps1` in [appendice](#appendix):

```PowerShell
PS C:\> .\Get-VirtualDiskFootprintBySSU.ps1

VirtualDiskFriendlyName TotalFootprint Server1 Server2 Server3 Server4 Server5 Server6
----------------------- -------------- ------- ------- ------- ------- ------- -------
MyVolume                300 GB         100 GB  100 GB  100 GB  100 GB  0       0      
```

Si noti che solo Server1, Server2, Server3 e Server4 contengono solette di *volume*.

### <a name="change-a-delimited-allocation"></a>Modificare un'allocazione delimitata

Utilizzare i nuovi cmdlet `Add-StorageFaultDomain` e `Remove-StorageFaultDomain` per modificare la modalità di delimitazione dell'allocazione.

Ad esempio, per spostare il *volume* su un solo server:

1. Specificare che il quinto server è in **grado** di archiviare le solette di *volume*:

    ```PowerShell
    Get-VirtualDisk MyVolume | Add-StorageFaultDomain -StorageFaultDomains $Servers[4]
    ```

2. Consente di specificare che il primo server **non è in grado** di archiviare le solette del *volume*:

    ```PowerShell
    Get-VirtualDisk MyVolume | Remove-StorageFaultDomain -StorageFaultDomains $Servers[0]
    ```

3. Ribilanciare il pool di archiviazione per rendere effettive le modifiche:

    ```PowerShell
    Get-StoragePool S2D* | Optimize-StoragePool
    ```

È possibile monitorare lo stato di avanzamento del ribilanciamento con `Get-StorageJob`.

Al termine, verificare che il *volume* sia stato spostato eseguendo di nuovo `Get-VirtualDiskFootprintBySSU.ps1`.

```PowerShell
PS C:\> .\Get-VirtualDiskFootprintBySSU.ps1

VirtualDiskFriendlyName TotalFootprint Server1 Server2 Server3 Server4 Server5 Server6
----------------------- -------------- ------- ------- ------- ------- ------- -------
MyVolume                300 GB         0       100 GB  100 GB  100 GB  100 GB  0      
```

Si noti che Server1 non contiene più le solette del *volume* , bensì server5.

## <a name="best-practices"></a>Procedure consigliate

Ecco le procedure consigliate da seguire quando si usa l'allocazione di volumi delimitati:

### <a name="choose-four-servers"></a>Scegliere quattro server

Delimitare ogni volume mirror a tre vie a quattro server, non più.

### <a name="balance-storage"></a>Bilancia archiviazione

Bilanciare la quantità di spazio di archiviazione allocata a ogni server, considerando le dimensioni del volume.

### <a name="stagger-delimited-allocation-volumes"></a>Scaglionare i volumi di allocazione delimitati

Per ottimizzare la tolleranza di errore, rendere univoca l'allocazione di ogni volume, vale a dire che non condivide *tutti* i server con un altro volume (alcune sovrapposizioni è accettabile). 

Ad esempio in un sistema a otto nodi: volume 1: Server 1, 2, 3, 4 volume 2: Server 5, 6, 7, 8 volume 3: Server 3, 4, 5, 6 volume 4: Server 1, 2, 7, 8

## <a name="analysis"></a>Analisi

In questa sezione viene derivata la probabilità matematica che un volume rimanga online e accessibile (o equivalente, la frazione prevista dell'archiviazione complessiva che rimane online e accessibile) come funzione del numero di errori e delle dimensioni del cluster.

   > [!NOTE]
   > Questa sezione è facoltativa per la lettura. Se si è interessati a vedere la matematica, leggere il In caso contrario, l' [utilizzo in PowerShell](#usage-in-powershell) e le [procedure consigliate](#best-practices) è sufficiente per implementare correttamente l'allocazione delimitata.

### <a name="up-to-two-failures-is-always-okay"></a>Un massimo di due errori è sempre accettabile

Ogni volume mirror a tre vie può sopravvivere fino a due errori allo stesso tempo, indipendentemente dall'allocazione. Se due unità hanno esito negativo o due server hanno esito negativo o uno di essi, ogni volume mirror a tre vie rimane online e accessibile, anche con l'allocazione regolare.

### <a name="more-than-half-the-cluster-failing-is-never-okay"></a>Più della metà del cluster il guasto non è mai accettabile

Viceversa, nel caso estremo che più della metà dei server o delle unità del cluster abbiano esito negativo contemporaneamente, il [quorum viene perso](understand-quorum.md) e ogni volume mirror a tre vie passa alla modalità offline e diventa inaccessibile, indipendentemente dall'allocazione.

### <a name="what-about-in-between"></a>Che cosa c'è tra?

Se si verificano tre o più errori in una sola volta, ma almeno la metà dei server e delle unità è ancora attiva, i volumi con allocazione delimitata possono rimanere online e accessibili, a seconda di quali server presentano errori.

## <a name="frequently-asked-questions"></a>Domande frequenti

### <a name="can-i-delimit-some-volumes-but-not-others"></a>È possibile delimitare alcuni volumi, ma non altri?

Sì. È possibile scegliere se delimitare l'allocazione per volume.

### <a name="does-delimited-allocation-change-how-drive-replacement-works"></a>L'allocazione delimitata cambia in che modo funziona la sostituzione delle unità?

No, è uguale a quello dell'allocazione normale.

## <a name="see-also"></a>Vedi anche

- [Panoramica di Spazi di archiviazione diretta](storage-spaces-direct-overview.md)
- [Tolleranza di errore in Spazi di archiviazione diretta](storage-spaces-fault-tolerance.md)

## <a name="appendix"></a>Appendice

Questo script consente di visualizzare il modo in cui vengono allocati i volumi.

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
