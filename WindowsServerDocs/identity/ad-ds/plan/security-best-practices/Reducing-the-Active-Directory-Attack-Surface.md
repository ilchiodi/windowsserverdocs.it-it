---
ms.assetid: 864ad4bc-8428-4a8b-8671-cb93b68b0c03
title: Riduzione della superficie di attacco di Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d692641d316b5fe7206cc3f413bdcfc9b74675b9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874152"
---
# <a name="reducing-the-active-directory-attack-surface"></a>Riduzione della superficie di attacco di Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questa sezione è incentrata sui controlli tecnici per implementare in modo da ridurre la superficie di attacco dell'installazione di Active Directory. La sezione contiene le informazioni seguenti:  
  
-   [Implementazione di modelli amministrativi con privilegi minimi](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md) degli account è incentrata sulla che identifica il rischio che l'uso di privilegi elevati per attività di ordinaria amministrazione presenta, oltre a fornire consigli per implementare in modo da ridurre il rischio tale privilegi account presente.  
  
-   [Implementazione host amministrativi protetti](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md) descrive i principi per la distribuzione dei sistemi amministrativi dedicati, protetti, oltre all'esempio di alcuni approcci a una distribuzione di host amministrativi protetto.  
  
-   [Protezione dei controller di dominio contro attacco](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md) illustra i criteri e impostazioni che, pur essendo simili ai consigli per l'implementazione di host amministrativi protetti, contengono alcuni consigli specifici dei controller di dominio per consentire Assicurarsi che i controller di dominio e i sistemi usati per gestirli siano ben protetti.  
  
## <a name="privileged-accounts-and-groups-in-active-directory"></a>Gli account con privilegi e gruppi in Active Directory  
In questa sezione vengono fornite informazioni sugli account con privilegi e gruppi in Active Directory può vengono illustrate le similitudini e differenze tra gli account con privilegi e gruppi in Active Directory. Conoscendo queste distinzioni, se si implementano le indicazioni contenute nella [implementazione di modelli amministrativi con privilegi minimi](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md) verbatim o scegliere di personalizzare per l'organizzazione, è necessario gli strumenti necessari per proteggere ogni gruppo e account in modo appropriato.  
  
### <a name="built-in-privileged-accounts-and-groups"></a>Account e gruppi con privilegi predefiniti  
Active Directory semplifica la delega dell'amministrazione e supporta il principio del privilegio minimo nell'assegnazione di diritti e autorizzazioni. "Normali" agli utenti che dispongono di account in un dominio sono, per impostazione predefinita, in grado di leggere la maggior parte di ciò che viene archiviato nella directory, ma sono in grado di modificare solo un set molto limitato di dati nella directory. Gli utenti che richiedono privilegi aggiuntivi possono essere concesse appartenenza a diversi gruppi "con privilegi" che vengono compilati nella directory in modo che si possono eseguire attività specifiche correlate ai ruoli, ma non possono eseguire attività che non sono rilevanti per le loro funzioni. Le organizzazioni possono anche creare i gruppi che sono specifiche a specifici ruoli professionali e vengono concessi i diritti granulari e le autorizzazioni che consentono al personale IT eseguire funzioni amministrative quotidiane senza concedere diritti e autorizzazioni superiori a cosa è obbligatorio per le funzioni.  
  
All'interno di Active Directory, tre gruppi predefiniti sono i gruppi con privilegi più elevati nella directory: Enterprise Admins, Domain Admins e amministratori. La configurazione predefinita e le funzionalità della ognuno di questi gruppi sono descritti nelle sezioni seguenti:  
  
#### <a name="highest-privilege-groups-in-active-directory"></a>Gruppi con privilegi più elevati in Active Directory  
  
