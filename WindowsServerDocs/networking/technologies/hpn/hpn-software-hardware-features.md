---
title: Funzionalità e tecnologie per software e hardware (SH, Software e Hardware) integrate
description: Queste funzionalità includono componenti software e hardware. Il software è strettamente correlato alle funzionalità hardware necessarie per il funzionamento della funzionalità. Esempi di questi includono VMMQ, VMQ, offload con checksum IPv4 di trasmissione e RSS.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/12/2018
ms.openlocfilehash: 2f6bb2190dbaa432d20f445c90a560843d6cf4e5
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871939"
---
# <a name="software-and-hardware-sh-integrated-features-and-technologies"></a>Funzionalità e tecnologie per software e hardware (SH, Software e Hardware) integrate

Queste funzionalità includono componenti software e hardware. Il software è strettamente correlato alle funzionalità hardware necessarie per il funzionamento della funzionalità. Esempi di questi includono VMMQ, VMQ, offload con checksum IPv4 di trasmissione e RSS.

>[!TIP]
>Le funzionalità SH e HO sono disponibili se la scheda di interfaccia di rete installata la supporta. Le descrizioni delle funzionalità seguenti illustrano come stabilire se la scheda di interfaccia di rete supporta la funzionalità.

## <a name="converged-nic"></a>NIC convergente 

Converged NIC è una tecnologia che consente alle schede di interfaccia di rete virtuali nell'host Hyper-V di esporre i servizi RDMA ai processi host. Windows Server 2016 non richiede più schede NIC separate per RDMA. La funzionalità NIC convergente consente alle schede di interfaccia di rete virtuali nella partizione host (schede) di esporre RDMA alla partizione host e di condividere la larghezza di banda delle schede di interfaccia di rete tra il traffico RDMA e la macchina virtuale e il traffico TCP/UDP in modo equo e gestibile.

![NIC convergente con SDN](../../media/Converged-NIC/conv-nic-sdn.png)

È possibile gestire l'operazione NIC convergente tramite VMM o Windows PowerShell. I cmdlet di PowerShell sono gli stessi cmdlet usati per RDMA (vedere di seguito).

Per usare la funzionalità NIC convergente:

1.  Assicurarsi di impostare l'host per DCB.

2.  Assicurarsi di abilitare RDMA sulla scheda di interfaccia di rete o, nel caso di un gruppo di SET, le schede di interfaccia di rete sono associate al Commuter Hyper-V. 

3.  Assicurarsi di abilitare RDMA in schede designato per RDMA nell'host. 

Per ulteriori informazioni su RDMA e su SET, vedere [accesso diretto a memoria remota (RDMA) e switch Embedded Teaming (set)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming).

## <a name="data-center-bridging-dcb"></a>Data Center Bridging (DCB) 

DCB è una famiglia di standard IEEE (Institute of Electrical and Electronics Engineers) che Abilita infrastrutture con convergenza nei data center. DCB fornisce la gestione della larghezza di banda basata su coda hardware in un host con una collaborazione dall'opzione adiacente. Tutto il traffico per l'archiviazione, la rete dati, la comunicazione tra processi (IPC) del cluster e la gestione condividono la stessa infrastruttura di rete Ethernet. In Windows Server 2016, DCB può essere applicato a qualsiasi scheda di interfaccia di rete singolarmente e alle schede di interfaccia di rete associato al Commuter Hyper-V.

Per DCB, Windows Server usa il controllo di flusso basato sulla priorità (PFC), standardizzato in IEEE 802.1 QBB. PFC crea un'infrastruttura di rete (quasi) senza perdita di traffico evitando overflow nelle classi di traffico. Windows Server usa anche la selezione di trasmissione migliorata (ETS), standardizzata in IEEE 802.1 Qaz. ETS consente la divisione della larghezza di banda in parti riservate per un massimo di otto classi di traffico. Ogni classe di traffico ha una propria coda di trasmissione e, tramite l'uso di PFC, può avviare e arrestare la trasmissione all'interno di una classe.

