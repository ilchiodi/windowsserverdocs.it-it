---
title: Pianificare un'infrastruttura Software Defined Network
description: In questo argomento fornisce informazioni su come pianificare la distribuzione dell'infrastruttura di rete SDN (Software Defined).
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: virtual-network
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: ea7e53c8-11ec-410b-b287-897c7aaafb13
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bb3b9313996637fa5ee7367c538fe04d7cbefea9
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="plan-a-software-defined-network-infrastructure"></a>Pianificare un'infrastruttura Software Defined Network

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Esaminare le informazioni seguenti per pianificare la distribuzione dell'infrastruttura di rete SDN (Software Defined). Dopo aver verificato le informazioni, vedere [distribuire un'infrastruttura Software Defined Network](../deploy/Deploy-a-Software-Defined-Network-Infrastructure.md) per informazioni sulla distribuzione.

>[!NOTE]
>Oltre a questo argomento, il contenuto di pianificazione SDN seguente è disponibile.  
>
> - [Installazione e i requisiti di preparazione per la distribuzione di Controller di rete](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  

Per informazioni su Hyper-V rete virtualizzazione, che è possibile utilizzare la virtualizzazione di reti in una distribuzione di SDN Microsoft, vedere [virtualizzazione rete Hyper-V](../technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md).  

## <a name="prerequisites"></a>Prerequisiti
In questo argomento vengono descritti alcuni prerequisiti hardware e software, inclusi:

-   **Rete fisica**  
    È necessario accedere ai dispositivi di rete fisica per configurare le VLAN, Routing, BGP, Data Center Bridging ETS () se si usa una tecnologia RDMA e Data Center Bridging (base alla priorità) se si usa un RoCE in base a tecnologia RDMA. Questo argomento illustra la configurazione manuale di commutatore, nonché il Peering BGP in commutatori di livello 3 al router o a una macchina virtuale di Routing e il Server di accesso remoto (RRAS).   

-   **Host di calcolo fisiche**  
Questi host eseguono Hyper-V e sono necessari per SDN host dell'infrastruttura e tenant macchine virtuali.  In questi host per ottenere prestazioni ottimali, descritto più avanti in, è necessario hardware di rete specifici di **hardware di rete** sezione.  
      
  
## <a name="physical-network-configuration"></a>Configurazione di rete fisica

Ogni host fisico calcolo richiede la connettività di rete tramite uno o più schede di rete collegate a un porte del commutatore fisico. La rete è suddivise in più segmenti della rete logica facoltativamente supportati da un livello 2 [VLAN](https://en.wikipedia.org/wiki/Virtual_LAN). I prefissi di subnet IP e gli ID di VLAN illustrato di seguito sono riportati alcuni esempi e devono essere personalizzati per l'ambiente in base alle indicazioni dall'amministratore di rete. Se sono privi di una rete logiche o in modalità accesso, usare ID VLAN 0 per queste reti quando si configurano le subnet logiche nei file di configurazione di script di System Center Virtual Machine Manager o PowerShell.

>[!IMPORTANT]
>Windows Server 2016 rete definita dal Software supporta gli indirizzi IPv4 per il Metti sotto e la sovrimpressione. IPv6 non è supportato.
  
### <a name="management-and-hnv-provider-logical-networks"></a>Gestione e il Provider di virtualizzazione rete reti logiche

Gli host devono avere accesso alla rete logica gestione e la rete logica Provider di virtualizzazione rete di calcolo fisiche tutti. Se le reti logiche utilizzano VLAN, gli host di calcolo fisiche necessario essere connessi a una porta del commutatore trunked che ha accesso a questi VLAN. Analogamente, le schede di rete fisica dell'host di calcolo non devono essere VLAN filtri attivato. Se si utilizza Teaming switch-Embedded (SET) e sono più membri del team NIC (ad esempio schede di rete) negli host di calcolo, è necessario connettere tutti i membri del team NIC per quel particolare host allo stesso dominio di trasmissione livello 2.  
  
Per indirizzo IP ai fini della pianificazione, ogni host fisico calcolo deve disporre di almeno un indirizzo IP assegnato della rete logica di gestione. Il controller di rete assegna automaticamente esattamente due indirizzi IP della rete logica Provider di virtualizzazione rete. Se l'host di calcolo fisiche è in esecuzione macchine virtuali di un'infrastruttura aggiuntiva (ad esempio, Controller di rete, SLB/MUX o Gateway) tale host deve disporre di un indirizzo IP aggiuntivo assegnato della rete logica di gestione per ogni infrastruttura macchine virtuali ospitate.   
  
Inoltre, ogni macchina virtuale di infrastruttura SLB/MUX deve disporre di un indirizzo IP riservato della rete logica Provider di virtualizzazione rete. 

>[!IMPORTANT]
>Questi indirizzi IP SLB/MUX devono essere assegnati all'esterno di pool di indirizzi IP che è configurato per la rete logica Provider di virtualizzazione rete. In caso contrario si potrebbero indirizzi IP duplicati nella rete. 

Il Controller di rete richiede un indirizzo riservato dalla rete di gestione come l'indirizzo IP REST. È necessario creare manualmente il record HOST DNS per l'indirizzo IP REST.  
  
Un server DHCP può assegnare automaticamente indirizzi IP per rete di gestione oppure è possibile assegnare manualmente l'indirizzo IP statico. Lo stack di SDN assegna automaticamente indirizzi IP per la rete di provider di virtualizzazione rete per i singoli host Hyper-V da un Pool IP specificati tramite e gestiti dal Controller di rete.   
  
L'amministratore dell'infrastruttura in modo statico assegna gli indirizzi IP Provider virtualizzazione rete utilizzati da SLB/MUX tramite script di PowerShell o di VMM. Il Controller di rete assegna un indirizzo IP del Provider di virtualizzazione rete in un host fisico calcolo solo dopo che l'agente Host Controller di rete riceve i criteri di rete per una macchina virtuale tenant specifico.  
  
#### <a name="sample-network-topology"></a>Esempio di topologia di rete

Personalizzare i prefissi di subnet, gli ID VLAN e indirizzi IP del gateway in base alle linee guida dell'amministratore di rete.  
  
Nome di rete|Subnet|Maschera|ID VLAN nel trunk|Gateway|Prenotazioni<br />(esempi)  
----------------|----------|--------|--------------------|-----------|-----------------------------  
|**Gestione**|10.184.108.0|24|7|10.184.108.1|10.184.108.1 - router<br /><br />10.184.108.4 - Controller di rete<br /><br />10.184.108.10 - host di calcolo 1<br /><br />10.184.108.11 - host di calcolo 2<br /><br />10.184.108.X - host di calcolo X  
|**Provider di virtualizzazione rete**|10.10.56.0|23|11|10.10.56.1|10.10.56.1 - router<br /><br />10.10.56.2 - SLB/MUX1  
  
### <a name="logical-networks-for-gateways-and-the-software-load-balancer"></a>Reti logiche per gateway e il bilanciamento del carico Software
  
Altre reti logiche devono essere creati e il provisioning per l'utilizzo SLB e gateway. Ancora una volta, è necessario contattare l'amministratore di rete per ottenere i prefissi IP corretti, gli ID VLAN e indirizzi IP del gateway di tali reti.

#### <a name="transit-logical-network"></a>Rete logica di transito
  
Il Gateway RAS e SLB/MUX utilizzare la rete logica di transito per lo scambio di informazioni di peering BGP e traffico tenant (esterno interno) Nord/Sud. Le dimensioni di questa subnet corrisponderà in genere più piccoli rispetto agli altri. Solo gli host di calcolo fisiche che eseguono Gateway RAS o macchine virtuali SLB/MUX necessario disporre della connettività a questa subnet con questi VLAN trunked e accessibile su porte del commutatore a cui le schede di rete il calcolo degli host sono connessi. Ogni SLB/MUX o una macchina virtuale Gateway RAS in modo statico viene assegnato un indirizzo IP della rete logica di transito.

#### <a name="public-vip-logical-network"></a>Rete logica VIP pubblica  
  
La rete logica VIP pubblico è necessario disporre i prefissi di subnet IP che sono instradabili di fuori dell'ambiente cloud (in genere Internet instradabile).  Questi sarà gli indirizzi IP front-end utilizzati dai client esterni di accedere alle risorse nelle reti virtuali inclusi front-end VIP per il gateway da sito a sito.

#### <a name="private-vip-logical-network"></a>Rete logica VIP privata
  
La rete logica VIP privato non è necessaria essere instradabile di fuori di cloud come viene utilizzato per gli indirizzi VIP che sono accessibili solo dal client di cloud interno, ad esempio le SLB Mananger o i servizi privati.
  
#### <a name="gre-vip-logical-network"></a>Rete logica GRE VIP

La rete VIP GRE è una subnet presente esclusivamente per la definizione di indirizzi VIP che sono assegnate alle macchine virtuali gateway in esecuzione nell'infrastruttura di SDN per un tipo di connessione S2S GRE. La rete non necessario per essere preconfigurato nel tuo router commutatori fisici e e non dispone di una VLAN assegnata.   

### <a name="sample-network-topology"></a>Esempio di topologia di rete

Personalizzare i prefissi di subnet, gli ID VLAN e indirizzi IP del gateway in base alle linee guida dell'amministratore di rete.  
  
Nome di rete|Subnet|Maschera|ID VLAN nel trunk|Gateway|Prenotazioni<br />(esempi)  
----------------|----------|--------|--------------------|-----------|-----------------------------  
|**Trasporti pubblici**|10.10.10.0|24|10|10.10.10.1|10.10.10.1 - router  
|**Pubblica VIP**|41.40.40.0|27|NA|41.40.40.1|41.40.40.1 - router<br /> 41.40.40.2 - SLB/MUX VIP<br />41.40.40.3 - VIP VPN S2S di IPSec  
|**Privato VIP**|20.20.20.0|27|NA|20.20.20.1|20.20.20.1 - GW predefinito (router)  
|**GRE VIP**|31.30.30.0|24|NA|31.30.30.1|31.30.30.1 - GW predefinito|  
  
### <a name="logical-networks-required-for-rdma-based-storage"></a>Reti logiche necessari per l'archiviazione basata su RDMA  
  
Se non si RDMA tramite l'archiviazione basata su, quindi è necessario definire una VLAN e subnet per ogni scheda fisica nell'host di calcolo e archiviazione. In genere hai due schede fisiche per ogni nodo per questa configurazione.  
  
> [!IMPORTANT]  
> Commutatori fisici più richiedono traffico RDMA inviati in una VLAN con tag nell'ordine della qualità delle impostazioni del servizio venga applicato correttamente.  Non inserire il traffico RDMA su un tag VLAN o su una porta in modalità accesso fisico.  
  
Nome di rete  |Subnet  |Maschera  |ID VLAN nel trunk  |Gateway  |Prenotazioni<br />(esempi)    
---------|---------|---------|---------|---------|---------  
**Storage1**     |    10.60.36.0     | 25        |   8      |  10.60.36.1       |  10.60.36.1 - router<br />10.60.36.x - host di calcolo x<br />10.60.36.y - y host di calcolo<br />10.60.36.v - cluster di calcolo<br />10.60.36.w - cluster di archiviazione  
|**Storage2**|10.60.36.128|25|9|10.60.36.129|10.60.36.129 - router<br />10.60.36.x - host di calcolo x<br />10.60.36.y - y host di calcolo<br />10.60.36.v - cluster di calcolo<br />10.60.36.w - cluster di archiviazione  
  
Per ulteriori informazioni sulla configurazione delle opzioni, vedere il **esempi di configurazione** sezione.  
  
## <a name="routing-infrastructure"></a>Infrastruttura di routing  
  
Se si esegue la distribuzione dell'infrastruttura SDN tramite script, la gestione, virtualizzazione rete Provider, trasporti pubblici, e subnet di indirizzi VIP devono essere instradabile sulla rete fisica.     
  
Informazioni di routing \ (ad esempio hop\ Avanti) per l'indirizzo VIP subnet viene annunciata dal SLB/MUX e gateway RAS in rete fisica usando il peering BGP interna. Le reti logiche VIP non dispone di una VLAN assegnata e non preconfigurate nel commutatore livello 2 (ad esempio, il commutatore Top-of-Rack).  
  
È necessario creare un peer BGP nel router che viene utilizzato dall'infrastruttura SDN per ricevere le route per le reti logiche VIP annunciate dal SLB/MUXes e gateway RAS. Il peering BGP solo deve verificarsi un modo (da SLB/MUX o Gateway RAS per i peer BGP esterno).  Di sopra di primo livello di routing è possibile utilizzare route statiche o un altro protocollo di routing dinamico, ad esempio OSPF, tuttavia, come affermato in precedenza, il prefisso di subnet IP per le reti logiche di indirizzi VIP devono essere instradabile dalla rete fisica per il peer BGP esterno.   
  
Il peering BGP in genere è configurato in un commutatore gestito o un router come parte dell'infrastruttura di rete. Il peer BGP potrebbe inoltre essere configurato in un Server Windows con il ruolo Server di accesso remoto (RAS) installato in una modalità solo Routing. Il peer di router BGP nell'infrastruttura di rete deve essere configurato per il proprio ASN e consentano peering da ASN assegnato ai componenti SDN \ (SLB/MUX e RAS Gateways\). Da router fisico o l'amministratore di rete nel controllo di tale router, è necessario ottenere le informazioni seguenti:

- Router ASN  
- Indirizzo IP del router  
- ASN per l'utilizzo da componenti SDN (può essere qualsiasi numero AS dall'intervallo ASN privata)

>[!NOTE]
>ASN a quattro byte non sono supportati da SLB/MUX. È necessario allocare due byte ASN SLB/MUX e il router wo che si connette. È possibile utilizzare a 4 byte ASN altrove nel proprio ambiente.  
  
Utente o dall'amministratore di rete è necessario configurare il peer di router BGP per accettare connessioni da parte di ASN e l'indirizzo IP o un indirizzo di subnet della rete logica trasporti pubblici che utilizzano il gateway RAS e SLB/MUXes.
  
Per ulteriori informazioni, vedere [Border Gateway Protocol & #40; BGP & #41;](../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md).
  
## <a name="default-gateways"></a>Gateway predefiniti

Macchine che sono configurate per connettersi a più reti, ad esempio host fisici e macchine virtuali gateway devono avere solo un gateway predefinito configurato. Il gateway predefinito verrà in genere configurato nella scheda utilizzata per raggiungere completamente a Internet.

Per le macchine virtuali, utilizzare le regole seguenti per decidere quale rete da utilizzare come gateway predefinito:

1. Utilizzare la rete di transito come gateway predefinito se una macchina virtuale è connessa alla rete trasporti pubblici o se è multihomed in transito e qualsiasi altra rete.  
2. Utilizzare la rete di gestione come gateway predefinito se una macchina virtuale è connesso solo alla rete di gestione.  
3.  La rete di virtualizzazione rete Provider non deve essere utilizzata come gateway predefinito. Il solo le macchine virtuali connesse a questa rete sarà il SLB/MUXes e il gateway RAS.  
4.  Le macchine virtuali non verranno mai connesse direttamente alle reti Storage1, Storage2, VIP pubblico o privato VIP.  
  
Per gli host Hyper-V e i nodi di archiviazione, usare la rete di gestione come gateway predefinito.  Le reti di archiviazione non è necessario un gateway predefinito assegnato.
  
## <a name="network-hardware"></a>Hardware di rete

È possibile utilizzare le sezioni seguenti per pianificare la distribuzione di hardware di rete.

### <a name="network-interface-cards-nics"></a>Schede di interfaccia di rete (NIC)

Per ottenere prestazioni ottimali, le schede di interfaccia di rete che usi nell'host Hyper-V e host di archiviazione sono necessarie funzionalità specifiche.  
 
Diretta accesso memoria remota (RDMA) è un kernel ignorare tecnica che consente di trasferire grandi quantità di dati senza coinvolgere CPU dell'host. Poiché il motore DMA sulla scheda di rete esegue il trasferimento, la CPU non viene utilizzata per il movimento di memoria.  In questo modo la CPU per eseguire altre operazioni.  

Switch Embedded Teaming (SET) è un'alternativa soluzione gruppo NIC che è possibile utilizzare in ambienti che includono Hyper-V e lo stack di rete SDN (Software Defined) in Windows Server 2016. SET di alcune funzionalità gruppo NIC si integra il commutatore virtuale Hyper-V.

Per ulteriori informazioni, vedere [Remote Direct Memory Access & #40; RDMA & #41; e passare gruppo incorporato & #40; Imposta & #41;](../../../virtualization//hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  

Per soddisfare il carico del traffico di rete virtuale tenant dovuto intestazioni di incapsulamento VXLAN o NVGRE, è necessario impostare il valore MTU della rete dell'infrastruttura di livello 2 (commutatori e host) a maggiore o uguale a byte 1674 \ (inclusi headers\ Ethernet di livello 2). Schede di rete che supportano il nuovo *EncapOverhead* scheda Avanzate parola chiave verrà impostato il valore MTU automaticamente tramite il controller di rete agente Host. Schede di rete che non supportano il nuovo *EncapOverhead* parola chiave è necessario impostare manualmente la dimensione MTU in ogni host fisico mediante il *JumboPacket* \(or equivalent\) parola chiave.

### <a name="switches"></a>Commutatori
  
Durante la selezione di un commutatore fisico e i router per l'ambiente assicurarsi che supporta il set di funzionalità seguente.  

- Switchport MTU impostazioni \(required\)  
- Impostare MTU > = byte 1674 \ (inclusi Header\ L2 Ethernet)  
- L3 protocolli \(required\)  
- ECMP  
- ECMP \(IETF RFC 4271\)\-based BGP

Le implementazioni devono supportare le istruzioni deve negli standard IETF seguenti.

- RFC 2545: "BGP-4 multiprotocollo estensioni per IPv6 Inter-Domain Routing"  
- RFC 4760: "estensioni multiprotocollo per BGP-4"  
- RFC 4893: "supporto BGP per quattro ottetti AS numero spazio"  
- RFC 4456: "BGP Route Reflection: un'alternativa a Full Mesh interno BGP (IBGP)"  
- RFC 4724: "riavvio normale meccanismo per BGP"  

Sono necessari i seguenti protocolli di assegnazione di tag.

- VLAN - isolamento tra diversi tipi di traffico
- 802.1 Q trunk

Gli elementi seguenti forniscono il controllo di collegamento.

- Qualità del servizio \ (obbligatorio solo se si usa RoCE\ base alla priorità)
- Enhanced traffico \(802.1Qaz\) selezione
- Controllo di flusso basato sulla priorità \ (802.1 p/Q e 802.1Qbb\)

Gli elementi seguenti forniscono disponibilità e la ridondanza.

- Disponibilità di commutatore (obbligatoria)
- Per eseguire funzioni di gateway è necessario un router a disponibilità elevata. È possibile farlo tramite un router switch\ multi-chassis o tecnologie come VRRP.
        
Gli elementi seguenti forniscono funzionalità di gestione.

**Monitoraggio**

- SNMP v1 o v2 SNMP (obbligatorio se si usa il Controller di rete per il monitoraggio commutatore fisico)  
- MIB SNMP \ (obbligatorio se si utilizza il Controller di rete per monitoring\ commutatore fisico)  
- MIB-II (RFC 1213), LLDP, interfaccia MIB \(RFC 2863\), se-MIB, IP-MIB, IP-FORWARD-MIB, Q-BRIDGE-MIB, BRIDGE MIB, LLDB MIB, entità MIB, IEEE8023-ritardo-MIB  
  
I diagrammi seguenti mostrano un esempio di installazione di quattro nodi. Per motivi di chiarezza, il primo diagramma mostra il controller di rete, la seconda consente di visualizzare il controller di rete più il bilanciamento del carico software e il terzo diagramma mostra il controller di rete, bilanciamento del carico software e il gateway.  
  
VNICs e reti di archiviazione non sono shonwn in questi diagrammi. Se si prevede di usare l'archiviazione basata su SMB, queste connessioni sono necessarie.    
  
Le macchine virtuali sia l'infrastruttura e tenant può essere ridistribuite tra qualsiasi host di calcolo fisiche (presupponendo che esista la connettività di rete corrette per le reti logiche corrette).  
  
### <a name="network-controller-deployment"></a>Distribuzione del Controller di rete

Prima di distribuire Controller di rete, è necessario verificare l'installazione e requisiti software, nonché configurare gruppi di sicurezza e la registrazione DNS dinamica. Per ulteriori informazioni, vedere [installazione e i requisiti di preparazione per la distribuzione di Controller di rete](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md).

Il programma di installazione è a disponibilità elevata con tre nodi di Controller di rete configurati in macchine virtuali. Viene inoltre illustrato due tenant con rete virtuale del Tenant 2 suddivisa in due subnet virtuale per simulare un livello web e un livello di database.  

![Pianificazione di SDN NC](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-NC-Planning.png)

### <a name="network-controller-and-software-load-balancer-deployment"></a>Distribuzione del bilanciamento del caricamento di software e controller di rete

Per la disponibilità elevata, esistono due o più nodi SLB/MUX.
   
![Pianificazione di SDN NC](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-SLB-Deployment.png)
  
### <a name="network-controller-software-load-balancer-and-ras-gateway-deployment"></a>Distribuzione di Controller di rete, bilanciamento del carico Software e Gateway RAS

Esistono tre macchine virtuali gateway; due sono attivi e uno è ridondante.

![Pianificazione di SDN NC](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-GW-Deployment.png)  
  
  
  
Per l'automazione della distribuzione basata su TP5, Active Directory deve essere disponibile e raggiungibile da queste subnet. Per ulteriori informazioni su Active Directory, vedere [Panoramica di servizi di dominio di Active Directory](https://technet.microsoft.com/en-us/library/mt703721.aspx).  
  
>[!IMPORTANT] 
>Se si distribuisce tramite VMM, verificare le macchine virtuali di infrastruttura (Server VMM, Active Directory/DNS, SQL Server, e così via) non sono ospitati in uno qualsiasi dei quattro host illustrato nei diagrammi.  
  
## <a name="switch-configuration-examples"></a>Esempi di configurazione di commutatore  
  
Per facilitare la configurazione del commutatore fisico o del router, sono disponibili in un set di file di configurazione di esempio per un'ampia gamma di modelli di commutatore e fornitori di [repository Github SDN Microsoft](https://github.com/microsoft/SDN/tree/master/SwitchConfigExamples). Vengono forniti un file Leggimi dettagliate e i comandi testata interfaccia della riga di comando (CLI) per i parametri specifici.         
  
  
## <a name="compute"></a>Calcolo  
Tutti gli host Hyper-V devono disporre di Windows Server 2016 già installato, abilitato Hyper-V e un commutatore virtuale esterno a Hyper-V creato con almeno una scheda fisica connessa alla rete logica di gestione. L'host deve essere raggiungibile tramite un indirizzo IP di gestione assegnato al vNIC Host di gestione.  
  
È possibile utilizzare qualsiasi tipo di archiviazione compatibile con Hyper-V, locale o condivisa.   
  
> [!TIP]  
> È utile se si utilizza lo stesso nome per tutti i commutatori virtuali, ma non è obbligatorio. Se si prevede di distribuire con script, vedere il commento associato il `vSwitchName`variabile nel file config.psd1.  
  
**Requisiti di calcolo di host**  
Nella tabella seguente illustra i requisiti minimi di hardware e software per gli host fisici quattro utilizzati nella distribuzione di esempio.  
  
Host|Requisiti hardware|Requisiti software|  
--------|-------------------------|-------------------------  
|Host Hyper-v fisici|4 core, 2,66 GHz CPU<br /><br />32 GB di RAM<br /><br />300 GB di spazio su disco<br /><br />1 Gb/s (o più veloce) scheda di rete fisica|Sistema operativo: Windows Server 2016<br /><br />Ruolo Hyper-V installato|  
  
  
**Requisiti di ruolo macchina virtuale dell'infrastruttura SDN**  
  
Ruolo|Requisiti di CPU virtuali|Requisiti di memoria|Requisiti del disco|  
--------|---------------------|-----------------------|---------------------  
|Controller di rete (tre nodi)|4 Vcpu|4 GB min (8 GB consigliati)|75 GB per l'unità del sistema operativo  
|SLB/MUX (tre nodi)|8 Vcpu|Si consiglia di 8 GB|75 GB per l'unità del sistema operativo  
|Gateway RAS<br /><br />(singolo pool di gateway di tre nodi, attivo due, uno passivo)|8 Vcpu|Si consiglia di 8 GB|75 GB per l'unità del sistema operativo  
|Router BGP Gateway RAS per il peering SLB/MUX<br /><br />(in alternativa utilizzare commutatore ToR come Router BGP)|2 Vcpu|2 GB|75 GB per l'unità del sistema operativo|  
  
  
Se si utilizza VMM per la distribuzione, sono necessari per VMM e un'altra infrastruttura SDN non risorse della macchina virtuale un'infrastruttura aggiuntiva. Per ulteriori informazioni, vedere [requisiti Hardware minimi per System Center Technical Preview.](https://technet.microsoft.com/library/dn997303.aspx)  
  
## <a name="extending-your-infrastructure"></a>Estensione dell'infrastruttura  
I requisiti di dimensionamento e risorse per l'infrastruttura dipendono dalle macchine virtuali tenant carico di lavoro che si prevede di ospitare. La CPU, memoria e i requisiti di disco per le macchine virtuali di infrastruttura (ad esempio: gateway di rete controller, SLB, e così via) sono elencati nella tabella precedente. È possibile aggiungere più di queste macchine virtuali dell'infrastruttura di scalabilità orizzontale in base alle esigenze. Tutte le macchine virtuali tenant in esecuzione negli host Hyper-V però i propri della CPU, memoria e i requisiti di disco che è necessario considerare.   
  
Quando le macchine virtuali di carico di lavoro tenant iniziare a usare troppe risorse negli host Hyper-V fisici, è possibile estendere l'infrastruttura aggiungendo altri host fisici. Questa operazione può essere eseguita con Virtual Machine Manager o tramite script di PowerShell (a seconda di come inizialmente per la distribuzione dell'infrastruttura) per creare nuove risorse di server tramite il controller di rete. Se si desidera aggiungere altri indirizzi IP per la rete di virtualizzazione rete Provider, è possibile creare nuove subnet logica (con pool IP corrispondenti) che è possono utilizzare gli host.  
  
  
## <a name="see-also"></a>Vedere anche  
[Installazione e i requisiti di preparazione per la distribuzione di Controller di rete](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  
[Software Defined Networking & #40; SDN & #41;](../Software-Defined-Networking--SDN-.md)  
  


