---
title: Configurare qualità del servizio (QoS) per una scheda di rete VM tenant
description: Quando si configura QoS per una scheda di rete della macchina virtuale tenant, è possibile scegliere tra i Data Center Bridging \(DCB\)o Software Defined Networking \(SDN\) QoS.
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
ms.openlocfilehash: 0b9ce318c3d249b23d7560e0b6bb90a83e60d64d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880602"
---
# <a name="configure-quality-of-service-qos-for-a-tenant-vm-network-adapter"></a>Configurare qualità del servizio (QoS) per una scheda di rete VM tenant

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Quando si configura QoS per una scheda di rete della macchina virtuale tenant, è possibile scegliere tra i Data Center Bridging \(DCB\)o Software Defined Networking \(SDN\) QoS.

1.  **DCB**. È possibile configurare DCB utilizzando i cmdlet di Windows PowerShell NetQoS. Per un esempio, vedere la sezione "Abilitare Data Center Bridging" nell'argomento [diretta accesso memoria remota (RDMA) e Switch Embedded Teaming (SET)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

2.  **QoS di rete SDN**. È possibile abilitare QoS di rete SDN usando Controller di rete, che può essere impostata per limitare la larghezza di banda in un'interfaccia virtuale per impedire che una VM con traffico elevato blocchi ad altri utenti.  È anche possibile configurare QoS di rete SDN per riservare una quantità di larghezza di banda per una macchina virtuale per assicurarsi che sia accessibile indipendentemente dalla quantità di traffico di rete della macchina virtuale specifica.  

Applica tutte le impostazioni QoS di rete SDN tramite le impostazioni della porta delle proprietà di interfaccia di rete. Fare riferimento alla tabella di seguito per altri dettagli.

|Nome elemento|Descrizione|
|------------|-----------| 
|macSpoofing| Consente alle macchine virtuali modificare il controllo di accesso di origine multimediali \(MAC\) indirizzo pacchetti in uscita a un indirizzo MAC non è stato assegnato alla macchina virtuale.<p>Valori consentiti:<ul><li>Abilitata: usare un indirizzo MAC diverso.</li><li>Disabilitata: usare solo l'indirizzo MAC assegnato a esso.</li></ul>|
|arpGuard| Consente di guard ARP solo gli indirizzi specificati in ArpFilter passare attraverso la porta.<p>Valori consentiti:<ul><li>Abilitato: consentito</li><li>Disabilitata: non consentito</li></ul>|
|dhcpGuard| Consente di o Elimina tutti i messaggi DHCP da una macchina virtuale che dichiara di essere un server DHCP. <p>Valori consentiti:<ul><li>Abilitata: Elimina i messaggi DHCP perché il server DHCP virtualizzato è considerato non attendibile.</li><li>Disabilitata: consente il messaggio da ricevere perché il server DHCP virtualizzato è considerato attendibile.</li></ul>|
|stormLimit| Il numero di pacchetti (broadcast, multicast e unicast sconosciuta) al secondo una macchina virtuale è consentito per l'invio tramite la scheda di rete virtuale. Vengono rilasciati pacchetti oltre il limite durante l'intervallo di 1 secondo. Un valore pari a zero \(0\) non esiste alcun limite...|
|portFlowLimit| Il numero massimo di flussi possa essere eseguito per la porta. Un valore blank o zero \(0\) non esiste alcun limite. |
|vmqWeight| Il peso relativo descrive l'affinità della scheda di rete virtuale da usare coda macchine virtuali (VMQ). L'intervallo del valore va da 0 a 100.<p>Valori consentiti:<ul><li>0: disabilita la funzionalità coda macchine Virtuali nella scheda di rete virtuale.</li><li>1-100 – abilita la funzionalità coda macchine Virtuali nella scheda di rete virtuale.</li></ul>|
|iovWeight| Il peso relativo imposta l'affinità della scheda di rete virtuale per la virtualizzazione dei / o single-root assegnata \(SR-IOV\) funzione virtuale. <p>Valori consentiti:<ul><li>0 – disabilita SR-IOV nella scheda di rete virtuale.</li><li>1-100 – Abilita SR-IOV nella scheda di rete virtuale.</li></ul>|
|iovInterruptModeration|<p>Valori consentiti:<ul><li>predefinito: impostazione del fornitore scheda rete fisica determina il valore.</li><li>adattivo: </li><li>Disattivato </li><li>Bassa</li><li>media</li><li>Elevata</li></ul><p>Se si sceglie **predefinito**, impostazione del fornitore scheda rete fisica determina il valore.  Se si sceglie, **adattivo**, il traffico di runtime modello determina la frequenza di regolazione di interrupt.|
|iovQueuePairsRequested| Il numero di coppie di code hardware allocate a una funzione virtuale SR-IOV. Se il receive-side scaling \(RSS\) è obbligatorio e se la scheda di rete fisica che viene associata al commutatore virtuale supporta RSS in funzioni virtuali SR-IOV, sarà necessaria più di una coppia di coda. <p>Valori consentiti: tra 1 e 4294967295.|
|QosSettings| Configurare le impostazioni Qos seguenti, ognuno dei quali sono facoltativi: <ul><li>**outboundReservedValue** : se il valore indica la larghezza di banda in Mbps, sicuramente la porta virtuale per la trasmissione (traffico in uscita) è "assoluto" outboundReservedMode. Se outboundReservedMode è "peso" il valore indica la porzione ponderata della larghezza di banda garantito.</li><li>**outboundMaximumMbps** -indica il numero massimo consentito di larghezza di banda lato trasmissione, in Mbps, per la porta virtuale (traffico in uscita).</li><li>**InboundMaximumMbps** -indica il numero massimo consentito della larghezza di banda lato di ricezione per la porta virtuale (traffico in ingresso) in Mbps.</li></ul> |

---