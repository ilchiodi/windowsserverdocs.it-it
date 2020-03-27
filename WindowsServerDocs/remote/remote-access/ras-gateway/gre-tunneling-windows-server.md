---
title: Tunneling GRE in Windows Server 2016
description: È possibile utilizzare questo argomento per ottenere informazioni sugli aggiornamenti alla funzionalità del tunnel GRE (Generic Routing Encapsulation) per il gateway RAS in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: df2023bf-ba64-481e-b222-6f709edaa5c1
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d246f0e56681f75e4336ed225d1557a0e05c581b
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80308549"
---
# <a name="gre-tunneling-in-windows-server-2016"></a>Tunneling GRE in Windows Server 2016

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Windows Server 2016 fornisce aggiornamenti per Generic Routing Encapsulation \(la funzionalità di tunnel\) GRE per il gateway RAS.  
  
GRE è un protocollo di tunneling leggero in grado di incapsulare un'ampia gamma di protocolli a livello di rete all'interno di collegamenti point-to-point virtuali in un sistema Internetwork IP (Internet Protocol, protocollo Internet). L'implementazione di Microsoft GRE può incapsulare IPv4 e IPv6.  
  
I tunnel GRE sono utili in molti scenari perché:  
  
-   Sono conformi a Lightweight e RFC 2890, rendendolo interoperabile con diversi dispositivi fornitore  
  
-   È possibile usare Border Gateway Protocol \(\) BGP per il routing dinamico  
  
-   È possibile configurare i gateway RAS multitenant GRE da usare con Software Defined Networking \(SDN\)
  
-   È possibile usare System Center Virtual Machine Manager per gestire i gateway RAS basati su\-GRE
  
-   È possibile ottenere fino a 2,0 Gbps di velocità effettiva in una macchina virtuale a 6 core configurata come gateway RAS GRE
  
-   Un singolo gateway supporta più modalità di connessione  
  
I tunnel basati su GRE consentono la connettività tra reti virtuali tenant e reti esterne. Poiché il protocollo GRE è leggero e il supporto per GRE è disponibile nella maggior parte dei dispositivi di rete, diventa la scelta ideale per il tunneling in cui la crittografia dei dati non è necessaria. 

Il supporto di GRE nei tunnel da sito a sito (S2S) risolve il problema dell'invio tra reti virtuali tenant e reti esterne tenant tramite un gateway multi-tenant, come descritto più avanti in questo argomento.  
  
La funzionalità di tunneling GRE è progettata per soddisfare i requisiti seguenti:  
  
-   Un provider di hosting deve essere in grado di creare reti virtuali per l'invio senza modificare la configurazione del commutere fisico.  
  
-   Un provider di hosting deve essere in grado di aggiungere subnet alle reti rivolte all'esterno senza modificare la configurazione dei commutatori fisici all'interno dell'infrastruttura.  
La funzionalità di tunneling GRE Abilita o migliora diversi scenari chiave per l'hosting di provider di servizi tramite tecnologie Microsoft per implementare software defined networking nelle offerte di servizio.  
  
Di seguito sono riportati alcuni scenari di esempio:  
  
