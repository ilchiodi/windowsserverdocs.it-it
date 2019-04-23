---
title: Prestazioni e velocità effettiva del tunnel GRE di Gateway RAS
description: Questo argomento, destinato ai professionisti IT (Information Technology), fornisce informazioni sulle prestazioni di velocità effettiva sui tunnel RAS Gateway Generic Routing Encapsulation (GRE).
manager: brianlic
ms.prod: windows-server-threshold
ms.date: ''
ms.technology: networking-ras
ms.topic: article
ms.assetid: c051b2ec-de0f-49d1-82b9-5742b259cd7c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 73ae4e573d926f4a77b076c880c1d74ed69f032d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880762"
---
# <a name="ras-gateway-gre-tunnel-throughput-and-performance"></a>Prestazioni e velocità effettiva del tunnel GRE di Gateway RAS

>Si applica a: Windows Server \(canale semestrale\)

È possibile utilizzare questo argomento per informazioni su Server di accesso remoto \(RAS\) Gateway Generic Routing Encapsulation \(GRE\) tunneling delle prestazioni in Windows Server, versione 1709, in un non - Software Defined Networking \( SDN\) prova basata su ambiente.

Gateway RAS è un router software e gateway che è possibile usare in modalità a tenant singolo o in modalità multi-tenant. Questo argomento illustra una modalità a tenant singolo, una configurazione a elevata disponibilità con Clustering di Failover. Le statistiche sulle prestazioni di tunnel GRE presentate in questo argomento sono valide per il Gateway RAS in modalità multi-tenant e tenant singele.

>[!NOTE]
>Clustering di failover è una funzionalità di Windows Server che consente di raggruppare più server in un cluster a tolleranza di errore. Per altre informazioni, vedere [Clustering di Failover](../../../failover-clustering/failover-clustering-overview.md)

Modalità a tenant singolo consente alle organizzazioni di qualsiasi dimensione per la distribuzione del gateway come un esterno o a Internet\-rivolta verso la rete privata virtuale di edge \(VPN\) server. In modalità a tenant singolo, è possibile distribuire un Gateway RAS su un server fisico o macchina virtuale \(VM\). In questo argomento viene descritta la distribuzione di Gateway RAS in due macchine virtuali \(macchine virtuali\) che sono configurati in un cluster di failover.

>[!IMPORTANT]
>Poiché i tunnel GRE forniscono incapsulamento ma non la crittografia, non è consigliabile utilizzare RAS Gateway configurato con GRE come un gateway Internet di Microsoft edge. Per altre informazioni sugli utilizzi ottimali per il Gateway RAS con tunnel GRE, vedere [Tunneling GRE in Windows Server](gre-tunneling-windows-server.md).

GRE è leggero che può incapsulare un'ampia gamma di protocolli a livello rete all'interno di un punto virtuale protocollo di tunneling\-a\-punto collegamenti su reti Internet Protocol. L'implementazione Microsoft GRE incapsula sia IPv4 che IPv6.

Per altre informazioni, vedere la sezione **scenari di distribuzione di Gateway RAS** nell'argomento [Gateway RAS](https://docs.microsoft.com/windows-server/remote/remote-access/ras-gateway/ras-gateway#bkmk_deploy). 

In questo scenario di test, è illustrato nella figura seguente, il flusso del traffico che viene misurato sposta da 2 Intranet dell'organizzazione out su 1 la rete Intranet dell'organizzazione. Macchine virtuali del carico di lavoro tenant inviare il traffico di rete da 2 Intranet su 1 Intranet mediante Gateway RAS.

![Panoramica dello scenario di test tunnel GRE del Gateway RAS](../../media/GRE-Tunnel-Perf/Gre-Infrastructure.jpg)

## <a name="test-environment-configuration"></a>Configurazione dell'ambiente di test

Questa sezione vengono fornite informazioni sull'ambiente di test e la configurazione di Gateway RAS.

Nell'ambiente di test, RAS Gateway VM vengano distribuite in Hyper\-host V in un failover del cluster per la disponibilità elevata.

### <a name="hyper-v-host-configuration"></a>Hyper\-V configurazione dell'Host

Due Hyper\-V host sono configurati per supportare lo scenario di test nel modo seguente. 

- Dual due\-homed, i computer fisici sono configurati con Windows Server, versione 1709
- Le due schede di rete fisica in ognuno dei due server sono connesse a subnet diverse, che rappresentano le subnet della rete Intranet dell'organizzazione. Entrambe le reti e supporto hardware hanno una capacità pari a 10 GBps.
- Hyper-Threading nei server fisici è disattivato. In questo modo la velocità effettiva massima dalle schede NIC fisiche.
- Il Hyper\-ruolo server V è installato su entrambi i server e configurato con due esterna Hyper\-commutatori virtuali V, uno per ogni scheda di rete fisica.
- Poiché entrambi i server sono connessi alla stessa intranet, i server possono comunicare tra loro.
- Il Hyper\-V host sono configurati in un cluster di failover tramite la rete intranet. 

>[!NOTE]
>Per ulteriori informazioni, vedere [commutatore virtuale Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/hyper-v-virtual-switch).

### <a name="vm-configuration"></a>Configurazione della macchina virtuale

