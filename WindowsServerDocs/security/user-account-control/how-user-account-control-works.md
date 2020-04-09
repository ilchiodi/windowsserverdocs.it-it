---
title: Funzionamento di Controllo dell'account utente
description: Sicurezza di Windows Server
ms.prod: windows-server
ms.technology: security-tpm
ms.topic: article
ms.assetid: da83ddb2-6182-417c-aa8e-0b47b2e17d13
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 9cc4e9aab01421a2da09b75d67edad4d2f85dc33
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857594"
---
# <a name="how-user-account-control-works"></a>Funzionamento di Controllo dell'account utente

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Controllo dell'account utente consente di impedire ai programmi dannosi (denominati anche malware) di danneggiare un computer e permette alle aziende di distribuire un desktop gestito in modo ottimale. Grazie a Controllo dell'account utente, le applicazioni e le attività vengono eseguite nel contesto di sicurezza di un account non amministrativo, a meno che un amministratore non autorizzi in modo specifico l'accesso al sistema a livello di amministratore. Controllo dell'account utente può bloccare l'installazione automatica di applicazioni non autorizzate e impedire modifiche involontarie alle impostazioni del sistema.

## <a name="uac-process-and-interactions"></a>Processo e interazioni del controllo dell'account utente
Ogni applicazione per la quale è necessario il token di accesso amministrativo deve richiedere il consenso dell'amministratore. L'unica eccezione è costituita dalla relazione esistente tra processo padre e figlio. Il processo figlio eredita il token di accesso dal processo padre. Entrambi i processi, padre e figlio, devono avere tuttavia lo stesso livello di integrità. Windows Server 2012 protegge i processi contrassegnando i relativi livelli di integrità. ovvero una misurazione di attendibilità. Un'applicazione con livello di integrità "alto" esegue attività che modificano i dati del sistema, ad esempio un'applicazione di partizionamento del disco, mentre un'applicazione con livello di integrità "basso" esegue attività che potenzialmente possono compromettere il sistema operativo, ad esempio un browser Web. Le applicazioni con livelli di integrità bassi non possono modificare i dati contenuti in applicazioni con livelli di integrità alti. Quando un utente standard tenta di eseguire un'applicazione per la quale è necessario un token di accesso amministrativo, Controllo dell'account utente richiede all'utente di fornire credenziali amministrative valide.

Per comprendere meglio il modo in cui si verifica questo processo, è importante esaminare i dettagli del processo di accesso di Windows Server 2012.

### <a name="windows-server-2012-logon-process"></a>Processo di accesso di Windows Server 2012
Nell'illustrazione seguente sono evidenziate le differenze tra il processo di accesso di un amministratore e il processo di accesso di un utente standard.

![Illustrazione che illustra come il processo di accesso per un amministratore differisce dal processo di accesso per un utente standard](../media/How-User-Account-Control-Works/UACWindowsLogonProcess.gif)

Per impostazione predefinita gli utenti standard e gli amministratori possono accedere alle risorse ed eseguire applicazioni nel contesto di protezione per utenti standard. Quando un utente accede al computer, il sistema crea un token di accesso per tale utente. Il token di accesso contiene informazioni sul livello di accesso concesso all'utente, inclusi specifici identificatori di sicurezza (SID, Specific Security Identifier) e privilegi di Windows.

Quando un amministratore accede al computer, vengono creati due token di accesso distinti per l'utente: un token di accesso per utente standard e un token di accesso per amministratore. Il token di accesso per utente standard contiene le stesse informazioni specifiche sull'utente contenute nel token per amministratore, ma senza i privilegi amministrativi di Windows e gli identificatori SID. Il token di accesso per utente standard viene utilizzato per avviare applicazioni che non eseguono attività amministrative (applicazioni per utente standard). Il token di accesso per utente standard viene inoltre utilizzato per visualizzare il desktop (Explorer.exe). Explorer.exe è il processo padre da cui tutti gli altri processi avviati dall'utente ereditano il rispettivo token di accesso. Tutte le applicazioni vengono eseguite pertanto come utente standard, a meno che un utente non specifichi il proprio consenso o le credenziali necessarie affinché un'applicazione possa utilizzare il token di accesso amministrativo completo.

