---
title: Informazioni sulla cache in Spazi di archiviazione diretta
ms.assetid: 69b1adc0-ee64-4eed-9732-0fb216777992
ms.prod: windows-server
ms.author: cosdar
ms.manager: dongill
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 07/17/2019
ms.localizationpriority: medium
ms.openlocfilehash: f2c2e0435d06c18dbacab4e85db770ba86e654b3
ms.sourcegitcommit: b5c12007b4c8fdad56076d4827790a79686596af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78865420"
---
# <a name="understanding-the-cache-in-storage-spaces-direct"></a>Informazioni sulla cache in Spazi di archiviazione diretta

>Si applica a: Windows Server 2019, Windows Server 2016

[Spazi di archiviazione diretta](storage-spaces-direct-overview.md) dispone di una cache integrata lato server per ottimizzare le prestazioni di archiviazione. Si tratta di una cache di lettura *e* scrittura di grandi dimensioni, permanente e in tempo reale. La cache viene configurata automaticamente quando si abilita Spazi di archiviazione diretta. Nella maggior parte dei casi non è richiesta alcuna gestione manuale.
Il funzionamento della cache dipende dai tipi di unità presenti.

Il video seguente descrive dettagliatamente il funzionamento della memorizzazione nella cache per Spazi di archiviazione diretta e fornisce altre considerazioni sulla progettazione.

<strong>Considerazioni sulla progettazione di Spazi di archiviazione diretta</strong><br>(20 minuti)<br>
<iframe src="https://channel9.msdn.com/Blogs/windowsserver/Design-Considerations-for-Storage-Spaces-Direct/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="drive-types-and-deployment-options"></a>Tipi di unità e opzioni di distribuzione

Attualmente, Spazi di archiviazione diretta è utilizzabile con tre tipi di dispositivi di archiviazione:

<table>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:70px">
            <img src="media/understand-the-cache/NVMe-100px.png">
        </td>
        <td style="padding: 10px; border: 0;" valign="middle">
            Unità NVMe (Non-Volatile Memory Express)
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:70px">
            <img src="media/understand-the-cache/SSD-100px.png">
        </td>
        <td style="padding: 10px; border: 0;" valign="middle">
            Unità SATA/SAS SSD (Solid-State Drive)
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:70px">
            <img src="media/understand-the-cache/HDD-100px.png">
        </td>
        <td style="padding: 10px; border: 0;" valign="middle">
            Unità disco rigido
        </td>
    </tr>
</table>

Tali unità possono essere combinate in sei modi, raggruppati in due categorie: "all-flash" e "ibride".

### <a name="all-flash-deployment-possibilities"></a>Possibilità di distribuzione all-flash

Lo scopo delle distribuzioni all-flash è di ottimizzare le prestazioni di archiviazione. Questo tipo di distribuzioni non include le unità disco rigido rotazionali.

![All-Flash-Deployment-Possibilities](media/understand-the-cache/All-Flash-Deployment-Possibilities.png)

### <a name="hybrid-deployment-possibilities"></a>Possibilità di distribuzione ibrida

Lo scopo delle distribuzioni ibride è di bilanciare le prestazioni e la capacità oppure ottimizzare. Questo tipo di distribuzioni include le unità disco rigido rotazionali.

![Hybrid-Deployment-Possibilities](media/understand-the-cache/Hybrid-Deployment-Possibilities.png)

## <a name="cache-drives-are-selected-automatically"></a>Le unità cache vengono selezionate automaticamente

Nelle distribuzioni con più tipi di unità, Spazi di archiviazione diretta utilizza automaticamente tutte le unità del tipo "più rapido" per l'inserimento nella cache. Le unità rimanenti vengono utilizzate per la capacità.

Il tipo "più rapido" viene determinato in base alla gerarchia seguente.

![Drive-Type-Hierarchy](media/understand-the-cache/Drive-Type-Hierarchy.png)

Ad esempio, se si dispone di unità NVMe e SSD, le unità NVMe fungeranno da cache per le unità SSD.

