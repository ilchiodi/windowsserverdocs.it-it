---
title: Prestazioni e velocità effettiva del tunnel GRE di Gateway RAS
description: Questo argomento, destinato ai professionisti IT (Information Technology), fornisce informazioni sulle prestazioni della velocità effettiva sui tunnel GRE (Generic Routing Encapsulation) del gateway RAS.
manager: brianlic
ms.prod: windows-server
ms.date: ''
ms.technology: networking-ras
ms.topic: article
ms.assetid: c051b2ec-de0f-49d1-82b9-5742b259cd7c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 79a6e822c3ff36f789a7a08b8cd56163014185a4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404685"
---
# <a name="ras-gateway-gre-tunnel-throughput-and-performance"></a>Prestazioni e velocità effettiva del tunnel GRE di Gateway RAS

>Si applica a: Windows Server \(Semi-canale annuale @ no__t-1

È possibile usare questo argomento per informazioni sul server di accesso remoto \(RAS @ no__t-1 gateway Generic Routing Encapsulation \(GRE @ no__t-3 Performance tunneling in Windows Server, versione 1709, in una rete non definita dal software \(SDN @ no__t-5 based test ambiente.

Il gateway RAS è un router software e un gateway che è possibile usare in modalità single-tenant o multi-tenant. In questo argomento viene illustrata una configurazione a disponibilità elevata in modalità tenant singolo con clustering di failover. Le statistiche sulle prestazioni del tunnel GRE presentate in questo argomento sono valide per il gateway RAS in modalità tenant e multi-tenant.

>[!NOTE]
>Il clustering di failover è una funzionalità di Windows Server che consente di raggruppare più server in un cluster a tolleranza di errore. Per ulteriori informazioni, vedere [clustering di failover](../../../failover-clustering/failover-clustering-overview.md)

La modalità single-tenant consente alle organizzazioni di qualsiasi dimensione di distribuire il gateway come esterno o Internet @ no__t-0facing Edge Virtual Private Network \(VPN @ no__t-2 server. In modalità a tenant singolo è possibile distribuire il gateway RAS in un server fisico o in una macchina virtuale \(VM @ no__t-1. Questo argomento descrive la distribuzione del gateway RAS in due macchine virtuali \(VMs @ no__t-1 configurate in un cluster di failover.

>[!IMPORTANT]
>Poiché i tunnel GRE forniscono l'incapsulamento ma non la crittografia, è consigliabile non usare il gateway RAS configurato con GRE come gateway Internet Edge. Per informazioni sugli usi migliori per il gateway RAS con i tunnel GRE, vedere [tunneling GRE in Windows Server](gre-tunneling-windows-server.md).

GRE è un protocollo di tunneling leggero in grado di incapsulare un'ampia gamma di protocolli a livello di rete all'interno di un punto virtuale @ no__t-0to @ no__t-1Point collegamenti tramite un protocollo Internet. L'implementazione di Microsoft GRE incapsula sia IPv4 che IPv6.

Per ulteriori informazioni, vedere la sezione **scenari di distribuzione di gateway RAS** nell'argomento [gateway RAS](https://docs.microsoft.com/windows-server/remote/remote-access/ras-gateway/ras-gateway#bkmk_deploy). 

In questo scenario di test, illustrato nella figura seguente, il flusso di traffico misurato passa dalla Intranet dell'organizzazione 2 alla Intranet dell'organizzazione 1. Le macchine virtuali del carico di lavoro tenant inviano il traffico di rete dalla rete Intranet 2 alla Intranet 1 usando il gateway RAS.

![Panoramica dello scenario di test del tunnel GRE del gateway RAS](../../media/GRE-Tunnel-Perf/Gre-Infrastructure.jpg)

## <a name="test-environment-configuration"></a>Configurazione dell'ambiente di test

In questa sezione vengono fornite informazioni sull'ambiente di test e sulla configurazione del gateway RAS.

Nell'ambiente di test, le VM del gateway RAS vengono distribuite negli host Hyper @ no__t-0V in un cluster di failover per la disponibilità elevata.

### <a name="hyper-v-host-configuration"></a>Configurazione host Hyper @ no__t-0V

Due host Hyper @ no__t-0V sono configurati per supportare lo scenario di test nel modo seguente. 

- Due computer fisici a doppia @ no__t-0homed sono configurati con Windows Server, versione 1709
- Le due schede di rete fisiche in ciascuno dei due server sono connesse a subnet diverse, entrambe rappresentate da subnet di una Intranet dell'organizzazione. Sia le reti che i componenti hardware di supporto hanno una capacità di 10 GBps.
- L'Hyper-Threading sui server fisici è disabilitato. Questo consente di ottenere la velocità effettiva massima dalle schede di rete fisiche.
- Il ruolo del server Hyper @ no__t-0V viene installato in entrambi i server e configurato con due commutatori virtuali Hyper @ no__t-1V esterni, uno per ogni scheda di rete fisica.
- Poiché entrambi i server sono connessi alla stessa Intranet, i server possono comunicare tra loro.
- Gli host Hyper @ no__t-0V sono configurati in un cluster di failover sulla rete Intranet. 

>[!NOTE]
>Per ulteriori informazioni, vedere [commutatore virtuale Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/hyper-v-virtual-switch).

### <a name="vm-configuration"></a>Configurazione della macchina virtuale

Due macchine virtuali sono configurate per supportare lo scenario di test nel modo seguente.

- In ogni server è installata una macchina virtuale che esegue Windows Server, versione 1709. Ogni macchina virtuale è configurata con 10 core e 8 GB di RAM.
- Ogni macchina virtuale viene configurata anche con due schede di rete virtuali. Una scheda di rete virtuale è connessa al commutere virtuale Intranet 1 e l'altra scheda di rete virtuale è connessa al commutere virtuale Intranet 2.
- Ogni macchina virtuale ha un gateway RAS installato e configurato come server VPN GRE @ no__t-0based.
- Le macchine virtuali del gateway sono configurate in un cluster di failover. Quando si esegue il clustering, una macchina virtuale è attiva e l'altra VM è passiva.

### <a name="workload-hyper-v-hosts-and-vms"></a>Carichi di lavoro Hyper @ no__t-0V host e VM

Per questo test, due host Hyper @ no__t-0V del carico di lavoro vengono installati nella Intranet e in ogni host è installata una macchina virtuale. Se si sta duplicando questo test nel proprio ambiente di test, è possibile installare il numero di server e macchine virtuali del carico di lavoro appropriato per le proprie esigenze.

- Per gli host Hyper @ no__t-0V del carico di lavoro è installata una scheda di rete fisica connessa alla Intranet dell'organizzazione.
- Nel Commuter virtuale Hyper @ no__t-0V viene creato un commutire virtuale in ogni host. Il Commuter è esterno ed è associato a una scheda di rete connessa alla Intranet.
- Le macchine virtuali del carico di lavoro sono configurate con 2 GB di RAM e 2 Core.
- Ognuna delle VM del carico di lavoro dispone di una scheda di rete virtuale connessa al Commuter virtuale Intranet.

### <a name="traffic-generator-tool"></a>Strumento generatore di traffico

Lo strumento generatore di traffico usato in questo test è lo strumento ctsTraffic. Il repository Git per questo strumento si trova in [https://github.com/Microsoft/ctsTraffic](https://github.com/Microsoft/ctsTraffic).

## <a name="ras-gateway-performance"></a>Prestazioni del gateway RAS

Le illustrazioni in questa sezione descrivono le visualizzazioni di gestione attività della velocità effettiva del tunnel GRE con più connessioni TCP.

È possibile ottenere fino a 2,0 Gbps di velocità effettiva su macchine virtuali no__t-0core che sono configurate come gateway RAS GRE.

### <a name="gre-tunnel-performance-with-multiple-tcp-sessions"></a>Prestazioni del tunnel GRE con più sessioni TCP

Con più sessioni TCP, l'utilizzo della CPU raggiunge il 100% e la velocità effettiva massima nel tunnel GRE è 2,0 Gbps.

La figura seguente illustra l'utilizzo della CPU in entrambe le VM del gateway RAS. La macchina virtuale attiva, la macchina virtuale del gateway RAS #1, si trova a sinistra, mentre la macchina virtuale passiva, VM gateway RAS #2, si trova a destra.

![Utilizzo CPU della macchina virtuale del gateway in Gestione attività](../../media/GRE-Tunnel-Perf/Gre-Tunnel-01.jpg)

Nella figura seguente viene illustrata la velocità effettiva della rete Ethernet sulle VM del gateway RAS. La macchina virtuale attiva, la macchina virtuale del gateway RAS #1, si trova a sinistra, mentre la macchina virtuale passiva, VM gateway RAS #2, si trova a destra.

![Velocità effettiva della rete Ethernet della VM del gateway in Gestione attività](../../media/GRE-Tunnel-Perf/Gre-Tunnel-02.jpg)


### <a name="gre-tunnel-performance-with-one-tcp-connection"></a>Prestazioni del tunnel GRE con una connessione TCP

Con la configurazione di test modificata da più sessioni TCP a una singola sessione TCP, solo un core CPU raggiunge la capacità massima sulle VM del gateway RAS.

La velocità effettiva massima nel tunnel GRE è compresa tra 400-500 Mbps.

La figura seguente illustra l'utilizzo della CPU in entrambe le VM del gateway RAS. La macchina virtuale attiva, la macchina virtuale del gateway RAS #1, si trova a sinistra, mentre la macchina virtuale passiva, VM gateway RAS #2, si trova a destra.

![Utilizzo CPU della macchina virtuale del gateway in Gestione attività](../../media/GRE-Tunnel-Perf/Gre-Tunnel-03.jpg)


Nella figura seguente viene illustrata la velocità effettiva della rete Ethernet sulle VM del gateway RAS. La macchina virtuale attiva, la macchina virtuale del gateway RAS #1, si trova a sinistra, mentre la macchina virtuale passiva, VM gateway RAS #2, si trova a destra.

![Velocità effettiva della rete Ethernet della VM del gateway in Gestione attività](../../media/GRE-Tunnel-Perf/Gre-Tunnel-04.jpg)

Per altre informazioni sulle prestazioni del gateway RAS, vedere [ottimizzazione delle prestazioni del gateway HNV in reti definite da software](https://docs.microsoft.com/windows-server/administration/performance-tuning/subsystem/software-defined-networking/hnv-gateway-performance).