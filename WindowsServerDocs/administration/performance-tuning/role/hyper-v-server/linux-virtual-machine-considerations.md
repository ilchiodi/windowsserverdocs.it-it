---
title: Considerazioni sulla macchina virtuale Linux
description: Macchina virtuale Linux e BSD
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: cc6aab7825754579269eb05e591ca2a3cf5a561b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869682"
---
# <a name="linux-virtual-machine-considerations"></a>Considerazioni sulla macchina virtuale Linux

BSD macchine virtuali Linux e usano considerazioni aggiuntive rispetto alle macchine virtuali Windows in Hyper-V.

La prima considerazione è determinare se sono presenti i servizi di integrazione o se la macchina virtuale è in esecuzione solo su hardware emulato con alcuna illuminazione. È disponibile in una tabella delle versioni di Linux e BSD dotati di Integration Services predefiniti o scaricabili [macchine virtuali Linux e FreeBSD supportate per Hyper-V in Windows](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows). Queste pagine hanno griglie delle funzionalità di Hyper-V disponibili per le versioni di distribuzione di Linux e note di tali funzionalità eventualmente disponibili.

Anche quando l'utente guest è in esecuzione Integration Services, può essere configurato con l'hardware legacy che non mostrano le prestazioni migliori. Ad esempio, configurare e usare una scheda ethernet virtuale per il guest invece di usare una scheda di rete legacy. Con Windows Server 2016, advanced networking, ad esempio SR-IOV sono anche disponibili.

## <a name="linux-network-performance"></a>Prestazioni di rete Linux

Linux per impostazione predefinita consente l'accelerazione hardware e gli offload per impostazione predefinita. Se vRSS è abilitata nelle proprietà di un'interfaccia di rete nell'host e guest Linux ha la possibilità di usare vRSS verrà abilitata la funzionalità. In Powershell è possibile modificare questo stesso parametro con il `EnableNetAdapterRSS` comando.

Analogamente, la funzionalità VMMQ (RSS commutatore virtuale) può essere abilitata nella scheda di rete fisica utilizzata dal guest **delle proprietà** > **Configura...**   >  **Avanzate** scheda > impostare **RSS commutatore virtuale** al **abilitato** o abilitare VMMQ in Powershell usando quanto segue:

```PowerShell
 Set-VMNetworkAdapter -VMName **$VMName** -VmmqEnabled $True
 ```

Nel sistema guest un'ottimizzazione aggiuntiva TCP può essere eseguita mediante l'aumento dei limiti. Per ottenere prestazioni ottimali del carico di lavoro di distribuzione su più CPU e con carichi di lavoro avanzati genera la massima velocità effettiva, come carichi di lavoro virtualizzati avrà una latenza maggiore rispetto a "computer bare metal" quelle.

Alcuni parametri di regolazione di esempio che sono state utili per i benchmark di rete includono:

```PowerShell
net.core.netdev_max_backlog = 30000
net.core.rmem_max = 67108864
net.core.wmem_max = 67108864
net.ipv4.tcp_wmem = 4096 12582912 33554432
net.ipv4.tcp_rmem = 4096 12582912 33554432
net.ipv4.tcp_max_syn_backlog = 80960
net.ipv4.tcp_slow_start_after_idle = 0
net.ipv4.tcp_tw_reuse = 1
net.ipv4.ip_local_port_range = 10240 65535
net.ipv4.tcp_abort_on_overflow = 1
```

È uno strumento utile per rete microbenchmarks ntttcp, che è disponibile per Linux e Windows. La versione di Linux è open source e disponibile dal [ntttcp-for-linux in github.com](https://github.com/Microsoft/ntttcp-for-linux). La versione di Windows è reperibile nella [area download](https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769). Durante l'ottimizzazione di carichi di lavoro è consigliabile usare tutti i flussi in base alle esigenze per ottenere la massima velocità effettiva. Usando ntttcp al traffico di modello, il `-P` parametro imposta il numero di connessioni parallele utilizzate.

## <a name="linux-storage-performance"></a>Prestazioni di archiviazione di Linux

Alcune procedure consigliate, analogo al seguente sono elencati nella [procedure consigliate per Linux in esecuzione in Hyper-V](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/best-practices-for-running-linux-on-hyper-v). Il kernel Linux include diverse utilità di pianificazione dei / o per riordinare le richieste con algoritmi diversi. NOOP è una coda first in, First Out che passa la decisione pianificazione deve risultare dall'hypervisor. È consigliabile usare NOOP come utilità di pianificazione durante l'esecuzione di macchina virtuale Linux in Hyper-V. Per modificare l'utilità di pianificazione per un dispositivo specifico, nella configurazione del caricatore di avvio (/ etc/grub.conf, ad esempio), aggiungere `elevator=noop` ai parametri del kernel e quindi riavviare.

Analogamente alla rete, le prestazioni del guest Linux con l'archiviazione dei benefici riservato il massimo vantaggio da più code con sufficiente dettaglio mantenga l'host occupato. Le prestazioni di archiviazione Microbenchmarking probabilmente sono meglio con lo strumento di benchmark fio con il motore libaio.

## <a name="see-also"></a>Vedere anche

-   [Terminologia di Hyper-V](terminology.md)

-   [Architettura di Hyper-V](architecture.md)

-   [Configurazione del server Hyper-V](configuration.md)

-   [Prestazioni del processore di Hyper-V](processor-performance.md)

-   [Prestazioni della memoria di Hyper-V](memory-performance.md)

-   [Archiviazione di Hyper-V delle prestazioni dei / o](storage-io-performance.md)

-   [Rete Hyper-V delle prestazioni dei / o](network-io-performance.md)

-   [Rilevamento dei colli di bottiglia in un ambiente virtualizzato](detecting-virtualized-environment-bottlenecks.md)
