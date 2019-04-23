---
ms.assetid: 84754c23-f039-4de4-a378-853942e662df
title: Introduzione
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 8e2717af6183944b26a71e55b36f31cef51cf2e7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831872"
---
# <a name="introduction"></a>Introduzione

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Gli attacchi contro computing infrastrutture, siano esse semplici o complesse, che fosse presente purché dispongano di computer. Nel corso dell'ultimo decennio, tuttavia, un numero crescente di organizzazioni di qualsiasi dimensione in tutto il mondo ha subito attacchi e danni che hanno modificato in modo significativo il panorama delle minacce. La guerra cibernetica e il crimine informatico sono cresciuti con velocità record. "Hacktivism", in cui gli attacchi motivati da posizioni di attivismo sociale, è stato indicato come la motivazione per svariate violazioni destinate a esporre le informazioni segrete delle organizzazioni, per creare rifiuti del servizio, o addirittura distruggere l'infrastruttura. Gli attacchi contro istituzioni pubbliche e private con l'obiettivo di frequentissimi della proprietà intellettuale (IP) dell'azienda sono diventati molto diffusa.  
  
Nessuna organizzazione con un'infrastruttura degli information technology (IT) è immune agli attacchi, ma se i controlli, processi e i criteri appropriati vengono implementati per proteggere i segmenti essenziali dell'infrastruttura informatica dell'organizzazione, l'escalation degli attacchi da potrebbero che si riesca penetrazione completare compromissione. Poiché il numero e la scala di attacchi provenienti dall'esterno dell'organizzazione ha tempo eclissato minacce interne negli ultimi anni, in questo documento viene spesso agli aggressori esterni piuttosto che un uso improprio dell'ambiente per gli utenti autorizzati. Ciononostante, i principi e le indicazioni fornite in questo documento sono progettate per proteggere l'ambiente da attacchi esterni e interni fuorviati o malintenzionati.  
  
Le informazioni e le indicazioni fornite in questo documento vengono disegnate da un numero di origini e procedure specifiche per proteggere le installazioni di Active Directory dalle compromissioni. Anche se non è possibile impedire gli attacchi, è possibile ridurre la superficie di attacco di Active Directory e implementare i controlli che rendono compromissione della directory molto più difficile per gli utenti malintenzionati. Questo documento presenta i tipi più comuni di vulnerabilità che abbiamo osservato in ambienti di compromessi e i consigli più comuni che sono stati apportati ai clienti per migliorare la sicurezza delle installazioni Active Directory.  
  
## <a name="account-and-group-naming-conventions"></a>Convenzioni di account e la denominazione del gruppo  
Nella tabella seguente fornisce una Guida per le convenzioni di denominazione usata in questo documento per i gruppi e account di cui viene fatto riferimento in tutto il documento. Incluso nella tabella è la posizione di ogni gruppo/account, il nome e come questi account o i gruppi vengono citati nel presente documento.  
  


|**Posizione/gruppo di account**|**Nome dell'Account o gruppo**|**Modo in cui viene fatto riferimento in questo documento**|
| --- | --- | --- |   
|Active Directory - ogni dominio|Administrator|Account predefinito Administrator|  
|Active Directory - ogni dominio|Administrators|Gruppo incorporato Administrators (BA)|  
|Active Directory - ogni dominio|Domain Admins|Gruppo Domain Admins (DA)|  
|Active Directory - dominio radice della foresta|Enterprise Admins|Gruppo Enterprise Admins (EA)|  
|Database di gestione degli account di sicurezza di computer locale nei computer che eseguono Windows Server e workstation che non sono controller di dominio|Administrator|Account amministratore locale|  
|Database di gestione degli account di sicurezza di computer locale nei computer che eseguono Windows Server e workstation che non sono controller di dominio|Administrators|Gruppo Administrators locale|  
  
## <a name="about-this-document"></a>Su questo documento  
L'organizzazione Microsoft Information Security e gestione dei rischi (ISRM), che fa parte di Microsoft informazioni tecnologia (MSIT), funziona con le unità aziendali interne, i clienti esterni e peer del settore per raccogliere, diffondere e definire i criteri, procedure consigliate e i controlli. Queste informazioni è utilizzabile da Microsoft e i clienti per aumentare la sicurezza e ridurre la superficie di attacco delle infrastrutture IT. Le indicazioni fornite in questo documento si basano su un numero di fonti di informazioni e procedure usate all'interno di MSIT e ISRM. Le sezioni seguenti presentano ulteriori informazioni sulle origini di questo documento.  
  
### <a name="microsoft-it-and-isrm"></a>Microsoft IT e ISRM  
Una serie di procedure consigliate e i controlli è stata sviluppata all'interno di MSIT e ISRM per proteggere le foreste di Microsoft AD DS e domini. In cui questi controlli sono ampiamente applicabili, sono state integrate in questo documento. SAFE-T (acceleratori di soluzioni per le tecnologie emergenti) è un team all'interno di ISRM cui charter consiste nell'identificare le tecnologie emergenti e per definire i requisiti di sicurezza e controlli per accelerare l'adozione.  
  
