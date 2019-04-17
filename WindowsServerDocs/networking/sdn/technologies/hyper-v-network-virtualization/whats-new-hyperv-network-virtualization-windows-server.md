---
title: Novità di virtualizzazione rete Hyper-V in Windows Server 2016
description: Questo argomento fornisce informazioni sulle nuove funzionalità di virtualizzazione rete Hyper-V in Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0254275a-0a77-40a9-b68a-1029284c03fe
ms.author: pashort
author: shortpatti
ms.date: 03/19/2018
ms.openlocfilehash: 0954768944e44848debfbb7fb752a13ca47031c2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-hyper-v-network-virtualization-in-windows-server-2016"></a>Novità di virtualizzazione rete Hyper-V in Windows Server 2016

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Questo argomento descrive le funzionalità di Hyper-V rete virtualizzazione nuove sono modificate in Windows Server 2016.  
  
## <a name="BKMK_IPAM2012R2"></a>Aggiornamenti in virtualizzazione rete  
Virtualizzazione rete offre supporto avanzato nelle aree seguenti:  
  
|Funzionalità|Nuove o migliorate|Descrizione|  
|--------------------------|-------------------|---------------|  
|[Programmabile commutatore Hyper-V](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#SDN)|Nuovo|Criteri di virtualizzazione rete sono programmabili tramite il Controller di rete Microsoft.|  
|[Supporto di incapsulamento VXLAN](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#VXLAN)|Nuovo|Virtualizzazione rete supporta ora l'incapsulamento VXLAN.|  
|[Interoperabilità del servizio di bilanciamento carico (SLB) del software](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#SLB)|Nuovo|Virtualizzazione rete è completamente integrato con bilanciamento del carico Software Microsoft.|  
|[Intestazioni Ethernet IEEE conforme](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#L2)|Migliorato|Conforme agli standard IEEE Ethernet|  
  
### <a name="SDN"></a>Programmabile commutatore Hyper-V  
Virtualizzazione rete è un blocco predefinito fondamentale per la soluzione di rete SDN (Software Defined) aggiornata di Microsoft ed è completamente integrato nello stack di SDN.  
  
Il nuovo Controller di rete Microsoft inserisce i criteri di virtualizzazione rete fino a un agente Host in esecuzione in ogni host utilizzando vSwitch aprire Database Management Protocol (OVSDB) come interfaccia SouthBound (SBI). L'agente Host archivia questo criterio utilizzando un programma di personalizzazione del [schema VTEP](https://github.com/openvswitch/ovs/blob/master/vtep/vtep.ovsschema) e i programmi di regole di flusso complesso in un motore del flusso ad alte prestazioni nel commutatore Hyper-V.  
  
Il motore del flusso all'interno del commutatore Hyper-V è lo stesso motore usato in Microsoft Azure&trade;, che è stata sottoposta a livello di hyper-nel cloud pubblico Microsoft Azure. Inoltre, l'intero stack SDN backup tramite il Controller di rete e il Provider di risorse di rete (dettagli presto disponibile) è coerente con Microsoft Azure, pertanto la potenza della cloud pubblica di Microsoft Azure per i nostri enterprise e i clienti provider di hosting del servizio.  
  
> [!NOTE]  
> Per ulteriori informazioni su OVSDB, vedere [RFC 7047](http://www.rfc-editor.org/info/rfc7047).  
  
Il commutatore Hyper-V supporta entrambe le regole con stato e senza flusso in base a semplice motore del flusso 'corrisponde azione' all'interno di Microsoft.  
 
![Commutatore Windows Server 2016 Hyper-V](../../../media/what-s-new-in-hyper-v-network-virtualization-in-windows-server/HNVOverview.png)  
  
### <a name="VXLAN"></a>Supporto di incapsulamento VXLAN  
Virtual eXtensible Local Area Network (VXLAN - [7348 RFC](http://www.rfc-editor.org/info/rfc7348)) protocollo è stato ampiamente adottato nel mercato, grazie al supporto di fornitori come Cisco, Brocade, Dell, HP e altri utenti. Virtualizzazione rete ora supporta anche questo schema di incapsulamento utilizza la modalità di distribuzione MAC tramite il Controller di rete Microsoft per il mapping di programma per tenant sovrimpressione indirizzi IP di rete (indirizzo cliente o CA) fisico Metti sotto rete indirizzi IP (indirizzo Provider o PA). NVGRE e VXLAN trasferisce il carico di attività sono supportato per migliorare le prestazioni tramite i driver di terze parti.  
  
### <a name="SLB"></a>Interoperabilità del servizio di bilanciamento carico (SLB) del software  
Windows Server 2016 include un servizio di bilanciamento del carico software (SLB) con il supporto completo per il traffico di rete virtuale e di interagire con virtualizzazione rete. Il SLB è implementata tramite il motore del flusso in dati piano commutatore v ad alte prestazioni e controllata dal Controller di rete per IP virtuale (VIP) / dinamico mapping IP (DIP).  
  
### <a name="L2"></a>Intestazioni Ethernet IEEE conforme  
Virtualizzazione rete implementa le intestazioni Ethernet L2 corrette per garantire l'interoperabilità con dispositivi fisici e virtuali di terze parti che dipendono da protocolli standard del settore. Microsoft garantisce che tutti i pacchetti trasmessi contengono valori conformi in tutti i campi per garantire l'interoperabilità. Inoltre, il supporto per il frame Jumbo (MTU Maximum > 1780) nella rete fisica L2 sarà necessario all'account per i pacchetti sovraccarico introdotti da protocolli encapsulation (NVGRE, VXLAN), assicurando guest macchine virtuali collegate a una rete virtuale di virtualizzazione rete consente di gestire una MTU 1514.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Panoramica di virtualizzazione rete Hyper-V](hyperv-network-virtualization-overview-windows-server.md)  
  
-   [Dettagli tecnici sulla virtualizzazione rete Hyper-V](hyperv-network-virtualization-technical-details-windows-server.md)  
  
-   [Software Defined Networking](../../Software-Defined-Networking--SDN-.md)  
  