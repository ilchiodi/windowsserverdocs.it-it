---
ms.assetid: 45a65504-70b5-46ea-b2e0-db45263fabaa
title: Supporto per l'uso della replica Hyper-V per controller di dominio virtualizzati
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 92324ef7c0fab81e80974a1f05eeec4833f09875
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390435"
---
# <a name="support-for-using-hyper-v-replica-for-virtualized-domain-controllers"></a>Supporto per l'uso della replica Hyper-V per controller di dominio virtualizzati

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento illustra il supporto previsto per l'uso della replica Hyper-V per replicare una macchina virtuale (VM) eseguita come controller di dominio. La replica Hyper-V è una nuova funzionalità di Hyper-V, disponibile a partire da Windows Server 2012, che fornisce un meccanismo di replica incorporato a livello di VM.  
  
Replica in modo asincrono determinate VM di un host Hyper-V primario in un host Hyper-V di replica attraverso collegamenti LAN o WAN. Dopo il completamento della replica iniziale, le successive modifiche vengono replicate a un intervallo definito dall'amministratore.  
  
Il failover può essere pianificato o non pianificato. Un failover pianificato viene avviato da un amministratore nella VM primaria ed eventuali modifiche non replicate vengono copiate nella VM di replica per evitare la perdita di dati. Un failover non pianificato viene avviato nella VM di replica in risposta a un errore imprevisto della VM primaria. È possibile che si verifichi una perdita di dati perché non esiste la possibilità di trasmettere le modifiche della VM primaria che potrebbero non essere ancora state replicate.  
  
