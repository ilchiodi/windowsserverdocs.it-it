---
title: Reti
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
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "6066841"
---
# Reti

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

>[!TIP]
> Cerchi informazioni sull'installazione delle versioni precedenti di Windows Server? Vedi le nostre altre [librerie di Windows Server](/previous-versions/windows/) in docs.microsoft.com. Puoi anche [cercare informazioni specifiche nel sito](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions).

<img src="../media/landing-icons/network.png" style='float:left; padding:.5em;' alt="Icon depicting two networked computers"> Le reti rappresentano una parte fondamentale della piattaforma Software Defined Datacenter \(SDDC\) e Windows Server 2016 offre tecnologie Software Defined Networking \(SDN\) nuove e migliorate che consentono di passare a una soluzione SDDC completamente realizzata per l'organizzazione.

Quando si gestiscono le reti come risorse software–defined, è possibile descrivere i requisiti di un'applicazione relativi all'infrastruttura una sola volta e quindi scegliere dove eseguire l'applicazione, se in locale o nel cloud. 

Grazie a questa coerenza, le applicazioni sono ora più scalabili e possono essere eseguite senza problemi ovunque e sempre con lo stesso livello di sicurezza, prestazioni, qualità del servizio e disponibilità.

>[!Note]
> Per scaricare Windows Server, vedi la pagina delle [versioni di valutazione di Windows Server](https://www.microsoft.com/evalcenter/evaluate-windows-server-technical-preview).

In Windows Server 2016 sono disponibili le nuove tecnologie di rete seguenti:

- Software Defined Networking: Controller di rete rappresenta un punto di automazione programmabile e centralizzato per gestire, configurare e monitorare l'infrastruttura di rete fisica e virtuale nel centro dati e di risolverne gli eventuali problemi. Controller di rete consente di usare la virtualizzazione delle funzioni di rete per distribuire facilmente le macchine virtuali \(VM, Virtual Machine\) per Software Load Balancing \(SLB\) per ottimizzare i carichi di traffico di rete per i tenant. Consente anche di usare gateway RAS per garantire ai tenant le opzioni di connettività necessarie tra le risorse in Internet, locali e nel cloud. È anche possibile usare Controller di rete per gestire il firewall del centro dati su macchine virtuali e host Hyper–V.

- Piattaforma di rete: tramite nuove funzionalità per le tecnologie di piattaforma di rete esistenti, è possibile usare criteri DNS per personalizzare le risposte del server DNS alle query, usare una scheda di interfaccia di rete convergente in grado di gestire il traffico Ethernet e RDMA \(Accesso diretto a memoria remota\), usare la tecnologia Switch Embedded Teaming \(SET\) per creare commutatori virtuali Hyper-V connessi a schede di interfaccia di rete RDMA e usare Gestione indirizzi IP per gestire zone e server DNS, nonché indirizzi IP e DHCP.

Per altre informazioni, vedi [Scenari di rete supportati in Windows Server](windows-server-supported-networking-scenarios.md).

Le sezioni seguenti forniscono informazioni sulle tecnologie SDN e di piattaforma di rete.

## Tecnologie Software Defined Networking

### [Software Defined Networking &#40;SDN&#41;](sdn/software-defined-networking.md)

Questo argomento contiene informazioni sulle tecnologie SDN disponibili in Windows Server, System Center e Microsoft Azure.

>[!NOTE]
>Per gli host e le macchine virtuali \(VM\) Hyper-V che eseguono server di infrastruttura SDN, ad esempio i nodi Software Load Balancing e Controller di rete, devi installare Windows Server 2016 Datacenter. Per gli host Hyper-V che contengono solo macchine virtuali con carico di lavoro tenant connessi a reti controllate tramite SDN, puoi eseguire Windows Server 2016 Standard.

### [Distribuire un'infrastruttura Software Defined Network usando gli script](sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)
Questa guida offre alcune istruzioni per la distribuzione di Controller di rete con le reti e i gateway virtuali in un lab di test.

### [Controller di rete](sdn/technologies/network-controller/Network-Controller.md)

Controller di rete rappresenta un punto di automazione programmabile e centralizzato per gestire, configurare e monitorare l'infrastruttura di rete fisica e virtuale nel centro dati e di risolverne gli eventuali problemi.

### [Software Load Balancing &#40;SLB&#41; per SDN](sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md)

I provider di servizi cloud \(CSP, Cloud Service Provider\) e le aziende che distribuiscono reti SDN (Software Defined Networking) in Windows Server 2016 possono usare Software Load Balancing \(SLB\) per SDN per distribuire uniformemente il traffico di rete del tenant e dei clienti di questo tra le risorse della rete virtuale. Windows Server SLB consente di abilitare più server per l'hosting dello stesso carico di lavoro, offrendo disponibilità e scalabilità elevate.

### [Gateway RAS per SDN](sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)

Un gateway RAS, ovvero un router basato su software, multi-tenant, idoneo per BGP \(Border Gateway Protocol\) in Windows Server 2016, è destinato ai provider di servizi cloud \(CSP\) e alle aziende che ospitano più reti virtuali tenant tramite Virtualizzazione rete Hyper-V. 

### [Virtualizzazione delle funzioni di rete](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md)

Nei centri dati software-defined le funzioni di rete eseguite da appliance hardware \(ad esempio servizi di bilanciamento del carico, firewall, router, commutatori e così via\) vengono distribuite sempre più spesso come appliance virtuali. Questo "virtualizzazione delle funzioni di rete" è una progressione naturale di virtualizzazione di server e virtualizzazione di rete.

### [Informazioni generali sul firewall del centro dati](sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)

Il firewall del centro dati è un firewall multi–tenant, con stato, a 5 tuple (protocollo, numeri di porta di origine e destinazione, indirizzi IP di origine e destinazione), a livello rete.

## <a name="bkmk_networking"></a>Tecnologie di rete

La tabella seguente mette a disposizione i collegamenti ad alcune tecnologie di rete in Windows Server 2016.

### [Novità relative alle funzionalità di rete](../networking/What-s-New-in-Networking.md)

Nelle seguenti sezioni puoi trovare informazioni sulle nuove tecnologie di rete e sulle nuove funzionalità per le tecnologie esistenti in Windows Server 2016.

### [BranchCache](branchcache/BranchCache.md)

BranchCache è una tecnologia di ottimizzazione della larghezza di banda di per le reti WAN \(Wide Area Network\). Per ottimizzare la larghezza di banda della rete WAN quando gli utenti accedono al contenuto di server remoti, BranchCache recupera il contenuto dai server di contenuti della sede centrale o ospitati nel cloud e lo memorizza nella cache presso le succursali, consentendo ai computer client delle succursali stesse di accedere al contenuto in locale anziché attraverso la rete WAN.

### [Guida alla rete core per Windows Server 2016](core-network-guide/core-network-guide-windows-server.md)

Scopri come distribuire una rete di Windows Server con la Guida alla rete core e aggiungere funzionalità alla distribuzione di rete con le Guide complementari alla rete core.

### [DirectAccess](../remote/remote-access/directaccess/DirectAccess.md)

DirectAccess consente agli utenti remoti di connettersi alle risorse di rete dell'organizzazione. 

La documentazione di DirectAccess è ora disponibile nella sezione [Gestione di accesso remoto e server](https://docs.microsoft.com/windows-server/remote/) nel sommario di Windows Server 2016, in [Accesso remoto](https://docs.microsoft.com/windows-server/remote/remote-access/remote-access). Per altre informazioni, vedi [DirectAccess](../remote/remote-access/directaccess/DirectAccess.md).

### [DNS &#40;Domain Name System&#41;](dns/dns-top.md)

Domain Name System \(DNS\) è una suite di protocolli standard di settore che comprende il protocollo TCP/IP. Insieme, il client e il server DNS offrono ai computer e agli utenti servizi di risoluzione del nome tramite il mapping tra nome computer e indirizzo IP.

### [DHCP &#40;Dynamic Host Configuration Protocol&#41;](technologies/dhcp/dhcp-top.md)

Dynamic Host Configuration Protocol \(DHCP\) è un protocollo client/server che fornisce automaticamente un host IP \(Internet Protocol\) con l'indirizzo IP e altre informazioni di configurazione correlati, ad esempio la subnet mask e il gateway predefinito.

### [Virtualizzazione rete Hyper-V](sdn/technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)

Virtualizzazione rete Hyper-V \(HNV\) consente la virtualizzazione di reti clienti in un'infrastruttura di rete fisica condivisa.

### [Commutatore virtuale Hyper-V](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)

Il commutatore virtuale Hyper-V è un commutatore di rete Ethernet di livello 2 basato sul software, disponibile nella console di gestione di Hyper-V quando si installa il ruolo del server Hyper-V. Il commutatore include funzionalità estendibili e gestibili a livello di programmazione, per la connessione delle macchine virtuali a una rete fisica o virtuale. Il commutatore virtuale Hyper-V supporta inoltre l'imposizione di criteri per sicurezza, l'isolamento e i livelli di servizio. 

La documentazione del commutatore virtuale Hyper-V è ora disponibile nella sezione **Virtualizzazione** del sommario di Windows Server 2016. Per ulteriori informazioni, vedi [Commutatore virtuale Hyper-V](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).

### [Gestione indirizzi IP](technologies/ipam/ipam-top.md)

Gestione indirizzi IP è una suite integrata di strumenti che consentono la pianificazione, la distribuzione, la gestione e il monitoraggio end-to-end dell'infrastruttura di indirizzi IP, con un'esperienza utente avanzata. Gestione indirizzi IP individua automaticamente i server dell'infrastruttura di indirizzi IP e i server DNS \(Domain Name System\) nella rete e consente di gestirli da un'interfaccia centrale.

### [Bilanciamento carico di rete](technologies/Network-Load-Balancing.md)

Bilanciamento carico di rete distribuisce il traffico tra più server usando il protocollo di rete TCP/IP. Per le distribuzioni non SDN, Bilanciamento carico di rete garantisce che le applicazioni senza stato, ad esempio i server Web che eseguono Internet Information Services \(IIS\) siano scalabili con l'aggiunta di altri server man mano che il carico aumenta.

### [Reti ad alte prestazioni](technologies/hpn/hpn-top.md)

Le tecnologie di ottimizzazione e offload di rete in Windows Server 2016 includono funzionalità e tecnologie per solo software (SO, Software Only), funzionalità e tecnologie per software e hardware (SH, Software e Hardware) integrate e funzionalità e tecnologie per solo hardware (HO, Hardware Only).

È anche disponibile la seguente documentazione sulle tecnologie di ottimizzazione e offload di rete.

- [Giuda alla configurazione della scheda di interfaccia di rete (NIC) convergente](technologies/conv-nic/cnic-top.md)
- [Data Center Bridging (DCB)](technologies/dcb/dcb-top.md)
- [Virtual Receive Side Scaling (vRSS)](technologies/vrss/vrss-top.md)


### [Server dei criteri di rete](technologies/nps/nps-top.md)

Server dei criteri di rete consente di creare e imporre criteri di accesso di rete a livello di organizzazione per l'autorizzazione e l'autenticazione delle richieste di connessione.

### [Shell di rete (Netsh)](technologies/netsh/netsh.md)

È possibile utilizzare l'utilità di rete netsh per gestire tecnologie di rete in Windows Server 2016 e Windows 10.

### [Ottimizzazione delle prestazioni del sottosistema di rete](technologies/network-subsystem/net-sub-performance-top.md)

Questo argomento include informazioni su come scegliere la scheda di rete più adatta per il carico di lavoro del server e ordinare le interfacce di rete, i contatori delle prestazioni correlati alla rete, le schede di rete di ottimizzazione delle prestazioni e le tecnologie di rete correlate, come RSS \(Receive-Side Scaling\), RSC \(Receive-Side Coalescing\) e altre.

### [Gruppo NIC](technologies/nic-teaming/NIC-Teaming.md)

Gruppo NIC consente di raggruppare le schede di rete Ethernet fisiche in una o più schede di rete virtuali basate su software. Queste schede di rete virtuali garantiscono prestazioni elevate e tolleranza di errore in caso di errore delle schede di rete.

### [Criteri Qualità del servizio (QoS)](technologies/qos/qos-policy-top.md)

Puoi utilizzare i criteri QoS come un punto centrale per la gestione della larghezza di banda di rete tra l'intera infrastruttura di Active Directory creando profili QoS, le cui impostazioni vengono distribuite con Criteri di gruppo.

### [Accesso remoto](../remote/remote-access/remote-access.md)

Puoi utilizzare le tecnologie di Accesso remoto, ad esempio DirectAccess e Rete privata virtuale \(VPN\) per consentire ai lavoratori remoti di connettersi alle risorse di rete interne. Inoltre, è possibile utilizzare Accesso remoto per il routing della rete locale \(LAN\) e per Proxy applicazione Web, che rende disponibili funzionalità di proxy inverso per le applicazioni Web all'interno della rete aziendale, in modo da consentire agli utenti con qualsiasi dispositivo di accedere a tali applicazioni dall'esterno della rete aziendale.

La documentazione relativa ad Accesso remoto è ora disponibile nella sezione [Gestione di accesso remoto e server](https://docs.microsoft.com/windows-server/remote/) nel sommario di Windows Server 2016, in Accesso remoto. Per altre informazioni, vedi [Accesso remoto](../remote/remote-access/remote-access.md).

Per ulteriori informazioni su Proxy applicazione Web, che è un servizio ruolo del ruolo del server Accesso remoto, vedi [Proxy applicazione Web in Windows Server 2016](https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/web-application-proxy-windows-server).

### [Rete privata virtuale (VPN)](../remote/remote-access/vpn/vpn-top.md)

In Windows Server 2016 **DirectAccess e VPN** è ora un servizio del ruolo del server **Accesso remoto**.

Quando installi Accesso remoto come server VPN, puoi utilizzare la rete privata virtuale \(VPN\) per fornire ai dipendenti remoti la possibilità di eseguire la connessione alla rete dell'organizzazione tramite Internet, garantendo al contempo la privacy per le informazioni grazie a connessioni crittografate.

Con VPN per Accesso remoto di Windows Server 2016 e computer client Windows 10 puoi ora distribuire VPN Always On. Una VPN Always On ti consente di gestire i client VPN remoti sempre connessi, agevolando al contempo i lavoratori remoti, che non dovranno più connettersi e disconnettersi manualmente dalla VPN alla rete dell'organizzazione.

Per ulteriori informazioni, vedi la [Guida per Windows Server 2016 e Windows 10 relativa alla distribuzione di VPN Always On per Accesso remoto](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy).

>[!NOTE]
>La documentazione di VPN è ora disponibile nella sezione [Gestione di accesso remoto e server](https://docs.microsoft.com/windows-server/remote/) nel sommario di Windows Server 2016, in [Accesso remoto](https://docs.microsoft.com/windows-server/remote/remote-access/remote-access).

Per ulteriori informazioni su VPN, vedi [Reti private virtuali (VPN)](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/vpn-top).

### [Rete di contenitori di Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/container-networking)

Rete di contenitori di Windows ti consente di creare e gestire le reti per la connessione di endpoint contenitore su host Windows 10 e Windows Server, utilizzando flussi di lavoro e strumenti standard di settore. Le reti di contenitori di Windows supportano più topologie, tra cui reti private, L2 piatte e L3 reindirizzate.

Sono supportati anche overlay che possono essere creati in locale sull'host utilizzando Docker, Kubernetes o Windows PowerShell tramite plug-in grado di comunicare con il servizio rete host \(HNS, Host Networking Service\) di Windows. Puoi creare e gestire le reti di cluster multinodo tramite sistemi di orchestrazione di livello superiore eseguendo la comunicazione con il servizio rete host di ciascun nodo tramite un agente locale.

### [Windows Internet Name Service (WINS)](technologies/wins/wins-top.md)

WINS (Windows Internet Name Service) è un servizio legacy di registrazione e risoluzione dei nomi di computer che esegue il mapping dei nomi NetBIOS dei computer a indirizzi IP. È consigliabile utilizzare DNS rispetto a WINS.

## Risorse aggiuntive

Le risorse sulla rete per i sistemi operativi precedenti a Windows Server 2016 sono disponibili negli articoli seguenti.

- [Panoramica delle reti](https://technet.microsoft.com/library/hh831357.aspx) - Windows Server 2012 e Windows Server 2012 R2
- [Funzionalità di rete](https://technet.microsoft.com/library/cc753940) - Windows Server 2008 e Windows Server 2008 R2
- [Contenuti ritirati relativi a Windows Server 2003/2003 R2 ](https://www.microsoft.com/download/details.aspx?id=53314) - Windows Server 2003
