---
title: Tecnologie SDN
description: Negli argomenti di questa sezione forniscono una panoramica e informazioni tecniche sulle tecnologie di Software Defined Networking inclusi in Windows Server 2016.
manager: dougkim
ms.prod: windows-server-threshold
ms.service: virtual-network
ms.technology: networking-sdn
ms.topic: article
ms.assetid: b491089c-5bcb-49d4-95b1-915b7ce69f88
ms.author: pashort
author: shortpatti
ms.date: 02/14/2019
ms.openlocfilehash: acf3e1dc3e5a229c525ba7cad23819640c0d5261
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59891162"
---
# <a name="sdn-technologies"></a>Tecnologie SDN

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server (canale semestrale)

Negli argomenti di questa sezione forniscono una panoramica e informazioni tecniche sulle tecnologie di Software Defined Networking inclusi in Windows Server 2016.  

## <a name="network-controllernetwork-controllernetwork-controllermd"></a>[Controller di rete](network-controller/Network-Controller.md)

Il controller di rete offre un punto programmabile e centralizzato di automazione per gestire, configurare, monitorare e risolvere i problemi sia virtuale e l'infrastruttura di rete fisica nel Data Center. Con il controller di rete, è possibile automatizzare la configurazione dell'infrastruttura di rete invece di eseguire la configurazione manuale dei dispositivi di rete e dei servizi. 

Il controller di rete è un server altamente disponibile e scalabile e offre due application programming interface (API):

1. **API southbound** : consente al controller di rete di comunicare con la rete.
2. **API northbound** – consente all'utente di comunicare con il controller di rete.

È possibile utilizzare Windows PowerShell, l'API Representational State Transfer (REST) o un'applicazione di gestione per gestire l'infrastruttura di rete fisica e virtuale seguente:

- Macchine virtuali e commutatori virtuali Hyper-V 
- Commutatori di rete fisici 
- Router di rete fisici 
- Software di firewall 
- Gateway VPN, inclusi gateway multi-tenant del servizio di accesso remoto (RAS) 
- Servizi di bilanciamento del carico 
  

  
## <a name="hyper-v-network-virtualizationhyper-v-network-virtualizationhyper-v-network-virtualizationmd"></a>[Virtualizzazione rete Hyper-V](hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)

Hyper-V rete virtualizzazione consente di astrarre le applicazioni e carichi di lavoro dalla rete fisica tramite reti virtuali. Le reti virtuali forniscono l'isolamento multi-tenant necessario durante l'esecuzione su un'infrastruttura di rete fisica condivisa e in questo modo incrementano l'utilizzo delle risorse. Per garantire che si possono riportano i tuoi investimenti esistenti, è possibile configurare reti virtuali in un dispositivo di rete esistente. Inoltre, le reti virtuali sono compatibili con reti locali virtuali (VLAN).   
  
  
## <a name="hyper-v-virtual-switchvirtualizationhyper-v-virtual-switchhyper-v-virtual-switchmd"></a>[Commutatore virtuale Hyper-V](../../../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md) 

Il commutatore virtuale Hyper-V è un software in base al livello 2 switch di rete Ethernet che è disponibile nella console di gestione Hyper-V dopo aver installato il ruolo server Hyper-V. Il commutatore include funzionalità estendibili e gestibili a livello di programmazione, per la connessione delle macchine virtuali a una rete fisica o virtuale. Commutatore virtuale Hyper-V fornisce, inoltre, l'applicazione dei criteri per sicurezza, isolamento e livelli di servizio.
  
