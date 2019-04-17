---
ms.assetid: 864ad4bc-8428-4a8b-8671-cb93b68b0c03
title: Ridurre la superficie di attacco di Active Directory
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b2de254076b10a1a75d658f006c2245d523de6b7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="reducing-the-active-directory-attack-surface"></a>Ridurre la superficie di attacco di Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questa sezione vengono illustrati i controlli tecnici per implementare per ridurre la superficie di attacco di installazione di Active Directory. La sezione contiene le informazioni seguenti:  
  
-   [Implementazione dei modelli amministrativi con privilegi minimi](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md) è incentrato sull'identificazione il rischio che presenta l'uso di account con privilegi elevati per l'amministrazione quotidiana, oltre a fornire consigli per implementare per ridurre il rischio che presenti account con privilegi.  
  
-   [Implementazione host amministrativi protetti](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md) descrive principi per la distribuzione di sistemi amministrativi dedicati e sicura, oltre ad alcuni esempi si avvicina a una distribuzione di host amministrativi sicuro.  
  
-   [Protezione dei controller di dominio dagli attacchi](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md) illustra i criteri e impostazioni che, nonostante sia simile alle raccomandazioni per l'implementazione di host amministrativi sicuri, contengono alcuni consigli specifici del controller di dominio per garantire che i controller di dominio e i sistemi utilizzati per la loro gestione siano ben protetti.  
  
## <a name="privileged-accounts-and-groups-in-active-directory"></a>Account con privilegi e gruppi in Active Directory  
In questa sezione vengono fornite informazioni sugli account con privilegi e gruppi in Active Directory devono spiegare le diversità; e le differenze tra gli account con privilegi e gruppi in Active Directory. In base a queste differenze, se si implementano i consigli forniti in [implementazione di modelli amministrativi con privilegi minimi](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md) testuale o scegliere di personalizzazione per l'organizzazione, gli strumenti necessari per ogni gruppo di protezione e account in modo appropriato.  
  
### <a name="built-in-privileged-accounts-and-groups"></a>Account e gruppi con privilegi predefiniti  
Active Directory facilita la delega dell'amministrazione e l'assegnazione di diritti e autorizzazioni supporta il principio dei privilegi minimi. "Normali" utenti che dispongono di account in un dominio sono, per impostazione predefinita, in grado di leggere molte delle quali è archiviato nella directory, ma sono in grado di modificare solo un set molto limitato di dati nella directory. Gli utenti che richiedono privilegi aggiuntivi possono essere concesse appartenenza a diversi gruppi "con privilegi" che vengono creati nella directory in modo che possono eseguire attività specifiche correlate al loro ruoli, ma non possono eseguire attività che non sono rilevanti per le proprie attività. Le organizzazioni possono inoltre creare gruppi che sono su misura per i ruoli professionali specifici e vengono concessi diritti granulari e le autorizzazioni che consentono il personale IT per eseguire le funzioni amministrative quotidiane senza concedere diritti e autorizzazioni che superano cosa è necessario per queste funzioni.  
  
All'interno di Active Directory, i tre gruppi predefiniti sono i gruppi con privilegi di livello più alti nella directory: Administrators, Enterprise Admins e Domain Admins. La configurazione predefinita e funzionalità di ognuno di questi gruppi sono descritti nelle sezioni seguenti:  
  
#### <a name="highest-privilege-groups-in-active-directory"></a>Gruppi con privilegi più elevati in Active Directory  
  
