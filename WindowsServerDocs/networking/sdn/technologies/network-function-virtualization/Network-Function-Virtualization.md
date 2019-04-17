---
title: Virtualizzazione delle funzioni di rete
description: È possibile utilizzare questo argomento per informazioni su virtualizzazione delle funzioni di rete, che consente di distribuire dispositivi di rete virtuali come Firewall del centro dati, multi-tenant RAS Gateway e il bilanciamento del carico Software (SLB) in Windows Server 2016.
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
ms.openlocfilehash: 7caa9eef42cb7ab95a13d64c1dcd3639b1132eb3
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="network-function-virtualization"></a>Virtualizzazione delle funzioni di rete

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su virtualizzazione delle funzioni di rete, che consente di distribuire dispositivi di rete virtuali, ad esempio Firewall del centro dati, multitenant Gateway RAS e il bilanciamento del carico Software \(SLB\) \(MUX\) multiplexer.
  
>[!NOTE]  
>Oltre a questo argomento, è disponibile la seguente documentazione di virtualizzazione delle funzioni di rete.  
> - [Cenni preliminari sul datacenter Firewall](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)  
> - [Gateway RAS per SDN](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)  
> - [Software Load Balancing (SLB) per SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)  
  
Nel software di oggi Data Center definito, le funzioni di rete che vengono eseguite da Appliance hardware (ad esempio servizi di bilanciamento del carico, firewall, router, commutatori e così via) sono viene virtualizzato sempre più spesso come Appliance virtuali. Questo "virtualizzazione delle funzioni di rete" è una progressione naturale di virtualizzazione di server e virtualizzazione di rete. Appliance virtuali sono rapidamente emergenti e creazione di un nuovo mercato. Continuano a generare interesse e ottenere l'espansione di entrambe le piattaforme di virtualizzazione e i servizi cloud.  
  