### <a name="active-directory-security-assessments"></a>Valutazioni della sicurezza di Active Directory  
All'interno di Microsoft ISRM, la valutazione, consulenza e Team Engineering (ACE) funziona con interne Microsoft business unit e ai clienti esterni per valutare la sicurezza dell'applicazione e l'infrastruttura e fornire indicazioni tattiche e strategiche per aumentare la comportamento di sicurezza dell'organizzazione. Offerta di servizio una voce ACE è l'Active Directory Security Assessment (ADSA), ovvero una valutazione completa dell'ambiente di dominio Active Directory dell'organizzazione che consente di valutare le persone, processi e tecnologie e genera le indicazioni specifiche del cliente. I clienti ricevono suggerimenti che si basano sulle caratteristiche uniche dell'organizzazione, procedure consigliate e al desiderio di rischio. ADSAs sono stati eseguiti per le installazioni di Active Directory in Microsoft oltre a quelli dei clienti. Nel corso del tempo, un numero di raccomandazioni è stato trovato sia applicabile tra i clienti di vari settori e dimensioni.  
  
### <a name="content-origin-and-organization"></a>Origine e organizzazione dei contenuti  
Gran parte del contenuto di questo documento è derivata dal ADSA e valutazioni altri Team ACE eseguite per i clienti compromessi e i clienti che non si siano verificati compromissione significativo. Anche se non sono stati usati dati di singoli clienti per creare questo documento, abbiamo raccolto le vulnerabilità sfruttate più comunemente sono state identificate le valutazioni e le raccomandazioni sono stati apportati ai clienti per migliorare la sicurezza dei loro Active Directory Domain Services installazioni. Non tutte le vulnerabilità sono applicabili a tutti gli ambienti e non tutte le indicazioni possono essere implementate in ogni organizzazione.  
  
Questo documento è organizzato come indicato di seguito:  
  
## <a name="executive-summary"></a>Schema riepilogativo  
L'Executive riepilogo, che può essere letto come documento autonomo o in combinazione con il documento completo, fornisce un riepilogo dettagliato di questo documento. I vettori di attacco più comuni che è stato osservato usato per compromettere gli ambienti dei clienti, riepilogo di sicurezza consigliati per le installazioni di Active Directory e gli obiettivi di base per i clienti che intendono distribuire di nuovo Active Directory Domain Services sono incluse nel riepilogo esecutivo insiemi di strutture subito o in futuro.  
  
### <a name="introduction"></a>Introduzione  
Questa è la sezione che si esegue la lettura a questo punto.  
  
### <a name="avenues-to-compromise"></a>Esposizione a possibili violazioni  
Questa sezione vengono fornite informazioni su alcune delle domande più comunemente sfruttato le vulnerabilità sono state trovate per essere usato dagli utenti malintenzionati per compromettere le infrastrutture dei clienti. In questa sezione inizia con categorie generali di vulnerabilità e come essi vengono usate per inizialmente penetrare infrastrutture dei clienti, propagare i compromessi tra altri sistemi e infine destinata AD DS e controller di dominio per ottenere completo controllo delle foreste di organizzazioni.  
  
In questa sezione non fornisce indicazioni dettagliate sulla risoluzione di ogni tipo di vulnerabilità, in particolare nelle aree in cui le vulnerabilità non vengono utilizzate per Active Directory hanno come destinazione diretta. Per ogni tipo di vulnerabilità, tuttavia, Microsoft fornisce collegamenti a informazioni aggiuntive che è possibile usare per sviluppare contromisure e ridurre la superficie di attacco della propria organizzazione.  
  
### <a name="reducing-the-active-directory-attack-surface"></a>Riduzione della superficie di attacco di Active Directory  
In questa sezione inizia fornendo informazioni complementari sugli account con privilegi e gruppi in Active Directory per fornire le informazioni che consente di chiarire i motivi per le successive indicazioni per la sicurezza e gestione di gruppi con privilegi e account. Saranno quindi illustrati gli approcci in modo da ridurre la necessità di usare account con privilegi elevati per l'amministrazione quotidiana, che non richiede il livello di privilegio concesso ai gruppi, ad esempio le Enterprise Admins (EA), Domain Admins (DA) e incorporati Gruppi Administrators (BA) in Active Directory. Successivamente, vengono fornite indicazioni per proteggere gli account e gruppi con privilegi e per l'implementazione di sistemi e prassi amministrative sicure.  
  
Anche se in questa sezione fornisce informazioni dettagliate su queste impostazioni di configurazione, è stato inoltre incluso appendici per ogni raccomandazione che forniscono istruzioni dettagliate per la configurazione che possono essere utilizzato "così com'è" o possono essere modificate per il esigenze dell'organizzazione. In questa sezione viene completata, fornendo le informazioni per distribuire e gestire i controller di dominio, che devono essere tra i sistemi protetti più rigorosamente nell'infrastruttura in modo sicuro.  
  
