---
ms.assetid: 7a7ab95c-9cb3-4a7b-985a-3fc08334cf4f
title: Implementazione dei modelli amministrativi con privilegi minimi
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9716d442446b2705dfd2803d061cb884a5e72fbe
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="implementing-least-privilege-administrative-models"></a>Implementazione dei modelli amministrativi con privilegi minimi

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

L'estratto seguente è tratto dal [di Administrator Accounts Security Planning Guide](https://technet.microsoft.com/library/cc162797.aspx), prima pubblicazione il 1 aprile 1999:  
  
> "Corsi di formazione più correlate alla sicurezza e la documentazione esaminata l'implementazione di un principio di privilegio minimo, ma raramente seguono le organizzazioni. Il principio è semplice e l'impatto dell'applicazione corretta aumenta la sicurezza e riduce i rischi. Il principio specifica che tutti gli utenti devono accedere con un account utente che ha le autorizzazioni necessarie per completare l'attività corrente e nient'altro. In questo modo fornisce protezione da codice dannoso, tra altri attacchi. Questo principio si applica ai computer e gli utenti di tali computer.   
> "Un motivo per cui che questo principio funziona bene è la necessità di eseguire ricerche interne. Ad esempio, è necessario determinare i privilegi di accesso da un computer o un utente deve e quindi implementarli. Per molte organizzazioni, questa attività può inizialmente sembrare una gran quantità di lavoro; Tuttavia, è essenziale per proteggere correttamente l'ambiente di rete.   
> "È necessario concedere tutti gli utenti amministratore di dominio i propri privilegi di dominio con il concetto di privilegio minimo. Ad esempio, se un amministratore accede con un account con privilegi e inavvertitamente esegue un programma antivirus, il virus dispone di accesso amministrativo al computer locale e all'intero dominio. Se l'amministratore ha invece l'accesso con un account senza privilegi (non amministrativo), ambito del virus di danni essere solo il computer locale in quanto viene eseguito come un utente del computer locale.   
> "In un altro esempio, gli account a cui vengono concessi diritti di amministratore di dominio devono non disponga di diritti elevati in un'altra foresta, anche se esiste una relazione di trust tra foreste. Questa strategia consente di evitare danni estesi se un utente malintenzionato di compromettere un insieme di strutture gestito. Le organizzazioni devono controllare regolarmente la rete per la protezione contro non autorizzati di privilegi più elevati."  
  
L'estratto seguente è tratto dal [Microsoft Windows Security Resource Kit](https://www.microsoft.com/learning/en/us/book.aspx?ID=6815&locale=en-us), primo pubblicato nel 2005:  
  
> "Sempre pensare alla sicurezza in termini di concedere il minimo di privilegi necessari per eseguire l'operazione. Se un'applicazione che dispone di privilegi troppi deve essere compromesso, l'autore dell'attacco potrebbe essere in grado di espandere l'attacco oltre che verrebbe restituito se l'applicazione fosse stata sotto il minimo di privilegi possibili. Ad esempio, esaminare le conseguenze di un amministratore di rete involontariamente aprire allegati di posta elettronica che avvia un virus. Se l'amministratore è connesso utilizzando l'account Administrator del dominio, il virus disporrà di privilegi di amministratore su tutti i computer nel dominio e pertanto di accesso illimitato a quasi tutti i dati in rete. Se l'amministratore accede utilizzando un account amministratore locale, il virus disporrà di privilegi di amministratore nel computer locale e pertanto sarebbe in grado di accedere ai dati nel computer e installare il software dannoso, ad esempio la registrazione traccia chiave software nel computer. Se l'amministratore viene effettuato l'accesso utilizzando un account utente normale, il virus avrà accesso solo ai dati dell'amministratore e non sarà in grado di installare il software dannoso. Utilizzando i privilegi minimi necessari per la lettura di posta elettronica, in questo esempio, l'ambito del danno potenziale è notevolmente ridotto."  
  
## <a name="the-privilege-problem"></a>Il problema con privilegi  
I principi descritti in estratti precedenti non sono stati modificati, ma nel valutare le installazioni di Active Directory, troviamo invariabilmente un numero eccessivo di account che sono stati concessi i diritti e autorizzazioni ben oltre quelle necessarie per eseguire operazioni quotidiane. Le dimensioni dell'ambiente interessa i valori non elaborati di account con privilegi, ma non le directory proportionmidsized possono contenere decine di account nei gruppi con privilegi più elevati, mentre installazioni di grandi dimensioni potrebbero essere centinaia o persino migliaia. Con poche eccezioni, indipendentemente dalla complessità di un utente malintenzionato competenze e arsenale, gli autori di attacchi seguono in genere il percorso più semplice. Consentono di aumentare la complessità dei relativi strumenti e approccio solo se e quando più semplici meccanismi fail o vengono violate da difensori.  
  
Sfortunatamente, il percorso più semplice in molti ambienti ha dimostrato di essere l'utilizzo eccessivo di account con privilegi vasto e complesso. Ampi privilegi sono diritti e autorizzazioni che consentono a un account eseguire attività specifiche in una sezione di grandi dimensione di ambiente, ad esempio, personale dell'Help Desk può disporre di autorizzazioni che consentono loro di reimpostare le password in numero di account utente.  
  
Deep privilegi sono potenti che vengono applicati a un segmento stretto della popolazione, tali fornendo un tecnico diritti di amministratore in un server in modo che possano eseguire riparazioni. È necessariamente pericoloso privilegio ampia né privilegio completa, ma quando molti account nel dominio in modo permanente vengono concessi privilegi vasto e complesso, se solo uno degli account viene compromesso, rapidamente può essere utilizzato per riconfigurare l'ambiente per scopi dell'utente malintenzionato o persino distruggere segmenti dell'infrastruttura di grandi dimensioni.  
  
Gli attacchi pass-the-hash, che sono un tipo di attacco contro il furto di credenziali, vengono generati poiché gli strumenti per eseguire tali operazioni sono liberamente disponibili e facile da usare e molti ambienti sono vulnerabili agli attacchi. Gli attacchi pass-the-hash, tuttavia, non sono il problema reale. Il punto cruciale del problema è duplice:  
  
1.  È generalmente semplice per un utente malintenzionato di ottenere privilegi complete in un singolo computer, quindi propagare tale privilegio su vasta scala in altri computer.  
  
2.  Esistono troppi account permanente con livelli di privilegio elevati attraverso l'intero spettro di elaborazione.  
  
Anche se vengono eliminati attacchi pass-the-hash, gli utenti malintenzionati utilizzerà semplicemente strategie differenti, non una strategia diversa. Anziché impianto malware che contiene gli strumenti di furto delle credenziali, si potrebbe prevede malware che registra le pressioni di tasti o utilizzare un numero qualsiasi di altri approcci per acquisire le credenziali che sono potenti in ambiente. Indipendentemente dalla tattiche, gli obiettivi rimangono gli stessi: account con privilegi vasto e complesso.  
  
Concessione dei privilegi eccessivi non solo trovare in Active Directory negli ambienti di compromessi. Quando un'organizzazione ha sviluppato l'abitudine di concessione di più privilegi è necessaria, in genere risulta in tutta l'infrastruttura come descritto nelle sezioni seguenti.  
  