È anche possibile distribuire il commutatore virtuale Hyper-V con Direct accesso memoria remota (RDMA) e Switch Embedded Teaming (SET). Per altre informazioni, vedere la sezione [diretta accesso memoria remota (RDMA) e Switch Embedded Teaming (SET)](#bkmk_rdma) in questo argomento.  

## <a name="internal-dns-service-idns-for-sdnidns-for-sdnmd"></a>[Servizio DNS interno (iDNS) per SDN](Idns-for-Sdn.md)

Host macchine virtuali (VM) e le applicazioni richiedono DNS per la comunicazione all'interno delle reti e con risorse esterne su Internet. Con i nomi IDN, è possibile fornire tenant con servizi di risoluzione dei nomi DNS per lo spazio dei nomi locale, isolato e le risorse Internet. 
  
## <a name="network-function-virtualizationnetwork-function-virtualizationnetwork-function-virtualizationmd"></a>[Virtualizzazione di rete (funzione)](network-function-virtualization/Network-Function-Virtualization.md)

Appliance hardware, ad esempio servizi di bilanciamento del carico, firewall, router e commutatori stanno diventando sempre più Appliance virtuali. Microsoft ha virtualizzati reti, commutatori, i gateway, NAT, servizi di bilanciamento del carico e firewall. Questo "virtualizzazione delle funzioni di rete" è una progressione naturale di virtualizzazione di server e virtualizzazione di rete. Appliance virtuali sono rapidamente emergenti e creazione di un nuovo mercato. Continuano a generare interesse e ottenere l'espansione di entrambe le piattaforme di virtualizzazione e i servizi cloud. 
  
Sono disponibili le seguenti tecnologie di virtualizzazione delle funzioni di rete.  
  
-   **Bilanciamento del carico software (SLB) e NAT (NAT)**. Migliora la velocità effettiva grazie al supporto Direct Server Return in cui il traffico di rete restituito può ignorare il bilanciamento del carico multiplexer. Per altre informazioni, vedere [/(SLB/) il bilanciamento del carico Software per SDN](network-function-virtualization/software-load-balancing-for-sdn.md).
  
-   **datacenter Firewall**. Fornire elenchi di controllo granulare di accesso (ACL), che consente di applicare criteri del firewall a livello di interfaccia di macchina virtuale o il livello di subnet. Per altre informazioni, vedere [Cenni preliminari sul Firewall Datacenter](network-function-virtualization/Datacenter-Firewall-Overview.md).
  
-   **Gateway RAS per SDN**. Instradare il traffico di rete tra la rete fisica e le risorse di rete della macchina virtuale, indipendentemente dalla posizione. È possibile instradare il traffico di rete nella stessa posizione fisica o molte posizioni diverse. Per altre informazioni, vedere [Gateway RAS per SDN](network-function-virtualization/RAS-Gateway-for-SDN.md).

  
## <a name="remote-direct-memory-access-rdma-and-switch-embedded-teaming-sethttpsdocsmicrosoftcomwindows-servervirtualizationhyper-v-virtual-switchrdma-and-switch-embedded-teaming"></a>[Accesso diretto a memoria remota (RDMA) e Switch Embedded Teaming (SET)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming)  
In Windows Server 2016, è possibile abilitare RDMA sulle schede di rete che sono associate a un commutatore virtuale Hyper-V con o senza Switch Embedded Teaming (SET). In questo modo è possibile utilizzare un minor numero di schede di rete quando si desidera utilizzare SET e RDMA nello stesso momento.  
  
SET è una soluzione alternativa gruppo NIC che è possibile usare in ambienti che includono Hyper-V e lo stack di rete SDN (Software Defined) in Windows Server 2016. SET integra alcune delle funzionalità gruppo NIC nel commutatore virtuale Hyper-V.  
  
SET consente di raggruppare tra uno e otto Ethernet schede di rete fisiche in una o più schede di rete virtuale basata su software. Queste schede di rete virtuali garantiscono prestazioni elevate e tolleranza di errore in caso di errore delle schede di rete.  
Tutte le schede di rete membro SET devono essere installate nello stesso host fisico Hyper-V da inserire in un team.  
  
Inoltre, è possibile utilizzare i comandi di Windows PowerShell per abilitare Data Center Bridging (DCB), creare un commutatore virtuale Hyper-V con una NIC virtuale (vNIC) di RDMA e creare un commutatore virtuale Hyper-V con Vnic SET e RDMA.  

  

## <a name="border-gateway-protocol-bgpremoteremote-accessbgpborder-gateway-protocol-bgpmd"></a>[Border Gateway Protocol (BGP)](../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)
  
Protocollo BGP (Border Gateway) è un protocollo di routing dinamico che apprende automaticamente le route tra i siti che usano le connessioni VPN site-to-site. Pertanto, BGP consente di ridurre la configurazione manuale del router.   Quando si configura Gateway RAS, BGP consente di gestire il routing del traffico di rete tra le reti VM dei tenant e i siti remoti.  
  
## <a name="software-load-balancing-slb-for-sdnnetwork-function-virtualizationsoftware-load-balancing-for-sdnmd"></a>[Software Load Balancing (SLB) per SDN](network-function-virtualization/software-load-balancing-for-sdn.md)
Provider di servizi cloud (CSP) e aziende che distribuiscono SDN possono utilizzare il bilanciamento del carico Software (SLB) per distribuire uniformemente il traffico di rete cliente tenant tra le risorse della rete virtuale e del tenant. Windows Server SLB consente di abilitare più server per l'hosting dello stesso carico di lavoro, offrendo disponibilità e scalabilità elevate. 

## <a name="windows-server-containerscontainerscontainer-networking-overviewmd"></a>[Contenitori di Windows Server](Containers/Container-networking-overview.md)

Contenitori di Windows Server sono un metodo di virtualizzazione del sistema operativo leggero che separa le applicazioni o servizi da altri servizi in esecuzione nello stesso host contenitore. Ogni contenitore dispone di un proprio sistema operativo, processi, file system, Registro di sistema e indirizzi IP, che è possibile connettersi alle reti virtuali. 


## <a name="system-center"></a>System Center  
Distribuire e gestire l'infrastruttura SDN con [Virtual Machine Management (VMM)](https://docs.microsoft.com/system-center/vmm/) e [Operations Manager](https://docs.microsoft.com/system-center/scom/). Con VMM, puoi effettuare il provisioning e gestire le risorse necessarie per creare e distribuire macchine virtuali e servizi nei cloud privati.  Con Operations Manager, è monitorare servizi, dispositivi e operazioni nell'intera azienda per identificare i problemi per un intervento immediato. 


---