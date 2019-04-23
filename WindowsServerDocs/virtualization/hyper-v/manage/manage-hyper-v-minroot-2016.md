---
title: Minroot
description: Configurazione dei controlli delle risorse della CPU Host
keywords: windows 10, hyper-v
author: allenma
ms.date: 12/15/2017
ms.topic: article
ms.prod: windows-10-hyperv
ms.service: windows-10-hyperv
ms.assetid: ''
ms.openlocfilehash: e1269c11df32c8ce95cc7455d47d7170e9d0b1c8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844332"
---
# <a name="hyper-v-host-cpu-resource-management"></a>Gestione delle risorse CPU Host Hyper-V

I controlli delle risorse CPU host Hyper-V introdotti in Windows Server 2016 o versioni successive consentono agli amministratori di Hyper-V gestire meglio e allocare le risorse della CPU tra "root", o partizione di gestione e le macchine virtuali guest server host. Usa questi controlli, gli amministratori possono dedicare un subset dei processori di un sistema host nella partizione radice. Ciò può isolare il lavoro svolto in un host Hyper-V dai carichi di lavoro in esecuzione nelle macchine virtuali guest eseguendoli in base a subset dei processori di sistema separate.

Per informazioni dettagliate sull'hardware per gli host Hyper-V, vedere [requisiti di sistema di Windows 10 Hyper-V](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/hyper-v-requirements).

## <a name="background"></a>Informazioni

Prima di impostazione controlla per Hyper-V ospita le risorse di CPU, è utile esaminare le nozioni di base dell'architettura di Hyper-V.  
È possibile trovare un riepilogo generale nella console di [architettura di Hyper-V](https://docs.microsoft.com/windows-server/administration/performance-tuning/role/hyper-v-server/architecture) sezione.
Si tratta di concetti importanti di questo articolo:

* Hyper-V crea e gestisce le partizioni di macchina virtuale, tra cui calcolo delle risorse sono allocate e condivisa, sotto il controllo di hypervisor.  Le partizioni forniscono i limiti di isolamento forte tra tutte le macchine virtuali guest e tra le macchine virtuali guest e nella partizione radice.

* La partizione radice è a sua volta una partizione di macchina virtuale, anche se presenta proprietà univoche e molto maggiori privilegi rispetto alle macchine virtuali guest.  La partizione radice fornisce i servizi di gestione che consentono di controllare tutte le macchine virtuali guest, fornisce il supporto di dispositivi virtuali per i guest e gestisce tutti i dispositivi dei / o per le macchine virtuali guest.  Microsoft consiglia vivamente di una partizione host non è in esecuzione carichi di lavoro dell'applicazione.

* Ogni processore virtuale (VP) della partizione radice è 1:1 con mapping a un processore logico sottostante (LP).  Un host VP verrà sempre eseguito sulla stessa LP sottostante, non vi è alcuna migrazione del VPs della partizione radice.  

* Per impostazione predefinita, il LPs su cui eseguire VPs host possono essere eseguiti anche VPs guest.

* Vice presidente del settore un guest possono essere pianificati dall'hypervisor per l'esecuzione su tutti i processori logici disponibili.  Mentre l'utilità di pianificazione di hypervisor gestisce da prendere in considerazione la località della cache temporali, la topologia NUMA e molti altri fattori quando si pianifica un guest Vice presidente del settore, in definitiva la VP è stato possibile pianificare in qualsiasi host LP.

## <a name="the-minimum-root-or-minroot-configuration"></a>La radice minima, o "Minroot" configurazione

Nelle versioni precedenti di Hyper-V ha un limite massimo dell'architettura di 64 VPs per partizione.  Ciò applicato alle partizioni root e guest.  Come è stata visualizzata sistemi con più di 64 processori logici nel server di fascia alta, Hyper-V evoluto relativi limiti di scalabilità di host per supportare questi sistemi più grandi, a un certo punto un host con un massimo di 320 LPs di supporto.  Tuttavia, rilievo 64 presentato numerose sfide e introdotte delle complessità che ha effettuato che supportano più di 64 VPs per ogni partizione proibitivi Vice presidente del settore per ogni limite per le partizioni in quel momento.  Per risolvere questo problema, Hyper-V di un numero limitato di VPs assegnato a una partizione radice a 64, anche se il computer sottostante ha numero di processori logici disponibili.  L'hypervisor potrebbe continuare a usare tutti LPs disponibili per l'esecuzione di VPs guest, tuttavia limitati artificialmente la partizione radice a 64.  Questa configurazione è diventato noto come la "radice minimo" o "minroot" configuration.  Test delle prestazioni confermato che, anche nei sistemi su larga scala con più di 64 LPs radice non sono necessarie più di 64 VPs radice per fornire supporto sufficiente a un numero elevato di macchine virtuali guest e VPs guest, in realtà, molto meno di 64 radice VPs spesso era adeguata , a seconda naturalmente il numero e dimensioni delle macchine virtuali guest, le specifiche dei carichi di lavoro in esecuzione e così via.

Questo concetto di "minroot" continua a essere utilizzato oggi stesso.  In effetti, anche se Windows Server 2016 Hyper-V aumentato il limite massimo supporto per l'architettura per host LPs 512 LPs, la partizione radice continuerà a essere limitata a un massimo di 320 LPs.

## <a name="using-minroot-to-constrain-and-isolate-host-compute-resources"></a>Uso di Minroot per vincolare e isolare le risorse di calcolo di Host
Con la soglia predefinita elevata di 320 LPs in Windows Server 2016 Hyper-V, la configurazione minroot verrà utilizzata solo nei sistemi server molto più grandi.  Tuttavia, questa funzionalità può essere configurata per una soglia molto inferiore dall'amministratore di host Hyper-V e pertanto sfruttata per limitare notevolmente la quantità di risorse CPU host disponibile nella partizione radice.  Il numero specifico di radice LPs utilizzare naturalmente deve essere scelta attentamente per soddisfare le esigenze di numero massime di macchine virtuali e i carichi di lavoro allocati all'host.  Valori accettabili per il numero di host LPs, tuttavia, possono essere determinato mediante un'attenta valutazione e monitoraggio di carichi di lavoro di produzione e convalidato in ambienti non di produzione prima di distribuzioni di grandi dimensioni.

## <a name="enabling-and-configuring-minroot"></a>Abilitazione e configurazione Minroot

La configurazione minroot è controllata tramite le voci di BCD hypervisor. Per abilitare minroot, da un prompt dei comandi con privilegi di amministratore:

```
    bcdedit /set hypervisorrootproc n
```
Dove n è il numero di radice VPs. 

È necessario riavviare il sistema e il nuovo numero di processori di primo livello verrà mantenuti per la durata dell'avvio del sistema operativo.  La configurazione minroot non può essere modificata in modo dinamico in fase di esecuzione.

Se sono presenti più nodi NUMA, ogni nodo verrà visualizzato `n/NumaNodeCount` processori.

Si noti che con più nodi NUMA, è necessario verificare che la topologia della macchina virtuale è tale che esistono sufficienti LPs gratuito (ovvero LPs senza radice VPs) in ogni nodo NUMA per nodo NUMA della macchina virtuale corrispondente VPs di esecuzione.

## <a name="verifying-the-minroot-configuration"></a>Verifica della configurazione Minroot

È possibile verificare la configurazione dell'host minroot tramite Gestione attività, come illustrato di seguito.

![](./media/minroot-taskman.png)

Quando Minroot è attivo, Gestione attività visualizzerà il numero di processori logici attualmente allocato all'host, oltre al numero totale di processori logici nel sistema.
 
