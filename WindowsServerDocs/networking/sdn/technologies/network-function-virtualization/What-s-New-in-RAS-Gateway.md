---
title: Novità di Gateway RAS
description: È possibile utilizzare questo argomento per informazioni sulle nuove funzionalità per Gateway RAS, che è basato su software, multi-tenant, i router in grado di protocollo BGP (Border Gateway) in Windows Server 2016.
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 709cb192-313a-47b5-954e-eb5f6fee51a7
ms.author: anpaul
author: AnirbanPaul
ms.openlocfilehash: 40d553ad5f41c68e8135c3d2c56ac8571d738e95
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859584"
---
# <a name="whats-new-in-ras-gateway"></a>Novità di Gateway RAS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni sulle nuove funzionalità per Gateway RAS, che è basato su software, multi-tenant, i router in grado di protocollo BGP (Border Gateway) in Windows Server 2016. Il router RAS Gateway multi-tenant BGP è progettato per il provider di servizi Cloud (CSP) e le aziende che ospitano più reti virtuali tenant mediante la virtualizzazione rete Hyper-V.  
  
> [!NOTE]  
> In Windows Server 2012 R2, Gateway RAS denominato Gateway RRAS. e in System Center Virtual Machine Manager, Gateway RAS denominato Gateway Windows Server.  
  
In questo argomento sono contenute le seguenti sezioni.  
  
