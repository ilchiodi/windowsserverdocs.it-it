---
title: Data center software-defined di Windows Server
description: Panoramica su Windows Server SDDC
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: SDDC
ms.tgt_pltfrm: na
ms.topic: get-started article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 06/04/2019
ms.localizationpriority: medium
ms.openlocfilehash: 75a565eb9e26e03c1aad221059b327943485c81c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403893"
---
# <a name="windows-server-software-defined-datacenter"></a>Data center software-defined di Windows Server

>Si applica a: Windows Server 2019, Windows Server 2016

![](media/sddc/heading.png)

## <a name="what-is-windows-server-software-defined-datacenter"></a>Che cos'è il data center definito dal software di Windows Server?

Il data center definito dal software (SDDC) è un termine di settore comune che in genere si riferisce a un Data Center in cui tutta l'infrastruttura è virtualizzata. La virtualizzazione è la chiave e significa semplicemente che l'hardware e il software del data center si espandono oltre il tradizionale rapporto uno a uno. Grazie a un software hypervisor che emula l'hardware, i sistemi operativi e le applicazioni possono essere astratti dall'hardware fisico e moltiplicati in modo da creare pool di risorse flessibili di processori, memoria, I/O e reti.
 
L'implementazione di Microsoft del SDDC è costituito dalle tecnologie di Windows Server evidenziate in questo articolo. Inizia con l'hypervisor Hyper-V che fornisce la piattaforma di virtualizzazione su cui vengono incorporati reti e spazi di archiviazione. Le tecnologie di sicurezza, sviluppate per rispondere alle sfide specifiche dell'infrastruttura virtualizzata, consentono di ridurre le minacce interne ed esterne. Con PowerShell incorporata in Windows Server e l'aggiunta di [System Center](https://docs.microsoft.com/system-center/) e/o [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview), è possibile programmare e automatizzare provisioning, distribuzione, configurazione e gestione.

Le tecnologie integrate in Windows Server e System Center sono i principali elementi costitutivi dell'esperienza Windows Server SDDC. Ma anche se si tratta di una piattaforma virtualizzata, deve comunque risiedere sull'hardware appropriato. I partner Microsoft che partecipano alle **soluzioni Windows Server software-defined (WSSD)** e ai programmi per le **soluzioni HCI Azure stack** possono aiutare le aziende ad acquisire l'hardware corretto e renderlo operativo il giorno zero.

![](media/sddc/video.png) **[Guarda un video per scoprire di più sulle SDDC di Microsoft](https://mva.microsoft.com/en-US/training-courses/whats-new-in-windows-server-2016-16457?l=YcsJR6sXC_1006218965)**

![](media/sddc/poster-ico.png) **[scaricare un file poster size. pdf di questa pagina](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/WindowsServerDocs/media/sddc/sddc_poster_0801417_ANSI-E.pdf)**

![](media/sddc/spacer1.png)<a href="https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/WindowsServerDocs//media/sddc/sddc_poster_0801417_ANSI-E.pdf"><img src="media/sddc/poster.png"></a>

## <a name="azure-stack-hci-solutions"></a>Soluzioni di Azure Stack HCI

La creazione di un data center definito dal software Windows Server nell'infrastruttura hardware corretta è un primo passaggio cruciale per il successo. Ecco perché abbiamo collaborato con 15 partner per creare progettazioni SDDC convalidate da Microsoft e procedure consigliate per la distribuzione.

I partner Microsoft offrono una gamma di soluzioni che funzionano con Window Server 2019 tramite il programma Azure Stack HCI e Windows Server 2016 tramite il programma Windows Server software-defined (WSSD) per offrire prestazioni elevate, iperconvergenti, di archiviazione e di rete. infrastruttura. Le soluzioni iperconvergenti riuniscono elaborazione, archiviazione e reti su server e componenti standard di settore per un data center migliore sotto il profilo dell'intelligence e del controllo.

![](media/sddc/learn.png) ulteriori informazioni **[sulle soluzioni di Azure stack HCI](https://azure.microsoft.com/overview/azure-stack/hci)**

![](media/sddc/learn.png) ulteriori informazioni **[sulle soluzioni WSSD](https://www.microsoft.com/en-us/cloud-platform/software-defined-datacenter)**

## <a name="windows-server-virtualized-technologies"></a>Tecnologie virtualizzate di Windows Server ##

Nella parte restante di questo argomento vengono elencate le tecnologie di Windows Server SDDC e vengono forniti per ciascuna i collegamenti alla documentazione. Le tecnologie sono elencate nella tabella seguente:

![](media/sddc/table.png)

![](media/sddc/virtualize.png)

### <a name="windows-server-hyper-converged"></a>Windows Server, iperconvergente

Le tecnologie di virtualizzazione di Windows Server includono gli aggiornamenti per Hyper-V, il commutatore virtuale Hyper-V e l'infrastruttura protetta e le macchine virtuali (VM) schermate che migliorano sicurezza, scalabilità e affidabilità. Gli aggiornamenti per il clustering di failover, la rete e archiviazione rendono ancora più semplice distribuire e gestire queste tecnologie se utilizzata con Hyper-V.

![](media/sddc/spacer1.png)![](media/sddc/hyper-converged.png)

![](media/sddc/learn.png) ulteriori informazioni **[su Windows Server, iperconvergente](https://docs.microsoft.com/windows-server/get-started/what-s-new-in-windows-server-2016#computevirtualizationvirtualizationmd)**

### <a name="hyper-v-hypervisor"></a>Hypervisor Hyper-V

Hyper-V è una tecnologia di virtualizzazione basata su hypervisor per Windows. L'hypervisor è fondamentale per la virtualizzazione. È la piattaforma di virtualizzazione specifica del processore che consente a più sistemi operativi isolati di condividere una singola piattaforma hardware.

![](media/sddc/spacer1.png)![](media/sddc/hypervisor.png)

![](media/sddc/learn.png) ulteriori **[informazioni sull'hypervisor Hyper-V](https://www.microsoft.com/en-us/cloud-platform/server-virtualization)**

### <a name="guest-clustering-with-shared-vhdx"></a>Clustering guest con VHDX condiviso

![](media/sddc/virtualize-line.png)

Flessibile e sicuro, e non associato alla topologia di archiviazione sottostante, VHDX condiviso elimina la necessità di presentare l'archiviazione fisica sottostante a un sistema operativo guest. Il nuovo VHDX condiviso supporta il ridimensionamento online.

![](media/sddc/spacer1.png)![](media/sddc/cluster.png)

- VHDX condiviso può risiedere su un volume condiviso cluster (Cluster Shared Volume - CSV) su archiviazione a blocchi o archiviazione basata su file SMB.
- Protetto VHDX condiviso supporta la replica Hyper-V e il backup a livello di host.

![](media/sddc/learn.png) ulteriori informazioni **[sul clustering guest con VHDX condiviso](https://technet.microsoft.com/library/dn281956(v=ws.11).aspx)**

### <a name="hyper-v-replica"></a>Replica Hyper-V

![](media/sddc/virtualize-line.png)

Replica VM basata su software integrata sulla rete con certificati. Non associata all'hardware di archiviazione, rete o server in nessun sito.

![](media/sddc/spacer1.png)![](media/sddc/replica.png)

Nessuna necessità di altre tecnologie di replica di macchine virtuali in modo da ridurre i costi.
- Gestisce la migrazione in tempo reale automaticamente.
- Gestione e configurazione semplici: tramite Hyper-V Manager, PowerShell o con Azure Site Recovery.

![](media/sddc/learn.png) ulteriori **[informazioni sulla replica Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/set-up-hyper-v-replica)**

![](media/sddc/networking.png)

### <a name="network-controller"></a>Controller di rete

![](media/sddc/networking-line.png)

Un punto di automazione programmabile e centralizzato per gestire, configurare e monitorare l'infrastruttura di rete fisica e virtuale nel centro dati e di risolverne gli eventuali problemi.

![](media/sddc/spacer1.png)![](media/sddc/netcontroller.png)

Gli amministratori usano uno strumento di gestione che interagisce direttamente con il Controller di rete. Il controller di rete fornisce informazioni sull'infrastruttura di rete, inclusa l'infrastruttura fisica e virtuale, allo strumento di gestione.

![](media/sddc/learn.png) ulteriori **[informazioni sul controller di rete](https://docs.microsoft.com/windows-server/networking/sdn/technologies/network-controller/network-controller)**

### <a name="datacenter-firewall"></a>Firewall del data center

![](media/sddc/networking-line.png)

Quando viene distribuito e offerto come servizio, gli amministratori del tenant possono installare e configurare i criteri del firewall per proteggere le reti virtuali dal traffico indesiderato da Internet e le reti Intranet.

![](media/sddc/spacer1.png)![](media/sddc/firewall.png)

L'amministratore del service provider o l'amministratore del tenant può gestire i criteri del firewall del data center tramite il controller di rete.

![](media/sddc/learn.png) ulteriori **[informazioni sul firewall del Data Center](https://docs.microsoft.com/windows-server/networking/sdn/technologies/network-function-virtualization/datacenter-firewall-overview)**

### <a name="switch-embedded-teaming"></a>Switch Embedded Teaming

![](media/sddc/networking-line.png)

SET è una soluzione alternativa di gruppo NIC che è possibile utilizzare in ambienti che includono Hyper-V e lo stack [SDN (Software Defined Networking)](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking).

![](media/sddc/spacer1.png)![](media/sddc/teaming.png)

![](media/sddc/learn.png) ulteriori informazioni **[su switch Embedded Teaming](https://docs.microsoft.com/windows-server/networking/sdn/technologies/set-for-sdn)**

### <a name="software-load-balancing"></a>Software Load Balancing

![](media/sddc/networking-line.png)

SLB consente di abilitare più server per l'hosting dello stesso carico di lavoro, offrendo disponibilità e scalabilità elevate. Consente lo scale out delle funzionalità di bilanciamento del carico usando le macchine virtuali SLB sugli stessi server Hyper-V utilizzati per altri carichi di lavoro VM. SLB supporta la creazione rapida e l'eliminazione rapida degli endpoint di bilanciamento del carico per le operazioni del provider di servizi cloud. SLB supporta decine di GB per cluster, fornisce un modello di provisioning semplice e facilita lo scale out e la riduzione.

![](media/sddc/spacer1.png)![](media/sddc/balancer.png)

![](media/sddc/learn.png) ulteriori informazioni **[sul bilanciamento del carico software](https://docs.microsoft.com/windows-server/networking/sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn)**


![](media/sddc/storage.png)

### <a name="storage-spaces-direct"></a>Spazi di archiviazione diretta

![](media/sddc/storage-line.png)

Tramite l'uso di server standard di settore con unità collegate in locale, Spazi di archiviazione diretta offre un'archiviazione software-defined a disponibilità elevata altamente scalabile a un costo minimo rispetto agli array SAN o NAS tradizionali. La sua architettura semplifica notevolmente l'approvvigionamento e la distribuzione.

il nodo ![Each dispone di unità collegate localmente in pool a livello di cluster Spazi di archiviazione diretta quindi a cui è possibile accedere dalle macchine virtuali tramite CSVs @ no__t-1

Spazi di archiviazione diretta introduce il nuovo bus di archiviazione software e usa molte delle funzionalità di Windows Server, ad esempio il clustering di failover, i volumi condivisi cluster, Server Message Block (SMB) 3 e Spazi di archiviazione.

![](media/sddc/learn.png) ulteriori **[informazioni sulle spazi di archiviazione diretta](storage/storage-spaces/storage-spaces-direct-overview.md)**
### <a name="storage-quality-of-service"></a>QoS di archiviazione ###

![](media/sddc/storage-line.png)

Consente di monitorare e gestire centralmente le prestazioni di archiviazione delle macchine virtuali usando i ruoli Hyper-V e del file server di scalabilità orizzontale, contribuendo a migliorare l'equità delle risorse di archiviazione tra più macchine virtuali.

![](media/sddc/spacer1.png)![](media/sddc/qos.png)

La funzionalità QoS di archiviazione è incorporata nella soluzione di archiviazione definita da software Microsoft fornita da File server di scalabilità orizzontale e Hyper-V mediante il protocollo SMB3. Un nuovo strumento di gestione dei criteri fornisce il monitoraggio centralizzato delle prestazioni di archiviazione.

![](media/sddc/learn.png) ulteriori **[informazioni su QoS di archiviazione](https://docs.microsoft.com/windows-server/storage/storage-qos/storage-qos-overview)**

### <a name="storage-replica"></a>Replica archiviazione


![](media/sddc/storage-line.png)

Le funzionalità di ripristino di emergenza e preparazione rendono possibile una perdita di dati pari a zero, grazie alla possibilità di proteggere i dati in modo sincrono su diversi rack, piani, edifici, campus, città e paesi con un utilizzo più efficiente di più data center.

![](media/sddc/spacer1.png)
![](media/sddc/storage-replica.png)

Replica sincrona

1. L'applicazione scrive i dati
2. I dati del log vengono scritti e replicati nel sito remoto
3. I dati del log vengono scritti nel sito remoto
4. Riconoscimento dal sito remoto
5. Scrittura dell'applicazione riconosciuta

t & T1: Dati scaricati nel volume, i log scrivono sempre

![](media/sddc/learn.png) ulteriori **[informazioni su replica archiviazione](https://docs.microsoft.com/windows-server/storage/storage-replica/storage-replica-overview)**

![](media/sddc/security.png)

### <a name="guarded-fabric"></a>Infrastruttura sorvegliata

![](media/sddc/security-line.png)

L'infrastruttura sorvegliata consente ai provider di servizi cloud o agli amministratori del cloud privato aziendale di offrire un ambiente più sicuro per le macchine virtuali. Un'infrastruttura sorvegliata è costituita da un Servizio Sorveglianza host (Host Guardian Service - HGS), in genere, un cluster di tre nodi, oltre a uno o più host sorvegliati e un set di macchine virtuali schermate (VM).

![](media/sddc/spacer1.png)![](media/sddc/guarded-fabric.png)

![](media/sddc/learn.png) **[altre informazioni sull'infrastruttura sorvegliata](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms)**

### <a name="shielded-vms"></a>Macchine virtuali schermate

![](media/sddc/security-line.png)

I dati e lo stato di una macchina virtuale schermata sono protetti da ispezione, furti e manomissione, sia da malware che da amministratori di data center.

![](media/sddc/spacer1.png)![](media/sddc/shielded.png)

- Le macchine virtuali schermate verranno eseguite solo nelle infrastrutture designate come proprietari della macchina virtuale.
- Le macchine virtuali schermate vengono crittografate da BitLocker o altri mezzi, in modo che solo i proprietari designati possono eseguirle.
- Le macchine virtuali in esecuzione possono essere convertite in schermate.

![](media/sddc/learn.png) ulteriori informazioni **[sulle VM schermate](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms)**

### <a name="host-guardian-service"></a>Servizio Sorveglianza host

![](media/sddc/security-line.png)

Servizio sorveglianza host contiene le chiavi per le infrastrutture legittime, nonché le macchine virtuali crittografate.

![](media/sddc/spacer1.png)![](media/sddc/guardian.png)

![](media/sddc/learn.png) ulteriori informazioni **[sul servizio sorveglianza host](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-manage-hgs)**

### <a name="device-health-attestation"></a>Attestazione dell'integrità dei dispositivi

![](media/sddc/security-line.png)

L'attestazione consente alle aziende di aumentare il livello di sicurezza della propria organizzazione grazie a una protezione monitorata e garantita dell'hardware, con un impatto minimo o addirittura pari a zero sui costi di esercizio.


![](media/sddc/spacer1.png)![](media/sddc/attestation.png)


La modalità attendibile dell'hardware, illustrata sopra, offre il massimo livello di garanzia, con l'affidabilità basata sull'hardware TPM 2.0 e la conformità con i criteri di integrità del codice per il rilascio delle chiavi.


![](media/sddc/learn.png) ulteriori **[informazioni sulle attestazione dell'integrità del dispositivo](https://docs.microsoft.com/windows-server/security/device-health-attestation)**

![](media/sddc/management.png)

### <a name="powershell-desired-state-configuration"></a>Configurazione dello stato desiderato di PowerShell

![](media/sddc/management-line.png)

Windows PowerShell DSC (Desired State Configuration) è una piattaforma di gestione delle configurazioni integrata in Windows e basata su standard aperti. DSC è sufficientemente flessibile per funzionare in modo affidabile e coerente in ogni fase del ciclo di distribuzione (sviluppo, test, pre-produzione, produzione), nonché quando viene applicata scalabilità orizzontale.

![](media/sddc/spacer1.png)![](media/sddc/dsc.png)

DSC supporta "distribuzioni continue", pertanto è possibile distribuire le configurazioni più volte senza alcuna interruzione.

-  Le configurazioni DSC applicano solo le impostazioni che sono state modificate dall'originale per distribuzioni più veloci.
-  DSC può essere usato in locale o in un ambiente Cloud privato o pubblico.
-  È possibile integrare DSC con qualsiasi soluzione Microsoft o non Microsoft purché sia possibile eseguire uno script di PowerShell nel sistema di destinazione.

![](media/sddc/learn.png) ulteriori informazioni **[su PowerShell DSC](https://docs.microsoft.com/powershell/dsc/overview)**


### <a name="system-center-vmm"></a>System Center VMM

![](media/sddc/management-line.png)

Virtual Machine Manager è parte della suite di System Center, utilizzata per configurare, gestire e trasformare data center tradizionali per offrire un'esperienza di gestione unificata in locale, con service provider e il cloud di Azure.

![](media/sddc/spacer1.png)![](media/sddc/vmm.png)

- Data Center Configurare e gestire i componenti dei data center come una singola infrastruttura in VMM. 
- Host di virtualizzazione: VMM può aggiungere, effettuare il provisioning e gestire gli host e i cluster di virtualizzazione Hyper-V e VMware.
- Rete VMM offre la virtualizzazione di rete, incluso il supporto per la creazione e la gestione di reti virtuali e gateway di rete. 
- Archiviazione: VMM è in grado di individuare, classificare, effettuare il provisioning, allocare e assegnare l'archiviazione locale e remota.

![](media/sddc/learn.png) ulteriori informazioni **[su System Center VMM](https://docs.microsoft.com/system-center/vmm/)**

### <a name="windows-admin-center"></a>Windows Admin Center

![](media/sddc/management-line.png)

Windows Admin Center è uno strumento di gestione basato su browser, distribuito localmente che abilita l'amministrazione locale dei server Windows senza dipendenze cloud o Azure. Windows Admin Center offre agli amministratori IT il controllo completo di tutti gli aspetti dell'infrastruttura server ed è particolarmente utile per la gestione di reti private che non sono connesse a Internet.

![](media/sddc/spacer1.png)![](media/sddc/architecture.png)

La pubblicazione del server Web su DNS e l'impostazione del firewall aziendale consentono di accedere a Windows Admin Center da Internet pubblico, consentendoti di connetterti e gestire i server ovunque ti trovi con Microsoft Edge o Google Chrome.

![](media/sddc/learn.png) **[altre informazioni sull'interfaccia di amministrazione di Windows](manage/windows-admin-center/overview.md)**
