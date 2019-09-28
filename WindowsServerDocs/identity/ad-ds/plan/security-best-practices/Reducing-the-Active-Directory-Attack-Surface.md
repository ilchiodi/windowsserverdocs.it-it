---
ms.assetid: 864ad4bc-8428-4a8b-8671-cb93b68b0c03
title: Riduzione della superficie di attacco di Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 94bc65d42fa90dd7c93ba759a41d34edec10de09
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367653"
---
# <a name="reducing-the-active-directory-attack-surface"></a>Riduzione della superficie di attacco di Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questa sezione è incentrata sui controlli tecnici da implementare per ridurre la superficie di attacco dell'installazione del Active Directory. La sezione contiene le informazioni seguenti:  
  
-   L' [implementazione di modelli amministrativi con privilegi minimi](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md) è incentrata sull'identificazione del rischio di utilizzo di account con privilegi elevati per l'amministrazione quotidiana, oltre a fornire raccomandazioni da implementare per ridurre il rischio che sono presenti account con privilegi.  
  
-   L' [implementazione di host amministrativi protetti](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md) descrive i principi per la distribuzione di sistemi amministrativi dedicati e protetti, oltre ad alcuni approcci di esempio a una distribuzione host amministratore sicura.  
  
-   La [protezione dei controller di dominio contro gli attacchi](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md) illustra i criteri e le impostazioni che, sebbene simili ai suggerimenti per l'implementazione di host amministrativi sicuri, contengono alcune raccomandazioni specifiche del controller di dominio per la guida Assicurarsi che i controller di dominio e i sistemi usati per gestirli siano protetti.  
  
## <a name="privileged-accounts-and-groups-in-active-directory"></a>Account e gruppi con privilegi in Active Directory  
In questa sezione vengono fornite informazioni complementari sugli account e i gruppi con privilegi in Active Directory destinati a illustrare le comunanze e le differenze tra gli account con privilegi e i gruppi in Active Directory. Grazie alla comprensione di queste distinzioni, se si implementano le raccomandazioni nell' [implementazione di modelli amministrativi con privilegi minimi](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md) o si sceglie di personalizzarli per l'organizzazione, sono disponibili gli strumenti necessari per proteggere ogni gruppo e account appropriato.  
  
### <a name="built-in-privileged-accounts-and-groups"></a>Account e gruppi predefiniti con privilegi  
Active Directory facilita la delega dell'amministrazione e supporta il principio dei privilegi minimi nell'assegnazione di diritti e autorizzazioni. Gli utenti "normali" che dispongono di account in un dominio sono, per impostazione predefinita, in grado di leggere la maggior parte degli elementi archiviati nella directory, ma sono in grado di modificare solo un set molto limitato di dati nella directory. Agli utenti che richiedono privilegi aggiuntivi è possibile concedere l'appartenenza a diversi gruppi "con privilegi" compilati nella directory, in modo che possano eseguire attività specifiche correlate ai rispettivi ruoli, ma non possono eseguire attività che non sono rilevanti per le proprie mansioni. Le organizzazioni possono inoltre creare gruppi adattati a responsabilità specifiche per i processi e a cui vengono concessi diritti e autorizzazioni granulari che consentono al personale IT di eseguire funzioni amministrative quotidiane senza concedere diritti e autorizzazioni che superino il è obbligatorio per queste funzioni.  
  
All'interno Active Directory tre gruppi predefiniti sono i gruppi di privilegi più elevati nella directory: Enterprise Admins, Domain Admins e amministratori. Le funzionalità e la configurazione predefinite di ognuno di questi gruppi sono descritte nelle seguenti sezioni:  
  
#### <a name="highest-privilege-groups-in-active-directory"></a>Gruppi di privilegi più elevati in Active Directory  
  
##### <a name="enterprise-admins"></a>Enterprise Admins  
Enterprise Admins (EA) è un gruppo esistente solo nel dominio radice della foresta e, per impostazione predefinita, è un membro del gruppo Administrators in tutti i domini della foresta. L'account Administrator predefinito nel dominio radice della foresta è l'unico membro predefinito del gruppo EA. A EAs vengono concessi diritti e autorizzazioni che consentono di implementare modifiche a livello di foresta (ovvero modifiche che interessano tutti i domini della foresta), ad esempio l'aggiunta o la rimozione di domini, la definizione di trust tra foreste o la generazione di livelli di funzionalità della foresta. In un modello di delega progettato e implementato correttamente, l'appartenenza a EA è necessaria solo quando si crea prima la foresta o quando si apportano modifiche a livello di foresta, ad esempio la definizione di un trust tra foreste in uscita. La maggior parte dei diritti e delle autorizzazioni concesse al gruppo EA può essere delegata a utenti e gruppi con privilegi minori.  
  
##### <a name="domain-admins"></a>Domain Admins  

