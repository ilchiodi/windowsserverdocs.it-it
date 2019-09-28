---
title: Disponibilità elevata di Gateway RAS
description: È possibile usare questo argomento per informazioni sulle configurazioni a disponibilità elevata per RAS multi-tenant gateway per SDN (Software Defined Networking) in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 34d826c9-65bc-401f-889d-cf84e12f0144
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7d9c37629c0e0d9964554ba90887aa45f74a330a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355612"
---
# <a name="ras-gateway-high-availability"></a>Disponibilità elevata di Gateway RAS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni sulle configurazioni a disponibilità elevata per RAS multi-tenant gateway per SDN (Software Defined Networking).  
  
In questo argomento sono incluse le sezioni seguenti.  
  
-   [Panoramica del gateway RAS](#bkmk_overview)  
  
-   [Panoramica sui pool di gateway](#bkmk_pools)  
  
-   [Panoramica della distribuzione del gateway RAS](#bkmk_deployment)  
  
-   [Integrazione del gateway RAS con il controller di rete](#bkmk_integration)  
  
## <a name="bkmk_overview"></a>Panoramica del gateway RAS  
Se l'organizzazione è un provider di servizi cloud (CSP) o un'azienda con più tenant, è possibile distribuire il gateway RAS in modalità multi-tenant per consentire il routing del traffico di rete da e verso reti virtuali e fisiche, incluso Internet.  
  
È possibile distribuire il gateway RAS in modalità multi-tenant come gateway perimetrale per indirizzare il traffico di rete dei clienti tenant alle reti e alle risorse virtuali dei tenant.  
  
Quando si distribuiscono più istanze di VM gateway RAS che garantiscono disponibilità elevata e failover, si distribuisce un pool di gateway. In Windows Server 2012 R2 tutte le VM del gateway costituivano un singolo pool, che rendeva difficile la separazione logica della distribuzione del gateway.  Il gateway di Windows Server 2012 R2 ha offerto una distribuzione di ridondanza 1:1 per le macchine virtuali del gateway, causando un utilizzo inferiore della capacità disponibile per le connessioni VPN da sito a sito (S2S).  
  
Questo problema viene risolto in Windows Server 2016, che offre più pool di gateway, che possono essere di tipi diversi per la separazione logica. La nuova modalità di ridondanza M + N consente una configurazione di failover più efficiente.  
  
Per altre informazioni generali sul gateway RAS, vedere [gateway RAS](../../../../remote/remote-access/ras-gateway/RAS-Gateway.md).  
  
## <a name="bkmk_pools"></a>Panoramica sui pool di gateway  
In Windows Server 2016 è possibile distribuire gateway in uno o più pool.  
  
Nella figura seguente vengono illustrati diversi tipi di pool di gateway che forniscono il routing del traffico tra reti virtuali.  
  
![Pool di gateway RAS](../../../media/RAS-Gateway-High-Availability/ras_pools.png)  
  
Ogni pool dispone delle proprietà seguenti.  
  
-   Ogni pool è con ridondanza M + N. Ciò significa che viene eseguito il backup di un numero di VM gateway attive da un numerò n'di macchine virtuali del gateway di standby. Il valore di N (gateway di standby) è sempre minore o uguale a M (gateway attivi).  
  
-   Un pool può eseguire le singole funzioni del gateway, Internet Key Exchange versione 2 (IKEv2) S2S, livello 3 (L3) e Generic Routing Encapsulation (GRE), oppure il pool può eseguire tutte queste funzioni.  
  
-   È possibile assegnare un singolo indirizzo IP pubblico a tutti i pool o a un subset di pool. In questo modo si riduce notevolmente il numero di indirizzi IP pubblici che è necessario usare, perché è possibile che tutti i tenant si connettano al cloud in un unico indirizzo IP. La sezione seguente in merito a disponibilità elevata e bilanciamento del carico ne descrive il funzionamento.  
  
-   È possibile scalare facilmente un pool di gateway verso l'alto o verso il basso aggiungendo o rimuovendo le macchine virtuali gateway nel pool. Rimozione o aggiunta di gateway non interrotti i servizi forniti da un pool. È inoltre possibile aggiungere e rimuovere l'intero pool di gateway.  
  
-   Le connessioni di un singolo tenant possono terminare in più pool e più gateway in un pool. Tuttavia, se un tenant dispone di connessioni che terminano in un pool di **tutti i** tipi di gateway, non è in grado di sottoscrivere altri pool di gateway **di tipo o** di tipo singolo.  
  
I pool di Gateway offrono inoltre la flessibilità necessaria per abilitare altri scenari:  
  
-   Pool a tenant singolo: è possibile creare un pool per l'uso da parte di un tenant.  
  
-   Se si vendono servizi cloud tramite i canali partner (rivenditore), è possibile creare set distinti di pool per ogni rivenditore.  
  
-   Più pool possono fornire la stessa funzione del gateway ma diverse capacità. Ad esempio, è possibile creare un pool di gateway che supporta le connessioni IKEv2 S2S con velocità effettiva elevata e velocità effettiva ridotta.  
  
## <a name="bkmk_deployment"></a>Panoramica della distribuzione del gateway RAS  
Nella figura seguente viene illustrata una tipica distribuzione del provider di servizi cloud (CSP) del gateway RAS.  
  
![Panoramica della distribuzione del gateway RAS](../../../media/RAS-Gateway-High-Availability/ras_csp_deploy.png)  
  
Con questo tipo di distribuzione, i pool di gateway vengono distribuiti dietro un software Load Balancer (SLB), che consente al CSP di assegnare un singolo indirizzo IP pubblico per l'intera distribuzione. Più connessioni gateway di un tenant possono terminare in più pool di gateway e anche in più gateway all'interno di un pool. Questa operazione viene illustrata tramite le connessioni IKEv2 S2S nel diagramma precedente, ma lo stesso vale anche per altre funzioni del gateway, ad esempio i gateway L3 e GRE.  
  
Nell'illustrazione, il dispositivo BGP MT è un gateway RAS multi-tenant con BGP. Il protocollo BGP multi-tenant viene usato per il routing dinamico. Il routing per un tenant è centralizzato: un singolo punto, denominato RR (route reflector), gestisce il peering BGP per tutti i siti tenant. L'RR stesso viene distribuito in tutti i gateway in un pool. In questo modo si ottiene una configurazione in cui le connessioni di un tenant (percorso dati) terminano su più gateway, ma l'RR per il tenant (percorso di controllo del peering BGP) si trova su uno solo dei gateway.  
  
Il router BGP è separato nel diagramma per illustrare questo concetto di routing centralizzato. L'implementazione BGP del gateway fornisce anche il routing di transito, che consente al cloud di fungere da punto di transito per il routing tra due siti tenant. Queste funzionalità BGP sono applicabili a tutte le funzioni del gateway.  
  
## <a name="bkmk_integration"></a>Integrazione del gateway RAS con il controller di rete  
Il gateway RAS è completamente integrato con il controller di rete in Windows Server 2016. Quando si distribuiscono il gateway RAS e il controller di rete, il controller di rete esegue le funzioni seguenti.  
  
-   Distribuzione dei pool di gateway  
  
-   Configurazione delle connessioni tenant in ogni gateway  
  
-   Il passaggio del traffico di rete a un gateway di standby in caso di errore del gateway  
  
Le sezioni seguenti forniscono informazioni dettagliate sul gateway RAS e sul controller di rete.  
  
-   [Provisioning e bilanciamento del carico delle connessioni gateway (IKEv2, L3 e GRE)](#bkmk_provisioning)  
  
-   [Disponibilità elevata per IKEv2 S2S](#bkmk_ike)  
  
-   [Disponibilità elevata per GRE](#bkmk_gre)  
  
-   [Disponibilità elevata per gateway di inoltri L3](#bkmk_l3)  
  
### <a name="bkmk_provisioning"></a>Provisioning e bilanciamento del carico delle connessioni gateway (IKEv2, L3 e GRE)  
Quando un tenant richiede una connessione gateway, la richiesta viene inviata al controller di rete. Il controller di rete è configurato con informazioni su tutti i pool di gateway, tra cui la capacità di ogni pool e ogni gateway in ogni pool. Il controller di rete seleziona il pool e il gateway corretti per la connessione. Questa selezione è basata sul requisito di larghezza di banda per la connessione. Il controller di rete usa un algoritmo di "adattamento migliore" per scegliere le connessioni in modo efficiente in un pool. Il punto di peering BGP per la connessione viene designato anche in questo momento se si tratta della prima connessione del tenant.  
  
Dopo che il controller di rete ha selezionato un gateway RAS per la connessione, il controller di rete effettua il provisioning della configurazione necessaria per la connessione sul gateway. Se la connessione è una connessione S2S di IKEv2, il controller di rete effettua anche il provisioning di una regola NAT (Network Address Translation) nel pool SLB. Questa regola NAT nel pool di SLB indirizza le richieste di connessione dal tenant al gateway designato. I tenant sono differenziati dall'indirizzo IP di origine, che dovrebbe essere univoco.  
  
> [!NOTE]  
> Le connessioni L3 e GRE ignorano il SLB e si connettono direttamente al gateway RAS designato.  Per queste connessioni è necessario che il router dell'endpoint remoto (o un altro dispositivo di terze parti) sia configurato correttamente per la connessione al gateway RAS.  
  
Se il routing BGP è abilitato per la connessione, il peering BGP viene avviato dal gateway RAS e le route vengono scambiate tra i gateway locali e quelli cloud. Le route apprese da BGP (o che sono route configurate staticamente se non si usa BGP) vengono inviate al controller di rete. Il controller di rete esegue quindi il plumbing delle route negli host Hyper-V su cui sono installate le VM tenant. A questo punto, il traffico tenant può essere instradato al sito locale corretto. Il controller di rete crea anche criteri di virtualizzazione rete Hyper-V associati che specificano i percorsi del gateway e li Plumbing negli host Hyper-V.  
  
### <a name="bkmk_ike"></a>Disponibilità elevata per IKEv2 S2S  
Un gateway RAS in un pool è costituito da connessioni e peering BGP di tenant diversi. Ogni pool include gateway di standby ' n'Active gateway è n'.  
  
Il controller di rete gestisce l'errore dei gateway nel modo seguente.  
  
-   Il controller di rete effettua costantemente il ping dei gateway in tutti i pool e può rilevare un gateway che ha avuto esito negativo o negativo. Il controller di rete può rilevare i seguenti tipi di errori del gateway RAS.  
  
    -   Errore VM gateway RAS  
  
    -   Errore dell'host Hyper-V su cui è in esecuzione il gateway RAS  
  
    -   Errore del servizio gateway RAS  
  
    Il controller di rete archivia la configurazione di tutti i gateway attivi distribuiti. La configurazione è costituita da impostazioni di connessione e impostazioni di routing.  
  
-   Quando un gateway ha esito negativo, influisca sulle connessioni dei tenant sul gateway, nonché sulle connessioni dei tenant che si trovano in altri gateway ma il cui RR risiede nel gateway non riuscito. Il tempo di inattività delle ultime connessioni è minore del primo. Quando il controller di rete rileva un gateway con errore, esegue le attività seguenti.  
  
    -   Rimuove le route delle connessioni interessate dagli host di calcolo.  
  
    -   Rimuove i criteri di virtualizzazione rete Hyper-V in questi host.  
  
    -   Seleziona un gateway di standby, lo converte in un gateway attivo e configura il gateway.  
  
    -   Modifica i mapping NAT nel pool SLB per puntare le connessioni al nuovo gateway.  
  
-   Simultaneamente, quando la configurazione viene visualizzata nel nuovo gateway attivo, vengono ristabilite le connessioni S2S IKEv2 e il peering BGP. Le connessioni e il peering BGP possono essere avviati dal gateway cloud o dal gateway locale. I gateway aggiornano le route e le inviano al controller di rete. Dopo che il controller di rete apprende le nuove route individuate dai gateway, il controller di rete invia le route e i criteri di virtualizzazione rete Hyper-V associati agli host Hyper-V in cui risiedono le macchine virtuali dei tenant con conseguenze sugli errori. Questa attività del controller di rete è simile alla circostanza di una nuova configurazione della connessione, ma si verifica solo su una scala più ampia.  
  
### <a name="bkmk_gre"></a>Disponibilità elevata per GRE  
Il processo di risposta al failover del gateway RAS da parte del controller di rete, incluso il rilevamento degli errori, la copia della configurazione di connessione e routing nel gateway di standby, il failover del routing BGP/statico delle connessioni interessate (inclusi il ritiro e il ripristino delle route sugli host di calcolo e il peering BGP e la riconfigurazione dei criteri di virtualizzazione rete Hyper-V negli host di calcolo è lo stesso per i gateway e le connessioni GRE. Il ristabilimento delle connessioni GRE viene eseguito in modo diverso, tuttavia, e la soluzione a disponibilità elevata per GRE prevede alcuni requisiti aggiuntivi.  
  
![Disponibilità elevata per GRE](../../../media/RAS-Gateway-High-Availability/ras_ha.png)  
  
Al momento della distribuzione del gateway, a ogni macchina virtuale del gateway RAS viene assegnato un indirizzo IP dinamico (DIP). Inoltre, a ogni macchina virtuale del gateway viene assegnato un indirizzo IP virtuale (VIP) per la disponibilità elevata GRE. Gli indirizzi VIP vengono assegnati solo ai gateway nei pool che possono accettare connessioni GRE e non a pool non GRE. Gli indirizzi VIP assegnati vengono annunciati ai commutatori Top of rack (TOR) usando BGP, che quindi annuncia ulteriormente gli indirizzi VIP nella rete fisica cloud. In questo modo i gateway sono raggiungibili dai router remoti o da dispositivi di terze parti in cui risiede l'altra estremità della connessione GRE. Il peering BGP è diverso dal peering BGP a livello di tenant per lo scambio di route dei tenant.  
  
Al momento del provisioning della connessione GRE, il controller di rete seleziona un gateway, configura un endpoint GRE sul gateway selezionato e restituisce l'indirizzo VIP del gateway assegnato. Questo indirizzo VIP viene quindi configurato come indirizzo del tunnel GRE di destinazione nel router remoto.  
  
Quando un gateway ha esito negativo, il controller di rete copia l'indirizzo VIP del gateway non riuscito e altri dati di configurazione nel gateway di standby. Quando il gateway di standby diventa attivo, annuncia il VIP al commutatore TOR e la rete fisica. I router remoti continuano a connettere i tunnel GRE allo stesso indirizzo VIP e l'infrastruttura di routing garantisce che i pacchetti vengano instradati al nuovo gateway attivo.  
  
### <a name="bkmk_l3"></a>Disponibilità elevata per gateway di inoltri L3  
Un gateway di inoltri L3 di virtualizzazione rete Hyper-V è un bridge tra l'infrastruttura fisica nel Data Center e l'infrastruttura virtualizzata nel cloud di virtualizzazione rete Hyper-V. In un gateway di inoltri L3 multi-tenant ogni tenant usa la propria rete logica con tag VLAN per la connettività con la rete fisica del tenant.  
  
Quando un nuovo tenant crea un nuovo gateway L3, il gateway del controller di rete Service Manager seleziona una macchina virtuale del gateway disponibile e configura una nuova interfaccia del tenant con un indirizzo IP di spazio dell'indirizzo (CA) a disponibilità elevata (dalla rete logica con tag VLAN del tenant) ). L'indirizzo IP viene usato come indirizzo IP del peer sul gateway remoto (rete fisica) ed è l'hop successivo per raggiungere la rete di virtualizzazione rete Hyper-V del tenant.  
  
Diversamente dalle connessioni di rete IPsec o GRE, il commutatore TOR non apprenderà dinamicamente la rete con tag VLAN del tenant. Il routing per la rete VLAN con tag del tenant deve essere configurato nel commutatore TOR e in tutti i commutatori intermedi e i router tra l'infrastruttura fisica e il gateway per garantire la connettività end-to-end.  Di seguito è riportato un esempio di configurazione di rete virtuale CSP, come illustrato nella figura seguente.  
  
|Rete|Subnet|ID VLAN|Gateway predefinito|  
|-----------|----------|-----------|-------------------|  
|Rete logica di Contoso L3|10.127.134.0/24|1001|10.127.134.1|  
|Rete logica di Woodgrove L3|10.127.134.0/24|1002|10.127.134.1|  
  
Di seguito sono elencate le configurazioni del gateway tenant di esempio, come illustrato nella figura seguente.  
  
|Nome del tenant|Indirizzo IP del gateway L3|ID VLAN|Indirizzo IP peer|  
|---------------|-------------------------|-----------|-------------------|  
|Contoso|10.127.134.50|1001|10.127.134.55|  
|Woodgrove|10.127.134.60|1002|10.127.134.65|  
  
Di seguito è illustrata questa configurazione in un data center CSP.  
  
![Disponibilità elevata per gateway di inoltri L3](../../../media/RAS-Gateway-High-Availability/ras_fwdgw.png)  
  
Gli errori del gateway, il rilevamento degli errori e il processo di failover del gateway nel contesto di un gateway di inoltri L3 sono simili ai processi per i gateway RAS IKEv2 e GRE. Le differenze si trovano nel modo in cui vengono gestiti gli indirizzi IP esterni.  
  
Quando lo stato della macchina virtuale del gateway diventa non integro, il controller di rete seleziona uno dei gateway di standby dal pool ed esegue nuovamente il provisioning delle connessioni di rete e del routing sul gateway di standby. Durante lo spostamento delle connessioni, l'indirizzo IP dello spazio della CA a disponibilità elevata del gateway di inoltri L3 viene spostato nella nuova VM del gateway insieme all'indirizzo IP BGP dello spazio CA del tenant.  
  
Poiché l'indirizzo IP del peering L3 viene spostato nella nuova VM del gateway durante il failover, l'infrastruttura fisica remota è nuovamente in grado di connettersi a questo indirizzo IP e, successivamente, raggiungere il carico di lavoro di virtualizzazione rete Hyper-V. Per il routing dinamico BGP, poiché l'indirizzo IP BGP dello spazio CA viene spostato nella nuova VM del gateway, il router BGP remoto può ristabilire il peering e apprendere nuovamente tutte le route di virtualizzazione rete Hyper-V.  
  
> [!NOTE]  
> È necessario configurare separatamente i commutatori TOR e tutti i router intermedi per poter usare la rete logica con tag VLAN per la comunicazione dei tenant. Inoltre, il failover L3 è limitato solo ai rack configurati in questo modo. Per questo motivo, il pool di gateway L3 deve essere configurato con attenzione e la configurazione manuale deve essere completata separatamente.  
  