Microsoft ha incluso un gateway autonomo come appliance virtuale a partire da Windows Server 2012 R2. Per ulteriori informazioni, vedere [Windows Server Gateway](https://technet.microsoft.com/library/dn313101.aspx). Ora con Windows Server 2016 Microsoft continua a espandere e investire sul mercato di virtualizzazione rete funzione.  
  
## <a name="virtual-appliance-benefits"></a>Vantaggi dei dispositivi virtuali  
Un accessorio virtuale è dinamico e facile da modificare perché si tratta di una macchina virtuale predefinita e personalizzata. Può trattarsi di uno o più macchine virtuali in pacchetto, aggiornata e gestiti come un'unità. Insieme al software definito reti (SDN), ottenere la flessibilità e la flessibilità necessaria nell'infrastruttura basata su cloud di oggi. Per esempio:  
  
-   SDN presenta la rete come una risorsa in pool e dinamica.  
  
-   SDN facilita l'isolamento dei tenant.  
  
-   SDN ottimizza la scalabilità e prestazioni.  
  
-   Appliance virtuali la mobilità del carico di lavoro e l'espansione di capacità trasparente.  
  
-   Dispositivi di rete di ridurre al minimo la complessità operativa.  
  
-   Appliance virtuali consentono ai clienti di acquisire, distribuire e gestire soluzioni pre-integrate.  
  
    -   I clienti possono spostare agevolmente accessorio virtuale ovunque nel cloud.  
  
    -   I clienti possono aumentare Appliance virtuali o verso il basso in modo dinamico in base alle esigenze.  
  
Per ulteriori informazioni su SDN Microsoft vedere [rete definita dal Software](https://technet.microsoft.com/windows-server-docs/networking/sdn/software-defined-networking--sdn-).  
  
### <a name="what-network-functions-are-being-virtualized"></a>Le funzioni di rete sono viene virtualizzate?  
È in rapida crescita marketplace per le funzioni di rete virtualizzato. Le funzioni di rete seguenti sono viene virtualizzate:  
  
-   **Protezione**  
  
    -   Firewall  
  
    -   Antivirus  
  
    -   DDoS (distribuita Denial of Service)  
  
    -   IP/ID (prevenzione intrusioni/sistema rilevamento delle intrusioni)  
  
-   **Ottimizzatori WAN/applicazione**  
  
-   **Edge**  
  
    -   Gateway da sito a sito  
  
    -   Gateway L3  
  
    -   Router  
  
    -   Commutatori  
  
    -   NAT  
  
    -   Servizi di bilanciamento del carico (non necessariamente in corrispondenza del bordo)  
  
    -   Proxy HTTP  
  
## <a name="why-microsoft-is-a-great-platform-for-virtual-appliances"></a>Perché Microsoft è una piattaforma ottima per i dispositivi di rete  
![Stack di rete virtuale](../../../media/Network-Function-Virtualization/Microsoft-Network-Function-Virtualization.png)  
  
La piattaforma Microsoft è stata progettata per essere una piattaforma ottima per compilare e distribuire Appliance virtuali. Ecco perché:  
  
-   Microsoft fornisce le funzioni di rete virtualizzato chiave con Windows Server 2016.  
  
-   È possibile distribuire un accessorio virtuale dal fornitore di tua scelta.  
  
-   È possibile distribuire, configurare e gestire le Appliance virtuali con il Controller di rete Microsoft che viene fornito con Windows Server 2016. Per ulteriori informazioni sui Controller di rete, vedere [Controller di rete](../../../sdn/technologies/network-controller/Network-Controller.md).  
  
-   Hyper-V può ospitare i sistemi operativi guest principali che è necessario.  
  
## <a name="network-function-virtualization-in-windows-server-2016"></a>Virtualizzazione delle funzioni di rete in Windows Server 2016  
  
### <a name="virtual-appliances-functions-provided-by-microsoft"></a>Funzioni di Appliance virtuali fornite da Microsoft  
I dispositivi di rete seguenti vengono fornite con Windows Server 2016:  
  
**Bilanciamento del carico software**  
  
Un livello 4 bilanciamento del carico operativo su larga scala di Data Center. Si tratta di una versione simile di bilanciamento del carico di Azure che è stato distribuito su larga scala nell'ambiente di Azure. Per ulteriori informazioni su bilanciamento del carico Software Microsoft, vedere [Software Load Balancing (SLB) per SDN](https://technet.microsoft.com/library/mt632286.aspx). Per ulteriori informazioni sui servizi di bilanciamento del carico di Microsoft Azure, vedere [servizi di bilanciamento del carico di Microsoft Azure](https://azure.microsoft.com/blog/2014/04/08/microsoft-azure-load-balancing-services/).  
  
**Gateway**. Gateway RAS fornisce tutte le combinazioni delle funzioni di gateway seguenti.  
  
-   **Gateway da sito a sito**  
  
    Gateway RAS fornisce un protocollo BGP (Border Gateway)-gateway in grado di supportare, multi-tenant che consente ai tenant di accedere alle risorse e gestirle tramite connessioni VPN site-to-site da siti remoti e che consente il traffico di rete tra le risorse virtuali nelle reti fisiche cloud e tenant. Per ulteriori informazioni sul Gateway RAS, vedere [disponibilità elevata del Gateway RAS](https://technet.microsoft.com/library/mt631692.aspx) e [Gateway RAS](https://technet.microsoft.com/library/mt626650.aspx).  
  
-   **Gateway di inoltro**  
  
    RAS Gateway instrada il traffico tra reti virtuali e la rete fisica servizio di hosting di provider. Ad esempio, se creino uno o più reti virtuali tenant e richiedono l'accesso a risorse condivise sulla rete fisica del provider di hosting, il gateway di inoltro può instradare il traffico tra la rete virtuale e la rete fisica per fornire agli utenti che lavorano nella rete virtuale con i servizi che devono essere. Per ulteriori informazioni, vedere [disponibilità elevata del Gateway RAS](https://technet.microsoft.com/library/mt631692.aspx) e [Gateway RAS](https://technet.microsoft.com/library/mt626650.aspx).  
  
-   **Gateway tunnel GRE**  
  
    Basati su GRE consentono la connettività tunnel tra reti virtuali tenant e reti esterne. Poiché il protocollo GRE è leggero e il supporto per GRE è disponibile nella maggior parte dei dispositivi di rete, diventa la scelta ideale per il tunneling in cui non è necessaria la crittografia dei dati. Supporto di GRE nei tunnel di da sito a sito (S2S) risolve il problema dell'inoltro tra reti virtuali tenant e reti esterne tenant tramite un gateway multi-tenant. Per ulteriori informazioni sui tunnel GRE, vedere [GRE Tunneling in Windows Server 2016](https://technet.microsoft.com/library/dn765485.aspx).  
  
**Routing piano di controllo con il protocollo BGP**  
  
Controllo di Routing di Hyper-V rete virtualizzazione è l'entità logica centralizzata nel piano di controllo, che esegue tutte le route del piano di indirizzi del cliente apprende in modo dinamico e quindi aggiorna il router RAS Gateway distribuito nella rete virtuale. Per ulteriori informazioni, vedere [disponibilità elevata del Gateway RAS](https://technet.microsoft.com/library/mt631692.aspx) e [Gateway RAS](https://technet.microsoft.com/library/mt626650.aspx).  
  
**Firewall multi-tenant distribuito**  
  
Il firewall protegge il livello di rete di reti virtuali. I criteri vengono applicati alla porta vSwitch di SDN di ogni macchina virtuale del tenant. Consente di proteggere tutti i flussi di traffico: ovest orientale e Nord-Sud. I criteri vengono inviati tramite il portale tenant e il Controller di rete li distribuisce a tutti gli host applicabili. Per ulteriori informazioni sui firewall multi-tenant distribuita, vedere [Cenni preliminari sul Firewall Datacenter](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md).  
  