Ogni dominio in una foresta ha il proprio gruppo Domain Admins (DA), che è un membro del gruppo Administrators del dominio e un membro del gruppo Administrators locale in ogni computer aggiunto al dominio. L'unico membro predefinito del gruppo DA per un dominio è l'account amministratore predefinito per tale dominio. DAs sono "tutti potenti" all'interno dei propri domini, mentre EAs dispone di privilegi a livello di foresta. In un modello di delega progettato e implementato correttamente, l'appartenenza a Domain Admins dovrebbe essere necessaria solo negli scenari "Break Glass", ad esempio in situazioni in cui è necessario un account con livelli elevati di privilegio in ogni computer del dominio. Sebbene i meccanismi di delega Active Directory nativi consentano la delega nella misura in cui è possibile usare gli account DA solo negli scenari di emergenza, la costruzione di un modello di delega efficace può richiedere molto tempo e molte organizzazioni usano strumenti di terze parti per accelerare il processo.  
  
##### <a name="administrators"></a>Administrators  
Il terzo gruppo è il gruppo Administrators locale di dominio (BA) in cui sono annidate le funzionalità DAs e EAs. A questo gruppo vengono concessi molti diritti e autorizzazioni diretti nella directory e nei controller di dominio. Il gruppo Administrators per un dominio non dispone tuttavia dei privilegi per i server membri o le workstation. È tramite l'appartenenza al gruppo Administrators locale del computer a cui viene concesso il privilegio locale.  
  
> [!NOTE]  
> Sebbene queste siano le configurazioni predefinite di questi gruppi privilegiati, un membro di uno dei tre gruppi può modificare la directory per ottenere l'appartenenza a uno degli altri gruppi. In alcuni casi, è semplice ottenere l'appartenenza agli altri gruppi, mentre in altri è più difficile, ma dal punto di vista del potenziale privilegio, tutti e tre i gruppi devono essere considerati equivalenti.  
  
##### <a name="schema-admins"></a>Schema Admins  

Un quarto gruppo con privilegi, Schema Admins (SA), esiste solo nel dominio radice della foresta e dispone solo dell'account Administrator predefinito del dominio come membro predefinito, in modo analogo al gruppo Enterprise Admins. Il gruppo Schema Admins deve essere popolato solo temporaneamente e occasionalmente (quando è richiesta la modifica dello schema di servizi di dominio Active Directory).  
  
Sebbene il gruppo SA sia l'unico gruppo in grado di modificare lo schema di Active Directory (ovvero, le strutture di dati sottostanti della directory, ad esempio oggetti e attributi), l'ambito dei diritti e delle autorizzazioni del gruppo SA è più limitato rispetto a quanto descritto in precedenza. gruppi. È anche comune rilevare che le organizzazioni hanno sviluppato procedure appropriate per la gestione dell'appartenenza al gruppo SA perché l'appartenenza al gruppo è in genere necessaria raramente e solo per brevi periodi di tempo. Questa situazione è tecnicamente vera anche per i gruppi EA, DA e BA in Active Directory, ma è molto meno comune trovare che le organizzazioni abbiano implementato procedure simili per questi gruppi come per il gruppo SA.  
  
#### <a name="protected-accounts-and-groups-in-active-directory"></a>Account e gruppi protetti in Active Directory  
All'interno Active Directory, un set predefinito di account con privilegi e gruppi denominati account e gruppi "protetti" viene protetto in modo diverso rispetto ad altri oggetti nella directory. Qualsiasi account con appartenenza diretta o transitiva a qualsiasi gruppo protetto (indipendentemente dal fatto che l'appartenenza sia derivata da gruppi di sicurezza o di distribuzione) eredita questa sicurezza con restrizioni.  

  
Se, ad esempio, un utente è membro di un gruppo di distribuzione che, a sua volta, è membro di un gruppo protetto in Active Directory, tale oggetto utente viene contrassegnato come account protetto. Quando un account viene contrassegnato come account protetto, il valore dell'attributo adminCount dell'oggetto viene impostato su 1.  
  
> [!NOTE]
> Sebbene l'appartenenza transitiva in un gruppo protetto includa la distribuzione annidata e i gruppi di sicurezza annidati, gli account membri dei gruppi di distribuzione annidati non riceveranno il SID del gruppo protetto nei token di accesso. Tuttavia, i gruppi di distribuzione possono essere convertiti in gruppi di sicurezza in Active Directory, motivo per cui i gruppi di distribuzione sono inclusi nell'enumerazione dei membri del gruppo protetto. Se un gruppo di distribuzione annidato protetto viene mai convertito in un gruppo di sicurezza, gli account che sono membri del gruppo di distribuzione precedente riceveranno successivamente il SID del gruppo protetto padre nei token di accesso all'accesso successivo.  
  
La tabella seguente elenca i gruppi e gli account protetti predefiniti in Active Directory dalla versione del sistema operativo e dal livello di Service Pack.  
  
**Account e gruppi protetti predefiniti in Active Directory dalla versione del sistema operativo e del Service Pack (SP)**  
  