Se si dispone di unità SSD e di unità disco rigido, le prime fungeranno da cache per le unità disco rigido.

   >[!NOTE]
   > Le unità cache non contribuiscono alla capacità di archiviazione utilizzabile. Tutti i dati archiviati nella cache vengono archiviati anche altrove o lo saranno nel momento in cui vengono rimossi dalla cache. Ciò significa che la capacità di archiviazione totale non elaborata della distribuzione è unicamente la somma delle unità di capacità.

Se tutte le unità sono dello stesso tipo, nessuna cache viene configurata automaticamente. È disponibile l'opzione per configurare manualmente unità a maggiore resistenza in modo che fungano da cache per le unità a minore resistenza dello stesso tipo. Per scoprire come, vedere la sezione [Configurazione manuale](#manual-configuration).

   >[!TIP]
   > Nelle distribuzioni in cui sono presenti tutte unità NVMe oppure tutte unità SSD, specialmente su scala molto ridotta, l'assenza di unità destinate alla cache può migliorare l'efficienza dell'archiviazione in modo significativo.

## <a name="cache-behavior-is-set-automatically"></a>Il comportamento della cache viene impostato automaticamente

Il comportamento della cache è determinato automaticamente in base ai tipi di unità che vengono utilizzati per fungere da cache. Se la cache è utilizzata per le unità SSD (ad esempio se le unità NVMe fungono da cache per le unità SSD), all'interno della cache vengono memorizzate solo le scritture. Se la cache è utilizzata per le unità disco rigido (ad esempio, se le unità SSD fungono da cache per le unità disco rigido), nella cache vengono memorizzate sia le letture che le scritture.

![Cache-Read-Write-Behavior](media/understand-the-cache/Cache-Read-Write-Behavior.png)

### <a name="write-only-caching-for-all-flash-deployments"></a>Cache in sola scrittura per le distribuzioni all-flash

Se la cache è utilizzata per le unità SSD (NVMe o unità SSD), nella cache vengono memorizzate solo le scritture. In tal modo, l'usura delle unità di capacità viene ridotta, in quanto la cache consente di unire un gran numero di scritture e riscritture per poi essere eliminate in caso di necessità, riducendo il traffico cumulativo alle unità di capacità e prolungandone la durata. Per questa ragione, è consigliabile selezionare unità [a maggiore resistenza, ottimizzate per la scrittura](http://whatis.techtarget.com/definition/DWPD-device-drive-writes-per-day) per la cache. Le unità di capacità possono ragionevolmente disporre di una minore resistenza in scrittura.

Poiché le letture non influiscono in modo significativo sulla durata del flash e dal momento che le unità SSD offrono latenza di lettura ridotta a livello generale, le letture non vengono inserite nella cache: esse vengono servite direttamente dalle unità di capacità (tranne nei casi in cui la scrittura dei dati è così recente che essi non sono stati ancora rimossi dalla cache). Ciò consente di dedicare la cache interamente alle scritture, ottimizzandone l'efficienza.

Di conseguenza, le caratteristiche di scrittura quali la latenza, sono determinate dalle unità cache, mentre le caratteristiche di lettura sono determinate dalle unità di capacità. Entrambe sono coerenti, prevedibili e uniformi.

### <a name="readwrite-caching-for-hybrid-deployments"></a>Memorizzazione nella cache di lettura/scrittura per le distribuzioni ibride

Se la cache è utilizzata per le unità disco rigido, le letture *e* le scritture vengono memorizzate entrambe nella cache, per fornire una latenza di tipo flash (spesso migliore di circa 10 volte) per entrambe. All'interno della cache di lettura vengono memorizzati i dati letti di recente e frequentemente, per l'accesso rapido e per ridurre al minimo il traffico casuale alle unità disco rigido (a causa dei ritardi rotazionali, la latenza e il tempo perso correlati all'accesso casuale a un'unità disco rigido sono significativi). Le scritture vengono memorizzate nella cache per assorbire i picchi e, come in precedenza, per unire scritture e riscritture e ridurre al minimo il traffico cumulativo alle unità di capacità.

Spazi di archiviazione diretta implementa un algoritmo che annulla la generazione casuale delle scritture prima di rimuoverle dalla cache, per emulare un modello di I/O al disco che sembri sequenziale persino se l'attuale I/O proveniente dal carico di lavoro (ad esempio le macchine virtuali) è casuale. In tal modo IOPS e velocità effettiva alle unità disco rigido vengono ottimizzate.

