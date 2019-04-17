---
title: What's New in Gateway RAS
description: È possibile utilizzare questo argomento per informazioni sulle nuove funzionalità per Gateway RAS, che è basato su software, multi-tenant, il router in grado di protocollo BGP (Border Gateway) in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 709cb192-313a-47b5-954e-eb5f6fee51a7
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1a9ac762f6cd80d3889cf72478b7a7f8ce9e5cb7
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-ras-gateway"></a>What's New in Gateway RAS

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per informazioni sulle nuove funzionalità per Gateway RAS, che è basato su software, multi-tenant, il router in grado di protocollo BGP (Border Gateway) in Windows Server 2016. Il router RAS Gateway multi-tenant BGP è progettato per il provider di servizi Cloud (CSP) e le aziende che ospitano più reti virtuali tenant tramite virtualizzazione rete Hyper-V.  
  
> [!NOTE]  
> In Windows Server 2012 R2, Gateway RAS denominato Gateway RRAS. e in System Center Virtual Machine Manager, Gateway RAS denominato Gateway Windows Server.  
  
In questo argomento contiene le sezioni seguenti.  
  
-   [Opzioni di connettività Site-to-site](#bkmk_s2s)  
  
-   [Pool di gateway](#bkmk_pools)  
  
-   [Scalabilità dei Pool di gateway](#bkmk_gps)  
  
-   [Ridondanza dei Pool di Gateway M + N](#bkmk_m)  
  
-   [Riflettore delle route](#bkmk_rr)  
  
## <a name="bkmk_s2s"></a>Opzioni di connettività Site-to-site  
Gateway RAS ora supporta tre tipi di connessioni di VPN site-to-site: Internet Key Exchange versione 2 (IKEv2) tunnel rete privata virtuale (VPN) site-to-site VPN di livello 3 (L3) e Generic Routing Encapsulation (GRE).  
  
Per ulteriori informazioni su GRE, vedere [GRE Tunneling in Windows Server 2016](../../../../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md).  
  
## <a name="bkmk_pools"></a>Pool di gateway  
In Windows Server 2016, è possibile creare pool di gateway di tipi diversi. Pool di gateway includono più istanze del Gateway RAS e instradare il traffico di rete tra reti fisiche e virtuali. Pool di gateway può eseguire una delle funzioni di gateway singoli - Internet Key Exchange versione 2 (IKEv2) tunnel rete privata virtuale (VPN) site-to-site VPN di livello 3 (L3) e Generic Routing Encapsulation (GRE) - o il pool può eseguire tutte queste funzioni e agire come un pool misto.  
  
È possibile creare pool di gateway utilizzando qualsiasi logica che preferisci in base ai requisiti di infrastruttura. Ad esempio, è possibile creare pool di gateway in base a una delle seguenti caratteristiche.  
  
-   Tipi di tunnel VPN IKEv2, VPN, L3 GRE VPN)  
  
-   Capacità  
  
-   Livello di ridondanza (affidabilità in base al piano di fatturazione per tenant)  
  
-   Separazione personalizzata per i clienti  
  
Per ulteriori informazioni, vedere [disponibilità elevata del Gateway RAS](RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_gps"></a>Scalabilità dei Pool di gateway  
È possibile scalare facilmente un pool di gateway verso l'alto o verso il basso aggiungendo o rimuovendo le macchine virtuali gateway nel pool. Rimozione o aggiunta di gateway non interrotti i servizi forniti da un pool. Puoi anche aggiungere e rimuovere l'intero pool di gateway.  
  
Per ulteriori informazioni, vedere [disponibilità elevata del Gateway RAS](RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_m"></a>Ridondanza dei Pool di Gateway M + N  
Ogni pool di gateway è N + M ridondanti. Questo significa che un sto ' numero di macchine virtuali gateway attive (VM) viene eseguito il backup da un numero n di macchine virtuali gateway standby. M + N ridondanza offre una maggiore flessibilità nella determinazione del livello di affidabilità che è necessario quando si distribuisce Gateway RAS. Invece di usare un solo Gateway RAS standby per ogni macchina virtuale Gateway RAS attive, che è l'unica opzione di configurazione con Windows Server 2012 R2 - è ora possibile configurare macchine virtuali standby desiderato. La funzionalità di gestione del servizio Gateway di rete Controller utilizza in modo efficiente la capacità di macchina virtuale Gateway RAS standby per assicurare il failover affidabile se un oggetto attivo RAS Gateway VM non riesce o perde la connettività.  
  
Per ulteriori informazioni, vedere [disponibilità elevata del Gateway RAS](RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_rr"></a>Riflettore delle route  
Il protocollo BGP (Border Gateway) Route Reflector è ora incluso con Gateway RAS e fornisce un'alternativa alla topologia a maglia completa BGP necessarie per la sincronizzazione di route tra i router. A maglia completa sincronizzazione, tutti i router BGP devono connettersi con tutti gli altri router nella topologia di routing. Quando si utilizza Route Reflector, tuttavia, il riflettore delle Route è l'unico router che si connette con tutti gli altri router, denominati BGP client, semplificando in questo modo la sincronizzazione di route e ridurre il traffico di rete. Il riflettore delle Route apprende tutte le route, route migliori calcola e ridistribuisce le route migliori ai propri client BGP.  
  
Con Windows Server 2016, è possibile configurare tunnel di accesso remoto del tenant singolo per terminare in più di una macchina virtuale Gateway RAS. Questo fornisce maggiore flessibilità per provider di servizi Cloud di fronte a circostanze in cui è possibile soddisfare una macchina virtuale Gateway RAS tutti i requisiti di larghezza di banda delle connessioni tenant.  
  
Questa funzionalità, tuttavia, introduce complessità di gestione di route e sincronizzazione efficace di route tra siti remoti tenant e le risorse virtuali nel data center cloud. Fornire tenant con connessioni a più gateway RAS introduce anche complessità aggiuntiva in configurazione alla fine dell'organizzazione, in cui ogni sito tenant disporranno elementi adiacenti routing separati.  
  
Un riflettore di Route BGP nel piano di controllo risolvere questi problemi e rende trasparente la distribuzione di infrastruttura interni CSP per i tenant dell'organizzazione. Ecco alcuni aspetti importanti sul riflettore delle Route BGP incluso con Gateway RAS e integrato con Controller di rete.  
  
-   Una Route Reflector in una distribuzione di Software Defined Networking è un'entità logica che si trova sul piano di controllo tra i gateway RAS e il Controller di rete. Non è, tuttavia, coinvolto in routing piano dati.  
  
-   Quando si aggiunge un nuovo tenant al Data Center, Controller di rete configura automaticamente il primo tenant RAS Gateway come un riflettore Route.  
  
-   Ogni tenant dispone di un riflettore Route corrispondente, e si trova su una delle macchine virtuali Gateway RAS associati a tale tenant.  
  
-   Un tenant Route Reflector agisce come il riflettore delle Route per tutte le macchine virtuali Gateway RAS associati al tenant. I gateway tenant diverso il riflettore delle Route Gateway RAS sono i client di Reflector Route. Il riflettore delle Route esegue la sincronizzazione di route tra tutti i client di Reflector Route in modo che il percorso dati effettivi routing può verificarsi.  
  
-   Una Route Reflector non fornisce servizi di reflector route per il Gateway RAS su cui è configurato.  
  
-   Una Route Reflector Aggiorna Controller di rete con le route Enterprise che corrispondono ai siti Enterprise del tenant. In questo modo il Controller di rete configurare i criteri di virtualizzazione rete Hyper-V necessari nella rete virtuale tenant per l'accesso al percorso dati End-to-End.  
  
-   Se i clienti aziendali utilizzano Routing BGP nello spazio degli indirizzi cliente, il riflettore delle Route Gateway RAS è il solo esterno vicino BGP (eBGP) per tutti i siti del tenant corrispondente. Questo vale indipendentemente dal punti di interruzione tunnel del tenant dell'organizzazione. In altre parole, indipendentemente da quale VM Gateway RAS in CSP datacenter termina il tunnel VPN site-to-site per un sito tenant, il Peer eBGP per tutti i siti tenant è il riflettore delle Route.  
  
Per ulteriori informazioni, vedere [architettura di distribuzione di Gateway RAS](RAS-Gateway-Deployment-Architecture.md) e la richiesta di Internet Engineering Task Force (IETF) per argomento commenti [RFC 4456 BGP Route Reflection: un'alternativa a Full Mesh interno BGP (IBGP)](https://tools.ietf.org/html/rfc4456).  
  

