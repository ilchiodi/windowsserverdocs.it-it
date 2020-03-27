---
title: Virtualizzazione delle funzioni di rete
description: È possibile usare questo argomento per informazioni sulla virtualizzazione delle funzioni di rete, che consente di distribuire appliance di rete virtuali come il firewall del centro dati, il gateway RAS multi-tenant e il bilanciamento del carico software (SLB) in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 79df3bbe-48fd-4eff-8df6-35f6317566f3
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 1d5d5aaae5983e062dae203c60a7001f36e5629b
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309815"
---
# <a name="network-function-virtualization"></a>Virtualizzazione delle funzioni di rete

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile usare questo argomento per informazioni sulla virtualizzazione delle funzioni di rete, che consente di distribuire appliance di rete virtuale, ad esempio firewall del centro dati, gateway RAS multi-tenant e bilanciamento del carico software \(SLB\) multiplexer \(MUX\).
  
>[!NOTE]  
>Oltre a questo argomento, è disponibile la seguente documentazione relativa alla virtualizzazione delle funzioni di rete.  
> - [Panoramica del firewall del data center](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)  
> - [Gateway RAS per SDN](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)  
> - [Bilanciamento del carico software (SLB) per SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)  
  
Nei data center definiti dal software odierno, le funzioni di rete eseguite da appliance hardware (ad esempio i bilanciamenti del carico, i firewall, i router, i commutatori e così via) vengono virtualizzate sempre più come appliance virtuali. Questo "virtualizzazione delle funzioni di rete" è una progressione naturale di virtualizzazione di server e virtualizzazione di rete. I dispositivi virtuali sono in rapida emergenza e creano un nuovo mercato. Continuano a generare interesse e ottenere l'espansione di entrambe le piattaforme di virtualizzazione e i servizi cloud.  
  
