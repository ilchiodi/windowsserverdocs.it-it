---
ms.assetid: ba28bd05-16e6-465f-982b-df49633cfde4
title: Protezione dei controller di dominio dagli attacchi
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 06/18/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: a54e39cd25c0921516146c3f3b583bf4a3dbe467
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80821044"
---
# <a name="securing-domain-controllers-against-attack"></a>Protezione dei controller di dominio dagli attacchi

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Legge numero 3: se un utente malintenzionato ha accesso fisico illimitato al computer, non è più il computer.* - [dieci leggi di sicurezza non modificabili (versione 2,0)](https://technet.microsoft.com/security/hh278941.aspx)  
  
I controller di dominio forniscono lo spazio di archiviazione fisico per il database di servizi di dominio Active Directory, oltre a fornire i servizi e i dati che consentono alle aziende di gestire in modo efficace i server, le workstation, gli utenti e le applicazioni. Se l'accesso con privilegi a un controller di dominio viene ottenuto da un utente malintenzionato, l'utente può modificare, danneggiare o eliminare definitivamente il database di servizi di dominio Active Directory e, per estensione, tutti i sistemi e gli account gestiti da Active Directory.  
  
Poiché i controller di dominio possono leggere e scrivere nel database di servizi di dominio Active Directory, la compromissione di un controller di dominio significa che la foresta Active Directory non può mai essere considerata di nuovo attendibile a meno che non sia possibile eseguire il ripristino utilizzando un backup valido noto e chiudere i gap che hanno consentito il compromesso nel processo.  
  
A seconda della preparazione, degli strumenti e delle competenze di un utente malintenzionato, la modifica o persino danni irreparabili al database di servizi di dominio Active Directory può essere completata in pochi minuti a ore, non in giorni o settimane. Ciò che conta non è il tempo per cui un utente malintenzionato ha accesso con privilegi ai Active Directory, ma quanto l'autore dell'attacco ha pianificato il momento in cui si ottiene l'accesso con privilegi. Compromettere un controller di dominio può fornire il percorso più vantaggioso per la propagazione dell'accesso su larga scala o il percorso più diretto per la distruzione di server membri, workstation e Active Directory. Per questo motivo, i controller di dominio devono essere protetti separatamente e in modo più rigoroso rispetto all'infrastruttura generale di Windows.  

## <a name="physical-security-for-domain-controllers"></a>Sicurezza fisica per i controller di dominio

In questa sezione vengono fornite informazioni su come proteggere fisicamente i controller di dominio, se i controller di dominio sono macchine fisiche o virtuali, in posizioni dei centri dati, succursali e persino posizioni remote con solo controlli di infrastruttura di base.  
  
### <a name="datacenter-domain-controllers"></a>Controller di dominio del Data Center  
  
#### <a name="physical-domain-controllers"></a>Controller di dominio fisici

Nei data center, i controller di dominio fisici devono essere installati in rack o gabbie protette dedicate separate dal popolamento generale del server. Quando possibile, i controller di dominio devono essere configurati con i chip Trusted Platform Module (TPM) e tutti i volumi nei server del controller di dominio devono essere protetti tramite Crittografia unità BitLocker. BitLocker in genere aggiunge un sovraccarico delle prestazioni in percentuali a una sola cifra, ma protegge la directory da compromissione anche se i dischi vengono rimossi dal server. BitLocker consente inoltre di proteggere i sistemi da attacchi come i rootkit, perché la modifica dei file di avvio provocherà l'avvio del server in modalità di ripristino, in modo che sia possibile caricare i file binari originali. Se un controller di dominio è configurato per l'uso di RAID software, SCSI con connessione seriale, archiviazione SAN/NAS o volumi dinamici, non è possibile implementare BitLocker, quindi l'archiviazione collegata localmente (con o senza RAID hardware) deve essere usata nei controller di dominio laddove possibile.  
  
#### <a name="virtual-domain-controllers"></a>Controller di dominio virtuali 

Se si implementano i controller di dominio virtuali, è necessario assicurarsi che i controller di dominio vengano eseguiti in host fisici distinti rispetto ad altre macchine virtuali nell'ambiente. Anche se si usa una piattaforma di virtualizzazione di terze parti, è consigliabile distribuire controller di dominio virtuali nel server Hyper-V in Windows Server 2012 o Windows Server 2008 R2, che fornisce una superficie di attacco minima e può essere gestita con i controller di dominio che ospita, anziché essere gestita con gli altri host di virtualizzazione. Se si implementa System Center Virtual Machine Manager (SCVMM) per la gestione dell'infrastruttura di virtualizzazione, è possibile delegare l'amministrazione per gli host fisici in cui risiedono le macchine virtuali del controller di dominio e i controller di dominio per gli amministratori autorizzati. È inoltre consigliabile separare l'archiviazione dei controller di dominio virtuali per impedire agli amministratori di archiviazione di accedere ai file della macchina virtuale.  
  
### <a name="branch-locations"></a>Località del ramo  
  
#### <a name="physical-domain-controllers-in-branches"></a>Controller di dominio fisici nei rami

Nei percorsi in cui risiedono più server ma che non sono fisicamente protetti per quanto riguarda la protezione dei server dei Data Center, i controller di dominio fisici devono essere configurati con chip TPM e Crittografia unità BitLocker per tutti i volumi del server. Se un controller di dominio non può essere archiviato in una stanza bloccata in posizioni del ramo, è consigliabile distribuire RODC in tali percorsi.  
  
#### <a name="virtual-domain-controllers-in-branches"></a>Controller di dominio virtuali nei rami

Quando possibile, è consigliabile eseguire controller di dominio virtuali in succursali in host fisici distinti rispetto alle altre macchine virtuali nel sito. Nelle succursali in cui i controller di dominio virtuali non possono essere eseguiti in host fisici distinti dal resto del popolamento del server virtuale, è necessario implementare i chip TPM e Crittografia unità BitLocker in host in cui i controller di dominio virtuali vengono eseguiti come minimo e tutti gli host, se possibile. A seconda delle dimensioni della succursale e della protezione degli host fisici, è opportuno considerare la distribuzione di RODC in percorsi di rami.  
  
### <a name="remote-locations-with-limited-space-and-security"></a>Posizioni remote con spazio e sicurezza limitate

Se l'infrastruttura include percorsi in cui è possibile installare un solo server fisico, è necessario installare un server in grado di eseguire carichi di lavoro di virtualizzazione nella posizione remota e Crittografia unità BitLocker necessario configurare per proteggere tutti i volumi nel server. Una macchina virtuale nel server deve eseguire un RODC, con altri server in esecuzione come macchine virtuali separate nell'host. Per informazioni sulla pianificazione della distribuzione del controller di dominio di [sola lettura, vedere la guida alla pianificazione e distribuzione del controller di dominio di sola lettura](https://go.microsoft.com/fwlink/?LinkID=135993). Per ulteriori informazioni sulla distribuzione e la protezione dei controller di dominio virtualizzati, vedere [esecuzione di controller di dominio in Hyper-V nel](https://technet.microsoft.com/library/dd363553(v=ws.10).aspx) sito Web TechNet. Per istruzioni più dettagliate per la protezione avanzata di Hyper-V, la delega della gestione delle macchine virtuali e la protezione delle macchine virtuali, vedere la pagina relativa alla [Guida alla sicurezza di Hyper-v](https://www.microsoft.com/download/details.aspx?id=16650) nel sito Web Microsoft.  
  
## <a name="domain-controller-operating-systems"></a>Sistemi operativi dei controller di dominio

È consigliabile eseguire tutti i controller di dominio nella versione più recente di Windows Server supportata all'interno dell'organizzazione e definire le priorità per la rimozione delle autorizzazioni dei sistemi operativi legacy nella popolazione del controller di dominio. Mantenendo aggiornati i controller di dominio ed eliminando i controller di dominio legacy, è spesso possibile trarre vantaggio dalle nuove funzionalità e dalla sicurezza che potrebbero non essere disponibili in domini o foreste con controller di dominio che eseguono sistemi operativi legacy. I controller di dominio devono essere installati e innalzati di nuovo anziché essere aggiornati dai precedenti sistemi operativi o ruoli del server. ovvero, non eseguire aggiornamenti sul posto dei controller di dominio o eseguire l'installazione guidata di servizi di dominio Active Directory nei server in cui il sistema operativo non è installato in modo aggiornato. Implementando i controller di dominio appena installati, si garantisce che i file e le impostazioni legacy non vengano inavvertitamente lasciati nei controller di dominio e si semplifica l'applicazione della configurazione del controller di dominio coerente e sicuro.  
  
## <a name="secure-configuration-of-domain-controllers"></a>Configurazione sicura dei controller di dominio

Per creare una linea di base di configurazione di sicurezza iniziale per i controller di dominio che possono essere applicati dagli oggetti Criteri di gruppo, è possibile usare alcuni strumenti disponibili gratuitamente, alcuni dei quali sono installati per impostazione predefinita in Windows. Questi strumenti sono descritti qui.  
  
### <a name="security-configuration-wizard"></a>Configurazione guidata impostazioni di sicurezza  

Tutti i controller di dominio devono essere bloccati alla compilazione iniziale. Questa operazione può essere eseguita tramite la configurazione guidata impostazioni di sicurezza fornita in modo nativo in Windows Server per configurare le impostazioni di servizio, registro di sistema e WFAS in un controller di dominio di compilazione di base. Le impostazioni possono essere salvate ed esportate in un oggetto Criteri di gruppo che può essere collegato all'unità organizzativa controller di dominio in ogni dominio della foresta per applicare una configurazione coerente dei controller di dominio. Se il dominio contiene più versioni dei sistemi operativi Windows, è possibile configurare i filtri Strumentazione gestione Windows (WMI) per applicare gli oggetti Criteri di gruppo solo ai controller di dominio che eseguono la versione corrispondente del sistema operativo.  
  
### <a name="microsoft-security-compliance-toolkit"></a>Microsoft Security Compliance Toolkit

Le impostazioni del controller di dominio di [Microsoft Security Compliance Toolkit](https://www.microsoft.com/download/details.aspx?id=55319) possono essere combinate con le impostazioni della configurazione guidata impostazioni di sicurezza per produrre linee di base di configurazione complete per i controller di dominio distribuiti e applicati dagli oggetti Criteri di gruppo distribuiti nell'unità organizzativa Domain Controllers in Active Directory.  
  
### <a name="rdp-restrictions"></a>Restrizioni RDP

Criteri di gruppo gli oggetti che si collegano a tutti i controller di dominio ou in una foresta devono essere configurati per consentire le connessioni RDP solo da utenti e sistemi autorizzati (ad esempio, Jump Server). Questa operazione può essere eseguita tramite una combinazione di impostazioni di diritti utente e di configurazione di WFAS e deve essere implementata negli oggetti Criteri di gruppo in modo che i criteri vengano applicati in modo coerente. Se viene ignorato, il successivo aggiornamento Criteri di gruppo restituisce al sistema la configurazione corretta.  
  
### <a name="patch-and-configuration-management-for-domain-controllers"></a>Gestione delle patch e della configurazione per i controller di dominio

Sebbene possa sembrare illogico, è consigliabile prendere in considerazione l'applicazione di patch ai controller di dominio e ad altri componenti di infrastruttura critici separatamente dall'infrastruttura generale di Windows. Se si utilizza il software di gestione della configurazione aziendale per tutti i computer dell'infrastruttura, la compromissione del software di gestione dei sistemi può essere utilizzata per compromettere o eliminare tutti i componenti dell'infrastruttura gestiti da tale software. Separando la gestione delle patch e dei sistemi per i controller di dominio dal popolamento generale, è possibile ridurre la quantità di software installato nei controller di dominio, oltre a controllare rigorosamente la gestione.
  
### <a name="blocking-internet-access-for-domain-controllers"></a>Blocco dell'accesso a Internet per i controller di dominio  

Uno dei controlli eseguiti come parte di un Active Directory assessment di sicurezza è l'uso e la configurazione di Internet Explorer nei controller di dominio. Internet Explorer (o qualsiasi altro Web browser) non deve essere utilizzato nei controller di dominio, ma l'analisi di migliaia di controller di dominio ha rilevato numerosi casi in cui gli utenti con privilegi hanno utilizzato Internet Explorer per esplorare la rete Intranet o Internet dell'organizzazione.  
  
Come descritto in precedenza nella sezione "errata configurazione" delle [vie per compromettere](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md), esplorare Internet (o una rete Intranet infettata) da uno dei computer più potenti in un'infrastruttura Windows utilizzando un account con privilegi elevati (gli unici account autorizzati ad accedere localmente ai controller di dominio per impostazione predefinita) presentano un rischio straordinario per la sicurezza di un'organizzazione. Sia tramite un'unità scaricata che tramite download di "utilità" infette da malware, gli utenti malintenzionati possono ottenere l'accesso a tutti gli elementi necessari per compromettere o distruggere completamente l'ambiente Active Directory.  
  
Sebbene Windows Server 2012, Windows Server 2008 R2, Windows Server 2008 e le versioni correnti di Internet Explorer offrano un certo numero di protezioni contro i download dannosi, nella maggior parte dei casi in cui i controller di dominio e gli account con privilegi venivano usati per esplorare Internet, i controller di dominio eseguivano Windows Server 2003 oppure le protezioni offerte da più sistemi operativi e browser erano intenzionalmente disabilitate.  
  
L'avvio di Web browser nei controller di dominio deve essere vietato non solo tramite criteri, ma tramite controlli tecnici e i controller di dominio non devono essere autorizzati ad accedere a Internet. Se i controller di dominio devono essere replicati in più siti, è necessario implementare connessioni sicure tra i siti. Sebbene le istruzioni dettagliate per la configurazione esulano dall'ambito di questo documento, è possibile implementare una serie di controlli per limitare la possibilità che i controller di dominio vengano usati in modo improprio o non configurati correttamente e successivamente compromessi.  
  
### <a name="perimeter-firewall-restrictions"></a>Restrizioni del firewall perimetrale

È necessario configurare i firewall perimetrali per bloccare le connessioni in uscita dai controller di dominio a Internet. Sebbene i controller di dominio possano avere la necessità di comunicare attraverso i limiti del sito, è possibile configurare i firewall perimetrali per consentire la comunicazione tra siti seguendo le linee guida fornite in [come configurare un firewall per domini e trust](https://support.microsoft.com/kb/179442) nel sito Web supporto tecnico Microsoft.  
  
### <a name="dc-firewall-configurations"></a>Configurazioni del firewall del controller di dominio  

Come descritto in precedenza, è consigliabile usare la configurazione guidata impostazioni di sicurezza per acquisire le impostazioni di configurazione per il Windows Firewall con sicurezza avanzata nei controller di dominio. Esaminare l'output di configurazione guidata impostazioni di sicurezza per assicurarsi che le impostazioni di configurazione del firewall soddisfino i requisiti dell'organizzazione e quindi usare gli oggetti Criteri di gruppo per applicare le impostazioni di configurazione.  
  
### <a name="preventing-web-browsing-from-domain-controllers"></a>Prevenzione dell'esplorazione Web dai controller di dominio

È possibile utilizzare una combinazione di configurazione di AppLocker, configurazione proxy "Black Hole" e configurazione WFAS per impedire ai controller di dominio di accedere a Internet e impedire l'utilizzo di Web browser nei controller di dominio.
