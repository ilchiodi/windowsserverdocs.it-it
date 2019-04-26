---
title: Considerazioni relative all'alimentazione Hardware server
description: Considerazioni relative all'alimentazione Hardware server
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Qizha;TristanB
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 5261c856a0a29f9f58526e4f9580a16bbed5be56
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874302"
---
# <a name="server-hardware-power-considerations"></a>Considerazioni relative all'alimentazione Hardware server

È importante riconoscere l'importanza crescente del risparmio energetico in ambienti di centri di azienda e i dati. Low energy utilizzo e prestazioni elevate sono spesso obiettivi in conflitto, ma con attenzione la selezione di componenti server, è possibile ottenere il giusto equilibrio tra di essi. Le sezioni seguenti sono elencate le linee guida per le caratteristiche di potenza e le funzionalità dei componenti hardware server.

## <a name="processor-recommendations"></a>Consigli di processore

Frequenza, operativo tensione, le dimensioni della cache e la tecnologia di processo influisce sul consumo di energia dei processori. I processori hanno una progettazione termica punto rating (TDP) che offre un'indicazione di base del consumo di energia relativo altri modelli.

In generale, optare per il processore TDP più basso in grado di soddisfare gli obiettivi di prestazioni. Inoltre, più recente generazioni di processori sono in genere più energia efficiente e potrebbero esporre più stati di alimentazione per gli algoritmi di gestione dell'alimentazione di Windows, che consente una migliore gestione dell'alimentazione a tutti i livelli di prestazioni. O l'utilizzo di alcune delle nuove "cooperativo? tecniche di gestione di potenza che Microsoft ha sviluppato in collaborazione con i produttori di hardware.

Per altre informazioni sulle tecniche di gestione power cooperativo, vedere la sezione denominata controllo di prestazioni del processore collaborativo nel [Advanced Configuration and Power Interface Specification](http://www.uefi.org/sites/default/files/resources/ACPI_5_1release.pdf).


## <a name="memory-recommendations"></a>Consigli sulla memoria
Account di memoria per una frazione incremento della potenza totale del sistema. Il consumo di energia di un DIMM, di memoria come tecnologia della memoria, codice di correzione dell'errore (ECC), la frequenza del bus, capacità, densità e numero di ranghi influenzata da molti fattori. Pertanto, è consigliabile confrontare le classificazioni di alimentazione prevista prima dell'acquisto di grandi quantità di memoria.

A questo punto è disponibile memoria basso consumo, ma è necessario considerare i vantaggi e svantaggi delle prestazioni e costo. Se verrà paging del server, è necessario anche tenere in considerazione il costo di energia dei dischi il paging.


## <a name="disks-recommendations"></a>Consigli di dischi
RPM superiore indica che il consumo di energia maggiore. Nelle unità SSD sono più efficiente rispetto alle unità rotazionali. Inoltre, le unità da 2,5 richiedono in genere meno energia rispetto alle unità da 3,5 pollici.

## <a name="network-and-storage-adapter-recommendations"></a>Indicazioni sull'adattatore di archiviazione e rete
Alcune schede di ridurre il consumo di energia durante i periodi di inattività. Si tratta di una considerazione importante per schede di rete da 10 Gb e i collegamenti di archiviazione (4 a 8 Gb) di larghezza di banda elevata. Tale dispositivo può utilizzare grandi quantità di energia.


## <a name="power-supply-recommendations"></a>Implementare le raccomandazioni di alimentazione
Migliorare l'efficienza energetica della fornitura è un ottimo modo per ridurre i consumi energetici senza influire sulle prestazioni. Alimentatori ad alta efficienza consente di risparmiare molte kilowatt-hours all'anno, per ogni server.


## <a name="fan-recommendations"></a>Ventola raccomandazioni
Ventole, ad esempio alimentatori, sono un'area in cui è possibile ridurre i consumi energetici senza influire sulle prestazioni del sistema. Fan di velocità variabile può ridurre RPM come sistema carico diminuisce, eliminando il consumo energetico in caso contrario, non necessari.


## <a name="usb-devices-recommendations"></a>Dispositivi USB raccomandazioni
Windows Server 2016 Attiva sospensione selettiva. per i dispositivi USB per impostazione predefinita. Tuttavia, un driver di dispositivo scritta male può compromettere comunque l'efficienza energetica del sistema con un margine sizeable. Per evitare potenziali problemi, disconnettere i dispositivi USB, disabilitarli nel BIOS o selezione dei server che non richiedono i dispositivi USB.


## <a name="remotely-managed-power-strip-recommendations"></a>Raccomandazioni striscia Power gestito in remoto
Alimentazione non sono parte integrante dell'hardware del server, ma è possibile determinare una notevole differenza nel data center. Le misurazioni mostrano che i server con volumi che sono collegati, ma sono stati apparentemente spenta, comunque potrebbero richiedere fino a 30 watt di alimentazione.

Per evitare inutile consumo di energia elettrica, è possibile distribuire una striscia power gestito in remoto per ogni rack di server a livello di programmazione disconnettere power da server specifici.

## <a name="processor-terminology"></a>Terminologia del processore
La terminologia di processore usata in questo argomento rifletta la gerarchia di componenti disponibili nella figura seguente. Di seguito sono elencati i termini usati dal più grande al più piccolo granularità dei componenti:

-   Socket del processore
-   Nodo NUMA
-   Core
-   processore logico

![terminologia del processore](../media/perftune-guide-figure-1.png)

## <a name="see-also"></a>Vedere anche
- [Considerazioni sulle prestazioni dell'Hardware di server](index.md)
- [Risparmio energia e ottimizzazione delle prestazioni](power/power-performance-tuning.md)
- [Processore Power Management di ottimizzazione](power/processor-power-management-tuning.md)
- [Parametri bilanciato piano consigliato](power/recommended-balanced-plan-parameters.md)
