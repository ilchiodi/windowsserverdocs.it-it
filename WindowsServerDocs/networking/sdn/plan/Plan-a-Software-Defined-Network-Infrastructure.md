---
title: Pianificare un'infrastruttura Software Defined Network
description: Questo argomento fornisce informazioni su come pianificare la distribuzione dell'infrastruttura SDN (software defined Network).
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.service: virtual-network
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: ea7e53c8-11ec-410b-b287-897c7aaafb13
ms.author: lizross
author: eross-msft
ms.date: 08/10/2018
ms.openlocfilehash: 83f94d3770c475fca7f5d4b8cc2f5a5ade1a20d7
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317460"
---
# <a name="plan-a-software-defined-network-infrastructure"></a>Pianificare un'infrastruttura Software Defined Network

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Informazioni sulla pianificazione della distribuzione per un'infrastruttura di rete definita dal software, inclusi i prerequisiti hardware e software. 


## <a name="prerequisites"></a>Prerequisiti
In questo argomento vengono descritti alcuni prerequisiti hardware e software, tra cui:

-   **Gruppi di sicurezza, percorsi di file di log e registrazione DNS dinamici configurati** È necessario preparare il Data Center per la distribuzione del controller di rete, che richiede uno o più computer o macchine virtuali e un computer o una macchina virtuale. Prima di poter distribuire il controller di rete, è necessario configurare i gruppi di sicurezza, i percorsi dei file di log (se necessario) e la registrazione DNS dinamica.  Se il Data Center non è stato preparato per la distribuzione del controller di rete, vedere [requisiti di installazione e preparazione per la distribuzione del controller di rete](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md) per informazioni dettagliate.

-   **Rete fisica**  È necessario accedere ai dispositivi di rete fisica per configurare VLAN, routing, BGP, Data Center Bridging (ETS) se si usa una tecnologia RDMA e Data Center Bridging (PFC) se si usa una tecnologia RDMA basata su RoCE. Questo argomento illustra la configurazione del commutatore manuale e il peering BGP su commutatori/router di livello 3 o una macchina virtuale del server di routing e accesso remoto (RRAS).   

-   **Host di calcolo fisico**  Questi host eseguono Hyper-V e sono necessari per ospitare le macchine virtuali di infrastruttura SDN e tenant.  L'hardware di rete specifico è necessario in questi host per ottenere prestazioni ottimali, come descritto più avanti nella sezione **hardware di rete** .  


## <a name="physical-network-and-compute-host-configuration"></a>Configurazione della rete fisica e dell'host di calcolo

