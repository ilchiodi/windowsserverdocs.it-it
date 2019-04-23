---
title: Rete
description: In questo argomento viene fornita una panoramica delle tecnologie Software Defined Networking e di piattaforma di rete disponibili in Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.date: 05/08/2018
ms.assetid: daaf6b61-5953-4c2d-b6b8-7c885b552646
manager: dougkim
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: 7caa99f1b6b9e25e5a6f2c4333b033fb3088195d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862592"
---
# <a name="networking"></a>Rete

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

>[!TIP]
> Cerchi informazioni sull'installazione delle versioni precedenti di Windows Server? Vedi le nostre altre [librerie di Windows Server](/previous-versions/windows/) in docs.microsoft.com. Puoi anche [cercare nel sito](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) informazioni specifiche.

<img src="../media/landing-icons/network.png" style='float:left; padding:.5em;' alt="Icon depicting two networked computers"> Funzionalità di rete sono una parte fondamentale del Software definito Datacenter \(SDDC\) platform e Windows Server 2016 fornisce nuove e migliorate Software Defined Networking \(SDN\) tecnologie che consentono di spostare in una soluzione SDDC realizzata completamente per l'organizzazione.

Quando si gestiscono le reti come risorse software–defined, è possibile descrivere i requisiti di un'applicazione relativi all'infrastruttura una sola volta e quindi scegliere dove eseguire l'applicazione, se in locale o nel cloud. 

Grazie a questa coerenza, le applicazioni sono ora più scalabili e possono essere eseguite senza problemi ovunque e sempre con lo stesso livello di sicurezza, prestazioni, qualità del servizio e disponibilità.

