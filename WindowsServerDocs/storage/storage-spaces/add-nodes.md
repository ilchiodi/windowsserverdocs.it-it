---
ms.assetid: 898d72f1-01e7-4b87-8eb3-a8e0e2e6e6da
title: Aggiunta di server o unità a Spazi di archiviazione diretta
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: dongill
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 11/06/2017
description: Come aggiungere server o unità a un cluster di spazi di archiviazione diretta
ms.localizationpriority: medium
ms.openlocfilehash: ae639b920788911dbc16952d7b61aab85b0a391b
ms.sourcegitcommit: dfd25348ea3587e09ea8c2224237a3e8078422ae
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/16/2018
ms.locfileid: "4678600"
---
# Aggiunta di server o unità a Spazi di archiviazione diretta

>Si applica a: Windows Server 2019, Windows Server 2016

Questo argomento descrive come aggiungere server o unità a Spazi di archiviazione diretta.

## <a name="adding-servers"></a> Aggiunta di server

L'aggiunta di server (spesso chiamata anche scalabilità orizzontale) consente di aggiungere capacità di archiviazione, di migliorarne le prestazioni e di sbloccare una migliore efficienza di archiviazione. Se la distribuzione è iperconvergente, l'aggiunta di server rende disponibili altre risorse di calcolo per il carico di lavoro.

![Animazione di aggiunta di un server a un cluster di quattro nodi](media/add-nodes/Scaling-Out.gif)

Per le distribuzioni tipiche è facile eseguire la scalabilità orizzontale tramite l'aggiunta di server. La procedura include solo due passaggi:

