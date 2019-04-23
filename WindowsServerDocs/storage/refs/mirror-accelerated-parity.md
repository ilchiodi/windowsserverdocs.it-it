---
title: Parità accelerata con mirror
ms.prod: windows-server-threshold
ms.author: gawatu
ms.manager: masriniv
ms.technology: storage-file-systems
ms.topic: article
author: gawatu
ms.date: 10/17/2018
ms.assetid: ''
ms.openlocfilehash: ba7454f58255ba7a66624a5c59b062da9f871063
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865942"
---
# <a name="mirror-accelerated-parity"></a>Parità accelerata con mirror

>Si applica a: Windows Server 2019, Windows Server 2016

Il sistema Spazi di archiviazione può fornire la tolleranza di errore per i dati tramite due tecniche fondamentali: mirroring e parità. In [Spazi di archiviazione diretta](../storage-spaces/storage-spaces-direct-overview.md), ReFS introduce parità accelerata con mirroring, che consente di creare volumi che usano sia resilienze con mirroring che con parità. La parità accelerata con mirroring offre una soluzione di archiviazione conveniente ed efficiente in termini di spazio senza compromettere le prestazioni. 

![Volume-parità-accelerata-con-mirroring](media/mirror-accelerated-parity/Mirror-Accelerated-Parity-Volume.png)

## <a name="background"></a>Informazioni

Gli schemi di resilienza con mirroring e con parità hanno caratteristiche di archiviazione e di prestazioni fondamentalmente diverse:
- La resilienza con mirroring consente agli utenti di accelerare la scrittura, ma la replica dei dati per ogni copia non è efficiente in termini di spazio. 
- Se si usa la parità, invece, è necessario ricalcolarla per ogni operazione di scrittura, causando prestazioni di scrittura casuali. La parità tuttavia consente agli utenti di archiviare i dati con una maggiore efficienza di spazio. Per altre informazioni, vedi [tolleranza di errore di spazi di archiviazione](../storage-spaces/Storage-Spaces-Fault-Tolerance.md).

Pertanto, il mirroring è predisposto per offrire una soluzione di archiviazione sensibile alle prestazioni, mentre la parità offre un migliore uso della capacità di archiviazione. Nella parità accelerata con mirroring, ReFS sfrutta i vantaggi di ogni tipo di resilienza per offrire una soluzione di archiviazione efficiente in termini di capacità e sensibile alle prestazioni grazie alla combinazione di entrambi gli schemi di resilienza in un singolo volume.

## <a name="data-rotation-on-mirror-accelerated-parity"></a>Rotazione di dati sulla parità accelerata con mirror

ReFS ruota attivamente i dati tra mirroring e parità, in tempo reale. In questo modo le scritture in ingresso vengono eseguite rapidamente nel mirroring e successivamente ruotate nella parità per essere archiviate in modo efficiente. In questo modo, i dati di I/O in ingresso vengono gestiti rapidamente nel mirroring mentre i dati ad accesso sporadico vengono archiviati in modo efficiente nella parità, offrendo sia prestazioni ottimali che archiviazione a basso costo nello stesso volume. 

Per ruotare i dati tra mirroring e parità, ReFS divide logicamente il volume in aree di 64 MiB, ovvero l'unità di rotazione. L'immagine seguente illustra un volume di parità accelerata con mirroring suddiviso in aree. 

![Volume-parità-accelerata-con-mirroring-con-contenitori-archiviazione](media/mirror-accelerated-parity/Mirror-Accelerated-Parity-Volume-with-Storage-Containers.png)

