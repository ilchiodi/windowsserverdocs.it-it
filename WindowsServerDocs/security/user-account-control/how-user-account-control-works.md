---
title: Come funziona controllo dell'Account utente
description: Protezione di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tpm
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da83ddb2-6182-417c-aa8e-0b47b2e17d13
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 7f5ad234959ebfe0687b8bc10f9e43f2092252f2
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="how-user-account-control-works"></a>Come funziona controllo dell'Account utente

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Controllo Account utente (UAC) consente di impedire ai programmi dannosi (denominati anche malware) di danneggiare un computer e permette alle organizzazioni di distribuire un desktop gestito in modo migliore. Con controllo dell'account utente, applicazioni e attività sempre eseguire nel contesto di sicurezza di un account senza privilegi di amministratore, a meno che un amministratore autorizzi specificamente l'accesso amministrativo al sistema. Controllo dell'account utente può bloccare l'installazione automatica di applicazioni non autorizzate e impedire modifiche involontarie alle impostazioni del sistema.

## <a name="uac-process-and-interactions"></a>Processo di controllo dell'account utente e interazioni
Ogni applicazione che richiede il token di accesso amministrativo deve richiedere il consenso dell'amministratore. L'unica eccezione è costituita dalla relazione esistente tra padre e figlio processi. Il processo figlio eredita il token di accesso utente dal processo padre. I processi padre e figlio, tuttavia, devono avere lo stesso livello di integrità. Windows Server 2012 protegge i processi contrassegnando i livelli di integrità. Livelli di integrità sono le misurazioni di trust. Un'applicazione di integrità "elevata" è quello che esegue le attività che modificano i dati di sistema, ad esempio un disco di partizionamento di applicazione, mentre un'applicazione di integrità "basso" è quello che esegue le attività che potrebbero compromettere il sistema operativo, ad esempio un Web browser. Le applicazioni con livelli di integrità bassi non possono modificare i dati di applicazioni con livelli di integrità superiori. Quando un utente standard tenta di eseguire un'applicazione che richiede un token di accesso amministrativo, controllo dell'account utente richiede all'utente di fornire credenziali amministrative valide.

Per comprendere meglio come questo processo si verifica, è importante esaminare i dettagli del processo di accesso di Windows Server 2012.

### <a name="windows-server-2012-logon-process"></a>Processo di accesso di Windows Server 2012
Nella figura seguente illustra le differenze tra il processo di accesso per un amministratore dal processo di accesso per un utente standard.

![Figura che illustra le differenze tra il processo di accesso per un amministratore dal processo di accesso per un utente standard](../media/How-User-Account-Control-Works/UACWindowsLogonProcess.gif)

Per impostazione predefinita, gli amministratori e utenti standard di accedono alle risorse ed eseguono applicazioni nel contesto di sicurezza degli utenti standard. Quando un utente accede a un computer, il sistema crea un token di accesso per tale utente. Il token di accesso contiene informazioni sul livello di accesso concesso all'utente, inclusi gli identificatori di sicurezza specifici (SID) e privilegi di Windows.

Quando un amministratore accede, vengono creati due token di accesso distinti per l'utente: un token di accesso utente standard e un token di accesso di amministratore. Il token di accesso utente standard contiene le stesse informazioni specifiche dell'utente il token di accesso di amministratore, ma vengono rimossi i privilegi amministrativi di Windows e un SID. Il token di accesso utente standard viene utilizzato per avviare le applicazioni che non eseguono attività amministrative (applicazioni per utente standard). Il token di accesso utente standard viene quindi utilizzato per visualizzare il desktop (Explorer.exe). Explorer.exe è il processo padre da cui tutti gli altri processi avviati dall'utente ereditano il token di accesso. Di conseguenza, tutte le applicazioni eseguite come utente standard, a meno che un utente fornisce il consenso o le credenziali necessarie affinché un'applicazione per l'utilizzo di un token di accesso amministrativo completo.

Un utente che è un membro del gruppo Administrators possa accedere, esplorare il Web e leggere la posta elettronica utilizzando un token di accesso utente standard. Quando l'amministratore deve eseguire un'attività che richiede il token di accesso di amministratore, Windows Server 2012 automaticamente richiede l'approvazione dell'utente. Questa richiesta viene definita un'elevazione dei privilegi, e il relativo comportamento può essere configurato tramite lo snap-in Criteri di sicurezza locali (Secpol.msc) o criteri di gruppo.

