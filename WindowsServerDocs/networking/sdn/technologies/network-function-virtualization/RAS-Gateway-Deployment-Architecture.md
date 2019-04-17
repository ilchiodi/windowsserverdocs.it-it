---
title: Architettura di distribuzione di Gateway RAS
description: È possibile utilizzare questo argomento per informazioni sulla distribuzione del Gateway RAS in Windows Server 2016, inclusi i pool di Gateway RAS, catadiottri Route e la distribuzione di più gateway per i singoli tenant del Provider di servizi Cloud (CSP).
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: d46e4e91-ece0-41da-a812-af8ab153edc4
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 21b101e10dba3d3b9578d6804b4fd92fbbcd2167
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="ras-gateway-deployment-architecture"></a>Architettura di distribuzione di Gateway RAS

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per informazioni sulla distribuzione di Provider di servizi Cloud (CSP) di Gateway RAS, inclusi i pool di Gateway RAS, catadiottri Route e la distribuzione di più gateway per i singoli tenant.  
  
Le sezioni seguenti forniscono brevi descrizioni di alcune delle nuove funzionalità di Gateway RAS in modo che consentono di comprendere come usare queste funzionalità nella progettazione della distribuzione di gateway.  
  
Inoltre, viene fornito un esempio di distribuzione, incluse informazioni sul processo di aggiunta di nuovo tenant, sincronizzazione di route e routing piano dati, gateway e il failover riflettore delle Route e altro.  
  
In questo argomento contiene le sezioni seguenti.  
  
