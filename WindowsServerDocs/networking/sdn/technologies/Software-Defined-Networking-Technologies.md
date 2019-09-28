---
title: Tecnologie SDN
description: Negli argomenti di questa sezione vengono fornite informazioni generali e tecniche sulle tecnologie Software Defined Networking incluse in Windows Server 2016.
manager: dougkim
ms.prod: windows-server
ms.service: virtual-network
ms.technology: networking-sdn
ms.topic: article
ms.assetid: b491089c-5bcb-49d4-95b1-915b7ce69f88
ms.author: pashort
author: shortpatti
ms.date: 02/14/2019
ms.openlocfilehash: b71b17760ec11d7d2ea6a3bfeb118899be9504e7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405955"
---
# <a name="sdn-technologies"></a>Tecnologie SDN

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server (Canale semestrale)

Negli argomenti di questa sezione vengono fornite informazioni generali e tecniche sulle tecnologie Software Defined Networking incluse in Windows Server 2016.  

## <a name="network-controllernetwork-controllernetwork-controllermd"></a>[Controller di rete](network-controller/Network-Controller.md)

Il controller di rete offre un punto di automazione programmabile e centralizzato per gestire, configurare, monitorare e risolvere i problemi dell'infrastruttura di rete fisica e virtuale nel Data Center. Con il controller di rete è possibile automatizzare la configurazione dell'infrastruttura di rete anziché eseguire la configurazione manuale dei dispositivi e dei servizi di rete. 

Il controller di rete è un server a disponibilità elevata e scalabile e fornisce due API (Application Programming Interface):

1. **API sud** -consente al controller di rete di comunicare con la rete.
2. **API in Nord** : consente di comunicare con il controller di rete.

È possibile usare Windows PowerShell, l'API REST (Representational State Transfer) o un'applicazione di gestione per gestire l'infrastruttura di rete fisica e virtuale seguente:

- Macchine virtuali e commutatori virtuali Hyper-V 
- Commutatori di rete fisici 
- Router di rete fisici 
- Software di firewall 
- Gateway VPN, inclusi i gateway multi-tenant del servizio di accesso remoto (RAS) 
- Servizi di bilanciamento del carico 
  
## <a name="hyper-v-network-virtualizationhyper-v-network-virtualizationhyper-v-network-virtualizationmd"></a>[Virtualizzazione rete Hyper-V](hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)

Virtualizzazione rete Hyper-V (HNV) consente di astrarre le applicazioni e i carichi di lavoro dalla rete fisica usando le reti virtuali. Le reti virtuali forniscono l'isolamento multi-tenant necessario durante l'esecuzione su un'infrastruttura di rete fisica condivisa e in questo modo incrementano l'utilizzo delle risorse. Per assicurarsi che sia possibile proseguire con gli investimenti esistenti, è possibile configurare le reti virtuali in un ingranaggio di rete esistente. Inoltre, le reti virtuali sono compatibili con le reti locali virtuali (VLAN).
  
## <a name="hyper-v-virtual-switchvirtualizationhyper-v-virtual-switchhyper-v-virtual-switchmd"></a>[Commutatore virtuale Hyper-V](../../../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md) 

Il Commuter virtuale Hyper-V è un Commuter di rete Ethernet di livello 2 Basato su software disponibile nella console di gestione di Hyper-V dopo aver installato il ruolo server Hyper-V. Il commutatore include funzionalità estendibili e gestibili a livello di programmazione, per la connessione delle macchine virtuali a una rete fisica o virtuale. Inoltre, il Commuter virtuale Hyper-V fornisce l'applicazione dei criteri per sicurezza, isolamento e livelli di servizio.
  
