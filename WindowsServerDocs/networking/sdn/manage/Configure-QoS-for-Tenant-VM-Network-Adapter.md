---
title: Configurare qualità del servizio (QoS) per una scheda di rete di macchina virtuale Tenant
description: Questo argomento fa parte della Guida alla rete definita dal Software su come gestire carichi di lavoro Tenant e reti virtuali in Windows Server 2016.
manager: brianlic
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
ms.openlocfilehash: cde4e21beaec58a98a5d5fbe5c4631e2f113dbf7
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="configure-quality-of-service-qos-for-a-tenant-vm-network-adapter"></a>Configurare qualità del servizio (QoS) per una scheda di rete di macchina virtuale Tenant

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Quando si configura QoS per una scheda di rete di macchina virtuale tenant, è possibile scegliere tra Data Center Bridging \ (DCB\) o rete definita dal Software \(SDN\) QoS.

1.  **DCB**. È possibile configurare DCB utilizzando i cmdlet di Windows PowerShell NetQoS. Per un esempio, vedere la sezione "Abilitare Data Center Bridging" nell'argomento [diretta accesso memoria remota (RDMA) e Switch Embedded Teaming (SET)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

2.  **QoS SDN**. È possibile abilitare QoS SDN con Controller di rete, che può essere impostato per limitare la larghezza di banda su un'interfaccia virtuale per impedire che una macchina virtuale di traffico elevato impedire ad altri utenti.  È inoltre possibile configurare QoS SDN per riservare una quantità di larghezza di banda per una macchina virtuale garantire che la macchina virtuale sia accessibile a prescindere dalla quantità di traffico di rete specifica.  

Tutte le impostazioni Qos SDN vengono applicate tramite le impostazioni della porta le proprietà di interfaccia di rete in base alla tabella seguente.

|Nome elemento|Descrizione|
|------------|-----------| 
|macSpoofing|Specifica se le macchine virtuali possono modificare l'indirizzo origine media access control \(MAC\) pacchetti in uscita a un indirizzo MAC non assegnato alla macchina virtuale. I valori sono "attivati" è consentito \ (consentendo la macchina virtuale da utilizzare un diverso indirizzo MAC) e "disabled" \ (consentendo la macchina virtuale da utilizzare solo l'indirizzo MAC assegnato alla it\).|
|arpGuard|Specifica se è abilitato guard ARP.  Guard ARP consente solo indirizzi specificati nel ArpFilter di passare attraverso la porta.  I valori consentiti sono "attivati" o "disabled".
|Controllo DHCP|Specifica se eliminare i messaggi DHCP da una macchina virtuale che dichiara l'appartenenza a un server DHCP. I valori consentiti sono "attivati", che elimina i messaggi DHCP perché il server DHCP virtualizzato è considerato non attendibile, o "disabled", che consente il messaggio di essere ricevuto perché il server DHCP virtualizzato è considerato attendibile.
|stormLimit|Specifica il numero di pacchetti di broadcast, multicast e unicast sconosciuta che al secondo che una macchina virtuale è autorizzata a inviare tramite la scheda di rete virtuale specificata. Pacchetti di broadcast, multicast e unicast sconosciuta che superano il limite durante tale intervallo di un secondo vengono eliminati. Il valore zero \(0\) indica che non esiste alcun limite.
|portFlowLimit|Specifica il numero massimo di flussi che possono essere eseguite per la porta.  Un valore vuoto o zero \(0\) significa che non esiste alcun limite.
|vmqWeight|Specifica se la coda di macchine virtuali (VMQ) è abilitata nella scheda di rete virtuale. Il peso relativo descrive l'affinità della scheda di rete virtuale di usare coda macchine Virtuali. L'intervallo del valore è 0 e 100. Specificare 0 per disabilitare coda macchine Virtuali nella scheda di rete virtuale.
|iovWeight|Specifica se single-root i/o virtualization \(SR-IOV\) è abilitata in questa scheda di rete virtuale. Il peso relativo imposta l'affinità della scheda di rete virtuale alla funzione virtuale SR-IOV assegnata. L'intervallo del valore è 0 e 100. Specificare 0 per disabilitare SR-IOV nella scheda di rete virtuale. 
|iovInterruptModeration|Specifica il valore di regolazione interrupt per un single-root i/o virtualization \(SR-IOV\) funzione virtuale assegnato a una scheda di rete virtuale. I valori consentiti sono "default", "adattivo", "off", "basso", "Media" e "high".   Se si sceglie l'impostazione predefinita, il valore è determinato dall'impostazione del fornitore della scheda di rete fisica.  Se si sceglie adattiva, la frequenza di regolazione interrupt si basa sul modello di traffico di runtime. 
|iovQueuePairsRequested|Specifica il numero di coppie di code hardware da allocare a una funzione virtuale SR-IOV. Se è necessario il receive-side scaling \(RSS\) e se la scheda di rete fisica associata al commutatore virtuale supporta RSS virtuale SR-iov funzioni, più coppie di code è obbligatorio. Valori consentiti sono compresi tra 1 per 4294967295. 
|QosSettings|È possibile configurare le impostazioni Qos seguenti, ognuno dei quali sono facoltativi:  <br/><br />**outboundReservedValue:**<br/>Se è "assoluto" outboundReservedMode il valore indica la larghezza di banda, in Mbps, la porta virtuale per la trasmissione (in uscita) è garantito. Se outboundReservedMode è "peso" il valore indica la parte ponderata della larghezza di banda garantita. <br/><br />**outboundMaximumMbps:**  <br/>Indica che il numero massimo consentito di larghezza di banda sul lato invio, in Mbps, per la porta virtuale (in uscita). <br/><br/>**InboundMaximumMbps:**  <br/>Indica che il numero massimo consentito il receive-side della larghezza di banda per la porta virtuale (ingresso) in Mbps. |