### <a name="caching-in-deployments-with-drives-of-all-three-types"></a>Memorizzazione nella cache delle distribuzioni con unità di tutti e tre i tipi

Se sono presenti unità di tutti e tre i tipi, le unità NVMe forniscono la memorizzazione nella cache sia per le unità SSD che per le unità disco rigido. Il comportamento è quello descritto in precedenza: per le unità SSD solo le scritture vengono memorizzate nella cache, mentre per le unità disco rigido vengono memorizzate sia le letture che le scritture. L'onere della memorizzazione nella cache per le unità disco rigido viene distribuito in modo equo tra le unità della cache. 

## <a name="summary"></a>Riepilogo

Questa tabella riepiloga le unità utilizzate per la memorizzazione nella cache, quelle utilizzate per la capacità e il comportamento di memorizzazione nella cache relativo a ciascuna possibilità di distribuzione.

| Distribuzione     | Unità della cache                        | Unità di capacità | Comportamento della cache (predefinito)  |
| -------------- | ----------------------------------- | --------------- | ------------------------- |
| Tutte unità NVMe         | Nessuna (facoltativo: configurare manualmente) | NVMe            | Solo scrittura (se configurata)  |
| Tutte unità SSD          | Nessuna (facoltativo: configurare manualmente) | SSD             | Solo scrittura (se configurata)  |
| Unità NVMe + SSD       | NVMe                                | SSD             | Sola scrittura                  |
| Unità NVMe + HDD       | NVMe                                | Unità disco rigido             | Lettura + scrittura                |
| Unità SSD + unità disco rigido        | SSD                                 | Unità disco rigido             | Lettura + scrittura                |
| Unità NVMe + SSD + HDD | NVMe                                | Unità SSD + unità disco rigido       | Lettura + scrittura per unità disco rigido, sola scrittura per unità SSD  |

## <a name="server-side-architecture"></a>Architettura lato server

La cache è implementata a livello di unità: le singole unità cache all'interno di un server sono associate a una o molte unità di capacità all'interno dello stesso server.

Poiché la cache si trova al di sotto del restante stack dell'archiviazione definita da software di Windows, non riconosce né ha necessità di riconoscere concetti quali Spazi di archiviazione o tolleranza di errore. Può essere considerata lo strumento di creazione di unità "ibride" (in parte flash, in parte disco) che vengono presentate a Windows. Come accade con un'unità ibrida effettiva, lo spostamento in tempo reale dei dati ad accesso frequente e di quelli ad accesso sporadico tra le parti più rapide e quelle più lente dei supporti fisici è pressoché invisibile dall'esterno.

Dato che la resilienza in Spazi di archiviazione diretta è almeno a livello server (per cui le copie dei dati vengono scritte su server diversi; almeno una copia per server), i dati nella cache usufruiscono della stessa resilienza dei dati che non si trovano nella cache.

![Cache-Server-Side-Architecture](media/understand-the-cache/Cache-Server-Side-Architecture.png)

Ad esempio, quando si utilizza il mirroring a tre vie, tre copie di tutti i dati vengono scritte su server diversi, all'interno della cache. A prescindere dall'eventualità che vengano rimossi o meno dalla cache successivamente, esisteranno sempre tre copie.

## <a name="drive-bindings-are-dynamic"></a>Le associazioni di dati sono dinamiche

Il rapporto tra unità cache e unità di capacità nell'ambito dell'associazione può variare da 1:1 fino a 1:12 e oltre. Tale rapporto si regola dinamicamente ogni volta che si aggiungono o si rimuovono le unità, ad esempio durante un aumento o dopo i guasti. Ciò significa che è possibile aggiungere unità cache o unità di capacità in modo indipendente, ogni volta che si desidera.

![Dynamic-Binding](media/understand-the-cache/Dynamic-Binding.gif)

È consigliabile che il numero delle unità di capacità sia un multiplo del numero delle unità cache, per simmetria. Ad esempio, con quattro unità cache, le prestazioni risulteranno più uniformi con otto unità di capacità (rapporto 1:2) che con 7 o 9.