##### <a name="enterprise-admins"></a>Enterprise Admins  
Enterprise Admins (EA) è un gruppo che esiste solo nel dominio radice della foresta e per impostazione predefinita, è un membro del gruppo Administrators in tutti i domini nella foresta. L'account Administrator predefinito nel dominio radice della foresta è l'unico membro predefinito del gruppo EA. EAs vengono concessi i diritti e autorizzazioni che consentono di implementare le modifiche a livello di foresta (vale a dire, le modifiche che influiscono su tutti i domini nella foresta), ad esempio aggiungendo o rimuovendo domini, stabilire relazioni di trust o aumento dei livelli funzionali di foresta. In un modello di delega correttamente progettato e implementato, l'appartenenza al contratto Enterprise è richiesto solo durante la costruzione innanzitutto la foresta o quando si apportano determinate modifiche a livello di foresta, ad esempio stabilendo un trust tra foreste in uscita. La maggior parte dei diritti e autorizzazioni concesse al gruppo EA può essere delegata a gruppi e utenti con privilegi minori.  
  
##### <a name="domain-admins"></a>Domain Admins  

Ogni dominio in una foresta ha un proprio gruppo Domain Admins (DA), che è un membro del gruppo Administrators del dominio e un membro del gruppo Administrators locale in ogni computer che viene aggiunto al dominio. L'unico membro predefinito del gruppo per un dominio è l'account amministratore predefinito per tale dominio. DAs sono "sia" all'interno dei domini, mentre EAs dispone dei privilegi a livello di foresta. In un modello di delega correttamente progettato e implementato, l'appartenenza al gruppo Domain Admins deve solo in scenari con "break glass" (ad esempio situazioni in cui è necessario un account con elevati livelli di privilegio in ogni computer nel dominio). Sebbene i meccanismi di delega di Active Directory nativi consentono la delega nella misura in cui è possibile usare gli account DA solo in scenari di emergenza, la costruzione di un modello di delega efficace può richiedere tempi lunghe e sfruttano molte organizzazioni strumenti di terze parti per velocizzare il processo.  
  
##### <a name="administrators"></a>Administrators  
Il terzo gruppo è il gruppo di Administrators (BA) locale di dominio predefinito in cui sono annidati DAs ed EAs. Questo gruppo dispone di molte delle autorizzazioni nella directory e nei controller di dominio e diritti diretti. Tuttavia, il gruppo di amministratori per un dominio non dispone di privilegi sui server membri o workstation. È tramite l'appartenenza al gruppo Administrators locale dei computer che viene concesso il privilegio locale.  
  
> [!NOTE]  
> Anche se queste sono le configurazioni predefinite di questi gruppi con privilegi, un membro di uno qualsiasi dei tre gruppi consente di modificare la directory per ottenere l'appartenenza a uno qualsiasi degli altri gruppi. In alcuni casi, è semplice per ottenere l'appartenenza degli altri gruppi, mentre in altri casi è più difficile, ma dal punto di vista dei privilegi potenziali, tutti i tre gruppi devono essere considerati equivalenti in modo efficace.  
  
##### <a name="schema-admins"></a>Schema Admins  

Un quarto privilegiato, gruppo Schema Admins (SA), esiste solo nel dominio radice della foresta e dispone solo di quel dominio account Administrator predefinito come membro predefinito, simile al gruppo Enterprise Admins. Gruppo Schema Admins deve essere compilata solo temporaneamente e, in alcuni casi (quando è richiesta la modifica dello schema di Active Directory Domain Services).  
  
Anche se il gruppo dell'amministratore di sistema è l'unico gruppo che può modificare lo schema di Active Directory (ovvero, strutture di dati sottostanti della directory, ad esempio gli oggetti e attributi), l'ambito dei diritti e autorizzazioni del gruppo dell'amministratore di sistema è più limitata descritto in precedenza gruppi. È anche comune per trovare che le organizzazioni hanno sviluppato procedure consigliate appropriate per la gestione dell'appartenenza del gruppo dell'amministratore di sistema poiché l'appartenenza al gruppo è in genere necessaria raramente e solo per brevi periodi di tempo. Questo è tecnicamente vero dei EA, DA e BA gruppi in Active Directory, ma è molto meno comune per trovare che le organizzazioni hanno implementato procedure simili per questi gruppi come per il gruppo dell'amministratore di sistema.  
  
