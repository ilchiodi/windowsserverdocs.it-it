---
title: Pianificare un'infrastruttura Software Defined Network
description: In questo argomento vengono fornite informazioni su come pianificare la distribuzione di infrastruttura di rete SDN (Software Defined).
manager: dougkim
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
ms.date: 08/10/2018
ms.openlocfilehash: 16511628e979a95433360b0a3e900a89e9ba56eb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446270"
---
# <a name="plan-a-software-defined-network-infrastructure"></a>Pianificare un'infrastruttura Software Defined Network

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Informazioni sulla pianificazione della distribuzione per un'infrastruttura Software Defined Networking, inclusi i prerequisiti hardware e software. 


## <a name="prerequisites"></a>Prerequisiti
In questo argomento vengono descritti numerosi prerequisiti hardware e software, tra cui:

-   **Configurare i gruppi di sicurezza, percorsi dei file di log e la registrazione dinamica DNS** è necessario preparare il tuo Data Center per la distribuzione di Controller di rete, che richiede uno o più computer o macchine virtuali e un computer o macchina virtuale. Prima di distribuire Controller di rete, è necessario configurare i gruppi di sicurezza, percorsi dei file di log (se necessario) e la registrazione dinamica DNS.  Se non è stato creato il tuo Data Center per la distribuzione di Controller di rete, vedere [requisiti di installazione e preparazione per la distribuzione di Controller di rete](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md) per informazioni dettagliate.

-   **Rete fisica** è necessario accedere a dispositivi della rete fisica per configurare le VLAN, Routing, BGP, Data Center Bridging (ETS) se si usa una tecnologia RDMA, e Data Center Bridging (base alla priorità) se si usa un RoCE basato su tecnologia RDMA. Questo argomento viene illustrato configurazione commutatore manuali nonché di Peering BGP affidano ai commutatori di livello 3 / router o una macchina virtuale di Routing e il Server di accesso remoto (RRAS).   

-   **Gli host di calcolo fisiche** gli host eseguono Hyper-V e sono necessari alle macchine virtuali tenant e dell'infrastruttura host SDN.  In questi host per prestazioni ottimali, che è descritti più avanti in, è necessario hardware rete specifica la **hardware rete** sezione.  


## <a name="physical-network-and-compute-host-configuration"></a>Configurazione di host di calcolo e rete fisica

