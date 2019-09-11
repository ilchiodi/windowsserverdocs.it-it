---
title: Minroot
description: Configurazione di controlli risorse CPU host
keywords: windows 10, hyper-v
author: allenma
ms.date: 12/15/2017
ms.topic: article
ms.prod: windows-10-hyperv
ms.service: windows-10-hyperv
ms.assetid: ''
ms.openlocfilehash: 92de899a39aed05e2f598fcb3aae3fbae3f1cb67
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872045"
---
# <a name="hyper-v-host-cpu-resource-management"></a>Gestione delle risorse della CPU dell'host Hyper-V

I controlli delle risorse della CPU dell'host Hyper-V introdotti in Windows Server 2016 o versioni successive consentono agli amministratori di Hyper-V di gestire e allocare in modo migliore le risorse della CPU del server host tra la "radice" o la partizione di gestione e le macchine virtuali guest. Utilizzando questi controlli, gli amministratori possono dedicare un subset dei processori di un sistema host alla partizione radice. In questo modo è possibile separare il lavoro eseguito in un host Hyper-V dai carichi di lavoro in esecuzione nelle macchine virtuali guest eseguendoli in subset distinti dei processori di sistema.

Per informazioni dettagliate sull'hardware per gli host Hyper-V, vedere [requisiti di sistema di Hyper-v per Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/hyper-v-requirements).

## <a name="background"></a>Sfondo

Prima di impostare i controlli per le risorse della CPU dell'host Hyper-V, è utile esaminare le nozioni di base dell'architettura Hyper-V.  
È possibile trovare un riepilogo generale nella sezione relativa all' [architettura di Hyper-V](https://docs.microsoft.com/windows-server/administration/performance-tuning/role/hyper-v-server/architecture) .
Questi sono concetti importanti per questo articolo:

* Hyper-V crea e gestisce le partizioni delle macchine virtuali, in cui le risorse di calcolo vengono allocate e condivise, sotto il controllo dell'hypervisor.  Le partizioni forniscono limiti di isolamento forti tra tutte le macchine virtuali guest e tra le macchine virtuali guest e la partizione radice.

* La partizione radice è a sua volta una partizione di macchina virtuale, anche se dispone di proprietà univoche e privilegi molto maggiori rispetto alle macchine virtuali guest.  La partizione radice fornisce i servizi di gestione che controllano tutte le macchine virtuali guest, fornisce il supporto per i dispositivi virtuali per i guest e gestisce tutte le I/O dei dispositivi per le macchine virtuali guest.  Microsoft consiglia vivamente di non eseguire i carichi di lavoro di applicazioni in una partizione host.

* Ogni processore virtuale (VP) della partizione radice viene mappato a 1:1 a un processore logico sottostante (LP).  Un host VP verrà sempre eseguito sullo stesso LP sottostante. non viene eseguita alcuna migrazione del VPs della partizione radice.  

* Per impostazione predefinita, l'LP su cui è in esecuzione VPs host può eseguire anche il VPs Guest.

* Un VP Guest può essere pianificato dall'hypervisor per l'esecuzione in qualsiasi processore logico disponibile.  Mentre l'utilità di pianificazione dell'hypervisor prende in considerazione la località della cache temporale, la topologia NUMA e molti altri fattori durante la pianificazione di un VP Guest, infine il VP potrebbe essere pianificato in qualsiasi LP host.

## <a name="the-minimum-root-or-minroot-configuration"></a>La configurazione radice minima o "Minroot"

Nelle versioni precedenti di Hyper-V era previsto un limite massimo di architettura di 64 VPs per partizione.  Questa operazione viene applicata sia alla radice che alle partizioni guest.  Poiché i sistemi con più di 64 processori logici sono apparsi nei server di fascia alta, Hyper-V ha anche sviluppato i limiti di scalabilità dell'host per supportare questi sistemi più grandi, a un certo punto per supportare un host con un massimo di 320 LPs.  Tuttavia, il limite di 64 VP per partizione in quel momento presenta diverse difficoltà e introduce complessità che hanno consentito di supportare più di 64 VPs per partizione.  Per risolvere questo problema, Hyper-V limita il numero di VPs assegnato alla partizione radice a 64, anche se nel computer sottostante sono disponibili molti più processori logici.  L'hypervisor continuerà a usare tutti gli LPs disponibili per l'esecuzione del VPs Guest, ma ha artificialmente limitato la partizione radice a 64.  Questa configurazione era nota come configurazione "radice minima" o "minroot".  Il test delle prestazioni ha confermato che, anche su sistemi su larga scala con più di 64 LPs, la radice non ha bisogno di più di 64 VPs radice per fornire un supporto sufficiente a un numero elevato di macchine virtuali guest e VPs Guest. in realtà, molto meno di 64, il VPs radice era spesso adatto , a seconda del corso sul numero e sulle dimensioni delle VM guest, sui carichi di lavoro specifici eseguiti e così via.

Questo concetto "minroot" continua a essere utilizzato oggi.  Di fatto, anche se Windows Server 2016 Hyper-V ha aumentato il limite massimo di supporto per l'architettura per gli LPs degli host a 512 LPs, la partizione radice sarà ancora limitata a un massimo di 320 LPs.

## <a name="using-minroot-to-constrain-and-isolate-host-compute-resources"></a>Uso di Minroot per vincolare e isolare le risorse di calcolo host
Con la soglia predefinita elevata di 320 LPs in Windows Server 2016 Hyper-V, la configurazione di minroot verrà utilizzata solo nei sistemi server molto più grandi.  Tuttavia, questa funzionalità può essere configurata su una soglia molto inferiore da parte dell'amministratore host Hyper-V e pertanto utilizzata per limitare notevolmente la quantità di risorse della CPU dell'host disponibili per la partizione radice.  Il numero specifico di LP radice da usare deve essere ovviamente scelto con attenzione per supportare le richieste massime delle VM e dei carichi di lavoro allocati all'host.  Tuttavia, i valori ragionevoli per il numero di LPs host possono essere determinati mediante un'attenta valutazione e monitoraggio dei carichi di lavoro di produzione e convalidati in ambienti non di produzione prima della distribuzione estesa.

## <a name="enabling-and-configuring-minroot"></a>Abilitazione e configurazione di Minroot

La configurazione minroot è controllata tramite le voci BCD hypervisor. Per abilitare minroot, da un prompt dei comandi con privilegi di amministratore:

```
    bcdedit /set hypervisorrootproc n
```
Dove n è il numero di VPs radice. 

Il sistema deve essere riavviato e il nuovo numero di processori radice verrà mantenuto per la durata dell'avvio del sistema operativo.  La configurazione di minroot non può essere modificata dinamicamente in fase di esecuzione.

Se sono presenti più nodi NUMA, ogni nodo `n/NumaNodeCount` otterrà i processori.

Si noti che con più nodi NUMA, è necessario assicurarsi che la topologia della macchina virtuale sia tale che ci siano sufficienti LPs gratuiti (ad esempio, LPs senza VP radice) in ogni nodo NUMA per eseguire il VPs del nodo NUMA della VM corrispondente.

## <a name="verifying-the-minroot-configuration"></a>Verifica della configurazione di Minroot

È possibile verificare la configurazione minroot dell'host tramite Gestione attività, come illustrato di seguito.

![](./media/minroot-taskman.png)

Quando Minroot è attivo, gestione attività visualizzerà il numero di processori logici attualmente assegnati all'host, oltre al numero totale di processori logici nel sistema.
 
