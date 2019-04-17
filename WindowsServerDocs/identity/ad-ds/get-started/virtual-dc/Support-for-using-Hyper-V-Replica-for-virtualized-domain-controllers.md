---
ms.assetid: 45a65504-70b5-46ea-b2e0-db45263fabaa
title: Supporto per l'utilizzo di Replica Hyper-V per controller di dominio virtualizzati
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0444198196ed08a22aba92a0f59cc6e7a2518a2e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="support-for-using-hyper-v-replica-for-virtualized-domain-controllers"></a>Supporto per l'utilizzo di Replica Hyper-V per controller di dominio virtualizzati

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento illustra il supporto dell'uso della Replica Hyper-V per replicare una macchina virtuale (VM) che viene eseguito come un controller di dominio (DC). Replica Hyper-V è una nuova funzionalità di a partire da Windows Server 2012 che offre un meccanismo di replica incorporato a livello di macchina virtuale Hyper-V.  
  
Replica Hyper-V replica in modo asincrono determinate VM da un host Hyper-V primario in un host Hyper-V replica tramite LAN o collegamenti WAN. Una volta completata la replica iniziale, le successive modifiche vengono replicate a un intervallo definito dall'amministratore.  
  
Failover può essere pianificato o non pianificato. Un failover pianificato viene avviato da un amministratore della VM primaria ed eventuali modifiche non replicate vengono copiate nella VM per evitare perdite di dati di replica. Un failover non pianificato viene avviato nella VM di replica in risposta a un errore imprevisto della VM primaria. Perdita di dati è possibile perché esiste la possibilità di trasmettere le modifiche della VM primaria che potrebbero non essere state ancora replicate.  
  
