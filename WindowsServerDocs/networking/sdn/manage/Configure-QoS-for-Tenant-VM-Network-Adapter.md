---
title: Configurare la qualità del servizio (QoS) per una scheda di rete della macchina virtuale tenant
description: Quando si configura QoS per una scheda di rete della macchina virtuale tenant, è possibile scegliere tra Data Center bridging DCB o software defined networking Sdn QoS.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6d783ff6-7dd5-496c-9ed9-5c36612c6859
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 99ef286b91bec4bcb008bfd9f62003e75a5a5921
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870023"
---
# <a name="configure-quality-of-service-qos-for-a-tenant-vm-network-adapter"></a>Configurare la qualità del servizio (QoS) per una scheda di rete della macchina virtuale tenant

>Si applica a Windows Server (Canale semestrale), Windows Server 2016

Quando si configura QoS per una scheda di rete della macchina virtuale tenant, è possibile scegliere tra Data Center \(bridging\)DCB o software defined \(networking\) Sdn QoS.

1.  **DCB**. È possibile configurare DCB usando i cmdlet di Windows PowerShell NetQoS. Per un esempio, vedere la sezione "abilitare il Data Center Bridging" nell'argomento [accesso diretto a memoria remota (RDMA) e switch Embedded Teaming (set)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

2.  **QoS Sdn**. È possibile abilitare la QoS SDN usando il controller di rete, che può essere impostato per limitare la larghezza di banda in un'interfaccia virtuale per impedire a una VM con traffico elevato di bloccare altri utenti.  È anche possibile configurare SDN QoS per riservare una quantità di larghezza di banda specifica per una macchina virtuale per garantire che la macchina virtuale sia accessibile indipendentemente dalla quantità di traffico di rete.  

Applicare tutte le impostazioni QoS SDN tramite le impostazioni della porta delle proprietà dell'interfaccia di rete. Per ulteriori informazioni, fare riferimento alla tabella riportata di seguito.

|Nome dell'elemento|Descrizione|
|------------|-----------| 
|macSpoofing| Consente alle macchine virtuali di modificare l' \(origine\) Media Access Control indirizzo Mac nei pacchetti in uscita in un indirizzo Mac non assegnato alla macchina virtuale.<p>Valori consentiti:<ul><li>Abilitato: usare un indirizzo MAC diverso.</li><li>Disabilitato: usare solo l'indirizzo MAC assegnato.</li></ul>|
|arpGuard| Consente di passare attraverso la porta solo gli indirizzi ARP Guard specificati in ArpFilter.<p>Valori consentiti:<ul><li>Abilitato-consentito</li><li>Disabled-non consentito</li></ul>|
|Controllo DHCP| Consente o Elimina tutti i messaggi DHCP da una macchina virtuale che dichiara di essere un server DHCP. <p>Valori consentiti:<ul><li>Enabled: Elimina i messaggi DHCP perché il server DHCP virtualizzato è considerato non attendibile.</li><li>Disabled: consente la ricezione del messaggio perché il server DHCP virtualizzato è considerato attendibile.</li></ul>|
|stormLimit| Il numero di pacchetti (broadcast, multicast e unicast sconosciuti) al secondo che una macchina virtuale può inviare tramite la scheda di rete virtuale. I pacchetti oltre il limite durante l'intervallo di un secondo vengono eliminati. Un valore pari a \(zero\) 0 indica che non esiste alcun limite.|
|portFlowLimit| Numero massimo di flussi che è consentito eseguire per la porta. Il valore blank o zero \(0\) indica che non esiste alcun limite. |
|vmqWeight| Il peso relativo descrive l'affinità della scheda di rete virtuale per l'utilizzo della coda della macchina virtuale (VMQ). L'intervallo di valori è compreso tra 0 e 100.<p>Valori consentiti:<ul><li>0: Disabilita VMQ sulla scheda di rete virtuale.</li><li>1-100: Abilita VMQ sulla scheda di rete virtuale.</li></ul>|
|iovWeight| Il peso relativo imposta l'affinità della scheda di rete virtuale sulla funzione virtuale Single-Root I/O Virtualization \(SR-IOV\) assegnata. <p>Valori consentiti:<ul><li>0: Disabilita SR-IOV sulla scheda di rete virtuale.</li><li>1-100: Abilita SR-IOV sulla scheda di rete virtuale.</li></ul>|
|iovInterruptModeration|<p>Valori consentiti:<ul><li>impostazione predefinita: l'impostazione del fornitore della scheda di rete fisica determina il valore.</li><li>Adaptive </li><li>Disattivato </li><li>Basso</li><li>media</li><li>Alta</li></ul><p>Se si sceglie **predefinito**, l'impostazione del fornitore della scheda di rete fisica determina il valore.  Se si sceglie, **Adaptive**, il modello di traffico di runtime determina il tasso di moderazione dell'interrupt.|
|iovQueuePairsRequested| Numero di coppie di code hardware allocate a una funzione virtuale SR-IOV. Se il RSS\) Receive- \(Side Scaling è necessario e se la scheda di rete fisica associata al commutere virtuale supporta RSS nelle funzioni virtuali SR-IOV, sono necessarie più coppie di code. <p>Valori consentiti: da 1 a 4294967295.|
|QosSettings| Configurare le impostazioni QoS seguenti, tutte facoltative: <ul><li>**outboundReservedValue** -se outboundReservedMode è "Absolute", il valore indica la larghezza di banda, in Mbps, garantita alla porta virtuale per la trasmissione (in uscita). Se outboundReservedMode è "weight", il valore indica la porzione ponderata della larghezza di banda garantita.</li><li>**outboundMaximumMbps** : indica la larghezza di banda di invio massima consentita, in Mbps, per la porta virtuale (in uscita).</li><li>**InboundMaximumMbps** : indica la larghezza di banda di ricezione massima consentita per la porta virtuale (ingresso) in Mbps.</li></ul> |

---