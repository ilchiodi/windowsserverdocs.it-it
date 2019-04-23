---
title: Dettagli tecnici di virtualizzazione rete Hyper-V in Windows Server 2016
description: In questo argomento fornisce informazioni tecniche su virtualizzazione rete Hyper-V in Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: virtual-network
ms.suite: na
ms.technology:
- networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9efe0231-94c1-4de7-be8e-becc2af84e69
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8db47479d0c6e6014a7df6cecd59b1e87920b76a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887552"
---
# <a name="hyper-v-network-virtualization-technical-details-in-windows-server-2016"></a>Dettagli tecnici di virtualizzazione rete Hyper-V in Windows Server 2016

>Si applica a:  Windows Server 2016

La virtualizzazione dei server consente l'esecuzione contemporanea di più istanze di server, comunque isolate tra di loro, su un unico host fisico. Ogni macchina virtuale, in pratica, opera come se fosse l'unico server in esecuzione sul computer fisico.  
  
Virtualizzazione di rete offre una funzionalità simile, in cui più reti virtuali (potenzialmente con indirizzi IP sovrapposti) eseguite sulla stessa infrastruttura di rete fisica e ogni rete virtuale funziona come se fosse l'unica rete virtuale in esecuzione sull'infrastruttura di rete condivisa. Nella Figura 1 viene mostrata questa relazione.  
  
![confronto tra virtualizzazione di server e virtualizzazione di rete](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF1.gif)  
  
Figura 1: confronto tra virtualizzazione di server e virtualizzazione di rete  
  
## <a name="hyper-v-network-virtualization-concepts"></a>Concetti di Virtualizzazione rete Hyper-V  
In Hyper-V rete Virtualizzazione, un cliente o del tenant viene definito come il "proprietario" di un set di subnet IP che vengono distribuiti in un enterprise o datacenter. Un cliente può essere una società o organizzazione con più reparti o business unit in un Data Center privato che richiedono l'isolamento della rete o un tenant in un centro dati pubblico che è ospitato da un provider di servizi. Ogni cliente può disporre di uno o più [le reti virtuali](#VirtualNetworks) nel datacenter e ogni virtuali rete costituita da uno o più [subnet virtuale](#VirtualSubnets).  
  
Esistono due implementazioni di virtualizzazione che saranno disponibili in Windows Server 2016: HNVv1 e HNVv2.  
  
-   **HNVv1**  
  
    HNVv1 è compatibile con Windows Server 2012 R2 e System Center 2012 R2 Virtual Machine Manager (VMM). Configurazione per HNVv1 si basa su gestione WMI e i cmdlet di Windows PowerShell (facilitati tramite System Center VMM) per definire le impostazioni di isolamento e indirizzo cliente (CA) - rete virtuale - mapping di indirizzo fisico (PA) e routing. Nessuna funzionalità aggiuntiva è stati aggiunti a HNVv1 in Windows Server 2016 e non include nuove funzionalità sono previsti.  
    
    • IMPOSTARE gruppo NIC e HNV V1 non sono compatibili dalla piattaforma.
    
    o per usare gli utenti del gateway a disponibilità elevata NVGRE necessario a una delle due utilizzare gruppo LBFO o alcun team. Oppure
    
    i gateway o utilizzare il Controller di rete distribuiti con SET raggruppate commutatore.

  
-   **HNVv2**  
  
    Un numero significativo di nuove funzionalità è inclusi in HNVv2 che viene implementato utilizzando Azure virtuale filtro piattaforma (VFP) estensione nel commutatore Hyper-V di inoltro. HNVv2 è completamente integrato con Microsoft Azure Stack che include il nuovo Controller di rete nello Stack di rete SDN (Software Defined).  Criteri di rete virtuale viene definito tramite Microsoft [Controller di rete](../../../sdn/technologies/network-controller/Network-Controller.md) utilizzando un RESTful NorthBound (NB) API e bloccato all'avvio di un agente Host tramite più SouthBound Intefaces (SBI) inclusi OVSDB. L'agente Host programmi criteri nell'estensione VFP del commutatore Hyper-V in cui viene applicato.  
  
    > [!IMPORTANT]  
    > In questo argomento è incentrato su HNVv2.  
  
### <a name="VirtualNetworks"></a>Rete virtuale  
  
-   Ogni rete virtuale è costituito da uno o più subnet virtuali. Una rete virtuale costituisce un limite di isolamento in cui le macchine virtuali all'interno di una rete virtuale possono comunicare solo tra loro. In genere, è stato applicato questo isolamento con VLAN 802.1 q e un intervallo di indirizzi IP segregato Tag o ID VLAN. Ma con virtualizzazione di RETE, isolamento viene applicata utilizzando incapsulamento NVGRE o VXLAN per creare reti sovrapposizione con la possibilità di sovrapposizione subnet IP tra i clienti o i tenant.  
  