Ogni host di calcolo fisico richiede la connettività di rete tramite una o più schede di rete collegate a una o più porte di commutazione fisiche.  Una [VLAN](https://en.wikipedia.org/wiki/Virtual_LAN) di livello 2 supporta le reti divise in più segmenti di rete logica. 

>[!TIP]
>Usare la VLAN 0 per le reti logiche in modalità di accesso o senza tag. 

>[!IMPORTANT]
>Windows Server 2016 Software Defined Networking supporta l'indirizzamento IPv4 per il sottoposto e la sovrimpressione. IPv6 non è supportato.

### <a name="logical-networks"></a>Reti logiche

#### <a name="management-and-hnv-provider"></a>Provider di gestione e HNV 

Tutti gli host di calcolo fisici devono accedere alla rete logica di gestione e alla rete logica del provider HNV.  Per scopi di pianificazione degli indirizzi IP, ogni host di calcolo fisico deve avere almeno un indirizzo IP assegnato dalla rete logica di gestione. Il controller di rete richiede un indirizzo IP riservato che funga da indirizzo IP REST. 

Un server DHCP può assegnare automaticamente gli indirizzi IP per la rete di gestione oppure è possibile assegnare manualmente un indirizzo IP statico. Lo stack SDN assegna automaticamente gli indirizzi IP per la rete logica del provider HNV per i singoli host Hyper-V da un pool IP specificato tramite e gestito dal controller di rete. 

>[!NOTE]
>Il controller di rete assegna un indirizzo IP del provider HNV a un host di calcolo fisico solo dopo che l'agente host del controller di rete riceve i criteri di rete per una macchina virtuale tenant specifica. 


|                                                               Relazione                                                               |                                                                                                                                                                          Necessità di ripristino                                                                                                                                                                           |
|-----------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                  Le reti logiche utilizzano le VLAN,                                                  |                                                                 l'host di calcolo fisico deve connettersi a una porta di commutazione trunk che ha accesso a queste VLAN. È importante notare che le schede di rete fisiche nell'host del computer non devono avere alcun filtro VLAN attivato.                                                                 |
|                Con Switched-Embedded Teaming (SET) e con più membri del team NIC, ad esempio schede di rete,                |                                                                                                                        è necessario connettere tutti i membri del gruppo NIC per quel particolare host allo stesso dominio broadcast di livello 2.                                                                                                                         |
| L'host di calcolo fisico esegue macchine virtuali di infrastruttura aggiuntive, ad esempio controller di rete, SLB/MUX o gateway. | l'host deve disporre di un indirizzo IP aggiuntivo assegnato dalla rete logica di gestione per ogni macchina virtuale ospitata.<p>Inoltre, ogni macchina virtuale dell'infrastruttura SLB/MUX deve avere un indirizzo IP riservato per la rete logica del provider HNV. La mancata presenza di un indirizzo IP riservato può comportare l'uso di indirizzi IP duplicati nella rete. |

---

Per informazioni su virtualizzazione rete Hyper-V (HNV), che è possibile usare per virtualizzare le reti in una distribuzione di Microsoft SDN, vedere [virtualizzazione rete Hyper-v](../technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md).  



#### <a name="gateways-and-the-software-load-balancer"></a>Gateway e software Load Balancer

È necessario creare ed eseguire il provisioning di reti logiche aggiuntive per il gateway e l'utilizzo di SLB. Assicurarsi di ottenere i prefissi IP corretti, gli ID VLAN e gli indirizzi IP del gateway per queste reti.


|                                 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **Rete logica di transito**   | Il gateway RAS e SLB/MUX usano la rete logica di transito per scambiare informazioni sul peering BGP e il traffico del tenant nord/sud (esterno-interno). Le dimensioni di questa subnet saranno in genere inferiori alle altre. Solo gli host di calcolo fisici che eseguono il gateway RAS o le macchine virtuali SLB/MUX devono disporre della connettività a questa subnet con le VLAN configurate e accessibili nelle porte di commutazione a cui sono connesse le schede di rete degli host di calcolo. A ogni macchina virtuale SLB/MUX o gateway RAS viene assegnato in modo statico un indirizzo IP dalla rete logica di transito. |
| **Rete logica VIP pubblici**  |                                                                                                                             Per la rete logica VIP pubblica è necessario disporre di prefissi di subnet IP instradabili al di fuori dell'ambiente cloud, in genere instradabile su Internet.  Si tratta degli indirizzi IP front-end usati dai client esterni per accedere alle risorse nelle reti virtuali, incluso il VIP front-end per il gateway da sito a sito.                                                                                                                             |
| **Rete logica VIP privata** |                                                                                                                                                                                       Non è necessario che la rete logica VIP privata sia instradabile all'esterno del cloud, perché viene usata per gli indirizzi VIP accessibili solo da client cloud interni, ad esempio SLB Manager o servizi privati.                                                                                                                                                                                       |
|   **Rete logica VIP GRE**   |                                                                                                                                           La rete VIP GRE è una subnet che esiste esclusivamente per la definizione di indirizzi VIP assegnati alle macchine virtuali del gateway in esecuzione nell'infrastruttura SDN per un tipo di connessione S2S GRE. Questa rete non deve essere preconfigurata nei commutatori fisici o nel router e non è necessario assegnare una VLAN.                                                                                                                                            |

---


#### <a name="sample-network-topology"></a>Topologia di rete di esempio
Modificare i prefissi di subnet IP di esempio e gli ID VLAN per l'ambiente in uso. 


| **Nome di rete** |  **Subnet**  | **Maschera** | **ID VLAN su camion** | **Gateway**  |                                                           **Prenotazioni (esempi)**                                                           |
|------------------|--------------|----------|----------------------|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
|    Gestione    | 10.184.108.0 |    24    |          7           | 10.184.108.1 | 10.184.108.1-router 10.184.108.4-controller di rete 10.184.108.10-Compute host 110.184.108.11-Compute host 210.184.108. X-Compute host X |
|   Provider HNV   |  10.10.56.0  |    23    |          11          |  10.10.56.1  |                                                    10.10.56.1-router 10.10.56.2-SLB/MUX1                                                     |
|     Transito      |  rete 10.10.10.0  |    24    |          10          |  10.10.10.1  |                                                               10.10.10.1-router                                                               |
|    Indirizzo VIP pubblico    |  41.40.40.0  |    27    |          N/D          |  41.40.40.1  |                                    41.40.40.1-router 41.40.40.2-SLB/MUX VIP 41.40.40.3-IPSec S2S VPN VIP                                    |
|   VIP privato    |  20.20.20.0  |    27    |          N/D          |  20.20.20.1  |                                                        20.20.20.1-default GW (router)                                                         |
|     VIP GRE      |  31.30.30.0  |    24    |          N/D          |  31.30.30.1  |                                                             31.30.30.1-valore predefinito: GW                                                             |

---

### <a name="logical-networks-required-for-rdma-based-storage"></a>Reti logiche richieste per l'archiviazione basata su RDMA  

Se si usa l'archiviazione basata su RDMA, definire una VLAN e una subnet per ogni scheda fisica (due schede per nodo) negli host di calcolo e di archiviazione.  

>[!IMPORTANT]
>Per applicare correttamente la qualità del servizio (QoS), i commutatori fisici richiedono una VLAN con tag per il traffico RDMA.

| **Nome di rete** |  **Subnet**  | **Maschera** | **ID VLAN su camion** | **Gateway**  |                                                           **Prenotazioni (esempi)**                                                            |
|------------------|--------------|----------|----------------------|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
|     Storage1     |  10.60.36.0  |    25    |          8           |  10.60.36.1  |  10.60.36.1-router<p>10.60.36. x-calcolo host X<p>10.60.36. y-calcolo host Y<p>10.60.36. V-cluster di calcolo<p>10.60.36. W-cluster di archiviazione  |
|     Storage2     | 10.60.36.128 |    25    |          9           | 10.60.36.129 | 10.60.36.129-router<p>10.60.36. x-calcolo host X<p>10.60.36. y-calcolo host Y<p>10.60.36. V-cluster di calcolo<p>10.60.36. W-cluster di archiviazione |

---


## <a name="routing-infrastructure"></a>Infrastruttura di routing  

Se si distribuisce l'infrastruttura SDN usando gli script, le subnet di gestione, provider HNV, transito e VIP devono essere instradabili tra loro nella rete fisica.     

Le informazioni di routing \(ad esempio\) di hop successivo per le subnet VIP sono annunciate dai gateway SLB/MUX e RAS nella rete fisica usando il peering BGP interno. Alle reti logiche VIP non è assegnata una VLAN e non è preconfigurata nell'opzione di livello 2 (ad esempio, Commuter Top-of-rack).  

È necessario creare un peer BGP nel router usato dall'infrastruttura SDN per ricevere le route per le reti logiche VIP annunciate dai gateway SLB/mux e RAS. Il peering BGP deve essere eseguito solo in un modo, da SLB/MUX o dal gateway RAS al peer BGP esterno.  Al di sopra del primo livello di routing è possibile usare route statiche o un altro protocollo di routing dinamico, ad esempio OSPF. Tuttavia, come indicato in precedenza, il prefisso della subnet IP per le reti logiche VIP deve essere instradabile dalla rete fisica al peer BGP esterno.   

Il peering BGP viene in genere configurato in un switch o un router gestito come parte dell'infrastruttura di rete. Il peer BGP può anche essere configurato in un server Windows con il ruolo server di accesso remoto installato in una modalità di solo routing. Questo peer router BGP nell'infrastruttura di rete deve essere configurato in modo da avere il proprio ASN e consentire il peering da un ASN assegnato ai componenti SDN \(SLB/MUX e gateway RAS\). È necessario ottenere le informazioni seguenti dal router fisico o dall'amministratore di rete in controllo del router:

- ASN router  
- Indirizzo IP router  
- ASN per l'uso da parte dei componenti SDN (può essere qualsiasi numero AS dall'intervallo ASN privato)

