---
title: Virtual Receive Side Scaling (vRSS)
description: Informazioni sulle Virtual Receive Side Scaling (vRSS) in Windows Server e come configurare una scheda di rete virtuale per caricare il traffico di rete in ingresso equilibrio tra più core logici in una macchina virtuale. È inoltre possibile configurare più core fisici per un host Card di interfaccia di rete virtuale (vNIC).
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 9be477b3-f81d-4e84-a6b0-ac4c1ea97715
ms.date: 09/05/2018
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0c1cb11cb8ce69463a31cfa5061290f79d8dda91
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133687"
---
# Virtuale ricevere lato ridimensionamento \(vRSS\)

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo argomento scoprirai Virtual Receive Side Scaling (vRSS) e come configurare una scheda di rete virtuale per caricare il traffico di rete in ingresso equilibrio tra più core logici in una macchina virtuale. È inoltre possibile utilizzare vRSS per configurare più core fisici per un \(vNIC\) scheda di rete virtuale host.

Questa configurazione consente il caricamento da una scheda di rete virtuale deve essere distribuito tra più processori virtuali in una macchina virtuale \(VM\), consentendo la macchina virtuale elaborare più il traffico di rete in modo più rapido possibile con un singolo processore logico.

>[!TIP]
>È possibile utilizzare vRSS nelle macchine virtuali negli host Hyper-V con più processori, un singolo più del processore di core, o più core più installato e configurato per l'uso della macchina virtuale.

vRSS è compatibile con tutte le altre tecnologie di rete Hyper-V. vRSS dipende dalla coda di macchina virtuale \(VMQ\) nell'host Hyper-V e RSS nella macchina virtuale o in vNIC host.

Per impostazione predefinita, Windows Server consente di vRSS, ma è possibile disabilitarla in una macchina virtuale tramite Windows PowerShell. Per altre informazioni, vedi [Gestisci vRSS](vrss-manage.md) e [I comandi di Windows PowerShell per RSS e vRSS](vrss-wps.md).



## Compatibilità del sistema operativo

È possibile utilizzare RSS su qualsiasi computer con più processori o multicore - o vRSS in qualsiasi macchina virtuale più processori o multicore - che esegue Windows Server 2016.

Multiprocessore o multicore macchine virtuali che eseguono i seguenti sistemi operativi Microsoft supportano anche vRSS.

- WindowsServer 2016
- Windows 10 Pro o Enterprise
- Windows Server 2012 R2
- Windows 8.1 Pro o Enterprise
- Windows Server 2012 con i componenti di integrazione di Windows Server 2012 R2 installati.
- Windows 8 con i componenti di integrazione di Windows Server 2012 R2 installati.

Per informazioni sul supporto vRSS per le macchine virtuali in esecuzione di FreeBSD o Linux come sistema operativo guest su Hyper-V, vedere [le macchine virtuali Linux e FreeBSD supportate per Hyper-V in Windows](https://docs.microsoft.com/windows-server/virtualization/hyper-v/Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows).
  
## Requisiti hardware

Di seguito sono i requisiti hardware per vRSS.
 
- Le schede di rete fisica devono supportare \(VMQ\) coda di macchina virtuale. Se VMQ è disabilitata o non è supportato, vRSS è disabilitato per l'host Hyper-V e tutte le macchine virtuali configurate nell'host.
- Le schede di rete devono avere una velocità di collegamento di 10 Gbps o più.
- Gli host Hyper-V devono essere configurati con più processori o del processore di almeno un multi\-core usare vRSS.
- Le macchine virtuali \(VMs\) deve essere configurato per l'uso di due o più processori logici.


## Scenari di casi di uso

I seguenti scenari di due uso maiuscole rappresentano utilizzo comune di vRSS per bilanciamento del carico del processore e bilanciamento del carico software.

### Bilanciamento del carico responsabile del trattamento
  
Lavoro, un amministratore di rete, è impostazione di un nuovo host Hyper-V con scheda di rete due che supporta la virtualizzazione singolo radice Input / Output \(SR\-IOV\). Si distribuisce Windows Server 2016 per ospitare un file server di macchina virtuale.

Dopo aver installato l'hardware e software, lavoro consente di configurare una macchina virtuale per usare otto processori virtuali e 4.096 MB di memoria. Purtroppo lavoro non prevede la possibilità di attivare SR\-IOV poiché il suo macchine virtuali si basano sui criteri attraverso il commutatore virtuale che ha creato con commutatore virtuale Hyper-V manager. Per questo motivo, lavoro decide di usare vRSS invece SR\-IOV.

Inizialmente, lavoro assegna quattro processori virtuali tramite Windows PowerShell devono per essere disponibili per l'uso con vRSS. L'uso di file server dopo una settimana risulta essere piuttosto diffuso, quindi lavoro controlla le prestazioni della macchina virtuale.  Egli individua utilizzo completo di quattro processori virtuali.

Per questo motivo, lavoro decide di aggiungere i processori alla macchina virtuale per l'uso da vRSS.  Due processori virtuali altre egli assegna alla macchina virtuale, che sono automaticamente disponibili per vRSS per gestire il carico di rete di grandi dimensioni. Il tentativo di ottenere prestazioni migliori per file server di macchina virtuale con i sei processori gestendo in modo efficiente il carico di traffico di rete.


### Bilanciamento del carico software

Sandra, un amministratore di rete, è configurazione di una singola macchina virtuale ad alte prestazioni su uno dei sistemi di follower per fungere da un servizio di bilanciamento del carico software. Mentre quello assegnato tutti i processori logici disponibili in questa macchina virtuale singola.

Dopo l'installazione di Windows Server, utilizza vRSS per ottenere il traffico di rete parallela, l'elaborazione in più processori logici nella macchina virtuale. Perché Windows Server consente vRSS, Sandra non dispone di apportare le modifiche di configurazione.


## Argomenti correlati

- [Pianificare l'uso di vRSS](vrss-plan.md)
- [Abilitare vRSS in una scheda di rete virtuale](vrss-enable.md)
- [Gestire vRSS](vrss-manage.md)
- [vRSS domande frequenti](vrss-faq.md)
- [Comandi di Windows PowerShell per RSS e vRSS](vrss-wps.md)

---
