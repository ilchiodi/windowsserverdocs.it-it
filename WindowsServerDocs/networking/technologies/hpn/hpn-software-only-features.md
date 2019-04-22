---
title: Funzionalità e tecnologie per solo software (SO, Software Only)
description: Queste funzionalità vengono implementate come parte del sistema operativo e sono indipendenti tra le schede di rete sottostante. A volte queste funzionalità richiedono alcune ottimizzazione dell'interfaccia di rete per un funzionamento ottimale. Esempi di queste funzionalità di Hyper-v, ad esempio la qualità del servizio (vmQoS) di macchina virtuale, elenchi di controllo di accesso (ACL) e le funzionalità di Hyper-V come gruppo NIC.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/20/2018
ms.openlocfilehash: 504bc92971e778b468812dc4064fa6f0afff87ad
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823782"
---
# <a name="software-only-so-features-and-technologies"></a>Funzionalità e tecnologie per solo software (SO, Software Only)
Solo le funzionalità di software vengono implementate come parte del sistema operativo e sono indipendenti tra le schede di rete sottostante. A volte queste funzionalità richiedono alcune ottimizzazione dell'interfaccia di rete per un funzionamento ottimale. Esempi di queste funzionalità di Hyper-v, ad esempio la qualità del servizio (vmQoS) di macchina virtuale, elenchi di controllo di accesso (ACL) e le funzionalità di Hyper-V come gruppo NIC.

## <a name="access-control-lists-acls"></a>Elenchi di controllo di accesso (ACL)