##### <a name="enterprise-admins"></a>Enterprise Admins  
Enterprise Admins (EA) è un gruppo che esiste solo nel dominio radice della foresta, e per impostazione predefinita, è un membro del gruppo Administrators in tutti i domini nella foresta. L'account Administrator predefinito nel dominio radice della foresta è l'unico membro predefinito del gruppo EA. EAs vengono concessi diritti e autorizzazioni che consentono di implementare le modifiche a livello di foresta (vale a dire le modifiche che influiscono su tutti i domini nella foresta), ad esempio aggiungendo o rimuovendo domini, stabilire i trust tra foreste o la generazione di livelli di funzionalità della foresta. In un modello di delega progettata e implementata correttamente, l'appartenenza al gruppo EA è necessario solo quando prima di tutto la costruzione della foresta o quando si apportano modifiche a livello di foresta, ad esempio stabilire un trust tra foreste in uscita. La maggior parte dei diritti e autorizzazioni concesse al gruppo EA possono essere delegata a gruppi e gli utenti con privilegi inferiori.  
  
##### <a name="domain-admins"></a>Domain Admins  

Ogni dominio in una foresta dispone del proprio gruppo Domain Admins (DA), che è un membro del gruppo Administrators del dominio e un membro del gruppo Administrators locale in ogni computer appartenente al dominio. L'unico membro predefinito del gruppo DA un dominio è l'account Administrator predefinito per tale dominio. DAs sono "potentissimi" all'interno dei domini, mentre EAs hanno privilegi a livello di foresta. In un modello di delega progettata e implementata correttamente, l'appartenenza al gruppo Domain Admins deve solo in scenari "break glass" (ad esempio situazioni in cui è necessario un account con livelli di privilegio elevati su ogni computer nel dominio). Sebbene nativi meccanismi di delega di Active Directory consentono la delega nella misura in cui è possibile utilizzare gli account DA solo in scenari di emergenza, la creazione di un modello di delega efficace può richiedere molto tempo e molte organizzazioni di sfruttare gli strumenti di terze parti per accelerare il processo.  
  
##### <a name="administrators"></a>Amministratori  
Il terzo gruppo è incorporato gruppo locale di dominio Administrators (BA) in cui sono annidati DAs ed EAs. Questo gruppo viene concessa molte diretta diritti e autorizzazioni nella directory e controller di dominio. Tuttavia, il gruppo Administrators per un dominio non ha privilegi sui server membri o workstation. È tramite l'appartenenza al gruppo Administrators locale dei computer che vengono concessi privilegi locali.  
  
> [!NOTE]  
> Anche se queste sono le configurazioni predefinite di questi gruppi con privilegi, un membro di uno dei tre gruppi consente di modificare la directory per ottenere l'appartenenza a uno qualsiasi di altri gruppi. In alcuni casi, è molto semplice per ottenere l'appartenenza ai gruppi, mentre in altri è più difficile, ma dal punto di vista dei privilegi potenziali, tutti e tre i gruppi devono essere considerati equivalenti in modo efficace.  
  
##### <a name="schema-admins"></a>Schema Admins  

Un quarto con privilegi, gruppo Schema Admins (SA), esiste solo nel dominio radice della foresta e ha solo del tale dominio account Administrator predefinito come un membro predefinito, simile al gruppo Enterprise Admins. Al gruppo Schema Admins deve essere compilato solo temporaneamente e in alcuni casi (quando è necessario modificare di schema di Active Directory).  
  
Anche se il gruppo di associazione di sicurezza è l'unico gruppo che è possibile modificare lo schema di Active Directory (valore valido., strutture di dati, ad esempio gli oggetti e attributi sottostanti della directory), l'ambito di diritti e autorizzazioni del gruppo amministratore di sistema è più limitato rispetto ai gruppi descritti in precedenza. È anche comune per trovare che le organizzazioni hanno sviluppato procedure appropriate per la gestione di appartenenza del gruppo di associazione di sicurezza perché l'appartenenza al gruppo in genere è raramente necessaria e solo per brevi periodi di tempo. Ciò è tecnicamente possibile dei EA, DA e BA gruppi in Active Directory, ma è molto meno comune per trovare che le organizzazioni hanno implementato consigliate simili per questi gruppi per quanto riguarda il gruppo di associazione di sicurezza.  
  