Microsoft ha incluso un gateway autonomo come appliance virtuale a partire da Windows Server 2012 R2. Per ulteriori informazioni, vedere [Windows Server Gateway](https://technet.microsoft.com/library/dn313101.aspx). Con Windows Server 2016 Microsoft continua ad espandersi e investire nel mercato della virtualizzazione delle funzioni di rete.  
  
## <a name="virtual-appliance-benefits"></a>Vantaggi dell'appliance virtuale  
Un appliance virtuale è dinamico e facile da modificare perché si tratta di una macchina virtuale predefinita e personalizzata. Può trattarsi di una o più macchine virtuali in pacchetto, aggiornate e gestite come unità. Insieme a SDN (Software Defined Networking), avrai la flessibilità e la flessibilità necessarie nell'infrastruttura di oggi basata sul cloud. Ad esempio,  
  
-   SDN presenta la rete come risorsa dinamica e in pool.  
  
-   SDN facilita l'isolamento del tenant.  
  
-   SDN ottimizza la scalabilità e le prestazioni.  
  
-   I dispositivi virtuali consentono una facile espansione della capacità e la mobilità del carico di lavoro.  
  
-   Le appliance virtuali riducono la complessità operativa.  
  
-   I dispositivi virtuali consentono ai clienti di acquisire, distribuire e gestire facilmente soluzioni preintegrate.  
  
    -   I clienti possono spostare facilmente l'appliance virtuale ovunque nel cloud.  
  
    -   I clienti possono ridimensionare le appliance virtuali in modo dinamico in base alle esigenze.  
  
Per ulteriori informazioni su Microsoft SDN, vedere [Software Defined Networking](https://technet.microsoft.com/windows-server-docs/networking/sdn/software-defined-networking--sdn-).  
  
### <a name="what-network-functions-are-being-virtualized"></a>Quali funzioni di rete vengono virtualizzate?  
Il Marketplace per le funzioni di rete virtualizzate sta crescendo rapidamente. È in corso la virtualizzazione delle funzioni di rete seguenti:  
  
-   **Sicurezza**  
  
    -   Firewall  
  
    -   Antivirus  
  
    -   DDoS (Distributed Denial of Service)  
  
    -   Indirizzi IP/ID (sistema di prevenzione delle intrusioni/sistema di rilevamento delle intrusioni)  
  
-   **Applicazioni/utilità di ottimizzazione WAN**  
  
-   **Bordo**  
  
    -   Gateway da sito a sito  
  
    -   Gateway L3  
  
    -   Router  
  
    -   Switch  
  
    -   NAT  
  
    -   Bilanciamento del carico (non necessariamente al perimetro)  
  
    -   Proxy HTTP  
  
## <a name="why-microsoft-is-a-great-platform-for-virtual-appliances"></a>Perché Microsoft è un'ottima piattaforma per appliance virtuali  
![Stack di rete virtuale](../../../media/Network-Function-Virtualization/Microsoft-Network-Function-Virtualization.png)  
  
La piattaforma Microsoft è stata progettata per essere un'ottima piattaforma per la creazione e la distribuzione di appliance virtuali. Ecco perché:  
  
-   Microsoft fornisce le principali funzioni di rete virtualizzate con Windows Server 2016.  
  
-   È possibile distribuire un'appliance virtuale dal fornitore di propria scelta.  
  
-   È possibile distribuire, configurare e gestire le appliance virtuali con il controller di rete Microsoft incluso in Windows Server 2016. Per ulteriori informazioni sul controller di rete, vedere [controller di rete](../../../sdn/technologies/network-controller/Network-Controller.md).  
  
-   Hyper-V può ospitare i principali sistemi operativi guest necessari.  
  
## <a name="network-function-virtualization-in-windows-server-2016"></a>Virtualizzazione delle funzioni di rete in Windows Server 2016  
  
### <a name="virtual-appliances-functions-provided-by-microsoft"></a>Funzioni appliance virtuali fornite da Microsoft  
Con Windows Server 2016 sono disponibili le appliance virtuali seguenti:  
  
**Bilanciamento del carico software**  
  
Un servizio di bilanciamento del carico di livello 4 che opera sulla scala del Data Center. Si tratta di una versione simile del servizio di bilanciamento del carico di Azure distribuito su vasta scala nell'ambiente Azure. Per ulteriori informazioni sul Load Balancer software Microsoft, vedere [bilanciamento del carico software (SLB) per Sdn](https://technet.microsoft.com/library/mt632286.aspx). Per ulteriori informazioni sui servizi di bilanciamento del carico di Microsoft Azure, vedere [Microsoft Azure servizi di bilanciamento del carico](https://azure.microsoft.com/blog/2014/04/08/microsoft-azure-load-balancing-services/).  
  
**Gateway**. Il gateway RAS fornisce tutte le combinazioni delle funzioni del gateway seguenti.  
  
-   **Gateway da sito a sito**  
  
    Il gateway RAS fornisce un gateway multi-tenant con supporto per Border Gateway Protocol (BGP) che consente ai tenant di accedere alle proprie risorse e gestirle tramite connessioni VPN da sito a sito da siti remoti e che consente il flusso del traffico di rete tra le risorse virtuali nelle reti fisiche del tenant e del cloud. Per altre informazioni sul gateway RAS, vedere disponibilità elevata [e gateway RAS](https://technet.microsoft.com/library/mt626650.aspx)del [gateway RAS](https://technet.microsoft.com/library/mt631692.aspx) .  
  
-   **Gateway di inoltri**  
  
    Il gateway RAS instrada il traffico tra le reti virtuali e la rete fisica del provider di hosting. Se, ad esempio, i tenant creano una o più reti virtuali e necessitano dell'accesso alle risorse condivise nella rete fisica del provider di hosting, il gateway di inoltro può instradare il traffico tra la rete virtuale e la rete fisica per consentire agli utenti di lavorare rete virtuale con i servizi necessari. Per altre informazioni, vedere la pagina relativa alla disponibilità elevata e al [gateway RAS](https://technet.microsoft.com/library/mt626650.aspx)del [gateway RAS](https://technet.microsoft.com/library/mt631692.aspx) .  
  
-   **Gateway Tunnel GRE**  
  
    I tunnel basati su GRE consentono la connettività tra reti virtuali tenant e reti esterne. Poiché il protocollo GRE è leggero e il supporto per GRE è disponibile nella maggior parte dei dispositivi di rete, diventa la scelta ideale per il tunneling in cui la crittografia dei dati non è necessaria. Il supporto di GRE nei tunnel da sito a sito (S2S) risolve il problema dell'inoltro tra reti virtuali tenant e reti esterne tenant tramite un gateway multi-tenant. Per ulteriori informazioni sui tunnel GRE, vedere la pagina relativa [al tunneling GRE in Windows Server 2016](https://technet.microsoft.com/library/dn765485.aspx).  
  
**Piano di controllo di routing con BGP**  
  
Il controllo di routing di virtualizzazione rete Hyper-V (HNV) è l'entità logica e centralizzata nel piano di controllo, che contiene tutte le route del piano di indirizzi del cliente e apprende dinamicamente e quindi aggiorna i router del gateway RAS distribuiti nella rete virtuale. Per altre informazioni, vedere la pagina relativa alla disponibilità elevata e al [gateway RAS](https://technet.microsoft.com/library/mt626650.aspx)del [gateway RAS](https://technet.microsoft.com/library/mt631692.aspx) .  
  
**Firewall multi-tenant distribuito**  
  
Il firewall protegge il livello di rete delle reti virtuali. I criteri vengono applicati alla porta SDN-vSwitch di ogni macchina virtuale tenant. Protegge tutti i flussi di traffico: est-ovest e nord-sud. Il push dei criteri viene effettuato tramite il portale tenant e il controller di rete li distribuisce a tutti gli host applicabili. Per ulteriori informazioni sul firewall multi-tenant distribuito, vedere [Cenni preliminari sul firewall di datacenter](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md).  
  


