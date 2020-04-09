---
title: Rete
description: In questo argomento viene fornita una panoramica delle tecnologie Software Defined Networking e di piattaforma di rete disponibili in Windows Server 2016.
ms.prod: windows-server
layout: LandingPage
ms.technology: networking
ms.topic: landing-page
ms.assetid: daaf6b61-5953-4c2d-b6b8-7c885b552646
manager: dougkim
ms.author: anpaul
author: AnirbanPaul
ms.localizationpriority: medium
ms.openlocfilehash: de1acf0a0ed233fdd7675da3bca63f7f16824a9a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855804"
---
# <a name="networking"></a>Rete

>[!TIP]
> Per informazioni sulle versioni precedenti di Windows Server, Vedi le nostre altre [librerie di Windows Server](/previous-versions/windows/) in docs.microsoft.com. È anche possibile cercare informazioni specifiche [in questo sito](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions).

<hr />

La funzionalità di rete è una parte fondamentale del software definito datacenter \(piattaforma di\) SDDC e Windows Server 2016 offre nuove e migliorate funzionalità Software Defined Networking \(SDN\) tecnologie che consentono di passare a una soluzione SDDC completamente realizzata per la propria organizzazione.

Quando si gestiscono le reti come risorsa definita dal software, è possibile descrivere i requisiti dell'infrastruttura di un'applicazione una sola volta e quindi scegliere la posizione in cui viene eseguita l'applicazione, in locale o nel cloud. 

Grazie a questa coerenza, le applicazioni sono ora più scalabili e possono essere eseguite senza problemi ovunque e sempre con lo stesso livello di sicurezza, prestazioni, qualità del servizio e disponibilità.

<hr />

<ul class="cardsF panelContent">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-whats-new.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h2><a href="../networking/What-s-New-in-Networking.md">&#39;Novità della rete</a></h2>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>

<hr />

<h2>Software Defined Networking</h2>

<ul class="cardsF panelContent">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="https://docs.microsoft.com/windows-server/networking/sdn/">SDN (Software Defined Networking)</a></h3>
                        <hr />
                        <p>Questo argomento contiene informazioni sulle tecnologie SDN disponibili in Windows Server, System Center e Microsoft Azure.</p>
                        <p><b>Nota:</b> Per gli host Hyper-V e le macchine virtuali (VM) che eseguono server di infrastruttura SDN, ad esempio i nodi del controller di rete e del bilanciamento del carico software, è necessario installare Windows Server Datacenter Edition. Per gli host Hyper-V che contengono solo macchine virtuali del carico di lavoro tenant connesse a reti controllate da SDN, è possibile eseguire Windows Server Standard Edition.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h3><a href="sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md">Distribuire un'infrastruttura software defined Network usando gli script</a></h3>
                    <hr />
                    <p>Questa guida offre alcune istruzioni per la distribuzione di Controller di rete con le reti e i gateway virtuali in un lab di test.</p>
                </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="sdn/technologies/network-controller/Network-Controller.md">Controller di rete</a></h3>
                        <hr />
                        <p>Controller di rete rappresenta un punto di automazione programmabile e centralizzato per gestire, configurare e monitorare l'infrastruttura di rete fisica e virtuale nel centro dati e di risolverne gli eventuali problemi.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md">Bilanciamento del carico software &#40;SLB&#41; per Sdn</a></h3>
                        <hr />
                        <p>I provider di servizi cloud (CSP) e le aziende che distribuiscono software defined Networking (SDN) in Windows Server 2016 possono utilizzare il bilanciamento del carico software (SLB) per distribuire in modo uniforme il traffico di rete dei clienti tenant e tenant tra le risorse di rete virtuale. Windows Server SLB consente di abilitare più server per l'hosting dello stesso carico di lavoro, offrendo disponibilità e scalabilità elevate.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md">Gateway RAS per SDN</a></h3>
                        <hr />
                        <p>Il gateway RAS, che è un router con supporto BGP (software, multi-tenant, Border Gateway Protocol) in Windows Server 2016, è progettato per i provider di servizi cloud (CSP) e le aziende che ospitano più reti virtuali tenant con la rete Hyper-V. Virtualizzazione.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md">Virtualizzazione delle funzioni di rete</a></h3>
                        <hr />
                        <p>Nei data center definiti dal software, le funzioni di rete eseguite dalle appliance hardware (ad esempio, i bilanciamenti del carico, i firewall, i router, i commutatori e così via) vengono virtualizzate sempre più come appliance virtuali. Questo &quot;&quot; di virtualizzazione delle funzioni di rete è una progressione naturale della virtualizzazione dei server e della virtualizzazione di rete.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md">Panoramica del firewall del data center</a></h3>
                        <hr />
                        <p>Il firewall del centro dati è un firewall multi–tenant, con stato, a 5 tuple (protocollo, numeri di porta di origine e destinazione, indirizzi IP di origine e destinazione), a livello rete.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>