>[!Note]
> Per scaricare Windows Server, vedi la pagina delle [versioni di valutazione di Windows Server](https://www.microsoft.com/evalcenter/evaluate-windows-server-technical-preview).

In Windows Server 2016 sono disponibili le nuove tecnologie di rete seguenti:

- Software Defined Networking: Controller di rete rappresenta un punto di automazione programmabile e centralizzato per gestire, configurare e monitorare l'infrastruttura di rete fisica e virtuale nel centro dati e di risolverne gli eventuali problemi. Controller di rete consente di usare la virtualizzazione della funzione di rete per distribuire facilmente le macchine virtuali \(VMs\) per il bilanciamento del carico Software \(SLB\) per ottimizzare il traffico di rete viene caricato per il tenant e RAS I gateway per offrire ai tenant con le opzioni di connettività necessarie tra Internet, in locale e cloud alle risorse. È anche possibile usare Controller di rete per gestire il firewall del centro dati su macchine virtuali e host Hyper–V.

- Piattaforma di rete: Usa le nuove funzionalità per le tecnologie di piattaforma di rete esistenti, è possibile utilizzare criteri DNS per personalizzare le risposte del server DNS alle query, usare un'interfaccia di rete convergente che gestisce combinati Remote Direct Memory Access \(RDMA\) e traffico Ethernet , utilizzare Switch Embedded Teaming \(impostata\) per creare commutatori virtuali Hyper-V connesso a schede NIC RDMA e usare Gestione indirizzi IP \(IPAM\) per gestire le zone DNS e server, nonché gli indirizzi IP e DHCP.

Per altre informazioni, vedi [Scenari di rete supportati in Windows Server](windows-server-supported-networking-scenarios.md).

Le sezioni seguenti forniscono informazioni sulle tecnologie SDN e di piattaforma di rete.

## <a name="software-defined-networking-technologies"></a>Tecnologie di Software Defined Networking

### <a name="software-defined-networking-40sdn41sdnsoftware-defined-networkingmd"></a>[Software Defined Networking &#40;SDN&#41;](sdn/software-defined-networking.md)

Questo argomento contiene informazioni sulle tecnologie SDN disponibili in Windows Server, System Center e Microsoft Azure.

>[!NOTE]
>Per gli host Hyper-V e macchine virtuali \(macchine virtuali\) che eseguono Server di infrastruttura SDN, ad esempio i nodi di Controller di rete e il bilanciamento del carico Software, è necessario installare Windows Server 2016 Datacenter edition. Per Hyper-V host che contengono solo tenant le macchine virtuali del carico di lavoro che sono connessi alla rete SDN\-controllato le reti, è possibile eseguire Windows Server 2016 Standard edition.

### <a name="deploy-a-software-defined-network-infrastructure-using-scriptssdndeploydeploy-a-software-defined-network-infrastructure-using-scriptsmd"></a>[Distribuire un'infrastruttura Software Defined Networking tramite script](sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)
Questa guida offre alcune istruzioni per la distribuzione di Controller di rete con le reti e i gateway virtuali in un lab di test.

### <a name="network-controllersdntechnologiesnetwork-controllernetwork-controllermd"></a>[Controller di rete](sdn/technologies/network-controller/Network-Controller.md)

Controller di rete rappresenta un punto di automazione programmabile e centralizzato per gestire, configurare e monitorare l'infrastruttura di rete fisica e virtuale nel centro dati e di risolverne gli eventuali problemi.

### <a name="software-load-balancing-40slb41-for-sdnsdntechnologiesnetwork-function-virtualizationsoftware-load-balancing-for-sdnmd"></a>[Il bilanciamento del carico software &#40;bilanciamento del carico software&#41; per la rete SDN](sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md)

I provider di servizi cloud \(CSP\) e le aziende che distribuiscono reti SDN (Software Defined) in Windows Server 2016 è possono usare il bilanciamento del carico Software \(SLB\) per distribuire uniformemente i tenant e tenant traffico di rete dei clienti tra le risorse della rete virtuale. Windows Server SLB consente di abilitare più server per l'hosting dello stesso carico di lavoro, offrendo disponibilità e scalabilità elevate.

### <a name="ras-gateway-for-sdnsdntechnologiesnetwork-function-virtualizationras-gateway-for-sdnmd"></a>[Gateway RAS per SDN](sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)

Gateway RAS, ovvero una basata su software, multi-tenant, Border Gateway Protocol \(BGP\) router in grado di supportare in Windows Server 2016, è progettato per i provider di servizi Cloud \(CSP\) e aziende che ospitano più reti virtuali tenant tramite virtualizzazione rete Hyper-V. 

### <a name="network-function-virtualizationsdntechnologiesnetwork-function-virtualizationnetwork-function-virtualizationmd"></a>[Virtualizzazione di rete (funzione)](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md)

In Data Center software definito, le funzioni che vengono eseguite da Appliance hardware di rete \(, ad esempio servizi di bilanciamento del carico, firewall, router, commutatori e così via\) sono sempre più spesso come Appliance virtuali. Questo "virtualizzazione delle funzioni di rete" è una progressione naturale di virtualizzazione di server e virtualizzazione di rete.

### <a name="datacenter-firewall-overviewsdntechnologiesnetwork-function-virtualizationdatacenter-firewall-overviewmd"></a>[Cenni preliminari sul datacenter Firewall](sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)

Il firewall del centro dati è un firewall multi–tenant, con stato, a 5 tuple (protocollo, numeri di porta di origine e destinazione, indirizzi IP di origine e destinazione), a livello rete.

## <a name="bkmk_networking"></a>Tecnologie di rete

La tabella seguente mette a disposizione i collegamenti ad alcune tecnologie di rete in Windows Server 2016.

### <a name="whats-new-in-networkingnetworkingwhat-s-new-in-networkingmd"></a>[What ' s New in rete](../networking/What-s-New-in-Networking.md)

Nelle seguenti sezioni puoi trovare informazioni sulle nuove tecnologie di rete e sulle nuove funzionalità per le tecnologie esistenti in Windows Server 2016.

### <a name="branchcachebranchcachebranchcachemd"></a>[BranchCache](branchcache/BranchCache.md)

BranchCache è una rete WAN \(WAN\) tecnologia di ottimizzazione della larghezza di banda. Per ottimizzare la larghezza di banda della rete WAN quando gli utenti accedono al contenuto di server remoti, BranchCache recupera il contenuto dai server di contenuti della sede centrale o ospitati nel cloud e lo memorizza nella cache presso le succursali, consentendo ai computer client delle succursali stesse di accedere al contenuto in locale anziché attraverso la rete WAN.

### <a name="core-network-guide-for-windows-server-2016core-network-guidecore-network-guide-windows-servermd"></a>[Guida alla rete core per Windows Server 2016](core-network-guide/core-network-guide-windows-server.md)

Scopri come distribuire una rete di Windows Server con la Guida alla rete core e aggiungere funzionalità alla distribuzione di rete con le Guide complementari alla rete core.

### <a name="directaccessremoteremote-accessdirectaccessdirectaccessmd"></a>[DirectAccess](../remote/remote-access/directaccess/DirectAccess.md)

DirectAccess consente agli utenti remoti di connettersi alle risorse di rete dell'organizzazione. 

La documentazione di DirectAccess è ora disponibile nella sezione [Gestione di accesso remoto e server](https://docs.microsoft.com/windows-server/remote/) nel sommario di Windows Server 2016, in [Accesso remoto](https://docs.microsoft.com/windows-server/remote/remote-access/remote-access). Per altre informazioni, vedi [DirectAccess](../remote/remote-access/directaccess/DirectAccess.md).

### <a name="domain-name-system-40dns41dnsdns-topmd"></a>[Domain Name System &#40;DNS&#41;](dns/dns-top.md)

DNS \(Domain Name System\) è una suite di protocolli standard di settore che comprende il protocollo TCP/IP. Insieme, il client e il server DNS offrono ai computer e agli utenti servizi di risoluzione del nome tramite il mapping tra nome computer e indirizzo IP.

### <a name="dynamic-host-configuration-protocol-40dhcp41technologiesdhcpdhcp-topmd"></a>[Dynamic Host Configuration Protocol &#40;DHCP&#41;](technologies/dhcp/dhcp-top.md)

DHCP \(Dynamic Host Configuration Protocol\) è un protocollo client/server che fornisce automaticamente un host IP \(Internet Protocol\) con l'indirizzo IP corrispondente e altre informazioni di configurazione correlate, ad esempio la subnet mask e il gateway predefinito.

### <a name="hyper-v-network-virtualizationsdntechnologieshyper-v-network-virtualizationhyper-v-network-virtualizationmd"></a>[Virtualizzazione rete Hyper-V](sdn/technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)

Virtualizzazione rete Hyper–V \(HNV\) consente la virtualizzazione di reti di clienti in un'infrastruttura di rete fisica condivisa.

### <a name="hyper-v-virtual-switchvirtualizationhyper-v-virtual-switchhyper-v-virtual-switchmd"></a>[Commutatore virtuale Hyper-V](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)

Il commutatore virtuale Hyper-V è un commutatore di rete Ethernet di livello 2 basato sul software, disponibile nella console di gestione di Hyper-V quando si installa il ruolo del server Hyper-V. Il commutatore include funzionalità estendibili e gestibili a livello di programmazione, per la connessione delle macchine virtuali a una rete fisica o virtuale. Il commutatore virtuale Hyper-V supporta inoltre l'imposizione di criteri per sicurezza, l'isolamento e i livelli di servizio. 

La documentazione del commutatore virtuale Hyper-V è ora disponibile nella sezione **Virtualizzazione** del sommario di Windows Server 2016. Per ulteriori informazioni, vedere [commutatore virtuale Hyper-V](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).

### <a name="ip-address-management-40ipam41technologiesipamipam-topmd"></a>[Gestione indirizzi IP &#40;gestione indirizzi IP&#41;](technologies/ipam/ipam-top.md)

Gestione indirizzi IP \(IPAM\) è una suite integrata di strumenti che consentono la pianificazione end-to-end, distribuzione, la gestione e monitoraggio dell'infrastruttura di indirizzi IP, con un'esperienza utente avanzata. Gestione indirizzi IP individua automaticamente i server dell'infrastruttura di indirizzi IP e DNS \(DNS\) server sulla rete e consente di gestirli da un'interfaccia centrale.

### <a name="network-load-balancingtechnologiesnetwork-load-balancingmd"></a>[Bilanciamento carico di rete](technologies/Network-Load-Balancing.md)

Bilanciamento carico di rete \(NLB\) distribuisce il traffico tra più server usando il protocollo di rete TCP/IP. Per le distribuzioni non SDN, NLB garantisce che le applicazioni senza stato, ad esempio i server Web che eseguono Internet Information Services \(IIS\) siano scalabili con l'aggiunta di altri server man mano che il carico aumenta.

### <a name="high-performance-networkingtechnologieshpnhpn-topmd"></a>[Rete ad alte prestazioni](technologies/hpn/hpn-top.md)

Le tecnologie di ottimizzazione e offload di rete in Windows Server 2016 includono funzionalità e tecnologie per solo software (SO, Software Only), funzionalità e tecnologie per software e hardware (SH, Software e Hardware) integrate e funzionalità e tecnologie per solo hardware (HO, Hardware Only).

È anche disponibile la seguente documentazione sulle tecnologie di ottimizzazione e offload di rete.

- [Guida alla configurazione di rete convergente interfaccia scheda (NIC)](technologies/conv-nic/cnic-top.md)
- [Data Center Bridging (DCB)](technologies/dcb/dcb-top.md)
- [Virtual Receive Side Scaling (vRSS)](technologies/vrss/vrss-top.md)


### <a name="network-policy-servertechnologiesnpsnps-topmd"></a>[Server dei criteri di rete](technologies/nps/nps-top.md)

Server dei criteri di rete consente di creare e imporre criteri di accesso di rete a livello di organizzazione per l'autorizzazione e l'autenticazione delle richieste di connessione.

### <a name="network-shell-netshtechnologiesnetshnetshmd"></a>[Network Shell (Netsh)](technologies/netsh/netsh.md)

È possibile usare la Shell di rete \(netsh\) networking utilità per gestire tecnologie di rete in Windows Server 2016 e Windows 10.

### <a name="network-subsystem-performance-tuningtechnologiesnetwork-subsystemnet-sub-performance-topmd"></a>[Le prestazioni del sottosistema di rete di ottimizzazione](technologies/network-subsystem/net-sub-performance-top.md)

Questo argomento vengono fornite informazioni sulla scelta la scheda di rete corretto per il carico di lavoro server, le interfacce di rete di ordinamento, i contatori delle prestazioni relativi alla rete e ottimizzazione delle prestazioni schede di rete e le tecnologie di rete correlate, ad esempio Receive-Side Scaling \(RSS\), ricevere Side Coalescing \(RSC\)e così via.

### <a name="nic-teamingtechnologiesnic-teamingnic-teamingmd"></a>[Gruppo NIC](technologies/nic-teaming/NIC-Teaming.md)

Gruppo NIC consente di raggruppare le schede di rete Ethernet fisiche in una o più schede di rete virtuali basate su software. Queste schede di rete virtuali garantiscono prestazioni elevate e tolleranza di errore in caso di errore delle schede di rete.

### <a name="quality-of-service-qos-policytechnologiesqosqos-policy-topmd"></a>[Qualità del servizio (QoS) dei criteri](technologies/qos/qos-policy-top.md)

Puoi utilizzare i criteri QoS come un punto centrale per la gestione della larghezza di banda di rete tra l'intera infrastruttura di Active Directory creando profili QoS, le cui impostazioni vengono distribuite con Criteri di gruppo.

### <a name="remote-accessremoteremote-accessremote-accessmd"></a>[Accesso remoto](../remote/remote-access/remote-access.md)

È possibile usare tecnologie di accesso remoto, ad esempio DirectAccess e la rete privata virtuale \(VPN\) per fornire ai lavoratori in remoto con la connettività alle risorse di rete interna. Inoltre, è possibile usare l'accesso remoto per la rete locale \(LAN\) routing e per il Proxy applicazione Web. che rende disponibili funzionalità di proxy inverso per le applicazioni Web all'interno della rete aziendale, in modo da consentire agli utenti con qualsiasi dispositivo di accedere a tali applicazioni dall'esterno della rete aziendale.

La documentazione relativa ad Accesso remoto è ora disponibile nella sezione [Gestione di accesso remoto e server](https://docs.microsoft.com/windows-server/remote/) nel sommario di Windows Server 2016, in Accesso remoto. Per altre informazioni, vedi [Accesso remoto](../remote/remote-access/remote-access.md).

Per ulteriori informazioni su Proxy applicazione Web, che è un servizio ruolo del ruolo del server Accesso remoto, vedi [Proxy applicazione Web in Windows Server 2016](https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/web-application-proxy-windows-server).

### <a name="virtual-private-networking-vpnremoteremote-accessvpnvpn-topmd"></a>[Rete privata virtuale (VPN)](../remote/remote-access/vpn/vpn-top.md)

In Windows Server 2016 **DirectAccess e VPN** è ora un servizio del ruolo del server **Accesso remoto**.

Quando si installa accesso remoto come server VPN, è possibile usare la rete privata virtuale \(VPN\) per fornire i dipendenti remoti con le connessioni alla rete dell'organizzazione attraverso la rete Internet -, garantendo contemporaneamente informazioni privacy con le connessioni crittografate.

Con VPN per Accesso remoto di Windows Server 2016 e computer client Windows 10 puoi ora distribuire VPN Always On. Una VPN Always On ti consente di gestire i client VPN remoti sempre connessi, agevolando al contempo i lavoratori remoti, che non dovranno più connettersi e disconnettersi manualmente dalla VPN alla rete dell'organizzazione.

Per ulteriori informazioni, vedi la [Guida per Windows Server 2016 e Windows 10 relativa alla distribuzione di VPN Always On per Accesso remoto](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy).

>[!NOTE]
>La documentazione di VPN è ora disponibile nella sezione [Gestione di accesso remoto e server](https://docs.microsoft.com/windows-server/remote/) nel sommario di Windows Server 2016, in [Accesso remoto](https://docs.microsoft.com/windows-server/remote/remote-access/remote-access).

Per ulteriori informazioni su VPN, vedi [Reti private virtuali (VPN)](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/vpn-top).

### <a name="windows-container-networkinghttpsdocsmicrosoftcomvirtualizationwindowscontainersmanage-containerscontainer-networking"></a>[Rete di contenitori di Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/container-networking)

Rete di contenitori di Windows ti consente di creare e gestire le reti per la connessione di endpoint contenitore su host Windows 10 e Windows Server, utilizzando flussi di lavoro e strumenti standard di settore. Le reti di contenitori di Windows supportano più topologie, tra cui reti private, L2 piatte e L3 reindirizzate.

Sono inoltre supportati che è possibile creare usando Docker, Kubernetes o Windows PowerShell tramite i plug-in grado di comunicare con il servizio di rete Host di Windows, in locale nell'host sovrimpressioni \(HNS\). È possibile creare e gestire più\-reti di cluster di nodi tramite sistemi di orchestrazione di livello superiore mediante la comunicazione tramite un agente locale per HNS ogni nodo.

### <a name="windows-internet-name-service-winstechnologieswinswins-topmd"></a>[Windows Internet Name Service (WINS)](technologies/wins/wins-top.md)

WINS (Windows Internet Name Service) è un servizio legacy di registrazione e risoluzione dei nomi di computer che esegue il mapping dei nomi NetBIOS dei computer a indirizzi IP. È consigliabile utilizzare DNS rispetto a WINS.

## <a name="additional-resources"></a>Risorse aggiuntive

Le risorse sulla rete per i sistemi operativi precedenti a Windows Server 2016 sono disponibili negli articoli seguenti.

- [Panoramica delle reti](https://technet.microsoft.com/library/hh831357.aspx) di Windows Server 2012 e Windows Server 2012 R2
- Windows Server 2008 and Windows Server 2008 R2 [Networking](https://technet.microsoft.com/library/cc753940) (Funzionalità di rete di Windows Server 2008 e Windows Server 2008 R2)
- [Contenuti ritirati relativi a Windows Server 2003/2003 R2 ](https://www.microsoft.com/download/details.aspx?id=53314) - Windows Server 2003
