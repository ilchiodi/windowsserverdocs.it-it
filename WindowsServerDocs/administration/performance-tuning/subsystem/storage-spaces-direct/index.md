---
title: Ottimizzazione delle prestazioni per Spazi di archiviazione diretta
description: In questo argomento viene descritto come Spazi di archiviazione diretta ottimizza automaticamente le prestazioni di base alla configurazione della cache dei componenti hardware in uso.
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.assetid: 15a519fa-37cc-4d84-a9fe-097d33bb71ea
author: phstee
ms.author: vshankar; danlo; clausjor; stevenek
ms.date: 4/14/2017
ms.openlocfilehash: a24bbdb83ec1b08f56989368a4831549c594f6c0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851604"
---
# <a name="performance-tuning-for-storage-spaces-direct"></a>Ottimizzazione delle prestazioni per Spazi di archiviazione diretta

Spazi di archiviazione diretta, una soluzione di archiviazione software-defined basata su Windows Server, ottimizza automaticamente le prestazioni evitando di dover specificare manualmente i conteggi delle colonne, la configurazione della cache dell'hardware in uso e altri fattori che devono essere impostati manualmente con le soluzioni di archiviazione SAS condivise. Per informazioni generali, vedere [Spazi di archiviazione diretta in Windows Server 2016](../../../../storage/storage-spaces/storage-spaces-direct-overview.md).

La cache del bus di archiviazione software di Spazi di archiviazione diretta viene configurata automaticamente in base ai tipi di risorse di archiviazione presenti nel sistema. Vengono riconosciuti tre tipi: **HDD**, **SSD** e **NVMe**. La cache richiede le risorse di archiviazione più veloci per la memorizzazione dei dati in lettura e/o scrittura e usa quelle più lente per l'archiviazione permanente dei dati.

La tabella seguente presenta un riepilogo delle configurazioni predefinite:

| Tipi di archiviazione | Configurazione della cache |
| --- | --- |
| Qualsiasi tipo singolo | Se è presente un solo tipo di risorsa di archiviazione, la cache del bus di archiviazione software non viene configurata. |
| SSD+HDD o NVMe+HDD | Per il livello della cache vengono configurate le risorse di archiviazione più veloci e la cache memorizza sia le operazioni di lettura sia quelle di scrittura. |
| SSD+SSD o NVMe+NVMe | Queste opzioni veloce+veloce sono destinate a combinazioni di risorse di archiviazione con maggiore e minore resistenza, ad esempio 10 unità SSD flash NAND DWPD (Drive Writes Per Day) per la cache e 1,5 unità SSD flash NAND DWPD per la capacità. Vengono abilitate assegnando a Spazi di archiviazione diretta un set di stringhe modello con cui identificare i dispositivi per la cache. Per altre informazioni, vedere la documentazione di riferimento sul cmdlet [Enable-StorageSpacesDirect](https://technet.microsoft.com/library/mt589697.aspx) (`CacheDeviceModel`). <br><br>In un sistema veloce+veloce, nella cache vengono memorizzate solo le operazioni di scrittura, mentre quelle di lettura non vengono memorizzate. |

Si noti che, su un dispositivo SSD o NVMe, per impostazione predefinita nella cache vengono memorizzate solo le operazioni di scrittura. L'idea alla base di questo comportamento è che, considerata la velocità del dispositivo per la capacità, è poco conveniente spostare contenuto in lettura nei dispositivi per la cache. In alcuni casi questa impostazione può non risultare valida, ma è opportuno fare attenzione perché l'abilitazione della cache di lettura può comportare un inutile utilizzo del dispositivo per la cache senza offrire alcun miglioramento delle prestazioni. Ecco alcuni esempi:

* **NVme+SSD** L'abilitazione della cache di lettura consente alle operazioni di IO in lettura di sfruttare la connettività PCIe e/o le maggiori prestazioni di IOPS dei dispositivi NVMe rispetto all'unità SSD aggregata. <br>Ciò _può_ essere vero per gli scenari orientati alla larghezza di banda a causa delle relative capacità di larghezza di banda dei dispositivi NVMe rispetto alla connessione HBA all'unità SSD. _Può invece non_ essere vero per gli scenari orientati alle operazioni di IOPS in cui i costi della CPU per tali operazioni possono limitare i sistemi prima ancora di riuscire a ottenere un miglioramento delle prestazioni.
* **NVMe+NVMe** Analogamente, se la capacità di lettura del dispositivo NVMe per la cache è superiore a quella del dispositivo NVMe combinato per la capacità, può essere conveniente abilitare la cache di lettura. <br>Sono tuttavia rari i casi in cui l'uso della cache di lettura risulta conveniente in configurazioni di questo tipo.

Per visualizzare e modificare la configurazione della cache, usare i cmdlet [Get-ClusterStorageSpacesDirect](https://technet.microsoft.com/library/mt634616.aspx) e [Set-ClusterStorageSpacesDirect](https://technet.microsoft.com/library/mt763265.aspx). Le proprietà `CacheModeHDD` e `CacheModeSSD` definiscono la modalità di funzionamento della cache su supporti per la capacità del tipo indicato.

## <a name="see-also"></a>Vedere anche

- [Informazioni su Spazi di archiviazione diretta](../../../../storage/storage-spaces/understand-storage-spaces-direct.md)
- [Pianificazione di Spazi di archiviazione diretta](../../../../storage/storage-spaces/plan-storage-spaces-direct.md)
- [Ottimizzazione delle prestazioni per file server](../../role/file-server/index.md)
- [Guida alle considerazioni di progettazione dell'archiviazione software-defined](https://technet.microsoft.com/library/mt243829.aspx) (per Windows Server 2012 R2 e archiviazione SAS condivisa)
