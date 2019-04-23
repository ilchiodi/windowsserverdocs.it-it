---
title: Che cosa sono le novità di virtualizzazione rete Hyper-V in Windows Server 2016
description: Questo argomento vengono fornite informazioni sulle nuove funzionalità di virtualizzazione rete Hyper-V in Windows Server 2016
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
ms.openlocfilehash: 24ec9e52be3acdfced35eae4fb5f98f16d8e18f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837952"
---
# <a name="whats-new-in-hyper-v-network-virtualization-in-windows-server-2016"></a>Che cosa sono le novità di virtualizzazione rete Hyper-V in Windows Server 2016

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Questo argomento descrive le funzionalità di Hyper-V rete virtualizzazione nuove o modificate in Windows Server 2016.  
  
## <a name="BKMK_IPAM2012R2"></a>Aggiornamenti in virtualizzazione rete  
Virtualizzazione rete offre supporto avanzato nelle aree seguenti:  
  
|Caratteristica/funzionalità|Novità o miglioramento|Descrizione|  
|--------------------------|-------------------|---------------|  
|[Programmable commutatore Hyper-V](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#SDN)|Nuova|Criteri HNV sono programmabili tramite il Controller di rete Microsoft.|  
|[Supporto di incapsulamento VXLAN](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#VXLAN)|Nuova|Virtualizzazione rete supporta ora l'incapsulamento VXLAN.|  
|[Interoperabilità di Load Balancer (SLB) del software](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#SLB)|Nuova|Virtualizzazione rete è completamente integrato con bilanciamento del carico Software Microsoft.|  
|[Intestazioni Ethernet IEEE conforme](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#L2)|Miglioramento|Conforme agli standard IEEE Ethernet|  
  
### <a name="SDN"></a>Programmable commutatore Hyper-V  
Virtualizzazione rete è un blocco predefinito fondamentale della soluzione di rete SDN (Software Defined) aggiornata di Microsoft ed è completamente integrato nello stack SDN.  
  
Il nuovo Controller di rete di Microsoft effettua il push dei criteri di virtualizzazione rete fino a un agente Host in esecuzione in ogni host usando vSwitch aprire Database Management Protocol (OVSDB) come l'interfaccia SouthBound (SBI). L'agente Host archivia questi criteri usando una personalizzazione della [schema VTEP](https://github.com/openvswitch/ovs/blob/master/vtep/vtep.ovsschema) e i programmi di regole di flusso complessi in un motore di flusso ad alte prestazioni nel commutatore Hyper-V.  
  
Il motore del flusso all'interno del commutatore Hyper-V è lo stesso motore utilizzato in Microsoft Azure&trade;, che ha già dimostrato su vasta scala nel cloud pubblico di Microsoft Azure. Inoltre, l'intero stack SDN backup tramite il Controller di rete e il Provider di risorse di rete (dettagli sarà presto disponibile) sia coerenza con Microsoft Azure, garantendo la potenza del cloud pubblico di Microsoft Azure per l'azienda e servizio di hosting clienti di provider.  
  
> [!NOTE]  
> Per altre informazioni sulle OVSDB, vedere [RFC 7047](https://www.rfc-editor.org/info/rfc7047).  
  
Il commutatore Hyper-V supporta entrambe le regole del flusso con e senza state basate sul motore di flusso 'azione corrispondere a quella' all'interno di Microsoft semplice.  
 
![Commutatore Windows Server 2016 Hyper-V](../../../media/what-s-new-in-hyper-v-network-virtualization-in-windows-server/HNVOverview.png)  
  
### <a name="VXLAN"></a>Supporto di incapsulamento VXLAN  
Virtual eXtensible LAN (VXLAN - [7348 RFC](https://www.rfc-editor.org/info/rfc7348)) protocollo è stato ampiamente adottato nel mercato, grazie al supporto di fornitori come Cisco, Brocade, Dell, HP e altri utenti. Virtualizzazione rete supporta ora anche in questo schema incapsulamento utilizza la modalità di distribuzione MAC tramite il Controller di rete Microsoft per i mapping di programma per tenant sovrimpressione indirizzi IP di rete (indirizzo cliente o autorità di certificazione) per il fisica Metti sotto indirizzi IP di rete (Provider Indirizzo o PA). NVGRE e VXLAN trasferisce il carico di attività sono supportato per prestazioni migliori attraverso i driver di terze parti.  
  
### <a name="SLB"></a>Interoperabilità di Load Balancer (SLB) del software  
Windows Server 2016 include un servizio di bilanciamento del carico software (SLB) con supporto completo per il traffico di rete virtuale e un'agevole interazione con virtualizzazione rete. Il bilanciamento del carico software viene implementata tramite il motore del flusso ad alte prestazioni nel piano dati v-commutatore e controllato dal Controller di rete per IP virtuale (VIP) / dinamico mapping IP (DIP).  
  
### <a name="L2"></a>Intestazioni Ethernet IEEE conforme  
Virtualizzazione rete implementa intestazioni Ethernet L2 corrette per garantire l'interoperabilità con le Appliance virtuali e fisiche terze parti che dipendono da protocolli standard del settore. Microsoft garantisce che tutti i pacchetti trasmessi abbiano valori conformi a tutti i campi per garantire l'interoperabilità. Inoltre, il supporto per il frame jumbo nell'ambito (MTU > 1780) nella rete L2 fisica verrà richiesto di account per pacchetto overhead introdotto dai protocolli encapsulation (NVGRE, VXLAN), garantendo guest mantenere le macchine virtuali collegate a una rete virtuale di virtualizzazione rete un IL VALORE MTU 1514.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Panoramica di Virtualizzazione rete Hyper-V](hyperv-network-virtualization-overview-windows-server.md)  
  
-   [Dettagli tecnici di Virtualizzazione rete Hyper-V](hyperv-network-virtualization-technical-details-windows-server.md)  
  
-   [Software Defined Networking](../../Software-Defined-Networking--SDN-.md)  
  