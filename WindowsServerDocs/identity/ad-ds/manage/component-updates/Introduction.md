---
ms.assetid: 84754c23-f039-4de4-a378-853942e662df
title: Introduzione
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: ab21cca727342f6dc69ceecfb0c8991b30b0f227
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389884"
---
# <a name="introduction"></a>Introduzione

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Gli attacchi contro le infrastrutture di calcolo, sia semplici che complesse, sono esistiti a condizione che i computer dispongano di. Nel corso dell'ultimo decennio, tuttavia, un numero crescente di organizzazioni di qualsiasi dimensione in tutto il mondo ha subito attacchi e danni che hanno modificato in modo significativo il panorama delle minacce. La guerra cibernetica e il crimine informatico sono cresciuti con velocità record. "Hacktivism", in cui gli attacchi sono motivati dalle posizioni degli attivisti, sono stati rivendicati come motivazione di una serie di violazioni destinate a esporre le informazioni segrete delle organizzazioni, per creare Denial of Service o persino per eliminare l'infrastruttura. Gli attacchi contro istituzioni pubbliche e private con l'obiettivo di divulgare la proprietà intellettuale (IP) delle organizzazioni sono diventati onnipresenti.  
  
Nessuna organizzazione con un'infrastruttura IT (Information Technology) è immune agli attacchi, ma se sono implementati criteri, processi e controlli appropriati per proteggere i segmenti chiave dell'infrastruttura di elaborazione di un'organizzazione, l'escalation degli attacchi da la penetrazione per completare la compromissione potrebbe essere evitabile. Poiché il numero e la scala degli attacchi originati dall'esterno di un'organizzazione hanno offuscato le minacce Insider negli ultimi anni, questo documento illustra spesso gli utenti malintenzionati esterni piuttosto che l'uso improprio dell'ambiente da parte di utenti autorizzati. Tuttavia, i principi e le indicazioni fornite in questo documento hanno lo scopo di proteggere l'ambiente da attacchi esterni e da utenti malintenzionati o dannosi.  
  
Le informazioni e le indicazioni fornite in questo documento sono tratte da diverse origini e derivate da procedure progettate per proteggere Active Directory installazioni da compromessi. Sebbene non sia possibile impedire gli attacchi, è possibile ridurre la superficie di attacco Active Directory e implementare i controlli che rendono la directory più difficile per gli utenti malintenzionati. Questo documento presenta i tipi più comuni di vulnerabilità osservate negli ambienti compromessi e le raccomandazioni più comuni apportate ai clienti per migliorare la sicurezza delle installazioni Active Directory.  
  
## <a name="account-and-group-naming-conventions"></a>Convenzioni di denominazione di account e gruppi  
La tabella seguente fornisce una guida alle convenzioni di denominazione usate in questo documento per i gruppi e gli account a cui viene fatto riferimento nel documento. Incluso nella tabella è il percorso di ogni account/gruppo, il nome e il modo in cui viene fatto riferimento a questi account/gruppi in questo documento.  
  


|**Località account/gruppo**|**Nome dell'account/gruppo**|**Come viene fatto riferimento in questo documento**|
| --- | --- | --- |   
|Active Directory-ogni dominio|Administrator|Account amministratore predefinito|  
|Active Directory-ogni dominio|Administrators|Gruppo Administrators predefinito (BA)|  
|Active Directory-ogni dominio|Domain Admins|Gruppo Domain Admins (DA)|  
|Dominio radice della foresta Active Directory|Enterprise Admins|Gruppo Enterprise Admins (EA)|  
|Database di gestione degli account di sicurezza del computer locale (SAM) nei computer che eseguono Windows Server e workstation che non sono controller di dominio|Administrator|Account amministratore locale|  
|Database di gestione degli account di sicurezza del computer locale (SAM) nei computer che eseguono Windows Server e workstation che non sono controller di dominio|Administrators|Gruppo Administrators locale|  
  
## <a name="about-this-document"></a>Informazioni su questo documento  
L'organizzazione Microsoft Information Security and Risk Management (ISRM), che fa parte di Microsoft Information Technology (MSIT), collabora con le business unit interne, i clienti esterni e i peer di settore per raccogliere, diffondere e definire i criteri. procedure e controlli. Queste informazioni possono essere usate da Microsoft e dai clienti per aumentare la sicurezza e ridurre la superficie di attacco delle infrastrutture IT. Le indicazioni fornite in questo documento sono basate su una serie di fonti di informazioni e procedure usate in MSIT e ISRM. Le sezioni seguenti presentano ulteriori informazioni sulle origini di questo documento.  
  
### <a name="microsoft-it-and-isrm"></a>Microsoft IT e ISRM  
In MSIT e ISRM sono state sviluppate diverse procedure e controlli per proteggere le foreste e i domini di Microsoft AD DS. Quando questi controlli sono ampiamente applicabili, sono stati integrati in questo documento. SAFE-T (Solution Accelerator per le tecnologie emergenti) è un team all'interno di ISRM il cui statuto consiste nell'identificare le tecnologie emergenti e definire i requisiti e i controlli di sicurezza per accelerarne l'adozione.  
  
