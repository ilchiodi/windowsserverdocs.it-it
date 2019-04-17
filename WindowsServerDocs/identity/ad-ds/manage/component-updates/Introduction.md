---
ms.assetid: 84754c23-f039-4de4-a378-853942e662df
title: Introduzione
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: dc89afc47eb78a388238e8edf5059b0bec3006ad
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="introduction"></a>Introduzione

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esistenti da attacchi alle infrastrutture informatiche, esse semplici o complesse, purché siano presenti computer. Tuttavia, all'interno dell'ultimo decennio, un numero crescente di organizzazioni di qualsiasi dimensione, in tutte le parti del mondo è state attacco e compromesso in modi che hanno modificato in modo significativo il panorama delle minacce. La guerra Cibernetica e il crimine informatico sono cresciuti con velocità record. "L'Hacktivism," in cui attacchi motivati da posizioni di attivismo sociale, è stato indicato come motivazione per svariate violazioni, destinate a esporre informazioni segrete delle organizzazioni, per creare rifiuti del servizio, o addirittura distruggere l'infrastruttura. Gli attacchi contro istituzioni pubbliche e private con l'obiettivo di divulgare la proprietà intellettuale dell'azienda (IP) sono diventati frequentissimi.  
  
Nessuna organizzazione con un'infrastruttura IT (IT) è immune agli attacchi, ma se implementati controlli, processi e criteri appropriati per proteggere i segmenti essenziali dell'infrastruttura di elaborazione di un'organizzazione, escalation degli attacchi dalla penetrazione al danno totale potrebbe essere inutili. Poiché il numero e alla scalabilità degli attacchi provenienti dall'esterno dell'organizzazione è sono minaccia interna negli ultimi anni, in questo documento viene spesso attacchi esterni piuttosto che l'utilizzo improprio dell'ambiente dagli utenti autorizzati. Tuttavia, i principi e i consigli forniti in questo documento sono destinati a proteggere l'ambiente da attacchi esterni e interni fuorviati o malintenzionati.  
  
Le informazioni e consigli forniti in questo documento sono disegnati da diverse origini e procedure specifiche per la protezione delle installazioni Active Directory dagli attacchi. Anche se non è possibile impedire gli attacchi, è possibile ridurre la superficie di attacco di Active Directory e implementare i controlli che rendono compromissione della directory molto più difficile per gli utenti malintenzionati. Questo documento presenta i tipi più comuni di vulnerabilità che abbiamo osservato in ambienti di compromessi e i consigli più comuni che sono stati apportati ai clienti per migliorare la sicurezza delle installazioni Active Directory.  
  
## <a name="account-and-group-naming-conventions"></a>Convenzioni di account e dei nomi di gruppo  
La tabella seguente fornisce una Guida alle convenzioni di denominazione utilizzata in questo documento per i gruppi e account a cui fa riferimento in tutto il documento. La tabella include il percorso di ogni account al gruppo, il nome e come questi account/gruppi viene fatto riferimento in questo documento.  
  


|**Percorso account al gruppo**|**Nome dell'Account al gruppo**|**Come viene fatto riferimento in questo documento**|
| --- | --- | --- |   
|Active Directory - ogni dominio|Amministratore|Account amministratore predefinito|  
|Active Directory - ogni dominio|Amministratori|Gruppo Administrators (BA) predefinito|  
|Active Directory - ogni dominio|Domain Admins|Gruppo di Domain Admins (DA)|  
|Active Directory - dominio radice della foresta|Enterprise Admins|Gruppo Enterprise Admins (EA)|  
|Database di gestione (SAM) gli account di sicurezza di computer locale nel computer che eseguono Windows Server e workstation che non sono controller di dominio|Amministratore|Account amministratore locale|  
|Database di gestione (SAM) gli account di sicurezza di computer locale nel computer che eseguono Windows Server e workstation che non sono controller di dominio|Amministratori|Gruppo Administrators locale|  
  
## <a name="about-this-document"></a>Su questo documento  
Il Microsoft Information Security organizzazione Risk Management (ISRM), che fa parte di Microsoft informazioni tecnologia (MSIT), funziona con le unità aziendali interne, clienti esterni e settore peer per raccogliere, distribuzione e definire criteri, procedure e controlli. Queste informazioni possono essere utilizzate da Microsoft e i nostri clienti per aumentare la sicurezza e ridurre la superficie di attacco di infrastrutture IT. Le indicazioni disponibili in questo documento sono basate su un numero di fonti di informazioni e procedure usate all'interno di MSIT e ISRM. Le sezioni seguenti presentano ulteriori informazioni sulle origini di questo documento.  
  
### <a name="microsoft-it-and-isrm"></a>Microsoft IT e ISRM  
Un numero di controlli e procedure consigliate è stato sviluppato all'interno di MSIT e ISRM per proteggere i domini e foreste Microsoft AD DS. In questi controlli sono applicabili su larga scala, sono state integrate in questo documento. SAFE-T (Solution Accelerator per nuove tecnologie) è un team all'interno di ISRM cui noleggio è per identificare nuove tecnologie e per definire i requisiti di sicurezza e controlli per accelerare l'adozione.  
  
