---
title: Funzionalità e tecnologie per software e hardware (SH, Software e Hardware) integrate
description: Queste funzionalità includono componenti hardware e software. Il software è strettamente a funzionalità hardware necessarie per usare la funzionalità. Esempi includono VMMQ VMQ, Offload Checksum IPv4 lato di trasmissione e RSS.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/12/2018
ms.openlocfilehash: 98857a5a6d665728c1aab2a6a2df64997d4166b0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446221"
---
# <a name="software-and-hardware-sh-integrated-features-and-technologies"></a>Funzionalità e tecnologie per software e hardware (SH, Software e Hardware) integrate

Queste funzionalità includono componenti hardware e software. Il software è strettamente a funzionalità hardware necessarie per usare la funzionalità. Esempi includono VMMQ VMQ, Offload Checksum IPv4 lato di trasmissione e RSS.

>[!TIP]
>SH e HO funzionalità sono disponibili se supporta l'interfaccia di rete installata. Le descrizioni delle funzionalità seguente illustra le procedure indicare se la scheda di rete supporta la funzionalità.

## <a name="converged-nic"></a>Scheda di rete convergente 

Scheda di rete convergente è una tecnologia che consente di schede di rete virtuale nell'host Hyper-V per esporre servizi RDMA per i processi host. Windows Server 2016 non richiede più gruppi NIC separati per RDMA. La funzionalità di interfaccia di rete convergente consente le schede di rete virtuale nella partizione Host (Vnic) per esporre RDMA alla partizione host e condividono la larghezza di banda di interfaccia di rete tra il traffico RDMA e la macchina virtuale e altro traffico TCP/UDP in modo notevole e gestibile.

![Scheda di rete convergente con SDN](../../media/Converged-NIC/conv-nic-sdn.png)

È possibile gestire l'operazione di interfaccia di rete convergente mediante VMM o Windows PowerShell. I cmdlet di PowerShell sono gli stessi cmdlet usati per RDMA (vedere sotto).

Usare la funzionalità di interfaccia di rete convergente:

1.  Assicurarsi di impostare l'host per DCB.

2.  Assicurarsi di abilitare RDMA sulla scheda di rete o nel caso di un gruppo SET, sono associate le schede di rete al commutatore Hyper-V. 

3.  Assicurarsi di abilitare RDMA sulla Vnic designato per RDMA nell'host. 

Per altre informazioni sui SET e RDMA, vedere [diretta accesso memoria remota (RDMA) e Switch Embedded Teaming (SET)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming).

## <a name="data-center-bridging-dcb"></a>Data Center Bridging (DCB) 

DCB è una suite di standard Institute of Electrical and Electronics Engineers (IEEE) che abilita infrastrutture con convergenza nei centri dati. DCB fornisce la gestione della larghezza di banda basata su coda hardware in un host con collaborazione da parte il commutatore adiacente. Tutto il traffico per l'archiviazione, rete, dati la comunicazione tra processi (IPC) in cluster e la gestione condividono la stessa infrastruttura di rete Ethernet. In Windows Server 2016, DCB può essere applicata singolarmente a qualsiasi interfaccia di rete e alle interfacce di rete associati a Hyper-V passare.

Per DCB, Windows Server utilizza basato sulla priorità flusso di controllo (base alla priorità), standardizzata IEEE 802.1Qbb. Base alla priorità consente di creare un'infrastruttura di rete (quasi) senza perdita di dati, impedendo l'overflow all'interno di classi di traffico. Windows Server utilizza anche migliorata la trasmissione selezione (ETS), standardizzato nella IEEE 802.1Qaz. ETS consente la divisione della larghezza di banda in parti riservate per un massimo di otto le classi di traffico. Ogni classe di traffico ha il proprio trasmissione della coda e, tramite l'uso di base alla priorità, possono avviare e interrompere la trasmissione all'interno di una classe.

