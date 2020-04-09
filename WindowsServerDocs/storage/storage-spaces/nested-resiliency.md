---
title: Resilienza annidata per Spazi di archiviazione diretta
ms.prod: windows-server
ms.author: jgerend
manager: dansimp
ms.technology: storagespaces
ms.topic: article
author: cosmosdarwin
ms.date: 03/15/2019
ms.openlocfilehash: ac4edccf0c1f8882dd2544b2544c3d8555bbc716
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857344"
---
# <a name="nested-resiliency-for-storage-spaces-direct"></a>Resilienza annidata per Spazi di archiviazione diretta

> Si applica a: Windows Server 2019

La resilienza annidata è una nuova funzionalità di [spazi di archiviazione diretta](storage-spaces-direct-overview.md) in Windows Server 2019 che consente a un cluster a due server di resistere a più errori hardware contemporaneamente senza perdita di disponibilità di spazio di archiviazione, in modo che gli utenti, le app e le macchine virtuali continuino a funzionare senza interruzioni. In questo argomento viene illustrato il funzionamento, vengono fornite istruzioni dettagliate per iniziare e vengono fornite le risposte alle domande più frequenti.

## <a name="prerequisites"></a>Prerequisiti

### <a name="green-checkmark-icon-consider-nested-resiliency-if"></a>![Icona segno di spunta verde.](media/nested-resiliency/supported.png) Prendere in considerazione la resilienza annidata se:

- Il cluster esegue Windows Server 2019; e
- Il cluster ha esattamente 2 nodi server

### <a name="red-x-icon-you-cant-use-nested-resiliency-if"></a>![Icona X rossa.](media/nested-resiliency/unsupported.png) Non è possibile usare la resilienza annidata se:

- Il cluster esegue Windows Server 2016; o
- Il cluster contiene 3 o più nodi server

## <a name="why-nested-resiliency"></a>Vantaggi della resilienza annidata

I volumi che usano la resilienza annidata possono **rimanere online e accessibili anche se si verificano contemporaneamente più errori hardware, a differenza della**resilienza del [mirroring a due vie](storage-spaces-fault-tolerance.md) classica. Se, ad esempio, due unità hanno esito negativo contemporaneamente o se un server si interrompe e si verifica un errore in un'unità, i volumi che usano la resilienza annidata resteranno online e accessibili. Per l'infrastruttura iperconvergente, questo aumenta il tempo di esecuzione per le app e le macchine virtuali; per i carichi di lavoro file server, ciò significa che gli utenti possono accedere senza interruzioni ai propri file.

![Disponibilità archiviazione](media/nested-resiliency/storage-availability.png)

Il compromesso è che la resilienza annidata presenta una **minore efficienza di capacità rispetto al mirroring a due vie classico**, ovvero si ottiene uno spazio leggermente meno utilizzabile. Per informazioni dettagliate, vedere la sezione relativa all' [efficienza della capacità](#capacity-efficiency) riportata di seguito.

## <a name="how-it-works"></a>Come funziona

### <a name="inspiration-raid-51"></a>Ispirazione: RAID 5 + 1

RAID 5 + 1 è una forma stabilita di resilienza dell'archiviazione distribuita che fornisce informazioni di base utili per comprendere la resilienza annidata. In RAID 5 + 1, all'interno di ogni server, la resilienza locale viene fornita da RAID-5, o una *singola parità*, per proteggersi dalla perdita di una singola unità. Una maggiore resilienza viene quindi garantita dal mirroring RAID-1 o *bidirezionale*tra i due server per proteggersi dalla perdita di uno dei server.

![RAID 5 + 1](media/nested-resiliency/raid-51.png)

### <a name="two-new-resiliency-options"></a>Due nuove opzioni di resilienza

Spazi di archiviazione diretta in Windows Server 2019 offre due nuove opzioni di resilienza implementate nel software, senza la necessità di hardware RAID specializzato:

- **Mirroring a due vie annidato.** All'interno di ogni server, la resilienza locale viene fornita dal mirroring a due vie, quindi l'ulteriore resilienza viene garantita dal mirroring a due vie tra i due server. Si tratta essenzialmente di un mirroring a quattro vie, con due copie in ogni server. Il mirroring a due vie annidato offre prestazioni senza compromessi: le Scritture passano a tutte le copie e le letture provengono da qualsiasi copia.

  ![Mirroring a due vie annidato](media/nested-resiliency/nested-two-way-mirror.png)