### <a name="active-directory-security-assessments"></a>Valutazioni della sicurezza di Active Directory  
All'interno di Microsoft ISRM, la valutazione, Consulting e Team Engineering (ACE) funziona con interna Microsoft business unit e clienti esterni per valutare la protezione dell'infrastruttura e dell'applicazione e fornire le indicazioni di tipo tattica e strategiche per aumentare la condizione di sicurezza dell'organizzazione. Offerta di servizio una voce ACE è di Active Directory Security Assessment (ADSA), ovvero una valutazione olistica dell'ambiente di dominio Active Directory di un'organizzazione che consente di valutare le persone, processi e tecnologia e produce consigli specifici per i clienti. I clienti vengono forniti con suggerimenti che si basano su caratteristiche uniche dell'organizzazione, procedure consigliate e al desiderio di rischio. ADSAs sono stati eseguiti per le installazioni di Active Directory in Microsoft oltre a quelli dei nostri clienti. Nel corso del tempo, un numero di indicazioni è stato trovato è applicabile tra i clienti di diverse dimensioni e settori.  
  
### <a name="content-origin-and-organization"></a>Organizzazione e l'origine del contenuto  
Gran parte del contenuto di questo documento è derivato dal ADSA e valutazioni altri Team ACE eseguite per i clienti compromessi e i clienti che non si sono verificati compromissione significativo. Anche se i dati dei singoli clienti non è stati utilizzati per creare questo documento, abbiamo raccolto le vulnerabilità più sfruttate abbiamo identificato le valutazioni e i consigli sono stati apportati ai clienti per migliorare la sicurezza dell'installazione di dominio Active Directory. Non tutte le vulnerabilità sono applicabili a tutti gli ambienti, né sono tutte le indicazioni possono essere implementate in ogni organizzazione.  
  
Questo documento è organizzato come segue:  
  
## <a name="executive-summary"></a>Schema riepilogativo  
Il Executive riepilogo, che possono essere letti come documento autonomo o in combinazione con l'intero documento, fornisce un riepilogo di alto livello di questo documento. Nel riepilogo Executive sono inclusi i vettori di attacco più comuni che si è notato usati per compromettere gli ambienti dei clienti, riepiloghi consigli per la protezione delle installazioni Active Directory e gli obiettivi di base per i clienti che prevede di distribuire nuove foreste Active Directory ora o in futuro.  
  
### <a name="introduction"></a>Introduzione  
Si tratta della sezione che stai leggendo ora.  
  
### <a name="avenues-to-compromise"></a>Esposizione a possibili violazioni  
Questa sezione vengono fornite informazioni su alcune delle più comunemente utilizzati vulnerabilità abbiamo trovato per essere usati da utenti malintenzionati per compromettere le infrastrutture dei clienti. In questa sezione inizia con categorie generali di vulnerabilità e come essi vengono usati per penetrare inizialmente infrastrutture dei clienti, propagare i compromessi tra altri sistemi e alla fine di destinazione AD DS e controller di dominio per ottenere il controllo completo di insiemi di strutture delle organizzazioni.  
  
In questa sezione non fornisce indicazioni dettagliate su ogni tipo di vulnerabilità, in particolare nelle aree in cui le vulnerabilità non vengono utilizzate per Active Directory di destinazione direttamente gli indirizzi. Tuttavia, per ogni tipo di vulnerabilità, Microsoft fornisce collegamenti a informazioni aggiuntive che è possibile utilizzare per sviluppare contromisure e ridurre la superficie di attacco dell'organizzazione.  
  
### <a name="reducing-the-active-directory-attack-surface"></a>Ridurre la superficie di attacco di Active Directory  
In questa sezione inizia fornendo informazioni generali sull'account con privilegi e gruppi in Active Directory per fornire le informazioni che contribuiscono a chiarire i motivi per i successivi consigli per proteggere e gestire gli account e gruppi con privilegi elevati. Parleremo quindi approcci per ridurre la necessità di usare l'account con privilegi elevati per l'amministrazione quotidiana, che non richiede il livello di privilegi concesso ai gruppi, ad esempio i gruppi Enterprise Admins (EA), Domain Admins (DA) e incorporato Administrators (BA) in Active Directory. Successivamente, sono disponibili indicazioni per proteggere gli account e gruppi con privilegi e per l'implementazione di sistemi e procedure amministrative sicure.  
  
Sebbene in questa sezione fornisce informazioni dettagliate su queste impostazioni di configurazione, abbiamo incluso anche appendici per ogni suggerimento che forniscono istruzioni dettagliate per la configurazione che possono essere usate "è" o possono essere modificate per le esigenze dell'organizzazione. In questa sezione viene completato, fornendo informazioni per distribuire e gestire i controller di dominio, devono essere tra i sistemi protetti più rigorosamente nell'infrastruttura di in modo sicuro.  
  
