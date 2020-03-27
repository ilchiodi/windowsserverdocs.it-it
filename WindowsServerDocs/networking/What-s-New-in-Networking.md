---
title: Novità di Reti
description: In questo argomento fornisce informazioni generali sulle nuove funzionalità e tecnologie per la rete in Windows Server 2016
ms.prod: windows-server
ms.technology: networking
ms.topic: get-started-article
ms.assetid: 08fb7563-d319-48a9-b181-ca0ca3032c18
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 35c3c3b2610918e8b0fd69ccf04422e3f6df4d0e
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318544"
---
# <a name="whats-new-in-networking"></a>Novità di Reti

>Si applica a: Windows Server 2016

Di seguito sono nuove o avanzate tecnologie di rete in Windows Server 2016.  
  UPD questo argomento contiene le sezioni seguenti.  
  
-   [Nuove funzionalità e tecnologie di rete](#bkmk_features)  
  
-   [Nuove funzionalità per tecnologie di rete aggiuntive](#bkmk_existing)  
  
## <a name="new-networking-features-and-technologies"></a><a name="bkmk_features"></a>Nuove funzionalità e tecnologie di rete

Funzionalità di rete sono una parte fondamentale della piattaforma Software definito Datacenter (SDDC) e Windows Server 2016 fornisce tecnologie nuove e migliorate di reti SDN (Software Defined) che consentono di passare a una soluzione SDDC realizzata completamente per l'organizzazione.  
  
Quando si gestiscono le reti come risorsa definite da software, è possibile descrivere i requisiti di infrastruttura di un'applicazione una sola volta e quindi scegliere in cui viene eseguita l'applicazione, in locale o nel cloud.  Questa coerenza significa che le applicazioni sono ora più scalabili e senza problemi, è possibile eseguire applicazioni, ovunque, con uguale probabilità intorno a sicurezza, prestazioni, qualità del servizio e disponibilità.  
  
Le sezioni seguenti contengono informazioni su queste nuove funzionalità e tecnologie di rete.  
  
### <a name="software-defined-networking-infrastructure"></a>Software definito dell'infrastruttura di rete

Di seguito sono le tecnologie di infrastruttura SDN nuove o migliorate.  
  
-   **Controller di rete**. Nuovo in Windows Server 2016, Controller di rete fornisce un punto programmabile e centralizzato di automazione per gestire, configurare, monitorare e risolvere i problemi dell'infrastruttura di rete fisica e virtuale nel proprio Data Center. Il Controller di rete permette di automatizzare la configurazione dell'infrastruttura di rete, invece di eseguire la configurazione manuale dei dispositivi e dei servizi di rete. Per ulteriori informazioni, vedere [Controller di rete](sdn/technologies/network-controller/Network-Controller.md) e [distribuire Software definito reti usando gli script](https://technet.microsoft.com/library/mt427380.aspx).  
  
-   **Commutatore virtuale Hyper-V**. Il commutatore virtuale Hyper-V viene eseguito negli host Hyper-V e consente di creare cambio distribuita e il routing e un livello di imposizione dei criteri che è allineato e compatibile con Microsoft Azure. Per ulteriori informazioni, vedere [commutatore virtuale Hyper-V](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).  
  
-   **Funzione virtualizzazione (NFV) di rete**. Nel software di oggi sempre più Data Center definito, le funzioni di rete che vengono eseguite da dispositivi hardware (ad esempio servizi di bilanciamento del carico, i firewall, router, commutatori e così via) sono distribuito come dispositivi virtuali. Questo "virtualizzazione delle funzioni di rete" è una progressione naturale di virtualizzazione di server e virtualizzazione di rete. I dispositivi virtuali sono in rapida emergenza e creano un nuovo mercato. Continuano a generare interesse e ottenere l'espansione di entrambe le piattaforme di virtualizzazione e i servizi cloud. Le seguenti tecnologie NFV sono disponibili in Windows Server 2016.  
  
    -   **datacenter Firewall**. Questo firewall distribuito fornisce elenchi di controllo granulare di accesso (ACL), che consentono di applicare criteri del firewall a livello di interfaccia di macchina Virtuale o a livello di subnet.  
  
        Per ulteriori informazioni, vedere [Cenni preliminari sul Firewall Datacenter](sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md).  
  
    -   **Gateway RAS**. È possibile utilizzare RAS Gateway per indirizzare il traffico tra reti virtuali e reti, comprese le connessioni VPN site-to-site dal data center cloud di siti remoti dei tenant. In particolare, è possibile distribuire Internet Key Exchange versione 2 (IKEv2) site-to-site reti private virtuali (VPN), VPN di livello 3 (L3) e i gateway Generic Routing Encapsulation (GRE). Inoltre, sono ora supportati i pool di gateway e ridondanza N + M gateway; e protocollo BGP (Border Gateway) con funzionalità di Reflector Route fornisce il routing dinamico tra le reti per tutti gli scenari di gateway (VPN IKEv2, GRE VPN e VPN L3).  
  
        Per ulteriori informazioni, vedere  [What's New in Gateway RAS](sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) e [Gateway RAS per SDN](sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md).  
          
    - **Bilanciamento del carico software (SLB) e NAT (NAT)** . 4 bilanciamento del carico di livello il Nord-Sud e Ovest orientale e migliora la velocità effettiva grazie al supporto Direct Server Return, con cui il traffico di rete restituito può ignorare il bilanciamento del carico multiplexer NAT.  
       Per ulteriori informazioni, vedere [il bilanciamento del carico Software & #40; SLB & #41; per SDN](sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).  
  
    Per ulteriori informazioni, vedere [virtualizzazione delle funzioni di rete](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md).  
  
-   **Protocolli standardizzati**. Controller di rete utilizza Representational State Transfer (REST) sulla relativa interfaccia northbound con payload di JavaScript Object Notation (JSON). L'interfaccia southbound di Controller di rete Usa vSwitch aprire Database Management Protocol (OVSDB).  
  
-   **Tecnologie di incapsulamento flessibile**. Queste tecnologie operano nel piano di dati e supportano Virtual Extensible LAN (VxLAN) sia Network Virtualization Generic Routing Encapsulation (NVGRE). Per ulteriori informazioni, vedere [GRE Tunneling in Windows Server 2016](../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md).  
  
Per ulteriori informazioni su SDN, vedere [la rete definita dal Software & #40; SDN & #41;](sdn/software-defined-networking.md).  
  
### <a name="cloud-scale-fundamentals"></a>Nozioni fondamentali su scala cloud
 
Nozioni fondamentali su scala di cloud seguenti sono ora disponibili.  
  
-   **Scheda di rete (NIC) convergente**. La scheda di RETE convergente consente di utilizzare una sola scheda di rete per la gestione, archiviazione abilitati RDMA Remote Direct Memory Access e traffico tenant. Ciò riduce le spese di capitale associate a ogni server nel proprio Data Center, in quanto è necessario un minor numero di schede di rete per gestire diversi tipi di traffico per ciascun server.  
  
-   **Pacchetti diretti**.  Direct pacchetto fornisce un'infrastruttura di elaborazione di pacchetti a bassa latenza e velocità effettiva del traffico di rete ad alta.  
  
-   **Opzione incorporato Teaming (SET)** .        SET è una soluzione gruppo NIC è integrata nel commutatore virtuale Hyper-V. SET consente il raggruppamento di fino a otto schede NIC fisiche in un singolo gruppo SET, che migliora la disponibilità e fornisce il failover. In Windows Server 2016, è possibile creare team SET che sono limitati all'utilizzo di Server Message Block (SMB) e RDMA. Inoltre, è possibile utilizzare SET team per distribuire il traffico di rete per la virtualizzazione rete Hyper-V. Per ulteriori informazioni, vedere [Remote Direct Memory Access & #40; RDMA & #41; e passare al gruppo incorporato & #40; IMP & #41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  
  
## <a name="new-features-for-additional-networking-technologies"></a><a name="bkmk_existing"></a>Nuove funzionalità per tecnologie di rete aggiuntive

In questa sezione contiene informazioni sulle nuove funzionalità per le tecnologie familiari di rete.
  
## <a name="dhcp"></a><a name="bkmk_dhcp"></a>DHCP  
DHCP è uno standard IETF (Internet Engineering Task Force) progettato per ridurre il carico amministrativo e la complessità della configurazione degli host in una rete basata su TCP/IP, ad esempio una Intranet privata. Se si usa il servizio server DHCP, il processo di configurazione di TCP/IP nei client DHCP è automatico.  
  
Per ulteriori informazioni, vedere [What's New in DHCP](technologies/dhcp/What-s-New-in-DHCP.md).  
  
## <a name="dns"></a><a name="bkmk_dns"></a>DNS  
DNS è un sistema per la denominazione di computer e servizi di rete utilizzato nelle reti TCP/IP. La denominazione DNS consente di individuare computer e servizi mediante nomi descrittivi. Quando un utente immette un nome DNS in un'applicazione, i servizi DNS possono risolvere il nome in altre informazioni a esso associate, ad esempio un indirizzo IP.  
  
Di seguito è informazioni Client DNS e Server DNS.  
  
### <a name="dns-client"></a><a name="bkmk_dnsc"></a>Client DNS  
Di seguito sono le tecnologie client DNS nuove o migliorate.  
  
-   **Associazione al servizio Client DNS**. In Windows 10, il servizio Client DNS offre supporto avanzato per i computer con più di un'interfaccia di rete.  
  
Per ulteriori informazioni, vedere Novità [del client DNS in Windows Server 2016](dns/What-s-New-in-DNS-Client.md)  
  
### <a name="dns-server"></a><a name="bkmk_dnss"></a>Server DNS  
Di seguito sono le tecnologie di server DNS nuove o migliorate.  
  
-   **Criteri DNS**.  È possibile configurare criteri DNS per specificare come un server DNS risponde alle query DNS. Le risposte DNS possono essere basate sull'indirizzo IP client (posizione), ora del giorno e molti altri parametri. I criteri di DNS consentono DNS con riconoscimento della posizione, la gestione del traffico, il bilanciamento del carico, DNS "split Brain" e altri scenari.  
  
-   **Supporto per file nano Server basato su DNS**,   è possibile distribuire i server DNS in Windows Server 2016 su un'immagine Nano Server. Questa opzione di distribuzione è disponibile se si utilizza DNS basato su file. Dal server DNS in esecuzione su un'immagine Nano Server, è possibile eseguire i server DNS con footprint ridotto, avvio rapido e applicazione di patch ridotta a icona.  
  
    > [!NOTE]   
    > DNS integrate in Active Directory non è supportato in Nano Server.  
  
-   **Velocità di risposta limitante (RRL)** .  È possibile abilitare la limitazione della velocità di risposta dei server DNS. In questo modo, evitare la possibilità di sistemi dannoso tramite i server DNS per avviare un attacco denial of service in un client DNS.  
  
-   **L'autenticazione basata su DNS di entità denominate (DANE)** .   È possibile utilizzare i record TLSA (autenticazione di sicurezza Layer di trasporto) per fornire informazioni al client DNS che indicano quali autorità di certificazione (CA) dovrebbero un certificato da per il nome di dominio. In questo modo si evitano gli attacchi man-in-the-Middle in cui un utente potrebbe danneggiare la cache DNS per puntare al proprio sito Web e fornire un certificato emesso da un'autorità di certificazione diversa.  
  
-   **Supporto di record sconosciuto**.   
     È possibile aggiungere i record che non sono supportati in modo esplicito dal server DNS di Windows utilizzando la funzionalità di record sconosciuto.  
  
-   **I parametri radice IPv6**.   
     È possibile utilizzare la connettività IPV6 nativa supportano i parametri radice per eseguire la risoluzione dei nomi internet utilizzando i server principali di IPV6.  
  
-   **Migliorato supporto di Windows PowerShell**.   
      Nuovi cmdlet di Windows PowerShell sono disponibili per il Server DNS.  
  
Per ulteriori informazioni, vedere Novità [di server DNS in Windows server 2016](dns/What-s-New-in-DNS-Server.md)  
  
## <a name="gre-tunneling"></a><a name="bkmk_GRE"></a>Tunneling GRE  
Gateway RAS supporta ora i tunnel Generic Routing Encapsulation (GRE) la disponibilità elevata per le connessioni da sito a sito e la ridondanza N + M di gateway. GRE è un protocollo di tunneling leggero in grado di incapsulare un'ampia gamma di protocolli a livello di rete all'interno di collegamenti point-to-point virtuali in un sistema Internetwork IP (Internet Protocol, protocollo Internet).  
  
Per ulteriori informazioni, vedere la pagina relativa [al tunneling GRE in Windows Server 2016](../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md).  
  
## <a name="hyper-v-network-virtualization"></a><a name="HNV"></a>Virtualizzazione rete Hyper-V  
Introdotto in Windows Server 2012, Hyper-V rete Virtualizzazione consente la virtualizzazione di reti dei clienti su un'infrastruttura di rete fisica condivisa. Apportando modifiche minime necessari per l'infrastruttura di rete fisica, virtualizzazione RETE fornisce ai provider di servizi la flessibilità di distribuire e migrare i carichi di lavoro tenant ovunque in tre aree: il cloud di provider del servizio, il cloud privato o la cloud pubblica di Microsoft Azure.  
  
Per ulteriori informazioni, vedere Novità [di virtualizzazione rete Hyper-V in Windows Server 2016](sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md) .  
  
## <a name="ipam"></a><a name="bkmk_ipam"></a>IPAM  
Gestione indirizzi IP offre funzionalità amministrative e di monitoraggio altamente personalizzabili per l'indirizzo IP e l'infrastruttura DNS nella rete di un'organizzazione. Utilizzando Gestione indirizzi IP, è monitorare, controllare e gestire i server che eseguono Dynamic Host Configuration Protocol (DHCP) e del sistema DNS (Domain Name).  
  
-   **Gestione indirizzi IP avanzata**.  
     Le funzionalità di gestione indirizzi IP sono migliorate per scenari quali la gestione delle subnet /32 IPv4 e IPv6 /128 e ricerca gratuita subnet di indirizzi IP e gli intervalli in un blocco di indirizzi IP.  
  
-   **Avanzato di gestione del servizio DNS**.  
     Gestione indirizzi IP supporta i record di risorse DNS, server d'inoltro condizionale e gestione delle zone DNS per i server DNS integrate in Active Directory e backup di file aggiunti al dominio.  
  
-   **Integrata la gestione degli indirizzi (DDI) DNS, DHCP e IP**.  
     Diverse nuove esperienze e gestione del ciclo di vita integrata sono abilitate le operazioni, ad esempio per visualizzare tutti i record di risorse DNS che si riferiscono a un indirizzo IP, automatizzati inventario degli indirizzi IP in base ai record di risorse e gestione del ciclo di vita degli indirizzi IP per le operazioni sia DNS e DHCP.  
  
-   **Supporto per più foreste Active Directory**.  
     È possibile utilizzare Gestione indirizzi IP per gestire i server DNS e DHCP di più foreste Active Directory quando esiste una relazione di trust bidirezionale tra la foresta in cui è installato Gestione indirizzi IP e ogni insieme di strutture remoti.  
  
-   **Supporto di Windows PowerShell per controllo di accesso basato sui ruoli**.  
     È possibile utilizzare Windows PowerShell per impostare gli ambiti di accesso per gli oggetti di gestione indirizzi IP.  
  
Per ulteriori informazioni, vedere [What's New in Gestione indirizzi IP](technologies/ipam/What-s-New-in-IPAM.md) e [gestire gestione indirizzi IP](technologies/ipam/Manage-IPAM.md).  
  

