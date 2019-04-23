---
title: Disponibilità elevata di Gateway RAS
description: È possibile utilizzare questo argomento per informazioni sulle configurazioni a disponibilità elevata per il Gateway multi-tenant RAS per SDN Software Defined Networking () in Windows Server 2016.
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
ms.openlocfilehash: 8ce515ceeb7ab6989ef18055f312983a6518a1a1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847762"
---
# <a name="ras-gateway-high-availability"></a>Disponibilità elevata di Gateway RAS

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni sulle configurazioni a disponibilità elevata per il Gateway multi-tenant RAS per reti SDN (Software Defined).  
  
In questo argomento sono incluse le sezioni seguenti.  
  
-   [Panoramica del Gateway RAS](#bkmk_overview)  
  
-   [Panoramica dei pool di gateway](#bkmk_pools)  
  
-   [Cenni preliminari sulla distribuzione di Gateway RAS](#bkmk_deployment)  
  
-   [Integrazione di Gateway RAS con Controller di rete](#bkmk_integration)  
  
## <a name="bkmk_overview"></a>Panoramica del Gateway RAS  
Se l'organizzazione è un Provider del servizio Cloud (CSP) o un'organizzazione con più tenant, è possibile distribuire Gateway RAS in modalità multi-tenant per fornire rete il routing del traffico da e verso reti virtuali e fisiche, Internet incluso.  
  
È possibile distribuire Gateway RAS in modalità multi-tenant come un gateway perimetrale per instradare il traffico di rete cliente di tenant per tenant delle risorse e le reti virtuali.  
  
Quando si distribuiscono più istanze di macchine virtuali Gateway RAS offrire la disponibilità elevata e failover, si distribuisce un pool di gateway. In Windows Server 2012 R2, tutti i gateway di macchine virtuali nel formato corretto un singolo pool che ha effettuato una separazione logica di distribuzione del gateway complessa.  Gateway di Windows Server 2012 R2 è disponibile una distribuzione di ridondanza 1:1 per il gateway di macchine virtuali, questo ha comportato correggerà utilizzo della capacità per site-to-site (S2S) disponibili connessioni VPN.  
  
Questo problema viene risolto in Windows Server 2016, che offre più pool di Gateway, che può essere di tipi diversi per la separazione logica. La nuova modalità di ridondanza N + M consente una configurazione di failover più efficiente.  
  
Per altre informazioni generali sul Gateway RAS, vedere [RAS Gateway](../../../../remote/remote-access/ras-gateway/RAS-Gateway.md).  
  
## <a name="bkmk_pools"></a>Panoramica dei pool di gateway  
In Windows Server 2016, è possibile distribuire gateway in uno o più pool.  
  
La figura seguente mostra diversi tipi di pool di gateway che forniscono il routing del traffico tra reti virtuali.  
  
![Pool di Gateway RAS](../../../media/RAS-Gateway-High-Availability/ras_pools.png)  
  
Ogni pool ha le proprietà seguenti.  
  
-   Ogni pool è N + M ridondanti. Ciò significa che un sto ' numero di macchine virtuali gateway attive viene sottoposti a backup da un numero n di macchine virtuali gateway standby. Il valore di N (gateway standby) è sempre minore o uguale a M (gateway attivo).  
  
-   Un pool può eseguire una delle funzioni di gateway singoli - Internet Key Exchange versione 2 (IKEv2) S2S, livello 3 (L3) e Generic Routing Encapsulation (GRE) - o il pool può eseguire tutte queste funzioni.  
  
-   È possibile assegnare un singolo indirizzo IP pubblico per tutti i pool o a un sottoinsieme dei pool. In questo modo notevolmente riduce il numero di indirizzi IP pubblici che è necessario usare, perché è possibile avere tutti i tenant di connettere al cloud in un unico indirizzo IP. La sezione seguente sulla disponibilità elevata e bilanciamento del carico descrive il funzionamento.  
  
-   È possibile scalare facilmente un pool di gateway verso l'alto o verso il basso aggiungendo o rimuovendo le macchine virtuali gateway nel pool. Rimozione o aggiunta di gateway non interrotti i servizi forniti da un pool. È inoltre possibile aggiungere e rimuovere l'intero pool di gateway.  
  
-   Le connessioni di un tenant singolo possono terminare in più pool e più gateway in un pool. Tuttavia, se un tenant dispone di connessioni che termina con un **tutti i** digitare il pool di gateway, non può sottoscrivere altri **tutti i** tipo o i pool di gateway di tipo individuale.  
  
I pool di gateway offrono anche la flessibilità necessaria per abilitare scenari aggiuntivi:  
  
-   Pool a tenant singolo: è possibile creare un pool per l'uso da un unico tenant.  
  
-   Se si vende servizi cloud tramite i canali di partner (rivenditore), è possibile creare più set di pool per ogni rivenditore.  
  
-   Più pool può fornire la stessa funzione di gateway ma capacità differenti. Ad esempio, è possibile creare un pool di gateway che supporta una velocità effettiva elevata e connessioni S2S IKEv2 velocità effettiva bassa.  
  
## <a name="bkmk_deployment"></a>Cenni preliminari sulla distribuzione di Gateway RAS  
La figura seguente illustra una tipica distribuzione di Cloud Service Provider (CSP) di Gateway RAS.  
  
![Cenni preliminari sulla distribuzione di Gateway RAS](../../../media/RAS-Gateway-High-Availability/ras_csp_deploy.png)  
  
Con questo tipo di distribuzione, i pool di gateway vengono distribuiti dietro un Software carico bilanciamento (SLB), che consente al CSP di assegnare un singolo indirizzo IP pubblico per l'intera distribuzione. Le connessioni del gateway più di un tenant possono terminare in più pool di gateway - e anche in più gateway in un pool. Questo comportamento è illustrato tramite connessioni S2S IKEv2 nel diagramma precedente, ma lo stesso è applicabile ad altre funzioni di gateway, ad esempio gateway GRE e L3.  
  
Nell'illustrazione, il dispositivo di destinazione master BGP è un Gateway multi-tenant RAS con BGP. Multi-tenant BGP viene usato per il routing dinamico. Il routing per un tenant è centralizzata: un singolo punto, chiamato il riflettore delle route (RR), gestisce il peering BGP per tutti i siti tenant. Il record di risorse viene distribuito tra tutti i gateway in un pool. Ciò comporta una configurazione in cui terminare le connessioni di un tenant (percorso dati) in più gateway, ma il record di risorse per il tenant (punto peering BGP - percorso del controllo) è in uno solo dei gateway.  
  
Il router BGP è separato nel diagramma per illustrare questo concetto di routing centralizzato. L'implementazione di BGP gateway fornisce anche il routing di transito, che consente di agire come un punto di transito per il routing tra siti due tenant cloud. Queste funzionalità BGP sono applicabili a tutte le funzioni di gateway.  
  
## <a name="bkmk_integration"></a>Integrazione di Gateway RAS con Controller di rete  
Gateway RAS è completamente integrato con Controller di rete in Windows Server 2016. Quando vengono distribuiti i Controller di rete e RAS Gateway, Controller di rete esegue le funzioni seguenti.  
  
-   Distribuzione dei pool di gateway  
  
-   Configurazione delle connessioni tenant su ogni gateway  
  
-   Passare il traffico di rete passano a un gateway in standby in caso di errore di gateway  
  
Le sezioni seguenti forniscono informazioni dettagliate sul Gateway RAS e Controller di rete.  
  
-   [Il provisioning e bilanciamento del carico delle connessioni del Gateway (IKEv2, L3 e GRE)](#bkmk_provisioning)  
  
-   [Disponibilità elevata per IKEv2 S2S](#bkmk_ike)  
  
-   [Disponibilità elevata per GRE](#bkmk_gre)  
  
-   [Disponibilità elevata per L3 i gateway di inoltro](#bkmk_l3)  
  
### <a name="bkmk_provisioning"></a>Il provisioning e bilanciamento del carico delle connessioni del Gateway (IKEv2, L3 e GRE)  
Quando un tenant richiede una connessione gateway, la richiesta viene inviata al Controller di rete. Controller di rete è configurato con informazioni su tutti i pool di gateway, tra cui la capacità di ogni pool e ogni gateway in ogni pool. Controller di rete consente di selezionare il pool corretti e il gateway per la connessione. Questa selezione si basa sul requisito di larghezza di banda per la connessione. Controller di rete Usa un algoritmo "best fit" per selezionare le connessioni in modo efficiente in un pool. Il punto di peering BGP per la connessione viene anche designato al momento se si tratta della prima connessione del tenant.  
  
Dopo che il Controller di rete consente di selezionare un Gateway RAS per la connessione, Controller di rete esegue il provisioning di configurazione necessarie per la connessione del gateway. Se la connessione è una connessione S2S IKEv2, Controller di rete inoltre effettua il provisioning di una regola Network Address Translation (NAT) nel pool di bilanciamento del carico software; Questa regola NAT per il pool di bilanciamento del carico software indirizza le richieste di connessione dal tenant al gateway designato. I tenant si differenziano per l'IP di origine, che dovrà essere univoco.  
  
> [!NOTE]  
> Le connessioni GRE e L3 ignorano il bilanciamento del carico software e potrai interagire direttamente con il Gateway RAS designato.  Queste connessioni richiedono che il router di endpoint remoto (o altro dispositivo di terze parti) deve essere configurato correttamente per connettersi con il Gateway RAS.  
  
Se il routing BGP è abilitato per la connessione, il peering BGP viene avviato dal Gateway RAS - e le route vengono scambiate tra origini locali e cloud gateway. Le route che vengono appresi dalle BGP (o che sono configurate staticamente route se non viene utilizzato il protocollo BGP) vengono inviate al Controller di rete. Controller di rete esegue il plumbing quindi le route fino agli host Hyper-V su cui sono installate le macchine virtuali tenant. A questo punto, il traffico del tenant può essere indirizzato al sito corretto in locale. Controller di rete anche crea i criteri di virtualizzazione rete Hyper-V associati che specificano le posizioni di gateway e li esegue il plumbing down gli host Hyper-V.  
  
### <a name="bkmk_ike"></a>Disponibilità elevata per IKEv2 S2S  
Un Gateway RAS in un pool è costituito da connessioni e il peering BGP di diversi tenant. Ogni pool ha sto ' gateway attivo e standby "n".  
  
Controller di rete gestisce l'errore di gateway nel modo seguente.  
  
-   Controller di rete costantemente ping del gateway in tutti i pool ed è possibile rilevare un gateway che non è riuscito o errore. Controller di rete può rilevare i seguenti tipi di errori di Gateway RAS.  
  
    -   Errore di macchina virtuale Gateway RAS  
  
    -   Errore dell'host Hyper-V su cui è in esecuzione il Gateway RAS  
  
    -   Errore del servizio Gateway RAS  
  
    Controller di rete archivia la configurazione di tutti i gateway active distribuiti. Configurazione è costituita da impostazioni di connessione e le impostazioni del routing.  
  
-   Quando un gateway non riesce, influisce sulle connessioni tenant nel gateway, nonché le connessioni di tenant che si trovano in un altro gateway, ma la cui RR risieda nel gateway non riuscite. Il tempo di inattività delle connessioni di quest'ultime è minore di nel primo caso. Quando il Controller di rete rileva un gateway non riuscito, esegue le attività seguenti.  
  
    -   Rimuove le route delle connessioni interessate dagli host di calcolo.  
  
    -   Rimuove i criteri di virtualizzazione rete Hyper-V in tali host.  
  
    -   Consente di selezionare un gateway in standby, lo converte in un gateway attivo e consente di configurare il gateway.  
  
    -   Modifica i mapping NAT per il pool di bilanciamento del carico software in modo da puntare le connessioni al nuovo gateway.  
  
-   Contemporaneamente, come la configurazione viene visualizzata nel nuovo gateway attivo, le connessioni S2S IKEv2 e i peering BGP sono ristabilite. Le connessioni e i peering BGP può essere avviate tramite il gateway nel cloud o il gateway da sito locale. I gateway di aggiornare le route e inviano al Controller di rete. Dopo che il Controller di rete apprende le route di nuovo individuate dai gateway, Controller di rete invia le route e i criteri di virtualizzazione rete Hyper-V associati agli host Hyper-V in cui si trovano le macchine virtuali dei tenant interessati errore. Questa attività di Controller di rete è simile nel caso di una nuova impostazione di connessione, si verifica solo su vasta scala.  
  
### <a name="bkmk_gre"></a>Disponibilità elevata per GRE  
Il processo di risposta del failover del Gateway RAS dal Controller di rete - tra cui il rilevamento degli errori, la copia di connessione e configurazione di routing al gateway di standby, il failover di routing BGP/statico le connessioni interessate (incluso il prelievo e nuovamente plumbing delle route nel calcolare gli host e i peering BGP di nuovo), e riconfigurazione dei criteri di virtualizzazione rete Hyper-V negli host di calcolo - è lo stesso per gateway GRE e delle connessioni. La riesecuzione della connessioni GRE avviene in modo diverso, tuttavia, e la soluzione a disponibilità elevata per GRE prevede alcuni requisiti aggiuntivi.  
  
![Disponibilità elevata per GRE](../../../media/RAS-Gateway-High-Availability/ras_ha.png)  
  
Al momento della distribuzione del gateway, tutte le macchine Virtuali Gateway RAS è assegnata un indirizzo IP dinamico (DIP). Inoltre, ogni VM del gateway viene inoltre assegnata un indirizzo IP virtuale (VIP) per la disponibilità elevata GRE. Vengono assegnati i VIP solo per i gateway in pool che può accettare connessioni GRE e non per i pool non GRE. Gli indirizzi VIP assegnati vengono annunciati nella parte superiore di opzioni di rack (TOR) tramite BGP, che quindi annuncia ulteriormente gli indirizzi VIP alla rete fisica del cloud. In questo modo i gateway raggiungibili dal router remoti o i dispositivi di terze parti in cui si trova a altra estremità della connessione GRE. Questo peering BGP è diverso dal a livello di tenant peer BGP per lo scambio delle route di tenant.  
  
Al momento del provisioning connessione GRE, Controller di rete consente di selezionare un gateway, consente di configurare un endpoint GRE nel gateway selezionato e restituisce l'indirizzo VIP del gateway assegnato. Questo indirizzo VIP viene quindi configurato come l'indirizzo di tunnel GRE nel router remoto di destinazione.  
  
Quando un gateway non riesce, Controller di rete consente di copiare l'indirizzo VIP del gateway non riuscite e altri dati di configurazione per il gateway in standby. Quando il gateway standby diventa attivo, annuncia l'indirizzo VIP per il commutatore TOR e ulteriormente in rete fisica. Router remoti continuano a connettersi i tunnel GRE per lo stesso indirizzo VIP e l'infrastruttura di routing assicura che i pacchetti vengono instradati al nuovo gateway attivo.  
  
### <a name="bkmk_l3"></a>Disponibilità elevata per L3 i gateway di inoltro  
Un gateway di inoltro L3 di virtualizzazione rete Hyper-V è un bridge tra l'infrastruttura fisica nel Data Center e l'infrastruttura virtualizzata nel cloud di virtualizzazione rete Hyper-V. In un gateway di inoltro L3 multi-tenant, ogni tenant Usa la propria rete logica con tag VLAN per la connettività con la rete fisica del tenant.  
  
Quando un nuovo tenant viene creato un nuovo gateway L3, gestione del servizio Gateway Controller di rete consente di selezionare un gateway a disponibilità della macchina virtuale e configura una nuova interfaccia di tenant con un indirizzo IP di spazio indirizzo cliente (CA) a disponibilità elevata (dalla rete logica con tag VLAN del tenant ). L'indirizzo IP viene utilizzato come l'indirizzo IP del peer nel gateway remoto (rete fisica) e sia l'Hop successivo per raggiungere la virtualizzazione rete Hyper-V rete tenant.  
  
A differenza delle connessioni di rete IPsec o GRE, il commutatore TOR non impareranno rete con tag VLAN del tenant in modo dinamico. Il routing di rete con tag VLAN del tenant deve essere configurata nel commutatore TOR e tutti gli intermedi commutatori e router tra infrastruttura fisica e il gateway per garantire la connettività end-to-end.  Di seguito è un esempio di configurazione di rete virtuale di CSP, come illustrato nell'immagine seguente.  
  
|Rete|Subnet|ID VLAN|Gateway predefinito|  
|-----------|----------|-----------|-------------------|  
|Rete logica L3 di Contoso|10.127.134.0/24|1001|10.127.134.1|  
|Rete logica Woodgrove L3|10.127.134.0/24|1002|10.127.134.1|  
  
Di seguito sono le configurazioni di gateway di esempio tenant come illustrato nell'immagine seguente.  
  
|Nome del tenant|Indirizzo IP del gateway L3|ID VLAN|Indirizzo IP del peer|  
|---------------|-------------------------|-----------|-------------------|  
|Contoso|10.127.134.50|1001|10.127.134.55|  
|Woodgrove|10.127.134.60|1002|10.127.134.65|  
  
Seguito è riportato nella figura di queste configurazioni in un Data Center CSP.  
  
![Disponibilità elevata per L3 i gateway di inoltro](../../../media/RAS-Gateway-High-Availability/ras_fwdgw.png)  
  
Gli errori di gateway, il rilevamento degli errori e il processo di failover di gateway nel contesto di un L3 gateway di inoltro è simile per i processi per IKEv2 e i gateway RAS GRE. Le differenze sono in modalità di gestione gli indirizzi IP esterni.  
  
Quando il gateway di stato della macchina virtuale diventa non integro, Controller di rete consente di selezionare uno dei gateway standby dal pool e nuovo effettuato il provisioning le connessioni di rete e routing nel gateway standby. Durante lo spostamento le connessioni, il gateway di inoltro L3 elevata indirizzo IP disponibile spazio di CA venga spostato anche al nuovo gateway VM con lo spazio CA indirizzo IP BGP del tenant.  
  
Poiché l'indirizzo IP di Peering L3 viene spostato al nuovo gateway VM durante il failover, l'infrastruttura fisica remota anche in questo caso è in grado di connettersi a questo indirizzo IP e, successivamente, raggiungere il carico di lavoro di virtualizzazione rete Hyper-V. Per BGP routing dinamico, perché lo spazio CA indirizzo IP BGP viene spostato al nuovo gateway VM, il Router BGP remoto possono ristabilire il peering e informazioni su tutte le route di virtualizzazione rete Hyper-V nuovamente.  
  
> [!NOTE]  
> È necessario configurarle separatamente i commutatori TOR e tutti i router intermedi per poter usare la rete logica con tag VLAN per la comunicazione di tenant. Inoltre, L3 failover è limitato a solo i rack che vengono configurati in questo modo. Per questo motivo, il pool di gateway L3 deve essere configurato con attenzione e la configurazione manuale debba essere completata separatamente.  
  