> [!NOTE]
> Il termine "elevare" viene utilizzato per fare riferimento al processo in Windows Server 2012 che richiede all'utente il consenso o le credenziali necessarie per l'utilizzo di un token di accesso amministrativo completo.

### <a name="the-uac-user-experience"></a>L'esperienza utente controllo dell'account utente
Quando questa opzione è abilitata, l'esperienza utente per gli utenti standard è diversa da quella degli amministratori in modalità Approvazione amministratore. Il metodo consigliato e più sicuro che esegue Windows Server 2012 consiste nel rendere l'utente primario un account utente standard. Esecuzione come utente standard consente di ottimizzare la protezione di un ambiente gestito. Con il componente integrato di elevazione dei privilegi controllo dell'account utente, gli utenti standard possono svolgere facilmente un'attività amministrativa immettendo credenziali valide per un account amministratore locale. Il valore predefinito, componente integrato di elevazione dei privilegi di controllo dell'account utente per gli utenti standard è la richiesta di credenziali.

L'alternativa all'esecuzione come utente standard viene eseguito come amministratore in modalità Approvazione amministratore. Con il componente integrato di elevazione dei privilegi controllo dell'account utente, i membri del gruppo Administrators locale possono svolgere facilmente un'attività amministrativa fornendo l'approvazione. Il valore predefinito, componente integrato di elevazione dei privilegi di controllo dell'account utente per un account di amministratore in modalità Approvazione amministratore viene chiamato la richiesta di consenso. Comportamento della richiesta di elevazione dei privilegi di controllo dell'account utente può essere configurato tramite lo snap-in Criteri di sicurezza locali (Secpol.msc) o criteri di gruppo.

**Le richieste di consenso e di credenziali**

Con controllo dell'account utente abilitato, Windows Server 2012 richiede il consenso o richiesta di credenziali di un account amministratore locale valido prima di avviare un programma o attività che richiede un token di accesso amministrativo completo. Questo prompt garantisce che nessun software dannoso può essere installato in modalità invisibile all'utente.

**La richiesta di consenso**

La richiesta di consenso viene visualizzata quando un utente tenta di eseguire un'attività che richiede il token di accesso amministrativo dell'utente. Di seguito è una cattura di schermata della richiesta di consenso di controllo dell'account utente.

