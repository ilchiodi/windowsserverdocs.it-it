---
title: Funzionalità e tecnologie per solo software (SO, Software Only)
description: Queste funzionalità vengono implementate come parte del sistema operativo e sono indipendenti dalle schede di interfaccia di rete sottostanti. A volte queste funzionalità richiedono l'ottimizzazione della scheda di interfaccia di rete per un funzionamento ottimale. Esempi di queste funzionalità includono funzionalità Hyper-v, ad esempio la qualità del servizio di macchine virtuali (vmQoS), gli elenchi di controllo di accesso (ACL) e le funzionalità non Hyper-V come gruppo NIC.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/20/2018
ms.openlocfilehash: a83a36ce7a47f0ebde35bf93bdca20796dd37a28
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316887"
---
# <a name="software-only-so-features-and-technologies"></a>Funzionalità e tecnologie per solo software (SO, Software Only)
Le funzionalità di solo software sono implementate come parte del sistema operativo e sono indipendenti dalle schede di interfaccia di rete sottostanti. A volte queste funzionalità richiedono l'ottimizzazione della scheda di interfaccia di rete per un funzionamento ottimale. Esempi di queste funzionalità includono funzionalità Hyper-v, ad esempio la qualità del servizio di macchine virtuali (vmQoS), gli elenchi di controllo di accesso (ACL) e le funzionalità non Hyper-V come gruppo NIC.

## <a name="access-control-lists-acls"></a>Elenchi di controllo di accesso (ACL)