-   [Utilizzo delle nuove funzionalità di Gateway RAS la progettazione della distribuzione](#bkmk_new)  
  
-   [Esempio di distribuzione](#bkmk_example)  
  
-   [Aggiunta di nuovo tenant e spazio di indirizzi (CA) cliente EBGP Peering](#bkmk_tenant)  
  
-   [Sincronizzazione di route e dati piano Routing](#bkmk_route)  
  
-   [Modalità di risposta Controller di rete per Gateway RAS e il Failover di Reflector Route](#bkmk_failover)  
  
-   [Vantaggi dell'utilizzo di nuove funzionalità di Gateway RAS](#bkmk_advantages)  
  
## <a name="bkmk_new"></a>Utilizzo delle nuove funzionalità di Gateway RAS la progettazione della distribuzione  
Gateway RAS include diverse nuove funzionalità che modificano e migliorare il modo in cui si distribuisce l'infrastruttura di gateway nel centro dati.  
  
### <a name="bgp-route-reflector"></a>Riflettore delle Route BGP  
La funzionalità del protocollo BGP (Border Gateway) Route Reflector è ora incluso con Gateway RAS e fornisce un'alternativa alla topologia a maglia completa BGP normalmente richiesto per la sincronizzazione di route tra i router. A maglia completa sincronizzazione, tutti i router BGP devono connettersi con tutti gli altri router nella topologia di routing. Quando si utilizza Route Reflector, tuttavia, il riflettore delle Route è l'unico router che si connette con tutti gli altri router, denominati riflettore delle Route BGP client, semplificando in questo modo la sincronizzazione di route e ridurre il traffico di rete. Il riflettore delle Route apprende tutte le route, route migliori calcola e ridistribuisce le route migliori ai propri client BGP.  
  
Per ulteriori informazioni, vedere [What's New in Gateway RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md).  
  
### <a name="bkmk_pools"></a>Pool di gateway  
In Windows Server 2016, è possibile creare pool di gateway molti tipi diversi. Pool di gateway includono più istanze del Gateway RAS e instradare il traffico di rete tra reti fisiche e virtuali.  
  
Per ulteriori informazioni, vedere [What's New in Gateway RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) e [disponibilità elevata del Gateway RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
### <a name="bkmk_gps"></a>Scalabilità dei Pool di gateway  
È possibile scalare facilmente un pool di gateway verso l'alto o verso il basso aggiungendo o rimuovendo le macchine virtuali gateway nel pool. Rimozione o aggiunta di gateway non interrotti i servizi forniti da un pool. Puoi anche aggiungere e rimuovere l'intero pool di gateway.  
  
Per ulteriori informazioni, vedere [What's New in Gateway RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) e [disponibilità elevata del Gateway RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
### <a name="bkmk_m"></a>Ridondanza dei Pool di Gateway M + N  
Ogni pool di gateway è N + M ridondanti. Questo significa che un sto ' numero di macchine virtuali gateway attive viene eseguito il backup da un numero n di macchine virtuali gateway standby. M + N ridondanza offre una maggiore flessibilità nella determinazione del livello di affidabilità che è necessario quando si distribuisce Gateway RAS.  
  
Per ulteriori informazioni, vedere [What's New in Gateway RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) e [disponibilità elevata del Gateway RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_example"></a>Esempio di distribuzione  
Nella figura seguente viene fornito un esempio con eBGP peering su connessioni VPN da sito a sito tra due tenant, Contoso e Woodgrove e nel Data Center CSP Fabrikam configurate.  
  
![eBGP peering su VPN da sito a sito](../../../media/RAS-Gateway-Deployment-Architecture/ras_gateway_architecture.png)  
  
In questo esempio, Contoso richiede larghezza di banda aggiuntiva gateway, causando la decisione di progettazione dell'infrastruttura di gateway per terminare il sito Contoso Los Angeles in GW3 anziché GW2. Per questo motivo, le connessioni VPN di Contoso da diversi siti terminare nel Data Center in due diversi gateway CSP.  
  
Entrambi questi gateway, GW2 e GW3, sono il primo gateway RAS configurato dal Controller di rete quando il provider CSP aggiunto i tenant di Contoso e Woodgrove alla propria infrastruttura. Per questo motivo, queste due gateway vengono configurati come catadiottri Route per questi clienti corrispondenti (o tenant). GW2 è il riflettore delle Route di Contoso e GW3 è il riflettore delle Route Woodgrove - oltre a essere il punto di interruzione Gateway RAS CSP per la connessione VPN con il sito Contoso Los Angeles sede.  
  
> [!NOTE]  
> Un Gateway RAS può instradare il traffico di rete fisici e virtuali per tenant di cento fino a diversi, a seconda dei requisiti di larghezza di banda di ogni tenant.  
  
Come catadiottri Route, GW2 invia route spazio CA Contoso al Controller di rete e GW3 invia route spazio CA Woodgrove al Controller di rete.  
  
Controller di rete effettua il push criteri di virtualizzazione rete Hyper-V per Contoso e Woodgrove reti virtuali, nonché i criteri per il gateway RAS e il bilanciamento del carico multiplexer (MUXes) che sono configurati come un pool di bilanciamento del carico Software criteri RAS.  
  
## <a name="bkmk_tenant"></a>Aggiunta di nuovo tenant e lo spazio indirizzo cliente (CA) eBGP Peering  
Quando accedi a un nuovo cliente e aggiungere il cliente come un nuovo tenant nel centro dati, è possibile utilizzare il processo seguente, la maggior parte dei quali viene eseguita automaticamente dal Controller di rete e RAS Gateway router eBGP.  
  
1.  Effettuare il provisioning di una nuova rete virtuale e i carichi di lavoro in base ai requisiti del tenant.  
  
2.  Se necessario, configurare la connettività remota tra il sito aziendale tenant remote e la rete virtuale del Data Center. Quando si distribuisce una connessione VPN da sito a sito per il tenant, seleziona una macchina virtuale Gateway RAS disponibile dal pool di gateway disponibili automaticamente Controller di rete e configura la connessione.  
  
3.  Durante la configurazione della macchina virtuale Gateway RAS per il nuovo tenant, Controller di rete consente di configurare il Gateway RAS come Router BGP e lo designa come il riflettore delle Route per il tenant. Ciò vale anche nel caso in cui il Gateway RAS viene utilizzato come gateway o come un gateway e il riflettore delle Route, per altri tenant.  
  
4.  A seconda che il routing spazio CA è configurato per reti di uso configurato in modo statico o il routing dinamico BGP, Controller di rete consente di configurare le route statiche corrispondente, router adiacenti BGP o entrambi nella macchina virtuale Gateway RAS e riflettore delle Route.  
  
    > [!NOTE]  
    > -   Dopo che il Controller di rete è configurato un Gateway RAS e il riflettore delle Route per il tenant, ogni volta che il tenant stesso richiede una nuova connessione VPN site-to-site, controlla il Controller di rete per la capacità disponibile in questa macchina virtuale Gateway RAS. Se il gateway originale può servizio necessario che la capacità, la nuova connessione di rete è configurata anche nella stessa macchina virtuale Gateway RAS. Se la macchina virtuale Gateway RAS non può gestire capacità aggiuntiva, Controller di rete seleziona una macchina virtuale Gateway RAS disponibili nuovi e configura la connessione di nuovo su di esso. Questa nuova macchina virtuale Gateway RAS associate al tenant diventa il client di Reflector Route del riflettore delle Route Gateway RAS tenant originale.  
    > -   Poiché il pool di Gateway RAS è protetti da servizi di bilanciamento del carico Software (SLBs), ogni uso VPN da sito a sito dei tenant risolve un unico indirizzo IP pubblico, chiamato un indirizzo IP virtuale (VIP), viene tradotto dal SLBs in un indirizzo IP interno Data Center, chiamato un indirizzo IP dinamico (DIP), per un Gateway RAS che instrada il traffico per il tenant dell'organizzazione. Questo mapping di indirizzi IP per pubblica-privata da SLB garantisce che i tunnel VPN site-to-site siano impostati correttamente tra i siti dell'organizzazione e il gateway RAS CSP e catadiottri Route.  
    >   
    >     Per ulteriori informazioni su SLB, gli indirizzi VIP e DIP, vedere [il bilanciamento del carico Software & #40; SLB & #41; per SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).  
  
5.  Dopo il tunnel VPN da sito a sito tra il sito dell'organizzazione e nel Data Center CSP che RAS Gateway viene stabilita per il nuovo tenant, le route statiche che sono associate i tunnel vengono automaticamente il provisioning in CSP sia aziendali i lati del tunnel.  
  
6.  Con spazio CA BGP routing, il peering tra i siti dell'organizzazione e il riflettore delle Route di Gateway RAS CSP eBGP viene anche stabilito.  
  
## <a name="bkmk_route"></a>Sincronizzazione di route e dati piano Routing  
Dopo aver stabilito peering eBGP tra siti aziendali e il riflettore delle Route di Gateway RAS CSP, il riflettore delle Route apprende tutte le route Enterprise usando il routing dinamico BGP. Il riflettore delle Route Sincronizza le route tra tutti i client di Reflector Route in modo che tutti sono configurati con lo stesso set di route.  
  
Route Reflector aggiorna anche queste route consolidate, tramite la sincronizzazione di route, al Controller di rete. Controller di rete quindi converte le route in Criteri di virtualizzazione rete Hyper-V e configura l'infrastruttura di rete per assicurarsi che il routing di End-to-End percorso dati viene eseguito il provisioning. Questo processo consente al tenant di rete virtuale accessibile dal tenant aziendale siti.  
  
Per il piano dati routing, i pacchetti in grado di raggiungere le macchine virtuali Gateway RAS vengono instradati direttamente alla rete virtuale del tenant, perché le route richieste sono ora disponibili con tutte le macchine virtuali Gateway RAS partecipanti.  
  
Con i criteri di virtualizzazione rete Hyper-V sul posto, in modo analogo, la rete virtuale del tenant instrada i pacchetti direttamente per le macchine virtuali Gateway RAS (senza conoscere il riflettore delle Route) e quindi ai siti aziendali attraverso i tunnel VPN site-to-site.  
  
Inoltre. Il traffico restituito dalla rete virtuale tenant per il sito aziendale tenant remote ignora il SLBs, un processo denominato diretta Server restituire DSR ().  
  
## <a name="bkmk_failover"></a>Modalità di risposta Controller di rete per Gateway RAS e il Failover di Reflector Route  
Di seguito sono due scenari possibili failover - uno per i client riflettore delle Route Gateway RAS - e uno per riflettori delle Route Gateway RAS incluse informazioni su come Controller di rete gestisce il failover per le macchine virtuali in entrambe le configurazioni.  
  
### <a name="vm-failure-of-a-ras-gateway-bgp-route-reflector-client"></a>Errore macchina virtuale di un Client di Reflector Route BGP Gateway RAS  
Quando un client riflettore delle Route Gateway RAS non riesce, Controller di rete esegue le operazioni seguenti.  
  
> [!NOTE]  
> Quando un Gateway RAS non è un riflettore di Route per l'infrastruttura di un tenant BGP, è un client di Reflector Route nell'infrastruttura BGP del tenant.  
  
-   Controller di rete seleziona una macchina virtuale Gateway RAS standby disponibili e la nuova macchina virtuale Gateway RAS con la configurazione della macchina virtuale Gateway RAS esegue il provisioning.  
  
-   Controller di rete aggiorna la configurazione di SLB corrispondente per garantire che i tunnel VPN site-to-site da siti tenant al Gateway RAS siano impostati correttamente con il nuovo Gateway RAS.  
  
-   Controller di rete consente di configurare il client di Reflector Route BGP nel nuovo gateway.  
  
-   Controller di rete consente di configurare il nuovo client riflettore di Route BGP Gateway RAS come attiva. Il Gateway RAS avvia immediatamente peering con Route Reflector del tenant di condividere le informazioni di routing e per consentire il peering eBGP per il corrispondente sito dell'organizzazione.  
  
### <a name="vm-failure-for-a-ras-gateway-bgp-route-reflector"></a>Errore di macchina virtuale di un riflettore delle Route BGP Gateway RAS  
Controller di rete esegue le operazioni seguenti quando si verifica un errore di un riflettore di Route BGP Gateway RAS.  
  
-   Controller di rete seleziona una macchina virtuale Gateway RAS standby disponibili e la nuova macchina virtuale Gateway RAS con la configurazione della macchina virtuale Gateway RAS esegue il provisioning.  
  
-   Controller di rete configura il riflettore delle Route nella nuova macchina virtuale Gateway RAS e assegna alla nuova macchina virtuale lo stesso indirizzo IP utilizzato dalla macchina virtuale non riuscita, fornendo quindi l'integrità delle route nonostante l'impossibilità di macchina virtuale.  
  
-   Controller di rete aggiorna la configurazione di SLB corrispondente per garantire che i tunnel VPN site-to-site da siti tenant al Gateway RAS siano impostati correttamente con il nuovo Gateway RAS.  
  
-   Controller di rete consente di configurare il nuovo BGP Route Reflector macchina virtuale Gateway RAS come attiva.  
  
-   Il riflettore delle Route immediatamente diventa attivo. Viene stabilito il tunnel VPN site-to-site all'organizzazione e il riflettore delle Route Usa peering eBGP e di scambiare le route con il router di sito aziendali.  
  
-   Dopo la selezione delle route BGP, i riflettore delle Route BGP di Gateway RAS tenant i client di Reflector Route nel Data Center degli aggiornamenti e sincronizza le route con Controller di rete, rendendo il percorso dati End-to-End disponibili per il traffico tenant.  
  
## <a name="bkmk_advantages"></a>Vantaggi dell'utilizzo di nuove funzionalità di Gateway RAS  
Ecco alcuni dei vantaggi dell'uso di queste nuove funzionalità di Gateway RAS quando si progetta la distribuzione di Gateway RAS.  
  
**Scalabilità di Gateway RAS**  
  
Poiché è possibile aggiungere come molte macchine virtuali Gateway RAS in base alle esigenze per pool di Gateway RAS, è possibile scalare facilmente la distribuzione di Gateway RAS per ottimizzare le prestazioni e capacità. Quando si aggiungono macchine virtuali a un pool, è possibile configurare questi gateway RAS con connessioni VPN site-to-site di alcun tipo (IKEv2, L3 GRE), eliminando la capacità dei colli di bottiglia senza tempi di inattività.  
  
**Gestione di Gateway di sito aziendali semplificata**  
  
Quando il tenant ha più siti aziendali, è possibile configurare il tenant tutti i siti con un indirizzo di IP VPN da sito a sito remoto e un indirizzo IP singolo router adiacente remoto - Data Center CSP RAS Gateway BGP Route Reflector VIP per tale tenant. Ciò semplifica la gestione gateway per tenant.  
  
**Monitoraggio e aggiornamento rapido di errore di Gateway**  
  
Per garantire una risposta rapida failover, è possibile configurare l'intervallo di parametro BGP Keepalive tra edge itinerari e il router di controllo per un intervallo di tempo breve, ad esempio minore o uguale a dieci secondi. Con questo intervallo alive mantenere breve, in caso di errore, un router perimetrale BGP Gateway RAS rapidamente viene rilevato l'errore e Controller di rete esegue la procedura fornita nelle sezioni precedenti. Questo vantaggio potrebbe ridurre l'esigenza per un protocollo di rilevamento di errore separati, ad esempio il protocollo di rilevamento di inoltro bidirezionale (BFD).  
  
 
  