## <a name="handling-cache-drive-failures"></a>Gestione degli errori delle unità cache

In caso di errore di un'unità cache, le scritture non ancora rimosse dalla cache vengono perse *nel server locale*, pertanto esistono solo nelle altre copie (negli altri server). Come accade dopo qualsiasi altro errore di unità, Spazi di archiviazione può eseguire il ripristino automatico, consultando le copie sopravvissute, ed è proprio questa l'operazione che viene eseguita.

Per un breve periodo, le unità di capacità associate all'unità cache persa appariranno non integre. Una volta eseguita la riassociazione della cache (operazione automatica) e completata la riparazione dei dati (operazione automatica), torneranno a essere visualizzate come integre.

Questo scenario spiega il motivo per cui per preservare le prestazioni, per ogni server sono necessarie almeno due unità cache.

![Handling-Failure](media/understand-the-cache/Handling-Failure.gif)

L'unità cache può essere sostituita come qualsiasi altra unità.

   >[!NOTE]
   > Per sostituire in sicurezza NVMe con fattore di forma Add-In Card (AIC) o M.2 potrebbe essere necessario spegnere il computer.

## <a name="relationship-to-other-caches"></a>Rapporti con le altre cache

Nello stack dell'archiviazione definita da software di Windows sono presenti varie altre cache non correlate. Tra gli esempi sono incluse la cache write-back Spazi di archiviazione e la cache di lettura in memoria Volume condiviso cluster.

Con Spazi di archiviazione diretta, il comportamento predefinito della cache write-back Spazi di archiviazione non deve essere modificato. Ad esempio, parametri quali **-WriteCacheSize** nel cmdlet **New-Volume** non devono essere utilizzati.

