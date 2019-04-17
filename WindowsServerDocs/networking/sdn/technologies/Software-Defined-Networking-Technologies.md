---
title: SDN Technologies
description: Gli argomenti in questa sezione forniscono informazioni generali e informazioni tecniche sulle tecnologie di rete definita dal Software che sono inclusi in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.service: virtual-network
ms.technology: networking-sdn
ms.topic: article
ms.assetid: b491089c-5bcb-49d4-95b1-915b7ce69f88
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1f842ac0d1a09106c1898374cf8dd1c7823d7dae
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="sdn-technologies"></a>SDN Technologies

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Gli argomenti in questa sezione forniscono informazioni generali e informazioni tecniche sulle tecnologie di rete definita dal Software che sono inclusi in Windows Server 2016.  
  
> [!NOTE]  
> Per ulteriore documentazione rete definita dal Software, è possibile utilizzare le sezioni seguenti di libreria.  
>   
> - [Plan SDN](../plan/Plan-Software-Defined-Networking.md)
> - [Deploy SDN](../deploy/Deploy-Software-Defined-Networking.md)
> - [Manage SDN](../manage/manage-sdn.md)
> - [Security for SDN](../security/sdn-security-top.md)
> - [Troubleshoot SDN](../troubleshoot/Troubleshoot-Software-Defined-Networking.md)

Sono disponibili numerose tecnologie che collaborano per creare soluzioni di rete SDN (Software Defined) di Microsoft, inclusi i seguenti:  
  
-   **[Border Gateway Protocol & #40; BGP & #41;](../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)**  
  
    Quando è configurato in un Gateway Windows Server 2016 Remote Access Service (RAS), protocollo BGP (Border Gateway) offre la possibilità di gestire il routing del traffico di rete tra reti VM dei tenant e i rispettivi siti remoti. BGP riduce la necessità di configurazione di configurare manualmente le route sui router perché è un protocollo di routing dinamico e apprende le route tra i siti che sono connessi tramite connessioni VPN site-to-site automaticamente.  
  
-   **[Cenni preliminari sul datacenter Firewall](../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)**  
  
    Firewall del centro dati è un nuovo servizio incluso in Windows Server 2016. È un livello di rete, 5 tuple (protocollo, numeri di porta di origine e di destinazione, indirizzi IP di origine e destinazione), con stato e multi-tenant. Quando distribuito e offerto come servizio dal provider di servizi, gli amministratori tenant possono installare e configurare criteri del firewall per proteggere le reti virtuali dal traffico indesiderato provenienti da Internet e reti intranet.  
  
  
-   **[Virtualizzazione rete Hyper-V](../../sdn/technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)**  
  
    Hyper-V rete virtualizzazione consente la virtualizzazione di reti di clienti in un'infrastruttura di rete fisica condivisa.  
  
- **[Interna del servizio DNS & #40; nomi IDN & #41; per SDN](../../sdn/technologies/Idns-for-Sdn.md)**

    Macchine virtuali ospitate \(VMs\) e le applicazioni richiedono DNS per comunicare all'interno di reti e risorse esterne su Internet. Con i nomi IDN, è possibile fornire tenant con servizi di risoluzione dei nomi DNS per la loro spazio dei nomi locali, isolato e per le risorse Internet.

-   **[Controller di rete](../../sdn/technologies/network-controller/Network-Controller.md)**  
  
    Il controller di rete offre un punto programmabile e centralizzato di automazione per gestire, configurare, monitorare e risolvere i problemi dell'infrastruttura di rete fisica e virtuale nel centro dati.  
  
-   **[Virtualizzazione delle funzioni di rete](../../sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md)**  
  
    Le funzioni di rete che vengono eseguite da Appliance hardware (ad esempio servizi di bilanciamento del carico, firewall, router, commutatori e così via) sono viene virtualizzate sempre più spesso come Appliance virtuali.  
  
    Microsoft ha virtualizzati reti, commutatori, gateway, NAT, servizi di bilanciamento del carico e firewall.  

-   **[Gateway RAS per SDN](../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)**
  
    Gateway RAS è basata su software, multi-tenant, protocollo BGP (Border Gateway) in grado di supportare router in Windows Server 2016 che è progettato per il provider di servizi Cloud (CSP) e alle aziende che ospitano più reti virtuali tenant tramite virtualizzazione rete Hyper-V.  
      
- **[Remote Direct Memory Access & #40; RDMA & #41; e passare gruppo incorporato & #40; Imposta & #41;](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)**  
  
    È possibile utilizzare una scheda di rete convergente per combinare il traffico sia RDMA ed Ethernet tramite una singola scheda di rete. La scheda di rete convergente consente di utilizzare una sola scheda di rete per la gestione, archiviazione abilitati RDMA Remote Direct Memory Access e traffico tenant. Ciò riduce le spese di capitale associate a ogni server nel centro dati, perché è necessario un minor numero di schede di rete per gestire diversi tipi di traffico per ciascun server.  
  
    SET è una soluzione gruppo NIC è integrata nel commutatore virtuale Hyper-V. SET consente il raggruppamento di fino a otto schede NIC fisiche in un singolo gruppo SET, che migliora la disponibilità e fornisce il failover. In Windows Server 2016, è possibile creare team SET che sono limitati all'utilizzo di Server Message Block (SMB) e RDMA.
  

-   **[Il bilanciamento del carico software & #40; SLB & #41; per SDN](../../sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md)**  

    Provider di servizi cloud (CSP) e le aziende che distribuiscono reti SDN (Software Defined) in Windows Server 2016 è possono utilizzare Software Load Balancing (SLB) per distribuire uniformemente il traffico di rete cliente tenant tra le risorse di rete virtuale e tenant. Windows Server SLB consente più server ospitare stesso carico di lavoro, offrendo disponibilità e scalabilità elevate.
  
-   **[System Center](../../sdn/Sc-Tech-for-Sdn.md) ** è possibile utilizzare System Center 2016 Virtual Machine Manager (VMM) e Operations Manager per distribuire e gestire l'infrastruttura SDN, inclusi i controller di rete, servizi di bilanciamento del carico software e gateway. È anche possibile utilizzare VMM per definire e controllare i criteri di rete virtuale e collegare i criteri per le applicazioni o carichi di lavoro in modo centralizzato.
  
- **[Contenitori di Windows](../technologies/Containers/Container-networking-overview.md)**
    
    Contenitori di Windows Server sono un metodo di virtualizzazione del sistema operativo leggero usato per separare applicazioni o servizi da altri servizi in esecuzione nello stesso host contenitore. A tale scopo, ogni contenitore dispone di una propria vista del sistema operativo, i processi, file system, del Registro di sistema e gli indirizzi IP. Con Windows Server 2016, è ora possibile connettersi contenitori di Windows Server alle reti virtuali. Contenitori di Windows funzionano in modo analogo alle macchine virtuali in merito alla rete. Ogni contenitore dispone di una scheda di rete virtuale connessa a un commutatore virtuale, in cui viene inoltrato il traffico in entrata e in uscita. Per applicare l'isolamento tra i contenitori nell'host stesso, viene creato un raggruppamento di rete per ogni Server di Windows e il contenitore di Hyper-V in cui è installata la scheda di rete per il contenitore. Contenitori di Windows Server utilizzano una scheda di rete Host per associare al commutatore virtuale. Contenitori di Hyper-V, utilizzare una scheda di rete VM sintetica (non esposte alla macchina virtuale di utilità) per associare al commutatore virtuale.