>[!NOTE]
>ASN a quattro byte non supportati da SLB/MUX. È necessario allocare due ASN di byte a SLB/MUX e al router wo a cui si connette. È possibile usare ASN a 4 byte in un'altra posizione nell'ambiente.  

L'utente o l'amministratore di rete deve configurare il peer del router BGP per accettare le connessioni dall'ASN e dall'indirizzo IP o dall'indirizzo della subnet della rete logica di transito usato dal gateway RAS e da SLB/MUX.

Per ulteriori informazioni, vedere [Border Gateway Protocol (BGP)](../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md).

## <a name="default-gateways"></a>Gateway predefiniti
I computer configurati per la connessione a più reti, ad esempio gli host fisici e le macchine virtuali gateway, devono avere un solo gateway predefinito configurato. Configurare il gateway predefinito sulla scheda utilizzata per raggiungere Internet.

Per le macchine virtuali, attenersi alle regole seguenti per decidere quale rete usare come gateway predefinito:

1. Usare la rete logica di transito come gateway predefinito se una macchina virtuale si connette alla rete di transito o se è multihomed per la rete di transito o per qualsiasi altra rete.
2. Utilizzare la rete di gestione come gateway predefinito se una macchina virtuale si connette solo alla rete di gestione. 
3. Usare la rete del provider HNV per i gateway SLB/mux e RAS. Non usare la rete del provider HNV come gateway predefinito. 
4. Non connettere le macchine virtuali direttamente a storage1, Storage2, VIP pubblici o reti VIP private.