1. Eseguire la [procedura guidata per la convalida dei cluster](https://technet.microsoft.com/library/cc732035(v=ws.10).aspx) utilizzando lo snap-in dei cluster di failover o con il cmdlet **Test-Cluster** in PowerShell (Esegui come amministratore). Includere il nuovo server *\<NewNode>* che si desidera aggiungere.

   ```PowerShell
   Test-Cluster -Node <Node>, <Node>, <Node>, <NewNode> -Include "Storage Spaces Direct", Inventory, Network, "System Configuration"
   ```

   Questo comando conferma che il nuovo server esegue Windows Server 2016 Datacenter Edition, è stato aggiunto allo stesso dominio Active Directory Domain Services dei server esistenti, dispone di tutti i ruoli e di tutte le funzionalità richiesti e che le funzioni di rete sono configurate correttamente.

   >[!IMPORTANT]
   > Se si stanno riutilizzando unità che contengono dati o metadati obsoleti non più necessari, cancellarli con **Gestione disco** o con il cmdlet **Reset-PhysicalDisk**. Se vengono rilevati dati o metadati obsoleti, le unità non sono messe in pool.

2. Eseguire il seguente cmdlet sul cluster per completare l'aggiunta del server:

```
Add-ClusterNode -Name NewNode 
```

   >[!NOTE]
   > Il pooling automatico dipende dalla presenza di un unico pool. Se si è aggirata la configurazione standard per creare più pool, è necessario aggiungere manualmente nuove unità per il pool preferito tramite **Add–PhysicalDisk**.

### Da due a tre server: sblocco del mirroring a 3 vie

![aggiunta di un terzo server a un cluster di due nodi](media/add-nodes/Scaling-2-to-3.png)

Con due server è possibile creare solo volumi con mirroring a 2 vie (confrontare con RAID–1 distribuito). Con tre server è possibile creare volumi con mirroring a 3 vie per una migliore tolleranza di errore. È consigliabile usare il mirroring a 3 vie ogni volta che sia possibile.

Non è possibile eseguire l'aggiornamento sul posto dei volumi con mirroring a 2 vie al mirroring a 3 vie. È possibile invece creare un nuovo volume e migrare (copiare, ad esempio usando [Replica archiviazione](../storage-replica/server-to-server-storage-replication.md)) i dati in esso e quindi rimuovere il vecchio volume.

Per iniziare la creazione di volumi con mirroring a 3 vie sono disponibili diverse opzioni. È possibile utilizzare quella preferita. 

#### Opzione 1

Specificare **PhysicalDiskRedundancy = 2** per ogni nuovo volume al momento della creazione.

```PowerShell
New-Volume -FriendlyName <Name> -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size <Size> -PhysicalDiskRedundancy 2
```

#### Opzione 2

Oppure, è possibile impostare **PhysicalDiskRedundancyDefault = 2** nell'oggetto **ResiliencySetting** denominato **Mirror** del pool. Quindi, tutti i nuovi volumi con mirroring utilizzeranno automaticamente il mirroring *a 3 vie* anche se non è specificato.

```PowerShell
Get-StoragePool S2D* | Get-ResiliencySetting -Name Mirror | Set-ResiliencySetting -PhysicalDiskRedundancyDefault 2

New-Volume -FriendlyName <Name> -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size <Size>
```

#### Opzione 3

Impostare **PhysicalDiskRedundancy = 2** per il modello **StorageTier** denominato *Capacity* e quindi creare i volumi facendo riferimento al livello.

```PowerShell
Set-StorageTier -FriendlyName Capacity -PhysicalDiskRedundancy 2 

New-Volume -FriendlyName <Name> -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -StorageTierFriendlyNames Capacity -StorageTierSizes <Size>
```

### Da tre a quattro server: sblocco della doppia parità

![aggiungere un server quarto in un cluster di tre nodi](media/add-nodes/Scaling-3-to-4.png)

Con quattro server è possibile usare la doppia parità, detta anche codifica di cancellazione (confrontare con RAID–6 distribuito). Ciò consente la stessa tolleranza di errori del mirroring a 3 vie, ma con una migliore efficienza di archiviazione. Per ulteriori informazioni, vedere [Tolleranza di errore ed efficienza di archiviazione](storage-spaces-fault-tolerance.md).

Se si passa a questa distribuzione da una distribuzione più ridotta, sono disponibili diverse opzioni per la creazione di volumi a doppia parità. È possibile utilizzare quella preferita.

#### Opzione 1

Specificare **PhysicalDiskRedundancy = 2** e **ResiliencySettingName = Parity** per ogni nuovo volume al momento della creazione.

```PowerShell
New-Volume -FriendlyName <Name> -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size <Size> -PhysicalDiskRedundancy 2 -ResiliencySettingName Parity
```

#### Opzione 2

Impostare **PhysicalDiskRedundancyDefault = 2** nell'oggetto **ResiliencySetting** denominato **Parity** del pool. Quindi, tutti i nuovi volumi con parità utilizzeranno automaticamente la parità *doppia* anche se non è specificata

```PowerShell
Get-StoragePool S2D* | Get-ResiliencySetting -Name Parity | Set-ResiliencySetting -PhysicalDiskRedundancyDefault 2

New-Volume -FriendlyName <Name> -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size <Size> -ResiliencySettingName Parity
```

Con quattro server è anche possibile iniziare con una parità accelerata con mirror, dove un singolo volume è in parte mirror e in parte parità.

A tale scopo, sarà necessario aggiornare i modelli **StorageTier** per avere sia il livello *Performance* che il livello *Capacity*, come sarebbero stati creati se prima si fosse eseguito **Enable–ClusterS2D** su quattro server. In particolare, entrambi i livelli devono avere **MediaType** dei dispositivi di capacità (quali, ad esempio SSD o HDD) e **PhysicalDiskRedundancy = 2**. Il livello *Performance* deve corrispondere a **ResiliencySettingName = Mirror** e il livello *Capacity* deve corrispondere a **ResiliencySettingName = Parity**.

#### Opzione 3

La cosa più semplice potrebbe essere rimuovere il modello del livello esistente e crearne due nuovi. Ciò non influirà su eventuali volumi preesistenti creati facendo riferimento al modello del livello: si tratta semplicemente di un modello.

```PowerShell
Remove-StorageTier -FriendlyName Capacity

New-StorageTier -StoragePoolFriendlyName S2D* -MediaType HDD -PhysicalDiskRedundancy 2 -ResiliencySettingName Mirror -FriendlyName Performance
New-StorageTier -StoragePoolFriendlyName S2D* -MediaType HDD -PhysicalDiskRedundancy 2 -ResiliencySettingName Parity -FriendlyName Capacity
```

Ecco fatto! È ora possibile creare volumi con parità accelerata con mirror facendo riferimento a questi modelli di livello.

#### Esempio

```PowerShell
New-Volume -FriendlyName "Sir-Mix-A-Lot" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -StorageTierFriendlyNames Performance, Capacity -StorageTierSizes <Size, Size> 
```

### Oltre quattro server: maggiore efficienza della parità

Man mano che si esegue il ridimensionamento oltre i quattro server, i nuovi volumi possono avvalersi di un'efficienza di codifica della parità ancora maggiore. Ad esempio, tra sei e sette server l'efficienza migliora dal 50,0% al 66,7%, poiché è ora possibile usare Reed–Solomon 4+2, anziché 2+2. Non sono previsti passaggi da eseguire per usufruire di questa nuova efficienza. La codifica più efficiente viene determinata automaticamente ogni volta che si crea un volume.

Tuttavia, i volumi esistenti *non* vengono "convertiti" alla nuova codifica più ampia. Un valido motivo è che questa operazione richiederebbe un calcolo di grandissima portata, che interesserebbe letteralmente *ogni singolo bit* dell'intera distribuzione. Se si vuole che i dati preesistenti siano codificati con una maggiore efficienza, è possibile eseguire la migrazione a nuovi volumi.

Per ulteriori dettagli, vedere [Tolleranza di errore ed efficienza di archiviazione](storage-spaces-fault-tolerance.md).

### Aggiunta di server con la tolleranza di errore chassis o rack

Se la distribuzione usa la tolleranza di errore chassis o rack, è necessario specificare lo chassis o il rack dei nuovi server prima di aggiungerli al cluster. Questo indica a Spazi di archiviazione diretta il modo più efficiente per distribuire i dati ottimizzando la tolleranza di errore.

1. Creare un dominio di errore temporaneo per il nodo aprendo una sessione di PowerShell con privilegi elevati e quindi usando il comando seguente, dove *\<NewNode>* è il nome del nuovo nodo del cluster:

   ```PowerShell
   New-ClusterFaultDomain -Type Node -Name <NewNode> 
   ```

2. Spostare il dominio di errore temporaneo nello chassis o nel rack in cui si trova fisicamente il nuovo server, come specificato da *\<ParentName>*:

   ```PowerShell
   Set-ClusterFaultDomain -Name <NewNode> -Parent <ParentName> 
   ```

   Per ulteriori informazioni, vedere [Informazioni sulla presenza di un dominio di errore in Windows Server 2016](../../failover-clustering/fault-domains.md).

3. Aggiungere il server al cluster come descritto in [Aggiunta di server](#adding-servers). Quando si aggiunge il nuovo server al cluster, viene automaticamente associato, tramite il nome, al dominio di errore segnaposto.

## <a name="adding-drives"></a> Aggiunta di unità

L'aggiunta di unità, detta anche scalabilità verticale, consente di aggiungere capacità di archiviazione e può anche garantire prestazioni migliori. Se sono disponibili slot liberi, è possibile aggiungere unità a ogni server per espandere la capacità di archiviazione senza aggiungere server. È possibile aggiungere unità di cache o unità di capacità, in modo indipendente in qualsiasi momento.

   >[!IMPORTANT]
   > È consigliabile configurare tutti i server con configurazioni di archiviazione identiche.

![Unità di animazione che mostra l'aggiunta a un sistema](media/add-nodes/Scale-Up.gif)

Per eseguire la scalabilità verticale, connettere le unità e verificare che Windows le individui. Queste unità dovrebbero essere visualizzate nell'output del cmdlet **Get-PhysicalDisk** in PowerShell con la proprietà **CanPool** impostata su **True**. Se vengono visualizzate con **CanPool = False**, è possibile capirne il motivo controllando la proprietà **CannotPoolReason**.

```PowerShell
Get-PhysicalDisk | Select SerialNumber, CanPool, CannotPoolReason
```

In breve tempo le unità idonee verranno automaticamente richieste da Spazi di archiviazione diretta e aggiunte al pool di archiviazione. I volumi verranno automaticamente [ridistribuiti in modo uniforme tra tutte le unità](https://blogs.technet.microsoft.com/filecab/2016/11/21/deep-dive-pool-in-spaces-direct/). A questo punto, puoi [estendere i volumi](resize-volumes.md) o [crearne di nuovi](create-volumes.md).

Se le unità non vengono visualizzate, esegui manualmente l'analisi per rilevare le modifiche hardware. Per eseguire questa operazione usare **Gestione dispositivi** nel menu **Azione**. Se le unità contengono dati o metadati obsoleti, considerare la possibilità di riformattarle. A tale scopo è possibile utilizzare **Gestione disco** o il cmdlet **Reset-PhysicalDisk**.

   >[!NOTE]
   > Il pooling automatico dipende dalla presenza di un unico pool. Se hai aggirato la configurazione standard per creare più pool, dovrai aggiungere manualmente nuove unità per il pool preferito tramite **Add–PhysicalDisk**.

## Ottimizzazione dell'utilizzo di unità dopo l'aggiunta di unità o server

Nel corso del tempo, come unità vengono aggiunti o rimossi, la distribuzione dei dati tra le unità nel pool di possa diventare non uniforme. In alcuni casi, ciò può comportare alcune unità riempimento mentre altre unità nel pool hanno molto più basso consumo.

Per mantenere anche attraverso il pool di allocazione di unità, spazi di archiviazione diretta di ottimizzare automaticamente all'uso di unità dopo l'aggiunta di unità o server al pool (questo è un processo manuale per i sistemi di spazi di archiviazione che usano alloggiamenti SAS condivisi). Ottimizzazione inizia 15 minuti dopo aver aggiunto una nuova unità al pool. Ottimizzazione del pool viene eseguito come un'operazione in background con priorità bassa, in modo che può richiedere ore o giorni per completare, in particolare se usi unità disco rigida di grandi dimensioni.

Ottimizzazione usa due processi - one denominata *Ottimizza* e uno denominata *ribilanciare* - ed è possibile monitorare lo stato di avanzamento con il comando seguente:

```powershell
Get-StorageJob
```

Puoi manualmente ottimizzare un pool di archiviazione con il cmdlet [Optimize-StoragePool](https://docs.microsoft.com/powershell/module/storage/optimize-storagepool?view=win10-ps) . Ecco un esempio:

```powershell
Get-StoragePool <PoolName> | Optimize-StoragePool
```
