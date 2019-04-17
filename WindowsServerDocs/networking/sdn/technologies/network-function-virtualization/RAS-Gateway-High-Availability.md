---
title: Disponibilità elevata del Gateway RAS
description: È possibile utilizzare questo argomento per informazioni sulle configurazioni di disponibilità elevata per il Gateway multi-tenant RAS per reti SDN (Software Defined) in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 34d826c9-65bc-401f-889d-cf84e12f0144
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 48794ff8312ca00eda25f6d8bdca9929fc47084f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="ras-gateway-high-availability"></a>Disponibilità elevata del Gateway RAS

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per informazioni sulle configurazioni di disponibilità elevata per il Gateway multi-tenant RAS per reti SDN (Software Defined).  
  
In questo argomento contiene le sezioni seguenti.  
  
-   [Panoramica di Gateway RAS](#bkmk_overview)  
  
-   [Panoramica dei pool di gateway](#bkmk_pools)  
  
-   [Cenni preliminari sulla distribuzione di Gateway RAS](#bkmk_deployment)  
  
-   [Integrazione di Gateway RAS con Controller di rete](#bkmk_integration)  
  
## <a name="bkmk_overview"></a>Panoramica di Gateway RAS  
Se l'organizzazione è un Provider di servizi Cloud (CSP) o un'organizzazione con più tenant, è possibile distribuire Gateway RAS in modalità multi-tenant per fornire rete routing del traffico da e verso le reti virtuali e fisiche, Internet incluso.  
  
È possibile distribuire Gateway RAS in modalità multi-tenant, come un gateway per indirizzare il traffico di rete tenant cliente a risorse e le reti virtuali dei tenant.  
  
Quando si distribuiscono più istanze di macchine virtuali Gateway RAS che forniscono il failover e la disponibilità elevata, si distribuisce un pool di gateway. In Windows Server 2012 R2, tutti il gateway di macchine virtuali creato un singolo pool che difficile un po' una separazione logica della distribuzione del gateway.  Gateway di Windows Server 2012 R2 è disponibile una distribuzione di ridondanza 1:1 per il gateway di macchine virtuali, generando insufficiente utilizzo della capacità disponibile per site-to-site (S2S) le connessioni VPN.  
  
Questo problema si risolve in Windows Server 2016, che fornisce più pool di Gateway - che può essere di tipi diversi per la separazione logica. La nuova modalità di ridondanza N + M consente una configurazione di failover più efficiente.  
  
Per ulteriori informazioni su Gateway RAS, vedere [Gateway RAS](../../../../remote/remote-access/ras-gateway/RAS-Gateway.md).  
  
## <a name="bkmk_pools"></a>Panoramica dei pool di gateway  
In Windows Server 2016, è possibile distribuire gateway in uno o più pool.  
  
La figura seguente mostra i diversi tipi di pool di gateway che forniscono il routing del traffico tra reti virtuali.  
  
![Pool di Gateway RAS](../../../media/RAS-Gateway-High-Availability/ras_pools.png)  
  
Ogni pool ha le proprietà seguenti.  
  
-   Ogni pool è N + M ridondanti. Questo significa che un sto ' numero di macchine virtuali gateway attive viene eseguito il backup da un numero n di macchine virtuali gateway standby. Il valore di N (gateway standby) è sempre minore o uguale a M (gateway attivi).  
  
-   Un pool può eseguire una delle funzioni di gateway singoli - Internet Key Exchange versione 2 (IKEv2) S2S, livello 3 (L3) e Generic Routing Encapsulation (GRE) - o il pool può eseguire tutte queste funzioni.  
  
-   È possibile assegnare un singolo indirizzo IP pubblico per tutti i pool o a un sottoinsieme dei pool. In questo modo notevolmente riduce il numero di indirizzi IP pubblici che è necessario utilizzare, perché è possibile che tutti i titolari di connettersi al cloud in un singolo indirizzo IP. La sezione sulla disponibilità elevata e bilanciamento del carico di seguito viene descritto il funzionamento.  
  
-   È possibile scalare facilmente un pool di gateway verso l'alto o verso il basso aggiungendo o rimuovendo le macchine virtuali gateway nel pool. Rimozione o aggiunta di gateway non interrotti i servizi forniti da un pool. Puoi anche aggiungere e rimuovere l'intero pool di gateway.  
  
-   Le connessioni di un singolo tenant possono terminare in più pool e più gateway in un pool. Tuttavia, se un tenant con connessioni che termina con un **tutti** digitare pool di gateway, non può sottoscrivere altri **tutti** tipo o il pool di gateway singolo tipo.  
  
Pool di gateway offrono la flessibilità necessaria per abilitare scenari aggiuntivi:  
  
-   Pool single-tenant, è possibile creare un pool per l'utilizzo di un tenant.  
  
-   Se vendi i servizi cloud tramite canali partner (rivenditore), è possibile creare un set distinto di pool per ogni rivenditore.  
  
-   Più pool possono fornire la stessa funzione gateway ma le diverse capacità. Ad esempio, è possibile creare un pool di gateway che supporta connessioni a bassa velocità effettiva S2S IKEv2 e velocità effettiva elevata.  
  
## <a name="bkmk_deployment"></a>Cenni preliminari sulla distribuzione di Gateway RAS  
Nella figura seguente illustra una distribuzione tipica di Provider di servizi Cloud (CSP) di Gateway RAS.  
  
![Cenni preliminari sulla distribuzione di Gateway RAS](../../../media/RAS-Gateway-High-Availability/ras_csp_deploy.png)  
  
Con questo tipo di distribuzione, i pool di gateway vengono distribuiti dietro un Software carico bilanciamento (SLB), che consente il CSP assegnare un singolo indirizzo IP pubblico per l'intera distribuzione. Connessioni gateway più di un tenant possono terminare in più pool di gateway - e anche in più gateway all'interno di un pool. Questa operazione è descritta tramite connessioni IKEv2 S2S nel diagramma precedente, ma lo stesso è applicabile ad altre funzioni di gateway, ad esempio gateway L3 e GRE.  
  
Nell'illustrazione, il dispositivo MT BGP è un Gateway multi-tenant RAS con il protocollo BGP. Multi-tenant BGP viene utilizzato per il routing dinamico. Il routing di un tenant è centralizzata - un singolo punto, il riflettore delle route (RR) denominato, gestisce il peering BGP in per tutti i siti tenant. Il record di risorse è distribuita tra tutti i gateway in un pool. Ciò produce una configurazione in cui le connessioni di un tenant (percorso dati) terminare in più gateway, ma il record di risorse per il tenant (punto peering BGP - percorso del controllo) è uno solo dei gateway.  
  
Il router BGP separato nel diagramma il concetto di routing centralizzata. L'implementazione di BGP gateway fornisce inoltre il routing di transito, che consente di agire come un punto di transito di routing tra siti due tenant cloud. Queste funzionalità BGP sono applicabili a tutte le funzioni di gateway.  
  
## <a name="bkmk_integration"></a>Integrazione di Gateway RAS con Controller di rete  
Gateway RAS è completamente integrato con il Controller di rete in Windows Server 2016. Quando vengono distribuiti Gateway RAS e Controller di rete, Controller di rete esegue le operazioni seguenti.  
  
-   Distribuzione di pool di gateway  
  
-   Configurazione delle connessioni tenant in ogni gateway  
  
-   Passare il traffico di rete passa a un gateway standby in caso di errore di gateway  
  
Le sezioni seguenti forniscono informazioni dettagliate sui Controller di rete e di Gateway RAS.  
  
-   [Provisioning e bilanciamento del carico di connessioni a Gateway (IKEv2, L3 e GRE)](#bkmk_provisioning)  
  
-   [Disponibilità elevata per IKEv2 S2S](#bkmk_ike)  
  
-   [Disponibilità elevata per GRE](#bkmk_gre)  
  
-   [Disponibilità elevata per L3 gateway di inoltro](#bkmk_l3)  
  
### <a name="bkmk_provisioning"></a>Provisioning e bilanciamento del carico di connessioni a Gateway (IKEv2, L3 e GRE)  
Quando un tenant richiede una connessione gateway, viene inviata la richiesta al Controller di rete. Controller di rete è configurato con informazioni su tutti i pool di gateway, tra cui la capacità di ogni pool e ogni gateway in ogni pool. Controller di rete consente di selezionare il pool corretto e i gateway per la connessione. Questa selezione si basa dei requisiti di larghezza di banda per la connessione. Controller di rete utilizza un algoritmo "adatta" per selezionare le connessioni in modo efficiente in un pool. Il punto di peering BGP per la connessione è designato anche in questo momento, se si tratta della prima connessione del tenant.  
  
Dopo che i Controller di rete consente di selezionare un Gateway RAS per la connessione, Controller di rete esegue il provisioning di configurazione necessarie per la connessione nel gateway. Se la connessione è una connessione S2S IKEv2, Controller di rete disposizioni anche una regola Network Address Translation (NAT) nel pool di SLB; Questa regola NAT nel pool di SLB indica le richieste di connessione dal tenant al gateway designato. I tenant si differenziano per l'IP di origine, deve essere univoco.  
  
> [!NOTE]  
> Connessioni L3 e GRE ignorare il SLB e connettono direttamente con il Gateway RAS designato.  Queste connessioni richiedono che il router endpoint remoto (o altri dispositivi di terze parti) devono essere configurato correttamente per la connessione con il Gateway RAS.  
  
Se il routing BGP è abilitato per la connessione, quindi il peering BGP viene avviato dal Gateway RAS - e route vengono scambiate tra locali e cloud gateway. Le route che appresi da BGP (o che sono configurati in modo statico route se non viene utilizzato il protocollo BGP) vengono inviate al Controller di rete. Controller di rete esegue quindi il plumbing delle route verso il basso per gli host Hyper-V su cui sono installate le macchine virtuali tenant. A questo punto, è possibile instradare il traffico tenant per il corretto nel sito locale. Controller di rete anche Crea criteri di virtualizzazione rete Hyper-V associati che specificano i percorsi di gateway e li esegue il plumbing verso il basso per gli host Hyper-V.  
  
### <a name="bkmk_ike"></a>Disponibilità elevata per IKEv2 S2S  
In un pool di Gateway RAS è costituito da connessioni e il peering BGP del tenant diversi. Ogni pool ha sto ' gateway attivi e gateway standby N'.  
  
Controller di rete gestisce l'errore di gateway nel modo seguente.  
  
-   Controller di rete costantemente ping i gateway in tutti i pool e possono rilevare un gateway che non è riuscito o errori. Controller di rete in grado di rilevare i seguenti tipi di errori di Gateway RAS.  
  
    -   Errore macchina virtuale Gateway RAS  
  
    -   Errore dell'host Hyper-V in cui è in esecuzione il Gateway RAS  
  
    -   Errore servizio Gateway RAS  
  
    Controller di rete archivia la configurazione di tutti i gateway attivi distribuiti. La configurazione prevede le impostazioni di connessione e routing.  
  
-   Quando si verifica un errore di un gateway, influisce connessioni tenant nel gateway, oltre a connessioni tenant che si trovano in altri gateway, ma il cui RR risiede nel gateway non riuscito. I tempi di inattività delle connessioni di quest'ultimo sono inferiore al primo. Quando il Controller di rete rileva un gateway non riuscito, esegue le attività seguenti.  
  
    -   Rimuove le route delle connessioni interessate dagli host di calcolo.  
  
    -   Rimuove i criteri di virtualizzazione rete Hyper-V in tali host.  
  
    -   Seleziona un gateway standby, lo converte in un gateway attivo e consente di configurare il gateway.  
  
    -   Modifica i mapping NAT nel pool di SLB in modo che punti al gateway nuove connessioni.  
  
-   Contemporaneamente, come la configurazione viene visualizzata nel nuovo gateway attive, le connessioni di IKEv2 S2S e BGP peering sono ristabilite. Le connessioni e il peering BGP può essere avviati il gateway del cloud o il gateway locale. I gateway aggiornare le route e inviano a Controller di rete. Dopo che i Controller di rete apprende le route nuovo individuate per i gateway, Controller di rete invia le route e i criteri di virtualizzazione rete Hyper-V associati agli host Hyper-V in cui risiedono le macchine virtuali del tenant interessate errore. Questa attività di Controller di rete è simile nel caso di una nuova impostazione di connessione, si verifica solo su vasta scala.  
  
### <a name="bkmk_gre"></a>Disponibilità elevata per GRE  
Il processo di risposta di failover di Gateway RAS dal Controller di rete - tra cui il rilevamento degli errori, la copia di connessione e configurazione di routing per il gateway standby, failover di routing BGP o statico di connessioni interessate (inclusi il ritiro e nuovamente plumbing delle route nel calcolo host e nuovamente il peering BGP) e la riconfigurazione dei criteri di virtualizzazione rete Hyper-V negli host di calcolo - è lo stesso per le connessioni e i gateway GRE. La nuova creazione di connessioni GRE avviene in modo diverso, tuttavia, la soluzione a disponibilità elevata per GRE è alcuni requisiti aggiuntivi.  
  
![Disponibilità elevata per GRE](../../../media/RAS-Gateway-High-Availability/ras_ha.png)  
  
Al momento della distribuzione di gateway, ogni macchina virtuale Gateway RAS è assegnato un indirizzo IP dinamico (DIP). Inoltre, ogni macchina virtuale gateway viene inoltre assegnato un indirizzo IP virtuale (VIP) per la disponibilità elevata GRE. Gli indirizzi VIP vengono assegnati solo per i gateway nei pool possono accettare connessioni GRE e non in pool non GRE. Gli indirizzi VIP assegnato vengono annunciate all'inizio dei commutatori rack (TOR) utilizzando il protocollo BGP, che quindi annuncia ulteriormente gli indirizzi VIP alla rete fisica cloud. In questo modo i gateway raggiungibili dal router remoti o i dispositivi di terze parti in cui si trova a altra estremità della connessione GRE. Il peering BGP è diverso da quello il peering per lo scambio delle route tenant di livello tenant BGP.  
  
Al momento del provisioning della connessione GRE, Controller di rete consente di selezionare un gateway, consente di configurare un endpoint GRE nel gateway selezionato e restituisce nuovamente l'indirizzo VIP del gateway assegnato. Questo indirizzo VIP viene quindi configurata come indirizzo di tunnel GRE sul router remoto di destinazione.  
  
Quando un gateway non riesce, Controller di rete copia l'indirizzo VIP del gateway non riuscito e altri dati di configurazione per il gateway standby. Quando il gateway standby diventa attivo, annuncia l'indirizzo VIP al commutatore TOR e in maggiore dettaglio in una rete fisica. Router remoti continuare a cui connettersi tunnel GRE stesso indirizzo VIP e l'infrastruttura di routing garantisce che i pacchetti vengono instradati al nuovo gateway attivo.  
  
### <a name="bkmk_l3"></a>Disponibilità elevata per L3 gateway di inoltro  
Un gateway di inoltro L3 di virtualizzazione rete Hyper-V è un collegamento tra l'infrastruttura fisica nel Data Center e l'infrastruttura virtualizzata nel cloud virtualizzazione rete Hyper-V. Un gateway di inoltro L3 multi-tenant, ogni tenant utilizza la propria rete logica con tag VLAN per la connettività con rete fisica del tenant.  
  
Quando un nuovo tenant crea un nuovo gateway L3, Gestione servizio Gateway di Controller di rete consente di selezionare un gateway macchina virtuale a disponibilità e configura una nuova interfaccia tenant con un indirizzo IP di spazio indirizzo cliente (CA) a disponibilità elevata (da VLAN con tag rete logica del tenant). L'indirizzo IP viene utilizzato come l'indirizzo IP del peer nel gateway fisico rete remota ed è l'Hop successivo per raggiungere la virtualizzazione rete Hyper-V rete tenant.  
  
A differenza delle connessioni di rete IPsec o GRE, il commutatore TOR non apprenderanno rete tag VLAN del tenant in modo dinamico. Il routing di rete con codifica VLAN del tenant deve essere configurata sul commutatore TOR e tutti i commutatori intermedi e router tra infrastruttura fisica e il gateway per verificare la connettività end-to-end.  Di seguito è un esempio di configurazione di rete virtuale CSP, come illustrato nella figura seguente.  
  
|Rete|Subnet|ID VLAN|Gateway predefinito|  
|-----------|----------|-----------|-------------------|  
|Rete logica contoso L3|10.127.134.0/24|1001|10.127.134.1|  
|Rete logica Woodgrove L3|10.127.134.0/24|1002|10.127.134.1|  
  
Di seguito sono le configurazioni di esempio tenant gateway come illustrato nella figura seguente.  
  
|Nome del tenant|Gateway L3 indirizzo IP|ID VLAN|Indirizzo IP del peer|  
|---------------|-------------------------|-----------|-------------------|  
|Contoso|10.127.134.50|1001|10.127.134.55|  
|Woodgrove|10.127.134.60|1002|10.127.134.65|  
  
Di seguito è l'illustrazione di queste configurazioni in un Data Center CSP.  
  
![Disponibilità elevata per L3 gateway di inoltro](../../../media/RAS-Gateway-High-Availability/ras_fwdgw.png)  
  
Il processo di failover gateway nel contesto di un L3 gateway di inoltro, il rilevamento degli errori e gli errori di gateway è simile ai processi di IKEv2 e gateway RAS GRE. Le differenze sono in modalità di gestione indirizzi IP esterni.  
  
Quando il gateway di stato della macchina virtuale risulta non integro, Controller di rete consente di selezionare uno dei gateway standby dal pool di e disposizioni nuovamente le connessioni di rete e routing nel gateway standby. Durante lo spostamento di elevata le connessioni, il gateway di inoltro L3 indirizzo IP disponibile in spazio CA viene spostato anche nuovo gateway VM con lo spazio CA indirizzo IP BGP del tenant.  
  
Poiché l'indirizzo IP di Peering L3 viene spostato in nuovo gateway VM durante il failover, l'infrastruttura fisica remoto è nuovamente in grado di connettersi a questo indirizzo IP e, successivamente, raggiungere il carico di lavoro di virtualizzazione rete Hyper-V. Per BGP routing dinamico, come lo spazio CA indirizzo IP BGP viene spostato al gateway nuova macchina virtuale, il Router BGP remoti possono ristabilire peering e scopri nuovamente tutte le route di virtualizzazione rete Hyper-V.  
  
> [!NOTE]  
> È necessario configurare i commutatori TOR e tutti i router intermedi separatamente per poter usare la rete logica con tag VLAN per la comunicazione tenant. Inoltre, L3 failover è limitato a solo il rack che sono configurati in questo modo. Per questo motivo, il pool di gateway L3 deve essere configurato con attenzione e la configurazione manuale deve essere eseguita separatamente.  
  