### <a name="monitoring-active-directory-for-signs-of-compromise"></a>Monitoraggio dei segnali di compromissione di Active Directory  
Se si è implementato una protezione robusta informazioni ed eventi (SIEM) di monitoraggio nell'ambiente in uso o si usano altri meccanismi per monitorare la sicurezza dell'infrastruttura, questa sezione vengono fornite informazioni che possono essere utilizzate per identificare gli eventi in Windows sistemi che possono indicare che un'organizzazione viene attaccata. Vengono illustrati i criteri di controllo tradizionale e avanzate, inclusa la configurazione effettiva di sottocategorie di controllo nei sistemi operativi Windows 7 e Windows Vista. In questa sezione include un elenco completo degli oggetti e sistemi di controllo e un'appendice associata sono elencati gli eventi per il quale è necessario monitorare se l'obiettivo consiste nel rilevare tentativi di compromissione.  
  
### <a name="planning-for-compromise"></a>Pianificazione del compromesso  
In questa sezione inizia con "l'esecuzione di istruzioni back" dai dettagli tecnici di concentrarsi sui principi e i processi che possono essere implementati per identificare gli utenti, applicazioni e sistemi che sono più importanti non solo per l'infrastruttura IT, ma all'azienda. Dopo l'identificazione degli elementi più importanti per la stabilità e operazioni dell'organizzazione, è possibile concentrarsi sulla adeguatamente e la protezione di questi asset, se sono di proprietà intellettuale, persone o sistemi. In alcuni casi, adeguatamente e protezione di asset possono essere eseguiti nell'ambiente Active Directory Domain Services esistente, mentre in altri casi, è consigliabile implementare piccole, separata "cells" che consentono di stabilire un limite sicuro attorno a asset critici e monitorarle gli asset in modo più rigoroso di componenti meno critici. Viene illustrato un concetto detto "creativa distruzione", ovvero un meccanismo mediante il quale i sistemi e applicazioni legacy possono essere eliminati tramite la creazione di nuove soluzioni e la sezione termina con consigli che possono aiutare a gestire un ambiente più protetto da combinazione di business e le informazioni di IT per costruire una panoramica dettagliata di ciò che è un normale stato operativo. Sapendo che cos'è normale per un'organizzazione, le anomalie che potrebbero indicare compromissione e attacchi più facilmente identificabili.  
  
### <a name="summary-of-best-practice-recommendations"></a>Riepilogo delle indicazioni  
In questa sezione viene fornita una tabella che riepiloga i consigli forniti in questo documento e li ordina in ordine di priorità relativa, oltre a fornire collegamenti a cui altre informazioni su ogni raccomandazione sono reperibile nel documento e relativo appendici.  
  
### <a name="appendices"></a>Appendici  
Appendici sono inclusi in questo documento per incrementare le informazioni contenute nel corpo del documento. L'elenco delle appendici e una breve descrizione della ognuno è incluso nella tabella seguente.  
  
 
|**Appendice**|**Descrizione**|
| --- | --- | 
|[Appendice b: Gli account con privilegi e gruppi in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)|Vengono fornite informazioni che aiuta a identificare gli utenti e gruppi, che è consigliabile concentrarsi sulla protezione perché essi possono essere sfruttate da utenti malintenzionati di compromettere e anche eliminare definitivamente l'installazione di Active Directory.|  
|[Appendice C: Gli account protetti e gruppi in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)|Contiene informazioni sui gruppi protetti in Active Directory. Include anche informazioni per la personalizzazione limitata (rimozione) di gruppi che sono considerati gruppi protetti e sono interessati da AdminSDHolder e SDProp.|  
|[Appendice d: Protezione predefiniti Administrator account in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)|Contiene le linee guida per contribuire a proteggere l'account di amministratore in ogni dominio nella foresta.|  
|[Appendice e: Protezione dei gruppi Enterprise Admins in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)|Contiene le linee guida per contribuire a proteggere il gruppo Enterprise Admins nella foresta.|  
|[Appendice f: Protezione Domain Admins gruppi in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)|Contiene le linee guida per contribuire a proteggere il gruppo Domain Admins in ogni dominio nella foresta.|  
|[Appendice g: Protezione dei gruppi Administrators in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)|Contiene le linee guida per contribuire a proteggere il gruppo Administrators predefinito in ogni dominio nella foresta.|  
|[Appendice h: Protezione dell'account di amministratore locale e i gruppi](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)|Contiene le linee guida per account di amministratore locale sicuro e gruppi di amministratori nei server aggiunti al dominio e workstation.|  
|[Appendice i: Creazione di gestione di account per gli account protetti e gruppi in Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)|Vengono fornite informazioni per creare gli account che dispongono di privilegi limitati e possono essere rigorosamente controllati, ma possono essere usati per popolare i gruppi con privilegi in Active Directory quando è richiesta l'elevazione temporanea.|  
|[Appendice l: Eventi da monitorare](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)|Elenca gli eventi per il quale è necessario monitorare nell'ambiente in uso.|  
|[Appendice m: Collegamenti a documenti e letture consigliate](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)|Contiene un elenco di letture consigliate. Contiene inoltre un elenco di collegamenti a documenti esterni e i relativi URL in modo che i lettori di copie cartacee di questo documento possono accedere a queste informazioni.|  
  


