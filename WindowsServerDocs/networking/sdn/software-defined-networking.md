---
title: Software Defined Networking (SDN)
description: È possibile utilizzare questo argomento contiene informazioni sulle tecnologie di rete SDN (Software Defined) che sono disponibili in Windows Server, System Center e Microsoft Azure.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 9a1ea73c-20cd-42c5-95ad-b003b9cc6d64
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 33351ccfa1466f667ef9351768c89b373734075d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="software-defined-networking-sdn"></a>Software Defined Networking (SDN)

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento contiene informazioni sulle tecnologie di rete SDN (Software Defined) disponibili in Windows Server Datacenter edition, System Center 2016 e Microsoft Azure.  
  
> [!NOTE]
> Oltre a questo argomento, il contenuto SDN seguente è disponibile.
> 
> - [Novità di SDN per Windows Server](sdn-whats-new.md)
> - [Introduction to SDN in Windows Server Datacenter](sdn-intro.md)
> - [SDN Technologies](technologies/Software-Defined-Networking-Technologies.md)  
> - [Plan SDN](plan/Plan-Software-Defined-Networking.md) 
> - [Deploy SDN](deploy/Deploy-Software-Defined-Networking.md)  
> - [Manage SDN](manage/manage-sdn.md)
> - [Security for SDN](security/sdn-security-top.md)
> - [Troubleshoot SDN](troubleshoot/Troubleshoot-Software-Defined-Networking.md)
> - [System Center Technologies for SDN](Sc-Tech-for-Sdn.md)
> - [Microsoft Azure and SDN](Azure_and_Sdn.md)
> - [Contattare il Data Center e Cloud Team di rete](contact-sdn-team.md)
  
## <a name="bkmk_sdn"></a>Software Defined Networking Panoramica

Software Defined Networking \(SDN\) offre un metodo per configurare e gestire centralmente i dispositivi di rete fisica e virtuale, ad esempio router, commutatori e gateway nel centro dati. Gli elementi di rete virtuale, ad esempio commutatore virtuale Hyper-V, virtualizzazione rete Hyper-V e Gateway RAS sono progettati come parte integrante dell'infrastruttura SDN.

>[!NOTE]
>Per gli host Hyper-V e \(VMs\) macchine virtuali che eseguono Server dell'infrastruttura SDN, ad esempio i nodi di Controller di rete e il bilanciamento del carico Software, è necessario installare Windows Server 2016 Datacenter edition. Per gli host Hyper-V che contengono solo tenant del carico di lavoro macchine virtuali che sono connessi alle reti controllato SDN\, è possibile eseguire Windows Server 2016 Standard edition.

Mentre è ancora possibile utilizzare i commutatori fisici esistenti, router e altri dispositivi hardware, è possibile ottenere una maggiore integrazione tra la rete virtuale e la rete fisica se questi dispositivi sono progettati per garantire la compatibilità con software definito networking.  
  
Poiché i piani di rete - i piani di gestione, controllo e dati - non sono più associati ai dispositivi di rete, ma sono astrazioni per l'uso da altre entità, ad esempio dal software di gestione come System Center 2016, è possibile SDN.  
  
SDN consente di gestire in modo dinamico la rete di Data Center per fornire un modo automatizzato e centralizzato per soddisfare i requisiti delle applicazioni e carichi di lavoro. Software definito networking offre le funzionalità seguenti.  
  
-   La possibilità di astrarre le applicazioni e carichi di lavoro dalla rete fisica sottostante, mediante la virtualizzazione di rete. Come con la virtualizzazione di server con Hyper-V, le astrazioni sono coerenti e lavorano con le applicazioni e carichi di lavoro in senza interruzioni. Ad esempio, definite da software networking fornisce astrazioni virtuali per gli elementi di rete fisica, ad esempio gli indirizzi IP, commutatori e servizi di bilanciamento del carico.  
  
-   La possibilità di definire a livello centralizzato e criteri di controllo che regolano le reti fisiche e virtuali, tra cui il flusso del traffico tra questi due tipi di rete.  
  
-   La possibilità di implementare i criteri di rete in modo coerente su larga scala, anche quando si distribuisce nuovi carichi di lavoro o si sposta tra reti virtuali o fisiche.  
  
## <a name="bkmk_ws"></a>Tecnologie di Windows Server per Software Defined Networking  
Windows Server include le seguenti tecnologie di rete definite da software.  

### <a name="bkmk_nc"></a>Controller di rete

Nuovo in Windows Server 2016, Controller di rete offre un punto programmabile e centralizzato di automazione per gestire, configurare, monitorare e risolvere i problemi di entrambi virtuale e l'infrastruttura di rete fisica nel centro dati. Controller di rete è possibile automatizzare la configurazione dell'infrastruttura di rete invece di eseguire la configurazione manuale dei dispositivi di rete e dei servizi.  
  