Ogni host fisico di calcolo richiede la connettività di rete tramite uno o più schede di rete collegate a una o più porte commutatore fisico.  Un livello 2 [VLAN](https://en.wikipedia.org/wiki/Virtual_LAN) supporta reti suddivise in più segmenti di rete logica. 

>[!TIP]
>Usare VLAN 0 per le reti logiche in modalità di accesso o senza codifica. 

>[!IMPORTANT]
>Windows Server 2016 Software Defined Networking supporta gli indirizzi IPv4 per il controllo overlay e Metti il sotto. IPv6 non è supportato.

### <a name="logical-networks"></a>Reti logiche

#### <a name="management-and-hnv-provider"></a>Gestione e del Provider HNV 

Tutti gli host fisico di calcolo devono accedere a rete logica di gestione e la rete logica Provider HNV.  Per l'indirizzo IP ai fini della pianificazione, ogni host fisico di calcolo deve avere almeno un indirizzo IP assegnato dalla rete logica di gestione. Il controller di rete richiede un indirizzo IP riservato da usare come l'indirizzo IP REST. 

Un server DHCP può assegnare automaticamente indirizzi IP per la rete di gestione, oppure è possibile assegnare manualmente indirizzi IP statici. Lo stack SDN assegna automaticamente gli indirizzi IP per il Provider HNV rete logica per i singoli host Hyper-V da un Pool IP specificati tramite e gestite dal Controller di rete. 

>[!NOTE]
>Il Controller di rete viene assegnato un indirizzo IP del Provider HNV in un host fisico di calcolo solo dopo che l'agente Host di Controller di rete riceve i criteri di rete per una macchina virtuale tenant specifico. 


|                                                               Se...                                                               |                                                                                                                                                                          Quindi...                                                                                                                                                                           |
|-----------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                  Le reti logiche usano alcuna VLAN,                                                  |                                                                 l'host fisico di calcolo è necessario connettersi a una porta di commutazione trunked può accedere a queste reti VLAN. È importante notare che le schede di rete fisica nell'host computer necessario non avere alcun filtro VLAN attivato.                                                                 |
|                Usa Switched-Embedded Teaming (SET) e dispone di più membri del team NIC, ad esempio le schede di rete                |                                                                                                                        è necessario connettere tutti i membri del team NIC per quel particolare host dello stesso dominio di broadcast di livello 2.                                                                                                                         |
| L'host fisico di calcolo è in esecuzione le macchine virtuali di un'infrastruttura aggiuntiva, ad esempio Controller di rete, SLB/MUX o un Gateway, | tale host deve disporre di un altro indirizzo IP assegnato dalla rete logica di gestione per ognuna delle macchine virtuali ospitate.<p>Inoltre, ogni macchina virtuale dell'infrastruttura SLB/MUX deve avere un indirizzo IP riservato per la rete logica Provider HNV. In assenza di un indirizzo IP riservato può comportare gli indirizzi IP duplicati nella rete. |

---

Per informazioni su Hyper-V rete virtualizzazione, che è possibile usare la virtualizzazione di reti in una distribuzione SDN Microsoft, vedere [virtualizzazione rete Hyper-V](../technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md).  



#### <a name="gateways-and-the-software-load-balancer"></a>I gateway e il bilanciamento del carico Software

Reti logiche aggiuntive devono essere creati e il provisioning per i gateway e l'utilizzo di bilanciamento del carico software. Assicurarsi di ottenere i prefissi IP corretti, gli ID VLAN e indirizzi IP del gateway per queste reti.


|                                 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **Rete logica di transito**   | Il Gateway RAS e SLB/MUX usare la rete logica di transito per lo scambio di informazioni relative al peering BGP e il traffico del tenant (esterno a interno) Nord/Sud. Le dimensioni di questa subnet sarà in genere più piccoli rispetto agli altri. Solo gli host fisico di calcolo che eseguono Gateway RAS o macchine virtuali SLB/MUX necessario disporre di connettività a questa subnet con le VLAN trunked e accessibile per le porte di commutazione a cui sono connesse le schede di rete degli host di calcolo. Ogni SLB/MUX o macchina virtuale Gateway RAS in modo statico viene assegnato un indirizzo IP dalla rete logica di transito. |
| **Rete logica VIP pubblici**  |                                                                                                                             La rete logica VIP pubblico è necessario per i prefissi di subnet IP che sono instradabili all'esterno dell'ambiente cloud (in genere Internet instradabili).  Questi saranno gli indirizzi IP front-end usati dai client esterni di accedere alle risorse nelle reti virtuali tra il front-end, VIP per il gateway da sito a sito.                                                                                                                             |
| **Rete logica VIP privati** |                                                                                                                                                                                       La rete logica VIP privati non devono essere instradabili all'esterno del cloud perché è usato per gli indirizzi VIP che sono accessibili solo dai client interni cloud, ad esempio il Manager di bilanciamento del carico software o i servizi privati.                                                                                                                                                                                       |
|   **Rete logica VIP GRE**   |                                                                                                                                           La rete VIP GRE è una subnet che esiste esclusivamente per la definizione di indirizzi VIP assegnati alle macchine virtuali gateway in esecuzione nell'infrastruttura SDN per un tipo di connessione S2S GRE. Questa rete non devono essere pre-configurate in commutatori fisici o nel router e non necessitano di una VLAN assegnata.                                                                                                                                            |

---


#### <a name="sample-network-topology"></a>Topologia di rete di esempio
Modificare i prefissi di subnet IP di esempio e gli ID VLAN per l'ambiente. 


| **Nome di rete** |  **Subnet**  | **Maschera** | **ID VLAN nel carrello** | **Gateway**  |                                                           **Prenotazioni (esempi)**                                                           |
|------------------|--------------|----------|----------------------|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
|    Management    | 10.184.108.0 |    24    |          7           | 10.184.108.1 | 10.184.108.1 – Router10.184.108.4 - rete Controller10.184.108.10 - calcolo host 110.184.108.11 - calcolo ospitare 210.184.108.X - host di calcolo X |
|   HNV Provider   |  10.10.56.0  |    23    |          11          |  10.10.56.1  |                                                    10.10.56.1 – Router10.10.56.2 - SLB/MUX1                                                     |
|     Transito      |  10.10.10.0  |    24    |          10          |  10.10.10.1  |                                                               10.10.10.1 – router                                                               |
|    Indirizzo VIP pubblico    |  41.40.40.0  |    27    |          N/D          |  41.40.40.1  |                                    41.40.40.1 – Router41.40.40.2 SLB/MUX VIP41.40.40.3 - IPSec S2S VPN VIP                                    |
|   VIP privati    |  20.20.20.0  |    27    |          N/D          |  20.20.20.1  |                                                        20.20.20.1 - GW predefinito (router)                                                         |
|     VIP GRE      |  31.30.30.0  |    24    |          N/D          |  31.30.30.1  |                                                             31.30.30.1 - GW predefinito                                                             |

---

### <a name="logical-networks-required-for-rdma-based-storage"></a>Reti logiche necessarie per l'archiviazione basata su RDMA  

Se si usa l'archiviazione basata su RDMA, definire una VLAN e subnet per ogni scheda fisica (due schede per ogni nodo) negli host di calcolo e archiviazione.  

>[!IMPORTANT]
>Per la qualità del servizio (QoS) in modo appropriato da applicare, commutatori fisici richiedono una VLAN con tag per il traffico RDMA.

| **Nome di rete** |  **Subnet**  | **Maschera** | **ID VLAN nel carrello** | **Gateway**  |                                                           **Prenotazioni (esempi)**                                                            |
|------------------|--------------|----------|----------------------|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
|     storage1     |  10.60.36.0  |    25    |          8           |  10.60.36.1  |  10.60.36.1 – router<p>10.60.36.X - host di calcolo X<p>10.60.36.Y - host di calcolo Y<p>10.60.36.V - cluster di calcolo<p>10.60.36.W - cluster di archiviazione  |
|     storage2     | 10.60.36.128 |    25    |          9           | 10.60.36.129 | 10.60.36.129 – router<p>10.60.36.X - host di calcolo X<p>10.60.36.Y - host di calcolo Y<p>10.60.36.V - cluster di calcolo<p>10.60.36.W - cluster di archiviazione |

---


## <a name="routing-infrastructure"></a>Infrastruttura di routing  

Se si distribuisce l'infrastruttura SDN tramite script, le subnet di gestione, Provider HNV, transito e VIP devono essere instradabile tra loro nella rete fisica.     

Le informazioni di routing \(hop successivo, ad esempio\) per l'indirizzo VIP subnet viene annunciato il SLB/MUX e un gateway RAS alla rete fisica tramite il peering BGP interno. Le reti logiche VIP non hanno una VLAN assegnata e non è già configurate nel commutatore livello 2 (ad esempio commutatore Top-of-Rack).  

È necessario creare un peer BGP sul router che viene usato dall'infrastruttura SDN per ricevere le route per le reti logiche VIP annunciate dal SLB/MUX e un gateway RAS. Il peering BGP solo deve essere eseguito uno dei modi (da SLB/MUX o un Gateway RAS con peer BGP esterno).  Sopra il livello prima del routing è possibile usare le route statiche o un altro protocollo di routing dinamico, ad esempio OSPF, tuttavia, come indicato in precedenza, è necessario il prefisso di subnet IP per le reti logiche VIP siano instradabili dalla rete fisica per il peer BGP esterno.   

Il peering BGP viene in genere configurato in un commutatore gestito o un router come parte dell'infrastruttura di rete. Il peer BGP potrebbe anche essere configurato in un Server Windows con il ruolo Server accesso remoto (RAS) installato in modalità solo Routing. Questo peer router BGP nell'infrastruttura di rete deve essere configurato per avere un proprio numero ASN e consentire il peering da un numero ASN assegnato per i componenti SDN \(SLB/MUX e i gateway RAS\). Da fisico router o l'amministratore di rete nel controllo di tale router, è necessario ottenere le informazioni seguenti:

- Router ASN  
- Indirizzo IP del router  
- ASN per l'utilizzo per i componenti SDN (può essere qualsiasi numero AS dall'intervallo ASN privato)