Per ulteriori informazioni sulla replica Hyper-V, vedere [Panoramica della replica Hyper-v](https://technet.microsoft.com/library/jj134172.aspx) e [distribuire la replica Hyper-v](https://technet.microsoft.com/library/jj134207.aspx).  
  
> [!NOTE]  
> La replica Hyper-V può essere eseguita solo in Hyper-V di Windows Server, non nelle versioni eseguite in Windows 8.  
  
## <a name="windows-server-2012-or-newer-domain-controllers-required"></a>È necessario un controller di dominio Windows Server 2012 o versione successiva

Windows Server 2012 Hyper-V ha introdotto VM-generazione (VMGenID). che fornisce un mezzo di comunicazione tra l'hypervisor e il sistema operativo guest quando si verificano modifiche significative. Ad esempio, l'hypervisor può comunicare con un controller di dominio virtualizzato generato dal ripristino di uno snapshot (tecnologia di ripristino di snapshot Hyper-V, non ripristino di backup). Servizi di dominio Active Directory in Windows Server 2012 e versioni successive è a conoscenza della tecnologia della macchina virtuale VMGenID e lo usa per rilevare il momento in cui vengono eseguite le operazioni dell'hypervisor, ad esempio il ripristino dello snapshot, che consente una migliore protezione.  
  
> [!NOTE]
> Solo i controller di dominio Active Directory in Windows Server 2012 o versioni successive forniscono queste misure di sicurezza derivanti da VMGenID; I controller di dominio che eseguono tutte le versioni precedenti di Windows Server sono soggetti a problemi quali il rollback degli USN che possono verificarsi quando un controller di dominio virtualizzato viene ripristinato utilizzando un meccanismo non supportato, ad esempio il ripristino dello snapshot. Per ulteriori informazioni su queste misure di sicurezza e su quando vengono attivate, vedere [architettura di controller di dominio virtualizzati](https://technet.microsoft.com/library/jj574118.aspx).  
  
Quando si verifica un failover della replica Hyper-V (pianificato o non pianificato), il controller di dominio virtualizzato rileva una reimpostazione del VMGenID, attivando le funzionalità di sicurezza indicate sopra. Le operazioni di Active Directory procedono quindi normalmente. La VM di replica viene eseguita al posto della VM primaria.  
  
> [!NOTE]  
> Considerando che a questo punto ci sono due istanze della stessa identità di controller di dominio, esiste la possibilità che vengano eseguite sia l'istanza primaria che quella replicata. Anche se la replica Hyper-V prevede meccanismi di sicurezza per evitarlo, è possibile che le VM primaria e di replica vengano eseguite simultaneamente nel caso in cui si verifichino errori di collegamento tra le due dopo la replica della VM. Nell'eventualità improbabile che si verifichi questo scenario, i controller di dominio virtualizzati che eseguono Windows Server 2012 hanno misure di sicurezza per proteggere Servizi di dominio Active Directory, a differenza di quelli che eseguono versioni precedenti di Windows Server.  
  
Quando si usa la replica Hyper-V, assicurarsi di seguire le procedure consigliate per l' [esecuzione di controller di dominio virtuali in Hyper-v](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(v=WS.10).aspx). Vengono riportati, ad esempio, suggerimenti per l'archiviazione di file di Active Directory in dischi SCSI virtuali, che offrono maggiori garanzie di durabilità dei dati.  
  
## <a name="supported-and-unsupported-scenarios"></a>Scenari supportati e non supportati

Solo le macchine virtuali che eseguono Windows Server 2012 o versioni successive sono supportate per il failover non pianificato e per il failover di test. Anche per il failover pianificato, è consigliabile Windows Server 2012 o versione successiva per il controller di dominio virtualizzato, in modo da ridurre i rischi nel caso in cui un amministratore avvii inavvertitamente sia la macchina virtuale primaria che la macchina virtuale replicata allo stesso tempo.  
  
Le VM che eseguono versioni precedenti di Windows Server sono supportate per il failover pianificato, ma non per quello non pianificato, a causa di un potenziale rollback degli USN. Per ulteriori informazioni sul rollback degli USN, vedere [USN e rollback](https://technet.microsoft.com/library/d2cae85b-41ac-497f-8cd1-5fbaa6740ffe(v=ws.10))degli USN.  
  
> [!NOTE]  
> Non sono previsti requisiti a livello funzionale per il dominio o per la foresta. Esistono solo requisiti del sistema operativo per i controller di dominio eseguiti come VM che vengono replicati con la replica Hyper-V. Le VM possono essere distribuite in una foresta che contiene altri controller di dominio fisici o virtuali con versioni precedenti di Windows Server e che possono o meno essere replicati con la replica Hyper-V.  
  
Questa affermazione sul supporto si basa su test eseguiti in una foresta a singolo dominio, sebbene siano supportate anche configurazioni di foreste multidominio. Per questi test, i controller di dominio DC1 e DC2 sono partner di replica di Active Directory nello stesso sito, ospitati in un server che esegue Hyper-V in Windows Server 2012. Per la VM guest che esegue DC2 è abilitata la replica Hyper-V. Il server di replica viene ospitato in un altro data center geograficamente distante. Per illustrare i processi dei test case descritti di seguito, la VM in esecuzione nel server di replica viene identificata come DC2-Rec (anche se in pratica conserva lo stesso nome della VM originale).  
  
### <a name="windows-server-2012"></a>Windows Server 2012

La tabella seguente illustra il supporto per controller di dominio virtualizzati che eseguono Windows Server 2012 e i test case.  
  
|||  
|-|-|  
|Failover pianificato|Failover non pianificato|  
|Supportato|Supportato|  
|Test case:<br /><br />-DC1 e DC2 eseguono Windows Server 2012.<br /><br />-DC2 viene arrestato e viene eseguito un failover in DC2-REC. Il failover può essere pianificato o non pianificato.<br /><br />-Dopo l'avvio di DC2-REC, verifica se il valore di VMGenID del database è uguale al valore del driver della macchina virtuale salvato dal server di replica Hyper-V.<br /><br />Di conseguenza, DC2-REC attiva le misure di sicurezza della virtualizzazione; in altre parole, reimposta il relativo InvocationID, ignora il pool di RID e imposta un requisito di sincronizzazione iniziale prima di assumere un ruolo di master operazioni. Per altre informazioni sul requisito di sincronizzazione iniziale, vedere .<br /><br />-DC2-REC salva quindi il nuovo valore di VMGenID nel proprio database ed esegue il commit degli aggiornamenti successivi nel contesto del nuovo InvocationID.<br /><br />-In seguito alla reimpostazione del InvocationID, DC1 convergerà su tutte le modifiche AD introdotte da DC2-REC anche se ne è stato eseguito il rollback nel tempo, ovvero gli eventuali aggiornamenti di AD eseguiti in DC2-REC dopo che il failover eseguirà la convergenza|Il test case è identico a quello del failover pianificato, con le eccezioni seguenti:<br /><br />-Tutti gli aggiornamenti di Active Directory ricevuti in DC2 ma non ancora replicati da AD a un partner di replica prima dell'evento di failover andranno perduti.<br /><br />-Gli aggiornamenti di Active Directory ricevuti in DC2 dopo l'ora del punto di ripristino replicato da AD a DC1 verranno replicati da DC1 a DC2-REC.|  
  
### <a name="windows-server-2008-r2-and-earlier-versions"></a>Windows Server 2008 R2 e versioni precedenti

La tabella seguente illustra il supporto per controller di dominio virtualizzati che eseguono Windows Server 2008 R2 e versioni precedenti.  
  
|||  
|-|-|  
|Failover pianificato|Failover non pianificato|  
|Supportato ma non consigliato, perché i controller di dominio che eseguono queste versioni di Windows Server non supportano VMGenID né usano le misure di sicurezza della virtualizzazione associate. Di conseguenza, sono soggetti al rischio di un rollback degli USN. Per altre informazioni, vedere [USN e rollback degli USN](https://technet.microsoft.com/library/d2cae85b-41ac-497f-8cd1-5fbaa6740ffe(v=ws.10)).|Nota non supportata **:** Il failover non pianificato sarebbe supportato se non esistesse il rischio di rollback degli USN, ad esempio con un singolo controller di dominio nella foresta (una configurazione non consigliata).|  
|Test case:<br /><br />-DC1 e DC2 eseguono Windows Server 2008 R2.<br /><br />-DC2 viene arrestato e viene eseguito un failover pianificato in DC2-REC. Tutti i dati di DC2 vengono replicati in DC2-Rec prima dell'arresto.<br /><br />-Dopo l'avvio di DC2-REC, riprende la replica con DC1 usando lo stesso invocationID di DC2.|N/D|  