Controller di rete è un ruolo del server a disponibilità e scalabilità elevate e offre un'application programming interface (API) - l'API Southbound, che permette al Controller di rete comunicare con la rete e una seconda API - l'API Northbound, che consente di comunicare con il Controller di rete.  
  
Usa Windows PowerShell, l'API Representational State Transfer (REST) o un'applicazione di gestione, è possibile utilizzare Controller di rete per gestire l'infrastruttura di rete fisica e virtuale seguente.  
  
-   Le macchine virtuali Hyper-V e i commutatori virtuali  
  
-   Commutatori di rete fisica  
  
-   Router di rete fisica  
  
-   Software firewall  
  
-   Gateway VPN, inclusi gateway multi-tenant del servizio di accesso remoto (RAS)  
  
-   Servizi di bilanciamento del carico  
  
Per ulteriori informazioni, vedere [Controller di rete](../sdn/technologies/network-controller/Network-Controller.md).  
  
### <a name="bkmk_hv"></a>Virtualizzazione rete Hyper-V

Virtualizzazione rete Hyper-V consente di astrarre le applicazioni e carichi di lavoro dalla rete fisica tramite le reti virtuali. Le reti virtuali forniscono l'isolamento multi-tenant necessario durante l'esecuzione in un'infrastruttura di rete fisica condivisa, questo modo incrementano l'utilizzo delle risorse. Per garantire che è possibile riportare gli investimenti esistenti, è possibile impostare le reti virtuali in un dispositivo di rete esistente. Inoltre, le reti virtuali sono compatibili con reti locali virtuali (VLAN).  
  
Per ulteriori informazioni, vedere [virtualizzazione rete Hyper-V](../sdn/technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md).  
  
### <a name="bkmk_switch"></a>Commutatore virtuale Hyper-V

Il commutatore virtuale Hyper-V è un basata sul software livello 2 switch di rete Ethernet che è disponibile in Hyper-V Manager dopo aver installato il ruolo server Hyper-V. Il commutatore include funzionalità estendibili e gestibili a livello di programmazione a cui connettersi le macchine virtuali sia le reti virtuali e la rete fisica. Inoltre, il commutatore virtuale Hyper-V fornisce imposizione di criteri per sicurezza, isolamento e livelli di servizio.  
  