<hr />

## <a name="networking-technologies"></a><a name="bkmk_networking"></a>Tecnologie di rete

<ul class="cardsF panelContent">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="branchcache/BranchCache.md">BranchCache</a></h3>
                        <hr />
                        <p>BranchCache è una tecnologia di ottimizzazione della larghezza di banda di per le reti WAN (Wide Area Network). Per ottimizzare la larghezza di banda della rete WAN quando gli utenti accedono al contenuto di server remoti, BranchCache recupera il contenuto dai server di contenuti della sede centrale o ospitati nel cloud e lo memorizza nella cache presso le succursali, consentendo ai computer client delle succursali stesse di accedere al contenuto in locale anziché attraverso la rete WAN.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="core-network-guide/core-network-guide-windows-server.md">Guida alla rete core</a></h3>
                        <hr />
                        <p>Scopri come distribuire una rete di Windows Server con la Guida alla rete core e aggiungere funzionalità alla distribuzione di rete con le Guide complementari alla rete core.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="../remote/remote-access/directaccess/DirectAccess.md">DirectAccess</a></h3>
                        <hr />
                        <p>DirectAccess consente agli utenti remoti di connettersi alle risorse di rete dell'organizzazione. </p>
                    </div>
                </div>
            </div>
        </div>
    </li>
   <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="dns/dns-top.md">Domain Name System (DNS)</a></h3>
                        <hr />
                        <p>Domain Name System (DNS) è una delle suite di protocolli standard di settore che includono TCP/IP e insieme il client DNS e il server DNS forniscono servizi di risoluzione dei nomi di mapping di indirizzi IP da nome computer a computer e utenti.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/dhcp/dhcp-top.md">Dynamic Host Configuration Protocol &#40;DHCP&#41;</a></h3>
                        <hr />
                        <p>Dynamic Host Configuration Protocol (DHCP) è un protocollo client/server che fornisce automaticamente un host IP (Internet Protocol) con l'indirizzo IP e altre informazioni di configurazione correlate, ad esempio subnet mask e gateway predefinito.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="sdn/technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md">Virtualizzazione rete Hyper-V</a></h3>
                        <hr />
                        <p>Virtualizzazione rete Hyper-V (HNV) consente la virtualizzazione delle reti dei clienti in un'infrastruttura di rete fisica condivisa.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md">Commutatore virtuale Hyper-V</a></h3>
                        <hr />
                        <p>Il commutatore virtuale Hyper-V è un commutatore di rete Ethernet di livello 2 basato sul software, disponibile nella console di gestione di Hyper-V quando si installa il ruolo del server Hyper-V. Il commutatore include funzionalità estendibili e gestibili a livello di programmazione, per la connessione delle macchine virtuali a una rete fisica o virtuale. Il commutatore virtuale Hyper-V supporta inoltre l'imposizione di criteri per sicurezza, l'isolamento e i livelli di servizio. </p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/ipam/ipam-top.md">Gestione indirizzi IP &#40;gestione indirizzi IP&#41;</a></h3>
                        <hr />
                        <p>Gestione indirizzi IP è una suite integrata di strumenti che consentono la pianificazione, la distribuzione, la gestione e il monitoraggio end-to-end dell'infrastruttura di indirizzi IP, con un'esperienza utente avanzata. Gestione indirizzi IP individua automaticamente i server dell'infrastruttura di indirizzi IP e i server DNS nella rete e consente di gestirli da un'interfaccia centrale.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/Network-Load-Balancing.md">Bilanciamento del carico di rete</a></h3>
                        <hr />
                        <p>Bilanciamento carico di rete distribuisce il traffico tra più server utilizzando il protocollo di rete TCP/IP. Per le distribuzioni non SDN, bilanciamento carico di rete garantisce che le applicazioni senza stato, ad esempio i server Web che eseguono Internet Information Services (IIS), siano scalabili mediante l'aggiunta di altri server man mano che aumenta il carico.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/hpn/hpn-top.md">Rete ad alte prestazioni</a></h3>
                        <hr />
                        <p>Le tecnologie di ottimizzazione e offload di rete in Windows Server 2016 includono funzionalità e tecnologie per solo software (SO, Software Only), funzionalità e tecnologie per software e hardware (SH, Software e Hardware) integrate e funzionalità e tecnologie per solo hardware (HO, Hardware Only).</p>
                        <p>È disponibile anche la documentazione relativa alle tecnologie di ottimizzazione e offload seguente:<p>
                        <hr />
                        <a href="technologies/conv-nic/cnic-top.md">Scheda di interfaccia di rete (NIC) convergente</a>
                        <hr />
                         di 
                        <a href="technologies/dcb/dcb-top.md">Data Center Bridging (DCB)</a><hr />
                     di 
                        <a href="technologies/vrss/vrss-top.md">Receive-Side Scaling virtuale (vRSS)</a></div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/nps/nps-top.md">Server dei criteri di rete</a></h3>
                        <hr />
                        <p>Server dei criteri di rete consente di creare e imporre criteri di accesso di rete a livello di organizzazione per l'autorizzazione e l'autenticazione delle richieste di connessione.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/netsh/netsh.md">Shell di rete (Netsh)</a></h3>
                        <hr />
                        <p>È possibile utilizzare l'utilità Network Shell (netsh) networking per gestire le tecnologie di rete in Windows Server 2016 e Windows 10.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/network-subsystem/net-sub-performance-top.md">Ottimizzazione delle prestazioni del sottosistema di rete</a></h3>
                        <hr />
                        <p>In questo argomento vengono fornite informazioni sulla scelta della scheda di rete adatta per il carico di lavoro del server, dell'ordinamento delle interfacce di rete, dei contatori delle prestazioni correlati alla rete e delle schede di rete di ottimizzazione delle prestazioni e delle tecnologie di rete Receive-Side Scaling (RSS), Receive-Side coalesation (RSC) e altri.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/nic-teaming/NIC-Teaming.md">Gruppo NIC</a></h3>
                        <hr />
                        <p>Gruppo NIC consente di raggruppare le schede di rete Ethernet fisiche in una o più schede di rete virtuali basate su software. Queste schede di rete virtuali garantiscono prestazioni elevate e tolleranza di errore in caso di errore delle schede di rete.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/qos/qos-policy-top.md">Criteri di qualità del servizio (QoS)</a></h3>
                        <hr />
                        <p>Puoi utilizzare i criteri QoS come un punto centrale per la gestione della larghezza di banda di rete tra l'intera infrastruttura di Active Directory creando profili QoS, le cui impostazioni vengono distribuite con Criteri di gruppo.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/wins/wins-top.md">Windows Internet Name Service (WINS)</a></h3>
                        <hr />
                        <p>WINS (Windows Internet Name Service) è un servizio legacy di registrazione e risoluzione dei nomi di computer che esegue il mapping dei nomi NetBIOS dei computer a indirizzi IP. È consigliabile utilizzare DNS rispetto a WINS.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="../remote/remote-access/remote-access.md">Accesso remoto</a></h3>
                        <hr />
                        <p>È possibile utilizzare le tecnologie di accesso remoto, ad esempio DirectAccess e la rete privata virtuale (VPN), per fornire agli utenti remoti la connettività alle risorse di rete interne. Inoltre, è possibile utilizzare l'accesso remoto per il routing della rete locale (LAN) e per il proxy dell'applicazione Web. che rende disponibili funzionalità di proxy inverso per le applicazioni Web all'interno della rete aziendale, in modo da consentire agli utenti con qualsiasi dispositivo di accedere a tali applicazioni dall'esterno della rete aziendale.</p>
                        <p>Per ulteriori informazioni su proxy applicazione Web, che è un servizio ruolo del ruolo del server accesso remoto, vedere <a href="https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/web-application-proxy-windows-server">proxy applicazione Web in Windows server 2016</a></p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="https://docs.microsoft.com/virtualization/windowscontainers/container-networking/architecture">Rete di contenitori di Windows</a></h3>
                        <hr />
                        <p>Rete di contenitori di Windows ti consente di creare e gestire le reti per la connessione di endpoint contenitore su host Windows 10 e Windows Server, utilizzando flussi di lavoro e strumenti standard di settore. Le reti di contenitori di Windows supportano più topologie, tra cui reti private, L2 piatte e L3 reindirizzate.</p>
                        <p>Sono supportate anche le sovrimpressioni che è possibile creare localmente nell'host usando Docker, Kubernetes o Windows PowerShell tramite plug-in che comunicano con il servizio di rete host Windows (HNS). È possibile creare e gestire reti di cluster a più nodi tramite sistemi di orchestrazione di livello superiore tramite la comunicazione tramite un agente locale per il HNS di ciascun nodo.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="../remote/remote-access/vpn/vpn-top.md">Rete privata virtuale (VPN)</a></h3>
                        <hr />
                        <p>DirectAccess e VPN è un servizio ruolo del ruolo del server Accesso remoto.</p>
                        <p>Quando si installa accesso remoto come server VPN, è possibile usare la rete privata virtuale (VPN) per fornire ai dipendenti remoti le connessioni alla rete aziendale attraverso Internet, mantenendo al tempo stesso la privacy delle informazioni con le connessioni crittografate .</p>
                        <p> Con una VPN per l'accesso remoto in Windows Server, e computer client Windows 10, puoi distribuire una VPN Always On. Una VPN Always On ti consente di gestire i client VPN remoti sempre connessi, agevolando al contempo i lavoratori remoti, che non dovranno più connettersi e disconnettersi manualmente dalla VPN alla rete dell'organizzazione.</p>
                        <p>Per altre informazioni, vedere <a href="https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy">Guida alla distribuzione di accesso remoto always on VPN per Windows Server 2016 e Windows 10</a></p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>

## <a name="additional-resources"></a>Risorse aggiuntive

Le risorse sulla rete per i sistemi operativi precedenti a Windows Server 2016 sono disponibili negli articoli seguenti.

- [Panoramica delle reti](https://technet.microsoft.com/library/hh831357.aspx) di Windows Server 2012 e Windows Server 2012 R2
- Windows Server 2008 and Windows Server 2008 R2 [Networking](https://technet.microsoft.com/library/cc753940) (Funzionalità di rete di Windows Server 2008 e Windows Server 2008 R2)
- [Contenuti ritirati di Windows server 2003 Windows server 2003/2003 R2](https://www.microsoft.com/download/details.aspx?id=53314)
