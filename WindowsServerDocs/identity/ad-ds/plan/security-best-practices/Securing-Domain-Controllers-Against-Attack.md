---
ms.assetid: ba28bd05-16e6-465f-982b-df49633cfde4
title: Protezione dei controller di dominio dagli attacchi
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 06/18/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 6be2899e85b68578518d9de1805c287367608163
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863092"
---
# <a name="securing-domain-controllers-against-attack"></a>Protezione dei controller di dominio dagli attacchi

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Legge numero tre: Se un utente malintenzionato ha accesso fisico illimitato al computer in uso, non è il computer più.* - [10 leggi immutabili della protezione (versione 2.0)](https://technet.microsoft.com/security/hh278941.aspx)  
  
Controller di dominio forniscano l'archiviazione fisica per il database di Active Directory Domain Services, oltre a fornire i servizi e i dati che consentono alle aziende di gestire in modo efficiente i server, le workstation, gli utenti e applicazioni. Se l'accesso con privilegi a un controller di dominio viene ottenuto da un utente malintenzionato, tale utente può modificare, danneggiare o eliminare definitivamente il database di Active Directory Domain Services e, di conseguenza, tutti i sistemi e gli account gestiti da Active Directory.  
  
Poiché i controller di dominio possono leggere e scrivere su un valore qualsiasi nel database di Active Directory Domain Services, compromissione di un controller di dominio significa che la foresta Active Directory non può mai essere considerata trustworthy nuovamente a meno che non si è in grado di eseguire il ripristino utilizzando un backup considerato valido e a chiusura dei gap che consentite la compromissione del processo.  
  
A seconda della preparazione, strumenti e competenze, modifica o il danneggiamento anche irreversibile per il dominio di Active Directory un utente malintenzionato database può essere completato in pochi minuti, ore, non giorni o settimane. Ciò che non è importante quanto tempo un utente malintenzionato ha accesso ad Active Directory con privilegi, ma quanto l'autore dell'attacco ha pianificato per il momento durante l'accesso con privilegi viene ottenuto. Compromettere un controller di dominio, è possibile fornire la modalità più vantaggiosa per la propagazione di scalabilità a livello di accesso o il percorso più diretto per la distruzione di Active Directory, le workstation e server membri. Per questo motivo, i controller di dominio devono essere protetti separatamente e in modo più rigoroso rispetto dell'infrastruttura generale di Windows.  

## <a name="physical-security-for-domain-controllers"></a>Sicurezza fisica per i controller di dominio

Questa sezione vengono fornite informazioni sulla sicurezza fisica dei controller di dominio, se i controller di dominio sono fisici o macchine virtuali, in posizioni dei Data Center, succursali e sedi remote anche con solo i controlli di un'infrastruttura di base.  
  
### <a name="datacenter-domain-controllers"></a>Controller di dominio di datacenter  
  
#### <a name="physical-domain-controllers"></a>Controller di dominio fisici

In Data Center, i controller di dominio fisico devono essere installati in rack sicuro dedicato o cabine che sono separate dal popolamento generale del server. Quando possibile, i controller di dominio devono essere configurati con chip Trusted Platform Module (TPM) e tutti i volumi nei server di controller di dominio devono essere protette tramite crittografia unità BitLocker. In genere BitLocker sovraccarico delle prestazioni in percentuale a una cifra, ma consente di proteggere la directory dalle compromissioni anche se i dischi vengono rimossi dal server. BitLocker consente inoltre di proteggere i sistemi contro attacchi di tipo rootkit perché la modifica dei file di avvio causerà il server per l'avvio in modalità di ripristino in modo che i file binari originali possono essere caricati. Se un controller di dominio è configurato per l'uso di RAID, serial attached SCSI, archiviazione SAN/NAS, software o i volumi dinamici, BitLocker non possono essere implementati, è opportuno utilizzare l'archiviazione quindi collegato al computer locale (con o senza RAID hardware) nel controller di dominio ogni volta che possibili.  
  
#### <a name="virtual-domain-controllers"></a>Controller di dominio virtuali 

Se si implementano i controller di dominio virtuale, è necessario assicurarsi che i controller di dominio eseguano in host fisici separati di altre macchine virtuali nell'ambiente. Anche se si usa una piattaforma di virtualizzazione di terze parti, è consigliabile distribuire controller di dominio virtuali nel Server Hyper-V in Windows Server 2012 o Windows Server 2008 R2, che fornisce una superficie di attacco minima e può essere gestito con i controller di dominio che ospita invece di essere gestiti con il resto degli host di virtualizzazione. Se si implementa il servizio System Center Virtual Machine Manager (SCVMM) per la gestione dell'infrastruttura di virtualizzazione, è possibile delegare l'amministrazione per gli host fisici in cui si trovano le macchine virtuali controller di dominio e i controller di dominio stessi agli amministratori autorizzati. È inoltre consigliabile separare l'archiviazione dei controller di dominio virtuali per impedire agli amministratori di sistema di accedere ai file della macchina virtuale.  
  
### <a name="branch-locations"></a>Posizioni di ramo  
  
#### <a name="physical-domain-controllers-in-branches"></a>Controller di dominio fisici nei rami

In posizioni in cui si trovano più server ma non sono fisicamente protetti per il livello di server dei Data Center protetti, controller di dominio fisici devono essere configurate con chip TPM e crittografia unità BitLocker per tutti i volumi del server. Se un controller di dominio non può essere archiviato in un locale chiuso nelle succursali, è consigliabile distribuire i RODC in questi percorsi.  
  
#### <a name="virtual-domain-controllers-in-branches"></a>Controller di dominio virtuale in rami

Quando possibile, è consigliabile eseguire controller di dominio virtuali delle succursali in host fisici separati di altre macchine virtuali nel sito. Nelle succursali in cui non è possibile eseguire controller di dominio virtuali in host fisici separati dal resto della popolazione di server virtuale, è necessario implementare i chip TPM e BitLocker Drive Encryption negli host in cui i controller di dominio virtuale eseguano come minimo, e tutti gli host se possibile. A seconda delle dimensioni delle succursali e la protezione degli host fisici, è necessario considerare la distribuzione dei controller in succursali.  
  
### <a name="remote-locations-with-limited-space-and-security"></a>Sedi remote con lo spazio limitato e sicurezza

Se l'infrastruttura include percorsi in cui è possibile installare solo un singolo server fisico, un server in grado di eseguire i carichi di lavoro di virtualizzazione deve essere installato nel percorso remoto e BitLocker Drive Encryption deve essere configurato per proteggere tutto volumi nel server. Una macchina virtuale nel server deve essere eseguito un RODC, con altri server in esecuzione come macchine virtuali separate dell'host. Informazioni sulla pianificazione per la distribuzione di controller di dominio è disponibile nel [Guida alla distribuzione e pianificazione del Controller di dominio di sola lettura](https://go.microsoft.com/fwlink/?LinkID=135993). Per altre informazioni sulla distribuzione e protezione dei controller di dominio virtualizzati, vedere [esecuzione di controller di dominio in Hyper-V](https://technet.microsoft.com/library/dd363553(v=ws.10).aspx) sul sito Web TechNet. Per informazioni più dettagliate per la protezione avanzata di Hyper-V, la delega della gestione di macchine virtuali e la protezione di macchine virtuali, vedere la [Guida alla protezione di Hyper-V](https://www.microsoft.com/download/details.aspx?id=16650) Solution Accelerator del sito Web Microsoft.  
  
## <a name="domain-controller-operating-systems"></a>Sistemi operativi dei controller di dominio

La versione più recente di Windows Server è supportata all'interno dell'organizzazione e definire le priorità rimozione delle autorizzazioni di sistemi operativi legacy della popolazione di controller di dominio, è consigliabile eseguire tutti i controller di dominio. Mantenendo i controller di dominio controller di dominio legacy corrente ed eliminando, è spesso possono sfruttare i vantaggi di nuove funzionalità e la sicurezza che potrebbero non essere disponibile in domini o foreste con controller di dominio che eseguono il sistema operativo legacy. I controller di dominio devono essere appena installati e alzate di livello anziché l'aggiornamento da sistemi operativi precedenti o ruoli del server; vale a dire, non eseguire aggiornamenti sul posto di controller di dominio o eseguire l'installazione guidata di AD DS nei server in cui il sistema operativo non è appena installato. Implementando i controller di dominio appena installata, assicurarsi che le impostazioni e file legacy non vengono inavvertitamente lasciate nei controller di dominio e si semplifica l'imposizione di configurazione del controller di dominio coerente e sicura.  
  
## <a name="secure-configuration-of-domain-controllers"></a>Configurazione protetta dei controller di dominio

Una serie di strumenti disponibili gratuitamente, alcuni dei quali sono installati per impostazione predefinita in Windows, è utilizzabile per creare una configurazione di base di sicurezza iniziali per i controller di dominio che successivamente può essere applicata dalla oggetti Criteri di gruppo. Questi strumenti sono descritti di seguito.  
  
### <a name="security-configuration-wizard"></a>Configurazione guidata impostazioni di sicurezza  

Tutti i controller di dominio devono essere bloccati in fase di compilazione iniziale. Ciò può essere ottenuto tramite la configurazione guidata sicurezza fornita in modo nativo in Windows Server per configurare servizio, del Registro di sistema, del sistema e impostazioni WFAS in un controller di dominio "build di base". Impostazioni possono essere salvate ed esportate in un oggetto Criteri di gruppo che possono essere collegati all'unità Organizzativa controller di dominio in ogni dominio nella foresta per imporre la coerenza della configurazione di controller di dominio. Se il dominio contiene più versioni dei sistemi operativi Windows, è possibile configurare i filtri Strumentazione gestione Windows (WMI) per applicare oggetti Criteri di gruppo solo ai controller di dominio che eseguono la versione corrispondente del sistema operativo.  
  
### <a name="microsoft-security-compliance-toolkit"></a>Toolkit di conformità di Microsoft Security

[Microsoft Security Compliance Toolkit](https://www.microsoft.com/download/details.aspx?id=55319) impostazioni controller di dominio possono essere combinate con le impostazioni di configurazione guidata sicurezza per produrre linee di base di configurazione completi per i controller di dominio che sono distribuiti e applicati da oggetti Criteri di gruppo distribuito all'unità Organizzativa controller di dominio Active Directory.  
  
### <a name="rdp-restrictions"></a>Restrizioni di RDP

Oggetti Criteri di gruppo che si collegano a tutte le unità organizzative in una foresta i controller di dominio deve essere configurati per consentire le connessioni RDP solo da utenti autorizzati e i sistemi (ad esempio, jump server). Ciò può essere ottenuto tramite una combinazione di impostazioni di diritti utente e la configurazione WFAS e deve essere implementata in oggetti Criteri di gruppo in modo che il criterio viene applicato in modo coerente. Se viene ignorato, al successivo aggiornamento di criteri di gruppo restituirà il sistema per la corretta configurazione.  
  
### <a name="patch-and-configuration-management-for-domain-controllers"></a>Gestione di patch e gestione della configurazione per i controller di dominio

Anche se potrebbe sembrare illogico, è opportuno considerare l'applicazione di patch controller di dominio e altri componenti critici dell'infrastruttura separatamente dall'infrastruttura generale di Windows. Se si utilizzano software di Gestione configurazione aziendale per tutti i computer dell'infrastruttura, compromissione di tale software è utilizzabile per compromettere o distruggere tutti i componenti di infrastruttura gestiti da tale software. Separando la gestione delle patch e i sistemi per i controller di dominio dal popolamento generale, è possibile ridurre la quantità di software installato nei controller di dominio, oltre a controllare strettamente della loro gestione.
  
### <a name="blocking-internet-access-for-domain-controllers"></a>Blocca l'accesso a Internet per i controller di dominio  

Uno dei controlli che viene eseguita come parte di una valutazione sicurezza Active Directory è l'uso e la configurazione di Internet Explorer nei controller di dominio. Internet Explorer (o qualsiasi altro web browser) non deve essere utilizzato nei controller di dominio, ma l'analisi di migliaia di controller di dominio ha rivelato numerosi casi in cui gli utenti con privilegi usato Internet Explorer per esplorare la rete intranet aziendale o Internet.  
  
Come descritto in precedenza nella sezione "Configurazione" del [esposizione a possibili violazioni](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md), esplorazione Internet (o una intranet infetta) in uno dei computer più potenti in un'infrastruttura di Windows usando un con privilegi elevati account (che sono gli unici account può accedere localmente ai controller di dominio per impostazione predefinita) presenta un rischio straordinario per la sicurezza dell'organizzazione. Se tramite un'unità tramite download o dal download di infetti da malware "utilità", gli utenti malintenzionati possono ottenere l'accesso a tutto il che necessario completamente danneggiare o distruggere l'ambiente Active Directory.  
  
Anche se Windows Server 2012, Windows Server 2008 R2, Windows Server 2008 e versioni correnti di Internet Explorer offrono numerosi meccanismi di download dannosi, nella maggior parte dei casi in cui domain controller e gli account con privilegi fossero stati utilizzati per accedere a Internet, i controller di dominio erano in esecuzione Windows Server 2003 o le protezioni offerte da più recenti sistemi operativi e browser era state disabilitate intenzionalmente.  
  
L'avvio di web browser nei controller di dominio deve essere vietato non solo dai criteri, ma tramite controlli tecnici e i controller di dominio devono essere consentiti l'accesso a Internet. Se i controller di dominio è necessario eseguire la replica tra siti, è necessario implementare le connessioni sicure tra i siti. Anche se le istruzioni di configurazione dettagliati sono all'esterno dell'ambito di questo documento, è possibile implementare un numero di controlli per limitare la capacità del controller di dominio per essere usato in modo improprio o configurato correttamente e successivamente compromesso.  
  
### <a name="perimeter-firewall-restrictions"></a>Restrizioni del Firewall perimetrale

Firewall perimetrali devono essere configurati per bloccare le connessioni in uscita a Internet dai controller di dominio. Anche se i controller di dominio potrebbe essere necessario comunicare attraverso i limiti del sito, firewall perimetrali può essere configurato per consentire la comunicazione tra siti, seguire le linee guida fornite [come configurare un firewall per domini e trust](https://support.microsoft.com/kb/179442) sul sito Web supporto tecnico Microsoft.  
  
### <a name="dc-firewall-configurations"></a>Configurazioni del Firewall controller di dominio  

Come descritto in precedenza, è necessario utilizzare la configurazione guidata impostazioni di sicurezza per acquisire le impostazioni di configurazione di Windows Firewall con sicurezza avanzata nei controller di dominio. È consigliabile esaminare l'output della configurazione guidata sicurezza per garantire che soddisfino i requisiti dell'organizzazione le impostazioni di configurazione di firewall e quindi usano oggetti Criteri di gruppo per applicare le impostazioni di configurazione.  
  
### <a name="preventing-web-browsing-from-domain-controllers"></a>Impedisce l'esplorazione del Web dai controller di dominio

È possibile usare una combinazione di configurazione AppLocker e la configurazione del proxy "black hole" configurazione WFAS per impedire l'accesso a Internet di controller di dominio e per impedire l'uso del web browser nei controller di dominio.
