---
title: Virtual Receive Side Scaling (vRSS)
description: Informazioni su Receive-Side Scaling virtuale (vRSS) in Windows Server e su come configurare una scheda di rete virtuale per il bilanciamento del carico del traffico di rete in ingresso tra più core del processore logico in una macchina virtuale. È anche possibile configurare i core fisici multiplo per una scheda di interfaccia di rete virtuale (vNIC) host.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 9be477b3-f81d-4e84-a6b0-ac4c1ea97715
ms.date: 09/05/2018
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b24cabe3597af35e7c7f3c6f81d360bb11675e23
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395793"
---
# <a name="virtual-receive-side-scaling-vrss"></a>Receive-Side Scaling \(virtuale vRSS\)

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo argomento vengono fornite informazioni su Receive-Side Scaling (vRSS) virtuale e come configurare una scheda di rete virtuale per il bilanciamento del carico del traffico di rete in ingresso tra più core del processore logico in una macchina virtuale. È anche possibile usare vRSS per configurare più core fisici per una scheda \(di interfaccia di rete virtuale dell'host vNIC.\)

Questa configurazione consente di distribuire il carico da una scheda di rete virtuale tra più processori virtuali in una macchina virtuale \(della\)macchina virtuale, consentendo alla macchina virtuale di elaborare più rapidamente il traffico di rete rispetto a una singola processore logico.

>[!TIP]
>È possibile usare vRSS in macchine virtuali in\-host Hyper-V con più processori, un singolo processore multicore o più processori core installati e configurati per l'uso di macchine virtuali.

vRSS è compatibile con tutte le altre\-tecnologie di rete Hyper-V. vRSS dipende da coda macchine virtuali \(VMQ\) nell'host Hyper\--V e RSS nella macchina virtuale o nell'host vNIC.

Per impostazione predefinita, Windows Server Abilita vRSS, ma è possibile disabilitarlo in una macchina virtuale usando Windows PowerShell. Per altre informazioni, vedere [gestire](vrss-manage.md) [i comandi VRSS e Windows PowerShell per RSS e vRSS](vrss-wps.md).



## <a name="operating-system-compatibility"></a>Compatibilità del sistema operativo

È possibile usare RSS in qualsiasi computer multiprocessore o multicore o vRSS in qualsiasi macchina virtuale multiprocessore o multicore che esegue Windows Server 2016.

Anche le VM multiprocessore o multicore che eseguono i seguenti sistemi operativi Microsoft supportano vRSS.

- Windows Server 2016
- Windows 10 Pro o Enterprise
- Windows Server 2012 R2
- Windows 8.1 Pro o Enterprise
- Windows Server 2012 con i componenti di integrazione di Windows Server 2012 R2 installati.
- Windows 8 con i componenti di integrazione di Windows Server 2012 R2 installati.

Per informazioni sul supporto di vRSS per le macchine virtuali che eseguono FreeBSD o Linux come sistema operativo guest in Hyper-V, vedere la pagina relativa alle [macchine virtuali Linux e FreeBSD supportate per Hyper-v in Windows](https://docs.microsoft.com/windows-server/virtualization/hyper-v/Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows).
  
## <a name="hardware-requirements"></a>Requisiti hardware

Di seguito sono riportati i requisiti hardware per vRSS.
 
- Le schede di rete fisiche devono \(supportare\)coda macchine virtuali VMQ. Se VMQ è disabilitato o non è supportato, vRSS è disabilitato per\-l'host Hyper-V e le macchine virtuali configurate nell'host.
- Le schede di rete devono avere una velocità di collegamento pari o superiore a 10 Gbps.
- Per\-usare vRSS, gli host Hyper-V devono essere configurati\-con più processori o almeno un processore multicore.
- Le VM \(\) delle macchine virtuali devono essere configurate per l'uso di due o più processori logici.


## <a name="use-case-scenarios"></a>Scenari di casi d'uso

I due scenari di casi d'uso seguenti illustrano l'utilizzo comune di vRSS per il bilanciamento del carico del processore e il bilanciamento del carico software.

### <a name="processor-load-balancing"></a>Bilanciamento del carico dei processori
  
Anthony, un amministratore di rete, sta configurando un nuovo host Hyper-V con due schede di rete che supportano la Single root \(input\--\)output Virtualization SR IOV. Distribuisce Windows Server 2016 per ospitare una macchina virtuale file server.

Dopo l'installazione dell'hardware e del software, Anthony configura una VM per l'utilizzo di otto processori virtuali e 4096 MB di memoria. Sfortunatamente, Anthony non ha la possibilità di attivare SR\-IOV perché le macchine virtuali si basano sull'applicazione dei criteri tramite il commutatore virtuale creato con la gestione commutatori virtuali Hyper\-V. Per questo motivo, Anthony decide di usare vRSS anziché SR\-IOV.

Inizialmente Anthony assegna quattro processori virtuali utilizzando Windows PowerShell per poter essere utilizzato con vRSS. L'uso del file server dopo una settimana sembra essere molto popolare, quindi Anthony controlla le prestazioni della macchina virtuale.  Rileva l'utilizzo completo dei quattro processori virtuali.

Per questo motivo, Anthony decide di aggiungere processori alla macchina virtuale per l'uso da parte di vRSS.  Assegna altri due processori virtuali alla VM, che sono automaticamente disponibili per vRSS per gestire il carico di rete di grandi dimensioni. Le sue attività comportano prestazioni migliori per la macchina virtuale file server, con i sei processori che gestiscono in modo efficiente il carico del traffico di rete.


### <a name="software-load-balancing"></a>Bilanciamento del carico software

Sandra, un amministratore di rete, sta configurando una singola macchina virtuale a prestazioni elevate su uno dei propri sistemi per agire come bilanciamento del carico software. Ha assegnato tutti i processori logici disponibili a questa singola macchina virtuale.

Dopo l'installazione di Windows Server, USA vRSS per ottenere l'elaborazione parallela del traffico di rete su più processori logici nella macchina virtuale. Poiché Windows Server Abilita vRSS, Sandra non deve apportare modifiche alla configurazione.


## <a name="related-topics"></a>Argomenti correlati

- [Pianificare l'uso di vRSS](vrss-plan.md)
- [Abilitare vRSS in una scheda di rete virtuale](vrss-enable.md)
- [Gestire vRSS](vrss-manage.md)
- [Domande frequenti su vRSS](vrss-faq.md)
- [Comandi di Windows PowerShell per RSS e vRSS](vrss-wps.md)

---