#### <a name="protected-accounts-and-groups-in-active-directory"></a>Gli account protetti e gruppi in Active Directory  
All'interno di Active Directory, un set predefinito di account con privilegi e gruppi denominati "protetto" account e gruppi sono protetti in modo diverso rispetto ad altri oggetti nella directory. Qualsiasi account che dispone di appartenenza diretta o transitivo in qualsiasi gruppo protetto (indipendentemente dal fatto se l'appartenenza è derivata da gruppi di protezione o di distribuzione) eredita per la sicurezza con restrizioni.  

  
Ad esempio, se un utente è membro di un gruppo di distribuzione che, a sua volta, un membro di un gruppo in Active Directory, che l'oggetto utente protetto viene contrassegnato come account protetti. Quando un account è contrassegnato come un account protetto, il valore dell'attributo adminCount dell'oggetto è impostato su 1.  
  
> [!NOTE]
> Anche se transitivo appartenenza a un gruppo protetto include annidata distribuzione e gruppi di sicurezza annidato, gli account che sono membri di gruppi di distribuzione nidificati non riceverà SID del gruppo protetti nei token di accesso. Tuttavia, i gruppi di distribuzione possono essere convertiti in gruppi di sicurezza in Active Directory, motivo per cui sono inclusi i gruppi di distribuzione nell'enumerazione membro gruppo protetto. Un gruppo di distribuzione annidati protetto deve essere convertito mai in un gruppo di sicurezza, gli account che sono membri del gruppo di distribuzione precedente successivamente verranno visualizzato l'elemento padre protetto del SID di gruppo nei token di accesso al successivo accesso.  
  
La tabella seguente elenca gli account predefiniti protetti e i gruppi in Active Directory da una versione e del service pack del sistema operativo.  
  
**Gli account protetti e gruppi in Active Directory dal sistema operativo e versione del Service Pack (SP) predefiniti**  
  
|||||  
|-|-|-|-|  
|**Windows 2000 < SP4**|**Windows 2000 SP4, Windows Server 2003**|**Windows Server 2003 SP1 +**|**Windows Server 2008, Windows Server 2012**|  
|Amministratori|Account Operators|Account Operators|Account Operators|  
||Amministratore|Amministratore|Amministratore|  
||Amministratori|Amministratori|Amministratori|  
|Domain Admins|Gruppo Backup Operators|Gruppo Backup Operators|Gruppo Backup Operators|  
||Cert Publishers|||  
||Domain Admins|Domain Admins|Domain Admins|  
|Enterprise Admins|Controller di dominio|Controller di dominio|Controller di dominio|  
||Enterprise Admins|Enterprise Admins|Enterprise Admins|  
||Krbtgt|Krbtgt|Krbtgt|  
||Operatori di stampa|Operatori di stampa|Operatori di stampa|  
||||Controller di dominio di sola lettura|  
||Servizio di replica|Servizio di replica|Servizio di replica|  
|Schema Admins|Schema Admins||Schema Admins|  
  
##### <a name="adminsdholder-and-sdprop"></a>AdminSDHolder e SDProp  
Nel contenitore di sistema di ogni dominio di Active Directory, viene creato automaticamente un oggetto denominato AdminSDHolder. Lo scopo dell'oggetto AdminSDHolder è garantire che le autorizzazioni per gli account protetti e gruppi vengono applicate in modo coerente, indipendentemente dal fatto in cui si trovano gli account e gruppi protetti nel dominio.  

Ogni 60 minuti (per impostazione predefinita), un processo noto come gestore dei descrittori di sicurezza (SDProp) viene eseguito sul controller di dominio che detiene il ruolo di emulatore PDC del dominio. SDProp confronta le autorizzazioni per l'oggetto AdminSDHolder del dominio con le autorizzazioni per gli account protetti e i gruppi nel dominio. Se le autorizzazioni per tutti i gruppi e gli account protetti le autorizzazioni per l'oggetto AdminSDHolder non corrispondono, vengono reimpostate le autorizzazioni per gli account protetti e gruppi corrispondenti a quelle dell'oggetto AdminSDHolder del dominio.  
  