### <a name="active-directory-security-assessments"></a>Valutazioni della sicurezza Active Directory  
All'interno di Microsoft ISRM, il team di valutazione, consulenza e progettazione (ACE) collabora con le business unit interne di Microsoft e con i clienti esterni per valutare la sicurezza delle applicazioni e dell'infrastruttura e fornire indicazioni tattiche e strategiche per aumentare il atteggiamento di sicurezza dell'organizzazione. Un'offerta di servizio ACE è il Active Directory Security Assessment (ADSA), una valutazione olistica dell'ambiente di servizi di dominio Active Directory di un'organizzazione che valuta persone, processi e tecnologie e produce raccomandazioni specifiche per i clienti. I clienti sono forniti con consigli basati sulle caratteristiche, le procedure e l'appetito dei rischi specifici dell'organizzazione. Sono state eseguite ADSAs per le installazioni di Active Directory in Microsoft, oltre a quelle dei clienti. Nel corso del tempo, è stata rilevata una serie di indicazioni applicabili a tutti i clienti di dimensioni e settori diversi.  
  
### <a name="content-origin-and-organization"></a>Origine e organizzazione dei contenuti  
Gran parte del contenuto di questo documento deriva da ADSA e altre valutazioni ACE Team eseguite per i clienti compromessi e i clienti che non hanno riscontrato una compromissione significativa. Anche se non sono stati usati dati dei singoli clienti per creare questo documento, abbiamo raccolto le vulnerabilità sfruttate più di frequente individuate nelle valutazioni e le raccomandazioni apportate ai clienti per migliorare la sicurezza dei servizi di dominio Active Directory. installazioni. Non tutte le vulnerabilità sono applicabili a tutti gli ambienti e non tutte le indicazioni possono essere implementate in ogni organizzazione.  
  
Questo documento è organizzato come segue:  
  
## <a name="executive-summary"></a>Schema riepilogativo  
Il riepilogo generale, che può essere letto come documento autonomo o in combinazione con il documento completo, fornisce un riepilogo di alto livello di questo documento. Inclusi nel Riepilogo di Executive sono i vettori di attacco più comuni osservati, usati per compromettere gli ambienti dei clienti, consigli riepilogativi per la protezione di installazioni Active Directory e obiettivi di base per i clienti che pianificano la distribuzione di nuovi servizi di dominio Active Directory. foreste ora o in futuro.  
  
### <a name="introduction"></a>Introduzione  
Questa è la sezione che si sta leggendo.  
  
### <a name="avenues-to-compromise"></a>Esposizione a possibili violazioni  
In questa sezione vengono fornite informazioni su alcune delle vulnerabilità sfruttate più di frequente, che sono state usate dagli utenti malintenzionati per compromettere le infrastrutture dei clienti. Questa sezione inizia con categorie generali di vulnerabilità e il modo in cui vengono usate per penetrare inizialmente le infrastrutture dei clienti, propagare i compromessi tra sistemi aggiuntivi e infine assegnare i controller di dominio e servizi di dominio Active Directory per ottenere il completamento controllo delle foreste delle organizzazioni.  
  
Questa sezione non fornisce indicazioni dettagliate sull'indirizzamento di ogni tipo di vulnerabilità, in particolare nelle aree in cui le vulnerabilità non vengono usate direttamente come destinazione Active Directory. Tuttavia, per ogni tipo di vulnerabilità sono disponibili collegamenti a informazioni aggiuntive che è possibile usare per sviluppare contromisure e ridurre la superficie di attacco dell'organizzazione.  
  
### <a name="reducing-the-active-directory-attack-surface"></a>Riduzione della superficie di attacco di Active Directory  
In questa sezione vengono fornite informazioni di carattere generale sugli account e i gruppi con privilegi in Active Directory per fornire le informazioni che consentono di chiarire i motivi per i successivi Consigli per la sicurezza e la gestione dei gruppi con privilegi e account. Verranno quindi illustrati gli approcci per ridurre la necessità di usare account con privilegi elevati per l'amministrazione quotidiana, che non richiede il livello di privilegio concesso a gruppi come Enterprise Admins (EA), Domain Admins (DA) e incorporati Gruppi Administrators (BA) in Active Directory. Vengono quindi fornite informazioni aggiuntive per la protezione dei gruppi e degli account con privilegi e per l'implementazione di sistemi e procedure amministrative protette.  
  
Anche se in questa sezione vengono fornite informazioni dettagliate su queste impostazioni di configurazione, sono state incluse anche le appendici per ciascuna raccomandazione che fornisce istruzioni dettagliate per la configurazione che possono essere utilizzate "così come sono" oppure possono essere modificate per il esigenze dell'organizzazione. In questa sezione vengono fornite informazioni per distribuire e gestire in modo sicuro i controller di dominio, che devono essere tra i sistemi più rigorosamente protetti nell'infrastruttura.  
  
