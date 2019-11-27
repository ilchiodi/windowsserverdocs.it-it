---
title: Materiale di riferimento per la protezione dell'accesso con privilegi
description: Controlli di sicurezza operativi per i domini di Windows Server Active Directory
ms.prod: windows-server
ms.topic: article
ms.assetid: 22ee9a77-4872-4c54-82d9-98fc73a378c0
ms.date: 02/14/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: mas
ms.openlocfilehash: 56e1c028a9b18db7b23e8f04e943e4113837b66b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407225"
---
# <a name="active-directory-administrative-tier-model"></a>Modello a livelli dell'amministrazione di Active Directory

>Si applica a: Windows Server

Lo scopo di questo modello a livelli è di proteggere i sistemi di identità mediante un set di zone buffer tra il controllo completo dell'ambiente (livello 0) e le risorse di workstation ad alto rischio che gli utenti malintenzionati compromettono frequentemente.

![Diagramma che illustra i tre livelli del modello a livelli](../media/securing-privileged-access-reference-material/PAW_RM_Fig1.JPG)

Il modello a livelli è composto da tre livelli e include solo gli account amministrativi, non gli account utente standard:

- **Livello 0**: controllo diretto dell'identità dell'organizzazione nell'ambiente. Il livello 0 include account, gruppi e altre risorse che detengono il controllo amministrativo diretto o indiretto della foresta, dei domini o dei controller di dominio di Active Directory e tutte le risorse in esso contenuti. Il livello di riservatezza di tutte le risorse di livello 0 è equivalente in quanto tutte controllano le altre in modo efficace.
- **Livello 1**: controllo dei server aziendali e delle applicazioni. Le risorse di livello 1 includono i sistemi operativi del server, i servizi cloud e le applicazioni aziendali. Gli account amministratore di livello 1 hanno il controllo amministrativo di una percentuale significativa del valore aziendale ospitato su queste risorse. Un ruolo di esempio comune sono gli amministratori del server che gestiscono questi sistemi operativi con la possibilità di influenzare tutti i servizi aziendali.
- **Livello 2**: controllo delle workstation e dei dispositivi degli utenti. Gli account amministratore di livello 2 hanno il controllo amministrativo di una percentuale significativa del valore aziendale ospitato sulle workstation e sui dispositivi degli utenti. Gli esempi includono l'Help Desk e gli amministratori di supporto del computer, in quanto possono avere un impatto sull'integrità della maggior parte dei dati utente.

> [!NOTE]
> I livelli fungono anche da meccanismo di base per la definizione delle priorità per la protezione delle risorse amministrative, ma è importante considerare che un utente malintenzionato con controllo di tutte le risorse a qualsiasi livello può accedere a tutti o alla maggior parte dei beni aziendali. I motivi per cui è utile come meccanismo di base per la definizione delle priorità sono le difficoltà e i costi a cui gli utenti malintenzionati sono soggetti. Per un utente malintenzionato è più facile agire con il controllo completo di tutte le identità (livello 0) o dei server e dei servizi cloud (livello 1) rispetto ad accedere a ogni singola workstation o dispositivo (livello 2) per ottenere i dati dell'organizzazione.

## <a name="containment-and-security-zones"></a>Aree di contenimento e sicurezza

I livelli si riferiscono a un'area di sicurezza specifica. Sebbene siano state denominate in vari modi, le aree di sicurezza sono un approccio consolidato che fornisce il contenimento delle minacce per la sicurezza attraverso l'isolamento del livello di rete tra di essi. Il modello a livelli si integra con l'isolamento, fornendo il contenimento degli avversari all'interno di un'area di sicurezza in cui l'isolamento rete non è efficace. Le aree di sicurezza possono estendersi sia nelle infrastrutture cloud che in quelle cloud, come nell'esempio in cui i controller di dominio e i membri del dominio nello stesso dominio sono ospitati in locale e in Azure.

![Diagramma che illustra come le aree di sicurezza possono estendersi sia in locale che nell'infrastruttura cloud](../media/securing-privileged-access-reference-material/PAW_RM_Fig2.JPG)

Il modello a livelli impedisce l'escalation dei privilegi limitando il controllo degli amministratori e le loro possibilità di accesso (dato che l'accesso a un computer concede il controllo di tali credenziali e di tutte le risorse gestite da queste credenziali).

### <a name="control-restrictions"></a>Restrizioni di controllo

Le restrizioni di controllo sono visualizzate nella figura seguente:

![Diagramma delle restrizioni di controllo](../media/securing-privileged-access-reference-material/PAW_RM_Fig3.JPG)

### <a name="primary-responsibilities-and-critical-restrictions"></a>Responsabilità principali e restrizioni critiche

**Amministratore di livello 0**: gestisce l'archivio identità e un numero ridotto di sistemi che lo controllano in modo efficace, e:

- Può gestire e controllare le risorse di qualsiasi livello in base alle esigenze
- Può accedere solo in modo interattivo o alle risorse attendibili di livello 0

**Amministratore di livello 1**: gestione di server aziendali, servizi e applicazioni, e:

- Può gestire e controllare solo le risorse di livello 1 o 2
- Può accedere solo alle risorse attendibili (tramite il tipo di accesso alla rete) di livello 1 o 0
- Può accedere solo in modo interattivo alle risorse attendibili di livello 1

**Amministratore di livello 2**: gestisce desktop aziendali, computer portatili, stampanti e altri dispositivi utente, e:

- Può gestire e controllare solo le risorse di livello 2
- Può accedere alle risorse (tramite il tipo di accesso alla rete) di qualsiasi livello in base alle esigenze
- Può accedere solo in modo interattivo alle risorse attendibili di livello 2

### <a name="logon-restrictions"></a>Restrizioni di accesso

Le restrizioni di accesso sono visualizzate nella figura seguente:

![Diagramma delle restrizioni di accesso](../media/securing-privileged-access-reference-material/PAW_RM_Fig4.JPG)

> [!NOTE]
> Si noti che alcune risorse possono avere un impatto sulla disponibilità dell'ambiente di livello 0, ma non direttamente sulla riservatezza o l'integrità delle risorse. Tali risorse includono il servizio server DNS e i dispositivi di rete critici come i proxy di Internet.

## <a name="clean-source-principle"></a>Principio di origine pulita

Il principio di origine pulita richiede che tutte le dipendenze di sicurezza siano attendibili come l'oggetto protetto.

![Diagramma che illustra come un soggetto che controlla un oggetto è una dipendenza di sicurezza di tale oggetto](../media/securing-privileged-access-reference-material/PAW_RM_Fig5.JPG)

Qualsiasi soggetto che controlla un oggetto è una dipendenza di sicurezza di tale oggetto. Se un antagonista può controllare qualsiasi elemento che effettivamente controlla un oggetto di destinazione, egli può controllare tale oggetto di destinazione. Per questo motivo, è necessario assicurarsi che le garanzie di tutte le dipendenze di sicurezza si trovino al livello di sicurezza desiderato dell'oggetto stesso, oppure al di sopra di esso.

Sebbene sia un principio semplice, per la sua applicazione è necessario comprendere le relazioni di controllo di una risorsa di interesse (oggetto) ed eseguire un'analisi delle sue dipendenze per individuare tutte le dipendenze di sicurezza (soggetto/i).

Poiché il controllo è transitivo, questo principio deve essere ripetuto in modo ricorsivo. Ad esempio, se A controlla B e B controlla C, A controlla indirettamente anche C.