### <a name="monitoring-active-directory-for-signs-of-compromise"></a>Monitoraggio dei segnali di compromissione di Active Directory  
Se si è implementato informazioni sulla protezione affidabile e monitoraggio (SIEM) nell'ambiente di eventi o utilizzano altri meccanismi per monitorare la protezione dell'infrastruttura, in questa sezione fornisce informazioni che possono essere utilizzate per identificare gli eventi nei sistemi Windows che possono indicare che un'organizzazione di violazione. Parleremo di criteri di controllo tradizionale e avanzate, tra cui configurazione efficace di sottocategorie di controllo nei sistemi operativi Windows 7 e Windows Vista. In questa sezione include un elenco completo dei sistemi di controllo e gli oggetti e un'associati appendice Elenca gli eventi per cui è necessario monitorare se l'obiettivo è di rilevare i tentativi di compromissione.  
  
### <a name="planning-for-compromise"></a>Pianificazione del compromesso  
In questa sezione inizia con "esecuzione back" da dettagli tecnici di concentrarsi su principi e i processi che possono essere implementati per identificare l'utenti, applicazioni e sistemi che sono più importanti non solo per l'infrastruttura IT, ma per l'azienda. Dopo avere identificato che cos'è più importanti per la stabilità e le operazioni dell'organizzazione, è possibile concentrarsi sulla separare e proteggere questi asset, siano essi sistemi, utenti o proprietà intellettuale. In alcuni casi, può essere eseguite separare e protezione delle risorse nell'ambiente di dominio Active Directory esistente, mentre in altri casi, è consigliabile implementare piccole dimensioni, separate "celle" che consentono di stabilire un limite di protezione per asset critici e monitorare le risorse in modo più rigoroso di componenti meno critico. Viene descritto il concetto di "creativa distruzione", che è un meccanismo mediante il quale sistemi e applicazioni legacy possono essere eliminati mediante la creazione di nuove soluzioni e la sezione termina con indicazioni che consentono di gestire un ambiente più sicuro grazie alla combinazione di business e le informazioni di IT per costruire una panoramica dettagliata di che cos'è un normale stato operativo. Conoscendo che cos'è normale per un'organizzazione, possono essere più facilmente identificate eventuali anomalie che potrebbero indicare gli attacchi e compromessi.  
  
### <a name="summary-of-best-practice-recommendations"></a>Riepilogo delle procedure consigliate  
Questa sezione viene fornita una tabella che riepiloga i suggerimenti forniti in questo documento e li ordina in base alla priorità relativa, oltre a fornire collegamenti a dove ulteriori informazioni su ogni indicazione sono reperibile nel documento e relativi appendici.  
  
### <a name="appendices"></a>Appendici  
Appendici sono inclusi in questo documento per potenziare le informazioni contenute nel corpo del documento. L'elenco di appendici e una breve descrizione di ogni è incluso nella tabella seguente.  
  
 
|**Appendice**|**Descrizione**|
| --- | --- | 
|[Appendice b: account con privilegi e gruppi in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)|Vengono fornite informazioni che consentono di identificare gli utenti e gruppi, che è consigliabile concentrarsi sulla protezione in quanto essi possono essere utilizzate dagli utenti malintenzionati di compromettere e persino distruggere l'installazione di Active Directory.|  
|[Appendice c: account protetti e gruppi in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)|Contiene informazioni sui gruppi protetti in Active Directory. Contiene inoltre informazioni per la personalizzazione limitata (rimozione) dei gruppi sono considerate gruppi protetti e vengono influenzati da AdminSDHolder e SDProp.|  
|[Appendice d: protezione predefiniti Administrator account in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)|Contiene le linee guida per proteggere l'account di amministratore in ogni dominio nella foresta.|  
|[Appendice e: protezione dei gruppi Enterprise Admins in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)|Contiene le linee guida per proteggere il gruppo Enterprise Admins nella foresta.|  
|[Appendice f: protezione Domain Admins gruppi in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)|Contiene le linee guida per proteggere il gruppo Domain Admins in ogni dominio nella foresta.|  
|[Appendice g: protezione dei gruppi Administrators in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)|Contiene le linee guida per proteggere il gruppo Administrators predefinito in ogni dominio nella foresta.|  
|[Appendice h: protezione Local Administrator account e gruppi](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)|Contiene le linee guida per la protezione account amministratore locale e gruppi di amministratori nei server appartenenti a un dominio e workstation.|  
|[Appendice i: creazione di gestione degli account per gli account protetti e gruppi in Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)|Fornisce informazioni per creare gli account che dispone di privilegi limitati e possono essere controllati rigorosamente, ma possono essere utilizzati per popolare i gruppi con privilegi in Active Directory quando è richiesta l'elevazione dei privilegi temporaneo.|  
|[Appendice l: eventi da monitorare](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)|Elenca gli eventi per cui è necessario monitorare nell'ambiente in uso.|  
|[Appendice m: collegamenti a documenti e letture consigliate](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)|Contiene un elenco di lettura consigliato. Contiene anche un elenco di collegamenti a documenti esterni e gli URL in modo che i lettori di copie di questo documento possono accedere a queste informazioni.|  
  