![Cattura di schermata della richiesta di consenso di controllo dell'account utente](../media/How-User-Account-Control-Works/UACConsentPrompt.gif)

**La richiesta di credenziali**

La richiesta di credenziali viene visualizzata quando un utente standard tenta di eseguire un'attività che richiede il token di accesso amministrativo dell'utente. Questo comportamento della richiesta predefinita utente standard può essere configurato tramite lo snap-in Criteri di sicurezza locali (Secpol.msc) o criteri di gruppo. Gli amministratori possono anche essere necessario fornire le proprie credenziali impostando il controllo dell'Account utente: comportamento della richiesta di elevazione dei privilegi per gli amministratori in modalità Approvazione amministratore impostazione del criterio valore al prompt dei comandi per le credenziali.

Lo screenshot seguente è riportato un esempio di richiesta di credenziali di controllo dell'account utente.

![Cattura di schermata che mostra un esempio di richiesta di credenziali di controllo dell'account utente](../media/How-User-Account-Control-Works/UACCredentialPrompt.gif)

**Richieste di elevazione dei privilegi di controllo dell'account utente**

Le richieste di elevazione dei privilegi di controllo dell'account utente sono a colori da specifici dell'applicazione, consentendo di identificare immediatamente di potenziali rischi di protezione dell'applicazione. Quando un'applicazione tenta di eseguire con il token di accesso completo dell'amministratore, Windows Server 2012 innanzitutto analizzato il file eseguibile per risalire all'autore. Le applicazioni vengono dapprima suddivise in tre categorie in base autore del file eseguibile: Windows Server 2012, autore verificato (con firma) e autore non verificato (senza firma). Il diagramma seguente illustra come Windows Server 2012 determina quali elevazione di colore per presentare all'utente.

L'elevazione dei privilegi colori per le richieste è come segue:

-   Sfondo rosso con icona Scudo rossa: l'applicazione è bloccata da criteri di gruppo o proviene da un autore che è stato bloccato.

-   Sfondo blu con icona Scudo blu e oro: l'applicazione è un'applicazione amministrativa di Windows Server 2012, ad esempio un elemento del Pannello di controllo.

-   Sfondo blu con icona Scudo blu: l'applicazione è firmata con Authenticode ed è considerata attendibile dal computer locale.

-   Sfondo giallo con icona Scudo gialla: l'applicazione è firmata o è firmata ma non ancora considerata attendibile dal computer locale.

**Icona Scudo di sicurezza**

Alcuni Pannello di controllo elementi, ad esempio **proprietà data e ora**, contengono una combinazione di amministratore e le operazioni di utente standard. Gli utenti standard possono visualizzare l'orologio e modificare il fuso orario, ma è necessario un token di accesso amministratore completo per modificare l'ora di sistema locale. Di seguito è un'immagine della schermata di **proprietà data e ora** elemento del Pannello di controllo.

![Schermata che mostra il * * data e ora proprietà * * del Pannello di controllo](../media/How-User-Account-Control-Works/UACShieldIcon.gif)

L'icona Scudo di sicurezza nel **Modifica data e ora** pulsante indica che il processo richiede un token di accesso amministrativo completo e visualizzerà una richiesta di elevazione dei privilegi controllo dell'account utente.

**La richiesta di elevazione dei privilegi di protezione**

Il processo di elevazione dei privilegi può essere protetto ulteriormente indirizzando la richiesta al desktop sicuro. Le richieste di consenso e di credenziali vengono visualizzate sul desktop sicuro per impostazione predefinita in Windows Server 2012. Solo i processi Windows possono accedere al desktop sicuro. Per livelli di sicurezza, è consigliabile mantenere il **controllo dell'Account utente: alla richiesta di elevazione dei privilegi, passa al desktop sicuro** impostazione dei criteri abilitata.

Quando un file eseguibile richiede l'elevazione dei privilegi, passa al desktop interattivo, denominato anche desktop utente, al desktop sicuro. Il desktop sicuro attenua il desktop dell'utente e visualizza una richiesta di elevazione dei privilegi che deve rispondere per poter continuare. Quando l'utente fa clic su Sì o No, il desktop passa al desktop dell'utente.

Malware potrebbe visualizzare un'imitazione del desktop sicuro, ma quando controllo dell'Account utente: comportamento della richiesta di elevazione dei privilegi per gli amministratori in modalità Approvazione amministratore è impostato su Richiedi consenso dell'utente, il malware non otterrà l'elevazione dei privilegi se l'utente fa clic su Sì nell'imitazione. Se l'impostazione dei criteri è impostata su Richiedi credenziali, malware che imita la richiesta di credenziali potrebbe essere in grado di raccogliere le credenziali dell'utente. Tuttavia, il malware non ottenere privilegi elevati e il sistema ha altre misure di protezione che limitano la capacità malware di prendere il controllo dell'interfaccia utente anche con una password raccolto.

Mentre malware potrebbe visualizzare un'imitazione del desktop sicuro, questo problema non può verificarsi a meno che un utente precedentemente installato il software dannoso nel computer. Poiché i processi che richiedono un token di accesso amministratore non possono installare automaticamente quando questa opzione è abilitata, l'utente deve fornire esplicitamente il consenso facendo **Sì** o fornendo le credenziali di amministratore. Il comportamento specifico di richieste di elevazione è dipendente da criteri di gruppo.

## <a name="uac-architecture"></a>Architettura di controllo dell'account utente
Il diagramma seguente illustra in dettaglio l'architettura di controllo dell'account utente.

![Diagramma che riporta in dettaglio l'architettura di controllo dell'account utente](../media/How-User-Account-Control-Works/UACArchitecture.gif)

Per comprendere meglio ogni componente, esamina la tabella seguente:

|Componente|Descrizione|
|-------|--------|
|**Utente**||
|Utente esegue l'operazione che richiede privilegi|Se l'operazione di modifica del file system o il Registro di sistema, una chiamata alla virtualizzazione. Tutte le altre operazioni chiamata a ShellExecute.|
|ShellExecute|ShellExecute chiamata a CreateProcess. ShellExecute attende l'errore ERROR_ELEVATION_REQUIRED proveniente da CreateProcess. Se riceve l'errore, ShellExecute esegue una chiamata al servizio informazioni applicazioni per tentare di eseguire l'attività richiesta al prompt dei comandi con privilegi elevati.|
|CreateProcess|Se l'applicazione richiede l'elevazione dei privilegi, CreateProcess rifiuta la chiamata con ERROR_ELEVATION_REQUIRED.|
|**Sistema**||
|Servizio informazioni applicazioni|Servizio di sistema che consente l'avvio di applicazioni che richiedono uno o più privilegi o diritti utente a eseguire, ad esempio attività amministrative locali e applicazioni che richiedono livelli di integrità superiori. A tale scopo, le informazioni sull'applicazione servizio per avviare tali applicazioni creando un nuovo processo per l'applicazione con il token di accesso completo di un utente amministrativo quando è richiesta l'elevazione dei privilegi e (a seconda dei criteri di gruppo) di consenso viene assegnato dall'utente.|
|Elevare un'installazione di ActiveX|Se ActiveX non è installato, il sistema controlla il livello di dispositivo di scorrimento del controllo dell'account utente. Se ActiveX è installato, il **controllo dell'Account utente: alla richiesta di elevazione dei privilegi, passa al desktop sicuro** viene verificata l'impostazione di criteri di gruppo.|
|Controllare il livello di dispositivo di scorrimento controllo dell'account utente|Controllo dell'account utente prevede ora quattro livelli di notifica e un dispositivo di scorrimento da utilizzare per selezionare il livello di notifica:<br /><br /><ul><li>Elevata<br /><br />    Se il dispositivo di scorrimento è impostato su **notifica sempre**, il sistema controlla se il desktop sicuro è abilitato.</li><li>Media<br /><br />    Se il dispositivo di scorrimento è impostato su **impostazione predefinita solo quando un programma tenta di eseguire modifiche nel computer**, **controllo dell'Account utente: eleva solo file eseguibili firmati e convalidati** viene verificata l'impostazione:<br /><br /><ul><li>Se l'impostazione dei criteri è abilitata, non viene applicata la convalida del percorso di certificazione dell'infrastruttura a chiave pubblica (PKI) per un determinato file eseguibile prima che ne sia consentita l'esecuzione.</li><li>Se l'impostazione dei criteri non sono abilitata (impostazione predefinita), la convalida del percorso di certificazione PKI non viene applicata prima di un determinato file eseguibile sia consentito l'esecuzione. Il **controllo dell'Account utente: alla richiesta di elevazione dei privilegi, passa al desktop sicuro** viene verificata l'impostazione di criteri di gruppo.</li></ul></li><li>Basso<br /><br />    Se il dispositivo di scorrimento è impostato su **invia una notifica solo quando un programma tenta di eseguire modifiche nel computer (non attenuare il desktop)**, una chiamata a CreateProcess.</li><li>Non notificare mai<br /><br />    Se il dispositivo di scorrimento è impostato su **mai invia una notifica quando**, verrà prompt dei comandi di controllo dell'account utente non notificare mai quando un programma tenta di installare o sta tentando di apportare qualsiasi modifica apportata nel computer. **Importante:** questa impostazione non è consigliata. Questa impostazione è la stessa impostazione di **controllo dell'Account utente: comportamento della richiesta di elevazione dei privilegi per gli amministratori in modalità Approvazione amministratore** impostazione dei criteri **Esegui con privilegi elevati senza chiedere conferma**.</li></ul>|
|Desktop sicuro attivato|Il **controllo dell'Account utente: alla richiesta di elevazione dei privilegi, passa al desktop sicuro** viene verificata l'impostazione:<br /><br />-Se il desktop sicuro è attivato, tutte le richieste di elevazione passa al desktop sicuro indipendentemente dalle impostazioni di criteri di comportamento per amministratori e utenti standard.<br />-Se il desktop sicuro non è abilitato, tutte le richieste di elevazione passa al desktop interattivo dell'utente e le impostazioni per ogni utente per gli amministratori e utenti standard vengono utilizzate.|
|CreateProcess|CreateProcess esegue una chiamata rilevamento AppCompat, Fusion e programma di installazione per valutare se l'applicazione richiede l'elevazione dei privilegi. Il file eseguibile viene quindi ispezionato per determinare il livello di esecuzione richiesto, che viene archiviato nel manifesto dell'applicazione per il file eseguibile. CreateProcess non riesce se il token di accesso non corrisponde a livello di esecuzione richiesto specificato nel manifesto e restituisce un errore (ERROR_ELEVATION_REQUIRED) a ShellExecute. |
|AppCompat|Il database AppCompat memorizza le informazioni nelle voci di correzione di compatibilità dell'applicazione per un'applicazione.|
|Fusion|Il database Fusion memorizza le informazioni provenienti dai manifesti che descrivono le applicazioni. Lo schema del manifesto viene aggiornato per aggiungere un nuovo campo livello di esecuzione richiesto.|
|Rilevamento del programma di installazione|Rilevamento del programma di installazione rileva file eseguibile di installazione, in modo da evitare di installazioni viene eseguito senza conoscenze e consenso dell'utente.|
|**Kernel**||
|Virtualizzazione|La tecnologia di virtualizzazione assicura che le applicazioni non conformi non automaticamente per l'esecuzione o non riuscire in modo che non è possibile determinare la causa. Questa funzionalità include anche la virtualizzazione di file e del Registro di sistema e la registrazione per le applicazioni che eseguono la scrittura in aree protette.|
|File system e Registro di sistema|La virtualizzazione dei file e del Registro di sistema per ogni utente reindirizza le richieste di scrittura file e Registro di sistema per ogni computer ai percorsi equivalenti per ogni utente. Le richieste di lettura vengono reindirizzate al percorso virtualizzato per ogni utente prima di tutto e il percorso per ogni computer secondo.|

Esiste una modifica nel controllo dell'account utente di Windows Server 2012 da versioni precedenti di Windows. Il nuovo dispositivo di scorrimento non sarà mai disattivare controllo dell'account utente completamente. La nuova impostazione sarà:

-   Mantenere il servizio di controllo dell'account utente in esecuzione.

-   Causa richiedere l'elevazione dei privilegi tutti avviata dagli amministratori per essere approvati automaticamente senza visualizzare un controllo dell'account utente prompt dei comandi.

-   Nega automaticamente tutte le richieste di elevazione dei privilegi per gli utenti standard.

> [!IMPORTANT]
> Per disabilitare completamente il controllo dell'account utente è necessario disabilitare il criterio **controllo dell'Account utente: Esegui tutti gli amministratori in modalità Approvazione amministratore**.

> [!WARNING]
> Applicazioni personalizzate non funziona in Windows Server 2012 quando controllo dell'account utente è disabilitato.

### <a name="virtualization"></a>Virtualizzazione
Poiché gli amministratori di sistema in ambienti aziendali tentano di proteggere i sistemi, molte applicazioni di line-of-business (LOB) sono progettate per usare solo un token di accesso utente standard. Di conseguenza, gli amministratori IT non è necessario sostituire la maggior parte delle applicazioni quando si esegue Windows Server 2012 con controllo dell'account utente abilitato.

 Windows Server 2012 include la tecnologia di virtualizzazione file e del Registro di sistema per le applicazioni che non sono conformi e che richiedono un token di accesso amministrativo per la corretta esecuzione. La virtualizzazione assicura che siano compatibili con Windows Server 2012 anche per le applicazioni che non sono conformi. Quando un'applicazione amministrativa che non è compatibile con controllo dell'account utente tenta di scrivere in una directory protetta, ad esempio programmi, controllo dell'account utente fornisce l'applicazione la propria visualizzazione virtualizzata della risorsa che sta tentando di modificare. La copia virtualizzata viene mantenuta nel profilo dell'utente. Questa strategia consente di creare una copia separata del file virtualizzato per ogni utente che esegue l'applicazione non conforme.

La maggior parte delle attività delle applicazioni funzionano correttamente con funzionalità di virtualizzazione. Sebbene la virtualizzazione consente la maggior parte delle applicazioni per l'esecuzione, si tratta di una correzione provvisoria e non una soluzione a lungo termine. Gli sviluppatori di applicazioni necessario modificare le loro applicazioni compatibili con il programma logo di Windows Server 2012 appena possibile, anziché sulla virtualizzazione di file, cartelle e del Registro di sistema.

La virtualizzazione non è disponibile nell'opzione negli scenari seguenti:

1.  La virtualizzazione non è applicabile per le applicazioni eseguite con un token di accesso amministrativo completo e con privilegi elevate.

2.  La virtualizzazione supporta solo le applicazioni a 32 bit. Applicazioni a 64 bit senza privilegi elevati ricevono semplicemente un messaggio di accesso negato quando tentano di acquisire un handle (identificatore univoco) a un oggetto Windows. Applicazioni di Windows a 64 bit native sono necessari per essere compatibile con controllo dell'account utente e di scrivere dati nei percorsi corretti.

3.  La virtualizzazione viene disattivata per un'applicazione se l'applicazione include un manifesto dell'applicazione con un attributo a livello di esecuzione richiesto.

### <a name="request-execution-levels"></a>Livelli di esecuzione richiesta
Un manifesto dell'applicazione è un file XML che descrive e identifica gli assembly side-by-side condivisi e privati che un'applicazione deve essere associato a in fase di esecuzione. In Windows Server 2012, il manifesto dell'applicazione include voci per scopi di compatibilità delle applicazioni di controllo dell'account utente. Le applicazioni amministrative che includono una voce nel manifesto dell'applicazione richiede all'utente l'autorizzazione per accedere token di accesso dell'utente. Anche se dispongono di una voce nel manifesto dell'applicazione, la maggior parte delle applicazioni amministrative possono eseguiti senza modifiche con correzioni di compatibilità delle applicazioni. Correzioni di compatibilità delle applicazioni sono voci del database che consentono alle applicazioni che non sono conformi a funzionare correttamente in Windows Server 2012.

Tutte le applicazioni conformi a controllo dell'account utente devono avere un livello di esecuzione richiesto aggiunto al manifesto dell'applicazione. Se l'applicazione richiede accesso amministrativo al sistema, contrassegnando l'applicazione con un'esecuzione richiesto livello di tipo "Richiedi amministratore" assicura che il sistema identifichi il programma come applicazione amministrativa ed esegue i passaggi di elevazione dei privilegi necessari. Di esecuzione richiesti livelli determinano i privilegi necessari per un'applicazione.

### <a name="installer-detection-technology"></a>Tecnologia di rilevamento del programma di installazione
Programmi di installazione sono applicazioni progettate per distribuire il software. La maggior parte dei programmi di installazione di scrittura directory di sistema e le chiavi del Registro di sistema. Questi percorsi protetti di sistema sono scritti in genere solo da un amministratore nella tecnologia di rilevamento del programma di installazione, il che significa che gli utenti standard non si dispongano di accesso sufficienti per installare i programmi. Windows Server 2012 il rilevamento euristico programmi di installazione e richiede le credenziali amministrative o l'approvazione da parte dell'utente amministratore per l'esecuzione con privilegi di accesso. Windows Server 2012 inoltre il rilevamento euristico gli aggiornamenti e i programmi di disinstallazione. Uno degli obiettivi di controllo dell'account utente è impedire l'esecuzione senza il consenso dell'utente di installazioni e di consenso poiché i programmi di installazione di scrittura in aree protette del file system e Registro di sistema.

Rilevamento del programma di installazione si applica solo a:

-   file eseguibili a 32 bit.

-   Applicazioni senza un attributo a livello di esecuzione richiesto.

-   Processi interattivi eseguiti come utente standard con controllo dell'account utente abilitato.

Prima della creazione di un processo a 32 bit, per determinare se si tratta di un programma di installazione vengono verificati gli attributi seguenti:

-   Il nome del file include parole chiave come "install," "setup" o "aggiornamento".

-   Campi di controllo delle versioni delle risorse contengono le parole chiave seguenti: fornitore, nome della società, nome prodotto, Descrizione File, nome del file originale, nome interno e nome esportazione.

-   Parole chiave nel manifesto side-by-side sono incorporate nel file eseguibile.

-   Parole chiave in voci StringTable specifiche collegate nel file eseguibile.

-   Attributi chiave nei dati di script della risorsa collegati nel file eseguibile.

-   Esistono sequenze di destinazione dei byte presenti nel file eseguibile.

> [!NOTE]
> Le parole chiave e sequenze di byte sono stati derivano da caratteristiche comuni osservate in varie tecnologie di programma di installazione.

> [!NOTE]
> Il controllo dell'Account utente: Rileva installazione applicazioni e richiedi elevazione deve essere abilitato per il rilevamento installer programmi di installazione. Questa impostazione è abilitata per impostazione predefinita e può essere configurata in locale tramite lo snap-in Criteri di sicurezza locali (Secpol.msc) o configurata per il dominio, unità Organizzativa o gruppi specifici da criteri di gruppo (Gpedit.msc).