Se lo si desidera, è possibile scegliere di utilizzare la cache Volume condiviso cluster. Questa cache è disattivata per impostazione predefinita in Spazi di archiviazione diretta, ma non è in conflitto in alcun modo con la nuova cache descritta in questo argomento. In determinati scenari, può fornire preziosi guadagni in termini di prestazioni. Per ulteriori informazioni, vedere [How to Enable CSV Cache](../../failover-clustering/failover-cluster-csvs.md#enable-the-csv-cache-for-read-intensive-workloads-optional) (Come abilitare la cache Volume condiviso cluster).

## <a name="manual-configuration"></a>Configurazione manuale

Per la maggior parte delle implementazioni non è richiesta la configurazione manuale. Se necessario, vedere le sezioni seguenti. 

Se è necessario apportare modifiche al modello di dispositivo di cache dopo l'installazione, modificare il documento relativo ai componenti di supporto di Servizio integrità, come descritto in [servizio integrità Overview](../../failover-clustering/health-service-overview.md#supported-components-document).

### <a name="specify-cache-drive-model"></a>Specificare il modello di unità cache

Nelle distribuzioni in cui tutte le unità sono dello stesso tipo, ad esempio tutte NVMe o tutte SSD, non viene configurata alcuna cache, in quanto Windows non è in grado di distinguere automaticamente caratteristiche quali la resistenza in scrittura tra unità dello stesso tipo.

Per utilizzare le unità a maggiore resistenza come cache per le unità a minore resistenza dello stesso tipo, è possibile specificare quale modello di unità utilizzare con il parametro **-CacheDeviceModel** del cmdlet **Enable-ClusterS2D**. Quando Spazi di archiviazione diretta è abilitato, tutte le unità di tale modello saranno utilizzate per la memorizzazione nella cache.

   >[!TIP]
   > Accertarsi di utilizzare la stessa stringa di modello che appare nell'output di **Get-PhysicalDisk**.

####  <a name="example"></a>Esempio

Per prima cosa, ottenere un elenco di dischi fisici:

```PowerShell
Get-PhysicalDisk | Group Model -NoElement
```

Ecco un output di esempio:

```
Count Name
----- ----
    8 FABRIKAM NVME-1710
   16 CONTOSO NVME-1520
```

Quindi immettere il comando seguente, specificando il modello di dispositivo della cache:

```PowerShell
Enable-ClusterS2D -CacheDeviceModel "FABRIKAM NVME-1710"
```

È possibile verificare che per la memorizzazione nella cache siano utilizzate le unità desiderate eseguendo **Get-PhysicalDisk** in PowerShell e verificando che il valore della proprietà **Usage** sia **"Journal"** .

### <a name="manual-deployment-possibilities"></a>Possibilità di distribuzione manuale

La configurazione manuale consente le seguenti possibilità di distribuzione.

![Exotic-Deployment-Possibilities](media/understand-the-cache/Exotic-Deployment-Possibilities.png)

### <a name="set-cache-behavior"></a>Impostare il comportamento della cache

È possibile eseguire l'override del comportamento predefinito della cache. Ad esempio, è possibile impostarlo in modo che la cache effettui le letture persino nella distribuzione all-flash. Non è consigliabile modificare il comportamento se non si è certi che l'impostazione predefinita non sia indicata per il proprio carico di lavoro.

Per eseguire l'override del comportamento, usare il cmdlet **set-ClusterStorageSpacesDirect** e i relativi parametri **-CacheModeSSD** e **-CacheModeHDD** . Il parametro **CacheModeSSD** imposta il comportamento della cache quando questa viene utilizzata per le unità SSD. Il parametro **CacheModeHDD** imposta il comportamento della cache quando questa viene utilizzata per le unità disco rigido. L'operazione può essere eseguita in qualsiasi momento dopo aver abilitato Spazi di archiviazione diretta.

È possibile usare **Get-ClusterStorageSpacesDirect** per verificare che sia impostato il comportamento.

#### <a name="example"></a>Esempio

Per prima cosa, ottenere le impostazioni Spazi di archiviazione diretta:

```PowerShell
Get-ClusterStorageSpacesDirect
```

Ecco un output di esempio:

```
CacheModeHDD : ReadWrite
CacheModeSSD : WriteOnly
```

Procedere quindi come segue:

```PowerShell
Set-ClusterStorageSpacesDirect -CacheModeSSD ReadWrite

Get-ClusterS2D
```

Ecco un output di esempio:

```
CacheModeHDD : ReadWrite
CacheModeSSD : ReadWrite
```

## <a name="sizing-the-cache"></a>Ridimensionamento della cache

Le dimensioni della cache dovrebbero essere tali da accogliere il working set (i dati che vengono letti o scritti attivamente in ogni dato momento) delle applicazioni e dei carichi di lavoro.

Ciò è particolarmente importante nelle distribuzioni ibride con unità disco rigido. Se il working set attivo supera le dimensioni della cache o se il working set attivo devia troppo rapidamente, i mancati riscontri della cache di lettura aumenteranno e le scritture dovranno essere rimosse dalla cache con maggiore determinazione, compromettendo le prestazioni complessive.

Per esaminare la frequenza dei mancati riscontri della cache, è possibile avvalersi dell'utilità Performance Monitor (PerfMon.exe) integrata in Windows. In modo specifico, è possibile confrontare il valore **Cache Miss Reads/sec** dell'insieme di contatori **Cluster Storage Hybrid Disk** impostato, con il numero di IOPS di lettura complessivo della distribuzione. Ciascun "Hybrid Disk" corrisponde a un'unità di capacità.

Ad esempio, 2 unità cache associate a 4 unità di capacità generano 4 istanze di oggetti "Hybrid Disk" per server.

![Performance-Monitor](media/understand-the-cache/PerfMon.png)

Non esiste una regola universale, tuttavia, se il numero di mancati riscontri della cache è eccessivo, le dimensioni potrebbero essere insufficienti e sarà necessario prendere in considerazione l'aggiunta di unità cache per espandere la cache. È possibile aggiungere unità cache o unità di capacità in modo indipendente, ogni volta che si desidera.

## <a name="see-also"></a>Vedere anche

- [Scelta di unità e tipi di resilienza](choosing-drives.md)
- [Tolleranza di errore ed efficienza di archiviazione](storage-spaces-fault-tolerance.md)
- [Requisiti hardware Spazi di archiviazione diretta](storage-spaces-direct-hardware-requirements.md)