Per ulteriori informazioni sulla Replica Hyper-V, vedere [Panoramica della Replica Hyper-V](https://technet.microsoft.com/library/jj134172.aspx) e [distribuire la Replica Hyper-V](https://technet.microsoft.com/library/jj134207.aspx).  
  
> [!NOTE]  
> Replica Hyper-V può essere eseguita solo in Windows Server Hyper-V, non la versione di Hyper-V che viene eseguito in Windows 8.  
  
## <a name="windows-server-2012-domain-controllers-required"></a>Controller di dominio Windows Server 2012 necessari  
Windows Server 2012 Hyper-V introduce anche l'ID di generazione VM (VMGenID). Che fornisce un mezzo per l'hypervisor comunicare con il sistema operativo guest quando si verificano modifiche significative. Ad esempio, l'hypervisor può comunicare con un controller di dominio virtualizzati che si sia verificato un ripristino dello snapshot (tecnologia Hyper-V snapshot ripristino, non ripristino di backup). Dominio di Active Directory in Windows Server 2012 è a conoscenza della tecnologia delle VM VMGenID e lo usa per rilevare quando vengono eseguite operazioni dell'hypervisor, ad esempio il ripristino dello snapshot, che consente una migliore protezione stesso.  
  
> [!NOTE]  
> Per rafforzare questo punto, solo di dominio Active Directory nei controller di dominio di Windows Server 2012 offre queste misure di sicurezza derivanti da VMGenID; I controller di dominio che eseguono tutte le versioni precedenti di Windows Server sono soggetti a problemi come il rollback degli USN che possono verificarsi quando un controller di dominio virtualizzato viene ripristinato utilizzando un meccanismo non supportato, ad esempio ripristino dello snapshot. Per ulteriori informazioni su queste misure di sicurezza e quando vengono attivate, vedere [architettura di Controller di dominio virtualizzati](https://technet.microsoft.com/library/jj574118.aspx).  
  
Quando (pianificato o non pianificato), si verifica un failover di replica Hyper-V, Windows Server 2012 virtualizzato controller di dominio rileva un ripristino di VMGenID, attivando le suddette funzionalità di sicurezza. Operazioni di Active Directory procedono quindi normalmente. La replica macchina virtuale viene eseguita al posto della VM primaria.  
  
> [!NOTE]  
> Dato che ora sono ora due istanze della stessa identità di controller di dominio, esiste la possibilità che sia l'istanza primaria che quella replicata per l'esecuzione. Durante la Replica Hyper-V prevede meccanismi per garantire primario e macchine virtuali di replica non vengono eseguite contemporaneamente, è possibile che possono essere eseguiti nello stesso momento nel caso in cui il collegamento tra di essi non riesce dopo la replica della macchina virtuale. In caso di questa condizione improbabile, controller di dominio virtualizzati che eseguono Windows Server 2012 hanno misure di sicurezza per proteggere i servizi di dominio Active Directory, mentre quelli che eseguono versioni precedenti di Windows Server non.  
  
Quando si utilizza la Replica Hyper-V, assicurarsi di seguire le procedure consigliate per [l'esecuzione di controller di dominio virtuale in Hyper-V](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(v=WS.10).aspx). Vengono riportati, ad esempio, consigli per l'archiviazione dei file di Active Directory in dischi SCSI virtuali, che offrono maggiori garanzie di durabilità dei dati.  
  
## <a name="supported-and-unsupported-scenarios"></a>Scenari supportati e non supportati  
Solo le macchine virtuali che eseguono Windows Server 2012 sono supportate per il failover non pianificato e per i test di failover. Anche per il failover pianificato, Windows Server 2012 è consigliata per il controller di dominio virtualizzati per ridurre i rischi nel caso in cui un amministratore avvii inavvertitamente sia la VM primaria che quella replicata contemporaneamente.  
  
Le macchine virtuali che eseguono versioni precedenti di Windows Server sono supportate per il failover pianificato, ma non supportate per il failover non pianificato a causa di potenziale rollback degli USN. Per ulteriori informazioni sul rollback degli USN, vedere [USN e Rollback degli USN](https://technet.microsoft.com/library/d2cae85b-41ac-497f-8cd1-5fbaa6740ffe(v=ws.10)).  
  
> [!NOTE]  
> Non sono previsti requisiti a livello funzionali per il dominio o un insieme di strutture. Esistono solo requisiti del sistema operativo per i controller di dominio eseguiti come VM che vengono replicate con la Replica Hyper-V. Le macchine virtuali possono essere distribuite in una foresta che contiene altri controller di dominio fisici o virtuali che eseguono versioni precedenti di Windows Server e che possono o potrebbe non essere replicati con la Replica Hyper-V.  
  
Questa affermazione sul supporto si basa su test eseguiti in una foresta a singolo dominio, anche se sono supportate anche configurazioni di foreste multidominio. Per questi test, controller di dominio virtualizzati DC1 e DC2 sono partner di nello stesso sito, Active Directory replica ospitato in un server che esegue Hyper-V in Windows Server 2012. Il guest della macchina virtuale che esegue DC2 è abilitata la Replica Hyper-V. Il server di Replica è ospitato in un altro Data Center geograficamente distante. Per illustrare i processi dei test case descritti di seguito, la macchina virtuale in esecuzione nel server di replica è detta DC2-Rec (anche se in pratica conserva lo stesso nome della VM originale).  
  
### <a name="windows-server-2012"></a>Windows Server 2012  
Nella tabella seguente illustra il supporto per controller di dominio virtualizzati che eseguono Windows Server 2012 e i test case.  
  
|||  
|-|-|  
|Failover pianificato|Failover non pianificato|  
|È supportato|È supportato|  
|Test case:<br /><br />-DC1 e DC2 eseguono Windows Server 2012.<br /><br />-DC2 viene arrestato e viene eseguito un failover in DC2-Rec. Il failover può essere pianificato o non pianificato.<br /><br />-Dopo il riavvio, DC2-Rec verifica se il valore di VMGenID presente nel proprio database è uguale al valore del driver della macchina virtuale salvato dal server di Replica Hyper-V.<br /><br />-Di conseguenza, DC2-Rec attiva le misure di sicurezza della virtualizzazione; in altri termini, reimposta l'ID di chiamata, rimuove il pool di RID e imposta un requisito di sincronizzazione iniziale prima che assuma un ruolo di master operazioni. Per ulteriori informazioni sui requisiti per la sincronizzazione iniziale, vedere.<br /><br />-DC2-Rec quindi Salva il nuovo valore di VMGenID nel proprio database ed esegue il commit di tutti i successivi aggiornamenti nel contesto del nuovo ID di chiamata.<br /><br />-Come risultato del ripristino di ID di chiamata, DC1 convergerà su tutte le modifiche di Active Directory introdotte da DC2-Rec, anche se è stato eseguito il rollback nel tempo, vale a dire eventuali aggiornamenti di Active Directory eseguiti in DC2-Rec dopo il failover convergeranno correttamente|Il test case è uguale a quella inviata a un failover pianificato, con le seguenti eccezioni:<br /><br />-Qualsiasi AD aggiornamenti ricevuti in DC2 ma non ancora replicati da Active Directory per un partner di replica prima dell'evento di failover andranno perso.<br /><br />-Aggiornamenti di Active Directory ricevuti in DC2 dopo l'ora del punto di ripristino che sono stati replicati da Active Directory in DC1 verrà replicato da DC1 a DC2-Rec.|  
  
### <a name="windows-server-2008-r2-and-earlier-versions"></a>Windows Server 2008 R2 e versioni precedenti  
Nella tabella seguente illustra il supporto per controller di dominio virtualizzati che eseguono Windows Server 2008 R2 e versioni precedenti.  
  
|||  
|-|-|  
|Failover pianificato|Failover non pianificato|  
|Supportato ma non consigliato perché i controller di dominio che eseguono queste versioni di Windows Server non supportano VMGenID né utilizzare misure di sicurezza della virtualizzazione associate. Conseguenza, sono soggetti al rischio di rollback degli USN. Per ulteriori informazioni, vedere [USN e Rollback degli USN](https://technet.microsoft.com/en-us/library/d2cae85b-41ac-497f-8cd1-5fbaa6740ffe(v=ws.10)).|Non è supportato **Nota:** failover non pianificato sarebbe supportato in cui il rollback degli USN non è un rischio, ad esempio un singolo controller di dominio nella foresta (una configurazione non consigliata).|  
|Test case:<br /><br />-DC1 e DC2 eseguono Windows Server 2008 R2.<br /><br />-DC2 viene arrestato e viene eseguito un failover pianificato in DC2-Rec. Tutti i dati in DC2 vengono replicati in DC2-Rec prima dell'arresto.<br /><br />-Dopo il riavvio, DC2-Rec riprende la replica con DC1 utilizzando l'ID di chiamata stesso come DC2.|N/D|  
  


