---
title: Architettura della distribuzione di Gateway RAS
description: È possibile usare questo argomento per informazioni sulla distribuzione del provider di servizi cloud (CSP) di gateway RAS in Windows Server 2016, inclusi i pool di gateway RAS, i riflettori di route e la distribuzione di più gateway per i singoli tenant.
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: d46e4e91-ece0-41da-a812-af8ab153edc4
ms.author: anpaul
author: AnirbanPaul
ms.openlocfilehash: 1704cd6d933af1e796a9ac1237ff2de3a49fb3d3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859604"
---
# <a name="ras-gateway-deployment-architecture"></a>Architettura della distribuzione di Gateway RAS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile usare questo argomento per informazioni sulla distribuzione del provider di servizi cloud (CSP) del gateway RAS, inclusi i pool di gateway RAS, i riflettori di route e la distribuzione di più gateway per i singoli tenant.  
  
Le sezioni seguenti forniscono brevi panoramiche su alcune delle nuove funzionalità del gateway RAS, in modo da poter comprendere come usare queste funzionalità nella progettazione della distribuzione del gateway.  
  
Viene inoltre fornita una distribuzione di esempio, incluse le informazioni sul processo di aggiunta di nuovi tenant, la sincronizzazione delle route e il routing del piano dati, il failover del gateway e del riflettore delle route e altro ancora.  
  
In questo argomento sono contenute le seguenti sezioni.  
  