-   Ogni rete virtuale con un univoco dominio ID Routing (RDID) nell'host. Questo RDID corrisponde approssimativamente a un ID di risorsa per identificare la rete virtuale risorse REST nel Controller di rete. La rete virtuale risorse REST viene fatto riferimento tramite un Uniform Resource Identifier (URI) di spazio dei nomi con l'ID di risorsa aggiunto.  
  
### <a name="VirtualSubnets"></a>Subnet virtuali  
  
-   Le subnet virtuali implementano la semantica della subnet IP livello 3 relative alle macchine virtuali della stessa subnet virtuale. La subnet virtuale costituisce un dominio di broadcast (simile a una VLAN) e isolamento viene applicata utilizzando il campo Identificatore rete VXLAN (VNI) o di NVGRE Tenant rete ID (TNI).  
  
-   Ogni subnet virtuale appartiene a una singola rete virtuale (RDID) e viene assegnato un unique ID Subnet virtuale (VSID) utilizzando la TNI o VNI nell'intestazione del pacchetto incapsulato. Il VSID deve essere univoco all'interno del centro dati ed è compreso nell'intervallo tra 4096 e 2 ^ 24-2.  
  
Un vantaggio fondamentale della rete virtuale e del dominio di routing è che consente ai clienti di portare le proprie topologie di rete (ad esempio, le subnet IP) per il cloud. Nella Figura 2 viene mostrato un esempio in cui la società Contoso dispone di due reti separate: quella per la ricerca e lo sviluppo (R&D) e quella per le vendite. Poiché dispongono di ID di dominio di routing diversi, queste due reti non possono interagire. In altre parole, la rete per la ricerca e lo sviluppo della società Contoso è isolata da quella per le vendite, anche se entrambe sono di proprietà della stessa società. La rete Contoso per la ricerca e lo sviluppo include tre subnet virtuali. Si noti come l'RDID e il VSID siano entrambi univoci all'interno di un centro dati.  
  
![reti clienti e subnet virtuali](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF6.gif)  
  
Figura 2: reti clienti e subnet virtuali  
  
**Inoltro di livello 2**  
  
Nella figura 2, le macchine virtuali in VSID 5001 possono essere i pacchetti inoltrati alle macchine virtuali che sono anche in VSID 5001 attraverso il commutatore Hyper-V. I pacchetti in ingresso provenienti da una macchina virtuale in VSID 5001 vengono inviati a un VPort specifico del commutatore Hyper-V. Regole in entrata (ad esempio incapsulamento) e i mapping (ad esempio intestazione incapsulamento) vengono applicati dal commutatore Hyper-V per tali pacchetti. I pacchetti vengono quindi inoltrati a un diverso VPort nel commutatore Hyper-V (se la macchina virtuale di destinazione è collegata allo stesso host) o a un altro commutatore Hyper-V in un host diverso (se la macchina virtuale di destinazione si trova in un host diverso).  
  
**Livello 3 Routing**  
  
Analogamente, le macchine virtuali in VSID 5001 sono i pacchetti indirizzati alle macchine virtuali in VSID 5002 o VSID 5003 dal router distribuito di virtualizzazione RETE presente nel VSwitch di ciascun host Hyper-V. Passare alla distribuzione del pacchetto per il ruolo Hyper-V, virtualizzazione RETE aggiorna il VSID del pacchetto in ingresso con il VSID della macchina virtuale di destinazione. Questo si verifica solo se entrambi i VSID si trovano nello stesso RDID.  Di conseguenza, le schede di rete virtuali con RDID1 non possono inviare pacchetti alle schede di rete virtuali con RDID2 senza attraversamento di un gateway.  
  
> [!NOTE]  
> Nella descrizione del flusso dei pacchetti precedente, il termine "macchina virtuale" significa la scheda di rete virtuale nella macchina virtuale. Generalmente, le macchine virtuali dispongono di una sola scheda di rete virtuale. In questo caso, le parole "macchina virtuale" e "scheda di rete virtuale" concettualmente possibile con lo stesso significato.  
  
Ogni subnet virtuale definisce una subnet IP livello 3 e un limite di dominio di trasmissione livello 2 (L2) simile a una VLAN. Quando una macchina virtuale trasmette un pacchetto, virtualizzazione RETE utilizza replica Unicast (UR) per creare una copia del pacchetto originale e sostituire l'indirizzo IP di destinazione e MAC con gli indirizzi di ogni macchina Virtuale che presenti lo stesso VSID.  
  
> [!NOTE]  
> Quando viene fornito in Windows Server 2016, trasmissione e la subnet multicast viene implementata tramite una replica unicast. Non sono supportati IGMP e il routing multicast tra subnet.  
  
Oltre a funzionare come dominio broadcast, il VSID garantisce l'isolamento. Una scheda di rete virtuale in virtualizzazione RETE è connessa a una porta del commutatore Hyper-V che disporrà di regole ACL applicate direttamente alla porta (risorsa REST virtualNetworkInterface) o per la subnet virtuale (VSID) che fa parte.  
  
