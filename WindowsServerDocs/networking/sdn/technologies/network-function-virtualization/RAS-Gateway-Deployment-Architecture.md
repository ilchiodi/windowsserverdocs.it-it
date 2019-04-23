---
title: Architettura della distribuzione di Gateway RAS
description: È possibile utilizzare questo argomento per informazioni sulla distribuzione di Cloud Service Provider (CSP) del Gateway RAS in Windows Server 2016, inclusi i pool di Gateway RAS, riflettori delle Route e la distribuzione di più gateway per singoli tenant.
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
ms.openlocfilehash: a3895e25a2af0437deb9eebe4ad89b110cfc9f2b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870272"
---
# <a name="ras-gateway-deployment-architecture"></a>Architettura della distribuzione di Gateway RAS

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni sulla distribuzione di Cloud Service Provider (CSP) di Gateway RAS, inclusi i pool di Gateway RAS, riflettori delle Route e la distribuzione di più gateway per singoli tenant.  
  
Le sezioni seguenti forniscono una breve panoramica di alcune delle nuove funzionalità di Gateway RAS in modo da poter comprendere come usare queste funzionalità nella progettazione della distribuzione del gateway.  
  
Inoltre, viene fornito un esempio di distribuzione, incluse le informazioni sul processo di aggiunta di nuovi tenant, sincronizzazione di route e routing piano dati, gateway e failover riflettore delle Route e altro ancora.  
  
In questo argomento sono incluse le sezioni seguenti.  
  