-   [Uso delle nuove funzionalità del gateway RAS per la progettazione della distribuzione](#bkmk_new)  
  
-   [Distribuzione di esempio](#bkmk_example)  
  
-   [Aggiunta di nuovi tenant e spazio degli indirizzi del cliente (CA) EBGP peering](#bkmk_tenant)  
  
-   [Routing della sincronizzazione e del piano dati](#bkmk_route)  
  
-   [Come il controller di rete risponde al failover del gateway RAS e del riflettore delle route](#bkmk_failover)  
  
-   [Vantaggi dell'utilizzo delle nuove funzionalità del gateway RAS](#bkmk_advantages)  
  
## <a name="using-ras-gateway-new-features-to-design-your--deployment"></a><a name="bkmk_new"></a>Uso delle nuove funzionalità del gateway RAS per la progettazione della distribuzione  
Il gateway RAS include più nuove funzionalità che modificano e migliorano il modo in cui si distribuisce l'infrastruttura del gateway nel Data Center.  
  
### <a name="bgp-route-reflector"></a>Riflettore Route BGP  
La funzionalità di Reflection Route Border Gateway Protocol (BGP) è ora inclusa nel gateway RAS e fornisce un'alternativa alla topologia full mesh BGP normalmente necessaria per la sincronizzazione delle route tra i router. A maglia completa sincronizzazione, tutti i router BGP devono connettersi con tutti gli altri router nella topologia di routing. Quando si usa la route reflector, tuttavia, il riflettore delle route è l'unico router che si connette con tutti gli altri router, detti client di Reflection Route BGP, semplificando in tal modo la sincronizzazione delle route e la riduzione del traffico di rete. Il riflettore delle Route apprende tutte le route, route migliori calcola e ridistribuisce le route migliori ai propri client BGP.  
  
Per ulteriori informazioni, vedere Novità [del gateway RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md).  
  
### <a name="gateway-pools"></a><a name="bkmk_pools"></a>Pool di gateway  
In Windows Server 2016 è possibile creare molti pool di gateway di tipi diversi. Pool di gateway includono più istanze del Gateway RAS e instradare il traffico di rete tra reti fisiche e virtuali.  
  
Per ulteriori informazioni, vedere Novità [di gateway RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) e [disponibilità elevata del gateway RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
### <a name="gateway-pool-scalability"></a><a name="bkmk_gps"></a>Scalabilità del pool di gateway  
È possibile scalare facilmente un pool di gateway verso l'alto o verso il basso aggiungendo o rimuovendo le macchine virtuali gateway nel pool. Rimozione o aggiunta di gateway non interrotti i servizi forniti da un pool. È inoltre possibile aggiungere e rimuovere l'intero pool di gateway.  
  
Per ulteriori informazioni, vedere Novità [di gateway RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) e [disponibilità elevata del gateway RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
### <a name="mn-gateway-pool-redundancy"></a><a name="bkmk_m"></a>Ridondanza del pool di gateway M + N  
Ogni pool di gateway è N + M ridondanti. Ciò significa che viene eseguito il backup di un numero di VM gateway attive da un numerò n'di macchine virtuali del gateway di standby. M + N ridondanza offre una maggiore flessibilità nella determinazione del livello di affidabilità che è necessario quando si distribuisce Gateway RAS.  
  
Per ulteriori informazioni, vedere Novità [di gateway RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) e [disponibilità elevata del gateway RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
## <a name="example-deployment"></a><a name="bkmk_example"></a>Distribuzione di esempio  
La figura seguente illustra un esempio con il peering eBGP per le connessioni VPN da sito a sito configurate tra due tenant, Contoso e Woodgrove e il Data Center di Fabrikam CSP.  
  
![peering eBGP su VPN da sito a sito](../../../media/RAS-Gateway-Deployment-Architecture/ras_gateway_architecture.png)  
  
In questo esempio contoso richiede una larghezza di banda del gateway aggiuntiva, causando la decisione di progettazione dell'infrastruttura gateway di terminare il sito di Contoso Los Angeles su GW3 anziché GW2. Per questo motivo, le connessioni VPN di Contoso da siti diversi vengono terminate nel data center CSP su due gateway diversi.  
  
Entrambi i gateway, GW2 e GW3, sono stati i primi gateway RAS configurati dal controller di rete quando il CSP ha aggiunto i tenant Contoso e Woodgrove alla propria infrastruttura. Per questo motivo, questi due gateway sono configurati come riflettori di route per i clienti o i tenant corrispondenti. GW2 è il riflettore della route contoso e GW3 è il riflettore della route Woodgrove, oltre a essere il punto di terminazione del gateway RAS CSP per la connessione VPN con il sito di Contoso Los Angeles HQ.  
  
> [!NOTE]  
> Un gateway RAS può instradare il traffico di rete virtuale e fisico per un massimo di 100 tenant diversi, in base ai requisiti di larghezza di banda di ogni tenant.  
  
Come riflettori di route, GW2 Invia le route dello spazio della CA contoso al controller di rete e GW3 Invia le route dello spazio della CA Woodgrove al controller di rete.  
  
Il controller di rete esegue il push dei criteri di virtualizzazione rete Hyper-V alle reti virtuali Contoso e Woodgrove, oltre ai criteri RAS ai gateway RAS e ai criteri di bilanciamento del carico per i multiplexer (MUX) configurati come bilanciamento del carico software piscina.  
  
## <a name="adding-new-tenants-and-customer-address-ca-space-ebgp-peering"></a><a name="bkmk_tenant"></a>Aggiunta di nuovi tenant e spazio degli indirizzi del cliente (CA) eBGP peering  
Quando si firma un nuovo cliente e si aggiunge il cliente come nuovo tenant nel Data Center, è possibile usare il processo seguente, molti dei quali vengono eseguiti automaticamente dai router eBGP del controller di rete e del gateway RAS.  
  
1.  Effettuare il provisioning di una nuova rete virtuale e dei carichi di lavoro in base ai requisiti del tenant.  
  
2.  Se necessario, configurare la connettività remota tra il sito Enterprise tenant remoto e la relativa rete virtuale nel Data Center. Quando si distribuisce una connessione VPN da sito a sito per il tenant, il controller di rete seleziona automaticamente una macchina virtuale del gateway RAS disponibile dal pool di gateway disponibile e configura la connessione.  
  
3.  Durante la configurazione della VM del gateway RAS per il nuovo tenant, il controller di rete configura anche il gateway RAS come router BGP e lo designa come il riflettore delle route per il tenant. Questo vale anche nei casi in cui il gateway RAS funge da gateway, o come un riflettore del gateway e della route, per gli altri tenant.  
  
4.  A seconda del fatto che il routing dello spazio CA sia configurato per l'uso di reti configurate in modo statico o di routing BGP dinamico, il controller di rete configura le route statiche corrispondenti, i vicini BGP o entrambi nella macchina virtuale del gateway RAS e nel riflettore delle route.  
  
    > [!NOTE]  
    > -   Dopo che il controller di rete ha configurato un gateway RAS e un riflettore route per il tenant, ogni volta che lo stesso tenant richiede una nuova connessione VPN da sito a sito, il controller di rete verifica la capacità disponibile in questa VM del gateway RAS. Se il gateway originale può servire la capacità richiesta, la nuova connessione di rete viene configurata anche nella stessa VM del gateway RAS. Se la macchina virtuale del gateway RAS non è in grado di gestire la capacità aggiuntiva, il controller di rete seleziona una nuova macchina virtuale del gateway RAS disponibile e configura la nuova connessione. Questa nuova VM del gateway RAS associata al tenant diventa il client di route reflector del riflettore della route del gateway RAS del tenant originale.  
    > -   Poiché i pool di gateway RAS sono dietro i bilanciamento del carico software (SLBs), gli indirizzi VPN da sito a sito dei tenant usano ciascuno un solo indirizzo IP pubblico, definito indirizzo IP virtuale (VIP), che viene convertito dal SLBs in un indirizzo IP interno del Data Center, denominato Indirizzo IP dinamico (DIP), per un gateway RAS che instrada il traffico per il tenant dell'organizzazione. Questo mapping di indirizzi IP da pubblico a privato di SLB garantisce che i tunnel VPN da sito a sito siano stabiliti correttamente tra i siti aziendali e i gateway RAS CSP e i riflettori di route.  
    >   
    >     Per ulteriori informazioni su SLB, VIP e DIP, vedere [bilanciamento del carico software &#40;SLB&#41; per Sdn](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).  
  
5.  Dopo aver stabilito il tunnel VPN da sito a sito tra il sito aziendale e il gateway RAS del data center CSP per il nuovo tenant, viene effettuato automaticamente il provisioning delle route statiche associate ai tunnel nei lati Enterprise e CSP del tunnel .  
  
6.  Con il routing BGP dello spazio CA, viene stabilito anche il peering eBGP tra i siti aziendali e il riflettore della route del gateway RAS CSP.  
  
## <a name="route-synchronization-and-data-plane-routing"></a><a name="bkmk_route"></a>Routing della sincronizzazione e del piano dati  
Dopo aver stabilito il peering eBGP tra i siti aziendali e il riflettore delle route del gateway RAS CSP, il riflettore delle route apprende tutte le route aziendali usando il routing BGP dinamico. Il riflettore delle route Sincronizza tali route tra tutti i client del riflettore delle route in modo che siano tutti configurati con lo stesso set di route.  
  
Route Reflector aggiorna anche queste route consolidate, usando la sincronizzazione della route, al controller di rete. Il controller di rete converte quindi le route nei criteri di virtualizzazione rete Hyper-V e configura la rete dell'infrastruttura per assicurarsi che venga eseguito il provisioning del routing del percorso dati end-to-end. Questo processo rende la rete virtuale tenant accessibile dai siti aziendali tenant.  
  
Per il routing del piano dati, i pacchetti che raggiungono le macchine virtuali del gateway RAS vengono indirizzati direttamente alla rete virtuale del tenant, perché le route necessarie sono ora disponibili con tutte le VM del gateway RAS partecipanti.  
  
Analogamente, con i criteri di virtualizzazione rete Hyper-V, la rete virtuale tenant instrada i pacchetti direttamente alle VM del gateway RAS (senza che sia necessario conoscere il riflettore delle route) e quindi ai siti aziendali tramite i tunnel VPN da sito a sito .  
  
Inoltre. il traffico restituito dalla rete virtuale tenant al sito aziendale del tenant remoto ignora SLBs, un processo chiamato Direct Server Return (DSR).  
  
## <a name="how-network-controller-responds-to-ras-gateway-and-route-reflector-failover"></a><a name="bkmk_failover"></a>Come il controller di rete risponde al failover del gateway RAS e del riflettore delle route  
Di seguito sono riportati due possibili scenari di failover: uno per i client di reflection del gateway RAS e uno per i riflettori della route del gateway RAS, incluse informazioni sul modo in cui il controller di rete gestisce il failover per le macchine virtuali in entrambe le configurazioni  
  
### <a name="vm-failure-of-a-ras-gateway-bgp-route-reflector-client"></a>Errore della macchina virtuale di un client di Reflection Route BGP del gateway RAS  
Il controller di rete esegue le azioni seguenti quando un client del riflettore della route del gateway RAS non riesce.  
  
> [!NOTE]  
> Quando un gateway RAS non è un riflettore delle route per l'infrastruttura BGP di un tenant, si tratta di un client di route reflector nell'infrastruttura BGP del tenant.  
  
-   Il controller di rete seleziona una VM del gateway RAS in standby disponibile ed effettua il provisioning della nuova VM del gateway RAS con la configurazione della VM del gateway RAS non riuscita.  
  
-   Il controller di rete aggiorna la configurazione SLB corrispondente per assicurarsi che i tunnel VPN da sito a sito dai siti tenant al gateway RAS non riuscito siano stabiliti correttamente con il nuovo gateway RAS.  
  
-   Il controller di rete configura il client di Reflector Route BGP nel nuovo gateway.  
  
-   Il controller di rete configura il nuovo client del riflettore della route BGP del gateway RAS come attivo. Il gateway RAS avvia immediatamente il peering con il riflettore della route del tenant per condividere le informazioni di routing e abilitare il peering eBGP per il sito aziendale corrispondente.  
  
### <a name="vm-failure-for-a-ras-gateway-bgp-route-reflector"></a>Errore della macchina virtuale per un riflettore della route BGP del gateway RAS  
Il controller di rete esegue le azioni seguenti quando un riflettore della route BGP del gateway RAS non riesce.  
  
-   Il controller di rete seleziona una VM del gateway RAS in standby disponibile ed effettua il provisioning della nuova VM del gateway RAS con la configurazione della VM del gateway RAS non riuscita.  
  
-   Il controller di rete configura il riflettore delle route nella nuova VM del gateway RAS e assegna la nuova VM allo stesso indirizzo IP usato dalla macchina virtuale non riuscita, fornendo in tal modo l'integrità della route nonostante l'errore della macchina virtuale.  
  
-   Il controller di rete aggiorna la configurazione SLB corrispondente per assicurarsi che i tunnel VPN da sito a sito dai siti tenant al gateway RAS non riuscito siano stabiliti correttamente con il nuovo gateway RAS.  
  
-   Il controller di rete configura la nuova VM del riflettore della route BGP del gateway RAS come attiva.  
  
-   Il riflettore della route diventa immediatamente attivo. Viene stabilito il tunnel VPN da sito a sito per l'azienda e il riflettore delle route usa il peering di eBGP e scambia le route con i router di sito aziendali.  
  
-   Dopo la selezione della route BGP, il riflettore della route BGP del gateway RAS aggiorna i client del riflettore della route tenant nel Data Center e sincronizza le route con il controller di rete, rendendo disponibile il percorso dei dati end-to-end per il traffico del tenant.  
  
## <a name="advantages-of-using-new-ras-gateway-features"></a><a name="bkmk_advantages"></a>Vantaggi dell'utilizzo delle nuove funzionalità del gateway RAS  
Di seguito sono riportati alcuni dei vantaggi derivanti dall'utilizzo di queste nuove funzionalità del gateway RAS durante la progettazione della distribuzione del gateway RAS.  
  
**Scalabilità del gateway RAS**  
  
Poiché è possibile aggiungere tutte le VM gateway RAS necessarie per i pool di gateway RAS, è possibile ridimensionare facilmente la distribuzione del gateway RAS per ottimizzare le prestazioni e la capacità. Quando si aggiungono macchine virtuali a un pool, è possibile configurare questi gateway RAS con connessioni VPN da sito a sito di qualsiasi tipo (IKEv2, L3, GRE), eliminando i colli di bottiglia della capacità senza tempi di inattività.  
  
**Gestione semplificata del gateway di sito aziendale**  
  
Quando il tenant dispone di più siti aziendali, il tenant può configurare tutti i siti con un indirizzo IP VPN da sito a sito remoto e un singolo indirizzo IP remoto remoto: l'indirizzo VIP del riflettore delle route BGP del gateway RAS del datacenter per il tenant. Ciò semplifica la gestione del gateway per i tenant.  
  
**Correzione rapida degli errori del gateway**  
  
Per garantire una risposta di failover veloce, è possibile configurare l'ora del parametro KeepAlive di BGP tra le route Edge e il router di controllo per un breve intervallo di tempo, ad esempio minore o uguale a 10 secondi. Con questo breve intervallo keep-alive, in caso di errore di un router Edge BGP del gateway RAS, l'errore viene rilevato rapidamente e il controller di rete segue i passaggi descritti nelle sezioni precedenti. Questo vantaggio potrebbe ridurre la necessità di un protocollo di rilevamento degli errori separato, ad esempio il protocollo BFD (Bidirectional inoltr Detection).  
  
 
  