#### <a name="protected-accounts-and-groups-in-active-directory"></a>Gli account protetti e gruppi in Active Directory  
All'interno di Active Directory, un set predefinito di account con privilegi e i gruppi denominati "protetto" account e gruppi protetti in modo diverso rispetto ad altri oggetti nella directory. Tutti gli account che ha appartenenza diretta o transitiva in qualsiasi gruppo protetto (indipendentemente dal fatto che l'appartenenza è derivata dai gruppi di sicurezza o di distribuzione) eredita la sicurezza con restrizioni.  

  
Ad esempio, se un utente è membro di un gruppo di distribuzione, a sua volta, un membro di un gruppo protetto in Active Directory, l'oggetto user viene contrassegnato come un account protetto. Quando un account viene contrassegnato come account protetti, il valore dell'attributo adminCount sull'oggetto è impostato su 1.  
  
> [!NOTE]
> Sebbene transitiva appartenenza a un gruppo protetto include distribuzione annidata e i gruppi di sicurezza nidificati, gli account che sono membri dei gruppi di distribuzione annidata non riceverà il SID del gruppo protetto nei token di accesso. Tuttavia, i gruppi di distribuzione possono essere convertiti a gruppi di sicurezza in Active Directory, motivo per cui i gruppi di distribuzione sono inclusi nell'enumerazione membro gruppo protetto. Un gruppo di distribuzione annidata protetti deve essere convertito mai in un gruppo di sicurezza, gli account che sono membri del gruppo di distribuzione precedente verranno visualizzato successivamente padre protetti SID del gruppo nella loro i token di accesso al successivo accesso.  
  
La tabella seguente elenca gli account predefiniti protetti e i gruppi in Active Directory tramite la versione e sui service pack del sistema operativo.  
  
**Account protetti e gruppi in Active Directory dal sistema operativo e versione del Service Pack (SP) predefiniti**  
  
|||||  
|-|-|-|-|  
|**Windows 2000 <SP4**|**Windows 2000 SP4 -Windows Server 2003**|**Windows Server 2003 SP1+**|**Windows Server 2008 -Windows Server 2012**|  
|Administrators|Account Operators|Account Operators|Account Operators|  
||Administrator|Administrator|Administrator|  
||Administrators|Administrators|Administrators|  
|Domain Admins|Backup Operators|Backup Operators|Backup Operators|  
||Cert Publishers|||  
||Domain Admins|Domain Admins|Domain Admins|  
|Enterprise Admins|Controller di dominio|Controller di dominio|Controller di dominio|  
||Enterprise Admins|Enterprise Admins|Enterprise Admins|  
||Krbtgt|Krbtgt|Krbtgt|  
||Print Operators|Print Operators|Print Operators|  
||||Controller di dominio di sola lettura|  
||Replicator|Replicator|Replicator|  
|Schema Admins|Schema Admins||Schema Admins|  
  
##### <a name="adminsdholder-and-sdprop"></a>AdminSDHolder e SDProp  
Nel contenitore di sistema di ogni dominio di Active Directory, viene creato automaticamente un oggetto denominato AdminSDHolder. Lo scopo dell'oggetto AdminSDHolder è garantire che le autorizzazioni per gli account protetti e gruppi vengano applicate in modo coerente, indipendentemente dal fatto in cui si trovano i gruppi protetti e gli account nel dominio.  

Ogni 60 minuti (impostazione predefinita), un processo noto come servizio propagazione descrittore di sicurezza (SDProp) viene eseguito nel controller di dominio che detiene il ruolo di emulatore PDC del dominio. SDProp mette a confronto le autorizzazioni sull'oggetto AdminSDHolder del dominio con le autorizzazioni per gli account protetti e i gruppi nel dominio. Se le autorizzazioni per gli account protetti e i gruppi di non corrispondono le autorizzazioni per l'oggetto AdminSDHolder, le autorizzazioni per gli account protetti e i gruppi vengono reimpostate corrispondano a quelli dell'oggetto AdminSDHolder del dominio.  
  
