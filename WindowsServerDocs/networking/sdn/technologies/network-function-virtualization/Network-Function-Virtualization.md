---
title: Virtualizzazione delle funzioni di rete
description: È possibile utilizzare questo argomento per informazioni su virtualizzazione delle funzioni di rete, che consente di distribuire le Appliance virtuali di rete, ad esempio Firewall del centro dati, Gateway RAS multi-tenant e il bilanciamento del carico Software (SLB) in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 79df3bbe-48fd-4eff-8df6-35f6317566f3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 59474a13d1cbce6a607f025caf3f6c1b839c7eed
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884552"
---
# <a name="network-function-virtualization"></a>Virtualizzazione delle funzioni di rete

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su virtualizzazione delle funzioni di rete, che consente di distribuire le Appliance virtuali di rete, ad esempio Firewall del centro dati, multi-tenant RAS Gateway e il bilanciamento del carico Software \(SLB\) multiplexer connesso \(MUX\).
  
>[!NOTE]  
>Oltre a questo argomento, la documentazione di virtualizzazione delle funzioni di rete seguente è disponibile.  
> - [Cenni preliminari sul datacenter Firewall](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)  
> - [Gateway RAS per SDN](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)  
> - [Software Load Balancing (SLB) per SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)  
  
Nel software di oggi sempre più Data Center definito, le funzioni di rete vengono eseguite da Appliance hardware (ad esempio servizi di bilanciamento del carico, firewall, router, commutatori e così via) sono spesso come Appliance virtuali. Questo "virtualizzazione delle funzioni di rete" è una progressione naturale di virtualizzazione di server e virtualizzazione di rete. Appliance virtuali sono rapidamente emergenti e creazione di un nuovo mercato. Continuano a generare interesse e ottenere l'espansione di entrambe le piattaforme di virtualizzazione e i servizi cloud.  
  