Per ulteriori informazioni, vedere [Data Center Bridging (DCB)](https://docs.microsoft.com/windows-server/networking/technologies/dcb/dcb-top).

## <a name="hyper-v-network-virtualization"></a>Virtualizzazione rete Hyper-V

|                            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|----------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **V1 (HNVv1)**       |                     Introdotta in Windows Server 2012, virtualizzazione rete Hyper-V (HNV) consente la virtualizzazione delle reti dei clienti in un'infrastruttura di rete fisica condivisa. Apportando modifiche minime necessari per l'infrastruttura di rete fisica, virtualizzazione RETE fornisce ai provider di servizi la flessibilità di distribuire e migrare i carichi di lavoro tenant ovunque in tre aree: il cloud di provider del servizio, il cloud privato o la cloud pubblica di Microsoft Azure.                     |
| **V2 NVGRE (HNVv2 NVGRE)** | In Windows Server 2016 e System Center Virtual Machine Manager Microsoft offre una soluzione di virtualizzazione di rete end-to-end che include gateway RAS, bilanciamento del carico software, controller di rete e altro ancora. Per ulteriori informazioni, vedere [Panoramica di virtualizzazione rete Hyper-V in Windows Server 2016](https://technet.microsoft.com/windows-server-docs/networking/sdn/technologies/hyper-v-network-virtualization/hyperv-network-virtualization-overview-windows-server). |
| **V2 VxLAN (HNVv2 VxLAN)** |                                                                                                                                                                                        In Windows Server 2016, fa parte dell'estensione SDN, che viene gestita tramite il controller di rete.                                                                                                                                                                                        |

---

## <a name="ipsec-task-offload-ipsecto"></a>Offload attività IPsec (IPsecTO) 

Offload attività IPsec è una funzionalità NIC che consente al sistema operativo di utilizzare il processore nella scheda di interfaccia di rete per il funzionamento della crittografia IPsec.

>[!IMPORTANT] 
>Offload attività IPsec è una tecnologia legacy che non è supportata dalla maggior parte delle schede di rete e da dove esiste, è disabilitata per impostazione predefinita.

## <a name="private-virtual-local-area-network-pvlan"></a>Rete locale virtuale privata (PVLAN). 

PVLAN consente la comunicazione solo tra le macchine virtuali nello stesso server di virtualizzazione. Una rete virtuale privata non è associata a una scheda di rete fisica. Una rete virtuale privata è isolata da tutto il traffico di rete esterno nel server di virtualizzazione, nonché da qualsiasi traffico di rete tra il sistema operativo di gestione e la rete esterna. Questo tipo di rete è utile quando è necessario creare un ambiente di rete isolato, ad esempio un dominio di test isolato. Gli stack Hyper-V e SDN supportano solo la modalità PVLAN isolata.

Per informazioni dettagliate sull'isolamento PVLAN, [vedere System Center: Blog](https://blogs.technet.microsoft.com/scvmm/2013/06/04/logical-networks-part-iv-pvlan-isolation/)sulla progettazione di Virtual Machine Manager.

## <a name="remote-direct-memory-access-rdma"></a>RDMA (Remote Direct Memory Access) 

RDMA è una tecnologia di rete che fornisce una comunicazione a bassa latenza e velocità effettiva elevata che riduce al minimo l'utilizzo della CPU. RDMA supporta la rete a copia zero consentendo alla scheda di rete di trasferire i dati direttamente da o verso la memoria dell'applicazione. RDMA-capable significa che la scheda di interfaccia di rete (fisica o virtuale) è in grado di esporre RDMA a un client RDMA. RDMA-Enabled, d'altra parte, indica che una scheda di interfaccia di rete che supporta RDMA sta esponendo l'interfaccia RDMA.

Per ulteriori informazioni su RDMA, vedere [accesso diretto a memoria remota (RDMA) e switch Embedded Teaming (set)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming).

## <a name="receive-side-scaling-rss"></a>Receive Side Scaling (RSS) 

RSS è una funzionalità NIC che separa diversi set di flussi e li recapita a processori diversi per l'elaborazione. RSS parallelizzazione l'elaborazione della rete, consentendo a un host di eseguire la scalabilità a frequenze dati molto elevate. 

Per ulteriori informazioni, vedere [Receive-Side Scaling (RSS)](https://docs.microsoft.com/windows-hardware/drivers/network/introduction-to-receive-side-scaling).

## <a name="single-root-input-output-virtualization-sr-iov"></a>Single root input-output Virtualization (SR-IOV) 

SR-IOV consente al traffico VM di spostarsi direttamente dalla scheda di interfaccia di rete alla macchina virtuale senza passare attraverso l'host Hyper-V. SR-IOV è un notevole miglioramento delle prestazioni per una macchina virtuale, ma non offre la possibilità per l'host di gestire la pipe. Usare SR-IOV solo se il carico di lavoro è correttamente, attendibile e in genere l'unica macchina virtuale nell'host.

Il traffico che usa SR-IOV ignora il Commuter Hyper-V, il che significa che qualsiasi criterio, ad esempio, ACL o gestione della larghezza di banda non verrà applicato. Anche il traffico SR-IOV non può essere passato attraverso alcuna funzionalità di virtualizzazione di rete, quindi non è possibile applicare l'incapsulamento NV-GRE o VxLAN. Usare SR-IOV solo per i carichi di lavoro affidabili in situazioni specifiche. Inoltre, non è possibile utilizzare i criteri host, la gestione della larghezza di banda e le tecnologie di virtualizzazione.

In futuro, due tecnologie consentivano SR-IOV: Tabelle di flusso generico (GFT) e offload QoS hardware (gestione della larghezza di banda nella scheda di interfaccia di rete): una volta supportate dalle schede di interfaccia di rete nell'ecosistema. La combinazione di queste due tecnologie renderebbe l'utilizzo di SR-IOV per tutte le macchine virtuali, consentirebbe l'applicazione di criteri, virtualizzazione e regole di gestione della larghezza di banda e potrebbe comportare un notevole passo avanti nell'applicazione generale di SR-IOV.

Per ulteriori informazioni, vedere [Panoramica di Single Root I/O Virtualization (SR-IOV)](https://docs.microsoft.com/windows-hardware/drivers/network/overview-of-single-root-i-o-virtualization--sr-iov-).

## <a name="tcp-chimney-offload"></a>TCP Chimney Offload

TCP Chimney Offload, noto anche come offload del motore TCP (TOE), è una tecnologia che consente all'host di eseguire l'offload di tutta l'elaborazione TCP nella scheda di interfaccia di rete. Poiché lo stack TCP di Windows Server è quasi sempre più efficiente del motore TOE, non è consigliabile usare TCP Chimney Offload.

>[!IMPORTANT]
>TCP Chimney Offload è una tecnologia deprecata. Si consiglia di non utilizzare TCP Chimney Offload perché Microsoft potrebbe smettere di supportarla in futuro.

## <a name="virtual-local-area-network-vlan"></a>Rete locale virtuale (VLAN) 

VLAN è un'estensione dell'intestazione del frame Ethernet per consentire il partizionamento di una LAN in più VLAN, ognuna con un proprio spazio degli indirizzi. In Windows Server 2016, le VLAN vengono impostate sulle porte del compartitore Hyper-V o impostando le interfacce del team nei team di gruppo NIC. Per ulteriori informazioni, vedere [Gruppo NIC e reti locali virtuali (VLAN)](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nict-and-vlans).

## <a name="virtual-machine-queue-vmq"></a>Coda macchine virtuali (VMQ) 

Code macchine virtuali è una funzionalità NIC che alloca una coda per ogni macchina virtuale. Quando Hyper-V è abilitato, è anche necessario abilitare VMQ. In Windows Server 2016, code macchine virtuali usa l'opzione NIC finestre con una singola coda assegnata al vPort per fornire la stessa funzionalità. Per ulteriori informazioni, vedere [Virtual Receive Side Scaling (vRSS)](https://docs.microsoft.com/windows-server/networking/technologies/vrss/vrss-top) e [Gruppo NIC](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

## <a name="virtual-machine-multi-queue-vmmq"></a>Macchina virtuale con più code (VMMQ) 

VMMQ è una funzionalità NIC che consente il traffico di una macchina virtuale da distribuire tra più code, ciascuna elaborata da un processore fisico diverso. Il traffico viene quindi passato a più LPs nella macchina virtuale come se si trovasse in vRSS, che consente di inviare una larghezza di banda di rete sostanziale alla macchina virtuale.

---