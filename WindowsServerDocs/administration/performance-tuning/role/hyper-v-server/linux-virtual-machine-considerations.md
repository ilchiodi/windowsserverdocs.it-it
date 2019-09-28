---
title: Considerazioni sulle macchine virtuali Linux
description: Macchina virtuale Linux e BSD
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 5668629e7eded214525561d30fec496a4e91b8dc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385074"
---
# <a name="linux-virtual-machine-considerations"></a>Considerazioni sulle macchine virtuali Linux

Le macchine virtuali Linux e BSD presentano considerazioni aggiuntive rispetto alle macchine virtuali Windows in Hyper-V.

La prima considerazione è se sono presenti Integration Services o se la macchina virtuale è in esecuzione semplicemente su hardware emulato senza illuminazione. Una tabella di versioni di Linux e BSD con Integration Services incorporato o scaricabile è disponibile nelle [macchine virtuali Linux e FreeBSD supportate per Hyper-V in Windows](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows). In queste pagine sono disponibili griglie di funzionalità Hyper-V disponibili per le versioni di distribuzione Linux e note su tali funzionalità, ove applicabile.

Anche quando il Guest esegue Integration Services, può essere configurato con hardware legacy che non presenta le prestazioni migliori. Ad esempio, configurare e usare una scheda Ethernet virtuale per il Guest invece di usare una scheda di rete legacy. Con Windows Server 2016, sono disponibili anche reti avanzate come SR-IOV.

## <a name="linux-network-performance"></a>Prestazioni della rete Linux

Per impostazione predefinita, Linux Abilita l'accelerazione hardware e gli offload. Se vRSS è abilitata nelle proprietà di una scheda di interfaccia di rete nell'host e il Guest Linux è in grado di usare vRSS, la funzionalità verrà abilitata. In PowerShell questo stesso parametro può essere modificato con il comando `EnableNetAdapterRSS`.

Analogamente, è possibile abilitare la funzionalità VMMQ (switch virtuale RSS) nella scheda di interfaccia di rete fisica utilizzata dalle **Proprietà**Guest  > **configure...** scheda**avanzate**  >  > impostare **RSS del Commuter virtuale** su **abilitato** o abilitare VMMQ in PowerShell usando quanto segue:

```PowerShell
 Set-VMNetworkAdapter -VMName **$VMName** -VmmqEnabled $True
 ```

Nell'ottimizzazione TCP aggiuntiva Guest può essere eseguita aumentando i limiti. Per ottenere prestazioni ottimali per la distribuzione del carico di lavoro su più CPU e con carichi di lavoro profondi, la velocità effettiva è ottimale, in quanto i carichi di lavoro virtualizzati avranno una latenza superiore rispetto a quella "bare metal"

Alcuni esempi di parametri di ottimizzazione utili nei benchmark di rete includono:

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

Uno strumento utile per i microbenchmark di rete è NTTTCP, disponibile sia in Linux che in Windows. La versione di Linux è open source e disponibile da [NTTTCP-for-Linux in GitHub.com](https://github.com/Microsoft/ntttcp-for-linux). È possibile trovare la versione di Windows nell' [area download](https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769). Quando si ottimizzano i carichi di lavoro, è preferibile usare il numero di flussi necessari per ottenere la migliore velocità effettiva. Usando NTTTCP per modellare il traffico, il parametro `-P` imposta il numero di connessioni parallele usate.

## <a name="linux-storage-performance"></a>Prestazioni di archiviazione Linux

Alcune procedure consigliate, come quelle riportate di seguito, sono elencate in procedure consigliate [per l'esecuzione di Linux in Hyper-V](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/best-practices-for-running-linux-on-hyper-v). Il kernel Linux ha diverse utilità di pianificazione di I/O per riordinare le richieste con algoritmi diversi. NOOP è una coda First-in First-out che passa la decisione di pianificazione che deve essere eseguita dall'hypervisor. Si consiglia di usare NOOP come utilità di pianificazione durante l'esecuzione di una macchina virtuale Linux in Hyper-V. Per modificare l'utilità di pianificazione per un dispositivo specifico, nella configurazione del caricatore di avvio (ad esempio,/etc/grub.conf) aggiungere `elevator=noop` ai parametri del kernel e quindi riavviare.

Analogamente alla rete, le prestazioni Guest Linux con archiviazione hanno un vantaggio maggiore rispetto a più code con una profondità sufficiente a occupare l'host. Il microbenchmarking delle prestazioni di archiviazione è probabilmente migliore con lo strumento di benchmark fio con il motore Libaio.

## <a name="see-also"></a>Vedere anche

-   [Terminologia di Hyper-V](terminology.md)

-   [Architettura Hyper-V](architecture.md)

-   [Configurazione dei server Hyper-V](configuration.md)

-   [Prestazioni del processore di Hyper-V](processor-performance.md)

-   [Prestazioni della memoria di Hyper-V](memory-performance.md)

-   [Prestazioni di I/O dell'archiviazione di Hyper-V](storage-io-performance.md)

-   [Prestazioni di I/O della rete di Hyper-V](network-io-performance.md)

-   [Rilevamento dei colli di bottiglia in un ambiente virtualizzato](detecting-virtualized-environment-bottlenecks.md)