Per altre informazioni, vedere [Data Center Bridging (DCB)](https://docs.microsoft.com/windows-server/networking/technologies/dcb/dcb-top).

## <a name="hyper-v-network-virtualization"></a>Virtualizzazione rete Hyper-V

|                            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|----------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **v1 (HNVv1)**       |                     Introdotta in Windows Server 2012, Hyper-V rete virtualizzazione consente la virtualizzazione di reti dei clienti su un'infrastruttura di rete fisica condivisa. Apportando modifiche minime necessari per l'infrastruttura di rete fisica, virtualizzazione RETE fornisce ai provider di servizi la flessibilità di distribuire e migrare i carichi di lavoro tenant ovunque in tre aree: il cloud di provider del servizio, il cloud privato o la cloud pubblica di Microsoft Azure.                     |
| **v2 NVGRE (HNVv2 NVGRE)** | In Windows Server 2016 e System Center Virtual Machine Manager, Microsoft fornisce una soluzione di virtualizzazione di rete end-to-end che include Gateway RAS, Software Load Balancing, Controller di rete e altro ancora. Per altre informazioni, vedere [Panoramica di virtualizzazione rete Hyper-V in Windows Server 2016](https://technet.microsoft.com/windows-server-docs/networking/sdn/technologies/hyper-v-network-virtualization/hyperv-network-virtualization-overview-windows-server). |
| **v2 VxLAN (HNVv2 VxLAN)** |                                                                                                                                                                                        In Windows Server 2016, fa parte del SDN-estensione, che è gestire tramite il Controller di rete.                                                                                                                                                                                        |

---

## <a name="ipsec-task-offload-ipsecto"></a>Offload attività IPsec (IPsecTO) 

Offload attività IPsec è una funzionalità di interfaccia di rete che consente al sistema operativo da utilizzare il processore sull'interfaccia di rete per le operazioni di crittografia IPsec.

>[!IMPORTANT] 
>Offload attività IPsec è una tecnologia legacy che non è supportata per la maggior parte delle schede di rete e in cui esiste, è disabilitato per impostazione predefinita.

## <a name="private-virtual-local-area-network-pvlan"></a>Virtual Local Area Network (PVLAN) private. 

Pvlan consentano la comunicazione solo tra le macchine virtuali nello stesso server di virtualizzazione. Una rete virtuale privata non è associata a una scheda di rete fisica. Una rete virtuale privata è isolata da tutto il traffico di rete esterno nel server di virtualizzazione, nonché qualsiasi traffico di rete tra il sistema operativo di gestione e la rete esterna. Questo tipo di rete è utile quando è necessario creare un ambiente di rete isolato, ad esempio un dominio di test isolato. Di Hyper-V e gli stack SDN supportano solo modalità PVLAN isolata porta.

Per informazioni dettagliate sull'isolamento PVLAN, vedere [System Center: Blog sulla progettazione di Virtual Machine Manager](https://blogs.technet.microsoft.com/scvmm/2013/06/04/logical-networks-part-iv-pvlan-isolation/).

## <a name="remote-direct-memory-access-rdma"></a>RDMA (Remote Direct Memory Access) 

RDMA è una tecnologia di rete che fornisce comunicazioni a velocità effettiva elevata e bassa latenza che riduce al minimo l'utilizzo della CPU. RDMA supporta rete copiare di zero, consentendo la scheda di rete trasferire i dati direttamente a o da memoria dell'applicazione. Supporto per RDMA, significa che l'interfaccia di rete (fisica o virtuale) è in grado di esporre RDMA a un client RDMA. Un'interfaccia di rete con supporto per RDMA è che espone l'interfaccia RDMA nello stack di abilitate per RDMA, d'altra parte, significa.

Per altre informazioni su RDMA, vedere [diretta accesso memoria remota (RDMA) e Switch Embedded Teaming (SET)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming).

## <a name="receive-side-scaling-rss"></a>Receive Side Scaling (RSS) 

RSS è una funzionalità di interfaccia di rete che segrega set diversi di flussi e li distribuisce ai processori diversi per l'elaborazione. RSS viene eseguita la parallelizzazione di connessione di rete, l'elaborazione, l'abilitazione di un host per la scalabilità a dati molto elevate. 

Per altre informazioni, vedere [Receive Side Scaling (RSS)](https://docs.microsoft.com/windows-hardware/drivers/network/introduction-to-receive-side-scaling).

## <a name="single-root-input-output-virtualization-sr-iov"></a>Single Root Input-Output Virtualization (SR-IOV) 

SR-IOV consente il traffico delle VM spostare direttamente dall'interfaccia di rete alla macchina virtuale senza passare attraverso l'host Hyper-V. SR-IOV è un incredibile miglioramento delle prestazioni per una macchina virtuale ma non ha la possibilità per l'host gestire tale pipe. Usare solo SR-IOV quando il carico di lavoro è ben progettata, considerato attendibile e in genere l'unica macchina virtuale nell'host.

Il traffico che utilizza SR-IOV ignora il commutatore Hyper-V, il che significa che tutti i criteri, ad esempio, gli ACL, o gestione della larghezza di banda non verrà applicato. Il traffico SR-IOV inoltre non può essere passato tramite qualsiasi funzionalità di virtualizzazione di rete, in modo che non può essere applicato incapsulamento VxLAN o NV GRE. Usare SR-IOV per i carichi di lavoro ben attendibile in situazioni specifiche. Inoltre, è possibile utilizzare i criteri host, la gestione della larghezza di banda e le tecnologie di virtualizzazione.

In futuro, due tecnologie consentirebbe SR-IOV: Flusso tabelle (GFT) e non generici interfaccia l'Offload QoS Hardware (risparmio della larghezza di banda nell'interfaccia di rete)-una volta che li supportano le schede di rete nel nostro ecosistema. La combinazione di queste due tecnologie renderebbe consentirebbe SR-IOV utile per tutte le macchine virtuali, i criteri di virtualizzazione e le regole di gestione della larghezza di banda da applicare e potrebbe comportare nulla ottimale inoltrare nella categoria generale dell'applicazione di SR-IOV.

Per altre informazioni, vedere [Panoramica di Single Root i/o Virtualization (SR-IOV)](https://docs.microsoft.com/windows-hardware/drivers/network/overview-of-single-root-i-o-virtualization--sr-iov-).

## <a name="tcp-chimney-offload"></a>TCP Chimney Offload

TCP Chimney Offload, detta anche TCP motore Offload (TOE), è una tecnologia che consente all'host per l'offload di TCP tutti l'elaborazione per l'interfaccia di rete. Poiché lo stack TCP di Windows Server è quasi sempre più efficiente rispetto al motore TOE, TCP Chimney Offload non è consigliata.

>[!IMPORTANT]
>TCP Chimney Offload è una tecnologia obsoleta. È consigliabile che non usare TCP Chimney Offload come Microsoft potrebbe smettere di supportandolo in futuro.

## <a name="virtual-local-area-network-vlan"></a>Virtual Local Area Network (VLAN) 

VLAN è un'estensione per l'intestazione di frame Ethernet per abilitare il partizionamento di una LAN in più VLAN, ognuna delle quali Usa un proprio spazio degli indirizzi. In Windows Server 2016, le VLAN vengono impostate su porte del commutatore Hyper-V o impostando le interfacce di team sul team NIC Teaming. Per altre informazioni, vedere [NIC e reti locali virtuali (VLAN)](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nict-and-vlans).

## <a name="virtual-machine-queue-vmq"></a>Coda macchine virtuali (VMQ) 

Code è una funzionalità di interfaccia di rete che consente di allocare una coda per ogni macchina virtuale. Ogni volta che si dispone di Hyper-V abilitato. è anche necessario abilitare coda macchine Virtuali. In Windows Server 2016, code usano VPort commutatore di interfaccia di rete con una singola coda assegnata al vPort per fornire la stessa funzionalità. Per altre informazioni, vedere [Virtual Receive-Side Scaling (vRSS)](https://docs.microsoft.com/windows-server/networking/technologies/vrss/vrss-top) e [NIC Teaming](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

## <a name="virtual-machine-multi-queue-vmmq"></a>Macchina virtuale più code (VMMQ) 

VMMQ è una funzionalità di interfaccia di rete che consenta il traffico per una macchina virtuale distribuiti tra più code, ognuna elaborato da un processore fisico diverso. Il traffico viene quindi passato al più LPs nella macchina virtuale come lo sarebbe di vRSS, che consente di offrire considerevole larghezza di banda di rete alla macchina virtuale.

---