>[!NOTE]
>ASN a quattro byte non sono supportati da SLB/MUX. È necessario allocare ASN a due byte per SLB/MUX e il router wo che si connette. È possibile usare numeri ASN a 4 byte in un' posizione nell'ambiente in uso.  

Utente o l'amministratore di rete deve configurare il peer router BGP affinché accetti connessioni dal numero ASN e l'indirizzo IP o l'indirizzo di subnet della rete logica di transito che usano il gateway RAS e le istanze MUX SLB /.

Per altre informazioni, vedere [protocollo BGP (Border Gateway)](../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md).

## <a name="default-gateways"></a>Gateway predefiniti
Le macchine che sono configurate per connettersi a più reti, ad esempio gli host fisici e macchine virtuali gateway devono avere solo un gateway predefinito configurato. Configurare il gateway predefinito in adapter utilizzato per accedere a Internet.

Per le macchine virtuali, seguire queste regole per decidere quali rete da utilizzare come gateway predefinito:

1. Usare la rete logica di transito del gateway predefinito se una macchina virtuale si connette alla rete di transito o se è multihomed per la rete di transito o qualsiasi altro tipo di rete.
2. Usare la rete di gestione come gateway predefinito se una macchina virtuale si connette solo alla rete di gestione. 
3. Usare la rete Provider HNV per le istanze MUX SLB/e gateway RAS. Non usare la rete Provider HNV come un gateway predefinito. 
4. Non si connettono macchine virtuali direttamente alle reti Storage1, Storage2, indirizzo VIP pubblico o privato.