Due macchine virtuali sono configurate per supportare lo scenario di test nel modo seguente.

- In ogni server che viene installata che è una macchina virtuale che esegue Windows Server, versione 1709. Ogni macchina virtuale è configurata con 10 core e 8 GB di RAM.
- Ogni macchina virtuale viene anche configurato con due schede di rete virtuale. Una scheda di rete virtuale è connessa al commutatore virtuale 1 di Intranet e l'altra scheda di rete virtuale è connessa al commutatore virtuale 2 di Intranet.
- Ogni macchina virtuale dispone di un Gateway RAS installato e configurato come un GRE\-basato su server VPN.
- Le macchine virtuali gateway vengono configurate in un cluster di failover. Quando in cluster, una macchina virtuale è attiva e l'altra macchina virtuale è passiva.

### <a name="workload-hyper-v-hosts-and-vms"></a>Carico di lavoro Hyper\-V host e macchine virtuali

Per questo test, due carichi di lavoro Hyper\-host V vengono installati nella intranet e ogni host dispone di una macchina virtuale installata. Se questo test vengono duplicati nel proprio ambiente di test, è possibile installare tutti i server del carico di lavoro e macchine virtuali come appropriato per gli scopi desiderati.

- Carico di lavoro Hyper\-host V hanno una scheda di rete fisica e in cui sia connessa alla rete Intranet dell'organizzazione.
- In Hyper\-virtuale commutatore Hyper-V, viene creato un commutatore virtuale in ogni host. L'opzione è esterno e viene associato alla scheda di una rete connessa alla intranet.
- Il carico delle VM è configurate con 2 core e 2 GB di RAM.
- Le macchine virtuali del carico di lavoro presentano una scheda di rete virtuale connessa al commutatore virtuale intranet.

### <a name="traffic-generator-tool"></a>Strumento generatore di traffico

Lo strumento generatore di traffico che viene usato in questo test è lo strumento ctsTraffic. Il repository Git per questo strumento si trova in [ https://github.com/Microsoft/ctsTraffic ](https://github.com/Microsoft/ctsTraffic).

## <a name="ras-gateway-performance"></a>Prestazioni del Gateway RAS

Le illustrazioni contenute in questa sezione illustrano consente di visualizzare Gestione attività di velocità effettiva del tunnel GRE con più connessioni TCP.

È possibile ottenere una velocità effettiva fino a 2.0 Gbps in più\-core delle macchine virtuali configurate come gateway RAS GRE.

### <a name="gre-tunnel-performance-with-multiple-tcp-sessions"></a>Prestazioni di Tunnel GRE con più sessioni TCP

Con più sessioni TCP, l'utilizzo della CPU raggiunge il 100% e la velocità effettiva massima nel tunnel GRE è 2.0 Gbps.

La figura seguente illustra l'utilizzo della CPU in entrambe le macchine virtuali Gateway RAS. La macchina virtuale attiva, RAS Gateway VM #1, è a sinistra, mentre la macchina virtuale passiva, RAS Gateway VM #2, è a destra.

![Utilizzo della CPU della VM del gateway in Gestione attività](../../media/GRE-Tunnel-Perf/Gre-Tunnel-01.jpg)

La figura seguente illustra una velocità effettiva di rete Ethernet nelle macchine virtuali Gateway RAS. La macchina virtuale attiva, RAS Gateway VM #1, è a sinistra, mentre la macchina virtuale passiva, RAS Gateway VM #2, è a destra.

![Velocità effettiva di rete Ethernet di macchine Virtuali gateway in Gestione attività](../../media/GRE-Tunnel-Perf/Gre-Tunnel-02.jpg)


### <a name="gre-tunnel-performance-with-one-tcp-connection"></a>Prestazioni di Tunnel GRE con una connessione TCP

Con la configurazione di test modificata da più sessioni TCP a una singola sessione TCP, un solo core della CPU raggiunge la capacità massima nelle macchine virtuali Gateway RAS.

La velocità effettiva massima nel tunnel GRE è compreso tra 400-500 Mbps.

La figura seguente illustra l'utilizzo della CPU in entrambe le macchine virtuali Gateway RAS. La macchina virtuale attiva, RAS Gateway VM #1, è a sinistra, mentre la macchina virtuale passiva, RAS Gateway VM #2, è a destra.

![Utilizzo della CPU della VM del gateway in Gestione attività](../../media/GRE-Tunnel-Perf/Gre-Tunnel-03.jpg)


La figura seguente illustra una velocità effettiva di rete Ethernet nelle macchine virtuali Gateway RAS. La macchina virtuale attiva, RAS Gateway VM #1, è a sinistra, mentre la macchina virtuale passiva, RAS Gateway VM #2, è a destra.

![Velocità effettiva di rete Ethernet di macchine Virtuali gateway in Gestione attività](../../media/GRE-Tunnel-Perf/Gre-Tunnel-04.jpg)

Per altre informazioni sulle prestazioni del Gateway RAS, vedere [HNV Gateway ottimizzazione delle prestazioni nel Software definito reti](https://docs.microsoft.com/windows-server/administration/performance-tuning/subsystem/software-defined-networking/hnv-gateway-performance).