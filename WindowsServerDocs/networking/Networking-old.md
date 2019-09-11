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
ms.openlocfilehash: cf0aa1d752167962c11171ad98ae26591a740221
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868994"
---
# <a name="networking"></a>Rete

>Si applica a Windows Server (Canale semestrale), Windows Server 2016

>[!TIP]
> Cerchi informazioni sull'installazione delle versioni precedenti di Windows Server? vedere le altre [librerie di Windows Server](/previous-versions/windows/) in docs.microsoft.com. Puoi anche [cercare nel sito](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) informazioni specifiche.

<img src="../media/landing-icons/network.png" style='float:left; padding:.5em;' alt="Icon depicting two networked computers"> La funzionalità di rete è una parte fondamentale della piattaforma software defined \(datacenter SDDC\) e Windows Server 2016 fornisce tecnologie Sdn\) nuove e migliorate \(di Software Defined Networking per facilitare il passaggio a una soluzione SDDC completamente realizzata per la tua organizzazione.

Quando si gestiscono le reti come risorsa definita dal software, è possibile descrivere i requisiti dell'infrastruttura di un'applicazione una sola volta e quindi scegliere la posizione in cui viene eseguita l'applicazione, in locale o nel cloud. 

Grazie a questa coerenza, le applicazioni sono ora più scalabili e possono essere eseguite senza problemi ovunque e sempre con lo stesso livello di sicurezza, prestazioni, qualità del servizio e disponibilità.

>[!Note]
> Per scaricare Windows Server, vedi la pagina delle [versioni di valutazione di Windows Server](https://www.microsoft.com/evalcenter/evaluate-windows-server-technical-preview).

In Windows Server 2016 sono disponibili le nuove tecnologie di rete seguenti:

- Software Defined Networking: Controller di rete rappresenta un punto di automazione programmabile e centralizzato per gestire, configurare e monitorare l'infrastruttura di rete fisica e virtuale nel centro dati e di risolverne gli eventuali problemi. Il controller di rete consente di usare la \(virtualizzazione \(delle funzioni di rete per\) distribuire facilmente le VM delle macchine\) virtuali per il bilanciamento del carico software SLB per ottimizzare il carico del traffico di rete per i tenant e RAS Gateway per fornire ai tenant le opzioni di connettività necessarie tra Internet, locale e le risorse cloud. È anche possibile usare Controller di rete per gestire il firewall del centro dati su macchine virtuali e host Hyper–V.

- Piattaforma di rete: Usando le nuove funzionalità per le tecnologie esistenti della piattaforma di rete, è possibile usare i criteri DNS per personalizzare le risposte del server DNS alle query, usare una scheda di interfaccia di rete \(convergente che gestisce il traffico RDMA\) e Ethernet con accesso diretto a memoria remota combinato usare switch Embedded Teaming \(set\) per creare commutatori virtuali Hyper-V connessi a schede di rete RDMA e\) usare \(gestione indirizzi IP per gestire le zone DNS e i server, nonché gli indirizzi IP e DHCP.

Per altre informazioni, vedi [Scenari di rete supportati in Windows Server](windows-server-supported-networking-scenarios.md).

Le sezioni seguenti forniscono informazioni sulle tecnologie SDN e di piattaforma di rete.

## <a name="software-defined-networking-technologies"></a>Tecnologie di Software Defined Networking

### <a name="software-defined-networking-40sdn41sdnsoftware-defined-networkingmd"></a>[Software Defined Networking &#40;Sdn&#41;](sdn/software-defined-networking.md)

Questo argomento contiene informazioni sulle tecnologie SDN disponibili in Windows Server, System Center e Microsoft Azure.

>[!NOTE]
>Per gli host Hyper-V e le \(macchine\) virtuali che eseguono i server di infrastruttura Sdn, ad esempio i nodi controller di rete e bilanciamento del carico software, è necessario installare Windows Server 2016 Datacenter Edition. Per gli host Hyper-V che contengono solo macchine virtuali del carico di lavoro tenant\-connesse a reti controllate da Sdn, è possibile eseguire Windows Server 2016 Standard Edition.

### <a name="deploy-a-software-defined-network-infrastructure-using-scriptssdndeploydeploy-a-software-defined-network-infrastructure-using-scriptsmd"></a>[Distribuire un'infrastruttura software defined Network usando gli script](sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)
Questa guida offre alcune istruzioni per la distribuzione di Controller di rete con le reti e i gateway virtuali in un lab di test.

### <a name="network-controllersdntechnologiesnetwork-controllernetwork-controllermd"></a>[Controller di rete](sdn/technologies/network-controller/Network-Controller.md)

Controller di rete rappresenta un punto di automazione programmabile e centralizzato per gestire, configurare e monitorare l'infrastruttura di rete fisica e virtuale nel centro dati e di risolverne gli eventuali problemi.

### <a name="software-load-balancing-40slb41-for-sdnsdntechnologiesnetwork-function-virtualizationsoftware-load-balancing-for-sdnmd"></a>[Bilanciamento del carico software &#40;SLB&#41; per Sdn](sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md)

I provider \(di servizi\) cloud CSP e le aziende che distribuiscono software defined Networking (SDN) in Windows Server 2016 possono \(usare il bilanciamento\) del carico software SLB per distribuire uniformemente tenant e tenant traffico di rete del cliente tra le risorse della rete virtuale. Windows Server SLB consente di abilitare più server per l'hosting dello stesso carico di lavoro, offrendo disponibilità e scalabilità elevate.

### <a name="ras-gateway-for-sdnsdntechnologiesnetwork-function-virtualizationras-gateway-for-sdnmd"></a>[Gateway RAS per SDN](sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)

Gateway RAS, ovvero un router basato su software, multi-tenant Border Gateway Protocol \(con\) supporto BGP in Windows Server 2016, è progettato per i provider \(di servizi\) cloud CSP e le aziende che ospitano più reti virtuali tenant che usano la virtualizzazione rete Hyper-V. 

### <a name="network-function-virtualizationsdntechnologiesnetwork-function-virtualizationnetwork-function-virtualizationmd"></a>[Virtualizzazione delle funzioni di rete](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md)

Nei data center definiti dal software, le funzioni di rete eseguite da appliance \(hardware quali i bilanciamenti del carico, i firewall, i router, i commutatori e così via\) sono sempre più virtualizzate come appliance virtuali. Questo "virtualizzazione delle funzioni di rete" è una progressione naturale di virtualizzazione di server e virtualizzazione di rete.

### <a name="datacenter-firewall-overviewsdntechnologiesnetwork-function-virtualizationdatacenter-firewall-overviewmd"></a>[Panoramica del firewall del data center](sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)

Il firewall del centro dati è un firewall multi–tenant, con stato, a 5 tuple (protocollo, numeri di porta di origine e destinazione, indirizzi IP di origine e destinazione), a livello rete.

## <a name="bkmk_networking"></a>Tecnologie di rete

La tabella seguente mette a disposizione i collegamenti ad alcune tecnologie di rete in Windows Server 2016.

### <a name="whats-new-in-networkingnetworkingwhat-s-new-in-networkingmd"></a>[Novità relative alla rete](../networking/What-s-New-in-Networking.md)

Nelle seguenti sezioni puoi trovare informazioni sulle nuove tecnologie di rete e sulle nuove funzionalità per le tecnologie esistenti in Windows Server 2016.

### <a name="branchcachebranchcachebranchcachemd"></a>[BranchCache](branchcache/BranchCache.md)

BranchCache è una tecnologia \(di\) ottimizzazione della larghezza di banda Wide Area Network WAN. Per ottimizzare la larghezza di banda della rete WAN quando gli utenti accedono al contenuto di server remoti, BranchCache recupera il contenuto dai server di contenuti della sede centrale o ospitati nel cloud e lo memorizza nella cache presso le succursali, consentendo ai computer client delle succursali stesse di accedere al contenuto in locale anziché attraverso la rete WAN.

### <a name="core-network-guide-for-windows-server-2016core-network-guidecore-network-guide-windows-servermd"></a>[Guida alla rete core per Windows Server 2016](core-network-guide/core-network-guide-windows-server.md)

Scopri come distribuire una rete di Windows Server con la Guida alla rete core e aggiungere funzionalità alla distribuzione di rete con le Guide complementari alla rete core.

### <a name="directaccessremoteremote-accessdirectaccessdirectaccessmd"></a>[DirectAccess](../remote/remote-access/directaccess/DirectAccess.md)

DirectAccess consente agli utenti remoti di connettersi alle risorse di rete dell'organizzazione. 

La documentazione di DirectAccess è ora disponibile nella sezione [Gestione di accesso remoto e server](https://docs.microsoft.com/windows-server/remote/) nel sommario di Windows Server 2016, in [Accesso remoto](https://docs.microsoft.com/windows-server/remote/remote-access/remote-access). Per altre informazioni, vedi [DirectAccess](../remote/remote-access/directaccess/DirectAccess.md).

### <a name="domain-name-system-40dns41dnsdns-topmd"></a>[DNS &#40;Domain Name System&#41;](dns/dns-top.md)

DNS \(Domain Name System\) è una suite di protocolli standard di settore che comprende il protocollo TCP/IP. Insieme, il client e il server DNS offrono ai computer e agli utenti servizi di risoluzione del nome tramite il mapping tra nome computer e indirizzo IP.

### <a name="dynamic-host-configuration-protocol-40dhcp41technologiesdhcpdhcp-topmd"></a>[Dynamic Host Configuration Protocol &#40;DHCP&#41;](technologies/dhcp/dhcp-top.md)

DHCP \(Dynamic Host Configuration Protocol\) è un protocollo client/server che fornisce automaticamente un host IP \(Internet Protocol\) con l'indirizzo IP corrispondente e altre informazioni di configurazione correlate, ad esempio la subnet mask e il gateway predefinito.

### <a name="hyper-v-network-virtualizationsdntechnologieshyper-v-network-virtualizationhyper-v-network-virtualizationmd"></a>[Virtualizzazione rete Hyper-V](sdn/technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)

Virtualizzazione rete Hyper–V \(HNV\) consente la virtualizzazione di reti di clienti in un'infrastruttura di rete fisica condivisa.

### <a name="hyper-v-virtual-switchvirtualizationhyper-v-virtual-switchhyper-v-virtual-switchmd"></a>[Commutatore virtuale Hyper-V](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)

Il commutatore virtuale Hyper-V è un commutatore di rete Ethernet di livello 2 basato sul software, disponibile nella console di gestione di Hyper-V quando si installa il ruolo del server Hyper-V. Il commutatore include funzionalità estendibili e gestibili a livello di programmazione, per la connessione delle macchine virtuali a una rete fisica o virtuale. Il commutatore virtuale Hyper-V supporta inoltre l'imposizione di criteri per sicurezza, l'isolamento e i livelli di servizio. 

La documentazione del commutatore virtuale Hyper-V è ora disponibile nella sezione **Virtualizzazione** del sommario di Windows Server 2016. Per ulteriori informazioni, vedere [commutatore virtuale Hyper-V](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).

### <a name="ip-address-management-40ipam41technologiesipamipam-topmd"></a>[Gestione indirizzi IP &#40;gestione indirizzi IP&#41;](technologies/ipam/ipam-top.md)

\(Gestione\) indirizzi IP è una suite integrata di strumenti che consentono la pianificazione, la distribuzione, la gestione e il monitoraggio end-to-end dell'infrastruttura di indirizzi IP, con un'esperienza utente avanzata. Gestione indirizzi IP individua automaticamente i server dell'infrastruttura degli indirizzi \(IP\) e Domain Name System i server DNS nella rete e consente di gestirli da un'interfaccia centrale.

### <a name="network-load-balancingtechnologiesnetwork-load-balancingmd"></a>[Bilanciamento del carico di rete](technologies/Network-Load-Balancing.md)

Bilanciamento carico di rete \(NLB\) distribuisce il traffico tra più server usando il protocollo di rete TCP/IP. Per le distribuzioni non SDN, NLB garantisce che le applicazioni senza stato, ad esempio i server Web che eseguono Internet Information Services \(IIS\) siano scalabili con l'aggiunta di altri server man mano che il carico aumenta.

### <a name="high-performance-networkingtechnologieshpnhpn-topmd"></a>[Rete ad alte prestazioni](technologies/hpn/hpn-top.md)

Le tecnologie di ottimizzazione e offload di rete in Windows Server 2016 includono funzionalità e tecnologie per solo software (SO, Software Only), funzionalità e tecnologie per software e hardware (SH, Software e Hardware) integrate e funzionalità e tecnologie per solo hardware (HO, Hardware Only).

È anche disponibile la seguente documentazione sulle tecnologie di ottimizzazione e offload di rete.

- [Guida alla configurazione della scheda di interfaccia di rete (NIC) convergente](technologies/conv-nic/cnic-top.md)
- [Data Center Bridging (DCB)](technologies/dcb/dcb-top.md)
- [Virtual Receive Side Scaling (vRSS)](technologies/vrss/vrss-top.md)


### <a name="network-policy-servertechnologiesnpsnps-topmd"></a>[Server dei criteri di rete](technologies/nps/nps-top.md)

Server dei criteri di rete consente di creare e imporre criteri di accesso di rete a livello di organizzazione per l'autorizzazione e l'autenticazione delle richieste di connessione.

### <a name="network-shell-netshtechnologiesnetshnetshmd"></a>[Shell di rete (Netsh)](technologies/netsh/netsh.md)

Per gestire le tecnologie di rete \(in\) Windows Server 2016 e Windows 10, è possibile usare l'utilità di rete della shell di rete Netsh.

### <a name="network-subsystem-performance-tuningtechnologiesnetwork-subsystemnet-sub-performance-topmd"></a>[Ottimizzazione delle prestazioni del sottosistema di rete](technologies/network-subsystem/net-sub-performance-top.md)

In questo argomento vengono fornite informazioni sulla scelta della scheda di rete adatta per il carico di lavoro del server, dell'ordinamento delle interfacce di rete, dei contatori delle prestazioni correlati alla rete e delle schede di rete di ottimizzazione delle prestazioni e delle tecnologie di rete Receive-Side \(scaling\)RSS \(, Receive-Side coalesing RSC\)e altri.

### <a name="nic-teamingtechnologiesnic-teamingnic-teamingmd"></a>[Gruppo NIC](technologies/nic-teaming/NIC-Teaming.md)

Gruppo NIC consente di raggruppare le schede di rete Ethernet fisiche in una o più schede di rete virtuali basate su software. Queste schede di rete virtuali garantiscono prestazioni elevate e tolleranza di errore in caso di errore delle schede di rete.

### <a name="quality-of-service-qos-policytechnologiesqosqos-policy-topmd"></a>[Criteri di qualità del servizio (QoS)](technologies/qos/qos-policy-top.md)

Puoi utilizzare i criteri QoS come un punto centrale per la gestione della larghezza di banda di rete tra l'intera infrastruttura di Active Directory creando profili QoS, le cui impostazioni vengono distribuite con Criteri di gruppo.

### <a name="remote-accessremoteremote-accessremote-accessmd"></a>[Accesso remoto](../remote/remote-access/remote-access.md)

È possibile utilizzare le tecnologie di accesso remoto, ad esempio DirectAccess e VPN \(\) di rete privata virtuale, per fornire agli utenti remoti la connettività alle risorse di rete interne. Inoltre, è possibile utilizzare l'accesso remoto per il routing LAN \(\) della rete locale e per il proxy dell'applicazione Web. che rende disponibili funzionalità di proxy inverso per le applicazioni Web all'interno della rete aziendale, in modo da consentire agli utenti con qualsiasi dispositivo di accedere a tali applicazioni dall'esterno della rete aziendale.

La documentazione relativa ad Accesso remoto è ora disponibile nella sezione [Gestione di accesso remoto e server](https://docs.microsoft.com/windows-server/remote/) nel sommario di Windows Server 2016, in Accesso remoto. Per altre informazioni, vedi [Accesso remoto](../remote/remote-access/remote-access.md).

Per ulteriori informazioni su Proxy applicazione Web, che è un servizio ruolo del ruolo del server Accesso remoto, vedi [Proxy applicazione Web in Windows Server 2016](https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/web-application-proxy-windows-server).

### <a name="virtual-private-networking-vpnremoteremote-accessvpnvpn-topmd"></a>[Rete privata virtuale (VPN)](../remote/remote-access/vpn/vpn-top.md)

In Windows Server 2016 **DirectAccess e VPN** è ora un servizio del ruolo del server **Accesso remoto**.

Quando si installa accesso remoto come server VPN, è possibile usare la VPN \(\) di rete privata virtuale per fornire ai dipendenti remoti le connessioni alla rete aziendale tramite Internet, mantenendo al tempo stesso le informazioni privacy con connessioni crittografate.

Con VPN per Accesso remoto di Windows Server 2016 e computer client Windows 10 puoi ora distribuire VPN Always On. Una VPN Always On ti consente di gestire i client VPN remoti sempre connessi, agevolando al contempo i lavoratori remoti, che non dovranno più connettersi e disconnettersi manualmente dalla VPN alla rete dell'organizzazione.

Per ulteriori informazioni, vedi la [Guida per Windows Server 2016 e Windows 10 relativa alla distribuzione di VPN Always On per Accesso remoto](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy).

>[!NOTE]
>La documentazione di VPN è ora disponibile nella sezione [Gestione di accesso remoto e server](https://docs.microsoft.com/windows-server/remote/) nel sommario di Windows Server 2016, in [Accesso remoto](https://docs.microsoft.com/windows-server/remote/remote-access/remote-access).

Per ulteriori informazioni su VPN, vedi [Reti private virtuali (VPN)](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/vpn-top).

### <a name="windows-container-networkinghttpsdocsmicrosoftcomvirtualizationwindowscontainersmanage-containerscontainer-networking"></a>[Rete di contenitori di Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/container-networking)

Rete di contenitori di Windows ti consente di creare e gestire le reti per la connessione di endpoint contenitore su host Windows 10 e Windows Server, utilizzando flussi di lavoro e strumenti standard di settore. Le reti di contenitori di Windows supportano più topologie, tra cui reti private, L2 piatte e L3 reindirizzate.

Sono supportate anche le sovrimpressioni che è possibile creare localmente nell'host usando Docker, Kubernetes o Windows PowerShell tramite i plug-in che comunicano con il servizio \(di\)rete host Windows HNS. È possibile creare e gestire\-reti cluster a più nodi tramite sistemi di orchestrazione di livello superiore comunicando tramite un agente locale al servizio HNS di ciascun nodo.

### <a name="windows-internet-name-service-winstechnologieswinswins-topmd"></a>[Windows Internet Name Service (WINS)](technologies/wins/wins-top.md)

WINS (Windows Internet Name Service) è un servizio legacy di registrazione e risoluzione dei nomi di computer che esegue il mapping dei nomi NetBIOS dei computer a indirizzi IP. È consigliabile utilizzare DNS rispetto a WINS.

## <a name="additional-resources"></a>Risorse aggiuntive

Le risorse sulla rete per i sistemi operativi precedenti a Windows Server 2016 sono disponibili negli articoli seguenti.

- [Panoramica delle reti](https://technet.microsoft.com/library/hh831357.aspx) di Windows Server 2012 e Windows Server 2012 R2
- Windows Server 2008 and Windows Server 2008 R2 [Networking](https://technet.microsoft.com/library/cc753940) (Funzionalità di rete di Windows Server 2008 e Windows Server 2008 R2)
- [Contenuti ritirati di Windows server 2003 Windows server 2003/2003 R2](https://www.microsoft.com/download/details.aspx?id=53314)