-   [Accesso dalle reti virtuali tenant alle reti fisiche tenant](#BKMK_Access)  
  
-   [Connettività ad alta velocità](#BKMK_Speed)  
  
-   [Integrazione con isolamento basato su VLAN](#BKMK_Integration)  
  
-   [Accedere alle risorse condivise](#BKMK_Shared)  
  
-   [Servizi di dispositivi di terze parti per i tenant](#BKMK_thirdparty)  
  
## <a name="key-scenarios"></a>Scenari principali

Di seguito sono riportati gli scenari principali a cui viene indirizzata la funzionalità del tunnel GRE.  
  
### <a name="access-from-tenant-virtual-networks-to-tenant-physical-networks"></a><a name="BKMK_Access"></a>Accesso dalle reti virtuali tenant alle reti fisiche tenant

Questo scenario consente un modo scalabile per fornire l'accesso dalle reti virtuali tenant alle reti fisiche dei tenant situate nella sede del provider di servizi di hosting. Un endpoint del tunnel GRE viene stabilito sul gateway multi-tenant, l'altro endpoint del tunnel GRE viene stabilito in un dispositivo di terze parti sulla rete fisica. Il traffico di livello 3 viene instradato tra le macchine virtuali nella rete virtuale e il dispositivo di terze parti sulla rete fisica.  
  
![Tunnel GRE che connette la rete fisica dell'hoster e la rete virtuale tenant](../../media/gre-tunneling-in-windows-server/GRE_.png)  
  
### <a name="high-speed-connectivity"></a><a name="BKMK_Speed"></a>Connettività ad alta velocità

Questo scenario consente un modo scalabile per offrire connettività ad alta velocità dalla rete locale del tenant alla rete virtuale che si trova nella rete del provider di servizi di hosting. Un tenant si connette alla rete del provider di servizi tramite la modalità MPLS (Multiprotocol Label Switching), in cui viene stabilito un tunnel GRE tra il router perimetrale del provider di servizi di hosting e il gateway multi-tenant per la rete virtuale del tenant.  
  
![GRE tunnel Connecting tenant Enterprise MPLS Network e rete virtuale tenant](../../media/gre-tunneling-in-windows-server/GRE-.png)  
  
### <a name="integration-with-vlan-based-isolation"></a><a name="BKMK_Integration"></a>Integrazione con isolamento basato su VLAN

Questo scenario consente di integrare l'isolamento basato su VLAN con la virtualizzazione rete Hyper-V. Una rete fisica nella rete del provider di hosting contiene un servizio di bilanciamento del carico che usa l'isolamento basato su VLAN. Un gateway multi-tenant stabilisce Tunnel GRE tra il servizio di bilanciamento del carico nella rete fisica e il gateway multi-tenant nella rete virtuale.  
  
È possibile stabilire più tunnel tra l'origine e la destinazione e la chiave GRE viene usata per distinguere i tunnel.  
  
![Più Tunnel GRE che connettono reti virtuali tenant](../../media/gre-tunneling-in-windows-server/GRE-VLANIsolation.png)  
  
### <a name="access-shared-resources"></a><a name="BKMK_Shared"></a>Accedere alle risorse condivise

Questo scenario consente di accedere alle risorse condivise in una rete fisica situata nella rete del provider di hosting.  
  
È possibile che un servizio condiviso si trovi in un server in una rete fisica che si trova nella rete del provider di hosting che si vuole condividere con più reti virtuali tenant.  
  
Le reti tenant con subnet non sovrapposte accedono alla rete comune tramite un tunnel GRE. Un gateway single-tenant instrada tra i tunnel GRE, indirizzando quindi i pacchetti alle reti tenant appropriate.  
  
In questo scenario, il gateway single-tenant può essere sostituito da appliance hardware di terze parti.  
  
![Un gateway a tenant singolo che usa più tunnel per connettere più reti virtuali](../../media/gre-tunneling-in-windows-server/GRE-SharedResource.png)  
  
### <a name="services-of-third-party-devices-to-tenants"></a><a name="BKMK_thirdparty"></a>Servizi di dispositivi di terze parti per i tenant

Questo scenario può essere usato per integrare i dispositivi di terze parti, ad esempio i bilanciamenti del carico hardware, nel flusso del traffico della rete virtuale del tenant. Ad esempio, il traffico originato da un sito aziendale passa attraverso un tunnel S2S al gateway multi-tenant. Il traffico viene indirizzato al servizio di bilanciamento del carico su un tunnel GRE. Il servizio di bilanciamento del carico instrada il traffico verso più macchine virtuali nella rete virtuale dell'organizzazione. La stessa cosa accade per un altro tenant con indirizzi IP potenzialmente sovrapposti nelle reti virtuali. Il traffico di rete è isolato nel servizio di bilanciamento del carico usando le VLAN ed è applicabile a tutti i dispositivi di livello 3 che supportano le VLAN.  
  
![Più Tunnel GRE che connettono le reti virtuali a dispositivi di terze parti](../../media/gre-tunneling-in-windows-server/GREThirdParty.png)  
  
## <a name="configuration-and-deployment"></a>Configurazione e distribuzione

Un tunnel GRE viene esposto come protocollo aggiuntivo all'interno di un'interfaccia S2S. Viene implementato in modo analogo a un tunnel S2S IPSec descritto nel Blog di rete seguente: [gateway VPN da sito a sito (S2S) multi-tenant con Windows Server 2012 R2](https://blogs.technet.com/b/networking/archive/2013/09/29/multi-tenant-site-to-site-s2s-vpn-gateway-with-windows-server-2012-r2.aspx)  
  
Vedere l'argomento seguente per un esempio che distribuisce i gateway, inclusi i gateway del tunnel GRE:  
  
[Distribuire un'infrastruttura software defined Network usando gli script](../../../networking/sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)
  
## <a name="more-information"></a>Altre informazioni

Per ulteriori informazioni sulla distribuzione di gateway S2S, vedere gli argomenti seguenti:  
  
-   [Gateway RAS](RAS-Gateway.md)  
  
-   [Border Gateway Protocol &#40;BGP&#41;](../bgp/Border-Gateway-Protocol-BGP.md)  
  
-   [Nuovo! Guida alla distribuzione di RAS multitenant gateway di Windows Server 2012 R2](https://blogs.technet.com/b/wsnetdoc/archive/2014/03/26/new-windows-server-2012-r2-RAS-multitenant-gateway-deployment-guide.aspx)  
  
-   [Distribuire Border Gateway Protocol (BGP) con RAS multi-tenant gateway](https://blogs.technet.com/b/wsnetdoc/archive/2014/04/03/deploy-border-gateway-protocol-bgp-with-the-RAS-multitenant-gateway.aspx)  
  