È anche possibile distribuire il Commuter virtuale Hyper-V con switch Embedded Teaming (SET) e accesso diretto a memoria remota (RDMA). Per ulteriori informazioni, vedere la sezione [accesso diretto a memoria remota (RDMA) e switch Embedded Teaming (set)](#remote-direct-memory-access-rdma-and-switch-embedded-teaming-set) in questo argomento.

## <a name="internal-dns-service-idns-for-sdnidns-for-sdnmd"></a>[Servizio DNS interno (IDN) per SDN](Idns-for-Sdn.md)

Le macchine virtuali (VM) e le applicazioni ospitate richiedono che il DNS comunichi all'interno delle proprie reti e con risorse esterne su Internet. Con IDN, è possibile fornire ai tenant i servizi di risoluzione dei nomi DNS per le risorse Internet, lo spazio dei nomi locale e isolato. 
  
## <a name="network-function-virtualizationnetwork-function-virtualizationnetwork-function-virtualizationmd"></a>[Virtualizzazione delle funzioni di rete](network-function-virtualization/Network-Function-Virtualization.md)

I dispositivi hardware, ad esempio i bilanciamenti del carico, i firewall, i router e i commutatori, diventano sempre più dispositivi virtuali. Microsoft dispone di reti virtualizzate, commutatori, gateway, NAT, bilanciamenti del carico e firewall. Questo "virtualizzazione delle funzioni di rete" è una progressione naturale di virtualizzazione di server e virtualizzazione di rete. I dispositivi virtuali sono in rapida emergenza e creano un nuovo mercato. Continuano a generare interesse e ottenere l'espansione di entrambe le piattaforme di virtualizzazione e i servizi cloud. 
  
Sono disponibili le seguenti tecnologie di virtualizzazione delle funzioni di rete.  
  
-   **Bilanciamento del carico software (SLB) e NAT (NAT)** . Migliorare la velocità effettiva supportando Direct Server Return in cui il traffico di rete restituito può aggirare il bilanciamento del carico multiplexer. Per ulteriori informazioni, vedere [bilanciamento del carico software/(SLB/) per Sdn](network-function-virtualization/software-load-balancing-for-sdn.md).
  
-   **datacenter Firewall**. Fornire elenchi di controllo di accesso (ACL) granulari che consentono di applicare i criteri del firewall a livello di interfaccia VM o di subnet. Per altri dettagli, vedere [Cenni preliminari sul firewall di datacenter](network-function-virtualization/Datacenter-Firewall-Overview.md).
  
-   **Gateway RAS per Sdn**. Instradare il traffico di rete tra la rete fisica e le risorse di rete della macchina virtuale, indipendentemente dalla posizione. È possibile instradare il traffico di rete nella stessa posizione fisica o in molte posizioni diverse. Per altri dettagli, vedere [gateway RAS per Sdn](network-function-virtualization/RAS-Gateway-for-SDN.md).

## <a name="remote-direct-memory-access-rdma-and-switch-embedded-teaming-set"></a>Accesso diretto a memoria remota (RDMA) e Switch Embedded Teaming (SET)  
In Windows Server 2016 è possibile abilitare RDMA nelle schede di rete associate a un Commuter virtuale Hyper-V con o senza switch Embedded Teaming (SET). In questo modo è possibile utilizzare un minor numero di schede di rete quando si desidera utilizzare RDMA e impostare contemporaneamente.  
  
SET è una soluzione di gruppo NIC alternativa che è possibile usare in ambienti che includono Hyper-V e lo stack SDN (Software Defined Networking) in Windows Server 2016. SET integra alcune delle funzionalità Gruppo NIC nel Commuter virtuale Hyper-V.  
  
SET consente di raggruppare una o otto schede di rete Ethernet fisiche in una o più schede di rete virtuali basate su software. Queste schede di rete virtuali garantiscono prestazioni elevate e tolleranza di errore in caso di errore delle schede di rete.  
IMPOSTARE le schede di rete del membro devono essere tutte installate nello stesso host Hyper-V fisico da inserire in un team.  
  
Inoltre, è possibile utilizzare i comandi di Windows PowerShell per abilitare Data Center Bridging (DCB), creare un commutire virtuale Hyper-V con una scheda di interfaccia di rete virtuale RDMA (vNIC) e creare un Commuti virtuale Hyper-V con SET e RDMA schede. Per ulteriori informazioni, vedere [accesso diretto a memoria remota (RDMA) e switch Embedded Teaming (set)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming.md).

## <a name="border-gateway-protocol-bgpremoteremote-accessbgpborder-gateway-protocol-bgpmd"></a>[Protocollo BGP (Border Gateway Protocol)](../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)
  
Border Gateway Protocol (BGP) è un protocollo di routing dinamico che apprende automaticamente le route tra i siti che usano connessioni VPN da sito a sito. Quindi, BGP riduce la configurazione manuale dei router.   Quando si configura il gateway RAS, BGP consente di gestire il routing del traffico di rete tra le reti VM dei tenant e i siti remoti.  
  
## <a name="software-load-balancing-slb-for-sdnnetwork-function-virtualizationsoftware-load-balancing-for-sdnmd"></a>[Bilanciamento del carico software (SLB) per SDN](network-function-virtualization/software-load-balancing-for-sdn.md)
I provider di servizi cloud (CSP) e le aziende che distribuiscono SDN possono usare il bilanciamento del carico software (SLB) per distribuire uniformemente il traffico di rete dei clienti tenant e tenant tra le risorse della rete virtuale. Windows Server SLB consente di abilitare più server per l'hosting dello stesso carico di lavoro, offrendo disponibilità e scalabilità elevate. 

## <a name="windows-server-containerscontainerscontainer-networking-overviewmd"></a>[Contenitori di Windows Server](Containers/Container-networking-overview.md)

I contenitori di Windows Server sono un metodo di virtualizzazione del sistema operativo semplice che separa le applicazioni o i servizi da altri servizi in esecuzione nello stesso host contenitore. Ogni contenitore dispone di un sistema operativo, processi, file system, registro di sistema e indirizzi IP propri, che è possibile connettere a reti virtuali. 

## <a name="system-center"></a>System Center

Distribuire e gestire l'infrastruttura SDN con [Virtual Machine Management (VMM)](https://docs.microsoft.com/system-center/vmm/) e [Operations Manager](https://docs.microsoft.com/system-center/scom/). Con VMM puoi effettuare il provisioning e gestire le risorse necessarie per creare e distribuire macchine virtuali e servizi nei cloud privati.  Con Operations Manager puoi monitorare servizi, dispositivi e operazioni in ambito aziendale per identificare problemi e intervenire tempestivamente. 


---