La porta del commutatore Hyper-V deve disporre di una regola ACL applicata. L'ACL può CONSENTIRE all, DENY ALL o essere più specifici per consentire solo a determinati tipi di traffico in base alla corrispondenza di 5 tuple (IP di origine, IP di destinazione, porta di origine, porta di destinazione, protocollo).  
  
> [!NOTE]  
> Le estensioni del commutatore Hyper-V non funzionerà con HNVv2 nello stack di nuove reti SDN (Software Defined). HNVv2 viene implementato utilizzando l'estensione del commutatore piattaforma filtro virtuale Azure (VFP) non può essere utilizzato in combinazione con altre estensioni commutatore 3rd party.  
  
## <a name="switching-and-routing-in-hyper-v-network-virtualization"></a>Il passaggio e il Routing di virtualizzazione rete Hyper-V  
HNVv2 implementa la semantica di routing Layer 3 (L3) per funzionano come uno switch fisico e cambio di livello 2 (L2) corretto o router funzionerebbe. Quando una macchina virtuale connessa a una rete virtuale di virtualizzazione RETE tenta di stabilire una connessione con un'altra macchina virtuale nella stessa subnet virtuale (VSID) è necessario innanzitutto individuare l'indirizzo MAC di autorità di certificazione della macchina virtuale remota. Se è presente una voce ARP per l'indirizzo IP della macchina virtuale di destinazione nella tabella ARP della macchina virtuale di origine, viene utilizzato l'indirizzo MAC da questa voce. Se una voce non esiste, la macchina virtuale invierà che un ARP broadcast con una richiesta per l'indirizzo MAC corrispondente all'indirizzo IP della macchina virtuale di destinazione deve essere restituito. Il commutatore Hyper-V intercetterà la richiesta e inviarlo per l'agente Host. L'agente Host avrà un aspetto nel relativo database locale per un indirizzo MAC corrispondente per l'indirizzo IP della macchina virtuale di destinazione richiesto.  
  
> [!NOTE]  
> L'agente Host funge da server OVSDB, Usa una variante dello schema VTEP per archiviare i mapping di CA-PA, tabella MAC e così via.  
  
Se un indirizzo MAC è disponibile, l'agente Host inserisce una risposta ARP e invia nuovamente alla macchina virtuale. Dopo che tutte le informazioni necessarie dell'intestazione L2 stack di rete della macchina virtuale, il frame viene inviato alla corrispondente porta Hyper-V nel commutatore V. Internamente, il commutatore Hyper-V verifica frame rispetto alle regole corrispondenti N-pLo assegnato alla porta V e applica determinate trasformazioni al frame in base a queste regole. Soprattutto, un set di trasformazioni di incapsulamento viene applicato per creare l'intestazione di incapsulamento tramite NVGRE o VXLAN, a seconda dei criteri definiti a Controller di rete. In base ai criteri programmato dall'agente Host, un mapping di CA-PA viene utilizzato per determinare l'indirizzo IP dell'host Hyper-V in cui risiede la macchina virtuale di destinazione. Il commutatore Hyper-V assicura le regole di routing corrette e tag VLAN vengono applicate al pacchetto esterno in modo da raggiungere l'indirizzo PA remoto.  
  
Se una macchina virtuale connessa a una rete virtuale di virtualizzazione RETE desidera creare una connessione con una macchina virtuale in un'altra subnet virtuale (VSID), il pacchetto deve essere indirizzato di conseguenza. Virtualizzazione RETE prevede una topologia di stella in cui è presente un solo indirizzo IP nello spazio CA utilizzato come hop successivo per raggiungere tutti i prefissi IP (significato una route/gateway predefinito). Attualmente, questo modo viene applicata una limitazione per una singola route predefinita e le route predefinite non sono supportate.  
  
### <a name="routing-between-virtual-subnets"></a>Routing tra subnet virtuali  
In una rete fisica, una subnet IP è un dominio di livello 2 (L2) in cui i computer (virtuali e fisici) possono comunicare direttamente tra loro. Il dominio L2 è un dominio di broadcast in cui le voci ARP (mappa indirizzo IP:MAC) vengono acquisite tramite richieste ARP che vengono trasmessi su tutte le interfacce e vengono inviate le risposte ARP all'host richiedente. Il computer utilizza le informazioni di MAC apprese dalla risposta ARP per costruire completamente il frame L2, incluse le intestazioni Ethernet. Tuttavia, se un indirizzo IP in un'altra subnet L3, la richiesta ARP non supera questo limite L3. Al contrario, un'interfaccia di router L3 (hop successivo o il gateway predefinito) con un indirizzo IP della subnet di origine deve rispondere a queste richieste ARP con il proprio indirizzo MAC.  
  
Reti Windows standard, un amministratore può creare route statiche e assegnarli a un'interfaccia di rete. Inoltre, un "gateway predefinito" è in genere configurato per essere l'indirizzo IP dell'hop successivo in un'interfaccia in cui vengono inviati i pacchetti destinati per la route predefinita (0.0.0.0/0). I pacchetti vengono inviati a questo gateway predefinito se non esistono alcuna route specifiche. Questo in genere è il router della rete fisica.  Virtualizzazione RETE utilizza un router incorporato che fa parte di ogni host e dispone di un'interfaccia in ogni VSID per creare un router distribuito per le reti virtuali.  
  