Commutatore virtuale Hyper-V in Windows Server 2016, è inoltre possibile distribuire Switch Embedded Teaming (SET) e diretta accesso memoria remota (RDMA). Per ulteriori informazioni, vedere la sezione [diretta accesso memoria remota (RDMA) e Switch Embedded Teaming (SET)](#bkmk_rdma) in questo argomento.  
  
Per ulteriori informazioni su commutatore virtuale Hyper-V, vedere [commutatore virtuale Hyper-V](../../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).

### <a name="bkmk_idns"></a>Interna del servizio DNS & #40; nomi IDN & #41;

Macchine virtuali ospitate \(VMs\) e le applicazioni richiedono DNS per comunicare all'interno di reti e risorse esterne su Internet. Con i nomi IDN, è possibile fornire tenant con servizi di risoluzione dei nomi DNS per la loro spazio dei nomi locali, isolato e per le risorse Internet.

Per ulteriori informazioni, vedere [nomi IDN servizio DNS interno & #40; & #41; per SDN](technologies/Idns-for-Sdn.md).
  
### <a name="bkmk_nfv"></a>Virtualizzazione delle funzioni di rete

Nel software di oggi Data Center definito, le funzioni di rete che vengono eseguite da Appliance hardware (ad esempio servizi di bilanciamento del carico, firewall, router, commutatori e così via) sono viene virtualizzato sempre più spesso come Appliance virtuali. Questo "virtualizzazione delle funzioni di rete" è una progressione naturale di virtualizzazione di server e virtualizzazione di rete. Appliance virtuali sono rapidamente emergenti e creazione di un nuovo mercato. Continuano a generare interesse e ottenere l'espansione di entrambe le piattaforme di virtualizzazione e i servizi cloud.  
  
Le tecnologie di virtualizzazione delle funzioni di rete seguenti sono disponibili.  
  
-   **Bilanciamento del carico software (SLB) e NAT (NAT)**. 4 bilanciamento del carico di livello il Nord-Sud e Ovest orientale e migliora la velocità effettiva grazie al supporto Direct Server Return, con cui il traffico di rete restituito può ignorare il bilanciamento del carico multiplexer NAT. Per ulteriori informazioni, vedere [Software Load Balancing (SLB) per SDN](technologies/network-function-virtualization/software-load-balancing-for-sdn.md).  
  
-   **Firewall del centro dati**. Questo firewall distribuito fornisce elenchi di controllo granulare di accesso (ACL), che consente di applicare criteri del firewall a livello di interfaccia di macchina virtuale o a livello di subnet.  
  
    Per ulteriori informazioni, vedere [Cenni preliminari sul Firewall Datacenter](../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md).  
  
-   **Gateway RAS**. È possibile utilizzare i gateway per il bridging del traffico tra reti virtuali e reti non virtualizzati; in particolare, è possibile distribuire gateway VPN da sito a sito, gateway di inoltro e i gateway Generic Routing Encapsulation (GRE). Inoltre, M + N ridondanza di gateway è supportata. Per ulteriori informazioni, vedere [Gateway RAS per SDN](technologies/network-function-virtualization/RAS-Gateway-for-SDN.md).
  
Per ulteriori informazioni, vedere [virtualizzazione delle funzioni di rete](technologies/network-function-virtualization/Network-Function-Virtualization.md).  
  
### <a name="bkmk_rdma"></a>Accesso diretto a memoria remota (RDMA) e Switch Embedded Teaming (SET)  
In Windows Server 2016, è possibile abilitare RDMA sulle schede di rete che sono associate a un commutatore virtuale Hyper-V con o senza Switch Embedded Teaming (SET). In questo modo è possibile utilizzare un minor numero di schede di rete quando si desidera utilizzare RDMA e SET nello stesso momento.  
  
SET è una soluzione alternativa gruppo NIC che è possibile utilizzare in ambienti che includono Hyper-V e lo stack di rete SDN (Software Defined) in Windows Server 2016. SET di alcune delle funzionalità gruppo NIC integra il commutatore virtuale Hyper-V.  
  
SET consente di raggruppare tra uno e otto fisiche schede di rete Ethernet in uno o più schede di rete virtuali basate su software. Queste schede di rete virtuali garantiscono prestazioni elevate e tolleranza di errore in caso di errore di scheda di rete.  
Tutte le schede di rete SET membro devono essere installate in stesso host fisico Hyper-V da inserire in un gruppo.  
  
Inoltre, è possibile utilizzare i comandi di Windows PowerShell per abilitare Data Center Bridging (DCB), creare un commutatore virtuale Hyper-V con una NIC virtuale RDMA (vNIC) e creare un commutatore virtuale Hyper-V con SET e RDMA.  
  
Per ulteriori informazioni, vedere [diretta accesso memoria remota (RDMA) e Switch Embedded Teaming (SET)](https://technet.microsoft.com/library/mt403349.aspx).  
  
### <a name="bkmk_rras"></a>Gateway RAS per SDN  
Gateway RAS è basata su software, multi-tenant, protocollo BGP (Border Gateway) in grado di supportare router in Windows Server 2016 che è progettato per il provider di servizi Cloud (CSP) e alle aziende che ospitano più reti virtuali tenant tramite virtualizzazione rete Hyper-V.  
  
Gateway RAS fornisce pool di gateway, più tipi di connessioni VPN site-to-site e riflettore delle Route BGP per fornirti scelte di progettazione flessibile per l'infrastruttura di gateway e ridondanza N + M.  
  
Per ulteriori informazioni, vedere [Gateway RAS per SDN](technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)  
  
### <a name="bkmk_slb"></a>Software Load Balancing (SLB)  
Provider di servizi cloud (CSP) e le aziende che distribuiscono reti SDN (Software Defined) in Windows Server 2016 è possono utilizzare Software Load Balancing (SLB) per distribuire uniformemente il traffico di rete cliente tenant tra le risorse di rete virtuale e tenant. Windows Server SLB consente più server ospitare stesso carico di lavoro, offrendo disponibilità e scalabilità elevate.  
  
Per ulteriori informazioni, vedere [il bilanciamento del carico Software & #40; SLB & #41; per SDN](../sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md).

### <a name="windows-server-containers"></a>Contenitori di Windows Server

Contenitori di Windows Server sono un metodo di virtualizzazione del sistema operativo leggero usato per separare applicazioni o servizi da altri servizi in esecuzione nello stesso host contenitore. A tale scopo, ogni contenitore dispone di una propria vista del sistema operativo, i processi, file system, del Registro di sistema e gli indirizzi IP. Con Windows Server 2016, è ora possibile connettersi contenitori di Windows Server alle reti virtuali. Per ulteriori informazioni, vedere [contenitori di Windows Server](technologies/containers/Container-networking-overview.md).

### <a name="contact-the-datacenter-and-cloud-networking-product-team"></a>Contatta il team di prodotto Data Center e Cloud di rete

Se sei interessato alla discussione tecnologie SDN con Microsoft o di altri clienti SDN, sono disponibili diversi metodi per rendere contatto.

Per ulteriori informazioni, vedere [contattare il Data Center e Cloud rete Team](contact-sdn-team.md).