Funzionalità Hyper-V e SDNv1 per la gestione della sicurezza per una macchina virtuale. Questa funzionalità si applica allo stack Hyper-V non virtualizzato e allo stack HVNv1. È possibile gestire gli ACL dei commutiri Hyper-V usando i cmdlet di PowerShell [Add-VMNetworkAdapterAcl](https://docs.microsoft.com/powershell/module/hyper-v/add-vmnetworkadapteracl?view=win10-ps) e [Remove-VMNetworkAdapterAcl](https://docs.microsoft.com/powershell/module/hyper-v/remove-vmnetworkadapteracl?view=win10-ps) .

## <a name="extended-acls"></a>ACL estesi

Gli ACL estesi del Commuter virtuale Hyper-V consentono di configurare gli ACL delle porte estese del Commuter virtuale Hyper-V per garantire la protezione del firewall e applicare criteri di sicurezza per le macchine virtuali tenant nei data center. Poiché gli ACL della porta sono configurati nel Commuter virtuale Hyper-V anziché nelle macchine virtuali, l'amministratore può gestire i criteri di sicurezza per tutti i tenant in un ambiente multi-tenant.

È possibile gestire gli ACL estesi del commutire Hyper-V usando i cmdlet di PowerShell [Add-VMNetworkAdapterExtendedAcl](https://docs.microsoft.com/powershell/module/hyper-v/add-vmnetworkadapterextendedacl?view=win10-ps) e [Remove-VMNetworkAdapterExtendedAcl](https://docs.microsoft.com/powershell/module/hyper-v/remove-vmnetworkadapteracl?view=win10-ps) .

>[!TIP] 
>Questa funzionalità si applica allo stack HNVv1. Per gli ACL nello stack SDN, vedere gli ACL di Software Defined Networking SDN di seguito.

Per altre informazioni sugli elenchi di controllo di accesso di porta estesi in questa libreria, vedere [creare criteri di sicurezza con elenchi di controllo di accesso di porta estesi](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/Create-Security-Policies-with-Extended-Port-Access-Control-Lists).

## <a name="nic-teaming"></a>Gruppo di schede di interfaccia di rete

Gruppo NIC, detto anche collegamento NIC, è l'aggregazione di più porte NIC in un'entità che l'host percepisce come una singola porta NIC. Gruppo NIC protegge da un errore di una singola porta NIC (o dal cavo connesso). Aggrega inoltre il traffico di rete per una maggiore velocità effettiva. Per ulteriori informazioni, vedere [Gruppo NIC](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

Con Windows Server 2016 sono disponibili due modi per eseguire il raggruppamento:

1.  Soluzione di gruppo di Windows Server 2012

2.  Windows Server 2016 switch Embedded Teaming (SET)


## <a name="rsc-in-the-vswitch"></a>RSC in vSwitch

La funzione di Unione del segmento Receive (RSC) in vSwitch è una funzionalità che accetta pacchetti che fanno parte dello stesso flusso e arrivano tra interruzioni di rete e li unisce in un unico pacchetto prima di distribuirli al sistema operativo. Il Commuter virtuale in Windows Server 2019 dispone di questa funzionalità. Per ulteriori informazioni su questa funzionalità, vedere Unione [dei segmenti Receive in vswitch](https://docs.microsoft.com/windows-server/networking/technologies/hpn/rsc-in-the-vswitch).

## <a name="software-defined-networking-sdn-acls"></a>ACL di Software Defined Networking (SDN)

L'estensione SDN in Windows Server 2016 ha migliorato i modi per supportare gli ACL. Nello stack Windows Server 2016 SDN V2 vengono usati gli ACL SDN anziché gli ACL e gli ACL estesi. È possibile usare il controller di rete per gestire gli ACL SDN. 

## <a name="sdn-quality-of-service-qos"></a>QoS (Quality of Service) SDN

L'estensione SDN in Windows Server 2016 ha migliorato i modi per fornire il controllo della larghezza di banda (prenotazioni in uscita, limiti in uscita e limiti di ingresso) in base a 5 tuple. In genere, questi criteri vengono applicati a livello di vNIC o scheda, ma è possibile renderli molto più specifici. Nello stack di Windows Server 2016 SDN V2, viene usato SDN QoS anziché vmQoS. È possibile usare il controller di rete per gestire QoS SDN.

## <a name="switch-embedded-teaming-set"></a>Switch Embedded Teaming (SET)

SET è una soluzione di gruppo NIC alternativa che è possibile usare in ambienti che includono Hyper-V e lo stack SDN (Software Defined Networking) in Windows Server 2016. SET integra alcune funzionalità di gruppo NIC nel Commuter virtuale Hyper-V. Per informazioni su switch Embedded Teaming in questa libreria, vedere [accesso diretto a memoria remota (RDMA) e switch Embedded Teaming (set)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming).

## <a name="virtual-receive-side-scaling-vrss"></a>Virtual Receive Side Scaling (vRSS)

Il software vRSS viene usato per distribuire il traffico in ingresso destinato a una macchina virtuale in più processori logici (LPs) della macchina virtuale. VRSS software offre alla VM la possibilità di gestire più traffico di rete di quanto un singolo LP possa gestire. Per ulteriori informazioni, vedere [Virtual Receive Side Scaling (vRSS)](https://docs.microsoft.com/windows-server/networking/technologies/vrss/vrss-top).

## <a name="virtual-machine-quality-of-service-vmqos"></a>Qualità del servizio della macchina virtuale (vmQoS)

La qualità del servizio della macchina virtuale è una funzionalità di Hyper-V che consente all'opzione di impostare limiti per il traffico generato da ogni macchina virtuale. Consente inoltre a una macchina virtuale di riservare una quantità di larghezza di banda nella connessione di rete esterna, in modo che una macchina virtuale non riesca a morire per la larghezza di banda. Nello stack di Windows Server 2016 SDN V2, SDN QoS sostituisce vmQoS.

vmQoS può impostare limiti in uscita e prenotazioni in uscita. Prima di creare il commutire Hyper-V, è necessario determinare la modalità di prenotazione in uscita (peso relativo o larghezza di banda assoluta).

-  Determinare la modalità di prenotazione in uscita con il parametro – MinimumBandwidthMode del cmdlet di PowerShell New-VMSwitch.

-  Impostare il valore del limite in uscita con il parametro – MaximumBandwidth nel cmdlet di PowerShell set-VMNetworkAdapter.

-  Impostare il valore per la prenotazione in uscita con uno dei seguenti parametri del cmdlet Set VMNetworkAdapter di PowerShell:

   -  Se il parametro – MinimumBandwidthMode nel cmdlet New-VMSwitch è Absolute, impostare il parametro – MinimumBandwidthAbsolute nel cmdlet Set VMNetworkAdapter.

   -  Se il parametro – MinimumBandwidthMode nel cmdlet New-VMSwitch è Weight, impostare il parametro – I parametri minimumbandwidthweight nel cmdlet Set VMNetworkAdapter.

A causa delle limitazioni nell'algoritmo usato per questa funzionalità, è consigliabile che il peso massimo o la larghezza di banda assoluta non siano superiori a 20 volte il peso minimo o la larghezza di banda assoluta. Se è necessario un maggiore controllo, provare a usare lo stack SDN e la funzionalità SDN-QoS.


---