Una funzionalità di Hyper-V e SDNv1 per la gestione della sicurezza per una macchina virtuale. Questa caratteristica è applicabile per lo stack di Hyper-V non virtualizzato e lo stack HVNv1. È possibile gestire Hyper-V passare tramite ACL [Add-VMNetworkAdapterAcl](https://docs.microsoft.com/powershell/module/hyper-v/add-vmnetworkadapteracl?view=win10-ps) e [Remove-VMNetworkAdapterAcl](https://docs.microsoft.com/powershell/module/hyper-v/remove-vmnetworkadapteracl?view=win10-ps) i cmdlet di PowerShell.

## <a name="extended-acls"></a>ACL estesi

Commutatore virtuale Hyper-V gli ACL estesi consente di configurare Hyper-V Virtual Switch porta gli ACL estesi per offrire protezione firewall e applicare criteri di sicurezza per le macchine virtuali tenant nei Data Center. Poiché l'ACL delle porte sono configurati nel commutatore virtuale Hyper-V anziché nelle macchine virtuali, l'amministratore può gestire i criteri di sicurezza per tutti i tenant in un ambiente multi-tenant.

È possibile gestire il commutatore Hyper-V gli ACL tramite esteso il [Add-VMNetworkAdapterExtendedAcl](https://docs.microsoft.com/powershell/module/hyper-v/add-vmnetworkadapterextendedacl?view=win10-ps) e [Remove-VMNetworkAdapterExtendedAcl](https://docs.microsoft.com/powershell/module/hyper-v/remove-vmnetworkadapteracl?view=win10-ps) i cmdlet di PowerShell.

>[!TIP] 
>Questa funzionalità si applica allo stack di HNVv1. Per gli ACL nello stack SDN, fare riferimento alla SDN Software Defined Networking) elenchi ACL riportato di seguito.

Per ulteriori informazioni estesa porta elenchi di controllo in questa libreria, vedere [creare i criteri di sicurezza con estesa porta controllo elenchi di accesso](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/Create-Security-Policies-with-Extended-Port-Access-Control-Lists).

## <a name="nic-teaming"></a>Gruppo NIC

Gruppo NIC, detto anche associazione NIC, rappresenta l'aggregazione di più porte di interfaccia di rete in un'entità che host percepisce come una singola porta di interfaccia di rete. Gruppo NIC consente di proteggere il guasto di una singola porta di interfaccia di rete (o il cavo sia connesso a esso). Aggrega anche il traffico di rete per una velocità effettiva. Per altre informazioni, vedere [NIC Teaming](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

Con Windows Server 2016 sono disponibili due modi per eseguire operazioni di raggruppamento:

1.  Soluzione gruppo NIC di Windows Server 2012

2.  Windows Server 2016 Switch Embedded Teaming (SET)


## <a name="rsc-in-the-vswitch"></a>RSC nel commutatore virtuale

Ricezione segmento Coalescing (RSC) nel commutatore virtuale è una funzionalità che richiede i pacchetti che fanno parte dello stesso flusso e di arrivano tra interruzioni di rete e li assegna un singolo pacchetto prima di inviarli al sistema operativo. Il commutatore virtuale in Windows Server 2019 ha questa funzionalità. Per altre informazioni su questa funzionalità, vedere [ricezione unione segmenti di vSwitch](https://docs.microsoft.com/windows-server/networking/technologies/hpn/rsc-in-the-vswitch).

## <a name="software-defined-networking-sdn-acls"></a>Software Defined Networking (SDN) ACL

L'estensione di SDN in Windows Server 2016 migliorata modi per supportare gli ACL. Nello stack v2 di Windows Server 2016 SDN, vengono usati gli ACL di rete SDN anziché gli ACL e ACL estesi. È possibile utilizzare Controller di rete per gestire gli ACL di rete SDN. 

## <a name="sdn-quality-of-service-qos"></a>SDN Quality of Service (QoS)

L'estensione SDN in Windows Server 2016 migliorata modi per fornire il controllo della larghezza di banda (prenotazioni in uscita, limiti di uscita e i limiti di ingresso) in base a una tupla con 5 elementi. In genere, questi criteri vengono applicati a livello di scheda di rete virtuale o vmNIC, ma è possibile renderli molto più specifico. Nello stack v2 di Windows Server 2016 SDN, anziché vmQoS viene utilizzato QoS di rete SDN. È possibile utilizzare Controller di rete per la gestione di QoS di rete SDN.

## <a name="switch-embedded-teaming-set"></a>Switch Embedded Teaming (SET)

SET è una soluzione alternativa gruppo NIC che è possibile usare in ambienti che includono Hyper-V e lo stack di rete SDN (Software Defined) in Windows Server 2016. SET integra alcune funzionalità gruppo NIC nel commutatore virtuale Hyper-V. Per informazioni su Switch Embedded Teaming in questa libreria, vedere [diretta accesso memoria remota (RDMA) e Switch Embedded Teaming (SET)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming).

## <a name="virtual-receive-side-scaling-vrss"></a>Virtual Receive Side Scaling (vRSS)

VRSS software viene usato per distribuire il traffico in arrivo destinato a una macchina virtuale tra più processori logici (LPs) della macchina virtuale. VRSS software offre la VM sarà in grado di gestire la capacità di gestire più traffico di rete rispetto a un singolo LP. Per altre informazioni, vedere [Virtual Receive-Side Scaling (vRSS)](https://docs.microsoft.com/windows-server/networking/technologies/vrss/vrss-top).

## <a name="virtual-machine-quality-of-service-vmqos"></a>Virtual Machine Quality of Service (vmQoS)

Qualità del servizio macchina virtuale è una funzionalità di Hyper-V che consente il passaggio a impostare i limiti per il traffico generato da ogni macchina virtuale. Consente inoltre una macchina virtuale di riservare una quantità di larghezza di banda per la connessione di rete esterna, in modo che una macchina virtuale non può sottrarre risorse a un'altra macchina virtuale per la larghezza di banda. Nello stack v2 di Windows Server 2016 SDN, QoS SDN sostituisce vmQoS.

vmQoS possibile impostare limiti di uscita e le prenotazioni di traffico in uscita. Prima di creare il commutatore Hyper-V, è necessario determinare la modalità di prenotazione in uscita (peso relativo o assoluto della larghezza di banda).

-  Determinare la modalità di prenotazione in uscita con il parametro – MinimumBandwidthMode del cmdlet di PowerShell New-VMSwitch.

-  Impostare il valore del limite per il traffico in uscita con il parametro – MaximumBandwidth nel cmdlet di PowerShell Set-VMNetworkAdapter.

-  Impostare il valore per la prenotazione in uscita con uno dei seguenti parametri del cmdlet di VMNetworkAdapter PowerShell impostare:

   -  Se il parametro – MinimumBandwidthMode sul cmdlet New-VMSwitch è assoluto, quindi impostare il parametro – MinimumBandwidthAbsolute sul cmdlet VMNetworkAdapter impostato.

   -  Se il parametro – MinimumBandwidthMode sul cmdlet New-VMSwitch peso, quindi impostare il parametro – i parametri MinimumBandwidthWeight sul cmdlet VMNetworkAdapter impostato.

A causa delle limitazioni nell'algoritmo utilizzato per questa funzionalità, è consigliabile che il peso più alto o la larghezza di banda assoluta non più di 20 volte il peso più basso o della larghezza di banda assoluta. Se è necessario un maggiore controllo, è consigliabile usare lo stack SDN e la funzionalità QoS di rete SDN.


---