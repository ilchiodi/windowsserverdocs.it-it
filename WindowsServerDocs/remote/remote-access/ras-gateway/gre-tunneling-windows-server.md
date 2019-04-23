---
title: Tunneling GRE in Windows Server 2016
description: È possibile utilizzare questo argomento per una conoscenza degli aggiornamenti alla capacità di tunnel Generic Routing Encapsulation (GRE) per il Gateway RAS in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: df2023bf-ba64-481e-b222-6f709edaa5c1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e0ec077ad5e97edd3db7d1dc4e662bb191f7885b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887862"
---
# <a name="gre-tunneling-in-windows-server-2016"></a>Tunneling GRE in Windows Server 2016

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Windows Server 2016 sono disponibili aggiornamenti per Generic Routing Encapsulation \(GRE\) tunnel funzionalità per il Gateway RAS.  
  
GRE è un protocollo di tunneling leggero in grado di incapsulare un'ampia gamma di protocolli a livello di rete all'interno di collegamenti point-to-point virtuali in un sistema Internetwork IP (Internet Protocol, protocollo Internet). L'implementazione Microsoft GRE può incapsulare IPv4 e IPv6.  
  
I tunnel GRE sono utili in molti scenari poiché:  
  
-   Essi sono elementi leggeri e 2890 RFC conforme, rendendo interoperabile con vari tipi di dispositivi fornitore  
  
-   È possibile usare Border Gateway Protocol \(BGP\) per il routing dinamico  
  
-   È possibile configurare multi-tenant GRE RAS gateway per l'utilizzo con Software Defined Networking \(SDN\)
  
-   È possibile usare System Center Virtual Machine Manager per gestire GRE\-RAS gateway basati su
  
-   È possibile ottenere una velocità effettiva fino a 2.0 Gbps in una macchina virtuale 6 core configurato come un Gateway RAS GRE
  
-   Un singolo gateway supporta più modalità di connessione  
  
I tunnel basati su GRE consentono la connettività tra reti virtuali tenant e reti esterne. Poiché il protocollo GRE è leggero e il supporto per GRE è disponibile nella maggior parte dei dispositivi di rete diventa la scelta ideale per il tunneling in cui non è necessaria la crittografia dei dati. 

Supporto di GRE nei tunnel di da sito a sito (S2S) risolve il problema dell'inoltro tra reti virtuali tenant e reti esterne tenant tramite un gateway multi-tenant, come descritto più avanti in questo argomento.  
  
La funzionalità di tunnel GRE è progettata per soddisfare i requisiti seguenti:  
  
-   Un provider di hosting deve essere in grado di creare reti virtuali per l'inoltro senza modificare la configurazione di commutatore fisico.  
  
-   Un provider di hosting deve essere in grado di aggiungere subnet alle reti accessibile pubblicamente senza modificare la configurazione delle opzioni nell'infrastruttura fisiche.  
La funzionalità di tunnel GRE supporta o Ottimizza diversi scenari chiave per l'hosting di provider di servizi tramite le tecnologie Microsoft per implementare Software Defined Networking nelle proprie offerte di servizio.  
  
Di seguito sono indicati alcuni scenari di esempio:  
  