Microsoft ha incluso un gateway autonomo come appliance virtuale a partire da Windows Server 2012 R2. Per altre informazioni, vedere [Windows Server Gateway](https://technet.microsoft.com/library/dn313101.aspx). Ora con Windows Server 2016 Microsoft continua a espandere e investire del mercato di virtualizzazione di rete (funzione).  
  
## <a name="virtual-appliance-benefits"></a>Vantaggi di appliance virtuale  
Un'appliance virtuale è dinamico e facili da modificare perché è una macchina virtuale predefinita e personalizzata. Può trattarsi di uno o più macchine virtuali incluso nel pacchetto, aggiornata e gestita come singola unità. Insieme a software definito networking (SDN), si ottiene l'agilità e la flessibilità necessaria in infrastrutture basate su cloud di oggi. Ad esempio:   
  
-   SDN presenta la rete come una risorsa dinamica e in pool.  
  
-   SDN facilita l'isolamento dei tenant.  
  
-   SDN Massimizza la scalabilità e prestazioni.  
  
-   Appliance virtuali di consentono la mobilità del carico di lavoro e l'espansione della capacità senza problemi.  
  
-   Appliance virtuali di ridurre al minimo la complessità operativa.  
  
-   Appliance virtuali di consentono ai clienti di acquisire, distribuire e gestire soluzioni preintegrate.  
  
    -   I clienti possono facilmente spostare l'appliance virtuale in un punto qualsiasi nel cloud.  
  
    -   I clienti possono aumentare Appliance virtuali o verso il basso in modo dinamico in base alla domanda.  
  
Per altre informazioni su SDN di Microsoft, vedere [Software Defined Networking](https://technet.microsoft.com/windows-server-docs/networking/sdn/software-defined-networking--sdn-).  
  
### <a name="what-network-functions-are-being-virtualized"></a>Quali funzioni di rete sono la virtualizzazione?  
Il marketplace per le funzioni di rete virtualizzato sta crescendo rapidamente. Le funzioni di rete seguenti sono la virtualizzazione:  
  
-   **Sicurezza**  
  
    -   Firewall  
  
    -   Antivirus  
  
    -   DDoS (Distributed Denial of Service)  
  
    -   IPS/IDS (sistema di rilevamento intrusione/sistema prevenzione intrusioni)  
  
-   **Utilità di ottimizzazione dell'applicazione/WAN**  
  
-   **Edge**  
  
    -   Gateway da sito a sito  
  
    -   Gateway L3  
  
    -   Router  
  
    -   Commutatori  
  
    -   NAT  
  
    -   Servizi di bilanciamento del carico (non necessariamente in corrispondenza del bordo)  
  
    -   Proxy HTTP  
  
## <a name="why-microsoft-is-a-great-platform-for-virtual-appliances"></a>Perché Microsoft è una piattaforma eccellente per le Appliance virtuali  
![Stack di rete virtuale](../../../media/Network-Function-Virtualization/Microsoft-Network-Function-Virtualization.png)  
  
La piattaforma Microsoft è stata progettata per creare una piattaforma eccellente per compilare e distribuire Appliance virtuali. Ecco perché:  
  
-   Microsoft fornisce le funzioni principali delle reti virtualizzate con Windows Server 2016.  
  
-   È possibile distribuire un'appliance virtuale dal fornitore di propria scelta.  
  
-   È possibile distribuire, configurare e gestire le Appliance virtuali con il Controller di rete Microsoft fornito con Windows Server 2016. Per altre informazioni sui Controller di rete, vedere [Controller di rete](../../../sdn/technologies/network-controller/Network-Controller.md).  
  
-   Hyper-V può ospitare i sistemi operativi guest superiore che è necessario.  
  
## <a name="network-function-virtualization-in-windows-server-2016"></a>Virtualizzazione delle funzioni di rete in Windows Server 2016  
  
### <a name="virtual-appliances-functions-provided-by-microsoft"></a>Funzioni di Appliance virtuali fornite da Microsoft  
Le Appliance virtuali seguenti vengono fornite con Windows Server 2016:  
  
**Bilanciamento del carico software**  
  
Un bilanciamento del carico di livello 4 opera su larga scala di Data Center. Si tratta di una versione analoga di bilanciamento del carico di Azure che è stata distribuita su vasta scala nell'ambiente Azure. Per altre informazioni sul bilanciamento del carico Software Microsoft, vedere [bilanciamento del carico Software (SLB) per SDN](https://technet.microsoft.com/library/mt632286.aspx). Per altre informazioni sui servizi di bilanciamento del carico di Microsoft Azure, vedere [servizi di bilanciamento del carico di Microsoft Azure](https://azure.microsoft.com/blog/2014/04/08/microsoft-azure-load-balancing-services/).  
  
**Gateway**. Gateway RAS fornisce tutte le combinazioni delle funzioni di gateway seguenti.  
  
-   **Gateway da sito a sito**  
  
    Il Gateway RAS offre un protocollo BGP (Border Gateway)-gateway in grado di supportare e multi-tenant che consente i tenant potranno accedere alle risorse e gestirle tramite connessioni VPN site-to-site da siti remoti e che consente di flusso del traffico di rete tra le risorse virtuali nelle reti fisiche cloud e del tenant. Per altre informazioni sul Gateway RAS, vedere [disponibilità elevata del Gateway RAS](https://technet.microsoft.com/library/mt631692.aspx) e [Gateway RAS](https://technet.microsoft.com/library/mt626650.aspx).  
  
-   **Gateway di inoltro**  
  
    RAS Gateway instrada il traffico tra reti virtuali e la rete fisica provider di hosting. Ad esempio, se i tenant creino una o più reti virtuali e richiedono l'accesso alle risorse condivise sulla rete fisica nel provider di hosting, il gateway di inoltro può instradare il traffico tra la rete virtuale e la rete fisica per consentire agli utenti che lavorano su la rete virtuale con i servizi di cui hanno bisogno. Per altre informazioni, vedere [disponibilità elevata del Gateway RAS](https://technet.microsoft.com/library/mt631692.aspx) e [Gateway RAS](https://technet.microsoft.com/library/mt626650.aspx).  
  
-   **Gateway tunnel GRE**  
  
    I tunnel basati su GRE consentono la connettività tra reti virtuali tenant e reti esterne. Poiché il protocollo GRE è leggero e il supporto per GRE è disponibile nella maggior parte dei dispositivi di rete, questa diventa la scelta ideale per il tunneling in cui non è necessaria la crittografia dei dati. Il supporto di GRE nei tunnel da sito a sito (S2S) risolve il problema dell'inoltro tra reti virtuali tenant e reti esterne tenant tramite un gateway multi-tenant. Per altre informazioni sui tunnel GRE, vedere [Tunneling GRE in Windows Server 2016](https://technet.microsoft.com/library/dn765485.aspx).  
  
**Piano di controllo del routing BGP**  
  
Controllo di Routing di Hyper-V rete virtualizzazione è l'entità logica e centralizzata nel piano di controllo, che contiene tutte le route di piano di indirizzo del cliente e apprende in modo dinamico e quindi aggiorna il router RAS Gateway distribuito nella rete virtuale. Per altre informazioni, vedere [disponibilità elevata del Gateway RAS](https://technet.microsoft.com/library/mt631692.aspx) e [Gateway RAS](https://technet.microsoft.com/library/mt626650.aspx).  
  
**Firewall multi-tenant distribuito**  
  
Il firewall consente di proteggere il livello di rete di reti virtuali. I criteri vengono applicati alla porta vSwitch SDN di ogni macchina virtuale tenant. Protegge tutti i flussi di traffico: EST-ovest e Nord-Sud. I criteri vengono inseriti tramite il portale tenant e li distribuisce il Controller di rete a tutti gli host applicabili. Per altre informazioni sul firewall multi-tenant distribuito, vedere [Cenni preliminari sul Firewall Datacenter](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md).  
  