-   [Utilizzando RAS Gateway nuove funzionalità per progettare la distribuzione](#bkmk_new)  
  
-   [Esempio di distribuzione](#bkmk_example)  
  
-   [Aggiunta di nuovi tenant e spazio di indirizzi (CA) cliente EBGP Peering](#bkmk_tenant)  
  
-   [Sincronizzazione di route e i dati del piano di Routing](#bkmk_route)  
  
-   [Modalità di risposta Controller di rete per Gateway RAS e il Failover di Reflector Route](#bkmk_failover)  
  
-   [Vantaggi dell'utilizzo delle nuove funzionalità di Gateway RAS](#bkmk_advantages)  
  
## <a name="bkmk_new"></a>Utilizzando RAS Gateway nuove funzionalità per progettare la distribuzione  
Gateway RAS offre molte nuove funzionalità che vengono modificati e migliorare il modo in cui si distribuisce l'infrastruttura gateway nel proprio Data Center.  
  
### <a name="bgp-route-reflector"></a>Riflettore delle Route BGP  
La funzionalità di protocollo BGP (Border Gateway) Route Reflector è ora incluso con Gateway RAS e fornisce un'alternativa alla topologia a maglia completa BGP normalmente richiesto per la sincronizzazione di route tra i router. A maglia completa sincronizzazione, tutti i router BGP devono connettersi con tutti gli altri router nella topologia di routing. Quando si utilizza Route Reflector, tuttavia, il riflettore delle Route è l'unico router che si connette con tutti gli altri router, denominati riflettore delle Route BGP client, semplificando in tal modo la sincronizzazione di route e ridurre il traffico di rete. Il riflettore delle Route apprende tutte le route, route migliori calcola e ridistribuisce le route migliori ai propri client BGP.  
  
Per altre informazioni, vedere [What ' s New in Gateway RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md).  
  
### <a name="bkmk_pools"></a>Pool di gateway  
In Windows Server 2016, è possibile creare più pool di gateway di tipi diversi. Pool di gateway includono più istanze del Gateway RAS e instradare il traffico di rete tra reti fisiche e virtuali.  
  
Per altre informazioni, vedere [What ' s New in Gateway RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) e [disponibilità elevata del Gateway RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
### <a name="bkmk_gps"></a>Gateway Pool Scalability  
È possibile scalare facilmente un pool di gateway verso l'alto o verso il basso aggiungendo o rimuovendo le macchine virtuali gateway nel pool. Rimozione o aggiunta di gateway non interrotti i servizi forniti da un pool. È inoltre possibile aggiungere e rimuovere l'intero pool di gateway.  
  
Per altre informazioni, vedere [What ' s New in Gateway RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) e [disponibilità elevata del Gateway RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
### <a name="bkmk_m"></a>Ridondanza dei Pool di Gateway M + N  
Ogni pool di gateway è N + M ridondanti. Ciò significa che un sto ' numero di macchine virtuali gateway attive viene sottoposti a backup da un numero n di macchine virtuali gateway standby. M + N ridondanza offre una maggiore flessibilità nella determinazione del livello di affidabilità che è necessario quando si distribuisce Gateway RAS.  
  
Per altre informazioni, vedere [What ' s New in Gateway RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) e [disponibilità elevata del Gateway RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_example"></a>Esempio di distribuzione  
Nella figura seguente viene fornito un esempio con peering tramite connessioni VPN site-to-site configurate tra due tenant, Contoso e Woodgrove e Data Center CSP Fabrikam eBGP.  
  
![eBGP peering tramite VPN site-to-site](../../../media/RAS-Gateway-Deployment-Architecture/ras_gateway_architecture.png)  
  
In questo esempio, Contoso richiede larghezza di banda aggiuntive per il gateway, causando la decisione di progettazione dell'infrastruttura gateway per terminare il sito Contoso Los Angeles su GW3 anziché GW2. Per questo motivo, le connessioni VPN di Contoso da siti diversi terminare nel Data Center in due diversi gateway CSP.  
  
Entrambi questi gateway, GW2 e GW3, sono stati il primo gateway RAS configurato dal Controller di rete quando il CSP aggiunto ai tenant Contoso e Woodgrove alla propria infrastruttura. Per questo motivo, questi due gateway vengono configurati come riflettori delle Route per questi clienti corrispondenti (o tenant). GW2 è il riflettore delle Route di Contoso e GW3 è il riflettore delle Route Woodgrove - oltre a essere il punto di terminazione di CSP RAS Gateway per la connessione VPN con il sito Contoso Los Angeles HQ.  
  
> [!NOTE]  
> Un Gateway RAS possibile instradare il traffico di rete fisici e virtuali per tenant diversi fino a cento, a seconda dei requisiti di larghezza di banda di ogni tenant.  
  
Come riflettori delle Route, GW2 invia le route spazio CA di Contoso al Controller di rete che GW3 route spazio CA Woodgrove al Controller di rete.  
  
Controller di rete esegue il push dei criteri di virtualizzazione rete Hyper-V per le reti virtuali di Contoso e Woodgrove, nonché i criteri RAS per il gateway RAS e i criteri per il multiplexer (i MUX) che sono configurati come un Software di bilanciamento del carico di bilanciamento del carico pool.  
  
## <a name="bkmk_tenant"></a>Aggiunta di nuovi tenant e lo spazio indirizzo cliente (CA) eBGP Peering  
Quando si accede un nuovo cliente e aggiungere customer come un nuovo tenant nel proprio Data Center, è possibile usare il processo seguente, la maggior parte dei quali viene eseguita automaticamente dal Controller di rete e RAS Gateway router eBGP.  
  
1.  Effettuare il provisioning di una nuova rete virtuale e i carichi di lavoro in base ai requisiti del proprio tenant.  
  
2.  Se necessario, configurare la connettività remota tra il sito aziendale tenant remote e la relativa rete virtuale al tuo Data Center. Quando si distribuisce una connessione VPN site-to-site per il tenant, Controller di rete consente di selezionare una macchina virtuale Gateway RAS disponibile dal pool di gateway disponibili automaticamente e configura la connessione.  
  
3.  Quando si configura la macchina virtuale Gateway RAS per il nuovo tenant, Controller di rete consente di configurare il Gateway RAS come Router BGP e si definisce come il riflettore delle Route per il tenant. Questo vale anche nel caso in cui il Gateway RAS funge da un gateway, o un gateway e una Route Reflector, per gli altri tenant.  
  
4.  A seconda che il routing di spazio di autorità di certificazione è configurato a reti di uso configurato in modo statico o routing dinamico BGP, Controller di rete consente di configurare le route statiche corrispondenti o vicini BGP sia che la macchina virtuale Gateway RAS riflettore delle Route.  
  
    > [!NOTE]  
    > -   Dopo che il Controller di rete è configurato un Gateway RAS e il riflettore delle Route per il tenant, ogni volta che lo stesso tenant richiede una nuova connessione VPN site-to-site, Controller di rete Cerca la capacità disponibile in questa macchina virtuale Gateway RAS. Se il gateway originale può soddisfare la capacità richiesta, la nuova connessione di rete è configurata anche nella stessa VM Gateway RAS. Se la macchina virtuale Gateway RAS non è possibile gestire capacità aggiuntiva, Controller di rete consente di selezionare una macchina virtuale Gateway RAS disponibili nuovi e configura la nuova connessione su di esso. Questa nuova macchina virtuale Gateway RAS associati al tenant diventa il client di Reflector Route del riflettore delle Route Gateway RAS tenant originale.  
    > -   Poiché i pool di Gateway RAS sono protetti da servizi di bilanciamento del carico Software (SLBs), gli indirizzi VPN site-to-site dei tenant utilizzino un singolo indirizzo IP pubblico, denominato un indirizzo IP virtuale (VIP), che viene convertito dal SLBs in un indirizzo IP interno a Data Center, chiamato un indirizzo IP dinamico (DIP), per un Gateway RAS che instrada il traffico per il tenant dell'organizzazione. Questo mapping di indirizzi IP a pubblica-privata dal bilanciamento del carico software assicura che i tunnel VPN site-to-site siano impostati correttamente tra i siti dell'organizzazione e il gateway RAS CSP e riflettori delle Route.  
    >   
    >     Per altre informazioni sul bilanciamento del carico software, gli indirizzi VIP e DIP, vedere [il bilanciamento del carico Software &#40;SLB&#41; per la rete SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).  
  
5.  Dopo il tunnel VPN site-to-site tra il sito aziendale e il Data Center CSP che RAS Gateway viene stabilita per il nuovo tenant, le route statiche che sono associate i tunnel vengono automaticamente effettuato il provisioning CSP sia dell'organizzazione i lati del tunnel .  
  
6.  Con spazio CA BGP routing, il peering tra i siti dell'organizzazione e il riflettore delle Route di CSP RAS Gateway eBGP viene inoltre stabilito.  
  
## <a name="bkmk_route"></a>Sincronizzazione di route e i dati del piano di Routing  
Dopo aver stabilito il peering eBGP tra siti aziendali e il riflettore delle Route di CSP RAS Gateway, il riflettore delle Route apprende tutte le route Enterprise con routing dinamico BGP. Il riflettore delle Route consente di sincronizzare le route tra tutti i client di Reflector Route in modo che tutti siano configurati con lo stesso set di route.  
  
Route Reflector aggiorna anche queste route consolidate, utilizza la sincronizzazione di route, al Controller di rete. Controller di rete si traduce le route in Criteri di virtualizzazione rete Hyper-V e consente di configurare la rete dell'infrastruttura per garantire che il routing di percorso dati End-to-End viene eseguito il provisioning. Questo processo consente di tenant di rete virtuale accessibile dal tenant Enterprise siti.  
  
Per il piano dati routing, i pacchetti che raggiungono le macchine virtuali Gateway RAS sono indirizzati direttamente alla rete virtuale del tenant, perché le route richieste sono ora disponibili con tutte le macchine virtuali Gateway RAS partecipanti.  
  
Analogamente, con i criteri di virtualizzazione rete Hyper-V in uso, la rete virtuale tenant indirizza i pacchetti direttamente alle macchine virtuali Gateway RAS (senza la necessità di conoscere il riflettore delle Route) e quindi ai siti aziendali attraverso i tunnel VPN site-to-site .  
  
Inoltre. il traffico di ritorno da rete virtuale tenant al sito aziendale tenant remoto consente di ignorare il SLBs, un processo noto come Direct Server Return (DSR).  
  
## <a name="bkmk_failover"></a>Modalità di risposta Controller di rete per Gateway RAS e il Failover di Reflector Route  
Di seguito sono due scenari di failover possibili: uno per i client di Reflector Route Gateway RAS e uno per riflettori delle Route Gateway RAS incluse informazioni su come Controller di rete gestisce il failover per le macchine virtuali nella configurazione.  
  
### <a name="vm-failure-of-a-ras-gateway-bgp-route-reflector-client"></a>Errore della macchina virtuale di un Client di Reflector Route BGP Gateway RAS  
Controller di rete esegue le azioni seguenti quando si verifica un errore di un client di Reflector Route Gateway RAS.  
  
> [!NOTE]  
> Quando una Route Reflector per infrastrutture di un tenant BGP non è un Gateway RAS, è un client di Reflector Route nell'infrastruttura BGP del tenant.  
  
-   Controller di rete consente di selezionare una macchina virtuale Gateway RAS standby disponibile ed esegue il provisioning la nuova macchina virtuale Gateway RAS con la configurazione della macchina virtuale Gateway RAS non riuscita.  
  
-   Controller di rete aggiorna la configurazione del bilanciamento del carico software corrispondente per assicurarsi che i tunnel VPN site-to-site dai siti tenant per il Gateway RAS non riusciti vengono stabiliti correttamente con il nuovo Gateway RAS.  
  
-   Controller di rete consente di configurare il client di Reflector Route BGP nel nuovo gateway.  
  
-   Controller di rete consente di configurare il nuovo client riflettore di Route BGP Gateway RAS come attiva. Il Gateway RAS avvia immediatamente il peering Route Reflector del tenant per condividere le informazioni di routing e di abilitare il peering eBGP per il sito aziendale corrispondente.  
  
### <a name="vm-failure-for-a-ras-gateway-bgp-route-reflector"></a>Errore della macchina virtuale per un riflettore delle Route BGP Gateway RAS  
Controller di rete esegue le azioni seguenti quando si verifica un errore di un riflettore di Route BGP Gateway RAS.  
  
-   Controller di rete consente di selezionare una macchina virtuale Gateway RAS standby disponibile ed esegue il provisioning la nuova macchina virtuale Gateway RAS con la configurazione della macchina virtuale Gateway RAS non riuscita.  
  
-   Controller di rete consente di configurare il riflettore delle Route nella nuova VM Gateway RAS e assegna la nuova macchina virtuale lo stesso indirizzo IP usato dalla macchina virtuale non riuscita, offrendo così l'integrità di route nonostante l'errore della macchina virtuale.  
  
-   Controller di rete aggiorna la configurazione del bilanciamento del carico software corrispondente per assicurarsi che i tunnel VPN site-to-site dai siti tenant per il Gateway RAS non riusciti vengono stabiliti correttamente con il nuovo Gateway RAS.  
  
-   Controller di rete consente di configurare il nuovo BGP Route Reflector macchina virtuale Gateway RAS come attiva.  
  
-   Il riflettore delle Route diventa immediatamente attiva. Stabilito il tunnel VPN site-to-site per l'azienda e il riflettore delle Route Usa il peering eBGP e scambia route con router Enterprise sito.  
  
-   Dopo la selezione delle route BGP, gli aggiornamenti del riflettore di Route BGP Gateway RAS i client di Reflector Route nel Data Center del tenant e sincronizza le route con Controller di rete, rendendo il percorso dati End-to-End disponibili per il traffico del tenant.  
  
## <a name="bkmk_advantages"></a>Vantaggi dell'utilizzo delle nuove funzionalità di Gateway RAS  
Di seguito sono alcuni dei vantaggi dell'uso di queste nuove funzionalità di Gateway RAS quando si progetta la distribuzione di Gateway RAS.  
  
**Scalabilità di Gateway RAS**  
  
Poiché è possibile aggiungere quante macchine virtuali Gateway RAS in base alle esigenze per i pool di Gateway RAS, è possibile aumentare facilmente la distribuzione di Gateway RAS per ottimizzare le prestazioni e capacità. Quando si aggiungono macchine virtuali a un pool, è possibile configurare questi gateway RAS con connessioni VPN site-to-site di qualsiasi tipo (IKEv2, L3 GRE), eliminando la capacità dei colli di bottiglia senza alcun tempo di inattività.  
  
**Gestione semplificata a livello aziendale del Gateway del sito**  
  
Quando il tenant dispone di più siti aziendali, i tenant possono configurare tutti i siti con un indirizzo di IP VPN site-to-site remoto e un indirizzo IP singolo adiacente remoto - Data Center CSP RAS Gateway BGP Route Reflector VIP per il tenant. Ciò semplifica la gestione del gateway per i tenant.  
  
**Monitoraggio e aggiornamento rapido di errore di Gateway**  
  
Per garantire una risposta rapida failover, è possibile configurare il tempo di parametro Keepalive BGP tra le route di edge e il router di controllo per un intervallo di tempo breve, ad esempio minore o uguale a 10 secondi. Con questo breve intervallo keep-alive, se si verifica un errore di un router perimetrale del protocollo BGP Gateway RAS, rapidamente viene rilevato l'errore e Controller di rete segue i passaggi descritti nelle sezioni precedenti. Questo vantaggio può ridurre la necessità di un protocollo di rilevamento errore separato, come protocollo di rilevamento di inoltro bidirezionale (BFD).  
  
 
  


