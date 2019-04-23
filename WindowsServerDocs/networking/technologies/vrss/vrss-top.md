---
title: Virtual Receive Side Scaling (vRSS)
description: Informazioni su Virtual Receive-Side Scaling (vRSS) in Windows Server e come configurare una scheda di rete virtuale per caricare bilanciare il traffico di rete in ingresso tra più core del processore logico in una macchina virtuale. È anche possibile configurare più core fisici per un host di scheda di interfaccia di rete virtuale (vNIC).
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875232"
---
# <a name="virtual-receive-side-scaling-vrss"></a>Virtual Receive-Side Scaling \(vRSS\)

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento illustra Virtual Receive-Side Scaling (vRSS) e su come configurare una scheda di rete virtuale per caricare bilanciare il traffico di rete in ingresso tra più core del processore logico in una macchina virtuale. È anche possibile usare vRSS per configurare più core fisici per un host virtual Network Interface Card \(vNIC\).

Questa configurazione consente il caricamento da una scheda di rete virtuale deve essere distribuito tra più processori virtuali in una macchina virtuale \(VM\), consentendo alla macchina virtuale per l'elaborazione in modo più rapido possibile con una singola maggiore traffico di rete processore logico.

>[!TIP]
>È possibile usare vRSS in macchine virtuali in Hyper\-host V con più processori, un singolo processore core più, oppure più processori di più core installato e configurato per l'utilizzo della macchina virtuale.

vRSS è compatibile con tutte le altre Hyper\-V tecnologie di rete. vRSS è dipendente da coda macchine virtuali \(VMQ\) nel Hyper\-RSS nella macchina virtuale o in vNIC host e host V.

Per impostazione predefinita, Windows Server consente di vRSS, ma è possibile disabilitarlo in una macchina virtuale con Windows PowerShell. Per altre informazioni, vedere [gestire vRSS](vrss-manage.md) e [i comandi di Windows PowerShell per RSS e vRSS](vrss-wps.md).



## <a name="operating-system-compatibility"></a>Compatibilità sistema operativo

È possibile usare RSS su qualsiasi computer multiprocessore o multicore - o vRSS in qualsiasi macchina virtuale multiprocessore o multicore - che esegue Windows Server 2016.

Multiprocessore o multicore macchine virtuali che eseguono sistemi operativi Microsoft seguenti supportano anche vRSS.

- Windows Server 2016
- Windows 10 Pro o Enterprise
- Windows Server 2012 R2
- Windows 8.1 Pro o Enterprise
- Con installati i componenti di integrazione di Windows Server 2012 R2, Windows Server 2012.
- Windows 8 con i componenti di integrazione di Windows Server 2012 R2 installati.

Per informazioni sul supporto di vRSS per le macchine virtuali Linux o FreeBSD in esecuzione come sistema operativo guest in Hyper-V, vedere [macchine virtuali Linux e FreeBSD supportate per Hyper-V in Windows](https://docs.microsoft.com/windows-server/virtualization/hyper-v/Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows).
  
## <a name="hardware-requirements"></a>Requisiti hardware

Di seguito sono i requisiti hardware per vRSS.
 
- Le schede di rete fisica devono supportare coda macchine virtuali \(VMQ\). Se coda macchine Virtuali vengano disabilitata o non supportato, quindi vRSS è disabilitata per il Hyper\-V host e tutte le macchine virtuali configurate nell'host.
- Le schede di rete devono avere una velocità di collegamento pari a 10 Gbps o più.
- Hyper\-V host devono essere configurati con più processori o almeno un multi\-core processore per l'uso di vRSS.
- Le macchine virtuali \(macchine virtuali\) deve essere configurato per usare due o più processori logici.


## <a name="use-case-scenarios"></a>Casi d'uso

I seguenti scenari dei casi d' due uso raffigurare utilizzo comune di vRSS per il bilanciamento del carico del processore e bilanciamento del carico software.

### <a name="processor-load-balancing"></a>Bilanciamento del carico dei processori
  
Antonio, un amministratore di rete, sta configurando un nuovo host Hyper-V con scheda di rete due che supporta Single Root Input-Output Virtualization \(SR\-IOV\). Davide distribuisce Windows Server 2016 per ospitare un server di file della macchina virtuale.

Dopo aver installato l'hardware e software, Antonio configura una macchina virtuale da usare otto processori virtuali e 4096 MB di memoria. Sfortunatamente, Anthony non hanno la possibilità di attivare SR\-IOV perché le proprie macchine virtuali si basano su applicazione di criteri tramite il commutatore virtuale ha creato con Hyper\-commutatore virtuale Hyper-V manager. Per questo motivo, Anthony decide di usare vRSS anziché SR\-IOV.

Antonio inizialmente assegna quattro processori virtuali mediante Windows PowerShell sia disponibile per l'uso con vRSS. L'uso del file server dopo una settimana prima era molto diffusa, in modo Antonio verifica le prestazioni della macchina virtuale.  Scopre di utilizzo di quattro processori virtuali.

Per questo motivo, Anthony decide di aggiungere processori nella macchina virtuale per l'uso da vRSS.  Egli assegna altri due processori virtuali nella macchina virtuale, che sono automaticamente disponibili per vRSS per gestire il carico di rete di grandi dimensioni. Il suo impegno ottenere migliori prestazioni per il server di file della macchina virtuale, con i sei processori gestendo in modo efficiente il carico del traffico di rete.


### <a name="software-load-balancing"></a>Bilanciamento del carico software

Sandra, un amministratore di rete, sta configurando una singola VM ad alte prestazioni su uno dei propri sistemi di agire come un servizio di bilanciamento del carico software. Anna ha assegnato tutti i processori logici disponibili per questa macchina virtuale singola.

Dopo aver installato Windows Server, Anna Usa vRSS per ottenere parallela del traffico di rete di elaborazione su più processori logici nella macchina virtuale. Poiché Windows Server consente di vRSS, Sandra non è necessario apportare eventuali modifiche alla configurazione.


## <a name="related-topics"></a>Argomenti correlati

- [Pianificare l'uso di vRSS](vrss-plan.md)
- [Abilitare vRSS su una scheda di rete virtuale](vrss-enable.md)
- [Gestire vRSS](vrss-manage.md)
- [vRSS domande frequenti](vrss-faq.md)
- [Comandi di Windows PowerShell per RSS e vRSS](vrss-wps.md)

---
