---
title: Considerazioni sull'alimentazione hardware del server
description: Considerazioni sull'alimentazione hardware del server
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Qizha;TristanB
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: a9d4653824d497ea0c42337260aa788bab354ba3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355020"
---
# <a name="server-hardware-power-considerations"></a>Considerazioni sull'alimentazione hardware del server

È importante riconoscere la crescente importanza dell'efficienza energetica negli ambienti aziendali e data center. Le prestazioni elevate e l'utilizzo ridotto dell'energia sono spesso obiettivi in conflitto, ma selezionando con attenzione i componenti server è possibile raggiungere il giusto equilibrio tra di essi. Le sezioni seguenti elencano le linee guida per le caratteristiche e le funzionalità di alimentazione dei componenti hardware del server.

## <a name="processor-recommendations"></a>Consigli per i processori

Frequenza, tensione operativa, dimensioni della cache e tecnologia di elaborazione influiscono sul consumo di energia dei processori. I processori hanno una classificazione di punti di progettazione termica (TDP) che offre un'indicazione di base del consumo di energia rispetto ad altri modelli.

In generale, optare per il processore TDP più basso che soddisferà gli obiettivi di prestazioni. Inoltre, le generazioni più recenti dei processori sono in genere più efficienti a livello di energia e possono esporre più Stati di alimentazione per gli algoritmi di risparmio energia di Windows, consentendo una migliore gestione del risparmio energia a tutti i livelli di prestazioni. In alternativa, possono utilizzare alcune delle nuove tecniche "cooperative" di risparmio energia sviluppate da Microsoft in collaborazione con i produttori di hardware.

Per ulteriori informazioni sulle tecniche di gestione dell'alimentazione cooperativa, vedere la sezione relativa al controllo delle prestazioni del processore collaborativo nella [specifica Advanced Configuration and Power Interface](http://www.uefi.org/sites/default/files/resources/ACPI_5_1release.pdf).


## <a name="memory-recommendations"></a>Consigli sulla memoria
Account di memoria per una frazione crescente della potenza totale del sistema. Molti fattori influiscono sul consumo di energia di un DIMM di memoria, ad esempio la tecnologia di memoria, il codice di correzione degli errori (ECC), la frequenza del bus, la capacità, la densità e il numero di classificazioni. Pertanto, è preferibile confrontare le classificazioni di alimentazione previste prima di acquistare grandi quantità di memoria.

La memoria a basso consumo è ora disponibile, ma è necessario considerare i compromessi in termini di prestazioni e costi. Se il server esegue il paging, è necessario anche calcolare il costo energetico dei dischi di paging.


## <a name="disks-recommendations"></a>Suggerimenti sui dischi
Un RPM più elevato comporta un aumento del consumo di energia. Le unità SSD sono più efficienti rispetto alle unità rotazionali. Inoltre, le unità da 2,5 pollici necessitano in genere di meno potenza rispetto alle unità da 3,5 pollici.

## <a name="network-and-storage-adapter-recommendations"></a>Consigli per schede di rete e adattatori di archiviazione
Alcuni adapter diminuiscono il consumo di energia durante i periodi di inattività. Si tratta di una considerazione importante per le schede di rete da 10 GB e i collegamenti di archiviazione con larghezza di banda elevata (4-8 GB). Tali dispositivi possono utilizzare quantità di energia significative.


## <a name="power-supply-recommendations"></a>Raccomandazioni per l'alimentazione
Migliorare l'efficienza dell'alimentazione è un ottimo modo per ridurre il consumo di energia senza influire sulle prestazioni. Gli alimentatori ad alta efficienza possono risparmiare molte ore kilowatt all'anno, per server.


## <a name="fan-recommendations"></a>Consigli ventola
Gli appassionati, ad esempio gli alimentatori, sono un'area in cui è possibile ridurre il consumo di energia senza influire sulle prestazioni del sistema. I ventilatori a velocità variabile possono ridurre il RPM quando il carico del sistema diminuisce, eliminando il consumo di energia altrimenti superfluo.


## <a name="usb-devices-recommendations"></a>Suggerimenti sui dispositivi USB
Per impostazione predefinita, Windows Server 2016 Abilita la sospensione selettiva per i dispositivi USB. Tuttavia, un driver di dispositivo scritto male può comunque compromettere l'efficienza energetica del sistema con un margine notevole. Per evitare potenziali problemi, disconnettere i dispositivi USB, disabilitarli nel BIOS o scegliere server che non richiedono dispositivi USB.


## <a name="remotely-managed-power-strip-recommendations"></a>Consigli di Power Strip gestiti in remoto
Le strisce di alimentazione non sono parte integrante dell'hardware del server, ma possono fare una grande differenza nel data center. Le misurazioni mostrano che i server del volume collegati, ma che sono stati spenti, potrebbero comunque richiedere fino a 30 watt di potenza.

Per evitare di sprecare energia elettrica, è possibile distribuire un Power Strip gestito in remoto per ogni rack di server per disconnettere l'alimentazione a livello di codice da server specifici.

## <a name="processor-terminology"></a>Terminologia del processore
La terminologia del processore utilizzata in questo argomento riflette la gerarchia dei componenti disponibili nella figura seguente. I termini utilizzati dalla granularità più grande alla più piccola dei componenti sono i seguenti:

-   Socket processore
-   Nodo NUMA
-   Core
-   Processore logico

![terminologia del processore](../media/perftune-guide-figure-1.png)

## <a name="see-also"></a>Vedere anche
- [Considerazioni sulle prestazioni dell'hardware del server](index.md)
- [Risparmio energia e ottimizzazione delle prestazioni](power/power-performance-tuning.md)
- [Ottimizzazione di Risparmio energia del processore](power/processor-power-management-tuning.md)
- [Parametri della combinazione per il risparmio di energia Bilanciato](power/recommended-balanced-plan-parameters.md)