Per gli host Hyper-V e i nodi di archiviazione, usare la rete di gestione come gateway predefinito.  Le reti di archiviazione non devono mai avere un gateway predefinito assegnato.


## <a name="network-hardware"></a>Hardware di rete

È possibile usare le sezioni seguenti per pianificare la distribuzione di hardware di rete.

### <a name="network-interface-cards-nics"></a>Schede interfaccia di rete (NIC)

Le schede di interfaccia di rete (NIC) utilizzate nell'host Hyper-V e host di archiviazione richiedono funzionalità specifiche per ottenere prestazioni ottimali. 

Direct accesso memoria remota (RDMA) è un kernel ignorare la tecnica che consente di trasferire grandi quantità di dati senza l'utilizzo di CPU, che quindi libera di eseguire altre operazioni dell'host. 

Switch Embedded Teaming (SET) è un'alternativa soluzione gruppo NIC che è possibile usare in ambienti che includono Hyper-V e lo stack di rete SDN (Software Defined) in Windows Server 2016. SET integra alcune funzionalità gruppo NIC nel commutatore virtuale Hyper-V. 

Per altre informazioni, vedere [diretta accesso memoria remota (RDMA) e Switch Embedded Teaming (SET)](../../../virtualization//hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).   

Per soddisfare il sovraccarico nel traffico di rete virtuale tenant causato dalle intestazioni di incapsulamento NVGRE o VXLAN, il MTU della rete dell'infrastruttura di livello 2 (commutatori e host) deve essere impostato su maggiore o uguale a byte 1674 \(inclusi Ethernet di livello 2 intestazioni\). 

Interfacce di rete che supportano il nuovo *EncapOverhead* parola chiave avanzata adapter imposta il valore MTU automaticamente tramite il controller di rete dell'agente Host. Schede di rete che non supportano il nuovo *EncapOverhead* parola chiave è necessario impostare la dimensione MTU manualmente in ogni host fisico usando la *JumboPacket* \(o equivalente\) (parola chiave). 


### <a name="switches"></a>Commutatori

Quando si seleziona un commutatore fisico e un router per l'ambiente, assicurarsi che supporta il set di funzionalità seguente:  

- Le impostazioni di MTU Switchport \(obbligatorio\)  
- Il valore MTU impostato su > = byte 1674 \(inclusione dell'intestazione L2 Ethernet\)  
- Protocolli L3 \(obbligatorio\)  
- ECMP  
- BGP \(IETF RFC 4271\)\-basato ECMP

Le implementazioni devono supportare le istruzioni devono negli standard IETF seguenti.

- RFC 2545: "Estensioni Multiprotocol BGP-4 per IPv6 Interdomain Routing"  
- RFC 4760: "Multiprotocol estensioni per il protocollo BGP-4"  
- RFC 4893: "Il supporto BGP per quattro ottetti AS Number spazio"  
- RFC 4456: "BGP Route Reflection: Un'alternativa a Full Mesh interno BGP (IBGP)"  
- RFC 4724: "Riavvio normale meccanismo per BGP"  

I protocolli di assegnazione di tag seguenti sono necessari.

- VLAN - isolamento di vari tipi di traffico
- 802.1q trunk

Gli elementi seguenti forniscono il controllo di collegamento.

- Qualità del servizio \(obbligatorio solo se si usa RoCE base alla priorità\)
- Migliorato il traffico selezione \(802.1Qaz\)
- Controllo di flusso basato sulla priorità \(802.1x p o Q e 802.1Qbb\)

Gli elementi seguenti forniscono disponibilità e ridondanza.

- Disponibilità del commutatore (obbligatoria)
- Un router a disponibilità elevata è necessario per eseguire funzioni di gateway. È possibile farlo mediante tecnologie quali e VRRP o un router switch\ multi chassis.

Gli elementi seguenti offrono funzionalità di gestione.

**Il monitoraggio**

- SNMP v1 o v2 SNMP (obbligatorio se si usa il Controller di rete per il monitoraggio commutatore fisico)  
- MIB SNMP \(necessari se si utilizza il Controller di rete per il monitoraggio commutatore fisico\)  
- MIB-II (RFC 1213), LLDP, interfaccia MIB \(RFC 2863\), IF-MIB, IP-MIB, IP-FORWARD-MIB, Q-BRIDGING-MIB, BRIDGE MIB, LLDB MIB, Entity-MIB, IEEE8023-intervallo-MIB  

I diagrammi seguenti mostrano una configurazione di quattro nodi di esempio. Per motivi di chiarezza, nella prima figura mostra solo il controller di rete, il secondo mostra il controller di rete e il bilanciamento del carico software e il terzo diagramma mostra il controller di rete, bilanciamento del carico software e il gateway.  

Questi diagrammi non vengono visualizzate schede e reti di archiviazione. Se si prevede di usare l'archiviazione basata su SMB, questi sono necessari.

Le macchine virtuali tenant sia l'infrastruttura può essere ridistribuite in qualsiasi host fisico di calcolo (presupponendo che esista la connettività di rete corretto per le reti logiche corrette).  



## <a name="switch-configuration-examples"></a>Esempi di configurazione del commutatore  

Per agevolare la configurazione del commutatore fisico o del router, sono disponibili in un set di file di configurazione di esempio per un'ampia gamma di fornitori e i modelli di commutatore il [repository Github Microsoft SDN](https://github.com/microsoft/SDN/tree/master/SwitchConfigExamples). Vengono forniti un file readme dettagliato e comandi testati command line interface (CLI) per le opzioni specifiche.         


## <a name="compute"></a>Calcolo  
Tutti gli host Hyper-V devono avere Windows Server 2016 già installato, abilitato Hyper-V e un commutatore virtuale Hyper-V esterno creato con almeno una scheda fisica connessa alla rete logica di gestione. L'host deve essere raggiungibile tramite un indirizzo IP di gestione assegnato per la scheda di rete virtuale Host di gestione.  

È possibile utilizzare qualsiasi tipo di archiviazione compatibile con Hyper-V, condivisa o locale.   

> [!TIP]  
> È utile se si usa lo stesso nome per tutti i commutatori virtuali, ma non è obbligatorio. Se si intende distribuire con gli script, vedere il commento associato il `vSwitchName` variabile nel file config.psd1.  

**Requisiti di calcolo di host**  
La tabella seguente illustra i requisiti hardware e software minimi per i quattro host fisici usati nella distribuzione di esempio.  

Hyper-V|Requisiti hardware|Requisiti software|  
--------|-------------------------|-------------------------  
|Host fisico Hyper-v|CPU 4 core 2,66 GHz<br /><br />32 GB di RAM<br /><br />300 GB di spazio su disco<br /><br />1 Gb/s (o superiore) scheda di rete fisica|OS: Windows Server 2016<br /><br />Ruolo Hyper-V installato|  


**Requisiti del ruolo macchina virtuale dell'infrastruttura SDN**  

Ruolo|requisiti di vCPU|Requisiti della memoria|Requisiti dei dischi|  
--------|---------------------|-----------------------|---------------------  
|Controller di rete (tre nodi)|4 Vcpu|4 GB min (8 GB consigliati)|75 GB per unità del sistema operativo  
|SLB/MUX (tre nodi)|8 Vcpu|8 GB consigliati|75 GB per unità del sistema operativo  
|Gateway RAS<br /><br />(solo pool di gateway di tre nodi, due attivo, una passiva)|8 Vcpu|8 GB consigliati|75 GB per unità del sistema operativo  
|Router BGP Gateway RAS per il peering di SLB/MUX<br /><br />(in alternativa usare commutatore ToR come Router BGP)|2 CPU virtuali|2 GB|75 GB per unità del sistema operativo|  


Se si usa VMM per la distribuzione, sono necessari per VMM e altri elementi dell'infrastruttura non SDN risorse delle macchine virtuali di un'infrastruttura aggiuntiva. Per altre informazioni, vedere [suggerimenti sui requisiti Hardware minimi per System Center Technical Preview.](https://technet.microsoft.com/library/dn997303.aspx)  

## <a name="extending-your-infrastructure"></a>Estendendo l'infrastruttura  
I requisiti di ridimensionamento e risorse per l'infrastruttura dipendono dalle macchine virtuali del carico di lavoro tenant che si intende ospitare. La CPU, memoria e i requisiti del disco per le macchine virtuali dell'infrastruttura (ad esempio: gateway di rete controller, bilanciamento del carico software, e così via) sono elencati nella tabella precedente. È possibile aggiungere più di queste macchine virtuali dell'infrastruttura per la scalabilità orizzontale in base alle esigenze. Tuttavia, tutte le macchine virtuali tenant in esecuzione negli host Hyper-V hanno i propri CPU, memoria e i requisiti del disco che è necessario prendere in considerazione.   

Quando le macchine virtuali del carico di lavoro tenant inizia a consumerà troppe risorse negli host Hyper-V fisici, è possibile estendere l'infrastruttura aggiungendo altri host fisici. Ciò può essere eseguita con Virtual Machine Manager o usando gli script di PowerShell (a seconda del modo in cui è distribuito inizialmente l'infrastruttura) per creare nuove risorse di server tramite il controller di rete. Se è necessario aggiungere altri indirizzi IP per la rete Provider HNV, è possibile creare nuove subnet logica (con pool IP corrispondente) che è possono usare gli host.  


## <a name="see-also"></a>Vedere anche  
[Requisiti di installazione e preparazione per la distribuzione di Controller di rete](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  
[Software Defined Networking &#40;SDN&#41;](../Software-Defined-Networking--SDN-.md)  