Poiché virtualizzazione RETE prevede una topologia a stella, il router di virtualizzazione RETE distribuito funge da gateway predefinito solo per tutto il traffico tra subnet virtuali che fanno parte della stessa rete VSID. L'indirizzo utilizzato come il valore predefinito del gateway predefinito è l'indirizzo IP più basso nell'ID di subnet VIRTUALE e viene assegnato al router di virtualizzazione RETE distribuita. Il router distribuito consente un modo molto efficiente per tutto il traffico all'interno di una rete VSID da indirizzare in modo appropriato, poiché ogni host può instradare direttamente il traffico all'host appropriato senza intermediari.  Questo è valido soprattutto quando due macchine virtuali della stessa rete VM ma di subnet virtuali diverse si trovano nello stesso host fisico.  Come è possibile osservare più avanti in questa sezione, il pacchetto non deve mai uscire dall'host fisico.  
  
### <a name="routing-between-pa-subnets"></a>Routing tra subnet PA  
A differenza di HNVv1 che allocato un indirizzo IP PA per ogni Subnet virtuale (VSID), HNVv2 ora utilizza un indirizzo IP PA per ogni membro del team NIC Teaming Switch-Embedded (SET). L'implementazione predefinita presuppone che un team di due schede di RETE e vengono assegnati due indirizzi IP PA per ogni host. Un singolo host dispone di indirizzi IP PA assegnati dalla stessa subnet logica Provider (PA) nella stessa VLAN. Due macchine virtuali tenant nella stessa subnet virtuale effettivamente potrebbe trovarsi in due host diversi che vengono connessi a due subnet logica provider diverso. Virtualizzazione RETE creerà le intestazioni IP esterne per il pacchetto incapsulato in base al mapping CA-PA. Tuttavia, si basa sullo stack TCP/IP host al protocollo ARP per il gateway PA predefinito e quindi vengono compilate le intestazioni Ethernet esterne in base alla risposta ARP. In genere, questa risposta ARP proviene dall'interfaccia SVI sul commutatore fisico o L3 router in cui l'host è connesso. Virtualizzazione RETE pertanto si basa sul router per il routing dei pacchetti incapsulati tra subnet logica provider L3 / VLAN.  
  
### <a name="routing-outside-a-virtual-network"></a>Routing all'esterno di una rete virtuale  
La maggior parte delle distribuzioni dei clienti richiede la comunicazione tra l'ambiente di Virtualizzazione rete Hyper-V e risorse che non fanno parte di tale ambiente. Per consentire la comunicazione tra questi due ambienti sono dunque necessari gateway di virtualizzazione rete. Infrastrutture che richiedono un Gateway di virtualizzazione RETE includono Cloud privato e Cloud ibrido. In pratica, i gateway di virtualizzazione RETE sono necessari per il livello 3 routing tra reti (fisiche) interne ed esterne (tra cui NAT) o tra siti diversi e/o cloud (privato o pubblico) che utilizzano un tunnel VPN IPSec o GRE.  
  
I gateway sono disponibili in diversi fattori di forma fisici. Possono essere basati su Windows Server 2016, incorporato in un commutatore Top of Rack (TOR) che agisce come un Gateway VXLAN, si accede tramite un IP virtuale (VIP) annunciata dal servizio di bilanciamento del carico, inseriti in altri dispositivi di rete esistente, oppure può essere un nuovo dispositivo di rete autonomi.  
  
Per ulteriori informazioni sulle opzioni di Gateway RAS di Windows, vedere [Gateway RAS](../../../../remote/remote-access/ras-gateway/RAS-Gateway.md).  
  
## <a name="packet-encapsulation"></a>Incapsulamento dei pacchetti  
Le singole schede di rete virtuali di Virtualizzazione rete Hyper-V sono associate a due indirizzi IP:  
  
-   **Indirizzo del cliente** (CA) l'indirizzo IP assegnato dal cliente, in base alla propria infrastruttura intranet. Questo indirizzo consente agli utenti di scambiare il traffico di rete con la macchina virtuale come se non è stato spostato in un cloud pubblico o privato. L'indirizzo CA è visibile alla macchina virtuale e raggiungibile dal cliente.  
  
-   **Indirizzo del provider** (PA) l'indirizzo IP assegnato dal provider di hosting o gli amministratori del Data Center in base alla loro infrastruttura di rete fisica. L'indirizzo PA viene visualizzato nei pacchetti di rete scambiati con il server Hyper-V che ospita la macchina virtuale. L'indirizzo PA è visibile sulla rete fisica, ma non sulla macchina virtuale.  
  
