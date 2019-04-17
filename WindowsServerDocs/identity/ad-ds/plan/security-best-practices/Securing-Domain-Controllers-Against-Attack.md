---
ms.assetid: ba28bd05-16e6-465f-982b-df49633cfde4
title: Protezione dei controller di dominio dagli attacchi
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: fc40d0ac536b128b799006214e360fa4d991ee44
ms.sourcegitcommit: 06a84f5caeab49b8480d6eed037aebbb59c77a9f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/14/2018
---
# <a name="securing-domain-controllers-against-attack"></a>Protezione dei controller di dominio dagli attacchi

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Legge numero tre: se un utente malintenzionato ha accesso fisico illimitato al computer, non è il computer più.* - [Ten Immutable Laws of Security (Version 2.0)](https://technet.microsoft.com/security/hh278941.aspx)  
  
Controller di dominio forniscono l'archiviazione fisica per il database di Active Directory, oltre a fornire i servizi e i dati che consentono alle organizzazioni di gestire in modo efficiente i server, workstation, gli utenti e applicazioni. Se l'accesso con privilegi per un controller di dominio viene ottenuto da un utente malintenzionato, tale utente può modificare, danneggiare o distruggere il database di Active Directory e, di conseguenza, tutti i sistemi e gli account che sono gestiti da Active Directory.  
  
Poiché i controller di dominio possono leggere e scrivere su un valore nel database di Active Directory, compromissione di un controller di dominio significa che la foresta di Active Directory non può mai essere considerata attendibile nuovamente a meno che non si è in grado di ripristinare utilizzando un backup considerato valido e chiudere le lacune consentiti la compromissione del processo.  
  
A seconda delle modifiche di preparazione, strumenti e competenze, un utente malintenzionato o danni anche irreversibili per il dominio di Active Directory database può essere completato in minuti, ore, non giorni o settimane. Ciò che non questioni quanto tempo un utente malintenzionato ha con privilegi di accesso ad Active Directory, ma viene ottenuto quanto l'utente malintenzionato ha pianificato per il momento durante l'accesso con privilegi. Compromettere un controller di dominio può fornire il percorso più vantaggioso propagazione su larga scala di accesso o il percorso più diretto alla distruzione di Active Directory, server membri e workstation. Per questo motivo, i controller di dominio devono essere protetti più rigorosamente rispetto l'infrastruttura generale di Windows e separatamente.  

  
## <a name="physical-security-for-domain-controllers"></a>Sicurezza fisica per i controller di dominio  
In questa sezione fornisce informazioni su come proteggere fisicamente i controller di dominio, se i controller di dominio sono fisici o macchine virtuali, in percorsi di Data Center, le succursali e sedi remote anche con solo i controlli di infrastruttura di base.  
  
### <a name="datacenter-domain-controllers"></a>Controller di dominio di Data Center  
  
#### <a name="physical-domain-controllers"></a>Controller di dominio fisici  
In Data Center, controller di dominio fisici deve essere installato in dedicato sicuro rack o gabbie separati dalla popolazione server generale. Quando possibile, i controller di dominio devono essere configurati con chip Trusted Platform Module (TPM) e tutti i volumi nei server di controller di dominio devono essere protette tramite crittografia unità BitLocker. In genere, BitLocker aggiunge overhead delle prestazioni in percentuale cifra ma protegge la directory dagli attacchi, anche se i dischi vengono rimossi dal server. BitLocker consente inoltre di proteggere i sistemi contro gli attacchi, ad esempio i rootkit perché il server per l'avvio in modalità di ripristino in modo che i file binari originali possono essere caricati causa la modifica dei file di avvio. Se un controller di dominio è configurato per utilizzare RAID, serial-attached SCSI, archiviazione SAN o NAS, software o i volumi dinamici, BitLocker non è possibile implementare, è opportuno utilizzare l'archiviazione in modo collegati in locale (con o senza RAID hardware) nei controller di dominio quando possibile.  
  
#### <a name="virtual-domain-controllers"></a>Controller di dominio virtuali  
Se si implementano i controller di dominio virtuale, è necessario assicurarsi che i controller di dominio eseguano in host fisici separati di altre macchine virtuali nell'ambiente. Anche se si usa una piattaforma di virtualizzazione di terze parti, valutare la distribuzione di controller di dominio virtuali nel Server Hyper-V in Windows Server 2012 o Windows Server 2008 R2, che fornisce una superficie di attacco minima e può essere gestito con i controller di dominio che ospita invece gestite con il resto degli host di virtualizzazione. Se si implementano System Center Virtual Machine Manager (SCVMM) per la gestione dell'infrastruttura di virtualizzazione, è possibile delegare l'amministrazione per gli host fisici in cui risiedono le macchine virtuali controller di dominio e controller di dominio stessi agli amministratori autorizzati. È inoltre consigliabile separare l'archiviazione dei controller di dominio virtuali per impedire agli amministratori di sistema di accedere ai file di macchina virtuale.  
  
### <a name="branch-locations"></a>Succursali  
  
#### <a name="physical-domain-controllers"></a>Controller di dominio fisici  
In posizioni in cui si trovano più server ma non sono protette fisicamente a livello di server datacenter sono protetti, i controller di dominio fisico devono essere configurati con chip TPM e crittografia unità BitLocker per tutti i volumi di server. Se un controller di dominio non può essere archiviato in una stanza nelle succursali, è consigliabile distribuire RODC in tali posizioni.  
  
#### <a name="virtual-domain-controllers"></a>Controller di dominio virtuali  
Ogni volta che è possibile, è necessario eseguire i controller di dominio virtuali nelle succursali in host fisici separati di altre macchine virtuali nel sito. Nelle succursali in cui i controller di dominio virtuale non possono eseguire in host fisici separati dal resto della popolazione server virtuale, è necessario implementare chip TPM e crittografia unità BitLocker su host in cui i controller di dominio virtuale eseguano almeno e tutti gli host se possibile. A seconda le dimensioni delle succursali e la protezione degli host fisici, è consigliabile distribuire lettura in succursali.  
  
### <a name="remote-locations-with-limited-space-and-security"></a>Posizioni remote con spazio limitato e sicurezza  
Se l'infrastruttura include percorsi in cui può essere installato solo un singolo server fisico, un server in grado di eseguire i carichi di lavoro di virtualizzazione deve essere installato in posizione remota e crittografia unità BitLocker deve essere configurata per proteggere tutti i volumi nel server. Una macchina virtuale nel server deve essere eseguita una sola lettura, con altri server separato le macchine virtuali in esecuzione nell'host. Informazioni sulla pianificazione per la distribuzione di controller di dominio viene fornita nel [pianificazione del Controller di dominio di sola lettura e Guida alla distribuzione](https://go.microsoft.com/fwlink/?LinkID=135993). Per ulteriori informazioni sulla distribuzione e proteggere i controller di dominio virtualizzati, vedere [i controller di dominio in esecuzione in Hyper-V](https://technet.microsoft.com/library/dd363553(v=ws.10).aspx) sul sito Web TechNet. Per ulteriori linee guida per la protezione di Hyper-V, la delega della gestione di macchine virtuali e protezione delle macchine virtuali, vedere il [Guida alla sicurezza di Hyper-V](https://www.microsoft.com/download/details.aspx?id=16650) Solution Accelerator del sito Web Microsoft.  
  
## <a name="domain-controller-operating-systems"></a>Sistemi operativi di Controller di dominio  
La versione più recente di Windows Server è supportata all'interno dell'organizzazione e definire le priorità rimozione delle autorizzazioni dei sistemi operativi legacy della popolazione di controller di dominio, è consigliabile eseguire tutti i controller di dominio. Mantenendo i controller di dominio corrente ed eliminando legacy controller di dominio, è possibile spesso sfruttare nuove funzionalità e della sicurezza che potrebbero non essere disponibile in domini o foreste con controller di dominio che eseguono il sistema operativo precedente. I controller di dominio devono essere appena installati e promosso anziché aggiornati da sistemi operativi precedenti o i ruoli del server; ovvero, non eseguire aggiornamenti sul posto di controller di dominio o eseguire l'installazione guidata di Active Directory nei server in cui il sistema operativo non è appena installato. Mediante l'implementazione di controller di dominio appena installato, verificare legacy file e le impostazioni non vengono inavvertitamente lasciate nei controller di dominio, che semplifica l'imposizione di configurazione del controller di dominio coerente e sicuro.  
  
## <a name="secure-configuration-of-domain-controllers"></a>Configurazione sicura dei controller di dominio  
Un numero di strumenti disponibili gratuitamente, alcune delle quali sono installati per impostazione predefinita in Windows, consente di creare una previsione di configurazione iniziale di sicurezza per i controller di dominio che può essere applicata successivamente dalla oggetti Criteri di gruppo. Questi strumenti sono descritti di seguito.  
  
### <a name="security-configuration-wizard"></a>Configurazione guidata impostazioni di sicurezza  

Tutti i controller di dominio devono essere bloccati verso il basso al momento della compilazione iniziali. Ciò può essere ottenuto tramite la configurazione guidata sicurezza fornito in modo nativo in Windows Server, configurare le impostazioni di WFAS, del Registro di sistema, sistema e del servizio in un controller di dominio "build di base". Le impostazioni possono essere salvate ed esportate in un oggetto Criteri di gruppo può essere collegato all'unità Organizzativa controller di dominio in ogni dominio della foresta per applicare la coerenza della configurazione dei controller di dominio. Se il dominio contiene più versioni dei sistemi operativi Windows, è possibile configurare i filtri di Strumentazione gestione Windows (WMI) per applicare oggetti Criteri di gruppo solo ai controller di dominio che eseguono la versione del sistema operativo corrispondente.  
  
### <a name="microsoft-security-compliance-manager"></a>Microsoft Security Compliance Manager  
[Microsoft Security Compliance Manager](https://technet.microsoft.com/library/cc677002.aspx) impostazioni controller di dominio possono essere combinate con le impostazioni di configurazione guidata impostazioni di sicurezza per produrre le linee di base di configurazione completi per i controller di dominio che vengono distribuiti e applicati da oggetti Criteri di gruppo distribuito nell'unità organizzativa controller di dominio in Active Directory.  
  
### <a name="applocker"></a>AppLocker  
AppLocker o uno strumento whitelist applicazione di terze parti deve essere utilizzato per configurare i servizi e applicazioni che sono autorizzate a eseguire nei controller di dominio e questi servizi e applicazioni consentite devono essere costituiti solo cosa è necessario che il computer host di dominio Active Directory e DNS probabilmente più del software di protezione di sistema, ad esempio software antivirus. Per whitelist consentiti dell'applicazione per i controller di dominio, viene aggiunto un ulteriore livello di sicurezza, in modo che anche se un'applicazione non autorizzata è installata su un controller di dominio, non può eseguire l'applicazione.  
  
### <a name="rdp-restrictions"></a>Restrizioni per RDP  
Oggetti Criteri di gruppo che si collegano a tutti i controller di dominio unità organizzative in una foresta deve essere configurati per consentire le connessioni RDP solo da utenti autorizzati e sistemi (ad esempio, jump server). Questo può essere ottenuto tramite una combinazione di impostazioni di diritti utente e la configurazione WFAS e deve essere implementato in oggetti Criteri di gruppo in modo che il criterio viene applicato in modo coerente. Se si viene ignorato, al successivo aggiornamento di criteri di gruppo restituisce il sistema per la configurazione corretta.  
  
### <a name="patch-and-configuration-management-for-domain-controllers"></a>Patch e gestione della configurazione per i controller di dominio  
Anche se potrebbe sembrare illogico, è necessario considerare l'applicazione di patch controller di dominio e altri componenti critici dell'infrastruttura separatamente dall'infrastruttura generale di Windows. Se si utilizzano software di Gestione configurazione aziendale per tutti i computer nell'infrastruttura, compromissione del software di gestione di sistemi può essere utilizzato per danneggiare o distruggere tutti i componenti dell'infrastruttura gestiti da tale software. Separando patch e sistemi di gestione per i controller di dominio al pubblico generale, è possibile ridurre la quantità di software installato nei controller di dominio, oltre a controllare attentamente la gestione.  
  
### <a name="blocking-internet-access-for-domain-controllers"></a>Bloccare l'accesso a Internet per i controller di dominio  

Uno dei controlli che viene eseguita come parte di una valutazione della sicurezza di Active Directory è l'utilizzo e la configurazione di Internet Explorer nei controller di dominio. Internet Explorer (o qualsiasi altro browser web) non deve essere utilizzato nei controller di dominio, ma l'analisi di migliaia di controller di dominio ha rivelato numerosi casi in cui gli utenti con privilegi utilizzato Internet Explorer per esplorare intranet dell'organizzazione o a Internet.  
  
Come descritto in precedenza nella sezione "Configurazione" di [esposizione a possibili violazioni](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md), esplorazione Internet (o una intranet infetta) da uno dei computer più potente in un'infrastruttura di Windows con un account con privilegiato elevati, che sono gli unici account può accedere localmente ai controller di dominio per impostazione predefinita, viene visualizzata un straordinario rischi per la sicurezza dell'organizzazione. Se tramite un'unità da scaricare o tramite download di "utilità" infettato da malware, gli utenti malintenzionati possono accedere a tutti i componenti che necessari completamente o distruggere l'ambiente Active Directory.  
  
Anche se Windows Server 2012, Windows Server 2008 R2, Windows Server 2008 e versioni correnti di Internet Explorer offrono un numero di protezioni download dannosi, nella maggior parte dei casi in cui dominio controller e gli account con privilegi fosse stati utilizzati per accedere a Internet, i controller di dominio erano in esecuzione Windows Server 2003 o era state disattivate intenzionalmente protezioni offerte dai sistemi operativi e browser più recenti.  
  
L'avvio di web browser nei controller di dominio deve essere vietato non solo dal criterio, ma tramite controlli tecnici e i controller di dominio devono essere consentiti l'accesso a Internet. Se i controller di dominio necessario per la replica tra siti, è necessario implementare connessioni protette tra i siti. Anche se le istruzioni di configurazione dettagliate non rientrano nell'ambito di questo documento, è possibile implementare un numero di controlli per limitare la capacità di controller di dominio utilizzato in modo errato o non configurato correttamente e successivamente compromesso.  
  
### <a name="perimeter-firewall-restrictions"></a>Restrizioni del Firewall perimetrale  
I firewall perimetrale devono essere configurati per bloccare le connessioni in uscita a Internet dai controller di dominio. Anche se i controller di dominio potrebbero essere necessario comunicare attraverso i limiti del sito, i firewall perimetrali può essere configurato per consentire la comunicazione tra siti seguendo le indicazioni fornite [come configurare un firewall per domini e trust](https://support.microsoft.com/kb/179442) nel sito Web del supporto Microsoft.  
  
### <a name="dc-firewall-configurations"></a>Configurazioni del Firewall controller di dominio  

Come descritto in precedenza, utilizzare la configurazione guidata impostazioni di sicurezza per acquisire le impostazioni di configurazione di Windows Firewall con sicurezza avanzata nei controller di dominio. È consigliabile esaminare l'output della configurazione guidata impostazioni di sicurezza per garantire che le impostazioni di configurazione di firewall soddisfano i requisiti dell'organizzazione e quindi usano oggetti Criteri di gruppo per applicare le impostazioni di configurazione.  
  
### <a name="preventing-web-browsing-from-domain-controllers"></a>Impedire l'esplorazione del Web dai controller di dominio  
È possibile utilizzare una combinazione di configurazione di AppLocker, "black hole" configurazione del proxy e configurazione WFAS per impedire l'accesso a Internet di controller di dominio e per impedire l'uso dei web browser nei controller di dominio.  
  