|||||  
|-|-|-|-|  
|**Windows 2000 < SP4**|**Windows 2000 SP4-Windows Server 2003**|**Windows Server 2003 SP1 +**|**Windows Server 2008-Windows Server 2012**|  
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
Nel contenitore di sistema di ogni dominio Active Directory, viene creato automaticamente un oggetto denominato AdminSDHolder. Lo scopo dell'oggetto AdminSDHolder è garantire che le autorizzazioni per gli account e i gruppi protetti vengano applicate in modo coerente, indipendentemente dalla posizione in cui si trovano i gruppi e gli account protetti nel dominio.  

Ogni 60 minuti (per impostazione predefinita), un processo noto come propagatore del descrittore di sicurezza (SDProp) viene eseguito nel controller di dominio che include il ruolo emulatore PDC del dominio. SDProp Confronta le autorizzazioni per l'oggetto AdminSDHolder del dominio con le autorizzazioni per gli account e i gruppi protetti nel dominio. Se le autorizzazioni per uno degli account e dei gruppi protetti non corrispondono alle autorizzazioni per l'oggetto AdminSDHolder, le autorizzazioni per gli account e i gruppi protetti vengono reimpostate in modo da corrispondere a quelle dell'oggetto AdminSDHolder del dominio.  
  
L'ereditarietà delle autorizzazioni è disabilitata per i gruppi e gli account protetti. Ciò significa che, anche se gli account o i gruppi vengono spostati in percorsi diversi nella directory, non ereditano le autorizzazioni dai nuovi oggetti padre. L'ereditarietà è disabilitata anche nell'oggetto AdminSDHolder, in modo che le autorizzazioni modificate per gli oggetti padre non modifichino le autorizzazioni di AdminSDHolder.  
  
> [!NOTE]
> Quando un account viene rimosso da un gruppo protetto, non viene più considerato un account protetto, ma il relativo attributo adminCount rimane impostato su 1 se non viene modificato manualmente. Il risultato di questa configurazione è che gli ACL dell'oggetto non vengono più aggiornati da SDProp, ma l'oggetto non eredita ancora le autorizzazioni dal relativo oggetto padre. Pertanto, l'oggetto può trovarsi in un'unità organizzativa (OU) a cui sono state delegate le autorizzazioni, ma l'oggetto precedentemente protetto non erediterà tali autorizzazioni delegate. Uno script per individuare e reimpostare gli oggetti precedentemente protetti nel dominio si trova nel [supporto tecnico Microsoft articolo 817433](https://support.microsoft.com/?id=817433).  
  
###### <a name="adminsdholder-ownership"></a>Proprietà di AdminSDHolder  
La maggior parte degli oggetti in Active Directory appartiene al gruppo BA del dominio. Tuttavia, l'oggetto AdminSDHolder è, per impostazione predefinita, di proprietà del gruppo DA del dominio. Si tratta di una circostanza in cui DAs non derivano i diritti e le autorizzazioni tramite l'appartenenza al gruppo Administrators per il dominio.  
  
Nelle versioni di Windows precedenti a Windows Server 2008, i proprietari di un oggetto possono modificare le autorizzazioni dell'oggetto, inclusa la concessione delle autorizzazioni stesse che non avevano originariamente. Pertanto, le autorizzazioni predefinite per l'oggetto AdminSDHolder di un dominio impediscono agli utenti che sono membri dei gruppi BA o EA di modificare le autorizzazioni per l'oggetto AdminSDHolder di un dominio. Tuttavia, i membri del gruppo Administrators per il dominio possono assumere la proprietà dell'oggetto e concedere autorizzazioni aggiuntive, il che significa che questa protezione è rudimentale e protegge solo l'oggetto da modifiche accidentali da parte degli utenti non membri del gruppo DA nel dominio. Inoltre, i gruppi BA e EA (dove applicabile) dispongono delle autorizzazioni per modificare gli attributi dell'oggetto AdminSDHolder nel dominio locale (dominio radice per EA).  
  
> [!NOTE]  
> Un attributo nell'oggetto AdminSDHolder, dSHeuristics, consente la personalizzazione limitata (rimozione) di gruppi considerati gruppi protetti e sono interessati da AdminSDHolder e SDProp. Questa personalizzazione deve essere considerata attentamente se è implementata, sebbene esistano circostanze valide in cui la modifica di dSHeuristics su AdminSDHolder è utile. Ulteriori informazioni sulla modifica dell'attributo dSHeuristics in un oggetto AdminSDHolder sono disponibili negli articoli supporto tecnico Microsoft [817433](https://support.microsoft.com/?id=817433) e [973840](https://support.microsoft.com/kb/973840)e in [Appendix C: Account e gruppi protetti in Active Directory @ no__t-0.  
  
Sebbene i gruppi con privilegi più elevati in Active Directory siano descritti in questo articolo, sono disponibili diversi altri gruppi a cui sono stati concessi livelli di privilegio elevati. Per ulteriori informazioni su tutti i gruppi predefiniti e incorporati in Active Directory e sui diritti utente assegnati a ognuno di essi, vedere [Appendix B: Account e gruppi con privilegi in Active Directory @ no__t-0.  
  