Gli indirizzi CA mantengono la topologia di rete del cliente, virtualizzata e disaccoppiata dagli effettivi indirizzi e dalla topologia di rete fisica sottostante, come implementato dagli indirizzi PA. Nel diagramma seguente viene mostrata la relazione concettuale esistente tra gli indirizzi CA della macchina virtuale e gli indirizzi PA dell'infrastruttura di rete in conseguenza della virtualizzazione di rete.  
  
![diagramma concettuale della virtualizzazione di rete su infrastruttura fisica](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF2.gif)  
  
Figura 6: diagramma concettuale della virtualizzazione di rete su infrastruttura fisica  
  
Nel diagramma, macchine virtuali dei clienti l'invio di pacchetti di dati nell'area di autorità di certificazione, che attraversano l'infrastruttura di rete fisica tramite le proprie reti virtuali, o "tunnel". Nell'esempio precedente, i tunnel possono essere considerati come "buste" che avvolgono i pacchetti di dati Contoso e Fabrikam con etichette di spedizione verdi (gli indirizzi PA) per essere recapitati all'host di destinazione a destra dall'host di origine a sinistra. La chiave è come gli host stabiliscono gli "indirizzi di spedizione" (PA) corrispondenti di Contoso e Fabrikam della CA, come la "busta" avvolge i pacchetti e come gli host di destinazione possono scartare i pacchetti e distribuire correttamente alle macchine virtuali di destinazione di Contoso e Fabrikam.  
  
Questa semplice analogia sottolinea gli aspetti essenziali della virtualizzazione di rete:  
  
-   L'indirizzo CA di ciascuna macchina virtuale è mappato all'indirizzo PA di un host fisico. Più indirizzi CA possono essere associati allo stesso indirizzo PA.  
  
-   Le macchine virtuali inviare pacchetti di dati in spazi degli indirizzi CA, che vengono inseriti in una "busta" con una coppia di origine e destinazione PA in base al mapping.  
  
-   I mapping degli indirizzi CA-PA devono consentire agli host di distinguere i pacchetti destinati a diversi macchine virtuali dei clienti.  
  
Di conseguenza, il meccanismo per virtualizzare la rete consiste nella virtualizzazione degli indirizzi di rete utilizzati dalle macchine virtuali. Il controller di rete è responsabile per il mapping degli indirizzi e l'agente host mantiene il database di mapping utilizzando lo schema MS_VTEP. Nella sezione successiva vengono descritti i meccanismi effettivi di virtualizzazione degli indirizzi.  
  
## <a name="network-virtualization-through-address-virtualization"></a>Virtualizzazione di rete attraverso la virtualizzazione degli indirizzi  
Virtualizzazione RETE implementa sovrapporre reti tenant tramite Network Virtualization Generic Routing Encapsulation (NVGRE) o Virtual eXtensible Local Area Network (VXLAN).  VXLAN è il valore predefinito.  
  