Ereditarietà delle autorizzazioni è disabilitato nei gruppi protetti e gli account, il che significa che anche se gli account o i gruppi vengono spostati in percorsi diversi nella directory, non ereditano le autorizzazioni dai relativi oggetti padre nuovo. Ereditarietà verrà anche disabilitata per l'oggetto AdminSDHolder in modo che le modifiche alle autorizzazioni per gli oggetti padre non modificare le autorizzazioni di AdminSDHolder.  
  
> [!NOTE]
> Quando un account viene rimosso da un gruppo protetto, questo viene considerato non è più un account protetto, ma rimane impostato su attributo relativo adminCount impostato su 1 se non viene modificato manualmente. Il risultato di questa configurazione è che gli ACL dell'oggetto non vengano più aggiornati da SDProp, ma l'oggetto ancora non eredita le autorizzazioni dal relativo oggetto padre. Pertanto, l'oggetto può trovarsi in un'unità organizzativa (OU), a cui sono state delegate le autorizzazioni, ma l'oggetto protetto in precedenza non erediterà le autorizzazioni delegate. Uno script per individuare e ripristinare gli oggetti protetti in precedenza nel dominio è reperibile nella [articolo del supporto Microsoft 817433](https://support.microsoft.com/?id=817433).  
  
###### <a name="adminsdholder-ownership"></a>AdminSDHolder Ownership  
La maggior parte degli oggetti in Active Directory appartengono al gruppo del dominio BA. Tuttavia, l'oggetto AdminSDHolder, per impostazione predefinita, appartiene a gruppo DA del dominio. (Si tratta di una circostanza in cui DAs non derivano i diritti e autorizzazioni tramite l'appartenenza al gruppo Administrators per il dominio).  
  
Nelle versioni di Windows precedenti a Windows Server 2008, i proprietari di un oggetto possono modificare le autorizzazioni dell'oggetto, tra cui attribuirsi autorizzazioni che non disponevano di origine. Di conseguenza, le autorizzazioni predefinite nell'oggetto AdminSDHolder di un dominio impediscono agli utenti che sono membri dei gruppi di base o con contratto Enterprise di modificare le autorizzazioni per l'oggetto AdminSDHolder di un dominio. Tuttavia, i membri del gruppo Administrators per il dominio possono assumere la proprietà dell'oggetto e concedere loro autorizzazioni aggiuntive, che significa che questa protezione viene paragonata e protegge solo l'oggetto dalla modifica accidentale da parte degli utenti che hanno non membri del gruppo nel dominio. Inoltre, i bin e con contratto Enterprise (dove applicabile) gruppi dispongono dell'autorizzazione per modificare gli attributi dell'oggetto AdminSDHolder nel dominio locale (dominio radice per EA).  
  
> [!NOTE]  
> Un attributo dell'oggetto AdminSDHolder, dSHeuristics, che consente la personalizzazione limitata (rimozione) di gruppi che sono considerati gruppi protetti e sono interessati da AdminSDHolder e SDProp. Questa personalizzazione deve essere considerata attentamente se viene implementato, anche se esistono circostanze validi in cui è utile modificare dSHeuristics in AdminSDHolder. Altre informazioni sulla modifica dell'attributo dSHeuristics su un oggetto AdminSDHolder sono reperibili negli articoli di supporto tecnico Microsoft [817433](https://support.microsoft.com/?id=817433) e [973840](https://support.microsoft.com/kb/973840)e in [appendice c: Account protetti e gruppi in Active Directory](Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
Anche se i gruppi con più privilegi in Active Directory sono descritte di seguito, esistono numerosi altri gruppi che sono stati concessi privilegi elevati livelli di privilegio. Per altre informazioni su tutti i gruppi predefiniti in Active Directory e dei diritti utente assegnati a ogni e predefiniti, vedere [appendice b: Gruppi in Active Directory e account con privilegi](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md).  
  