- **Parità con accelerazione speculare nidificata.** Combinare il mirroring bidirezionale annidato, precedente, con parità nidificata. All'interno di ogni server, la resilienza locale per la maggior parte dei dati viene fornita da un'unica [aritmetica con parità bit per bit](storage-spaces-fault-tolerance.md#parity), eccetto le nuove scritture recenti che usano il mirroring bidirezionale. Quindi, l'ulteriore resilienza per tutti i dati viene fornita dal mirroring a due vie tra i server. Per altre informazioni sul funzionamento della parità con accelerazione con mirroring, vedere [parità con accelerazione con mirroring](https://docs.microsoft.com/windows-server/storage/refs/mirror-accelerated-parity).

  ![Parità con accelerazione speculare nidificata](media/nested-resiliency/nested-mirror-accelerated-parity.png)

### <a name="capacity-efficiency"></a>Efficienza della capacità

L'efficienza della capacità è il rapporto tra spazio utilizzabile e [ingombro del volume](plan-volumes.md#choosing-the-size-of-volumes). Descrive il sovraccarico della capacità attribuibile alla resilienza e dipende dall'opzione di resilienza scelta. Come esempio semplice, l'archiviazione dei dati senza resilienza è efficiente al 100% (1 TB di dati occupa 1 TB di capacità di archiviazione fisica), mentre il mirroring a due vie è efficiente al 50% (1 TB di dati occupano 2 TB di capacità di archiviazione fisica).

- Il **mirroring a due vie annidato** scrive quattro copie di tutto, vale a dire per archiviare 1 TB di dati, sono necessari 4 TB di capacità di archiviazione fisica. Sebbene la semplicità sia interessante, l'efficienza della capacità del mirroring bidirezionale annidato del 25% è il livello più basso di qualsiasi opzione di resilienza in Spazi di archiviazione diretta.

- La **parità con accelerazione con mirroring annidato** raggiunge una maggiore efficienza della capacità, circa il 35%-40%, che dipende da due fattori: il numero di unità di capacità in ogni server e la combinazione di mirror e parità specificati per il volume. Questa tabella fornisce una ricerca per le configurazioni comuni:

  | Unità di capacità per server | mirror del 10% | mirroring del 20% | mirroring del 30% |
  |----------------------------|------------|------------|------------|
  | 4                          | 35,7%      | 34,1%      | 32,6%      |
  | 5                          | 37,7%      | 35,7%      | 33,9%      |
  | 6                          | 39,1%      | 36,8%      | 34,7%      |
  | 7+                         | 40,0%      | 37,5%      | 35,3%      |

  > [!NOTE]
  > **Se si è curiosi, di seguito è riportato un esempio di calcolo completo.** Si supponga di avere sei unità di capacità in ognuno dei due server e di creare un volume di 1 100 GB costituito da 10 GB di mirror e 90 GB di parità. Il mirroring a due vie locale del server è efficiente al 50,0%, ovvero i 10 GB di dati mirror richiedono 20 GB per l'archiviazione in ogni server. Con mirroring in entrambi i server, il footprint totale è di 40 GB. La parità locale del server, in questo caso, è 5/6 = 83,3% efficiente, ovvero i 90 GB di dati di parità sono necessari 108 GB per l'archiviazione in ogni server. Con mirroring in entrambi i server, il footprint totale è di 216 GB. Il footprint totale è quindi [(10 GB/50,0%) + (90 GB/83,3%)] × 2 = 256 GB, per l'efficienza complessiva della capacità del 39,1%.

Si noti che l'efficienza della capacità del mirroring a due vie classico (circa il 50%) e parità con accelerazione speculare nidificata (fino al 40%) non sono molto diversi. A seconda dei requisiti, è possibile che l'efficienza di capacità leggermente inferiore richieda un aumento significativo della disponibilità dell'archiviazione. Si sceglie la resilienza per volume, pertanto è possibile combinare volumi di resilienza annidati e volumi con mirroring a due vie classici nello stesso cluster.

![Compromesso](media/nested-resiliency/tradeoff.png)

## <a name="usage-in-powershell"></a>Utilizzo in PowerShell

È possibile usare cmdlet di archiviazione familiari in PowerShell per creare volumi con resilienza annidata.

### <a name="step-1-create-storage-tier-templates"></a>Passaggio 1: creare modelli del livello di archiviazione

Per prima cosa, creare nuovi modelli del livello di archiviazione usando il cmdlet `New-StorageTier`. È sufficiente eseguire questa operazione una sola volta e quindi ogni nuovo volume creato può fare riferimento a questi modelli. Specificare il `-MediaType` delle unità di capacità e, facoltativamente, il `-FriendlyName` di propria scelta. Non modificare gli altri parametri.

Se le unità di capacità sono unità disco rigido (HDD), avviare PowerShell come amministratore ed eseguire:

```PowerShell 
# For mirror
New-StorageTier -StoragePoolFriendlyName S2D* -FriendlyName NestedMirror -ResiliencySettingName Mirror -MediaType HDD -NumberOfDataCopies 4

# For parity
New-StorageTier -StoragePoolFriendlyName S2D* -FriendlyName NestedParity -ResiliencySettingName Parity -MediaType HDD -NumberOfDataCopies 2 -PhysicalDiskRedundancy 1 -NumberOfGroups 1 -FaultDomainAwareness StorageScaleUnit -ColumnIsolation PhysicalDisk 
``` 

Se le unità di capacità sono unità SSD, impostare invece il `-MediaType` su `SSD`. Non modificare gli altri parametri.

> [!TIP]
> Verificare che i livelli creati correttamente con `Get-StorageTier`.

### <a name="step-2-create-volumes"></a>Passaggio 2: creare volumi

Quindi, creare nuovi volumi utilizzando il cmdlet `New-Volume`.

#### <a name="nested-two-way-mirror"></a>Mirroring a due vie annidato

Per usare il mirroring a due vie annidato, fare riferimento al modello di livello `NestedMirror` e specificare le dimensioni. Ad esempio,

```PowerShell
New-Volume -StoragePoolFriendlyName S2D* -FriendlyName Volume01 -StorageTierFriendlyNames NestedMirror -StorageTierSizes 500GB
```

#### <a name="nested-mirror-accelerated-parity"></a>Parità con accelerazione speculare nidificata

Per utilizzare la parità con accelerazione speculare nidificata, fare riferimento ai modelli di livello `NestedMirror` e `NestedParity` e specificare due dimensioni, una per ogni parte del volume (mirror First, parità Second). Ad esempio, per creare un volume di 1 500 GB che è il 20% di mirroring a due vie annidato e il 80% di parità annidata, eseguire:

```PowerShell
New-Volume -StoragePoolFriendlyName S2D* -FriendlyName Volume02 -StorageTierFriendlyNames NestedMirror, NestedParity -StorageTierSizes 100GB, 400GB
```

### <a name="step-3-continue-in-windows-admin-center"></a>Passaggio 3: continuare nell'interfaccia di amministrazione di Windows

I volumi che usano la resilienza annidata vengono visualizzati nell'interfaccia di [amministrazione di Windows](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center) con l'etichettatura chiara, come nella schermata seguente. Una volta creati, è possibile gestirli e monitorarli usando l'interfaccia di amministrazione di Windows proprio come qualsiasi altro volume in Spazi di archiviazione diretta.

![](media/nested-resiliency/windows-admin-center.png)

### <a name="optional-extend-to-cache-drives"></a>Facoltativo: Estendi alle unità della cache

Con le relative impostazioni predefinite, la resilienza annidata protegge dalla perdita di più unità di capacità contemporaneamente o da un server e da un'unità di capacità nello stesso momento. Per estendere questa protezione alle [unità della cache](understand-the-cache.md) , è necessario prendere in considerazione altre considerazioni: poiché le unità della cache forniscono spesso la memorizzazione nella cache di lettura *e scrittura* per *più* unità di capacità, l'unico modo per garantire che sia possibile tollerare la perdita di un'unità cache quando l'altro server è inattivo è semplicemente non scrivere nella cache, ma ciò influisca sulle prestazioni.

Per risolvere questo scenario, Spazi di archiviazione diretta offre la possibilità di disabilitare automaticamente la memorizzazione nella cache in scrittura quando un server in un cluster a due server è inattivo e quindi riabilitare la memorizzazione nella cache in scrittura dopo il backup del server. Per consentire i riavvii di routine senza effetti sulle prestazioni, la memorizzazione nella cache in scrittura non viene disabilitata fino a quando il server non si arresta per 30 minuti. Quando la memorizzazione nella cache di scrittura è disabilitata, il contenuto della cache di scrittura viene scritto nei dispositivi a capacità. Al termine di questa operazione, il server può tollerare un dispositivo di cache non riuscita nel server online, anche se le letture della cache potrebbero essere ritardate o non riuscite in caso di errore di un dispositivo di cache.

Per impostare questo comportamento (facoltativo), avviare PowerShell come amministratore ed eseguire:

```PowerShell
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.NestedResiliency.DisableWriteCacheOnNodeDown.Enabled" -Value "True"
```

Una volta impostato su **true**, il comportamento della cache è il seguente:

| Situazione                       | Comportamento della cache                           | Può tollerare la perdita di unità cache? |
|---------------------------------|------------------------------------------|--------------------------------|
| Entrambi i server                 | Letture e scritture nella cache, prestazioni complete | Sì                            |
| Server inattivo, primi 30 minuti   | Letture e scritture nella cache, prestazioni complete | No (temporaneamente)               |
| Dopo i primi 30 minuti          | Solo letture cache, conseguenze sulle prestazioni   | Sì (dopo che la cache è stata scritta in unità di capacità)                           |

## <a name="frequently-asked-questions"></a>Domande frequenti

### <a name="can-i-convert-an-existing-volume-between-two-way-mirror-and-nested-resiliency"></a>È possibile convertire un volume esistente tra il mirroring a due vie e la resilienza annidata?

No, non è possibile convertire i volumi tra tipi di resilienza. Per le nuove distribuzioni in Windows Server 2019, decidere in anticipo quale tipo di resilienza è più adatto alle proprie esigenze. Se si sta eseguendo l'aggiornamento da Windows Server 2016, è possibile creare nuovi volumi con resilienza annidata, migrare i dati e quindi eliminare i volumi meno recenti.

### <a name="can-i-use-nested-resiliency-with-multiple-types-of-capacity-drives"></a>È possibile usare la resilienza annidata con più tipi di unità di capacità?

Sì, è sufficiente specificare il `-MediaType` di ogni livello di conseguenza nel [passaggio 1](#step-1-create-storage-tier-templates) precedente. Ad esempio, con NVMe, SSD e HDD nello stesso cluster, il NVMe fornisce la cache mentre le ultime due forniscono la capacità: impostare il livello di `NestedMirror` su `-MediaType SSD` e il livello di `NestedParity` su `-MediaType HDD`. In questo caso, si noti che l'efficienza della capacità di parità dipende solo dal numero di unità HDD ed è necessario almeno 4 per ogni server.

### <a name="can-i-use-nested-resiliency-with-3-or-more-servers"></a>È possibile utilizzare la resilienza annidata con tre o più server?

No, usare solo la resilienza annidata se il cluster ha esattamente 2 server.

### <a name="how-many-drives-do-i-need-to-use-nested-resiliency"></a>Quante unità sono necessarie per usare la resilienza annidata?

Il numero minimo di unità necessarie per Spazi di archiviazione diretta è 4 unità di capacità per nodo server, più 2 unità cache per ogni nodo server. Questa operazione è invariata rispetto a Windows Server 2016. Non sono previsti requisiti aggiuntivi per la resilienza annidata e la raccomandazione per la capacità di riserva è invariata.

### <a name="does-nested-resiliency-change-how-drive-replacement-works"></a>La resilienza annidata cambia in che modo funziona la sostituzione delle unità?

No.

### <a name="does-nested-resiliency-change-how-server-node-replacement-works"></a>La resilienza annidata cambia in che modo funziona la sostituzione del nodo server?

No. Per sostituire un nodo del server e le relative unità, seguire questo ordine:

1. Ritirare le unità nel server in uscita
2. Aggiungere il nuovo server con le relative unità al cluster
3. Il pool di archiviazione ribilanciarà
4. Rimuovere il server in uscita e le relative unità

Per informazioni dettagliate, vedere l'argomento [rimuovere server](remove-servers.md) .

## <a name="see-also"></a>Vedere anche

- [Panoramica di Spazi di archiviazione diretta](storage-spaces-direct-overview.md)
- [Informazioni sulla tolleranza di errore in Spazi di archiviazione diretta](storage-spaces-fault-tolerance.md)
- [Pianificare i volumi in Spazi di archiviazione diretta](plan-volumes.md)
- [Creazione di volumi in Spazi di archiviazione diretta](create-volumes.md)