### <a name="virtual-extensible-local-area-network-vxlan"></a>Virtual eXtensible Local Area Network (VXLAN)  
Virtual eXtensible Local Area Network (VXLAN) ([7348 RFC](https://www.rfc-editor.org/info/rfc7348)) protocollo è stato ampiamente adottato nel mercato, grazie al supporto di fornitori come Cisco, Brocade, Arista, Dell, HP e altri. Il protocollo VXLAN utilizza il protocollo UDP come trasporto. La porta di destinazione UDP assegnato da IANA per VXLAN è 4789 e la porta UDP di origine deve essere un hash di informazioni del pacchetto da utilizzare per la distribuzione ECMP interna. Dopo l'intestazione UDP, un'intestazione VXLAN viene aggiunto il pacchetto che include un campo a 4 byte riservato seguito da un campo di 3 byte per le VXLAN rete identificatore (VNI) - VSID - seguito da un altro campo a 1 byte riservato. Dopo l'intestazione VXLAN, viene aggiunto il frame L2 CA originale (senza frame Ethernet CA FCS).  
  
![Intestazione del pacchetto VXLAN](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VXLAN-packet-header.png)  
  
### <a name="generic-routing-encapsulation-nvgre"></a>Generic Routing Encapsulation (NVGRE)  
Questo meccanismo di virtualizzazione di rete utilizza Generic Routing Encapsulation (NVGRE) come parte dell'intestazione del tunnel. In NVGRE, pacchetto della macchina virtuale viene incapsulato all'interno di un altro pacchetto. L'intestazione del nuovo pacchetto così ottenuto contiene gli indirizzi IP PA di origine e di destinazione appropriati, oltre all'ID subnet virtuale memorizzato nel campo chiave dell'intestazione GRE, come illustrato nella Figura 7.  
  
![Incapsulamento NVGRE](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF3.gif)  
  
Figura 7: virtualizzazione di rete - Incapsulamento NVGRE  
  
L'ID Subnet virtuale consente di identificare la macchina virtuale del cliente per qualsiasi pacchetto, anche se l'indirizzo PA e CA's sui pacchetti si sovrappongano. In questo modo, tutte le macchine virtuali presenti nello stesso host possono condividere un unico indirizzo PA, come illustrato nella Figura 7.  
  
La condivisione dell'indirizzo PA influisce decisamente sulla scalabilità di rete, perché consente di ridurre drasticamente il numero di indirizzi IP e MAC che l'infrastruttura di rete deve apprendere. Ad esempio, se gli host presenti a ciascuna estremità della rete hanno in media 30 macchine virtuali, il numero degli indirizzi IP e MAC che l'infrastruttura di rete deve apprendere si riduce di un fattore 30. Anche gli ID subnet virtuale incorporati semplificano l'associazione dei pacchetti ai clienti effettivi.  
  
La condivisione dello schema per Windows Server 2012 R2 è un indirizzo PA per VSID per ogni host. Per Windows Server 2016 lo schema è un indirizzo PA per ogni membro del team NIC.  
  
Con Windows Server 2016 e versioni successive, virtualizzazione RETE completamente supporto NVGRE e VXLAN di; non richiede l'aggiornamento o acquistare nuovo hardware di rete, ad esempio schede di rete, commutatori o router. In questo modo i pacchetti in transito sono normale pacchetto IP nello spazio degli indirizzi PA, compatibile con l'infrastruttura di rete odierni.  Tuttavia, per ottenere le migliori prestazioni, utilizzare supportato NIC con i driver più recenti che supportano l'offload delle attività.  
  
## <a name="multi-tenant-deployment-example"></a>esempio di distribuzione multi-tenant  
Nel diagramma seguente viene illustrato un esempio di distribuzione di due clienti che si trova in un Data Center nel cloud con la relazione CA-PA definita dai criteri di rete.  
  
![esempio di distribuzione multi-tenant](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF5.png)  
  
Figura 8: esempio di distribuzione multi-tenant  
  
Osservare l'esempio della Figura 8. Prima dello spostamento nel servizio IaaS condiviso del provider di servizi di hosting:  
  
-   La società Contoso ha eseguito un'istanza di SQL Server (denominata **SQL**) all'indirizzo IP 10.1.1.11 e un'istanza di un server Web (denominata **Web**) all'indirizzo IP 10.1.1.12. Per le transazioni di database, questo server Web utilizza la propria istanza di SQL Server.  
  
-   La società Fabrikam ha eseguito un'istanza di SQL Server, denominata anch'essa **SQL** , assegnandole l'indirizzo IP 10.1.1.11 e un'istanza di un server Web, denominata anch'essa **Web** , assegnandole ugualmente l'indirizzo IP 10.1.1.12. Per le transazioni di database questo server Web utilizza la propria istanza di SQL Server.  
  
Si presuppone che il provider di servizi di hosting ha creato in precedenza la rete logica provider (PA) tramite il Controller di rete in modo che corrispondano alla loro topologia di rete fisica. Il Controller di rete alloca due indirizzi IP PA dal prefisso IP della subnet logica in cui gli host connessi. Il controller di rete indica inoltre il tag VLAN appropriato per applicare gli indirizzi IP.  
  
Con la rete Controller, Contoso Corp e Fabrikam quindi creare subnet che sono supportate dalla rete logica provider (PA) specificata dal provider di servizi e della rete virtuale. Le società Contoso e Fabrikam spostano le rispettive istanze di SQL Server e server Web nel servizio IaaS condiviso dello stesso provider di servizi di hosting, dove, in modo del tutto casuale, eseguono le macchine virtuali **SQL** su Hyper-V Host 1 e le macchine virtuali **Web** (IIS7) su Hyper-V Host 2. Tutte le macchine virtuali mantengono gli indirizzi IP Intranet originali (i propri indirizzi CA).  
  
Entrambe le società vengono assegnate il seguente ID Subnet virtuale (VSID) dal Controller di rete come indicato di seguito.  L'agente in ogni host Hyper-V Host riceve gli indirizzi IP PA allocati dal Controller di rete e crea due PA host vNICs in un raggruppamento di rete non predefinita. Un'interfaccia di rete viene assegnata a ciascuno di questi vNICs host in cui l'indirizzo IP PA viene assegnato come illustrato di seguito:  
  
-   Macchine virtuali della società Contoso VSID e PAs: **VSID** is 5001, **SQL PA** is 192.168.1.10, **Web PA** is 192.168.2.20  
  
-   Macchine virtuali della società Fabrikam VSID e PAs: **VSID** is 6001, **SQL PA** is 192.168.1.10, **Web PA** is 192.168.2.20  
  
Il Controller di rete esegue il plumbing tutti i criteri di rete (incluso il mapping di CA-PA) per l'agente Host SDN in grado di mantenere i criteri in un archivio persistente (nelle tabelle di database OVSDB).  
  
Quando la macchina virtuale Web di Contoso (10.1.1.12) su Hyper-V Host 2 crea una connessione TCP a SQL Server all'indirizzo 10.1.1.11, si verifica quanto segue:  
  
-   Macchina Virtuale non richiesti per l'indirizzo MAC di destinazione di 10.1.1.11  
  
-   L'estensione VFP di vSwitch intercetta il pacchetto e lo invia all'agente Host SDN  
  
-   L'agente Host SDN Cerca nell'archivio criteri per l'indirizzo MAC per 10.1.1.11  
  
-   Se viene trovato un MAC, l'agente Host inserisce una risposta ARP la macchina Virtuale  
  
-   Se non viene trovato un MAC, viene inviata alcuna risposta e la voce ARP nella macchina Virtuale per 10.1.1.11 viene contrassegnata come non raggiungibile.  
  
-   La macchina Virtuale ora costruisce un pacchetto TCP con le intestazioni di CA Ethernet e indirizzo IP corrette e lo invia il vSwitch  
  
-   VFP in vSwitch l'estensione di inoltro elabora il pacchetto attraverso i livelli VFP (descritta di seguito) assegnato alla porta vSwitch origine in cui il pacchetto è stato ricevuto e crea una nuova voce di flusso della tabella di flusso unificata VFP  
  
-   Il motore VFP esegue ricerca di regole di corrispondenza o una tabella di flusso per ogni livello (ad esempio, un livello di rete virtuale) in base alle intestazioni IP ed Ethernet.  
  
-   La regola corrispondente nel livello di rete virtuale fa riferimento a uno spazio di mapping CA-PA ed esegue l'incapsulamento.  
  
-   Il tipo di incapsulamento (VXLAN o NVGRE) è specificato nel livello di rete virtuale con il VSID.  
  
-   Nel caso di incapsulamento VXLAN, un'intestazione UDP esterna viene costruita con il VSID della 5001 nell'intestazione VXLAN.  
    Un'intestazione IP esterna viene costruita con l'indirizzo PA di origine e destinazione assegnato al Hyper-V Host 2 (192.168.2.20) e Hyper-V Host 1 (192.168.1.10) rispettivamente in base alle archivio criteri dell'agente Host SDN.  
  
-   Questo pacchetto viene quindi trasmesso al livello di routing PA in VFP.  
  
-   Il livello di routing PA in VFP farà riferimento il raggruppamento di rete utilizzato per il traffico spazio PA e un ID VLAN e utilizzare lo stack TCP/IP dell'host per inoltrare il pacchetto PA Hyper-V Host 1 correttamente.  
  
-   Al momento della ricezione del pacchetto incapsulato, Hyper-V Host 1 riceve il pacchetto nel raggruppamento rete PA e trasmette il vSwitch.  
  
-   VFP elabora il pacchetto tramite i livelli VFP e creare una nuova voce di flusso della tabella di flusso unificata VFP.  
  
-   Il motore VFP corrisponde alle regole ingres nel livello di rete virtuale e rimuove intestazioni Ethernet, IP e VXLAN del pacchetto incapsulato esterno.  
  
-   Il motore VFP inoltra quindi il pacchetto alla porta vSwitch a cui è connessa la macchina Virtuale di destinazione.  
  
Un processo simile per il traffico tra la società Fabrikam **Web** e **SQL** macchine virtuali vengono utilizzate le impostazioni di criteri di virtualizzazione RETE per la società Fabrikam Pertanto, con Virtualizzazione rete Hyper-V le macchine virtuali di Fabrikam e Contoso interagiscono come se si trovassero nelle rispettive Intranet originali. Non possono mai interagire tra di loro, anche se utilizzano gli stessi indirizzi IP.  
  
Gli indirizzi separati (CA e PA), le impostazioni dei criteri degli host Hyper-V e la conversione degli indirizzi tra la CA e PA per il traffico di macchina virtuale in ingresso e in uscita isolano questi insiemi di server con la chiave NVGRE o il VNID VLXAN. Inoltre, trasformazione e mapping di virtualizzazione disaccoppiano l'architettura di rete virtuale dall'infrastruttura di rete fisica. Sebbene le macchine virtuali **SQL** e **Web** di Contoso e **SQL** e **Web** di Fabrikam risiedano nelle proprie subnet IP CA (10.1.1/24), sono distribuite fisicamente su due host in diverse subnet PA, rispettivamente 192.168.1/24 e 192.168.2/24. Ne consegue che con Virtualizzazione rete Hyper-V il provisioning e la migrazione in tempo reale delle macchine virtuali tra subnet diventano possibili.  
  
## <a name="hyper-v-network-virtualization-architecture"></a>Architettura di Virtualizzazione rete Hyper-V  
In Windows Server 2016, HNVv2 viene implementato utilizzando Azure virtuale filtro piattaforma (VFP) che è un'estensione di filtro NDIS all'interno del commutatore Hyper-V. Il concetto chiave di VFP è che un'azione di corrispondenza del motore del flusso con un'API interno esposto all'agente SDN Host per la programmazione di criteri di rete. L'agente Host SDN stesso riceve i criteri di rete dal Controller di rete attraverso i canali di comunicazione WCF SouthBound e OVSDB. Non solo è criteri di rete virtuale (ad esempio, il mapping di CA-PA) programmato mediante VFP ma criteri aggiuntive, ad esempio ACL, QoS e così via.  
  
Gerarchia di oggetti per il vSwitch e VFP estensione di inoltro è il seguente:  
  
-   vSwitch  
  
    -   Gestione di RETE esterna  
  
    -   Scheda di RETE Hardware offload  
  
    -   Regole di inoltro globale  
  
    -   Port  
  
        -   Inoltro di livello per selettore di precisione di blocco in uscita  
  
        -   Elenchi di spazio per i mapping e i pool di NAT  
  
        -   Tabella di flusso unificato  
  
        -   Livello VFP  
  
            -   Tabella di flusso  
  
            -   Raggruppa  
  
            -   Regola  
  
                -   Le regole possono fare riferimento a spazi  
  
In VFP, un livello viene creato per ogni tipo di criteri (ad esempio, una rete virtuale) ed è un insieme generico di tabelle/flusso della regola. Non presenta alcuna funzionalità intrinseche fino a quando le regole specifiche vengono assegnate a tale livello per implementare tali funzionalità. Ogni livello viene assegnata una priorità e i livelli vengono assegnati a una porta dall'ordine crescente di priorità. Le regole sono organizzate in gruppi basati principalmente su direzione e famiglia di indirizzi IP. I gruppi sono inoltre assegnati una priorità e al massimo una regola da un gruppo può corrispondere a un determinato flusso.  
  
La logica di inoltro per vSwitch con estensione VFP è come segue:  
  
-   Elaborazione in ingresso (dal punto di vista del pacchetto in una porta in ingresso)  
  
-   Inoltro  
  
-   Elaborazione in uscita (in uscita dal punto di vista del pacchetto lasciando una porta)  
  
VFP supporta interna inoltro MAC per tipi di incapsulamento NVGRE e VXLAN nonché inoltro VLAN MAC basato su esterno.  
  
L'estensione VFP ha un percorso lento e fast-path per l'attraversamento del pacchetto. Il primo pacchetto in un flusso deve attraversare tutti i gruppi di regole in ogni livello ed eseguire una regola di ricerca che è un'operazione dispendiosa. Tuttavia, dopo aver registrato un flusso della tabella di flusso unificata con un elenco di azioni (in base alle regole di corrispondenza) tutti i pacchetti successivi verranno elaborati in base alle voci della tabella di flusso unificato.  
  
Criteri di virtualizzazione RETE sono programmato per l'agente host. Ogni scheda di rete della macchina virtuale è configurato con un indirizzo IPv4. Si tratta degli indirizzi CA che verranno utilizzati dalle macchine virtuali per comunicare tra di loro e sono trasportati nei pacchetti IP provenienti dalle macchine virtuali. Virtualizzazione RETE incapsula il frame di autorità di certificazione in un frame PA in base ai criteri di virtualizzazione di rete archiviati nel database dell'agente host.  
  
![Architettura di Virtualizzazione rete Hyper-V](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF7.png)  
  
Figura 9: Architettura di Virtualizzazione rete Hyper-V  
  
## <a name="summary"></a>Riepilogo  
I centri dati basati su cloud possono offrire diversi vantaggi, come una migliore scalabilità e un utilizzo ottimale delle risorse. Lo sfruttamento di questi potenziali vantaggi richiede una tecnologia in grado soprattutto di affrontare i problemi posti dalla scalabilità multi-tenant in un ambiente dinamico. La funzionalità Virtualizzazione rete Hyper-V è stata progettata per ovviare a questi problemi e per migliorare inoltre l'efficienza operativa del centro dati disaccoppiando la topologia della rete virtuale da quella della rete fisica. Creazione di uno standard esistente, virtualizzazione RETE viene eseguito nei centri dati odierni e funziona con l'infrastruttura VXLAN esistente. I clienti con virtualizzazione RETE possono ora consolidare i propri centri dati in un cloud privato oppure estendere con facilità i propri centri dati ambiente dei provider di server di hosting con cloud ibrido.  
  
## <a name="BKMK_LINKS"></a>Vedere anche  
Per ulteriori informazioni su HNVv2 vedere i collegamenti seguenti:  
  
|Tipo di contenuto|Riferimenti|  
|----------------|--------------|  
|**Risorse della community**|-   [Blog sull'architettura di Cloud privati](https://blogs.technet.com/b/privatecloud)<br />-Domande: [cloudnetfb@microsoft.com](mailto:%20cloudnetfb@microsoft.com)|  
|**RFC**|-   [NVGRE bozza di RFC su](https://www.ietf.org/id/draft-sridharan-virtualization-nvgre-07.txt)<br />-   [VXLAN - RFC 7348](https://www.rfc-editor.org/info/rfc7348)|  
|**Tecnologie correlate**|-Per dettagli tecnici di virtualizzazione rete Hyper-V in Windows Server 2012 R2, vedere [Dettagli tecnici sulla virtualizzazione rete Hyper-V](https://technet.microsoft.com/library/jj134174.aspx)<br />-   [Controller di rete](../../../sdn/technologies/network-controller/Network-Controller.md)|  
  


