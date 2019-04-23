---
title: Resilienza annidata di spazi di archiviazione diretta
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: dansimp
ms.technology: storagespaces
ms.topic: article
author: cosmosdarwin
ms.date: 11/06/2018
ms.openlocfilehash: 206d5d19ec55774f9055e7265d4c87d276c60590
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879572"
---
# <a name="nested-resiliency-for-storage-spaces-direct"></a>Resilienza annidata di spazi di archiviazione diretta

> Si applica a: Windows Server 2019

La resilienza annidata è una nuova funzionalità di [spazi di archiviazione diretta](storage-spaces-direct-overview.md) in Windows Server 2019 che consente a un cluster a due server di resistere agli errori hardware più nello stesso momento senza perdere la disponibilità di archiviazione, pertanto gli utenti, App, e le macchine virtuali continuano a essere eseguiti senza interruzioni. In questo argomento spiega come funziona, fornisce istruzioni dettagliate per iniziare a usare e le risposte più domande frequenti.

## <a name="prerequisites"></a>Prerequisiti

### <a name="green-checkmark-iconmedianested-resiliencysupportedpng-consider-nested-resiliency-if"></a>![Icona segno di spunta verde.](media/nested-resiliency/supported.png) Prendere in considerazione la resilienza nidificata se:

- Il cluster esegua Windows Server 2019; e
- Il cluster ha esattamente 2 nodi del server

### <a name="red-x-iconmedianested-resiliencyunsupportedpng-you-cant-use-nested-resiliency-if"></a>![Icona X rossa.](media/nested-resiliency/unsupported.png) È possibile usare la resilienza nidificata se:

- Il cluster esegua Windows Server 2016; o
- Nel cluster sono disponibili 3 o più nodi del server

## <a name="why-nested-resiliency"></a>Il motivo per cui la resilienza annidata

Volumi che usano la resilienza nidificata possono **rimanere online e accessibile anche se si verificano più errori hardware nello stesso momento**, a differenza di distribuzione classica [mirroring a 2 vie](storage-spaces-fault-tolerance.md) resilienza. Ad esempio, se due unità hanno esito negativo allo stesso tempo, o se un server si arresta e si verifica un errore di un'unità, volumi che usano la resilienza nidificata rimangono online e accessibile. Per l'infrastruttura iperconvergente, in questo modo aumenta il tempo di attività per le App e le macchine virtuali; per carichi di lavoro di file server, questo significa che gli utenti potranno usufruire accesso ininterrotto ai propri file.

![Disponibilità di archiviazione](media/nested-resiliency/storage-availability.png)

Il compromesso è dispone di tale resilienza annidata **ridurre l'efficienza della capacità di mirroring a 2 vie classico**, vale a dire è ottenere leggermente minore spazio utilizzabile. Per informazioni dettagliate, vedere la [efficienza capacità](#capacity-efficiency) sezione riportata di seguito.

## <a name="how-it-works"></a>Come funziona

### <a name="inspiration-raid-51"></a>Trovare l'ispirazione: RAID 5+1

RAID 5 + 1 è un form stabilito di resilienza di archiviazione distribuita che vengono fornite informazioni utili per garantire la resilienza nidificata comprensione. RAID 5 + 1, all'interno di ogni server, la resilienza locale viene fornito da RAID-5, oppure *singola parità*per proteggersi contro la perdita di una singola unità. Quindi, ulteriormente la resilienza è fornita da RAID-1, oppure *mirroring a 2 vie*, tra i due server per la protezione contro la perdita di uno dei due server.

![RAID 5+1](media/nested-resiliency/raid-51.png)

### <a name="two-new-resiliency-options"></a>Due nuove opzioni di resilienza

Spazi di archiviazione diretta in Windows Server 2019 offre due nuove opzioni di resilienza implementate nel software, senza la necessità di hardware specializzato RAID:

- **Mirror a due vie annidati.** All'interno di ogni server, la resilienza locale avviene tramite il mirroring a 2 e quindi avviene la ulteriore resilienza tramite mirroring a 2 vie tra i due server. Si tratta essenzialmente a quattro vie, con due copie in ogni server. Mirroring a 2 vie nidificata offre prestazioni senza compromessi: ma accede a tutte le copie e le letture provengono da qualsiasi copia.

  ![Mirroring a 2 vie annidati](media/nested-resiliency/nested-two-way-mirror.png)