## <a name="in-active-directory"></a>In Active Directory  
In Active Directory, è comune per trovare che i gruppi di EA, DA e BA contengano un numero eccessivo di account. In genere, gruppo EA di un'organizzazione contiene i membri minor numero, DA gruppi contengono in genere un moltiplicatore del numero di utenti del gruppo EA e gruppi di amministratori in genere contengono più membri popolazioni di altri gruppi combinati. Questo è spesso dovuto a una convinzione che gli amministratori sono in qualche modo "meno privilegiato" rispetto a DAs o EAs. Mentre i diritti e autorizzazioni concesse a ognuno di questi gruppi sono diverse, essi devono essere efficacemente considerati ugualmente gruppi potente perché può rendere membro di uno stesso o al membro di altri due.  
  
## <a name="on-member-servers"></a>Sui server membri  
Quando si recupera l'appartenenza del gruppo Administrators locale sui server membri in molti ambienti, troviamo appartenenza compreso tra un numero limitato di account locali e di dominio, e decine di gruppi nidificati che, se viene espansa, rivelare centinaia, migliaia di account con privilegi di amministratore locale sui server. In molti casi, i gruppi di dominio con le appartenenze di grandi dimensioni sono annidati in gruppo Administrators locale server membri, senza considerazione del fatto che tutti gli utenti possono modificare le appartenenze a tali gruppi nel dominio possono ottenere il controllo amministrativo di tutti i sistemi in cui il gruppo è stato annidato in un gruppo di amministratori locale.  
  
## <a name="on-workstations"></a>Sulle workstation  
Sebbene le workstation hanno in genere un numero decisamente inferiore di membri in gruppi di amministratori locali rispetto ai server membri, in molti ambienti, gli utenti vengono concesse l'appartenenza al gruppo Administrators locale nel computer personale. In questo caso, anche se questa opzione è abilitata, gli utenti presentano un rischio elevato per l'integrità delle workstation.  
  
> [!IMPORTANT]  
> È necessario considerare attentamente se gli utenti richiedono diritti amministrativi sulle proprie workstation e in caso affermativo, potrebbe essere un approccio migliore per creare un account locale separato nel computer in cui è membro del gruppo Administrators. Quando gli utenti richiedono l'elevazione dei privilegi, essi possono presentare le credenziali dell'account locale per l'elevazione dei privilegi, ma poiché l'account è locale, non può essere utilizzato per compromettere gli altri computer o accedere alle risorse di dominio. Come con tutti gli account locali, tuttavia, le credenziali per l'account con privilegiato locale devono essere univoche. Se si crea un account locale con le stesse credenziali di workstation più, si espongono i computer da attacchi pass-the-hash.  
  
## <a name="in-applications"></a>Nelle applicazioni  
Per gli attacchi in cui la destinazione è di proprietà intellettuale di un'organizzazione, gli account che sono stati concessi privilegi potente all'interno delle applicazioni di destinazione per consentire exfiltration dei dati. Anche se gli account che hanno accesso a dati riservati non potrebbero essere stato concesso privilegi elevati nel dominio o del sistema operativo, gli account che consente di modificare la configurazione di un'applicazione o accedere alle informazioni dell'applicazione fornisce rischio presenta.  
  
## <a name="in-data-repositories"></a>In archivi dati  
Come avviene con altre destinazioni, gli utenti malintenzionati tentando di accedere alla proprietà intellettuale sotto forma di documenti e altri file possono essere destinati gli account che controllano l'accesso al file di archivi, gli account che hanno accesso diretto ai gruppi di file, o persino o ruoli che hanno accesso ai file. Se viene utilizzato un file server per archiviare i documenti di contratto e accedere ai documenti mediante l'uso di un gruppo di Active Directory, ad esempio, un utente malintenzionato può modificare l'appartenenza del gruppo possa aggiungere compromesso account al gruppo e accedere ai documenti di contratto. Nei casi in cui viene fornito l'accesso ai documenti da applicazioni come SharePoint, gli utenti malintenzionati possono individuare le applicazioni come descritto in precedenza.  
  
## <a name="reducing-privilege"></a>Riduzione dei privilegi  
Il più grande e più complesso, un ambiente più difficile è per gestire e proteggere. In organizzazioni di piccole dimensioni, revisione e riduzione dei privilegi può essere un'attività relativamente complessa, ma ogni server aggiuntivo, workstation, account utente e applicazione in uso in un'organizzazione aggiunge un altro oggetto che deve essere protetta. Poiché può essere difficile o persino impossibile da correttamente proteggere ogni aspetto di un'organizzazione infrastruttura IT, è consigliabile concentrarsi sforzi innanzitutto sugli account con privilegi creare rischi maggiori, che sono in genere l'elemento predefinito con privilegiata di gruppi e account in Active Directory e account locali con privilegi sulle workstation e server membri.  
  
### <a name="securing-local-administrator-accounts-on-workstations-and-member-servers"></a>Protezione degli account di amministratore locale sui server membri e workstation  
Anche se questo documento è incentrato sulla protezione di Active Directory, come è stata discussa in precedenza, la maggior parte degli attacchi contro la directory iniziano come gli attacchi contro singoli host. Non è possibile fornire linee guida complete per la protezione dei gruppi locali nei sistemi di membro, ma i consigli seguenti possono essere usati per proteggere gli account amministratore locali sui server membri e workstation.  
  
#### <a name="securing-local-administrator-accounts"></a>Protezione degli account di amministratore locale  
In tutte le versioni di Windows attualmente coperte dal supporto mainstream, l'account Administrator locale è disabilitato per impostazione predefinita, che rende inutilizzabile per pass-the-hash e altri attacchi contro il furto di credenziali di account. Tuttavia, in domini contenenti i sistemi operativi legacy o gli account amministratore locali sono stati abilitati, questi account possono essere utilizzati come descritto in precedenza per propagare i compromessi tra le workstation e server membri. Per questo motivo, i seguenti controlli sono consigliati per tutti gli account amministratore locali in sistemi appartenenti a un dominio.  
  
Vengono fornite istruzioni dettagliate per l'implementazione di questi controlli [appendice h: protezione Local Administrator account e gruppi](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md). Prima di implementare queste impostazioni, tuttavia, assicurarsi che gli account amministratore locali non attualmente utilizzati nell'ambiente per eseguire servizi nei computer o eseguire altre attività per cui questi account non devono essere utilizzati. Testare attentamente queste impostazioni prima di implementarli in un ambiente di produzione.  
  