Un utente appartenente al gruppo Administrators può eseguire l'accesso, navigare nel Web e leggere la posta elettronica utilizzando un token di accesso per utente standard. Quando l'amministratore deve eseguire un'attività che richiede il token di accesso amministratore, Windows Server 2012 chiede automaticamente l'approvazione all'utente. Il comportamento di tale richiesta di elevazione dei privilegi può essere configurato tramite lo snap-in Criteri di sicurezza locali (Secpol.msc) o i Criteri di gruppo.

> [!NOTE]
> Il termine "elevate" viene usato per fare riferimento al processo in Windows Server 2012 che richiede all'utente il consenso o le credenziali per l'uso di un token di accesso amministratore completo.

### <a name="the-uac-user-experience"></a>Esperienza utente UAC
Quando Controllo dell'account utente è attivato, l'esperienza utente per gli utenti standard è diversa da quella degli amministratori in modalità Approvazione amministratore. Il metodo consigliato e più sicuro per l'esecuzione di Windows Server 2012 consiste nel rendere l'account utente primario un account utente standard. L'esecuzione come utente standard consente di potenziare al massimo la sicurezza in un ambiente gestito. Con il componente integrato di elevazione dei privilegi in Controllo dell'account utente, gli utenti standard possono svolgere facilmente un'attività amministrativa immettendo credenziali valide per un account amministratore locale. Il componente integrato di elevazione dei privilegi predefinito in Controllo dell'account utente è la richiesta di credenziali.

L'alternativa all'esecuzione come utente standard è rappresentata dall'esecuzione come amministratore in modalità Approvazione amministratore. Con il componente integrato di elevazione dei privilegi in Controllo dell'account utente, gli utenti appartenenti al gruppo Administrators locale possono svolgere facilmente un'attività amministrativa fornendo l'approvazione. Il componente integrato di elevazione dei privilegi predefinito in Controllo dell'account utente per un account amministratore in modalità Approvazione amministratore è la richiesta di consenso. Il comportamento della richiesta di elevazione dei privilegi in Controllo dell'account utente può essere configurato tramite lo snap-in Criteri di sicurezza locali (Secpol.msc) o Criteri di gruppo.

**Richieste di consenso e di credenziali**

Con UAC abilitato, Windows Server 2012 richiede il consenso o richiede le credenziali di un account amministratore locale valido prima di avviare un programma o un'attività che richiede un token di accesso amministratore completo. Tale richiesta impedisce che venga installato malware automaticamente.

**Richiesta di consenso**

La richiesta di consenso viene visualizzata quando un utente tenta di eseguire un'attività per la quale è richiesto un token di accesso amministrativo. Nella schermata seguente viene illustrata una richiesta di consenso di Controllo dell'account utente.