### <a name="monitoring-active-directory-for-signs-of-compromise"></a>Monitoraggio dei segnali di compromissione di Active Directory  
In questa sezione vengono fornite informazioni che possono essere utilizzate per identificare gli eventi in Windows, indipendentemente dal fatto che sia stato implementato il monitoraggio degli eventi e delle informazioni di sicurezza affidabili (SIEM) nell'ambiente in uso. sistemi che possono indicare l'attacco di un'organizzazione. Vengono illustrati i criteri di controllo tradizionali e avanzati, inclusa la configurazione efficace delle sottocategorie di controllo nei sistemi operativi Windows 7 e Windows Vista. Questa sezione include elenchi completi di oggetti e sistemi da controllare e un'appendice associata elenca gli eventi per i quali è necessario monitorare se l'obiettivo è quello di rilevare i tentativi di compromissione.  
  
### <a name="planning-for-compromise"></a>Pianificazione del compromesso  
Questa sezione inizia dal "passo indietro" dai dettagli tecnici per concentrarsi sui principi e i processi che possono essere implementati per identificare gli utenti, le applicazioni e i sistemi che sono più importanti non solo per l'infrastruttura IT, ma per l'azienda. Una volta identificate le funzionalità più importanti per la stabilità e le operazioni dell'organizzazione, è possibile concentrarsi sulla separazione e la protezione di queste risorse, sia che si tratti di proprietà intellettuale, persone o sistemi. In alcuni casi, la separazione e la protezione delle risorse possono essere eseguite nell'ambiente di Active Directory Domain Services esistente, mentre in altri casi è consigliabile implementare piccole "celle" separate che consentono di stabilire un limite sicuro intorno alle risorse critiche e di monitorarle asset più rigorosi rispetto ai componenti meno critici. Un concetto denominato "distruzione creativa", che è un meccanismo mediante il quale è possibile eliminare le applicazioni e i sistemi legacy mediante la creazione di nuove soluzioni, e la sezione termina con consigli che consentono di mantenere un ambiente più sicuro combinando informazioni aziendali e IT per costruire un'immagine dettagliata del normale stato operativo. Conoscendo le condizioni normali per un'organizzazione, le anomalie che possono indicare attacchi e compromessi possono essere identificate più facilmente.  
  
### <a name="summary-of-best-practice-recommendations"></a>Riepilogo delle procedure consigliate  
In questa sezione viene fornita una tabella in cui vengono riepilogate le indicazioni fornite in questo documento e vengono ordinate in base alla priorità relativa, oltre a fornire collegamenti alla posizione in cui è possibile trovare ulteriori informazioni su ogni raccomandazione nel documento e nelle relative appendici.  
  
### <a name="appendices"></a>Appendici  
In questo documento sono incluse le appendici per ampliare le informazioni contenute nel corpo del documento. La tabella seguente include l'elenco delle appendici e una breve descrizione di ciascuna di esse.  
  
 
|**Appendice**|**Descrizione**|
| --- | --- | 
|[Appendice B: Account e gruppi con privilegi in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)|Fornisce informazioni di base che consentono di identificare gli utenti e i gruppi che devono essere concentrati sulla sicurezza, in quanto possono essere utilizzati da utenti malintenzionati per compromettere e persino eliminare l'installazione Active Directory.|  
|[Appendice C: Account e gruppi protetti in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)|Contiene informazioni sui gruppi protetti in Active Directory. Contiene inoltre informazioni per la personalizzazione limitata (rimozione) di gruppi considerati gruppi protetti e che sono interessati da AdminSDHolder e SDProp.|  
|[Appendice D: Protezione degli account Administrator predefiniti in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)|Contiene linee guida che consentono di proteggere l'account amministratore in ogni dominio della foresta.|  
|[Appendice E: Protezione dei gruppi Enterprise Admins in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)|Contiene linee guida che consentono di proteggere il gruppo Enterprise Admins nella foresta.|  
|[Appendice F: Protezione dei gruppi Domain Admins in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)|Contiene linee guida che consentono di proteggere il gruppo Domain Admins in ogni dominio della foresta.|  
|[Appendice G: Protezione dei gruppi Administrators in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)|Contiene linee guida che consentono di proteggere il gruppo Administrators predefinito in ogni dominio della foresta.|  
|[Appendice H: Protezione degli account e dei gruppi Administrator locali](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)|Contiene linee guida che consentono di proteggere gli account amministratore locale e i gruppi Administrators in server e workstation aggiunti a un dominio.|  
|[Appendice I: Creazione di account di gestione per gli account e i gruppi protetti in Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)|Fornisce informazioni per creare account con privilegi limitati e che possono essere controllati in modo rigoroso, ma possono essere utilizzati per popolare i gruppi con privilegi in Active Directory quando è richiesta l'elevazione temporanea.|  
|[Appendice L: Eventi da monitorare](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)|Elenca gli eventi per i quali è necessario monitorare nell'ambiente in uso.|  
|[Appendice M: Collegamenti a documenti e letture consigliate](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)|Contiene un elenco di letture consigliate. Contiene anche un elenco di collegamenti a documenti esterni e i relativi URL, in modo che i lettori delle copie disco rigido di questo documento possano accedere a tali informazioni.|  
  