Ereditarietà delle autorizzazioni è disabilitato su protetti gruppi e account, il che significa che anche se gli account o i gruppi vengono spostati in diverse posizioni nella directory, non ereditano le autorizzazioni dai relativi oggetti padre nuovo. Ereditarietà è disabilitato anche per l'oggetto AdminSDHolder in modo che le modifiche alle autorizzazioni per gli oggetti padre non modificare le autorizzazioni di AdminSDHolder.  
  
> [!NOTE]
> Quando un account viene rimosso da un gruppo protetto, non è più considerato un account protetto, ma il relativo rimane attributo adminCount impostati su 1 se non viene modificato manualmente. Il risultato di questa configurazione è che gli ACL dell'oggetto non vengano più aggiornati da SDProp, ma l'oggetto ancora non eredita le autorizzazioni dal relativo oggetto padre. Di conseguenza, l'oggetto può risiedere in un'unità organizzativa (OU) in cui sono state delegate le autorizzazioni, ma l'oggetto protetto in precedenza non eredita le autorizzazioni delegate. Uno script per individuare e ripristinare gli oggetti protetti in precedenza nel dominio sono reperibili nel [articolo di supporto Microsoft 817433](https://support.microsoft.com/?id=817433).  
  
###### <a name="adminsdholder-ownership"></a>Proprietà AdminSDHolder  
La maggior parte degli oggetti in Active Directory appartengono al gruppo del dominio BA. Tuttavia, l'oggetto AdminSDHolder, per impostazione predefinita, appartiene al gruppo del dominio DA. (Questo è un caso in cui DAs non derivano propri diritti e autorizzazioni tramite l'appartenenza al gruppo Administrators per il dominio).  
  
Nelle versioni di Windows precedenti a Windows Server 2008, i proprietari di un oggetto possono modificare le autorizzazioni dell'oggetto, tra cui la concessione se stessi autorizzazioni che originariamente non dispone. Di conseguenza, le autorizzazioni predefinite per l'oggetto AdminSDHolder del dominio impediscono agli utenti che sono membri dei gruppi di base o EA di modificare le autorizzazioni per l'oggetto AdminSDHolder del dominio. Tuttavia, i membri del gruppo Administrators per il dominio possono assumere la proprietà dell'oggetto e concedere a se stessi autorizzazioni aggiuntive, il che significa che questa protezione è rudimentale e protegge solo l'oggetto da modifiche accidentali dagli utenti che non sono membri del gruppo DA nel dominio. Inoltre, il BA ed EA (se applicabile) gruppi dispongono dell'autorizzazione per modificare gli attributi dell'oggetto AdminSDHolder nel dominio locale (dominio radice per EA).  
  
> [!NOTE]  
> Un attributo dell'oggetto AdminSDHolder, dSHeuristics, consente la personalizzazione limitata (rimozione) dei gruppi sono considerate gruppi protetti e vengono influenzati da AdminSDHolder e SDProp. Questa personalizzazione necessario valutare con attenzione se viene implementato, anche se esistono valide circostanze in cui la modifica di dSHeuristics su AdminSDHolder è utile. Negli articoli di supporto Microsoft sono disponibili ulteriori informazioni sulla modifica dell'attributo dSHeuristics in un oggetto AdminSDHolder [817433](https://support.microsoft.com/?id=817433) e [973840](https://support.microsoft.com/kb/973840)e in [gli account protetti appendice c: e gruppi in Active Directory](Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
Anche se qui sono descritti i gruppi con più privilegi in Active Directory, esistono una serie di altri gruppi che sono stati concessi privilegi elevati i livelli di privilegi. Per ulteriori informazioni su tutti i gruppi predefiniti in Active Directory e i diritti utente assegnati a ogni e predefinito, vedere [appendice b: account con privilegi e gruppi in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md).  
  