![Screenshot della richiesta di consenso del controllo dell'account utente](../media/How-User-Account-Control-Works/UACConsentPrompt.gif)

**Richiesta di credenziali**

La richiesta di credenziali viene visualizzata quando un utente tenta di eseguire un'attività per la quale è richiesto un token di accesso amministrativo. Il comportamento della richiesta predefinita per l'utente standard può essere configurato tramite lo snap-in Criteri di sicurezza locali (Secpol.msc) o Criteri di gruppo. È possibile che agli amministratori venga richiesto di fornire le proprie credenziali impostando Controllo account utente: comportamento della richiesta di elevazione dei privilegi per gli amministratori in modalità Approvazione amministratore su Richiedi credenziali.

Nella schermata seguente viene illustrato un esempio di richiesta di credenziali di Controllo dell'account utente.

![Screenshot che mostra un esempio della richiesta di credenziali del controllo dell'account utente](../media/How-User-Account-Control-Works/UACCredentialPrompt.gif)

**Richieste di elevazione del controllo dell'account utente**

Le richieste di elevazione dei privilegi di Controllo dell'account utente sono codificate con colori specifici per ogni applicazione, pertanto consentono di identificare immediatamente un potenziale rischio per la sicurezza di un'applicazione. Quando un'applicazione tenta di eseguire con il token di accesso completo di un amministratore, Windows Server 2012 analizza innanzitutto il file eseguibile per determinarne l'autore. Le applicazioni vengono dapprima suddivise in tre categorie in base all'autore del file eseguibile: Windows Server 2012, autore verificato (firmato) e autore non verificato (senza firma). Il diagramma seguente illustra in che modo Windows Server 2012 determina la richiesta di elevazione dei colori da presentare all'utente.

I colori per le richieste di elevazione dei privilegi sono:

-   Sfondo rosso con icona scudo rossa: l'applicazione è bloccata da Criteri di gruppo o proviene da un autore che è bloccato.

-   Sfondo blu con un'icona a forma di scudo blu e oro: l'applicazione è un'applicazione amministrativa Windows Server 2012, ad esempio un elemento del pannello di controllo.

-   Sfondo blu con icona scudo blu: l'applicazione è firmata con Authenticode ed è considerata attendibile dal computer locale.

-   Sfondo giallo con icona scudo gialla: l'applicazione non è firmata o è firmata ma non è ancora considerata attendibile dal computer locale.

**Icona scudo**

Alcuni elementi del Pannello di controllo, ad esempio **Proprietà data e ora**, contengono una combinazione di operazioni per amministratore e utente standard. Gli utenti standard possono visualizzare l'orologio e modificare il fuso orario, tuttavia è necessario un token di accesso amministratore per modificare l'ora del sistema locale. Nella schermata seguente viene illustrato l'elemento del Pannello di controllo  **Proprietà data e ora**.

![Screenshot che mostra le proprietà di data e ora * * elemento del pannello di controllo](../media/How-User-Account-Control-Works/UACShieldIcon.gif)

L'icona scudo di sicurezza sul pulsante **Modifica data e ora** indica che per questo processo è necessario un token di accesso amministrativo completo e che verrà visualizzata una richiesta di elevazione dei privilegi di Controllo dell'account utente.

**Protezione della richiesta di elevazione dei privilegi**

Il processo di elevazione dei privilegi può essere protetto ulteriormente indirizzando la richiesta al desktop sicuro. Per impostazione predefinita, le richieste di consenso e di credenziali vengono visualizzate sul desktop sicuro in Windows Server 2012. Solo i processi Windows possono accedere al desktop sicuro. Per livelli di sicurezza più elevati, è consigliabile mantenere attivata l'impostazione **Controllo account utente: alla richiesta di elevazione passa al desktop sicuro**.

Quando è richiesta l'elevazione dei privilegi per un file eseguibile, al desktop interattivo, denominato anche desktop utente, subentra il desktop sicuro. Il desktop utente si attenua e sul desktop sicuro viene visualizzata una richiesta di elevazione dei privilegi alla quale è necessario rispondere per poter continuare. Quando l'utente fa clic su Sì o su No, viene ripristinato il desktop utente.

Un eventuale malware potrebbe visualizzare un'imitazione del desktop sicuro, ma quando il criterio Controllo account utente: comportamento della richiesta di elevazione dei privilegi per gli amministratori in modalità Approvazione amministratore è impostato su Richiedi consenso, il malware non ottiene l'elevazione dei privilegi se l'utente fa clic su Sì nell'imitazione. Se il criterio è impostato su Richiedi credenziali, un malware che imita la richiesta di credenziali potrebbe riuscire a ottenere le credenziali dall'utente, ma non otterrà l'elevazione dei privilegi. Il sistema dispone comunque di altri meccanismi di protezione che limitano la capacità del malware di prendere il controllo dell'interfaccia utente, anche dopo aver ottenuto una password.

Sebbene il malware possa visualizzare un'imitazione del desktop sicuro, questo problema non può verificarsi a meno che un utente non abbia installato il malware nel computer in precedenza. Poiché i processi che richiedono un token di accesso amministratore non possono essere installati in modo automatico quando Controllo dell'account utente è attivato, l'utente deve fornire il consenso in modo esplicito facendo clic su **Sì** o fornendo le credenziali di amministratore. Il comportamento specifico della richiesta di elevazione dei privilegi di Controllo dell'account utente dipende da Criteri di gruppo.

## <a name="uac-architecture"></a>Architettura di Controllo dell'account utente
Nel diagramma seguente viene illustrata in dettaglio l'architettura di Controllo dell'account utente.

![Diagramma che illustra in dettaglio l'architettura del controllo dell'account utente](../media/How-User-Account-Control-Works/UACArchitecture.gif)

Per comprendere meglio ogni componente, vedere la tabella seguente:

|Component|Descrizione|
|-------|--------|
|**Utente**||
|L'utente esegue un'operazione che richiede privilegi|Se l'operazione modifica il file system o il Registro di sistema, eseguita una chiamata alla virtualizzazione. Tutte le altre operazioni eseguono una chiamata a ShellExecute.|
|ShellExecute|ShellExecute esegue una chiamata a CreateProcess. ShellExecute attende l'errore ERROR_ELEVATION_REQUIRED proveniente da CreateProcess. Se riceve l'errore, ShellExecute esegue una chiamata al servizio Informazioni applicazioni per tentare di eseguire l'attività necessaria con la richiesta di elevazione dei privilegi.|
|CreateProcess|Se l'applicazione richiede l'elevazione dei privilegi, CreateProcess rifiuta la chiamata con ERROR_ELEVATION_REQUIRED.|
|**Sistema**||
|Servizio informazioni applicazioni|Un servizio di sistema che facilita l'avvio di applicazioni che richiedono l'esecuzione di uno o più privilegi o diritti utente elevati, ad esempio attività amministrative locali, oltre che di applicazioni che richiedono livelli di integrità superiori. Per avviare tali applicazioni, il servizio Informazioni applicazioni crea un nuovo processo per l'applicazione con un token di accesso completo per utente amministrativo quando è richiesta l'elevazione dei privilegi e (in base a Criteri di gruppo) viene fornito il consenso dall'utente in tal senso.|
|Elevazione di un'installazione ActiveX|Se ActiveX non è installato, viene verificato il livello del dispositivo di scorrimento di Controllo dell'account utente. Se ActiveX è installato, viene verificata l'impostazione **Controllo account utente: alla richiesta di elevazione passa al desktop sicuro**.|
|Verifica livello dispositivo di scorrimento Controllo dell'account utente|Controllo dell'account utente prevede ora quattro livelli di notifica e un dispositivo di scorrimento da utilizzare per selezionarne uno:<p><ul><li>Alta<p>    Se il dispositivo di scorrimento è impostato su **Notifica sempre**, viene verificato se il desktop sicuro è abilitato.</li><li>Media<p>    Se il dispositivo di scorrimento è impostato su **Un programma tenta di eseguire modifiche nel computer (impostazione predefinita)** , viene verificata l'impostazione **Controllo account utente: eleva solo file eseguibili firmati e convalidati**:<p><ul><li>Se l'impostazione è attivata, viene imposta la convalida del percorso di certificazione PKI di un determinato file eseguibile prima che ne sia consentita l'esecuzione.</li><li>Se l'impostazione non è attivata (impostazione predefinita), non viene imposta la convalida del percorso di certificazione PKI prima che sia consentita l'esecuzione di un determinato file eseguibile. Viene verificata l'impostazione **Controllo account utente: alla richiesta di elevazione passa al desktop sicuro**.</li></ul></li><li>Bassa<p>    Se il dispositivo di scorrimento è impostato su **Notifica solo quando un programma tenta di eseguire modifiche nel computer (non attenuare il desktop)** , viene eseguita una chiamata a CreateProcess.</li><li>Non notificare mai<p>    Se il dispositivo di scorrimento è impostato in modo da non ricevere alcuna **notifica quando**, il prompt di controllo dell'account utente non notificherà mai quando un programma tenta di installare o tenta di apportare modifiche al computer. **Importante:**     Questa impostazione non è consigliata. Questa impostazione è identica a quella dell'impostazione del **controllo dell'account utente: comportamento della richiesta di elevazione dei privilegi per gli amministratori in modalità Approvazione amministratore** per **elevare senza chiedere conferma**.</li></ul>|
|Desktop sicuro attivato|Viene verificata l'impostazione **Controllo account utente: alla richiesta di elevazione passa al desktop sicuro**:<p>-Se il desktop sicuro è abilitato, tutte le richieste di elevazione dei privilegi vengono indirizzate al desktop sicuro indipendentemente dalle impostazioni dei criteri del comportamento del prompt per gli amministratori e gli utenti standard.<br />-Se il desktop protetto non è abilitato, tutte le richieste di elevazione dei privilegi vengono indirizzate al desktop dell'utente interattivo e vengono usate le impostazioni per utente per gli amministratori e gli utenti standard.|
|CreateProcess|CreateProcess esegue una chiamata ad AppCompat, Fusion e rilevamento del programma di installazione per valutare se è necessario elevare i privilegi per l'applicazione. Il file eseguibile viene quindi ispezionato per verificare qual è il livello di esecuzione necessario, che è memorizzato nel manifesto dell'applicazione per il file eseguibile. Se il livello di esecuzione richiesto, specificato nel manifesto, non corrisponde al token di accesso, CreateProcess non funziona e restituisce un errore (ERROR_ELEVATION_REQUIRED) a ShellExecute. |
|AppCompat|Il database AppCompat memorizza le informazioni contenute nelle voci relative alle correzioni di compatibilità tra applicazioni.|
|Fusion|Il database Fusion memorizza le informazioni provenienti dai manifesti che descrivono le applicazioni. Lo schema del manifesto viene aggiornato in modo da aggiungere un nuovo campo per il livello di esecuzione richiesto.|
|Rilevamento del programma di installazione|Vengono rilevati i file di installazione eseguibili per impedire l'esecuzione di installazioni all'insaputa o senza il consenso dell'utente.|
|**Kernel**||
|Virtualizzazione|La tecnologia di virtualizzazione impedisce che l'esecuzione delle applicazioni non compatibili fallisca in modo invisible o che vengano generati errori la cui causa non possa essere determinata. In Controllo dell'account utente sono inoltre previste la virtualizzazione dei file e del Registro di sistema e l'accesso per le applicazioni che eseguono la scrittura in aree protette.|
|File system e Registro di sistema|La virtualizzazione del Registro di sistema e dei file a livello di singolo utente reindirizza le richieste di scrittura sul Registro di sistema e sui file a livello di computer verso percorsi equivalenti a livello di singolo utente. Le richieste di lettura vengono reindirizzate innanzitutto verso il percorso a livello di singolo utente virtualizzato e quindi verso il percorso a livello di computer.|

È stata apportata una modifica all'UAC di Windows Server 2012 dalle versioni precedenti di Windows. Il nuovo dispositivo di scorrimento non attiverà mai il controllo dell'account utente completamente. La nuova impostazione:

-   Mantieni in esecuzione il servizio UAC.

-   Verificare che tutte le richieste di elevazione dei privilegi avviate dagli amministratori vengano approvate automaticamente senza visualizzare un prompt UAC.

-   Nega automaticamente tutte le richieste di elevazione dei privilegi per gli utenti standard.

> [!IMPORTANT]
> Per disabilitare completamente UAC è necessario disabilitare il criterio **controllo account utente: Esegui tutti gli amministratori in modalità Approvazione amministratore**.

> [!WARNING]
> Le applicazioni personalizzate non funzioneranno in Windows Server 2012 quando il controllo dell'account utente è disabilitato.

### <a name="virtualization"></a>Virtualizzazione
In un ambiente aziendale gli amministratori di sistema tentano di proteggere i sistemi, pertanto molte applicazioni line-of-business sono progettate in modo tale da utilizzare solo un token di accesso per utente standard. Di conseguenza, gli amministratori IT non devono sostituire la maggior parte delle applicazioni quando eseguono Windows Server 2012 con UAC abilitato.

 Windows Server 2012 include la tecnologia di virtualizzazione di file e registro di sistema per le applicazioni che non sono conformi a UAC e che richiedono che il token di accesso di un amministratore venga eseguito correttamente. La virtualizzazione garantisce che anche le applicazioni che non sono conformi a UAC siano compatibili con Windows Server 2012. Quando un'applicazione amministrativa non conforme tenta di eseguire la scrittura in una directory protetta, ad esempio Programmi, Controllo dell'account utente fornisce una visualizzazione virtualizzata della risorsa che l'applicazione sta tentando di modificare. La copia virtualizzata viene mantenuta nel profilo dell'utente. Questa strategia consente di creare una copia del file virtualizzato per ogni utente che esegue l'applicazione non conforme.

Molte attività delle applicazioni funzionano correttamente grazie all'utilizzo delle funzionalità di virtualizzazione. Sebbene la virtualizzazione consenta l'esecuzione della maggioranza delle applicazioni, si tratta di una correzione provvisoria e non di una soluzione a lungo termine. Gli sviluppatori di applicazioni devono modificare le applicazioni in modo che siano conformi al programma Windows Server 2012 logo il prima possibile, invece di basarsi sulla virtualizzazione di file, cartelle e registro di sistema.

La virtualizzazione non è un'opzione disponibile negli scenari seguenti:

1.  La virtualizzazione non è compatibile con le applicazioni con privilegi elevati che vengono eseguite con un token di accesso amministrativo completo.

2.  La virtualizzazione supporta solo le applicazioni a 32 bit. Le applicazioni a 64 bit senza privilegi elevati ricevono semplicemente un messaggio di accesso negato quando tentano di acquisire un handle (identificatore univoco) per un oggetto Windows. È necessario che le applicazioni Windows a 64 bit native siano compatibili con Controllo dell'account utente per poter eseguire la scrittura dei dati nei percorsi corretti.

3.  La virtualizzazione viene disattivata per un'applicazione se questa include un manifesto dell'applicazione con un attributo relativo al livello di esecuzione richiesto.

### <a name="request-execution-levels"></a>Livelli di esecuzione delle richieste
Un manifesto di un'applicazione è un file XML in cui sono descritti e identificati gli assembly affiancati condivisi e privati a cui un'applicazione deve effettuare il binding in fase di esecuzione. In Windows Server 2012 il manifesto dell'applicazione include voci per la compatibilità delle applicazioni UAC. Le applicazioni amministrative che includono una voce nel manifesto dell'applicazione richiedono l'autorizzazione dell'utente per accedere al relativo token di accesso. Anche se nel file manifesto dell'applicazione non è inclusa una voce, la maggior parte delle applicazioni amministrative può comunque essere eseguita senza modifiche grazie alle correzioni di compatibilità, Le correzioni rapide per la compatibilità delle applicazioni sono voci di database che consentono alle applicazioni che non sono conformi a UAC di funzionare correttamente con Windows Server 2012.

Un livello di esecuzione richiesto dovrebbe essere aggiunto a tutte le applicazioni conformi a Controllo dell'account utente. Se l'applicazione richiede l'accesso amministrativo al sistema, contrassegnando l'applicazione con un livello di esecuzione richiesto di tipo "richiedi amministratore" si assicura che il sistema identifichi il programma come applicazione amministrativa ed esegua la necessaria procedura di elevazione dei privilegi. I livelli di esecuzione richiesti determinano i privilegi necessari per un'applicazione.

### <a name="installer-detection-technology"></a>Tecnologia di rilevamento del programma di installazione
I programmi di installazione sono applicazioni progettate per distribuire software e, nella maggior parte dei casi, eseguono la scrittura nelle directory di sistema e nelle chiavi di Registro del sistema. Con la tecnologia di rilevamento del programma di installazione, la scrittura in tali posizioni protette del sistema può essere eseguita in genere solo da un amministratore, pertanto gli utenti standard non dispongono di privilegi di accesso sufficienti per l'installazione dei programmi. Windows Server 2012 rileva in modo euristico i programmi di installazione e richiede le credenziali di amministratore o l'approvazione da parte dell'utente amministratore per poter eseguire con privilegi di accesso. Windows Server 2012 rileva euristicamente anche gli aggiornamenti e i programmi per la disinstallazione delle applicazioni. Uno degli obiettivi per cui Controllo dell'account utente è stato progettato è impedire l'esecuzione di installazioni all'insaputa e senza il consenso dell'utente, poiché i programmi di installazione eseguono la scrittura in aree protette del file system e del Registro di sistema.

Il rilevamento dei programmi di installazione si applica solo agli elementi seguenti:

-   File eseguibili a 32 bit

-   Applicazioni senza un attributo relativo al livello di esecuzione richiesto

-   Processi interattivi eseguiti come utente standard con Controllo dell'account utente attivato

Prima della creazione di un processo a 32 bit, vengono verificati gli attributi seguenti per stabilire se si tratta di un programma di installazione:

-   Parole chiave come "install," "setup" o "update" incluse nel nome file.

-   Parole chiave contenute nei campi della risorsa di versione: Fornitore, Società, Nome prodotto, Descrizione file, Nome file originale, Nome interno e Nome esportazione.

-   Parole chiave nel manifesto affiancato incorporate nel file eseguibile.

-   Parole chiave in voci StringTable specifiche collegate nel file eseguibile.

-   Attributi chiave nei dati di script della risorsa collegate nel file eseguibile.

-   Sequenze di destinazione dei byte presenti nel file eseguibile.

> [!NOTE]
> Le parole chiave e le sequenze di byte derivano da caratteristiche comuni che sono state osservate in varie tecnologie dei programmi di installazione.

> [!NOTE]
> È necessario che l'impostazione Controllo account utente: rileva installazione applicazioni e richiedi elevazione sia attivata per consentire il rilevamento dei programmi di installazione. Tale impostazione è attivata per impostazione predefinita e può essere configurata a livello locale con lo snap-in Criteri di sicurezza locali (secpol.msc) o a livello di dominio, unità organizzativa o gruppi specifici con Criteri di gruppo (Gpedit.msc).