Per gli host Hyper-V e i nodi di archiviazione, usare la rete di gestione come gateway predefinito.  Alle reti di archiviazione non deve mai essere assegnato un gateway predefinito.


## <a name="network-hardware"></a>Hardware di rete

È possibile usare le sezioni seguenti per pianificare la distribuzione dell'hardware di rete.

### <a name="network-interface-cards-nics"></a>Schede interfaccia di rete (NIC)

Le schede di interfaccia di rete (NIC) usate negli host Hyper-V e negli host di archiviazione richiedono funzionalità specifiche per ottenere prestazioni ottimali. 

Accesso diretto a memoria remota (RDMA) è una tecnica di bypass del kernel che consente di trasferire grandi quantità di dati senza usare la CPU dell'host, che libera la CPU per eseguire altre operazioni. 

Switch Embedded Teaming (SET) è una soluzione di gruppo NIC alternativa che è possibile usare in ambienti che includono Hyper-V e lo stack SDN (Software Defined Networking) in Windows Server 2016. SET integra alcune funzionalità di gruppo NIC nel Commuter virtuale Hyper-V. 

Per ulteriori informazioni, vedere [accesso diretto a memoria remota (RDMA) e switch Embedded Teaming (set)](../../../virtualization//hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).   

Per tenere conto dell'overhead nel traffico di rete virtuale del tenant causato dalle intestazioni di incapsulamento VXLAN o NVGRE, il valore MTU della rete dell'infrastruttura di livello 2 (commutatori e host) deve essere impostato su un valore maggiore o uguale a 1674 byte \(incluse le intestazioni Ethernet di livello 2\). 

Le schede di rete che supportano la nuova parola chiave *EncapOverhead* Advanced Adapter imposta la MTU automaticamente tramite l'agente host del controller di rete. Le schede di rete che non supportano la nuova parola chiave *EncapOverhead* devono impostare manualmente le dimensioni MTU in ogni host fisico usando la parola chiave *JumboPacket* \(o equivalente\). 


### <a name="switches"></a>Switch

Quando si seleziona un Commuter fisico e un router per l'ambiente, assicurarsi che supporti il set di funzionalità seguente:  

- Le impostazioni MTU switchport \(obbligatorie\)  
- MTU impostato su > = 1674 byte \(inclusa l'intestazione L2-Ethernet\)  
- \(necessari i protocolli L3\)  
- ECMP  
- BGP \(IETF RFC 4271\)ECMP basato su \-

Le implementazioni devono supportare le istruzioni MUST negli standard IETF seguenti.

- RFC 2545: "estensioni multiprotocollo BGP-4 per il routing tra domini IPv6"  
- RFC 4760: "estensioni multiprotocollo per BGP-4"  
- RFC 4893: "supporto BGP per quattro ottetti come spazio numerico"  
- RFC 4456: "Reflection Route BGP: un'alternativa a full mesh Internal BGP (IBGP)"  
- RFC 4724: "meccanismo di riavvio normale per BGP"  

Sono necessari i seguenti protocolli di assegnazione di tag.

- VLAN-isolamento di diversi tipi di traffico
- trunk 802.1 q

Gli elementi seguenti forniscono il controllo dei collegamenti.

- La qualità del servizio \(PFC è necessaria solo se si usa RoCE\)
- Selezione del traffico migliorata \(802.1 qaz\)
- Controllo di flusso basato sulla priorità \(802.1 p/Q e 802.1 QBB\)

Gli elementi seguenti forniscono disponibilità e ridondanza.

- Cambia disponibilità (obbligatorio)
- Un router a disponibilità elevata è necessario per eseguire le funzioni del gateway. A tale scopo, è possibile usare un switch a più chassis, ovvero tecnologie quali VRRP.

Gli elementi seguenti forniscono funzionalità di gestione.

**Monitoraggio**

- SNMP v1 o SNMP v2 (obbligatorio se si usa il controller di rete per il monitoraggio del Commuter fisico)  
- SNMP MIB \(obbligatorio se si usa il controller di rete per il monitoraggio del Commuter fisico\)  
- MIB-II (RFC 1213), LLDP, Interface MIB \(RFC 2863\), IF-MIB, IP-MIB, IP-INOLTR-MIB, Q-BRIDGE-MIB, BRIDGE-MIB, LLDB-MIB, Entity-MIB, IEEE8023-LAG-MIB  

I diagrammi seguenti mostrano una configurazione di esempio a quattro nodi. Per maggiore chiarezza, il primo diagramma mostra solo il controller di rete, il secondo Visualizza il controller di rete più il bilanciamento del carico software e il terzo diagramma mostra il controller di rete, il servizio di bilanciamento del carico software e il gateway.  

Questi diagrammi non mostrano le reti di archiviazione e schede. Se si prevede di usare l'archiviazione basata su SMB, queste sono obbligatorie.

Sia l'infrastruttura che le macchine virtuali tenant possono essere ridistribuite in qualsiasi host di calcolo fisico (presupponendo che sia presente la connettività di rete corretta per le reti logiche corrette).  



## <a name="switch-configuration-examples"></a>Esempi di configurazione switch  

Per configurare il Commuter o il router fisico, è disponibile un set di file di configurazione di esempio per un'ampia gamma di modelli di switch e fornitori nel [repository GitHub di Microsoft Sdn](https://github.com/microsoft/SDN/tree/master/SwitchConfigExamples). Sono disponibili un file Leggimi dettagliato e i comandi dell'interfaccia della riga di comando (CLI) testati per commutatori specifici.         


## <a name="compute"></a>Calcolo  
Per tutti gli host Hyper-V deve essere installato Windows Server 2016, Hyper-V abilitato e un commutire virtuale Hyper-V esterno creato con almeno una scheda fisica connessa alla rete logica di gestione. L'host deve essere raggiungibile tramite un indirizzo IP di gestione assegnato all'host di gestione vNIC.  

È possibile utilizzare qualsiasi tipo di archiviazione compatibile con Hyper-V, Shared o local.   

> [!TIP]  
> È utile usare lo stesso nome per tutti i commutatori virtuali, ma non è obbligatorio. Se si prevede di eseguire la distribuzione con script, vedere il commento associato alla variabile `vSwitchName` nel file config. psd1.  

**Requisiti di calcolo host**  
Nella tabella seguente vengono indicati i requisiti hardware e software minimi per i quattro host fisici utilizzati nella distribuzione di esempio.  

Host|Requisiti hardware|Requisiti software|  
--------|-------------------------|-------------------------  
|Host Hyper-v fisico|CPU 4 Core 2,66 GHz<br /><br />32 GB di RAM<br /><br />300 GB di spazio su disco<br /><br />scheda di rete fisica 1 GB/s (o superiore)|Sistema operativo: Windows Server 2016<br /><br />Ruolo Hyper-V installato|  


**Requisiti del ruolo macchina virtuale dell'infrastruttura SDN**  

Role|requisiti di vCPU|Requisiti della memoria|Requisiti dei dischi|  
--------|---------------------|-----------------------|---------------------  
|Controller di rete (tre nodi)|4 vCPU|4 GB min (8 GB consigliati)|75 GB per l'unità del sistema operativo  
|SLB/MUX (tre nodi)|8 vCPU|8 GB consigliati|75 GB per l'unità del sistema operativo  
|Gateway RAS<br /><br />(singolo pool di gateway a tre nodi, due attivo, uno passivo)|8 vCPU|8 GB consigliati|75 GB per l'unità del sistema operativo  
|Router BGP del gateway RAS per il peering di SLB/MUX<br /><br />(in alternativa, usare il commutatore ToR come router BGP)|2 vCPU|2 GB|75 GB per l'unità del sistema operativo|  


Se si usa VMM per la distribuzione, sono necessarie risorse aggiuntive per la macchina virtuale di infrastruttura per VMM e altre infrastrutture non SDN. Per ulteriori informazioni, vedere la pagina relativa alle [raccomandazioni minime hardware per System Center Technical Preview.](https://technet.microsoft.com/library/dn997303.aspx)  

## <a name="extending-your-infrastructure"></a>Estensione dell'infrastruttura  
I requisiti di dimensionamento e risorse per l'infrastruttura dipendono dalle macchine virtuali del carico di lavoro tenant che si prevede di ospitare. Nella tabella precedente sono elencati i requisiti della CPU, della memoria e del disco per le macchine virtuali dell'infrastruttura, ad esempio controller di rete, SLB, gateway e così via. È possibile aggiungere altre macchine virtuali dell'infrastruttura per la scalabilità orizzontale in base alle esigenze. Tuttavia, le macchine virtuali tenant in esecuzione negli host Hyper-V hanno i requisiti di CPU, memoria e disco che è necessario prendere in considerazione.   

Quando le macchine virtuali del carico di lavoro del tenant iniziano a usare un numero eccessivo di risorse negli host Hyper-V fisici, è possibile estendere l'infrastruttura aggiungendo host fisici aggiuntivi. Questa operazione può essere eseguita con Virtual Machine Manager o tramite script di PowerShell (a seconda di come è stata inizialmente distribuita l'infrastruttura) per creare nuove risorse server tramite il controller di rete. Se è necessario aggiungere altri indirizzi IP per la rete del provider HNV, è possibile creare nuove subnet logiche (con Pool IP corrispondenti) che gli host possono usare.  


## <a name="see-also"></a>Vedi anche  
[Requisiti di installazione e preparazione per la distribuzione del controller di rete](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  
[Software Defined Networking &#40;Sdn&#41;](../Software-Defined-Networking--SDN-.md)  