ReFS inizia la rotazione di aree complete dal mirroring alla parità quando il livello di mirroring ha raggiunto un valore di capacità specificato. Invece di spostare immediatamente i dati dal mirroring alla parità, ReFS attende e conserva i dati nel mirroring il più a lungo possibile, continuando in tal modo a offrire prestazioni ottimali per i dati (vedi "Prestazioni di I/O di seguito). 

Quando vengono spostati dal mirroring alla parità, i dati vengono letti, vengono calcolate le codifiche di parità e quindi i dati vengono scritti nella parità. L'animazione seguente illustra questo comportamento tramite un'area con mirroring a tre vie che viene convertita in un'area codificata di cancellazione durante la rotazione:

![Rotazione-parità-accelerata-con-mirroring](media/mirror-accelerated-parity/Container-Rotation.gif)

## <a name="io-on-mirror-accelerated-parity"></a>I/O su parità accelerata con mirroring
### <a name="io-behavior"></a>Comportamento I/O
**Operazioni di scrittura:** Servizi di reFS in ingresso vengono scritte in tre modi diversi:

1.  **Scrive Mirror:**

    - **1a.** Se la scrittura in ingresso modifica i dati esistenti nel mirroring, ReFS modificherà i dati sul posto.
    - **1b.** Se la scrittura in ingresso è una nuova scrittura e ReFS trova spazio libero sufficiente nel mirroring per gestire tale scrittura, ReFS scriverà nel mirroring.
    ![Write-to-Mirror](media/mirror-accelerated-parity/Write-to-Mirror.png)

2. **Operazioni di scrittura al server Mirror, riallocata da parità:**

    Se la scrittura in ingresso modifica i dati che si trovano in parità e ReFS trova spazio libero sufficiente nel mirroring per gestire la scrittura in ingresso, ReFS invaliderà prima di tutto i dati precedenti in parità e quindi scriverà sul mirroring. Questo invalidamento rappresenta un'operazione sui metadati rapida e conveniente che consente di migliorare in modo significativo le prestazioni di scrittura nella parità.
    ![Reallocated-Write](media/mirror-accelerated-parity/Reallocated-Write.png)

3. **Scritture automatiche a parità:**
    
    Se ReFS non trova spazio libero sufficiente nel mirroring, scriverà nuovi dati nella parità o modificherà direttamente i dati esistenti in parità. Nella sezione "Ottimizzazioni delle prestazioni" seguente vengono fornite indicazioni per ridurre il numero di scritture nella parità.
    ![Write-to-Parity](media/mirror-accelerated-parity/Write-to-Parity.png)

**Operazioni di lettura:** ReFS leggerà direttamente dal livello che contiene i dati rilevanti. Se la parità viene costruita con HDD, la cache in Spazi di archiviazione diretta memorizza tali dati per accelerare le letture successive. 

> [!NOTE]
> Le operazioni di lettura non causano mai la rotazione dei dati da parte di ReFS nel livello di mirroring. 

### <a name="io-performance"></a>Prestazioni dei / o

**Operazioni di scrittura:** Ogni tipo di scrittura descritto in precedenza ha le proprie caratteristiche di prestazioni. In altre parole, le operazioni di scrittura nel livello di mirroring sono molto più veloci rispetto alle scritture riallocate e queste ultime sono notevolmente più rapide rispetto a scritture eseguite direttamente a livello di parità. Questa relazione è illustrata dalla disuguaglianza seguente: 


- **Eseguire il mirroring di livello > riallocati scritture >> livello di parità**


**Operazioni di lettura:** Durante la lettura da parità non c'è alcun impatto sulle prestazioni significativo e negativo:
- Se mirroring e parità vengono costruiti con lo stesso tipo di supporto, le prestazioni in termini di lettura saranno equivalenti. 
- Se mirroring e parità sono costruiti da diversi tipi di supporti, ovvero con unità di mirroring SSD o unità di parità HDD, [la cache in Spazi di archiviazione diretta](../storage-spaces/understand-the-cache.md) consentirà di memorizzare dati ad accesso frequente per accelerare le letture da parità.

## <a name="refs-compaction"></a>Compattazione ReFS

Nella versione semestrale dell'autunno ReFS ha introdotto la compattazione, che migliora notevolmente le prestazioni per i volumi di parità accelerata con mirroring completi per oltre il 90%. 

**Sfondo:** In precedenza, come volumi di parità con accelerazione mirror è diventato completi, potrebbe influire negativamente le prestazioni di questi volumi. Tale peggioramento è dovuto alla combinazione di dati ad accesso frequente e sporadico nel volume. Ciò significa che nel mirroring possono essere archiviati meno dati ad accesso frequente perché lo spazio viene occupato anche dai dati ad accesso sporadico. L'archiviazione dei dati ad accesso frequente nel mirroring è fondamentale per garantire prestazioni elevate perché le scritture dirette nel mirroring sono molto più veloci rispetto alle scritture riallocate e notevolmente più veloci delle scritture dirette nella parità. Di conseguenza la presenza di dati ad accesso sporadico nel mirroring non è un fattore positivo per le prestazioni, perché diminuisce la probabilità che ReFS possa scrivere direttamente nel mirroring. 

La compattazione ReFS risolve questi problemi di prestazioni liberando spazio nel mirroring per i dati ad accesso frequente. La compattazione consolida prima tutti i dati, da mirroring e parità, in parità. Ciò riduce la frammentazione all'interno del volume e aumenta la quantità di spazio indirizzabile nel mirroring. Soprattutto, questo processo permette a ReFS di consolidare nel mirroring i dati ad accesso frequente:
-   Quando sono presenti nuove operazioni di scrittura, verranno eseguite nel mirroring. Di conseguenza, i nuovi dati ad accesso frequente scritti si trovano nel mirroring. 
-   Quando una scrittura di modifica viene eseguita su dati in parità, ReFS esegue una scrittura riallocata, in modo che anche questa venga gestita nel mirroring. Di conseguenza, i dati ad accesso frequente spostati in parità durante la compattazione verranno riallocati nel mirroring. 

## <a name="performance-optimizations"></a>Ottimizzazioni delle prestazioni

>[!IMPORTANT]
> È consigliabile posizionare dischi rigidi virtuali con intensa attività di scrittura nelle varie sottodirectory. Questo avviene perché ReFS scrive le modifiche ai metadati a livello di una directory e i relativi file. Pertanto, se si distribuiscono file intensa attività di scrittura in elenchi in linea, operazioni sui metadati sono più piccoli ed eseguiti in parallelo, riducendo la latenza per le app.

### <a name="performance-counters"></a>Contatori di prestazioni

ReFS gestisce contatori delle prestazioni per valutare le prestazioni della parità accelerata con mirroring. 
-   Come descritto in precedenza nella sezione Scritture in parità, ReFS scriverà direttamente nella parità quando non trova spazio sufficiente nel mirroring. In genere, ciò si verifica quando il livello di mirroring si riempie più velocemente di quanto ReFS possa ruotare i dati in parità. In altre parole, la rotazione ReFS non è in grado di tenere il passo con la frequenza di inserimento. I contatori delle prestazioni seguenti identificano il momento in cui ReFS scrive direttamente in parità:
```
ReFS\Data allocations slow tier/sec
ReFS\Metadata allocations slow tier/sec
```
-   Se questi contatori sono diverso da zero, ReFS non ruota i dati in modo sufficientemente veloce all'esterno del mirroring. Per evitare questo problema, è possibile modificare l'aggressività della rotazione o aumentare le dimensioni del livello con mirroring.

### <a name="rotation-aggressiveness"></a>Aggressività di rotazione

ReFS inizia la rotazione dei dati quando il mirroring ha raggiunto una soglia di capacità specificata.
-   Valori più elevati della soglia determinano un maggior tempo di permanenza dei dati nel livello di mirroring. Lasciare i dati ad accesso frequente nel livello di mirroring è una scelta ottimale per le prestazioni, ma ReFS non sarà in grado di gestire grandi quantità di dati I/O in ingresso. 
-   Valori più bassi consentono a ReFS di rimuovere i dati e di inserire i dati di I/O in ingresso in modo efficiente. Ciò si applica ai carichi di lavoro con numerosi inserimenti, ad esempio memorizzazione di archivi. Valori più bassi, tuttavia, potrebbero compromettere le prestazioni per carichi di lavoro generale. Una rotazione dei dati non necessaria all'esterno del mirroring comporta una riduzione delle prestazioni. 

ReFS introduce un parametro regolabile per modificare questa soglia, che può essere configurata con una chiave del Registro di sistema. Questa chiave del Registro di sistema deve essere configurata in **ogni nodo in una distribuzione di Spazi di archiviazione diretta** ed è necessario riavviare il computer perché tutte le modifiche abbiano effetto. 
-   **Chiave:** HKEY_LOCAL_MACHINE\System\CurrentControlSet\Policies
-   **ValueName (DWORD):** DataDestageSsdFillRatioThreshold
-   **ValueType:** Percentuale

Se tale chiave del Registro di sistema non è impostata, ReFS utilizzerà un valore predefinito di 85%.  Questo valore predefinito è consigliato per la maggior parte delle distribuzioni, mentre non sono consigliati valori inferiori al 50%. Il comando PowerShell seguente illustra come impostare questa chiave del Registro di sistema con un valore pari al 75%: 
```PowerShell
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Policies -Name DataDestageSsdFillRatioThreshold -Value 75
 ```
 Per configurare questa chiave del Registro di sistema in ciascun nodo in una distribuzione di Spazi di archiviazione diretta, è possibile utilizzare il comando PowerShell seguente:
 ```PowerShell
 $Nodes = 'S2D-01', 'S2D-02', 'S2D-03', 'S2D-04'
 Invoke-Command $Nodes {Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Policies -Name DataDestageSsdFillRatioThreshold -Value 75}
 ```

### <a name="increasing-the-size-of-the-mirrored-tier"></a>Aumento delle dimensioni del livello di mirroring

L'aumento delle dimensioni del livello di mirroring permette a ReFS di mantenere una parte maggiore del working set nel mirroring. Ciò aumenta la probabilità che ReFS scriva direttamente nel mirroring, con un conseguente incremento delle prestazioni. I cmdlet PowerShell seguente illustra come aumentare le dimensioni del livello di mirroring:
```PowerShell
Resize-StorageTier -FriendlyName “Performance” -Size 20GB
Resize-StorageTier -InputObject (Get-StorageTier -FriendlyName “Performance”) -Size 20GB
```
>[!TIP]
>Accertarsi di ridimensionare **Partizione** e **Volume** dopo il ridimensionamento di **StorageTier**. Per ulteriori informazioni ed esempi, vedere [Ridimensionare volumi](../storage-spaces/resize-volumes.md).

## <a name="creating-a-mirror-accelerated-parity-volume"></a>Creazione di un volume di parità accelerata con mirroring

Il cmdlet PowerShell seguente crea un volume con parità accelerata con mirroring con un rapporto mirror/parità del 20:80, ovvero la configurazione consigliata per la maggior parte dei carichi di lavoro. Per ulteriori informazioni ed esempi, vedi [Creazione di volumi in Spazi di archiviazione diretta](../storage-spaces/Create-volumes.md).

```PowerShell
New-Volume – FriendlyName “TestVolume” -FileSystem CSVFS_ReFS -StoragePoolFriendlyName “StoragePoolName” -StorageTierFriendlyNames Performance, Capacity -StorageTierSizes 200GB, 800GB
```

## <a name="see-also"></a>Vedere anche

-   [Cenni preliminari su reFS](refs-overview.md)
-   [Clonazione dei blocchi reFS](block-cloning.md)
-   [Flussi di integrità di reFS](integrity-streams.md)
-   [Panoramica di spazi diretti di archiviazione](../storage-spaces/storage-spaces-direct-overview.md)
