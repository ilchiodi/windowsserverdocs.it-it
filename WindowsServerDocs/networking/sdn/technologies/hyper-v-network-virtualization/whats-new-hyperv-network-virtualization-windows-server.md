---
title: Novità di virtualizzazione rete Hyper-V in Windows Server 2016
description: In questo argomento vengono fornite informazioni sulle nuove funzionalità di virtualizzazione rete Hyper-V in Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0254275a-0a77-40a9-b68a-1029284c03fe
ms.author: pashort
author: shortpatti
ms.date: 03/19/2018
ms.openlocfilehash: 57db82fdd8c7524afb427c61f754e9b8ede8e7b7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355670"
---
# <a name="whats-new-in-hyper-v-network-virtualization-in-windows-server-2016"></a>Novità di virtualizzazione rete Hyper-V in Windows Server 2016

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo argomento vengono descritte le funzionalità di virtualizzazione rete Hyper-V (HNV) nuove o modificate in Windows Server 2016.  
  
## <a name="BKMK_IPAM2012R2"></a>Aggiornamenti in HNV  
HNV offre supporto avanzato nelle aree seguenti:  
  
|Caratteristica/funzionalità|Novità o miglioramento|Descrizione|  
|--------------------------|-------------------|---------------|  
|[Commutire programmabile Hyper-V](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#SDN)|Nuova|Il criterio HNV è programmabile tramite il controller di rete Microsoft.|  
|[Supporto di incapsulamento VXLAN](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#VXLAN)|Nuova|HNV supporta ora l'incapsulamento di VXLAN.|  
|[Interoperabilità con Load Balancer software (SLB)](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#SLB)|Nuova|HNV è completamente integrato con il software Microsoft Load Balancer.|  
|[Intestazioni IEEE Ethernet conformi](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#L2)|Miglioramento|Conforme agli standard IEEE Ethernet|  
  
### <a name="SDN"></a>Commutire programmabile Hyper-V  
HNV è un blocco predefinito fondamentale della soluzione SDN (Software Defined Networking) aggiornata di Microsoft ed è completamente integrato nello stack SDN.  
  
Il nuovo controller di rete Microsoft effettua il push dei criteri di HNV a un agente host in esecuzione in ogni host usando il protocollo OVSDB (Open vSwitch Database Management Protocol) come interfaccia di sud (SBI). L'agente host archivia questi criteri usando una personalizzazione dello [schema VTEP](https://github.com/openvswitch/ovs/blob/master/vtep/vtep.ovsschema) e le regole di flusso complesse programmi in un motore di flusso a prestazioni elevate nel commutire Hyper-V.  
  
Il motore di flusso all'interno del Commuter Hyper-V è lo stesso motore usato in Microsoft Azure @ no__t-0, che è stato testato su larga scala nel cloud Microsoft Azure pubblico. Inoltre, l'intero stack SDN attraverso il controller di rete e il provider di risorse di rete (informazioni presto disponibili) sono coerenti con Microsoft Azure, in modo da offrire la potenza del Microsoft Azure cloud pubblico all'azienda e al servizio di hosting clienti del provider.  
  
> [!NOTE]  
> Per ulteriori informazioni su OVSDB, vedere la [specifica RFC 7047](https://www.rfc-editor.org/info/rfc7047).  
  
Il Commuter Hyper-V supporta regole di flusso con e senza stato in base alla semplice "azione di corrispondenza" all'interno del motore Microsoft Flow.  
 
![Switch Hyper-V di Windows Server 2016](../../../media/what-s-new-in-hyper-v-network-virtualization-in-windows-server/HNVOverview.png)  
  
### <a name="VXLAN"></a>Supporto di incapsulamento VXLAN  
Il protocollo VXLAN- [RFC 7348](https://www.rfc-editor.org/info/rfc7348)(Virtual Extensible Local Area Network) è stato ampiamente adottato sul mercato, con supporto da fornitori come Cisco, broccato, Dell, HP e altri. HNV supporta ora questo schema di incapsulamento usando la modalità di distribuzione MAC tramite il controller di rete Microsoft per programmare i mapping per gli indirizzi IP della rete sovrapposta ai tenant (indirizzo del cliente o CA) per gli indirizzi IP di rete sottoposti a sottofondo fisico (provider Indirizzo o PA. Gli offload delle attività NVGRE e VXLAN sono supportati per migliorare le prestazioni tramite driver di terze parti.  
  
### <a name="SLB"></a>Interoperabilità con Load Balancer software (SLB)  
Windows Server 2016 include un servizio di bilanciamento del carico software (SLB) con supporto completo per il traffico di rete virtuale e l'interazione senza problemi con HNV. SLB viene implementato tramite il motore di flusso ad esecuzione nell'opzione v del piano dati e controllato dal controller di rete per i mapping IP virtuali (VIP)/IP dinamico (DIP).  
  
### <a name="L2"></a>Intestazioni IEEE Ethernet conformi  
HNV implementa le intestazioni Ethernet L2 corrette per garantire l'interoperabilità con appliance virtuali e fisiche di terze parti che dipendono da protocolli standard del settore. Microsoft garantisce che tutti i pacchetti trasmessi abbiano valori conformi in tutti i campi per garantire l'interoperabilità. Inoltre, il supporto per i frame Jumbo (MTU > 1780) nella rete L2 fisica sarà necessario per tenere conto dell'overhead dei pacchetti introdotto dai protocolli di incapsulamento (NVGRE, VXLAN), assicurando al contempo le macchine virtuali guest collegate a una rete virtuale HNV MTU 1514.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Panoramica di Virtualizzazione rete Hyper-V](hyperv-network-virtualization-overview-windows-server.md)  
  
-   [Dettagli tecnici di Virtualizzazione rete Hyper-V](hyperv-network-virtualization-technical-details-windows-server.md)  
  
-   [SDN (Software Defined Networking)](../../Software-Defined-Networking--SDN-.md)  
  