![Diagramma che illustra in che modo se A controlla B e B controlla C, A controlla indirettamente anche C](../media/securing-privileged-access-reference-material/PAW_RM_Fig6.JPG)

Un utente malintenzionato che compromette A ottiene l'accesso a tutto ciò che A controlla (incluso B) e tutto ciò che B controlla (incluso C). Usando il linguaggio delle dipendenze di sicurezza in questo stesso esempio, B e A sono dipendenze di sicurezza di C e devono essere protette al livello di garanzia di C perché C abbia tale livello di garanzia.

Per i sistemi di identità e l'infrastruttura IT, questo principio deve essere applicato ai mezzi di controllo più comuni, compresi l'hardware in cui sono installati i sistemi, il supporto di installazione dei sistemi, l'architettura e la configurazione del sistema e delle operazioni quotidiane.

## <a name="clean-source-for-installation-media"></a>Origine pulita per i supporti di installazione

![Diagramma che illustra un'origine pulita per i supporti di installazione](../media/securing-privileged-access-reference-material/PAW_RM_CSIM.JPG)

Per applicare il principio di origine pulita al supporto di installazione è necessario assicurarsi che il supporto di installazione non sia stato manomesso da quando è stato rilasciato dal produttore (nel modo migliore in cui è possibile determinarlo). La figura illustra un utente malintenzionato che usa questo percorso per compromettere un computer:

![Figura che illustra un utente malintenzionato che usa questo percorso per compromettere un computer](../media/securing-privileged-access-reference-material/PAW_RM_Fig8.JPG)

Per applicare il principio di origine pulita al supporto di installazione è necessaria la convalida dell'integrità del software in tutto il suo ciclo di vita, anche durante l'acquisizione, l'archiviazione e il trasferimento.

### <a name="software-acquisition"></a>Acquisizione del software

L'origine del software deve essere convalidata in uno dei modi seguenti:

- Il software viene ottenuto da supporti fisici noti che provengono dal produttore o da una fonte autorevole. In genere si tratta di supporti spediti da un fornitore.
- Il software viene ottenuto da Internet e convalidato con hash di file forniti dal produttore.
- Il software viene ottenuto da Internet e convalidato scaricando e confrontando due copie separate:
   - Download in due host senza alcuna relazione di sicurezza (non nello stesso dominio e non gestiti dagli stessi strumenti), preferibilmente da connessioni Internet separate.
   - Confronto dei file scaricati mediante un'utilità come certutil: `certutil -hashfile <filename>`

Se possibile, tutti i software dell'applicazione, ad esempio gli strumenti e i programmi di installazione, devono avere la firma digitale ed essere verificati mediante Windows Authenticode con lo strumento [Windows Sysinternal](https://www.microsoft.com/sysinternals), *sigcheck.exe*, con la verifica delle revoche. Potrebbero essere necessari alcuni software in cui il fornitore potrebbe non fornire questo tipo di firma digitale.

### <a name="software-storage-and-transfer"></a>Trasferimento e archiviazione del software

Dopo aver ottenuto il software, questo deve essere archiviato in un percorso protetto dalle modifiche che possono essere apportate in particolare da host connessi a Internet o personale attendibile a un livello inferiore rispetto ai sistemi in cui verrà installato il sistema operativo o software. Questa archiviazione può essere un supporto fisico o un percorso elettronico sicuro.

### <a name="software-usage"></a>Utilizzo del software

In teoria, il software deve essere convalidato al momento del suo utilizzo, ad esempio quando viene installato manualmente, compresso per uno strumento di gestione della configurazione o importato in uno strumento di gestione della configurazione.

## <a name="clean-source-for-architecture-and-design"></a>Origine pulita per la progettazione e l'architettura

Per applicare il principio di origine pulita all'architettura di sistema è necessario assicurarsi che il sistema non sia dipendente da sistemi di attendibilità inferiori. Un sistema può essere dipendente da un sistema di attendibilità superiore, ma non da un sistema di attendibilità inferiore con standard di sicurezza più bassi.

Ad esempio, per Active Directory è accettabile controllare un PC desktop di un utente standard, ma il controllo di Active Directory rappresenta invece un'escalation significativa del rischio dei privilegi per un PC desktop di un utente standard.

![Diagramma che illustra in che modo un sistema può essere dipendente da un sistema di attendibilità superiore, ma non da un sistema di attendibilità inferiore con standard di sicurezza più bassi](../media/securing-privileged-access-reference-material/PAW_RM_Fig09.JPG)

La relazione di controllo può essere introdotta in diversi modi, inclusi gli elenchi di controllo di accesso (ACL) di sicurezza su oggetti come file system, l'appartenenza al gruppo di amministratori locale su un computer o gli agenti installati in un computer eseguito come Sistema (con la possibilità di eseguire script e codice non autorizzati).

Un esempio spesso trascurato è l'esposizione mediante accesso, che crea una relazione di controllo esponendo le credenziali amministrative di un sistema a un altro sistema. Questa è la causa principale per cui il furto di credenziali come la tecnica Pass-the-Hash è così potente. Quando un amministratore accede a un PC desktop di un utente standard con le credenziali di livello 0, sta esponendo le credenziali su quel PC desktop, consentendogli così il controllo di Active Directory e creando un'escalation del percorso con privilegi ad Active Directory. Per altre informazioni su questi attacchi, vedere [questa pagina](https://technet.microsoft.com/security/dn785092).

A causa dell'elevato numero di risorse che dipendono dai sistemi di identità come Active Directory, è necessario ridurre al minimo il numero di sistemi da cui Active Directory e i controller di dominio dipendono.

![Diagramma che mostra che è necessario ridurre il numero di sistemi da cui dipendono Active Directory e i controller di dominio](../media/securing-privileged-access-reference-material/PAW_RM_Fig010.JPG)

Per altre informazioni sulla protezione avanzata dei rischi principali di Active Directory, vedere [questa pagina](http://aka.ms/hardenAD).

## <a name="operational-standards-based-on-clean-source-principle"></a>Standard operativi basati sul principio di origine pulita

Questa sezione descrive gli standard operativi e le aspettative per il personale amministrativo. Questi standard sono progettati per proteggere il controllo amministrativo dei sistemi del reparto IT di un'organizzazione contro i rischi che possono sorgere dai processi e dalle procedure operative.

![Diagramma che illustra in che modo gli standard sono progettati per proteggere il controllo amministrativo dei sistemi del reparto IT di un'organizzazione contro i rischi che possono sorgere dai processi e dalle procedure operative](../media/securing-privileged-access-reference-material/PAW_RM_Fig11.JPG)

### <a name="integrating-the-standards"></a>Integrazione degli standard

È possibile integrare questi standard negli standard e nelle procedure generali dell'organizzazione. È possibile adattarli ai requisiti specifici, agli strumenti disponibili e al desiderio di rischio della propria organizzazione, ma è consigliabile apportare solo modifiche minime per ridurre i rischi. È consigliabile usare le impostazioni predefinite riportate in questa guida come benchmark per lo stato finale ideale e gestire eventuali delta come eccezioni da affrontare in ordine di priorità.

Le linee guida sugli standard sono organizzate nelle sezioni seguenti:

- Presupposti
- Comitato consultivo per le modifiche
- Procedure operative
   - Riepilogo
   - Dettagli sugli standard

### <a name="assumptions"></a>Presupposti

Gli standard in questa sezione si presuppongono che l'organizzazione disponga degli attributi seguenti:

- Tutti i server e le workstation nell'ambito, oppure la maggior parte, sono aggiunti ad Active Directory.
- Tutti i server da gestire eseguono Windows Server 2008 R2 o versione successiva e hanno abilitata la modalità RestrictedAdmin di RDP.
- Tutte le workstation da gestire eseguono Windows 7 o versione successiva e hanno abilitata la modalità RestrictedAdmin di RDP.

   > [!NOTE]
   > Per abilitare la modalità RestrictedAdmin di RDP, vedere [questa pagina](http://aka.ms/RDPRA).

- Le smart card sono disponibili e rilasciate per tutti gli account amministrativi.
- *Builtin\Administrator* di ogni dominio è stato designato come un account di accesso di emergenza
- Viene distribuita una soluzione di gestione dell'identità aziendale.
- [LAPS](http://aka.ms/laps) è stato distribuito ai server e alle workstation per gestire la password dell'account amministratore locale
- È disponibile una soluzione di Privileged Access Management, ad esempio Microsoft Identity Manager, o se ne adotterà una a breve.
- È stato predisposto personale che si occupa del monitoraggio degli avvisi di protezione e della relativa risposta.
- È disponibile la capacità tecnica per applicare rapidamente gli aggiornamenti sulla sicurezza di Microsoft.
- I baseboard management controller sul server non verranno usati, oppure dovranno conformarsi ai controlli di sicurezza.
- Gli account e i gruppi amministratore dei server (amministratori di livello 1) e delle workstation (amministratori di livello 2) verranno gestiti dagli amministratori del dominio (livello 0).
- Esiste un Comitato consultivo per le modifiche (CAB) o un'altra autorità designata sul posto che si occupa dell'approvazione delle modifiche di Active Directory.

### <a name="change-advisory-board"></a>Comitato consultivo per le modifiche

Il Comitato consultivo per le modifiche (CAB) è l'autorità di approvazione e del forum di discussione per le modifiche che potrebbero interferire con il profilo di sicurezza dell'organizzazione. Tutte le eccezioni a questi standard devono essere inviate al CAB con una valutazione dei rischi e una giustificazione.

Ogni standard in questo documento è influenzato dalla criticità di soddisfare gli standard per un determinato livello.

![Diagramma che illustra lo standard per determinati livelli](../media/securing-privileged-access-reference-material/PAW_RM_Fig12.JPG)

Tutte le eccezioni per gli elementi obbligatori (contrassegnati in questo documento con un triangolo arancione o un ottagono rosso) sono considerate temporanee e devono essere approvate dal CAB. Le linee guida includono:

- La richiesta iniziale implica l'accettazione del rischio e della giustificazione firmata dal supervisore immediato del personale e scade dopo sei mesi.
- I rinnovi richiedono l'accettazione del rischio e della giustificazione firmata da un direttore della business unit e scadono dopo sei mesi.

Tutte le eccezioni per gli elementi consigliati (contrassegnati in questo documento con un cerchio giallo) sono considerate temporanee e devono essere approvate dal CAB. Le linee guida includono:

- La richiesta iniziale implica l'accettazione del rischio e della giustificazione firmata dal supervisore immediato del personale e scade dopo 12 mesi.
- I rinnovi richiedono l'accettazione del rischio e della giustificazione firmata da un direttore della business unit e scadono dopo 12 mesi.

### <a name="operational-practices-standards-summary"></a>Riepilogo degli standard delle procedure operative

Le colonne del livello in questa tabella si riferiscono al livello dell'account amministrativo, il cui controllo in genere influisce su tutte le risorse in tale livello.

![Tabella che illustra i livelli dell'account amministrativo](../media/securing-privileged-access-reference-material/PAW_RM_Fig13.JPG)

Le decisioni operative che vengono prese regolarmente sono fondamentali per mantenere le condizioni di sicurezza dell'ambiente. Questi standard per i processi e le procedure aiutano a garantire che un errore operativo non comporti una vulnerabilità operativa sfruttabile nell'ambiente.

#### <a name="administrator-enablement-and-accountability"></a>Responsabilità e abilitazione dell'amministratore

Gli amministratori devono essere informati, abilitati, sottoposti a formazione e ritenuti responsabili per il funzionamento dell'ambiente nel modo più sicuro possibile.

##### <a name="administrative-personnel-standards"></a>Standard del personale amministrativo

Il personale amministrativo predisposto deve essere esaminato per assicurarne l'affidabilità e l'effettiva esigenza dei privilegi amministrativi:

- Eseguire controlli periodici sul personale prima dell'assegnazione di privilegi amministrativi.
- Esaminare i privilegi amministrativi ogni trimestre per stabilire quale personale abbia ancora un'esigenza aziendale legittima dell'accesso amministrativo.

##### <a name="administrative-security-briefing-and-accountability"></a>Responsabilità e istruzioni sulla sicurezza amministrativa

Gli amministratori devono essere ben informati e sono responsabili per i rischi dell'organizzazione e del loro ruolo nella gestione di tale rischio. Gli amministratori devono essere formati annualmente su:

- L'ambiente di rischio generale
   - Gli antagonisti specifici
   - Le tecniche di attacco tra cui Pass-the-Hash e furto di credenziali
- Le minacce specifiche dell'organizzazione e gli eventi imprevisti
- I ruoli dell'amministratore nella protezione contro gli attacchi
   - La gestione dell'esposizione delle credenziali con il modello a livelli
   - L'uso delle workstation amministrative
   - L'uso della modalità RestrictedAdmin di Remote Desktop Protocol
- Le procedure amministrative specifiche dell'organizzazione
   - Esaminare tutte le linee guida operative in questo standard
   - Implementare le regole principali seguenti:
      - Non usare account amministrativi se non nelle workstation amministrative
      - Non disabilitare o annullare i controlli di sicurezza sul proprio account o sulle workstation (ad esempio, le restrizioni di accesso o gli attributi necessari per le smart card)
      - Segnalare problemi o attività insolite

Per garantire l'affidabilità, tutto il personale che possiede in account amministrativo deve firmare un documento sul codice di condotta per i privilegi amministrativi in cui dichiara che intende seguire le procedure per i criteri amministrativi specifici dell'organizzazione.

##### <a name="provisioning-and-deprovisioning-processes-for-administrative-accounts"></a>Provisioning e deprovisioning dei processi per gli account amministrativi

Per soddisfare i requisiti del ciclo di vita, è necessario soddisfare gli standard seguenti.

- Tutti gli account amministrativi devono essere approvati dall'autorità di approvazione indicata nella tabella seguente.
   - L'approvazione deve essere concessa solo se il personale ha un'esigenza aziendale legittima dei privilegi amministrativi.
   - La concessione dei privilegi amministrativi non deve durare più di sei mesi.
- È necessario effettuare immediatamente il deprovisioning dei privilegi amministrativi quando:
   - Il personale modifica le proprie posizioni.
   - Il personale lascia l'organizzazione.
- Gli account devono essere disabilitati immediatamente quando il personale lascia l'azienda.
- Gli account disabilitati devono essere eliminati entro sei mesi e il record della loro eliminazione deve essere inserito nei record del comitato per l'approvazione delle modifiche.
- Esaminare mensilmente tutte le appartenenze agli account con privilegi per verificare che non siano state concesse autorizzazioni non autorizzate. Questo processo può essere sostituito da uno strumento automatico che segnala le modifiche.

|Livello dei privilegi dell'account|Autorità di approvazione|Frequenza di revisione dell'appartenenza|
|--------------|------------|----------------|
|Amministratore di livello 0|Comitato di approvazione delle modifiche|Mensile o automatico|
|Amministratore di livello 1|Amministratori di livello 0 o sicurezza|Mensile o automatico|
|Amministratore di livello 2|Amministratori di livello 0 o sicurezza|Mensile o automatico|

#### <a name="operationalize-least-privilege"></a>Rendere operativo il privilegio minimo

Questi standard aiutano a ottenere privilegi minimi, riducendo il numero di amministratori nel ruolo e la durata dei loro privilegi.

> [!NOTE]
> Per ottenere i privilegi minimi dell'organizzazione è necessario comprendere i ruoli aziendali, i relativi requisiti e i meccanismi di progettazione per verificare che siano in grado di eseguire il proprio lavoro con i privilegi minimi. Per raggiungere uno stato di privilegio minimo in un modello amministrativo è spesso necessario usare più approcci:
>
> - Limitare il numero di amministratori o membri di gruppi con privilegi
> - Delegare meno privilegi agli account
> - Assegnare privilegi a scadenza su richiesta
> - Assegnare ad altri membri del personale la capacità di eseguire attività (approccio concierge)
> - Fornire i processi per l'accesso di emergenza e gli scenari di uso rari

##### <a name="limit-count-of-administrators"></a>Limitare il numero di amministratori

È necessario assegnare un minimo di due membri del personale qualificato a ogni ruolo amministrativo per garantire la continuità aziendale.

Se il numero di membri del personale assegnati a qualsiasi ruolo supera i due, il comitato di approvazione delle modifiche deve approvare i motivi specifici per l'assegnazione dei privilegi ai singoli membri (inclusi i due originali). La giustificazione dell'approvazione deve includere:

- Il tipo di attività tecniche eseguite dagli amministratori che richiedono privilegi amministrativi
- La frequenza con cui vengono eseguite le attività
- Il motivo specifico per cui le attività non possono essere eseguite da un altro amministratore per suo conto
- Documentare tutti gli altri approcci alternativi noti per la concessione del privilegio e perché ognuno di loro non è accettabile

##### <a name="dynamically-assign-privileges"></a>Assegnare i privilegi in modo dinamico

Agli amministratori è richiesto di ottenere le autorizzazioni "JIT" per poterle usare durante l'esecuzione delle attività. Non verranno assegnate autorizzazioni permanenti per gli account amministrativi.

> [!NOTE]
> I privilegi di amministratore assegnati in modo permanente tendono a creare una strategia di "privilegio massimo" perché il personale amministrativo richiede un accesso rapido alle autorizzazioni per garantire la disponibilità operativa se si verifica un problema. Le autorizzazione JIT offrono la capacità di:
>
> - Assegnare autorizzazioni in modo più granulare, avvicinandosi ai privilegi minimi.
> - Ridurre il tempo di esposizione dei privilegi
> - Tenere traccia dell'uso delle autorizzazioni per rilevare attacchi o abusi.

#### <a name="manage-risk-of-credential-exposure"></a>Gestire il rischio di esposizione delle credenziali

Usare le procedure seguenti per gestire in modo corretto il rischio di esposizione delle credenziali.

##### <a name="separate-administrative-accounts"></a>Account amministrativi separati

Tutto il personale autorizzato a disporre di privilegi amministrativi deve possedere account separati per le funzioni amministrative diverse dagli account utente.

- **Account utente standard**: privilegi di utente standard concessi per le attività di utente standard, ad esempio la posta elettronica, l'esplorazione del Web e l'uso di applicazioni line-of-business. A questi account non devono essere concessi privilegi amministrativi.
- **Account amministrativi**: account separati creati per il personale assegnato ai privilegi amministrativi appropriati. Un amministratore a cui è richiesto di gestire le risorse in ogni livello deve possedere un account separato per ogni livello. Questi account non devono avere accesso alla posta elettronica o alla rete Internet pubblica.

##### <a name="administrator-logon-practices"></a>Procedure di accesso dell'amministratore

Prima che un amministratore possa accedere a un host in modo interattivo (in locale rispetto al protocollo RDP standard, usando RunAs oppure la console di virtualizzazione), tale host deve soddisfare o superare lo standard del livello dell'account amministrativo (o di un livello superiore).

Gli amministratori possono accedere alle proprie workstation solo con gli account amministrativi. Gli amministratori possono accedere alle risorse gestite solo usando la tecnologia di supporto approvata descritta nella sezione successiva.

> [!NOTE]
> Ciò è necessario perché l'accesso a un host in modo interattivo concede il controllo delle credenziali a tale host.
>
> Vedere [Strumenti di amministrazione e tipi di accesso](http://aka.ms/admintoolsecurity) per informazioni dettagliate sui tipi di accesso, sugli strumenti di gestione comuni e sull'esposizione delle credenziali.

##### <a name="use-of-approved-support-technology-and-methods"></a>Uso della tecnologia di supporto approvata e metodi

Gli amministratori che supportano gli utenti e i sistemi remoti devono seguire queste linee guida per evitare che un antagonista nel controllo del computer remoto possa rubare le credenziali amministrative.

- Se sono disponibili è necessario usare le opzioni di supporto primario.
- Le opzioni di supporto secondarie devono essere usate solo se l'opzione di supporto primario non è disponibile.
- Non è possibile usare i metodi di supporto non consentiti.
- Nessun account amministrativo può accedere a Internet o alla posta elettronica in nessun momento.

###### <a name="tier-0-forest-domain-and-dc-administration"></a>Foresta di livello 0, dominio e amministrazione dei controller di dominio

Assicurarsi che per questo scenario vengano applicate le procedure seguenti:

- **Supporto del server remoto**: quando si accede in modalità remota un server, gli amministratori di livello 0 devono attenersi alle seguenti indicazioni:
  - **Primario (strumento)** : strumenti remoti che sfruttano gli accessi alla rete (tipo 3). Per altre informazioni, vedere [Strumenti di amministrazione e tipi di accesso](http://aka.ms/admintoolsecurity).
  - **Primario (interattivo)** : usare RestrictedAdmin di RDP o una sessione RDP standard da una workstation amministrativa con un account di dominio

    > [!NOTE]
    > Se si dispone di una soluzione di gestione privilegi di livello 0, aggiungere "che usa autorizzazioni ottenute in tempo da una soluzione di Privileged Access Management."

- **Supporto del server fisico**: quando fisicamente presente in una console di un server o di una macchina virtuale (strumenti Hyper-V o VMWare), questi account non dispongono di alcuna restrizione per l'uso di strumenti di amministrazione specifici, ma solo delle restrizioni generali per le attività di utente standard come la posta elettronica e la navigazione nelle reti Internet aperte.

   > [!NOTE]
   > L'amministrazione di livello 0 è diversa dall'amministrazione degli altri livelli perché tutte le risorse di livello 0 possiedono già il controllo diretto o indiretto di tutte le risorse. Ad esempio, un utente malintenzionato che controlla un controller di dominio non ha bisogno di rubare le credenziali dagli amministratori che hanno effettuato l'accesso poiché ha già accesso a tutte le credenziali di dominio nel database.

###### <a name="tier-1-server-and-enterprise-application-support"></a>Server di livello 1 e supporto delle applicazione enterprise

Assicurarsi che per questo scenario vengano applicate le procedure seguenti:

- **Supporto del server remoto**: quando si accede in modalità remota un server, gli amministratori di livello 1 devono attenersi alle seguenti indicazioni:
   - **Primario (strumento)** : strumenti remoti che sfruttano gli accessi alla rete (tipo 3). Per altre informazioni, vedere [Mitigating Pass-the-Hash and Other Credential Theft](https://www.microsoft.com/pth) (Limitazione della tecnica Pass-the-Hash e furto di altre credenziali) versione 1 (pp 42-47).
   - **Primario (interattivo)** : usare RestrictedAdmin di RDP da una workstation amministrativa con un account di dominio che usa le autorizzazioni ottenute in tempo da una soluzione di Privileged Access Management.
   - **Secondario**: accedere al server usando la password di un account locale impostata da LAPS da una workstation amministrativa.
   - **Non consentito**: il protocollo RDP standard non deve essere usato con un account di dominio.
   - **Non consentito**: uso delle credenziali dell'account di dominio durante la sessione (ad esempio, uso di *RunAs* o dell'autenticazione per una condivisione). Ciò espone le credenziali di accesso al rischio di furto.
- **Supporto del server fisico**: quando fisicamente presente in una console di un server o di una macchina virtuale (strumenti Hyper-V o VMWare), gli amministratori di livello 1 devono recuperare la password dell'account locale da LAPS prima dell'accesso al server.
   - **Primario**: recuperare la password dell'account locale impostata da LAPS da una workstation amministrativa prima di accedere al server.
   - **Non consentito**: l'accesso con un account di dominio non è consentito in questo scenario.
   - **Non consentito**: uso delle credenziali dell'account di dominio durante la sessione (ad esempio, RunAs o autenticazione per una condivisione). Ciò espone le credenziali di accesso al rischio di furto.

###### <a name="tier-2-help-desk-and-user-support"></a>Help Desk di livello 2 e supporto utente

L'Help Desk e le organizzazioni di supporto degli utenti offrono assistenza agli utenti finali (che non richiedono privilegi amministrativi) e alle workstation dell'utente (che richiedono privilegi amministrativi).

**Supporto per l'utente**: le attività includono l'assistenza agli utenti per le attività che non richiedono alcuna modifica alla workstation, il che implica il mostrar loro una funzionalità dell'applicazione o del sistema operativo.

- **Supporto per l'utente da parte dell'Help Desk**: il personale di supporto di livello 2 si trova fisicamente nell'area di lavoro dell'utente.
   - **Primario**: il supporto "Over The Shoulder" può essere fornito con nessuno strumento.
   - **Non consentito**: l'accesso con le credenziali amministrative dell'account di dominio non è consentito in questo scenario. Se sono necessari privilegi amministrativi, rivolgersi al supporto per workstation da parte dell'Help Desk.
- **Supporto per l'utente remoto**: il personale di supporto di livello 2 è lontano fisicamente rispetto all'utente.
   - **Primario**: è possibile usare l'assistenza remota, Skype for Business o una condivisione dello schermo dell'utente simile. Per altre informazioni, vedere [Cos'è l'assistenza remota Windows?](https://windows.microsoft.com/en-us/windows/what-is-windows-remote-assistance)
   - **Non consentito**: l'accesso con le credenziali amministrative dell'account di dominio non è consentito in questo scenario. Se sono necessari privilegi amministrativi, passare al supporto per workstation.
- **Supporto per workstation**: le attività includono la manutenzione delle workstation o la risoluzione dei problemi che richiede l'accesso a un sistema per la visualizzazione dei registri, l'installazione di software, l'aggiornamento dei driver e così via.
   - **Supporto per workstation da parte dell'Help Desk**: il personale di supporto di livello 2 si trova fisicamente vicino alla workstation dell'utente.
      - **Primario**: recuperare la password dell'account locale impostata da LAPS da una workstation amministrativa prima di connettersi alla workstation dell'utente.
      - **Non consentito**: l'accesso con le credenziali amministrative dell'account di dominio non è consentito in questo scenario.
   - **Supporto per workstation remoto**: il personale di supporto di livello 2 si trova fisicamente lontano rispetto alla workstation dell'utente.
      - **Primario**: usare RestrictedAdmin di RDP da una workstation amministrativa con un account di dominio che usa le autorizzazioni ottenute in tempo da una soluzione di Privileged Access Management.
      - **Secondario**: recuperare una password dell'account locale impostata da LAPS da una workstation amministrativa prima di connettersi alla workstation dell'utente.
      - **Non consentito**: usare il protocollo RDP standard con un account di dominio.

###### <a name="no-browsing-the-public-internet-with-admin-accounts-or-from-admin-workstations"></a>Nessuna ricerca sulle reti Internet pubbliche con gli account amministrativi o dalla workstation amministrativa

Il personale amministrativo non può navigare nelle reti Internet aperte mentre è connesso con un account amministrativo o a una workstation amministrativa. L'unica eccezione autorizzata riguarda l'uso di un Web browser per amministrare un servizio basato su cloud.

###### <a name="no-accessing-email-with-admin-accounts-or-from-admin-workstations"></a>Nessun accesso alla posta elettronica con account amministrativi o da workstation amministrative

Il personale amministrativo non può accedere alla posta elettronica mentre è connesso con un account amministrativo o a una workstation amministrativa.

###### <a name="store-service-and-application-account-passwords-in-a-secure-location"></a>Conservare le password del servizio e dell'applicazione in un luogo sicuro

Usare le seguenti linee guida per i processi di sicurezza fisici che controllano l'accesso alla password:

- Conservare le password degli account del servizio in una cassaforte fisica.
- Assicurarsi che solo il personale considerato attendibile nella classificazione del livello dell'account o al di sopra di essa abbia accesso alla password dell'account.
- Limitare al minimo il numero di persone che accedono alle password per garantire l'affidabilità.
- Assicurarsi che tutti gli accessi alla password siano registrati, tracciati e monitorati da un'entità non interessata, ad esempio un responsabile che non è in grado di eseguire l'amministrazione IT.

##### <a name="strong-authentication"></a>Autenticazione avanzata

Usare le procedure seguenti per configurare correttamente l'autenticazione avanzata.

###### <a name="enforce-smartcard-multi-factor-authentication-mfa-for-all-admin-accounts"></a>Applicare l'autenticazione a più fattori (MFA) alle smart card di tutti gli account amministrativi

Nessun account amministrativo può usare una password per l'autenticazione. Le uniche eccezioni autorizzate sono gli account di accesso di emergenza che sono protetti dai processi appropriati.

Collegare tutti gli account amministrativi a una smart card e abilitare l'attributo "**Per l'accesso interattivo occorre una smart card**".

Uno script deve essere implementato in modo da reimpostare automaticamente e periodicamente il valore hash della password casuale disabilitando e riabilitando immediatamente l'attributo "**Per l'accesso interattivo occorre una smart card**".

Non autorizzare eccezioni per gli account usati dal personale delle risorse umane se non gli account di accesso di emergenza.

###### <a name="enforce-multi-factor-authentication-for-all-cloud-admin-accounts"></a>Applicare l'autenticazione a più fattori (MFA) a tutti gli account amministrativi del cloud

Tutti gli account con privilegi di amministratore in un servizio cloud, ad esempio Microsoft Azure e Office 365, devono usare l'autenticazione a più fattori.

##### <a name="rare-use-emergency-procedures"></a>Procedure di emergenza usare raramente

Le procedure operative devono supportare gli standard seguenti:

- Verificare che le interruzioni possano essere risolte rapidamente.
- Verificare che le attività rare con privilegi elevati possano essere completate in base alle esigenze.
- Verificare che vengano usate procedure sicure per proteggere le credenziali e i privilegi.
- Verificare che i processi di rilevamento e di approvazione siano appropriati.

###### <a name="correctly-follow-appropriate-processes-for-all-emergency-access-accounts"></a>Seguire correttamente i processi appropriati per tutti gli account di accesso di emergenza

Assicurarsi che ogni account di accesso di emergenza disponga di un foglio di rilevamento nella cassaforte.

La procedura descritta nel foglio di rilevamento della password deve essere seguita per ogni account. Essa include la modifica della password dopo ogni uso e la disconnessione da qualsiasi workstation o server usati dopo il completamento.

L'uso degli account di accesso di emergenza deve essere approvato a priori dal comitato di approvazione delle modifiche, oppure dopo i fatti come un'emergenza approvata.

###### <a name="restrict-and-monitor-usage-of-emergency-access-accounts"></a>Limitare e monitorare l'uso di account di accesso di emergenza

Per l'uso di account di accesso di emergenza:

- Solo gli amministratori di dominio autorizzati possono accedere agli account di accesso di emergenza con privilegi di amministratore di dominio.
- Gli account di accesso di emergenza possono essere usati solo sui controller di dominio e altri host di livello 0.
- Questo account deve essere usato solo per:
  - Eseguire la risoluzione dei problemi e la correzione di problemi tecnici che impediscono l'uso degli account amministrativi corretti.
  - Eseguire attività rare, ad esempio:
    - Amministrazione dello schema
    - Attività a livello di foresta che richiedono privilegi amministrativi aziendali

      > [!NOTE]
      > La gestione della topologia, compresa la gestione del sito di Active Directory e della subnet, viene delegata in modo da limitare l'uso di questi privilegi.

- L'uso di uno di questi account è soggetto a un autorizzazione scritta dal responsabile del gruppo di sicurezza
- La procedura nel foglio di rilevamento per ogni account di accesso di emergenza richiede che la password venga modificata a ogni uso. Un membro del team di sicurezza deve convalidare l'avvenuta modifica.

###### <a name="temporarily-assign-enterprise-admin-and-schema-admin-membership"></a>Assegnare temporaneamente l'appartenenza a Enterprise Admin e Schema Admin

I privilegi devono essere aggiunti ove necessario e rimossi dopo l'uso. Questi privilegi devono essere assegnati all'account di emergenza per la sola durata dell'attività e comunque per non più di 10 ore. L'uso e la durata di questi privilegi devono essere documentati nel record del comitato di approvazione delle modifiche dopo il completamento dell'attività.

## <a name="esae-administrative-forest-design-approach"></a>Approccio per la progettazione della foresta amministrativa ESAE

Questa sezione contiene l'approccio per una foresta amministrativa basata sull'architettura di riferimento dell'Ambiente amministrativo di sicurezza avanzata (ESAE, Enhanced Security Administrative Environment), distribuita dai team di servizi professionali Microsoft per proteggere i clienti da attacchi alla sicurezza informatica.

Le foreste amministrative dedicate consentono alle organizzazioni di ospitare gli account amministrativi, le workstation e i gruppi in un ambiente che dispone di controlli di sicurezza più avanzati rispetto all'ambiente di produzione.

Questa architettura consente un numero di controlli di sicurezza che non sono possibili o sono difficilmente configurabili in un'architettura a foresta singola, anche in una gestita da sistemi PAW (workstation amministrative con privilegi). Questo approccio consente il provisioning degli account come utenti standard senza privilegi nella foresta amministrativa che hanno privilegi elevati nell'ambiente di produzione, consentendo maggiore imposizione tecnica della governance. Questa architettura consente inoltre l'utilizzo della funzionalità di autenticazione selettiva di un trust come mezzo per limitare gli accessi e l'esposizione delle credenziali solo a host autorizzati. In situazioni in cui si desidera un maggiore livello di sicurezza per la foresta di produzione senza maggiori costi e complessità di una ricompilazione completa, una foresta amministrativa può fornire un ambiente che consenta di aumentare il livello di garanzia dell'ambiente di produzione.

Sebbene questo approccio aggiunga una foresta in un ambiente Active Directory, i costi e la complessità vengono limitati dalla progettazione fissa, dalla memoria software/hardware limitata e dal numero ridotto di utenti.

> [!NOTE]
> Questo approccio garantisce buoni risultati per l'amministrazione di Active Directory, ma molte applicazioni non sono compatibili con il tipo di amministrazione da parte di account provenienti da una foresta esterna e che usa un trust standard.

La figura illustra una foresta ESAE usata per l'amministrazione delle risorse di livello 0 e una foresta PRIV configurata per l'uso con la funzionalità di Privileged Access Management di Microsoft Identity Manager. Per altre informazioni sulla distribuzione di un'istanza PAM di MIM, vedere l'articolo [Privileged Access Management per Servizi di dominio Active Directory](https://technet.microsoft.com/library/mt150258.aspx).

![Figura che illustra una foresta ESAE usata per l'amministrazione delle risorse di livello 0 e una foresta PRIV configurata per l'uso con la funzionalità di Privileged Access Management di Microsoft Identity Manager](../media/securing-privileged-access-reference-material/PAW_RM_Fig14.JPG)

Una foresta amministrativa dedicata è una foresta Active Directory di un singolo dominio standard dedicata alla funzione di gestione di Active Directory. Le foreste amministrative e i domini possono essere protetti più rigorosamente delle foreste di produzione poichè vengono utilizzati in un minor numero di casi.

Durante la progettazione di una foresta amministrativa è necessario tenere in considerazione gli elementi seguenti:

- **Ambito limitato**: il valore primario di una foresta amministrativa è l'elevato livello di garanzia della sicurezza e la superficie di attacco ridotta che comporta un rischio residuo inferiore. La foresta può essere utilizzata per ospitare applicazioni e funzioni di gestione aggiuntive, ma ogni incremento nell'ambito aumenta la superficie di attacco della foresta e delle relative risorse. L'obiettivo consiste nel limitare le funzioni degli utenti all'interno della foresta e dell’amministrazione per mantenere la superficie di attacco minima, pertanto è consigliabile considerare attentamente ogni incremento dell'ambito.
- **Configurazioni del trust**: configurare il trust dalle foreste gestite o domini alla foresta amministrativa
   - Un trust unidirezionale viene richiesto dall'ambiente di produzione alla foresta amministrativa. Può trattarsi di un trust di dominio o un trust tra foreste. Non è necessario che il dominio/la foresta amministrativa consideri attendibili i domini/le foreste gestite per gestire Active Directory, anche se altre applicazioni possono richiedere una relazione di trust bidirezionale, una convalida di sicurezza e un test.
   - L’autenticazione selettiva dovrebbe essere utilizzata per limitare l’accesso degli account nella foresta amministrativa solo agli host di produzione appropriati. Per mantenere i controller di dominio e delegare i diritti in Active Directory, è necessaria in genere la concessione del diritto "Accesso consentito" ai controller di dominio per account amministrativi designati di livello 0 nella foresta amministrativa. Vedere Configuring Selective Authentication Settings (Configurazione delle impostazioni di autenticazione selettiva) per altre informazioni.
- **Privilegi e protezione avanzata del dominio**: la foresta amministrativa deve essere configurata per privilegi minimi, in base ai requisiti per l'amministrazione di Active Directory.
   - Per concedere i diritti per amministrare i controller di dominio e delegare le autorizzazioni è necessario aggiungere account amministrativi della foresta al gruppo locale di dominio BUILTIN\Administrators. Ciò è dettato dal fatto che il gruppo globale Domain Admins non può avere membri da un dominio esterno.
   - Un'avvertenza all'uso di questo gruppo per concedere diritti è che non si potrà avere accesso amministrativo ai nuovi oggetti Criteri di gruppo per impostazione predefinita. Tuttavia, seguendo la procedura descritta in [questo articolo della knowledge base](https://support.microsoft.com/kb/321476) è possibile modificare le autorizzazioni predefinite dello schema.
   - Gli account nella foresta amministrativa utilizzati per amministrare l'ambiente di produzione non devono avere privilegi amministrativi per la foresta amministrativa, i domini o le workstation in essa contenuti.
   - I privilegi amministrativi sulla foresta amministrativa devono essere controllati attentamente da un processo offline per ridurre la possibilità che un utente o dipendente malintenzionato cancelli i registri di controllo. Ciò consente di garantire che il personale con gli account amministrativi di produzione non possano ridurre le restrizioni per i loro account e aumentare i rischi per l'organizzazione.
   - La foresta amministrativa deve seguire le configurazioni di Microsoft Security Compliance Baseline (SCB) per il dominio, tra cui configurazioni complesse per i protocolli di autenticazione.
   - Tutti gli host della foresta amministrativa devono essere aggiornati automaticamente con gli aggiornamenti della sicurezza. Anche se ciò può comportare il rischio di interrompere le operazioni di manutenzione del controller di dominio, fornisce una mitigazione significativa dei rischi di sicurezza delle vulnerabilità senza patch.

      > [!NOTE]
      > È possibile configurare un'istanza dedicata di Windows Server Update Services per approvare automaticamente gli aggiornamenti. Per altre informazioni, vedere la sezione "Approvare automaticamente gli aggiornamenti per l'installazione" in Approvazione degli aggiornamenti.

- **Protezione avanzata della workstation**: compilare le workstation amministrative mediante le [workstation amministrative con privilegi](../securing-privileged-access/privileged-access-workstations.md) (fase 3), ma modificare l'appartenenza al dominio alla foresta amministrativa anziché all'ambiente di produzione.
- **Protezione avanzata dei controller di dominio e dei server**: per tutti i controller di dominio e i server nella foresta amministrativa:
   - Verificare che tutti i supporti siano convalidati sulla base delle indicazioni riportate in [Origine pulita per i supporti di installazione](http://aka.ms/cleansource)
   - Assicurarsi che i server della foresta amministrativa presentino i sistemi operativi più recenti, anche se questo non è fattibile nell'ambiente di produzione.
   - Gli host della foresta amministrativa devono essere aggiornati automaticamente con gli aggiornamenti della sicurezza.

      > [!NOTE]
      > Windows Server Update Services può essere configurato per approvare automaticamente gli aggiornamenti. Per altre informazioni, vedere la sezione "Approvare automaticamente gli aggiornamenti per l'installazione" in Approvazione degli aggiornamenti.

   - Gli Standard di sicurezza devono essere usati come configurazioni di avvio.

      > [!NOTE]
      > I clienti possono usare Microsoft Security Compliance Toolkit (SCT) per la configurazione degli standard sugli host di amministrazione.

   - Avvio protetto per impedire agli utenti malintenzionati o ai malware di caricare un codice non firmato nel processo di avvio.

      > [!NOTE]
      > Questa funzionalità è stata introdotta in Windows 8 per sfruttare la Unified Extensible Firmware Interface (UEFI).

   - Crittografia dell'intero volume per ridurre i rischi di perdita fisica del computer, ad esempio portatili amministrativi usati in modalità remota.

      > [!NOTE]
      > Per altre informazioni, vedere [BitLocker](https://technet.microsoft.com/library/dn641993.aspx).

   - Restrizioni USB per proteggere da vettori di infezioni fisici.

      > [!NOTE]
      > Per altre informazioni, vedere [Control Read or Write Access to Removable Devices or Media](https://technet.microsoft.com/library/cc730808(v=ws.10).aspx) (Controllo dell'accesso in lettura o in scrittura ai dispositivi o supporti rimovibili).

   - Isolamento rete per proteggere da attacchi di rete e azioni di amministrazione accidentali. I firewall host dovrebbero bloccare tutte le connessioni in ingresso tranne quelle esplicitamente necessarie e bloccare completamente l'accesso Internet in uscita.

   - Antimalware per proteggere da malware e minacce note.

   - Analisi della superficie di attacco per impedire l'introduzione di nuovi vettori di attacchi a Windows durante l'installazione del nuovo software.

      > [!NOTE]
      > L'uso di strumenti, ad esempio l'[analizzatore della superficie di attacco (ASA)](https://www.microsoft.com/en-us/download/details.aspx?id=24487) consente di valutare le impostazioni di configurazione in un host e identificare i vettori degli attacchi dovuti a modifiche software o alla configurazione.

- Protezione avanzata dell'account
   - L'autenticazione a più fattori deve essere configurata per tutti gli account della foresta di amministrazione, ad eccezione di un account. Almeno un account amministrativo deve essere basato su password per garantire l'accesso anche nel caso in cui il processo di autenticazione a più fattori di elaborare le interruzioni si interrompe. Questo account deve essere protetto da un processo di controllo fisico rigoroso.
   - Gli account configurati per l'autenticazione a più fattori devono essere configurati in modo da impostare regolarmente un nuovo hash NTLM sugli account. A questo scopo, disabilitare e abilitare l'attributo dell'account Per l'accesso interattivo occorre una smart card.

      > [!NOTE]
      > Questa azione può interrompere le operazioni in corso che usano questo account; pertanto, il processo deve essere avviato solo quando gli amministratori non devono usare l'account, ad esempio durante la notte o nei fine settimana.

- Controlli detective
   - I controlli detective per la foresta amministrativa devono essere progettati per segnalare eventuali anomalie nella foresta amministrativa. Il numero limitato di scenari autorizzati e attività consente di ottimizzare questi controlli in modo più accurato rispetto all'ambiente di produzione.

Per altre informazioni sui servizi Microsoft per la progettazione e la distribuzione di un ESAE per l'ambiente, vedere [questa pagina](https://www.microsoft.com/services).

## <a name="tier-0-equivalency"></a>Equivalenza di livello 0

La maggior parte delle organizzazioni di controlla l'appartenenza ai gruppi di Active Directory di livello 0 come Administrators, Domain Admins ed Enterprise Admins.  Molte organizzazioni trascurano il rischio di altri gruppi con privilegi equivalenti in un ambiente tipico di Active Directory. Questi gruppi offrono agli utenti malintenzionati un percorso di riassegnazione relativamente facile agli stessi privilegi di livello 0 espliciti usando vari metodi di attacco.

Ad esempio, un operatore di server può accedere a un supporto di backup di un controller di dominio ed estrarre tutte le credenziali dai file del supporto, per poi usarle per riassegnare i privilegi.

Le organizzazioni devono controllare e monitorare l'appartenenza a tutti i gruppi di livello 0 (inclusa l'appartenenza nidificata) tra cui:

- Enterprise Admins
- Domain Admins
- Schema Admin
- BUILTIN\Administrators
- Account Operators
- Backup Operators
- Print Operators
- Server Operators
- Controller di dominio
- Controller di dominio di sola lettura
- Proprietari autori criteri di gruppo
- Operazioni di crittografia
- Utenti DCOM
- Altri gruppi delegati: gruppi personalizzati che possono essere creati dall'organizzazione per gestire le operazioni di directory e che possono avere anche accesso al livello 0.

## <a name="administrative-tools-and-logon-types"></a>Strumenti di amministrazione e tipi di accesso

Le seguenti sono informazioni di riferimento per identificare il rischio di esposizione delle credenziali associato all'uso di diversi strumenti di amministrazione per l'amministrazione remota.

In uno scenario di amministrazione remota, le credenziali sono sempre esposte nel computer di origine. Pertanto, è sempre consigliabile associare una workstation amministrativa attendibile con privilegi (PAW) agli account sensibili o ad alto impatto. L'esposizione delle credenziali al rischio di furto potenziale sul computer (remoto) di destinazione dipende principalmente dal tipo di accesso a Windows usato dal metodo di connessione.

Questa tabella include istruzioni per gli strumenti di amministrazione e i metodi di connessione più comuni:

|Metodo di connessione|Tipo di accesso|Credenziali riusabili nella destinazione|Commenti|
|-----------|-------|--------------------|------|
|Accedere alla console|Interactive (Interattivo)|v|Include l'accesso remoto all'hardware/schede Lights-Out e KVMs di rete.|
|RUNAS|Interactive (Interattivo)|v||
|RUNAS/NETWORK|NewCredentials|v|Clona la sessione LSA corrente per l'accesso locale, ma usa le nuove credenziali quando ci si connette alle risorse di rete.|
|Desktop remoto (esito positivo)|RemoteInteractive|v|Se il client Desktop remoto è configurato per la condivisione di dispositivi e risorse locali, anche questi possono risultare compromessi.|
|Desktop remoto (errore: tipo di accesso negato)|RemoteInteractive|-|Per impostazione predefinita, se si verifica un errore durante l'accesso RDP le credenziali vengono archiviate solo brevemente. Ciò potrebbe non valere se il computer viene compromesso.|
|Net use * \\\SERVER|Network|-||
|Net use * \\\SERVER /u:utente|Network|-||
|Snap-in MMC al computer remoto|Network|-|Esempio: gestione computer, visualizzatore eventi, dispositivo di gestione, servizi|
|PowerShell WinRM|Network|-|Esempio: server Enter-PSSession|
|PowerShell WinRM con CredSSP|NetworkClearText|v|Server New-PSSession<br />-Credssp di autenticazione<br />-Credenziale Credential|
|PsExec senza credenziali esplicite|Network|-|Esempio: PsExec \\\server cmd|
|PsExec con credenziali esplicite|Rete + interattivo|v|PsExec \\\server -u utente -p pwd cmd<br />Crea più sessioni di accesso.|
|Registro di sistema remoto|Network|-||
|Gateway Desktop remoto|Network|-|Autenticazione a Gateway Desktop remoto.|
|Attività pianificata|Batch|v|La password verrà salvata anche come segreto LSA sul disco.|
|Eseguire gli strumenti come un servizio|Servizio|v|La password verrà salvata anche come segreto LSA sul disco.|
|Scanner delle vulnerabilità|Network|-|Per impostazione predefinita la maggior parte degli scanner usa gli accessi alla rete, anche se alcuni fornitori possono implementare gli accessi non alla rete e introdurre ulteriori rischi di furto delle credenziali.|

Per l'autenticazione Web, usare il riferimento dalla tabella riportata di seguito:

|Metodo di connessione|Tipo di accesso|Credenziali riusabili nella destinazione|Commenti|
|-----------|-------|--------------------|------|
|"Autenticazione di base" di IIS|NetworkClearText<br />(IIS 6.0+)<br /><br />Interactive (Interattivo)<br />(precedente a IIS 6.0)|v||
|"Autenticazione integrata di Windows" di IIS|Network|-|Provider Kerberos e NTLM.|

Definizioni delle colonne:

- **Tipo di accesso** identifica il tipo di accesso avviato dalla connessione.
- **Credenziali riusabili nella destinazione** indica che i seguenti tipi di credenziali verranno archiviati nella memoria del processo LSASS nel computer di destinazione in cui l'account specificato è connesso in locale:
   - Hash LM e NT
   - TGT di Kerberos
   - Password non crittografata (se applicabile).

I simboli in questa tabella sono definiti come segue:

- (-) indica quando le credenziali non sono esposte.
- (-) indica quando le credenziali sono esposte.

Per le applicazioni di gestione che non sono presenti in questa tabella, è possibile determinare il tipo di accesso nel campo del tipo di accesso in Controlla eventi di accesso. Per altre informazioni, vedere [Controlla eventi di accesso](https://technet.microsoft.com/library/cc787567(v=ws.10).aspx).

In computer basati su Windows, tutte le autenticazioni vengono elaborate come uno dei diversi tipi di accesso, indipendentemente dal protocollo di autenticazione o autenticatore usato. Questa tabella include i tipi di accesso più comuni e i relativi attributi rispetto al furto di credenziali:

|Tipo di accesso|#|Autenticatori accettati|Credenziali riusabili nella sessione LSA|Esempi|
|-------|---|--------------|--------------------|------|
|Interattivo (anche noto come accesso locale)|2|Password, smart card,<br />altro|Sì|Accesso alla console;<br />RUNAS;<br />Soluzioni di controllo remoto dell'hardware (ad esempio KVM di rete o accesso remoto/scheda Lights-Out nel server)<br />Autenticazione di base di IIS (prima di IIS 6.0)|
|Network|3|Password,<br />hash NT,<br />ticket Kerberos|No (eccetto se è abilitata la delega, quindi il ticket Kerberos è presente)|NET USE;<br />Chiamate RPC;<br />Registro di sistema remoto;<br />Autenticazione integrata di Windows di IIS;<br />Autenticazione di Windows SQL;|
|Batch|4|Password (in genere archiviata come segreto LSA)|Sì|Attività pianificate|
|Servizio|5|Password (in genere archiviata come segreto LSA)|Sì|Servizi Windows|
|NetworkClearText|8|Password|Sì|Autenticazione di base di IIS (IIS 6.0 e versioni successive);<br />Windows PowerShell con CredSSP|
|NewCredentials|9|Password|Sì|RUNAS/NETWORK|
|RemoteInteractive|10|Password, smart card,<br />altro|Sì|Desktop remoto (precedentemente noto come "Servizi Terminal")|

Definizioni delle colonne:

- **Tipo di accesso** è il tipo di accesso richiesto.
- **#** è l'identificatore numerico per il tipo di accesso che viene segnalato negli eventi di controllo nel registro eventi di sicurezza.
- **Autenticatori accettati** indica quali tipi di autenticatori sono in grado di avviare un accesso di questo tipo.
- **Credenziali riusabili** in una sessione LSA indica se il tipo di accesso risulta nella sessione LSA che contiene le credenziali, ad esempio le password in non crittografate, gli hash NT o i ticket Kerberos che potrebbero essere usati per eseguire l'autenticazione ad altre risorse di rete.
- In **Esempi** vengono elencati scenari comuni in cui viene usato il tipo di accesso.

> [!NOTE]
> Per altre informazioni sui tipi di accesso, vedere [SECURITY_LOGON_TYPE enumeration](https://technet.microsoft.com/library/aa380129(VS.85).aspx) (Enumerazione SECURITY_LOGON_TYPE).