- **Parità con accelerazione mirror annidate.** Annidati combinano mirroring a 2 vie, da sopra, con parità annidate. All'interno di ogni server, la resilienza locale per la maggior parte dei dati viene fornita da singolo [aritmetiche bit per bit parità](storage-spaces-fault-tolerance.md#parity), ad eccezione delle nuove operazioni di scrittura recenti che usano il mirroring a 2. Quindi, un'ulteriore resilienza per tutti i dati viene fornita dal mirroring a 2 vie tra il server. Per altre informazioni sul funzionamento di parità con accelerazione mirror, vedere [parità con accelerazione Mirror](https://docs.microsoft.com/windows-server/storage/refs/mirror-accelerated-parity).

  ![Parità con accelerazione mirror annidate](media/nested-resiliency/nested-mirror-accelerated-parity.png)

### <a name="capacity-efficiency"></a>Efficienza della capacità

L'efficienza della capacità è la percentuale di spazio utilizzabile per [footprint volume](plan-volumes.md#choosing-the-size-of-volumes). Descrive l'overhead di capacità attribuibile a resilienza e dipende dall'opzione resilienza che scelto. Un esempio semplice, l'archiviazione dei dati senza la resilienza è 100% della capacità efficiente (1 TB di dati consuma 1 TB di capacità di archiviazione fisica) durante il mirroring a 2 vie è 50% efficiente (1 TB di dati accetta fino a 2 TB di capacità di archiviazione fisica).

- **Mirroring a 2 vie nidificata** scrive quattro copie di tutti gli elementi, vale a dire per l'archiviazione di 1 TB di dati, è necessario 4 TB di capacità di archiviazione fisica. Anche se la sua semplicità è interessante, l'efficienza capacità del nidificata mirroring a 2 vie 25% è la più bassa di qualsiasi opzione di resilienza in spazi di archiviazione diretta.

- **Annidati parità con accelerazione mirror** consente di ottenere una maggiore efficienza della capacità, circa il 35% - 40%, che dipende da due fattori: il numero della capacità di unità in ogni server e la combinazione di mirror e parità specificati per il volume. Questa tabella fornisce una ricerca per le configurazioni comuni:

  | Unità di capacità per ogni server | mirror del 10% | 20% mirror | mirror del 30% |
  |----------------------------|------------|------------|------------|
  | 4                          | 35.7%      | 34.1%      | 32.6%      |
  | 5                          | 37.7%      | 35.7%      | 33.9%      |
  | 6                          | 39.1%      | 36.8%      | 34.7%      |
  | 7+                         | 40.0%      | 37.5%      | 35.3%      |

  > [!NOTE]
  > **Per curiosità, ecco un esempio di calcoli matematici completo.** Si supponga che abbiamo sei unità di capacità in ognuno dei due server e si vuole creare un solo volume di 100 GB, costituito da 10 GB di mirror e 90 GB di parità. Mirroring a 2 vie locale del server è 50,0% efficiente, vale a dire i 10 GB di dati di mirror richiede 20 GB di archiviare in ogni server. Il mirroring di entrambi i server, il footprint complessivo è 40 GB. Server-local parità singola, in questo caso, è 5/6 = 83.3% efficiente, vale a dire il 90 GB di dati di parità accetta 108 GB per l'archiviazione in ogni server. Il mirroring di entrambi i server, il footprint complessivo è 216 GB. Il footprint complessivo è pertanto [(10 GB/50,0%) + (90 GB / % 83.3)] × 2 = 256 GB, per % 39.1 complessiva dell'efficienza della capacità.

Si noti che l'efficienza della capacità di distribuzione classica (circa il 50%) mirroring a due vie e nidificati con accelerazione mirror parità (fino al 40%) non sono molto diversi. A seconda dei requisiti, l'efficienza della capacità leggermente più bassa potrebbe essere senz' la pena nel significativo aumento della disponibilità di archiviazione. Si sceglie la resilienza per il volume, pertanto è possibile combinare volumi con resilienza annidati e i volumi di mirroring a 2 vie classica all'interno del cluster stesso.

![Tradeoff](media/nested-resiliency/tradeoff.png)

## <a name="usage-in-powershell"></a>Utilizzo in PowerShell

È possibile utilizzare i cmdlet di archiviazione ha familiarità in PowerShell per creare volumi con resilienza con mirroring annidata.

### <a name="step-1-create-storage-tier-templates"></a>Passaggio 1: Creare modelli di archiviazione a livelli

Innanzitutto, creare nuovi modelli di livello di archiviazione usando il `New-StorageTier` cmdlet. È sufficiente eseguire questa operazione una sola volta e quindi ogni nuovo volume creato può fare riferimento a questi modelli. Specificare il `-MediaType` capacità unità del computer e, facoltativamente, il `-FriendlyName` di propria scelta. Non modificare gli altri parametri.

Se le unità di capacità sono dischi rigidi (HDD), avviare PowerShell come amministratore ed eseguire:

```PowerShell 
# For mirror
New-StorageTier -StoragePoolFriendlyName S2D* -FriendlyName NestedMirror -ResiliencySettingName Mirror -MediaType HDD -NumberOfDataCopies 4

# For parity
New-StorageTier -StoragePoolFriendlyName S2D* -FriendlyName NestedParity -ResiliencySettingName Parity -MediaType HDD -NumberOfDataCopies 2 -PhysicalDiskRedundancy 1 -NumberOfGroups 1 -FaultDomainAwareness StorageScaleUnit -ColumnIsolation PhysicalDisk 
``` 

Se le unità di capacità sono unità SSD (), impostare il `-MediaType` a `SSD` invece. Non modificare gli altri parametri.

> [!TIP]
> Verificare i livelli creati correttamente con `Get-StorageTier`.

### <a name="step-2-create-volumes"></a>Passaggio 2: Creare volumi

Quindi, creare nuovi volumi che usano il `New-Volume` cmdlet.

#### <a name="nested-two-way-mirror"></a>Mirroring a 2 vie annidati

Per utilizzare nidificata mirroring a 2 vie, fare riferimento il `NestedMirror` livello di modello e specificare le dimensioni. Ad esempio: 

```PowerShell
New-Volume -StoragePoolFriendlyName S2D* -FriendlyName Volume01 -StorageTierFriendlyNames NestedMirror -StorageTierSizes 500GB
```

#### <a name="nested-mirror-accelerated-parity"></a>Parità con accelerazione mirror annidate

Per usare la parità con accelerazione mirror nidificata, fare riferimento a entrambi i `NestedMirror` e `NestedParity` livello modelli, quindi specificare le dimensioni due dei, uno per ogni parte del volume (eseguire il mirroring in primo luogo, parità secondo). Ad esempio, per creare un solo volume di 500 GB che è mirror a due vie nidificata 20% e l'80% annidati con parità, eseguire:

```PowerShell
New-Volume -StoragePoolFriendlyName S2D* -FriendlyName Volume02 -StorageTierFriendlyNames NestedMirror, NestedParity -StorageTierSizes 100GB, 400GB
```

### <a name="step-3-continue-in-windows-admin-center"></a>Passaggio 3: Continuare in Windows Admin Center

Volumi che usano la resilienza annidata sono visualizzati nel [Windows Admin Center](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center) con Cancella l'assegnazione di etichette, come illustrato nello screenshot seguente. Una volta che vengono create, è possibile gestirli e monitorarli tramite Windows Admin Center esattamente come qualsiasi altro volumi in spazi di archiviazione diretta.

![](media/nested-resiliency/windows-admin-center.png)

### <a name="optional-extend-to-cache-drives"></a>Facoltativo: Estendere alle unità di cache

Con le impostazioni predefinite, resilienza nidificata protegge contro la perdita di più unità di capacità allo stesso tempo, o un server e un'unità di capacità nello stesso momento. Per estendere la protezione per [memorizza nella cache le unità](understand-the-cache.md) dispone di un'ulteriore considerazione: poiché le unità di cache spesso forniscono read *e scrivere* la memorizzazione nella cache per *più* le unità di capacità , l'unico modo per assicurarsi che è in grado di tollerare la perdita di un'unità cache quando l'altro server è inattivo consiste semplicemente non scrive nella cache, ma questo può impattare sulle prestazioni.

Per risolvere questo scenario, spazi di archiviazione diretta offre la possibilità di disabilitare automaticamente la cache in scrittura quando non è disponibile un server in un cluster a due server e quindi abilitare nuovamente la cache in scrittura quando il server è eseguire il backup. Per consentire i riavvii routine senza impatto sulle prestazioni, scrivere la memorizzazione nella cache non viene disabilitato fino a quando il server è rimasto inattivo per 30 minuti.

Per impostare questo comportamento (facoltativo), avviare PowerShell come amministratore ed eseguire:

```PowerShell
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.NestedResiliency.DisableWriteCacheOnNodeDown.Enabled" -Value "True"
```

Una volta impostato su `True`, il comportamento della cache è:

| Situazione                       | Comportamento della cache                           | Può tollerare la perdita di unità di cache? |
|---------------------------------|------------------------------------------|--------------------------------|
| Entrambi i server di backup                 | Cache di lettura e scrittura, completo delle prestazioni | Yes                            |
| Server verso il basso, primo 30 minuti   | Cache di lettura e scrittura, completo delle prestazioni | No (temporaneamente)               |
| Dopo 30 minuti prima          | Letture cache solo le prestazioni peggiorate   | Yes                            |

## <a name="frequently-asked-questions"></a>Domande frequenti

### <a name="can-i-convert-an-existing-volume-between-two-way-mirror-and-nested-resiliency"></a>È possibile convertire un volume esistente tra mirroring a due vie e resilienza nidificata?

No, i volumi non possono essere convertiti tra tipi di resilienza. Per le nuove distribuzioni in Windows Server 2019, decidere anticipo quale tipo di resilienza migliore adatta alle proprie esigenze. Se esegue l'aggiornamento da Windows Server 2016, è possibile creare nuovi volumi con resilienza con mirroring annidata, la migrazione dei dati e quindi eliminare i volumi precedenti.

### <a name="can-i-use-nested-resiliency-with-multiple-types-of-capacity-drives"></a>È possibile usare la resilienza annidata con più tipi di unità di capacità?

Sì, è sufficiente specificare il `-MediaType` di ogni livello di conseguenza durante [passaggio 1](#step-1-create-storage-tier-templates) sopra. Ad esempio, con NVMe, unità SSD e HDD nello stesso cluster, il NVMe fornisce cache mentre gli ultimi due offrono capacità: impostare il `NestedMirror` livello `-MediaType SSD` e il `NestedParity` livello per `-MediaType HDD`. In questo caso, si noti che l'efficienza della capacità di parità dipende dal numero di unità HDD solo, è necessario almeno 4 per ogni server.

### <a name="can-i-use-nested-resiliency-with-3-or-more-servers"></a>È possibile usare la resilienza annidata con 3 o più server?

No, non usare solo la resilienza annidata se il cluster include esattamente 2 server.

### <a name="how-many-drives-do-i-need-to-use-nested-resiliency"></a>Il numero di unità è necessario usare nidificata resilienza?

Il numero minimo di unità necessarie per spazi di archiviazione diretta è 4 unità di capacità per ogni nodo del server, oltre a cache 2 unità per ogni nodo del server (se presente). È rimasta invariata da Windows Server 2016. Non vi è alcun requisito aggiuntivo per garantire la resilienza nidificata e la raccomandazione per la capacità di riserva è invariata troppo.

### <a name="does-nested-resiliency-change-how-drive-replacement-works"></a>Resilienza nidificata modifica come sostituzione di un'unità funziona?

No.

### <a name="does-nested-resiliency-change-how-server-node-replacement-works"></a>Resilienza nidificata modifica come sostituzione nodo server funziona?

No. Per sostituire un nodo del server con le relative unità, seguire questo ordine:

1. Ritirare le unità nel server in uscita
2. Aggiungere il nuovo server, con relative unità, al cluster
3. Bilancerà il pool di archiviazione
4. Rimuovere il server in uscita con le relative unità

Per altre informazioni vedere la [rimuovere i server](remove-servers.md) argomento.

## <a name="see-also"></a>Vedere anche

- [Panoramica di spazi diretti di archiviazione](storage-spaces-direct-overview.md)
- [Comprendere la tolleranza di errore in spazi di archiviazione diretta](storage-spaces-fault-tolerance.md)
- [Pianificare i volumi in spazi di archiviazione diretta](plan-volumes.md)
- [Creare i volumi in spazi di archiviazione diretta](create-volumes.md)