-   [Opzioni di connettività da sito a sito](#bkmk_s2s)  
  
-   [Pool di gateway](#bkmk_pools)  
  
-   [Scalabilità del pool di gateway](#bkmk_gps)  
  
-   [Ridondanza del pool di gateway M + N](#bkmk_m)  
  
-   [Riflettore Route](#bkmk_rr)  
  
## <a name="site-to-site-connectivity-options"></a><a name="bkmk_s2s"></a>Opzioni di connettività da sito a sito  
Gateway RAS ora supporta tre tipi di connessioni VPN site-to-site: Internet Key Exchange versione 2 (IKEv2) tunnel rete privata virtuale (VPN) site-to-site VPN di livello 3 (L3) e Generic Routing Encapsulation (GRE).  
  
Per ulteriori informazioni su GRE, vedere [GRE Tunneling in Windows Server 2016](../../../../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md).  
  
## <a name="gateway-pools"></a><a name="bkmk_pools"></a>Pool di gateway  
In Windows Server 2016, è possibile creare pool di gateway di tipi diversi. Pool di gateway includono più istanze del Gateway RAS e instradare il traffico di rete tra reti fisiche e virtuali. Pool di gateway può eseguire una delle funzioni di gateway singoli - Internet Key Exchange versione 2 (IKEv2) tunnel rete privata virtuale (VPN) site-to-site VPN di livello 3 (L3) e Generic Routing Encapsulation (GRE) - o il pool può eseguire tutte queste funzioni e agire come un pool misto.  
  
È possibile creare pool di gateway utilizzando qualsiasi logica di programmazione in base ai requisiti di infrastruttura. Ad esempio, è possibile creare pool di gateway in base a una delle seguenti caratteristiche.  
  
-   Tipi di tunnel VPN IKEv2, VPN, L3 GRE VPN)  
  
-   Capacità  
  
-   Livello di ridondanza (affidabilità in base al piano di fatturazione per tenant)  
  
-   Separazione personalizzata per i clienti  
  
Per ulteriori informazioni, vedere [RAS Gateway un'elevata disponibilità](RAS-Gateway-High-Availability.md).  
  
## <a name="gateway-pool-scalability"></a><a name="bkmk_gps"></a>Scalabilità del pool di gateway  
È possibile scalare facilmente un pool di gateway verso l'alto o verso il basso aggiungendo o rimuovendo le macchine virtuali gateway nel pool. Rimozione o aggiunta di gateway non interrotti i servizi forniti da un pool. È inoltre possibile aggiungere e rimuovere l'intero pool di gateway.  
  
Per ulteriori informazioni, vedere [RAS Gateway un'elevata disponibilità](RAS-Gateway-High-Availability.md).  
  
## <a name="mn-gateway-pool-redundancy"></a><a name="bkmk_m"></a>Ridondanza del pool di gateway M + N  
Ogni pool di gateway è N + M ridondanti. Ciò significa che un sto ' numero di macchine virtuali gateway attive (VM) viene eseguito il backup da un numero n di macchine virtuali gateway standby. M + N ridondanza offre una maggiore flessibilità nella determinazione del livello di affidabilità che è necessario quando si distribuisce Gateway RAS. Anziché utilizzare un solo Gateway RAS standby per ogni macchina Virtuale Gateway RAS attive, che è l'unica opzione di configurazione di Windows Server 2012 R2 - è ora possibile configurare macchine virtuali standby il numero desiderato. La funzionalità di gestione del servizio Gateway di rete Controller utilizza in modo efficiente la capacità di macchina Virtuale Gateway RAS standby per assicurare il failover affidabile se un oggetto attivo RAS Gateway VM non riesce o perde la connettività.  
  
Per ulteriori informazioni, vedere [RAS Gateway un'elevata disponibilità](RAS-Gateway-High-Availability.md).  
  
## <a name="route-reflector"></a><a name="bkmk_rr"></a>Riflettore Route  
Il protocollo BGP (Border Gateway) Route Reflector è ora incluso con Gateway RAS e fornisce un'alternativa alla topologia a maglia completa BGP necessarie per la sincronizzazione di route tra i router. A maglia completa sincronizzazione, tutti i router BGP devono connettersi con tutti gli altri router nella topologia di routing. Quando si utilizza Route Reflector, tuttavia, il riflettore delle Route è l'unico router che si connette con tutti gli altri router, denominato BGP client, semplificando in questo modo la sincronizzazione di route e ridurre il traffico di rete. Il riflettore delle Route apprende tutte le route, route migliori calcola e ridistribuisce le route migliori ai propri client BGP.  
  
Con Windows Server 2016, è possibile configurare tunnel di accesso remoto del tenant singolo per terminare in più di una macchina Virtuale Gateway RAS. Questo fornisce maggiore flessibilità per i provider di servizi Cloud di fronte a circostanze in cui è possibile soddisfare una macchina Virtuale Gateway RAS tutti i requisiti di larghezza di banda delle connessioni tenant.  
  
Questa funzionalità, tuttavia, introduce complessità di gestione di route e sincronizzazione efficace di route tra siti remoti tenant e le risorse virtuali nel data center cloud. Fornire tenant con connessioni a più gateway RAS introduce inoltre una complessità aggiuntiva in configurazione alla fine dell'organizzazione, in ogni sito tenant disporranno di elementi adiacenti routing separati.  
  
Un riflettore Route BGP nel piano di controllo consente di risolvere questi problemi e in modo trasparente la distribuzione di infrastruttura interni CSP per i tenant dell'organizzazione. Di seguito sono riportati alcuni aspetti importanti sul riflettore delle Route BGP incluso con Gateway RAS e integrato con il Controller di rete.  
  
-   Una Route Reflector in una distribuzione con la rete definita dal Software è un'entità logica che si trova sul piano di controllo tra i gateway RAS e il Controller di rete. Non viene, tuttavia, utilizzato nel piano del routing dei dati.  
  
-   Quando si aggiunge un nuovo tenant al Data Center, Controller di rete configura automaticamente il primo tenant RAS Gateway come un riflettore di Route.  
  
-   Ogni tenant dispone di un riflettore Route corrispondente, e si trova su una delle macchine virtuali Gateway RAS associati a tale tenant.  
  
-   Un tenant Route Reflector agisce come il riflettore delle Route per tutte le macchine virtuali Gateway RAS associati al tenant. I gateway tenant diverso il riflettore delle Route Gateway RAS sono i client di Reflector Route. Il riflettore delle Route esegue la sincronizzazione di route tra tutti i client di Reflector Route in modo che il percorso dati effettivi routing può verificarsi.  
  
-   Una Route Reflector non fornisce servizi di reflector route per il Gateway RAS su cui è configurato.  
  
-   Una Route Reflector Aggiorna Controller di rete con le route Enterprise che corrispondono ai siti Enterprise del tenant. In questo modo il Controller di rete configurare i criteri di virtualizzazione rete Hyper-V richiesti nella rete virtuale tenant per l'accesso al percorso dati End-to-End.  
  
-   Se i clienti aziendali utilizzano Routing BGP nello spazio degli indirizzi cliente, il riflettore delle Route Gateway RAS è il solo esterno vicino BGP (eBGP) per tutti i siti del tenant corrispondente. Questo vale indipendentemente dal punti di interruzione tunnel del tenant dell'organizzazione. In altre parole, indipendentemente da quale VM Gateway RAS in CSP datacenter termina il tunnel VPN site-to-site per un sito tenant, il Peer eBGP per tutti i siti tenant è il riflettore delle Route.  
  
Per ulteriori informazioni, vedere [architettura di distribuzione di Gateway RAS](RAS-Gateway-Deployment-Architecture.md) e la richiesta di Internet Engineering Task Force (IETF) per argomento commenti [RFC 4456 BGP Route Reflection: un'alternativa a Full Mesh interno BGP (IBGP)](https://tools.ietf.org/html/rfc4456).  
  