#### <a name="controls-for-local-administrator-accounts"></a>Controlli per gli account amministratore locale  
Account amministratore predefinito non deve mai essere utilizzato come account del servizio sui server membri, né deve essere utilizzati per accedere al computer locale (eccetto in modalità provvisoria, che è consentito anche se l'account è disabilitato). L'obiettivo dell'implementazione di impostazioni descritte di seguito è per impedire account amministratore locale di ogni computer utilizzabile a meno che i controlli di protezione vengono innanzitutto invertiti. Implementazione di questi controlli e monitoraggio gli account amministratore per le modifiche, è notevolmente possibile ridurre la probabilità di successo di un attacco che gli account amministratore locale di destinazioni.  
  
##### <a name="configuring-gpos-to-restrict-administrator-accounts-on-domain-joined-systems"></a>Configurazione di oggetti Criteri di gruppo per limitare l'account Administrator in sistemi appartenenti a un dominio  
Uno o più oggetti Criteri di gruppo creare e collegare workstation e server membro unità organizzative in ogni dominio, aggiungere l'account amministratore per i seguenti diritti utente in **Configurazione computer\Criteri\Impostazioni di Windows\Impostazioni protezione\Criteri Settings\nome diritti**:  
  
-   Nega accesso al computer dalla rete  
  
-   Nega accesso come processo batch  
  
-   Nega accesso come servizio  
  
-   Nega accesso tramite Servizi Desktop remoto  
  
Quando si aggiungono account di amministratore a questi diritti utente, specificare se si aggiunge l'account amministratore locale o un amministratore del dominio dell'account comunque creare un'etichetta per l'account. Ad esempio, per aggiungere l'account di amministratore del dominio NWTRADERS a queste negare diritti, è necessario digitare l'account come **NWTRADERS\Administrator**, o selezionare l'account di amministratore per il dominio NWTRADERS. Per assicurarsi di limitare l'account amministratore locale, digitare **amministratore** in queste impostazioni in Editor criteri di gruppo i diritti utente.  
  
> [!NOTE]  
> Anche se gli account amministratore locali vengono rinominati, i criteri saranno ancora validi.  
  
Queste impostazioni garantirà l'account di amministratore del computer non può essere utilizzato per connettersi ad altri computer, anche se è accidentalmente o intenzionalmente abilitato. Gli accessi locali utilizzando l'account Administrator locale non possono essere disabilitati completamente, né si tenti di eseguire questa operazione, perché l'account amministratore locale del computer è progettato per essere usato negli scenari di ripristino di emergenza.  
  
Necessario un server membro o una workstation diventano separata dal dominio con alcun altro account locali concessi privilegi di amministratore, il computer può essere avviato in modalità provvisoria, può essere abilitato l'account amministratore e quindi l'account consente di effettuare operazioni di ripristino nel computer. Al termine delle operazioni di ripristino, l'account amministratore nuovamente deve essere disabilitata.  
  
### <a name="securing-local-privileged-accounts-and-groups-in-active-directory"></a>Gli account con privilegi di protezione locali e gruppi in Active Directory  
*Legge numero sei: un computer è protetto, l'amministratore è considerato attendibile.* - [Ten Immutable Laws of Security (Version 2.0)](https://technet.microsoft.com/security/hh278941.aspx)  
  
Le informazioni fornite qui sono destinate a fornire linee guida generali per la protezione di più account con privilegi e gruppi in Active Directory. Le istruzioni dettagliate vengono anche fornite [appendice d: protezione predefiniti Administrator account in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md), [appendice e: protezione dei gruppi Enterprise Admins in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md), [appendice f: protezione Domain Admins gruppi in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)e in [appendice g: protezione dei gruppi Administrators in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md).  
  
Prima di implementare una di queste impostazioni, è inoltre necessario verificare tutte le impostazioni attentamente per determinare se sono appropriati per l'ambiente. Non tutte le organizzazioni saranno in grado di implementare queste impostazioni.  
  
#### <a name="securing-built-in-administrator-accounts-in-active-directory"></a>Protezione predefiniti Administrator account in Active Directory  
In ogni dominio in Active Directory, un account amministratore viene creato come parte della creazione del dominio. Questo account è per impostazione predefinita un membro dei gruppi di amministratore nel dominio e Domain Admins, e se il dominio è il dominio radice della foresta, l'account sia anche membro del gruppo Enterprise Admins. Utilizzo dell'account amministratore locale del dominio deve essere riservata solo per attività di compilazione iniziali e, possibilmente, scenari di ripristino di emergenza. Per garantire un account amministratore predefinito può essere utilizzato per operazioni di ripristino nel caso in cui è non possibile utilizzare nessun altro account, è consigliabile non modificare l'appartenenza predefinita dell'account amministratore in qualsiasi dominio nella foresta. Al contrario, si deve seguendo le linee guida per proteggere l'account di amministratore in ogni dominio nella foresta. Vengono fornite istruzioni dettagliate per l'implementazione di questi controlli [appendice d: protezione predefiniti Administrator account in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
#### <a name="controls-for-built-in-administrator-accounts"></a>Controlli per gli account amministratore predefinito  
L'obiettivo dell'implementazione di impostazioni descritte di seguito è per impedire account amministratore di ciascun dominio (non un gruppo) siano utilizzabili, a meno che un numero di controlli è invertito. Implementazione di questi controlli e monitoraggio gli account di amministratore per le modifiche, è possibile ridurre notevolmente la probabilità di successo di un attacco sfruttando l'account di amministratore del dominio. Per l'account di amministratore in ogni dominio della foresta, è necessario configurare le impostazioni seguenti.  
  
##### <a name="enable-the-account-is-sensitive-and-cannot-be-delegated-flag-on-the-account"></a>Abilitare il flag "Account è sensibile e non può essere delegato" all'account  
Per impostazione predefinita, tutti gli account in Active Directory possono essere delegati. La delega consente a un computer o un servizio per presentare le credenziali per un account che ha autenticato sul computer o in altri computer per ottenere i servizi per conto dell'account. Quando si abilita il **Account è sensibile e non può essere delegato** attributo su un account basato su dominio, le credenziali dell'account non possono essere presentate ad altri computer o i servizi in rete, che limita gli attacchi che sfruttano la delega a usare le credenziali dell'account in altri sistemi.  
  
##### <a name="enable-the-smart-card-is-required-for-interactive-logon-flag-on-the-account"></a>Abilitare il flag "Smart card è necessaria per l'accesso interattivo" all'account  
Quando si abilita il **Smart card è necessaria per l'accesso interattivo** attributo su un account di Windows consente di reimpostare la password dell'account su un valore casuale a 120 caratteri. Impostando questo flag su un account amministratore predefinito, assicurarsi che la password per l'account non solo lunga e complessa, ma non è noto a tutti gli utenti. Non è tecnicamente necessario per la creazione di smart card per gli account prima di abilitare questo attributo, ma se possibile, le smart card deve essere creata per ogni account di amministratore prima di configurare le restrizioni di account e le smart card devono essere archiviate in percorsi sicuri.  
  
Sebbene l'impostazione di **Smart card è necessaria per l'accesso interattivo** flag Reimposta la password dell'account, impedisce che un utente con diritti per reimpostare la password dell'account da impostare l'account a un valore noto e utilizzando l'account nome e la nuova password per accedere alle risorse sulla rete. Per questo motivo, è necessario implementare i seguenti controlli aggiuntivi sul conto.  
  
##### <a name="disable-the-account"></a>Disabilitare l'Account  
Se l'account amministratore non è già disabilitato, è possibile disabilitarla al termine della configurazione delle proprietà dell'account. Ciò impedisce che l'account viene utilizzato per qualsiasi scopo a meno che non viene prima abilitato. In uno scenario di ripristino di emergenza in cui non sono disponibili account per eseguire operazioni di ripristino dell'ambiente di dominio Active Directory, puoi avviare un controller di dominio in modalità provvisoria, accedere localmente con l'account Administrator predefinito (che non è bloccato da un accesso locale) e abilitare l'account di amministratore del dominio, se necessario.  
  
##### <a name="configuring-gpos-to-restrict-domains-administrator-accounts-on-domain-joined-systems"></a>Configurazione di oggetti Criteri di gruppo per limitare l'account di amministratore in sistemi appartenenti a un dominio  
Sebbene la disabilitazione dell'account di amministratore in un dominio rende inutilizzabile in modo efficace l'account, è necessario implementare restrizioni aggiuntive per l'account nel caso in cui l'account è abilitato inavvertitamente o intenzionalmente. Sebbene questi controlli, in ultima analisi, possono essere annullati dall'account di amministratore, l'obiettivo consiste nel creare controlli di stato di avanzamento di un utente malintenzionato di rallentare e può provocare limite danno l'account.  
  
Uno o più oggetti Criteri di gruppo creare e collegare workstation e server membro unità organizzative in ogni dominio, aggiungere account di amministratore di ciascun dominio per i seguenti diritti utente in **Configurazione computer\Criteri\Impostazioni di Windows\Impostazioni protezione\Criteri Settings\nome diritti**:  
  
-   Nega accesso al computer dalla rete  
  
-   Nega accesso come processo batch  
  
-   Nega accesso come servizio  
  
-   Nega accesso tramite Servizi Desktop remoto  
  
> [!NOTE]  
> Quando si aggiungono l'account amministratore locale per questa impostazione, è necessario specificare se si configura account amministratore locale o un account amministratore di dominio. Ad esempio, per aggiungere account amministratore locale del dominio NWTRADERS a queste negare diritti, è necessario digitare l'account come **NWTRADERS\Administrator**, o selezionare l'account amministratore locale per il dominio NWTRADERS. Se si digita **amministratore** in queste impostazioni diritti utente in Editor criteri di gruppo, si limiterà l'account amministratore locale su ciascun computer in cui viene applicato l'oggetto Criteri di gruppo.  
>   
> È consigliabile limitare l'account amministratore locale sul server e workstation membri allo stesso modo come account di amministratore basati su dominio. Pertanto, è necessario aggiungere in genere l'account Administrator per ogni dominio nella foresta e l'account di amministratore per il computer locale per queste impostazioni diritti utente. La schermata seguente mostra un esempio di configurazione di questi diritti utente per bloccare l'account amministratore locale e account di amministratore del dominio di eseguire accessi che non sono necessari per questi account.  

>   
> ![modelli di amministrazione privilegi minimi](media/Implementing-Least-Privilege-Administrative-Models/SAD_20.gif)  
  
##### <a name="configuring-gpos-to-restrict-administrator-accounts-on-domain-controllers"></a>Configurazione di oggetti Criteri di gruppo per limitare l'account di amministratore nei controller di dominio  
In ogni dominio nella foresta, il criterio controller di dominio predefinito o un criterio collegato all'unità Organizzativa controller di dominio deve essere modificato per aggiungere i seguenti diritti utente in account di amministratore di ciascun dominio **Configurazione computer\Criteri\Impostazioni di Windows\Impostazioni protezione\Criteri Settings\nome diritti**:  
  
-   Nega accesso al computer dalla rete  
  
-   Nega accesso come processo batch  
  
-   Nega accesso come servizio  
  
-   Nega accesso tramite Servizi Desktop remoto  
  
> [!NOTE]  
> Queste impostazioni garantirà l'account Administrator locale non può essere utilizzato per connettersi a un controller di dominio, anche se l'account, se abilitata, può accedere localmente ai controller di dominio. Poiché solo questo account deve essere abilitato e utilizzato negli scenari di ripristino di emergenza, si prevede che accesso fisico per almeno un controller di dominio sarà disponibile o che è possibile utilizzare altri account con autorizzazioni per accedere in remoto i controller di dominio.  
  
##### <a name="configure-auditing-of-built-in-administrator-accounts"></a>Configurare il controllo dell'account amministratore predefinito  
Dopo avere protetto account amministratore di ciascun dominio e disattivato, è necessario configurare il controllo per monitorare le modifiche all'account. Se l'account è abilitato, viene reimpostata la password o vengono apportate altre modifiche all'account, è necessario inviare avvisi per gli utenti o team responsabile dell'amministrazione dei servizi di dominio Active Directory, oltre ai team di risposta agli eventi imprevisti nell'organizzazione.  
  
### <a name="securing-administrators-domain-admins-and-enterprise-admins-groups"></a>La protezione Administrators, Domain Admins e i gruppi Enterprise Admins  
  
#### <a name="securing-enterprise-admin-groups"></a>Protezione dei gruppi di amministratore dell'organizzazione  
Il gruppo Enterprise Admins, che si trova nel dominio radice della foresta, deve contenere alcun utente su base giornaliera, con la possibile eccezione dell'account amministratore locale del dominio, a condizione che si è protetto come descritto in precedenza e in [appendice d: protezione predefiniti Administrator account in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
Quando è necessario accedere EA, gli utenti con account richiedono autorizzazioni e diritti EA devono essere inseriti temporaneamente nel gruppo Enterprise Admins. Anche se gli utenti utilizzino gli account con privilegi elevati, le attività devono essere controllate e preferibilmente eseguite con un utente che esegue le modifiche e un altro utente osservare le modifiche per ridurre al minimo la probabilità di uso improprio accidentale o errori di configurazione. Quando le attività sono state completate, gli account devono essere rimosso dal gruppo EA. Questo può essere ottenuto tramite procedure manuali e documentato processi, software di gestione (PIM/PAM) identità/accesso privilegiato terze parti o una combinazione di entrambi. Linee guida per la creazione di account che può essere utilizzata per controllare l'appartenenza di gruppi con privilegi in Active Directory sono incluse [interessante degli account per il furto di credenziali](../../../ad-ds/plan/security-best-practices/Attractive-Accounts-for-Credential-Theft.md) e vengono fornite istruzioni dettagliate [appendice i: creazione di account di gestione per gli account protetti e gruppi in Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
Enterprise Admins sono, per impostazione predefinita, i membri del gruppo Administrators predefinito in ogni dominio nella foresta. La rimozione del gruppo Enterprise Admins dal gruppo Administrators in ogni dominio è una modifica non appropriata perché in caso di uno scenario, ripristino di emergenza foresta diritti EA sarà probabilmente necessari. Se il gruppo Enterprise Admins è stato rimosso dal gruppo Administrators in una foresta, deve essere aggiunto al gruppo Administrators in ogni dominio e devono essere implementati i seguenti controlli aggiuntivi:  
  
-   Come descritto in precedenza, il gruppo Enterprise Admins non deve contenere alcun utente su base giornaliera, con la possibile eccezione dell'account di amministratore del dominio radice della foresta, che deve essere protetto come descritto in [appendice d: protezione predefiniti Administrator account in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
-   Oggetti Criteri di gruppo collegati alle unità organizzative che contiene i server membri e workstation in ogni dominio, il gruppo EA deve essere aggiunte ai diritti utente seguenti:  
  
    -   Nega accesso al computer dalla rete  
  
    -   Nega accesso come processo batch  
  
    -   Nega accesso come servizio  
  
    -   Nega accesso locale  
  
    -   Nega accesso tramite Servizi Desktop remoto.  
  
Membri del gruppo EA sarà in grado di accedere alle workstation e server membri. Se jump server vengono utilizzati per amministrare i controller di dominio e Active Directory, verificare che jump server si trovano in un'unità Organizzativa a cui non sono collegati i GPO restrittivi.  
  
-   Il controllo deve essere configurato per l'invio di avvisi se vengono apportate modifiche per le proprietà o l'appartenenza al gruppo EA. Questi avvisi devono essere inviati, come minimo, a utenti o team responsabile della risposta incidente e amministrazione di Active Directory. È inoltre necessario definire processi e procedure per il popolamento temporaneamente il gruppo EA, incluse le procedure di notifica quando viene eseguito legittimo popolamento del gruppo.  
  
#### <a name="securing-domain-admins-groups"></a>Protezione dei gruppi di amministratori di dominio  
Come avviene con il gruppo Enterprise Admins, l'appartenenza ai gruppi Domain Admins deve solo in scenari di compilazione o di ripristino di emergenza. Non dovrebbe esserci alcun account di utenti di gruppo DA fatta eccezione per l'account amministratore locale per il dominio, se è stato protetto come descritto in [appendice d: protezione predefiniti Administrator account in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
Quando viene richiesto DA accesso, gli account che richiedono questo livello di accesso devono essere inseriti temporaneamente nel gruppo di DA per il dominio in questione. Anche se gli utenti utilizzino gli account con privilegi elevati, le attività devono essere controllate e preferibilmente eseguite con un utente che esegue le modifiche e un altro utente osservare le modifiche per ridurre al minimo la probabilità di uso improprio accidentale o errori di configurazione. Quando le attività sono state completate, gli account devono essere rimosso dal gruppo Domain Admins. Questo può essere ottenuto tramite procedure manuali e documentato processi, tramite il software di gestione (PIM/PAM) identità/accesso privilegiato terze parti, o una combinazione di entrambi. Linee guida per la creazione di account che può essere utilizzata per controllare l'appartenenza di gruppi con privilegi in Active Directory sono incluse [appendice i: creazione di account di gestione per gli account protetti e gruppi in Active Directory](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
Domain Admins sono, per impostazione predefinita, i membri del gruppo Administrators locale in tutti i server membri e workstation nei rispettivi domini. La nidificazione predefinito non deve essere modificata perché ha effetto opzioni di ripristino di emergenza e supporto. Se gruppi Domain Admins sono stati rimossi dal gruppo Administrators locale nei server membri, essi deve essere aggiunto al gruppo Administrators in ogni server membro e workstation nel dominio tramite impostazioni di gruppo con restrizioni in oggetti Criteri di gruppo collegati. I seguenti controlli generali, descritte in dettaglio in [appendice f: protezione Domain Admins gruppi in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md) deve inoltre essere implementata.  
  
Per il gruppo Domain Admins in ogni dominio nella foresta:  
  
1.  Rimuovere tutti i membri dal gruppo DA, con la possibile eccezione dell'account amministratore predefinito per il dominio, purché sia stata impostata come descritto in [appendice d: protezione predefiniti Administrator account in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
2.  Oggetti Criteri di gruppo collegati alle unità organizzative che contiene i server membri e workstation in ogni dominio, il gruppo DA deve essere aggiunte ai diritti utente seguenti:  
  
    -   Nega accesso al computer dalla rete  
  
    -   Nega accesso come processo batch  
  
    -   Nega accesso come servizio  
  
    -   Nega accesso locale  
  
    -   Nega accesso tramite Servizi Desktop remoto  
  
    Membri del gruppo DA sarà in grado di accedere alle workstation e server membri. Se jump server vengono utilizzati per amministrare i controller di dominio e Active Directory, verificare che jump server si trovano in un'unità Organizzativa a cui non sono collegati i GPO restrittivi.  
  
3.  Il controllo deve essere configurato per l'invio di avvisi se vengono apportate modifiche per le proprietà o l'appartenenza al gruppo DA. Questi avvisi devono essere inviati, come minimo, a utenti o team responsabile della risposta incidente e amministrazione di dominio Active Directory. È inoltre necessario definire processi e procedure per il popolamento temporaneamente il gruppo DA, incluse le procedure di notifica quando viene eseguito legittimo popolamento del gruppo.  
  
#### <a name="securing-administrators-groups-in-active-directory"></a>Protezione dei gruppi Administrators in Active Directory  
Come avviene con i gruppi di EA e DA, l'appartenenza al gruppo Administrators (BA) dovrebbe essere necessaria solo in scenari di compilazione o di ripristino di emergenza. Non dovrebbe esserci alcun account di utenti del gruppo Administrators, fatta eccezione per l'account amministratore locale per il dominio, se è stato protetto come descritto in [appendice d: protezione predefiniti Administrator account in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
Quando è necessario agli amministratori l'accesso, gli account che richiedono questo livello di accesso devono essere inseriti temporaneamente nel gruppo di amministratori per il dominio in questione. Anche se gli utenti utilizzino gli account con privilegi elevati, le attività devono essere controllate e, preferibilmente, eseguite con un utente esegue le modifiche e un altro utente osservare le modifiche per ridurre al minimo la probabilità di uso improprio accidentale o errori di configurazione. Quando le attività sono state completate, gli account devono essere rimosso immediatamente dal gruppo Administrators. Questo può essere ottenuto tramite procedure manuali e documentato processi, tramite il software di gestione (PIM/PAM) identità/accesso privilegiato terze parti, o una combinazione di entrambi.  
  
Gli amministratori sono, per impostazione predefinita, i proprietari della maggior parte degli oggetti di dominio Active Directory nei rispettivi domini. L'appartenenza a questo gruppo potrebbe essere necessario negli scenari di ripristino di emergenza e compilazione in cui è necessaria la proprietà o la possibilità di assumere la proprietà di oggetti. Inoltre, DAs ed EAs ereditare un numero di propri diritti e autorizzazioni per l'appartenenza predefinita del gruppo Administrators. Nidificazione dei gruppi per gruppi con privilegi in Active Directory non devono essere modificati per impostazione predefinita e gruppo di amministratori di ciascun dominio deve essere protetto come descritto in [appendice g: protezione dei gruppi Administrators in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)e in istruzioni generali riportate di seguito.  
  
1.  Rimuovere tutti i membri dal gruppo Administrators, con la possibile eccezione dell'account amministratore locale per il dominio, purché sia stata impostata come descritto in [appendice d: protezione predefiniti Administrator account in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
2.  I membri del gruppo Administrators del dominio mai necessario accedere al server membri o workstation. In uno o più oggetti Criteri di gruppo collegato a workstation e server membro unità organizzative in ogni dominio, il gruppo di amministratori deve essere aggiunte ai diritti utente seguenti:  
  
    -   Nega accesso al computer dalla rete  
  
    -   Nega accesso come processo batch,  
  
    -   Nega accesso come servizio  
  
    -   Ciò impedirà i membri del gruppo Administrators viene utilizzato per accedere o connettersi al server membri o workstation (a meno che più controlli vengono innanzitutto violati), in cui le proprie credenziali potessero essere memorizzato nella cache e compromessi. Un account con privilegi mai essere utilizzato per accedere a un sistema con meno privilegi e applicazione di questi controlli offrono protezione contro un numero di attacchi.  
  
3.  I controller di dominio, unità Organizzativa in ogni dominio nella foresta, il gruppo di amministratori deve essere concesso l'utente seguente at diritti (se non si dispone già di questi diritti), in modo che i membri del gruppo Administrators per eseguire le funzioni necessarie per uno scenario di ripristino di emergenza a livello di foresta:  
  
    -   Accesso al computer dalla rete  
  
    -   Consenti accesso locale  
  
    -   Consenti accesso tramite Servizi Desktop remoto  
  
4.  Il controllo deve essere configurato per l'invio di avvisi se vengono apportate modifiche per le proprietà o l'appartenenza del gruppo Administrators. Questi avvisi devono essere inviati, come minimo, ai membri del team responsabile dell'amministrazione di Active Directory. Gli avvisi devono essere inviati anche ai membri del team di protezione e procedure devono essere definite per modificare l'appartenenza del gruppo Administrators. In particolare, questi processi dovranno includere una procedura mediante il quale il team di protezione è una notifica quando il gruppo di amministratori sta per essere modificato in modo che quando vengono inviati avvisi, sono previsti e non viene generato un avviso. Inoltre, devono essere implementati per notificare il team di protezione quando l'utilizzo del gruppo Administrators è stata completata e gli account utilizzati sono stati rimossi dal gruppo di processi.  
  
> [!NOTE]  
> Quando si implementano restrizioni nel gruppo di amministratori in oggetti Criteri di gruppo, Windows applica le impostazioni per i membri del gruppo Administrators locale del computer oltre a gruppo di amministratori del dominio. Pertanto, è necessario prestare attenzione quando si implementa restrizioni nel gruppo di amministratori. Sebbene divieto di accessi alla rete, batch e del servizio per i membri del gruppo Administrators è consigliabile nel punto in cui è possibile implementare, non si limitano gli accessi locali o gli accessi tramite Servizi Desktop remoto. Il blocco di questi tipi di accesso può bloccare l'amministrazione legittimo di un computer da membri del gruppo Administrators locale. La schermata seguente mostra le impostazioni di configurazione che bloccano l'utilizzo improprio incorporati locale e amministratore account di dominio, oltre a uso improprio di gruppi di amministratori locale o di dominio predefiniti. Si noti che il **Nega accesso tramite Servizi Desktop remoto** diritto utente non include il gruppo Administrators, perché incluso in questa impostazione blocca anche questi accessi per gli account che sono membri del gruppo Administrators del computer locale. Se servizi nel computer sono configurati per l'esecuzione nel contesto di uno qualsiasi dei gruppi con privilegi descritti in questa sezione, implementare queste impostazioni può causare servizi e delle applicazioni. Pertanto, come con tutti i consigli forniti in questa sezione, è necessario testarne le impostazioni per l'applicabilità nell'ambiente in uso.  

>   
> ![modelli di amministrazione privilegi minimi](media/Implementing-Least-Privilege-Administrative-Models/SAD_3.gif)  
  
### <a name="role-based-access-controls-rbac-for-active-directory"></a>Controlli di accesso basate sui ruoli (RBAC) per Active Directory  
In generale, i controlli di accesso basate sui ruoli (RBAC) sono un meccanismo per raggruppare utenti e fornire l'accesso alle risorse in base alle regole di business. Nel caso di Active Directory, l'implementazione RBAC per Active Directory è il processo di creazione di ruoli a cui sono delegate diritti e autorizzazioni per consentire ai membri del ruolo di eseguire attività amministrative quotidiane senza concedere loro privilegi eccessivi. RBAC per Active Directory può essere progettata e implementata tramite gli strumenti nativi e interfacce, sfruttando software che può già proprietari, tramite l'acquisto di prodotti di terze parti o qualsiasi combinazione di questi approcci. In questa sezione vengono fornite istruzioni dettagliate per implementare RBAC per Active Directory, ma viene invece fattori da considerare nella scelta di un approccio all'implementazione RBAC nelle installazioni di dominio Active Directory.  
  
#### <a name="native-approaches-to-rbac-for-active-directory"></a>Approcci nativi per RBAC per Active Directory  
Nell'implementazione RBAC più semplice, è possibile implementare i ruoli come gruppi di dominio Active Directory e delegare i diritti e autorizzazioni per i gruppi che consentono loro di eseguire l'amministrazione giornaliera all'interno dell'ambito designato del ruolo.  
  
In alcuni casi, è possono utilizzare gruppi di sicurezza esistente in Active Directory per concedere diritti e le autorizzazioni appropriate per una funzione del processo. Ad esempio, se dipendenti specifici nell'organizzazione IT responsabili della gestione e manutenzione di record e le zone DNS, il delegare le responsabilità può essere semplice come la creazione di un account per ogni amministratore DNS e aggiungerlo al gruppo DNS Admins in Active Directory. Gruppo DNS Admins, a differenza di gruppi con privilegi più elevati, dispone di alcuni diritti potente tra Active Directory, anche se i membri di questo gruppo sono state delegate le autorizzazioni che consentono loro di amministrazione DNS.  
  
In altri casi, potrebbe essere necessario creare gruppi di sicurezza e delegare i diritti e autorizzazioni per gli oggetti di Active Directory, oggetti del file system e gli oggetti del Registro di sistema per consentire ai membri dei gruppi per eseguire attività amministrative designata. Ad esempio, se gli operatori di Help Desk sono responsabili per la reimpostazione delle password dimenticate, assistere gli utenti con problemi di connettività e risoluzione dei problemi di impostazioni dell'applicazione, si potrebbe essere necessario combinare le impostazioni di delega per gli oggetti utente in Active Directory con privilegi che consentono agli utenti dell'Help Desk di connettersi in remoto ai computer degli utenti per visualizzare o modificare le impostazioni di configurazione degli utenti. Per definire ogni ruolo, è necessario identificare:  
  
1.  I membri del ruolo quali attività eseguono su base giornaliera e quali attività vengono eseguite meno frequentemente.  
  
2.  In quali sistemi e delle applicazioni che i membri di un ruolo devono essere concesso diritti e autorizzazioni.  
  
3.  Gli utenti che devono essere concesse appartenenza in un ruolo.  
  
4.  Come verrà eseguita la gestione di appartenenze ai ruoli.  
  
In molti ambienti, la creazione manuale di controlli di accesso basata su ruoli per l'amministrazione di un ambiente Active Directory può essere difficile da implementare e gestire. Se è stato definito chiaramente ruoli e responsabilità per l'amministrazione dell'infrastruttura IT, si desidera utilizzare altri strumenti che facilitano la creazione di una distribuzione RBAC nativa gestibile. Se, ad esempio, Forefront Identity Manager (FIM) è in uso nell'ambiente in uso, è possibile utilizzare FIM per automatizzare la creazione e popolamento dei ruoli amministrativi, che possono facilitare l'amministrazione in corso. Se si utilizza System Center Configuration Manager (SCCM) e System Center Operations Manager (SCOM), è possibile utilizzare i ruoli specifici dell'applicazione per delegare la gestione e funzioni di monitoraggio e inoltre applicare la coerenza della configurazione e controllo sui diversi sistemi nel dominio. Se è stato implementato un'infrastruttura a chiave pubblica (PKI), è possibile emettere e richiedere smart card per il personale IT responsabile dell'amministrazione dell'ambiente. Con gestione delle credenziali di FIM (FIM CM), è anche possibile combinare la gestione dei ruoli e le credenziali per il personale amministrativo.  
  
In altri casi, potrebbe essere preferibile per un'organizzazione considerare la distribuzione software di terze parti RBAC che fornisce la funzionalità "out-of-box". Soluzioni (COTS) commerciali, disponibili sul mercato per RBAC per Active Directory, Windows e le directory non Windows e sistemi operativi sono offerte da un numero di fornitori. Quando si sceglie tra soluzioni native e i prodotti di terze parti, è necessario considerare i fattori seguenti:  
  
1.  Budget: Da investire nello sviluppo di RBAC tramite software e gli strumenti che può già proprietari, è possibile ridurre i costi software coinvolti nella distribuzione di una soluzione. Tuttavia, a meno che non si dispone di personale di esperti nella creazione e distribuzione di soluzioni RBAC native, potrebbe essere necessario coinvolgere consulenze risorse per sviluppare la soluzione. È opportuno valutare attentamente i costi previsti per una soluzione personalizzata con i costi per distribuire una soluzione "out-of-box", in particolare se il budget è limitato.  
  
2.  Composizione dell'ambiente IT: se l'ambiente è costituito principalmente sistemi Windows, o se già sfruttate Active Directory per la gestione dei sistemi non Windows e gli account, soluzioni personalizzate native possono fornire la soluzione ottimale per le proprie esigenze. Se l'infrastruttura contiene molti sistemi che non eseguono Windows e non sono gestiti da Active Directory, è necessario prendere in considerazione le opzioni per la gestione dei sistemi non Windows separatamente dall'ambiente di Active Directory.  
  
3.  Modello con privilegi nella soluzione: se un prodotto si basa sul posizionamento del relativo account del servizio in gruppi con privilegi elevati in Active Directory e le opzioni di offerta che non richiedono privilegi eccessivi essere concesso per il software RBAC, Active Directory non è stata ridotta realmente surfaceyou attacco solo cambiata la composizione dei gruppi con più privilegi nella directory. A meno che un fornitore dell'applicazione può fornire controlli per gli account del servizio che riducono al minimo la probabilità che l'account viene compromesso e utilizzato da utenti malintenzionati, si desidera considerare altre opzioni.  

  
### <a name="privileged-identity-management"></a>Gestione delle identità con privilegi  
Con privilegi di gestione delle identità (PIM), talvolta definito come account con privilegi (PAM) di gestione o la gestione delle credenziali con privilegi (PCM) è la progettazione, costruzione, e implementazione degli approcci alla gestione dei privilegi account nell'infrastruttura. In generale, PIM offre meccanismi che gli account vengono concessi diritti temporanei e autorizzazioni necessarie per eseguire una compilazione o interruzione correggere funzioni, anziché lasciare privilegi in modo permanente allegati agli account. Se la funzionalità PIM viene creata manualmente o è implementata tramite la distribuzione del software di terze parti uno o più delle funzionalità seguenti potrebbe essere disponibile:  
  

-   Credenziali "insiemi di credenziali," in cui le password degli account privilegiati sono "estratti" assegnate una password iniziale, quindi "archiviate" quando le attività sono state completate, momento in cui vengono nuovamente reimpostare le password degli account.  
  
-   Scadenza restrizioni relative all'utilizzo delle credenziali con privilegi  
  
-   Credenziali di un utilizzo  
  
-   Generati dal flusso di lavoro concessione dei privilegi di monitoraggio e reporting delle attività eseguite e la rimozione automatica dei privilegi quando le attività vengono completate o assegnate tempo scaduto  
  
-   Sostituzione delle credenziali a livello di codice, ad esempio nomi utente e password negli script con application programming interface (API) che consentono di credenziali da recuperare da insiemi di credenziali in base alle esigenze  
  
-   Gestione automatica delle credenziali di account di servizio  
  
### <a name="creating-unprivileged-accounts-to-manage-privileged-accounts"></a>Creazione account senza privilegi per gestire gli account con privilegi  
Una delle sfide da Gestione degli account con privilegi è che, per impostazione predefinita, gli account che possono gestire gli account con privilegi e protetti e gruppi hanno privilegi e gli account protetti. Se si implementano soluzioni RBAC e PIM appropriate per l'installazione di Active Directory, le soluzioni possono includere approcci che consentono di depopulate in modo efficace l'appartenenza di gruppi con più privilegi nella directory, popolare i gruppi solo temporaneamente e, se necessari.  
  
Se si implementano native RBAC e PIM, tuttavia, è consigliabile creare gli account che dispongono di alcun privilegio e con la funzione di popolamento e depopulating con privilegi sola i gruppi in Active Directory quando necessario. [Appendice i: creazione di gestione degli account per gli account protetti e gruppi in Active Directory](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md) vengono fornite istruzioni dettagliate che è possibile utilizzare per creare gli account a questo scopo.  
  
### <a name="implementing-robust-authentication-controls"></a>Implementazione dei controlli di autenticazione affidabile  
*Legge numero sei: non vi è qualcuno che vi tentino di indovinare le password.* - [10 Immutable Laws of Security Administration](https://technet.microsoft.com/library/cc722488.aspx)  
  
Pass-the-hash e altri attacchi contro il furto di credenziali non sono specifici di sistemi operativi Windows, non risultano di nuovo. Il primo attacco pass-the-hash è stato creato nel 1997. In passato, tuttavia, questi attacchi necessari strumenti personalizzati, sono stati hit-or-miss nella loro riuscita e autori di attacchi a un livello elevato di competenze necessarie. Un aumento esponenziale del numero e il successo di attacchi contro il furto di credenziali negli ultimi anni ha comportato l'introduzione di strumenti disponibili gratuitamente, facile da usare che consente di estrarre in modo nativo le credenziali. Tuttavia, gli attacchi contro il furto di credenziali sono in alcun modo i meccanismi soli mediante il quale le credenziali sono destinate e compromessi.  
  
Anche se è necessario implementare i controlli per garantire la protezione contro gli attacchi contro il furto di credenziali, è anche deve identificare gli account nell'ambiente che più probabilmente essere indirizzati da utenti malintenzionati e implementare i controlli di autenticazione affidabile per questi account. Se l'account con privilegi più utilizza l'autenticazione a fattore singolo, ad esempio nomi utente e password (entrambe sono "cosa conosce," che è un fattore di autenticazione), tali account sono protetti in modo debole. Tutto quello che necessita di un utente malintenzionato è conoscenza del nome utente e conoscere la password associata all'account e attacchi pass-the-hash non sono requiredthe attacco può eseguire l'autenticazione utente per i sistemi che accetta le credenziali di un fattore.  
  
Sebbene l'implementazione di autenticazione a più fattori non protegge contro gli attacchi pass-the-hash, l'implementazione dell'autenticazione a più fattori in combinazione con sistemi protetti possono. Ulteriori informazioni sull'implementazione di sistemi protetti sono disponibili nel [implementazione host amministrativi Secure](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md), e nelle sezioni seguenti vengono descritte le opzioni di autenticazione.  
  
#### <a name="general-authentication-controls"></a>Controlli di autenticazione generale  
Se non è già stata implementata l'autenticazione a più fattori, ad esempio smart card, eseguire l'operazione. Le smart card implementare applicata dall'hardware di protezione delle chiavi private in una coppia di chiavi pubblica / privata, impedendo chiave privata dell'utente da cui si accede o utilizzato a meno che l'utente presenta il PIN corretto, passcode o identificatore biometrico alla smart card. Anche se un utente PIN o passcode viene intercettato da un keystroke logger in un computer, per un utente malintenzionato di riutilizzare il PIN o passcode, la scheda deve anche essere fisicamente presente.  
  
Nei casi in cui le password lunghe e complesse sono rivelati difficili da implementare a causa di resistenza utente, le smart card forniscono un meccanismo mediante il quale gli utenti possono implementare PIN relativamente semplice o dei codici di accesso senza le credenziali siano soggette ad attacchi di forza bruta o con arcobaleno tabella attacchi. PIN della smart card non vengono archiviati in Active Directory o in database SAM locali, anche se gli hash delle credenziali possono ancora essere archiviati nella memoria LSASS protetti nei computer in cui sono state utilizzate smart card per l'autenticazione.  
  
#### <a name="additional-controls-for-vip-accounts"></a>Controlli aggiuntivi per gli account VIP  
Un altro vantaggio dell'implementazione di smart card o altri meccanismi di autenticazione basata su certificati è la possibilità di sfruttare verifica del meccanismo di autenticazione per proteggere dati riservati che sono accessibili agli utenti di VIP. Verifica del meccanismo di autenticazione sono disponibile nei domini in cui è impostato il livello di funzionalità a Windows Server 2012 o Windows Server 2008 R2. Quando è abilitato, verifica del meccanismo di autenticazione aggiunge un'appartenenza a gruppo globale designato amministratore al token Kerberos di un utente quando vengono autenticate le credenziali dell'utente durante l'accesso utilizzando un metodo di accesso basata su certificati.  
  
Questo rende possibile per gli amministratori di risorse controllare l'accesso alle risorse, ad esempio file, cartelle e stampanti, in base se l'utente accede utilizzando un metodo di accesso basato sui certificati, oltre al tipo di certificato utilizzato. Ad esempio, quando un utente accede utilizzando una smart card, l'accesso dell'utente alle risorse di rete può essere specificato come diverse dal quale l'accesso è quando l'utente non utilizza una smart card (ovvero, quando l'utente accede immettendo il nome utente e password). Per ulteriori informazioni sulla verifica del meccanismo di autenticazione, vedere il [verifica del meccanismo di autenticazione di dominio Active Directory nella Guida dettagliata di Windows Server 2008 R2](https://technet.microsoft.com/library/dd378897.aspx).  
  
#### <a name="configuring-privileged-account-authentication"></a>Configurazione dell'autenticazione di Account con privilegi  
In Active Directory per tutti gli account amministrativi, abilitare il **Richiedi smart card per l'accesso interattivo** attributo e controllo delle modifiche apportate a (almeno), uno degli attributi nel **Account** scheda per l'account (ad esempio, cn, nome, sAMAccountName, userPrincipalName e userAccountControl) gli oggetti utente con privilegi amministrativi.  
  
Sebbene l'impostazione di **Richiedi smart card per l'accesso interattivo** account Reimposta la password dell'account su un valore casuale a 120 caratteri e richiede smart card per gli accessi interattivi, l'attributo può ancora essere sovrascritto da utenti con autorizzazioni che consentono loro di modificare le password degli account, nonché gli account possono quindi essere utilizzati per stabilire gli accessi interattivi con solo nome utente e password.  
  
In altri casi, a seconda della configurazione di account nelle impostazioni di Active Directory e il certificato in Servizi certificati Active Directory (AD CS) o un PKI di terze parti, il nome dell'entità utente (UPN) gli attributi per amministrazione o account VIP può essere oggetto di un tipo specifico di attacco, come descritto di seguito.  
  
##### <a name="upn-hijacking-for-certificate-spoofing"></a>UPN Hijack per lo spoofing degli indirizzi di certificato  
Sebbene una discussione approfondita di attacchi di infrastrutture a chiave pubblica (PKI) esula dall'ambito di questo documento, gli attacchi contro PKI pubbliche e private sono aumentati in modo esponenziale dal 2008. Violazioni della PKI pubbliche ampiamente pubblicizzati, ma gli attacchi contro PKI interna di un'organizzazione sono probabilmente ancora più elevati. Un attacco sfrutta Active Directory e i certificati per consentire un attacco di spoofing le credenziali di altri account in modo che possono essere difficili da rilevare.  
  
Quando viene visualizzato un certificato per l'autenticazione a un sistema di dominio, il contenuto dell'oggetto o l'attributo di nome alternativo soggetto (SAN) del certificato viene utilizzato per eseguire il mapping del certificato a un oggetto utente in Active Directory. A seconda del tipo di certificato e come viene creato, l'attributo dell'oggetto in un certificato contiene in genere il nome comune dell'utente (CN), come illustrato nella schermata seguente.  
  
![modelli di amministrazione privilegi minimi](media/Implementing-Least-Privilege-Administrative-Models/SAD_4.gif)  
  
Per impostazione predefinita, Active Directory crea CN un utente concatenando il nome dell'account prima + "" + cognome. Tuttavia, componenti CN degli oggetti utente in Active Directory non necessari o garantiti l'univocità e lo spostamento di un account utente in una posizione diversa nella directory viene modificato il nome dell'account distinto (DN), che è il percorso completo per l'oggetto nella directory, come illustrato nel riquadro inferiore della schermata precedente.  
  
Poiché i nomi di soggetto certificato non sono necessariamente essere statici o univoco, il contenuto del nome alternativo soggetto viene spesso utilizzato per individuare l'oggetto utente in Active Directory. L'attributo SAN per i certificati emessi per gli utenti da autorità di certificazione (CA integrata di Active Directory) contiene in genere l'indirizzo di posta elettronica o UPN dell'utente. Poiché l'UPN è garantito l'univocità in una foresta di Active Directory, individuare un oggetto utente da UPN è comunemente eseguite nell'ambito di autenticazione, con o senza certificati coinvolti nel processo di autenticazione.  
  
L'uso di nomi UPN in attributi SAN dei certificati di autenticazione può essere sfruttata da utenti malintenzionati per ottenere i certificati fraudolenti. Se un utente malintenzionato ha compromesso un account che è in grado di leggere e scrivere i nomi UPN degli oggetti utente, l'attacco è implementato come segue:  
  
L'attributo UPN in un oggetto utente (ad esempio, un utente VIP) viene modificato temporaneamente in un valore diverso. L'attributo del nome account SAM e CN può essere modificato anche in questo momento, anche se non si tratta in genere necessario per i motivi descritti in precedenza.  
  
Quando l'attributo UPN all'account di destinazione è stato modificato, al valore originariamente assegnata all'account di destinazione viene modificato un account utente abilitato non aggiornati o attributo UPN dell'account di un utente appena creato. Gli account utente abilitato non aggiornati sono account che non sono connessi per lunghi periodi di tempo, ma non sono stati disabilitati. Esse sono destinate dai pirati informatici che intendono per "nascondere all'aperto" per i motivi seguenti:  
  
1.  Poiché l'account è abilitato, ma non è stato utilizzato di recente, utilizzando l'account è improbabile per attivare gli avvisi il modo in cui potrebbe abilitare un account utente disabilitato.  
  
2.  Utilizzo di un account esistente non richiede la creazione di un nuovo account utente che potrebbero essere notati dal personale amministrativo.  
  
3.  Gli account utente non aggiornati che sono ancora abilitati sono in genere membri di vari gruppi di sicurezza e vengono concesso l'accesso alle risorse di rete, semplificando l'accesso e "la fusione" a una popolazione di utenti esistente.  
  
Viene utilizzato l'account utente in cui la destinazione UPN è stata configurata per richiedere uno o più certificati da Servizi certificati Active Directory.  
  
Quando una volta ottenuti i certificati per account dell'utente malintenzionato, l'UPN nell'account "nuovi" e l'account di destinazione viene restituito i valori originali.  
  
L'autore dell'attacco dispone ora di uno o più certificati che possono essere presentati per l'autenticazione per risorse e applicazioni come se l'utente è l'indirizzo VIP il cui account è stato temporaneamente modificata. Anche se una trattazione completa di tutti i modi in cui i certificati e infrastruttura a chiave pubblica può essere indirizzati da utenti malintenzionati esula dall'ambito di questo documento, questo meccanismo di attacco è fornito per illustrare il motivo per cui è necessario monitorare con privilegi e VIP account di dominio Active Directory per le modifiche, in particolare per le modifiche apportate a uno degli attributi nel **Account** scheda per l'account (ad esempio cn, nome, sAMAccountName, userPrincipalName e userAccountControl). Oltre a monitorare gli account, è consigliabile limitare chi può modificare l'account come piccole un set di utenti con privilegi amministrativi possibili. Analogamente, gli account degli utenti amministrativi devono essere protetti e verificare la presenza di modifiche non autorizzate.  
  