-   [Accesso da reti virtuali tenant alle reti fisiche del tenant](#BKMK_Access)  
  
-   [Connettività ad alta velocità](#BKMK_Speed)  
  
-   [Integrazione con l'isolamento basato su VLAN](#BKMK_Integration)  
  
-   [Risorse di accesso condiviso](#BKMK_Shared)  
  
-   [Servizi di dispositivi di terze parti per i tenant](#BKMK_thirdparty)  
  
## <a name="key-scenarios"></a>Scenari principali

Di seguito sono gli scenari principali che il GRE tunneling gli indirizzi di funzionalità.  
  
### <a name="BKMK_Access"></a>Accesso da reti virtuali tenant alle reti fisiche del tenant

Questo scenario consente di fornire l'accesso da reti virtuali tenant alle reti fisiche che si trova presso la sede del provider del servizio host del tenant in modo scalabile. Un endpoint del tunnel GRE è stabilite nel gateway multi-tenant, gli altri endpoint del tunnel GRE viene stabilito in un dispositivo di terze parti nella rete fisica. Livello 3 traffico instradato tra le macchine virtuali nella rete virtuale e il dispositivo di terze parti nella rete fisica.  
  
![Tunnel GRE la connessione di rete fisica di hosting e rete virtuale del tenant](../../media/gre-tunneling-in-windows-server/GRE_.png)  
  
### <a name="BKMK_Speed"></a>Connettività ad alta velocità

Questo scenario consente di fornire la connettività ad alta velocità dalla rete locale tenant alla rete virtuale che si trova nella rete di provider del servizio di hosting in modo scalabile. Un tenant si connette alla rete del provider del servizio tramite mpls (MPLS), in cui viene stabilito un tunnel GRE tra i router perimetrali del provider di servizi di hosting e il gateway multi-tenant alla rete virtuale del tenant.  
  
![Tunnel GRE la connessione di rete MPLS enterprise del tenant e reti virtuali tenant](../../media/gre-tunneling-in-windows-server/GRE-.png)  
  
### <a name="BKMK_Integration"></a>Integrazione con l'isolamento basato su VLAN

Questo scenario consente di integrare isolamento VLAN basato su virtualizzazione rete Hyper-V. Una rete fisica in rete del provider di hosting contiene un servizio di bilanciamento del carico Usa l'isolamento VLAN-based. Un gateway multi-tenant consente di stabilire i tunnel GRE tra il servizio di bilanciamento del carico sulla rete fisica e il gateway multi-tenant nella rete virtuale.  
  
È possibile stabilire più tunnel tra origine e destinazione e la chiave GRE è utilizzata per discriminare tra i tunnel.  
  
![Esegue il tunneling GRE più la connessione di reti virtuali tenant](../../media/gre-tunneling-in-windows-server/GRE-VLANIsolation.png)  
  
### <a name="BKMK_Shared"></a>Risorse di accesso condiviso

Questo scenario consente di accedere alle risorse condivise in una rete fisica che si trova nella rete del provider di hosting.  
  
Potrebbe essere un servizio condiviso che si trova in un server in una rete fisica che si trova nella rete del provider hosting che si desidera condividere con più reti virtuali tenant.  
  
Le reti tenant con subnet non sovrapposta accedere alla rete più comune tramite un tunnel GRE. Un gateway tenant singolo di route tra i tunnel GRE, pertanto il routing di pacchetti per le reti tenant appropriata.  
  
In questo scenario, il gateway singolo tenant può essere sostituito da Appliance hardware di terze parti.  
  
![Un gateway a tenant singolo con più tunnel per connettere più reti virtuali](../../media/gre-tunneling-in-windows-server/GRE-SharedResource.png)  
  
### <a name="BKMK_thirdparty"></a>Servizi di dispositivi di terze parti per i tenant

Questo scenario può essere utilizzato per integrare i dispositivi di terze parti (ad esempio, il bilanciamento del carico hardware) nel flusso di traffico di rete virtuale tenant. Ad esempio, il traffico proveniente da un sito aziendale passa attraverso un tunnel S2S al gateway multi-tenant. Il traffico viene instradato al bilanciamento del carico su un tunnel GRE. Il servizio di bilanciamento del carico instrada il traffico a più macchine virtuali nella rete virtuale dell'azienda. Lo stesso avviene per un tenant diverso con potenzialmente sovrapposti gli indirizzi IP nelle reti virtuali. Il traffico di rete isolato sul bilanciamento del carico usano VLAN ed è applicabile a tutti i dispositivi di livello 3 che supportano le VLAN.  
  
![Esegue il tunneling GRE più reti virtuali che si connette ai dispositivi di terze parti](../../media/gre-tunneling-in-windows-server/GREThirdParty.png)  
  
## <a name="configuration-and-deployment"></a>Configurazione e distribuzione

Un tunnel GRE viene esposta come un protocollo aggiuntivo all'interno di un'interfaccia S2S. Viene implementato in modo simile come un tunnel IPSec S2S descritto nel Blog di rete seguente: [Gateway VPN multi-tenant Site-to-Site (S2S) con Windows Server 2012 R2](https://blogs.technet.com/b/networking/archive/2013/09/29/multi-tenant-site-to-site-s2s-vpn-gateway-with-windows-server-2012-r2.aspx)  
  
Vedere l'argomento seguente per un esempio che distribuisce un gateway, inclusi gateway tunnel GRE:  
  
[Distribuire un'infrastruttura Software Defined Networking tramite script](../../../networking/sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)
  
## <a name="more-information"></a>Altre informazioni

Per altre informazioni sulla distribuzione di gateway S2S, vedere gli argomenti seguenti:  
  
-   [Gateway RAS](RAS-Gateway.md)  
  
-   [Border Gateway Protocol &#40;BGP&#41;](../bgp/Border-Gateway-Protocol-BGP.md)  
  
-   [Nuovo! Guida alla distribuzione di Windows Server 2012 R2 RAS Gateway multi-tenant](https://blogs.technet.com/b/wsnetdoc/archive/2014/03/26/new-windows-server-2012-r2-RAS-multitenant-gateway-deployment-guide.aspx)  
  
-   [Distribuire Border Gateway Protocol (BGP) con il Gateway multi-tenant RAS](https://blogs.technet.com/b/wsnetdoc/archive/2014/04/03/deploy-border-gateway-protocol-bgp-with-the-RAS-multitenant-gateway.aspx)  
  


