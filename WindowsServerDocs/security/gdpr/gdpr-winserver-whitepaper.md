---
title: Applicazione del Regolamento generale sulla protezione dei dati (RGPD) per Windows Server 2016
description: Questo articolo ti consente di comprendere che cosa è l'RGPD e di conoscere i prodotti che Microsoft fornisce per introdurti alla conformità.
keywords: privacy, RGPD
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.technology:
- techgroup-security
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 09/25/2017
ms.author: nirb
ms.openlocfilehash: be9509de0291924bb95733f995b447230bb75214
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870132"
---
# <a name="beginning-your-general-data-protection-regulation-gdpr-journey-for-windows-server"></a>Inizia il tuo percorso di protezione del regolamento GDPR (General Data) per Windows Server 

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Questo articolo fornisce informazioni sul regolamento RGPD, tra cui la sua definizione e i prodotti che Microsoft offre per favorire la conformità.

## <a name="introduction"></a>Introduzione
Il 25 maggio 2018 entrerà in vigore una legge europea sulla privacy che stabilisce un nuovo standard globale per i diritti di privacy, sicurezza e conformità.

Il regolamento generale sulla protezione dei dati o RGPD serve fondamentalmente a proteggere e consentire i diritti di privacy degli utenti. L'RGPD stabilisce severi requisiti globali sulla privacy che disciplinano il modo in cui gestire e proteggere i dati personali rispettando la scelta singola, indipendentemente da dove vengano inviati, trattati o archiviati i dati.

Microsoft e i nostri clienti sono ora in viaggio per conseguire gli obiettivi di privacy dell'RGPD. In Microsoft siamo convinti che la privacy sia un diritto fondamentale e riteniamo che l'RGPD sia un importante passo avanti per chiarire e consentire i diritti di privacy dei singoli. Ci rendiamo anche conto che l'RGPD richiederà modifiche significative da parte delle organizzazioni di tutto il mondo.

Abbiamo illustrato il nostro impegno per l'RGPD e come supportiamo i nostri clienti nel post del blog [Get GDPR compliant with the Microsoft Cloud](https://blogs.microsoft.com/on-the-issues/2017/02/15/get-gdpr-compliant-with-the-microsoft-cloud/#hv52B68OZTwhUj2c.99) del nostro Chief Privacy Officer [Brendon Lynch](https://blogs.microsoft.com/on-the-issues/author/brendonlynch/) e nel post del blog [Earning your trust with contractual commitments to the General Data Protection Regulation](https://blogs.microsoft.com/on-the-issues/2017/04/17/earning-trust-contractual-commitments-general-data-protection-regulation/#6QbqoGWXCLavGM63.99)"di [Rich Sauer](https://blogs.microsoft.com/on-the-issues/author/rsauer/), Corporate Vice President e Deputy General Counsel di Microsoft.

Sebbene il viaggio verso la conformità all'RGPD possa sembrare complesso, abbiamo deciso di aiutarti. Per informazioni specifiche sull'RGPD, i nostri impegni e su come iniziare il viaggio, visita la [sezione RGPD del Microsoft Trust Center](https://www.microsoft.com/en-us/trustcenter/privacy/gdpr).

## <a name="gdpr-and-its-implications"></a>RGPD e le relative implicazioni
L'RGPD è un regolamento complesso che potrebbe richiedere modifiche significative nella modalità di raccolta, trattamento e gestione dei dati personali. Microsoft ha una lunga storia per quanto riguarda l'offerta di aiuto ai clienti nel conformarsi alle normative complesse e quando si tratta di preparazione per l'RGPD, siamo partner in questo viaggio.

L'RGPD impone regole alle organizzazioni che offrono prodotti e servizi a coloro che risiedono nell'Unione europea (UE) o che raccolgono e analizzano dati vincolati ai residenti nell'Unione europea, indipendentemente da dove si trovino tali aziende. Di seguito sono indicati alcuni degli elementi principali dell'RGPD:

- **Diritti di privacy personali avanzata.** Aumentare la protezione dei dati per i residenti dell'Unione europea assicurando che abbiano il diritto di accedere ai loro dati personali per correggere imprecisioni, cancellarli, rifiutare il loro trattamento e trasferirli.

- **Maggiore lavoro per la protezione dei dati personali.** Rafforzata la responsabilità delle organizzazioni che trattano i dati personali, fornendo una maggiore chiarezza di responsabilità nel garantire la conformità.

- **Dati personali obbligatorio violazione della creazione di report.** Le organizzazioni che controllano i dati personali sono obbligate a segnalare tempestivamente alle autorità di controllo violazioni dei dati personali che rappresentino un rischio per i diritti e le libertà delle persone e, laddove possibile, non oltre 72 ore dal momento in cui vengono a conoscenza della violazione.

Come è prevedibile, l'RGPD può avere un impatto significativo sull'azienda, obbligando di fatto ad aggiornare le informative sulla privacy, a implementare e rafforzare i controlli di protezione dei dati e le procedure di notifica delle violazioni, a implementare criteri di elevata trasparenza e a investire ulteriormente nell'IT e nella formazione. Microsoft Windows 10 può aiutarti in modo efficace ed efficiente a soddisfare alcuni di questi requisiti.

## <a name="personal-and-sensitive-data"></a>Dati personali e sensibili
Nell'ambito dell'impegno per la conformità all'RGPD dovrai comprendere come il regolamento definisce i dati personali e sensibili e come tali definizioni siano correlate ai dati conservati dalla tua organizzazione. Base che understanding che sarà in grado di individuare in cui viene creati che i dati, elaborazione, gestiti e archiviati.

L'RGPD considera come dati personali qualsiasi dato relativo a una persona naturale identificata o identificabile. Ciò può includere sia l'identificazione diretta (ad esempio, il nome legale) sia l'identificazione indiretta (ad esempio, dati specifici che identifichino chiaramente la persona cui fanno riferimento). L'RGPD chiarisce inoltre che il concetto di dati personali include gli identificatori online (ad esempio, gli indirizzi IP, gli ID del dispositivo mobile) e i dati sulla posizione.

L'RGPD introduce le definizioni specifiche di dati genetici (ad esempio, la sequenza genetica di una persona) e di dati biometrici. I dati genetici e i dati biometrici insieme ad altre categorie secondarie di dati personali (dati personali che rivelano l'origine razziale o etnica, le opinioni politiche, religiose o filosofiche o l'appartenenza a sindacati: dati relativi alla salute o dati relativi alla vita o all'orientamento sessuale di una persona) sono considerati dati personali sensibili ai sensi dell'RGPD. I dati personali sensibili sono sottoposti a protezioni avanzate e in genere richiedono il consenso esplicito di una persona laddove devono essere trattati.

### <a name="examples-of-info-relating-to-an-identified-or-identifiable-natural-person-data-subject"></a>Esempi di dati relativi a una persona naturale identificata o identificabile (soggetto titolare di dati)
In questo elenco vengono forniti esempi di diversi tipi di dati che saranno regolati tramite l'RGPD. Non è un elenco esaustivo.

-   Nome

-   Numero di identificazione (ad esempio, CF)

-   Dati sulla posizione (ad esempio, indirizzo di casa)

-   Identificatore online (ad esempio, indirizzo di posta elettronica, nomi visualizzati, l'indirizzo IP, ID del dispositivo)

-   Pseudonimi (ad esempio, l'utilizzo di un codice per identificare le persone)

-   Dati genetici (ad esempio, campioni biologici di una persona)

-   Dati biometrici (ad esempio, le impronte digitali, il riconoscimento facciale)

## <a name="getting-started-on-the-journey-towards-gdpr-compliance"></a>Guida introduttiva sul viaggio verso la conformità RGPD
Una volta specificato quanto sia necessario per diventare compatibili con l'RGPD, ti consigliamo di non aspettare che la compatibilità venga imposta. È opportuno che tu esamini ora le procedure relative alla privacy e alla gestione dei dati. Ti consigliamo di iniziare il viaggio verso la conformità all'RGPD concentrandoti su quattro fasi principali:

-   **Informazioni su.** Identifica i dati personali in tuo possesso e dove si trovano. 

-   **Consente di gestire.** Disciplina la modalità di trattamento e accessibilità dei dati personali.

-   **Proteggere.** Stabilisci controlli di sicurezza per impedire, rilevare vulnerabilità e violazioni dei dati e rispondere ad esse.  

-   **Report.** Agisci sulle richieste di dati, segnala violazioni dei dati e conserva la documentazione richiesta.

    ![Grafico sull'interazione delle 4 fasi RGPD chiave](../media/GDPR-Windows-Server-Overview/gdpr-steps-diagram.png)

Per ognuna delle fasi abbiamo descritto gli strumenti di esempio, le risorse e le funzionalità in varie soluzioni Microsoft, che possono essere utilizzate per aiutarti a soddisfare i requisiti di quella fase. Sebbene questo articolo non è una guida completa "procedura", sono stati inclusi i collegamenti che consentono di ottenere ulteriori informazioni dettagliate e altre informazioni sono disponibili nel [sezione GDPR di Microsoft Trust Center](https://www.microsoft.com/en-us/trustcenter/privacy/gdpr).

## <a name="windows-server-security-and-privacy"></a>Privacy e sicurezza di Windows Server
Il regolamento GDPR è necessario implementare misure di sicurezza tecniche e organizzative appropriate per proteggere i dati personali e i sistemi di elaborazione. Nel contesto del GDPR, gli ambienti di server fisici e virtuali potenzialmente elaborano i dati personali e sensibili. L'elaborazione può significare qualsiasi operazione o un set di operazioni, ad esempio la raccolta dei dati, archiviazione e recupero.

La capacità di soddisfare questo requisito e implementare misure di sicurezza tecniche appropriate deve riflettere le minacce che devi affrontare in ambiente IT numero crescente di minacce attuali. Il panorama attuale delle minacce alla sicurezza mostra minacce aggressive e resistenti. Negli anni precedenti gli autori degli attacchi dannosi erano principalmente alla ricerca del riconoscimento da parte della community degli attacchi o del brivido di una disconnessione temporanea del sistema. Da allora le motivazioni dell'utente malintenzionato sono cambiate per includere la possibilità di un guadagno, compresa la presa in ostaggio di dispositivi e dati fino a quando il proprietario non paghi il ransom richiesto.

Gli attacchi moderni si concentrano sempre più sul furto della proprietà intellettuale su larga scala, sulla riduzione delle prestazioni del sistema di destinazione che può comportare perdite finanziarie e ora anche sul terrorismo informatico che minaccia la sicurezza delle persone, delle aziende e degli interessi nazionali a livello mondiale. Questi attacchi sono in genere progettati da persone altamente specializzate ed esperti in sicurezza, alcune delle quali sono alle dipendente di stati-nazione con grandi budget e risorse umane apparentemente illimitate. Minacce come queste richiedono un approccio all'altezza della sfida.

Non solo queste minacce sono un rischio per la possibilità di mantenere il controllo dei propri dati personali o sensibili, ma sono anche un rischio serio per le aziende in generale. Prendere in considerazione i dati recenti dalla rivista McKinsey, Ponemon Institute, Verizon e Microsoft:

- Il costo medio del tipo di violazione dei dati previsto dall'RGPD è di 3,5 milioni di dollari.

- Il 63% di tali violazioni riguarda password deboli o rubate, che l'RGPD prevede sia tu a risolvere.

- Più di 300.000 nuovi esempi di malware vengono creati e distribuiti ogni giorno per rendere ancora più difficile la tua attività di protezione dei dati.

Come accade con gli attacchi Ransomware recenti, una volta chiamati il nero tormenta di Internet, gli utenti malintenzionati stanno dopo le destinazioni più grande che possono permettersi di pagare di più, con conseguenze potenzialmente irreversibile. Il regolamento GDPR include sanzioni che rendono i sistemi, tra cui computer desktop e portatili, che contengono effettivamente destinazioni avanzate dei dati personali e sensibili.

Due principi fondamentali sono diventate e continuano a guidare lo sviluppo di Windows:

- **Sicurezza.** I dati che software e dei servizi di archiviano per conto dei nostri clienti devono essere protetta da danni e usati o modificare solo nel modo appropriato. Modelli di sicurezza dovrebbero essere facili per gli sviluppatori a comprendere e creare nelle proprie applicazioni.

- **Privacy.** Gli utenti devono essere in controllo dell'utilizzo dei dati. I criteri per l'utilizzo di informazioni devono essere chiaro per l'utente. Utenti devono essere controllare quando e se ricevere le informazioni per usare meglio del tempo. Dovrebbe essere facile agli utenti di specificare l'utilizzo appropriato delle loro informazioni tra cui controllo dell'utilizzo di posta elettronica vengono inviati.

Microsoft è rimasto in assenza contro questi principi come indicati da Microsoft Satya Nadella, CEO, 

> "_Poiché tutto il mondo in continua evoluzione ed evolversi delle esigenze aziendali, alcuni elementi sono coerenti: richiesta del cliente per la sicurezza e privacy._ "

Mentre lavora per assicurare la conformità con GDPR, informazioni sul ruolo dei server fisici e virtuali nella creazione, l'accesso, l'elaborazione, l'archiviazione e la gestione dei dati che può essere considerata idonea come personali e dati potenzialmente sensibili ambito del regolamento GDPR sono importanti. Windows Server fornisce funzionalità che consentono di soddisfare i requisiti GDPR per implementare misure di sicurezza tecniche e organizzative appropriate per proteggere i dati personali.

Le condizioni di sicurezza di Windows Server 2016 non sono un bolt-on. è un principio architetturale. Inoltre, possono essere riconosciuta meglio nella quattro entità:

- **Proteggere.** Lo stato attivo in corso e l'innovazione su misure preventive; bloccare tutti i malware conosciuti e attacchi noti.

- **Rilevare.** Gli strumenti che consentono di individuare eventuali anomalie e rispondere agli attacchi più veloce di monitoraggio completo.

- **Rispondere.** Principali tecnologie di risposta e il ripristino, oltre a avanzato le competenze di consulenza.

- **Isolare.** Isolare i segreti di componenti e i dati di sistema operativo, limitare i privilegi di amministratore e rigorosamente misurare l'integrità di host.

Con Windows Server, la possibilità di proteggere, rilevare e difesa dai tipi di attacchi che possono provocare violazioni dei dati risulta notevolmente migliorata. Dati i rigidi requisiti nell'RGPD riguardanti la notifica delle violazioni, un'ottima difesa dei sistemi desktop e portatili riduce i rischi che dovrai affrontare e che potrebbero comportare costose analisi e notifiche delle violazioni.

Nella sezione che segue, verrà visualizzato come Windows Server fornisce le funzionalità di base mai interrompersi nella fase "Protezione" del raggiungimento della conformità al GDPR. Queste funzionalità sono suddivisi in tre scenari di protezione:

- **Proteggere le credenziali e limitare i privilegi di amministratore.** Windows Server 2016 consente di implementare queste modifiche, per evitare che il sistema viene utilizzata come un punto di avvio per un'ulteriore intrusioni.

- **Proteggere il sistema operativo per eseguire le App e infrastruttura.** Windows Server 2016 offre livelli di protezione, che consente di bloccare attacchi esterni da software dannoso è in esecuzione o che sfruttano le vulnerabilità.

- **Virtualizzazione sicura.** Windows Server 2016 consente una virtualizzazione sicura, l'utilizzo di infrastruttura protetta e macchine virtuali schermate. Ciò consente di crittografare ed eseguire le macchine virtuali in host attendibili nell'infrastruttura, una migliore protezione da attacchi dannosi.

Queste funzionalità, descritti in dettaglio di seguito con i riferimenti a specifici requisiti RGPD, sono basate sulla protezione avanzata dei dispositivi che consente di mantenere l'integrità e sicurezza del sistema operativo e dati.

Il provisioning chiave entro il regolamento GDPR è la protezione dei dati per impostazione predefinita e per impostazione predefinita e con la possibilità di soddisfare tale disposizione per aiutarti a sono funzionalità all'interno di Windows 10, ad esempio crittografia dispositivo BitLocker. BitLocker utilizza la tecnologia Trusted Platform Module (TPM), che offre funzioni correlate alla sicurezza basate su hardware. Questa crittografia-processore chip include diversi meccanismi di sicurezza fisica per renderlo di manomissione e software dannoso è in grado di alterare le funzioni di protezione del TPM.

Il chip include più meccanismi di sicurezza fisica in grado di proteggerlo da manomissioni; il software dannoso non sarà pertanto in grado di manomettere le funzioni di sicurezza del TPM. Alcuni dei principali vantaggi dell'uso della tecnologia TPM sono elencati di seguito:

-   Generare, archiviare e limitare l'uso di chiavi di crittografia.

-   Usare la tecnologia TPM per l'autenticazione dei dispositivi della piattaforma usando la chiave RSA univoca del TPM, che è integrata al suo interno.

-   Garantire l'integrità della piattaforma mediante l'analisi e l'archiviazione delle misure di sicurezza.

La protezione aggiuntiva del dispositivo avanzato, importante per l'esecuzione senza violazioni di dati, include l'avvio sicuro di Windows che consente di garantire l'integrità del sistema verificando che il malware non sia in grado di avviarsi prima delle difese di sistema.

## <a name="windows-server-supporting-your-gdpr-compliance-journey"></a>Windows Server: Supporto del raggiungimento della conformità al GDPR
Le funzionalità principali all'interno di Windows Server consente di implementare in modo efficiente ed efficace i meccanismi di sicurezza e privacy per che il regolamento GDPR richiede la conformità. Durante l'uso di queste funzionalità non garantisce la conformità, supporteranno l'impegno a questo scopo.

Il sistema operativo server si trova a un livello strategico nell'infrastruttura di un'organizzazione, offrendo una nuove opportunità per creare livelli di protezione da attacchi in grado di rubare i dati e di interrompere l'azienda. Gli aspetti chiave del GDPR, ad esempio Privacy by Design, protezione dei dati e controllo di accesso devono essere risolti all'interno dell'infrastruttura IT a livello di server.

Lavorando per aiutare a proteggere le identità, sistema operativo e i livelli di virtualizzazione, Windows Server 2016 consente di bloccare i vettori di attacco comune usati per accedere illecitamente ai tuoi sistemi: furto di credenziali, malware e un'infrastruttura di virtualizzazione compromesso. Oltre a ridurre il rischio di business, i componenti di sicurezza integrate di Guida di Windows Server 2016 i requisiti di conformità di indirizzo per la chiave al settore pubblico e alle normative di sicurezza. 

Queste identità, sistema operativo e le protezioni di virtualizzazione consentono di proteggere meglio il tuo Data Center che esegue Windows Server come macchina virtuale in qualsiasi cloud e limitare la possibilità agli utenti malintenzionati di compromettere le credenziali, avviare malware e rimarrà non rilevato di rete. Analogamente, se distribuito come un host Hyper-V, Windows Server 2016 offre la garanzia di sicurezza per gli ambienti di virtualizzazione tramite le macchine virtuali schermate e funzionalità di firewall distribuito. Con Windows Server 2016, il sistema operativo server diventa un partecipante attivo nella protezione del Data Center.

### <a name="protect-your-credentials-and-limit-administrator-privileges"></a>Proteggere le credenziali e limitare i privilegi di amministratore 
Controllare l'accesso ai dati personali e i sistemi che elaborano i dati, è un'area con gli standard GDPR che presenta requisiti specifici, incluso l'accesso da parte degli amministratori. Le identità con privilegi sono gli account con privilegi, ad esempio gli account utente che sono membri del Domain Administrators, gli amministratori dell'organizzazione, gli amministratori locali o anche i gruppi di utenti di alimentazione elevate. Tali identità possono anche includere gli account che sono stati concessi privilegi direttamente, ad esempio l'esecuzione di backup, in corso l'arresto del sistema o altri diritti elencati nel nodo dell'assegnazione diritti utente nella console Criteri di sicurezza locali.

Come principio di controllo di accesso generale e in linea con gli standard GDPR, è necessario proteggere le identità con privilegi contro le violazioni da potenziali utenti malintenzionati. In primo luogo, è importante comprendere come le identità sono compromessi; è quindi possibile pianificare impedire agli utenti malintenzionati di accedere a queste identità con privilegi.

#### <a name="how-do-privileged-identities-get-compromised"></a>Come compromesse le identità con privilegi?
Le identità con privilegi possono ottenere compromessi quando le organizzazioni non hanno le linee guida per la protezione. Di seguito vengono riportati alcuni esempi:

- **Più privilegi rispetto al necessario.** Uno dei problemi più comuni è che gli utenti hanno più privilegi rispetto al necessario per eseguire la propria funzione di processo. Utenti che gestiscono DNS, ad esempio, potrebbero essere un amministratore di AD. In genere, ciò avviene per evitare la necessità di configurare diversi livelli di amministrazione. Tuttavia, se questo tipo di account è compromesso, l'autore dell'attacco automaticamente dispone di privilegi elevati.

- **Costantemente eseguito l'accesso con privilegi elevati.** Un altro problema comune è che gli utenti con privilegi elevati possono utilizzarla per un tempo illimitato. Questa situazione è molto comune con i professionisti IT che accedono a un computer desktop utilizzando un account con privilegi, rimangono connessi e usano l'account con privilegi per esplorare il web e usare posta elettronica (tipica del lavoro IT funzioni del processo). Durata un numero illimitata di account con privilegi rende più vulnerabili agli attacchi di account e aumenta le probabilità che l'account sarà compromesso.

- **Ricerca di ingegneria sociale.** La maggior parte delle minacce credenziale iniziano con la ricerca dell'organizzazione e quindi viene eseguito tramite l'ingegneria sociale. Ad esempio, un utente malintenzionato potrebbe eseguire un attacco di phishing tramite posta elettronica di compromissione degli account legittimo, ma gli account non necessariamente con privilegi elevati, che hanno accesso alla rete dell'organizzazione. L'autore dell'attacco Usa quindi questi account validi per eseguire ricerche aggiuntive nella rete e per identificare gli account con privilegi possono eseguire attività amministrative. 

- **Usa gli account con privilegi elevati.** Anche con un account utente normale, senza privilegi elevati nella rete, gli utenti malintenzionati possono ottenere l'accesso agli account con autorizzazioni elevate. Uno dei metodi più comuni di tale operazione è così usando il Pass-the-Hash o attacchi Pass-the-Token. Per altre informazioni sul Pass-the-Hash e di altre tecniche di furto delle credenziali, vedere le risorse nel [paginaPass-the-Hash (PtH)](https://technet.microsoft.com/dn785092.aspx).

Ovviamente esistono altri metodi che gli utenti malintenzionati possono usare per identificare e compromettere le identità con privilegi (con nuovi metodi viene creati ogni giorno). È quindi importante stabilire procedure consigliate per gli utenti di accedere con l'account con privilegi minimi per ridurre la capacità di utenti malintenzionati di ottenere l'accesso alle identità con privilegi. Le sezioni seguenti illustrano la funzionalità in Windows Server possono limitare tali rischi.

#### <a name="just-in-time-admin-jit-and-just-enough-admin-jea"></a>Amministratore Just-in-Time (JIT) e Just Enough Admin (JEA)
Mentre la protezione da attacchi Pass-the-Ticket o Pass-the-Hash è importante, le credenziali di amministratore possono ancora essere rubate con altri mezzi, tra cui l'ingegneria sociale e dipendenti insoddisfatti a condurre attacchi di forza bruta. Pertanto, oltre all'isolamento delle credenziali quanto possibile, si desidera anche un modo per limitare la portata dei privilegi di amministratore nel caso in cui sono compromessi.

Oggi, un numero eccessivo di account amministratore è privilegi eccessivi, anche se hanno solo un'area di responsabilità. Ad esempio, un amministratore DNS, che richiede un insieme molto limitato dei privilegi per gestire i server DNS, è spesso concessi privilegi di amministrazione a livello di dominio. Inoltre, poiché queste credenziali vengono concesse per sempre, non è previsto alcun limite su quanto tempo possono essere usati.

Ogni account con privilegi di amministrazione a livello di dominio non necessari aumenta l'esposizione a utenti malintenzionati che cercano di compromettere le credenziali. Per ridurre al minimo la superficie di attacco, si vuole fornire solo il set specifico di diritti di amministratore deve eseguire il processo – e solo per il periodo di tempo necessario per completare l'operazione.

Utilizzo di Just Enough Administration e Just-in-Time amministrazione, agli amministratori di richiedere i privilegi specifici necessari per la finestra esatta del tempo necessario. Per un amministratore DNS, ad esempio, l'uso di PowerShell per abilitare Just Enough Administration consente di creare un set limitato di comandi disponibili per la gestione dei DNS.

Se l'amministratore DNS deve eseguire un aggiornamento a uno dei proprio server, Anna potrebbe richiedere l'accesso per gestire il DNS utilizzando Microsoft Identity Manager 2016. Il flusso di lavoro richiesta può includere un processo di approvazione, ad esempio l'autenticazione a due fattori, che è stato chiamata al telefono cellulare dell'amministratore per verificare la propria identità prima di concedere i privilegi richiesti. Una volta concesso, tali privilegi DNS forniscono l'accesso al ruolo di PowerShell per DNS per un intervallo di tempo specifico.

Se le credenziali dell'amministratore DNS sono state rubate, si immagini questo scenario. In primo luogo, poiché le credenziali senza privilegi di amministratore a cui è collegati, l'autore dell'attacco non sarebbe in grado di effettuare l'accesso al server DNS: o altri sistemi, apportare modifiche. Se l'utente malintenzionato tenta di richiedere i privilegi per il server DNS, autenticazione fattori potrebbe chiedere di confermare la propria identità. Poiché è improbabile che l'autore dell'attacco dispone di telefono cellulare dell'amministratore DNS, l'autenticazione avrà esito negativo. Ciò bloccare l'autore dell'attacco dal sistema e avvisa l'organizzazione IT che le credenziali potrebbero essere compromessi.

Inoltre, molte organizzazioni usano la versione gratuita [soluzione Password amministratore locale (LAP)](http://aka.ms/laps) come un meccanismo di amministrazione JIT semplice ma potente per i sistemi server e client. La funzionalità LAPS fornisce la gestione delle password di account locale dei computer aggiunti al dominio. Le password vengono archiviate in Active Directory (AD) e protetti tramite elenco di controllo di accesso (ACL) e pertanto gli utenti idonei solo possono leggerlo o richiedere la reimpostazione.

Come indicato nella [Windows Credential Theft Mitigation Guide](https://www.microsoft.com/en-us/download/confirmation.aspx?id=54095), 

> "_i criminali strumenti e tecniche utilizzano per eseguire i furti di credenziali e migliorare gli attacchi di riutilizzo, malintenzionati vengono rilevate più facile da raggiungere i propri obiettivi. Furto di credenziali si basa su procedure operative o l'esposizione delle credenziali utente, spesso in modo efficace mitigazioni richiedono un approccio olistico che risolve le persone, processi e tecnologia. Inoltre, questi attacchi si basano su un utente malintenzionato di rubare le credenziali dopo compromettere un sistema per espandere o salvare in modo permanente l'accesso, in modo che le organizzazioni devono contenere le violazioni rapidamente implementando le strategie che impediscono ai pirati informatici di spostare liberamente e non viene rilevata un rete compromessa._ "

Una considerazione importante per Windows Server è stato riduzione dei furti di credenziali, in particolare, derivati delle credenziali. Credential Guard fornisce notevole miglioramento della protezione contro il furto di credenziali derivate e riutilizzo per implementare modifiche significative all'architettura di Windows progettata per consentire l'eliminazione di attacchi di isolamento basato su hardware anziché tentare semplicemente di difesa contro di esse.

Anche se Usa Windows Defender Credential Guard, NTLM e Kerberos credenziali derivate sono protetti mediante la sicurezza basata sulla virtualizzazione, le tecniche di attacco il furto di credenziali e gli strumenti utilizzati molti attacchi mirati vengono bloccati. Il malware eseguito nel sistema operativo, anche se con privilegi amministrativi, non può estrarre segreti protetti tramite la sicurezza basata su virtualizzazione. Anche se Windows Defender Credential Guard è una soluzione potente, minacce persistenti attacchi verrà probabilmente MAIUSC per nuove tecniche di attacco e si devono inoltre incorporare Device Guard, come illustrato di seguito, insieme ad altre strategie di sicurezza e le architetture.

#### <a name="windows-defender-credential-guard"></a>Windows Defender Credential Guard
Windows Defender Credential Guard Usa la sicurezza basata su virtualizzazione per isolare le informazioni sulle credenziali, impedendo che gli hash delle password o i ticket Kerberos vengano intercettati. Usa un completamente nuovo processo dell'autorità di protezione locale (LSA) isolata, che non è accessibile al resto del sistema operativo. Tutti i file binari usati da LSA isolata vengono firmati con i certificati che vengono convalidati prima di avviare essi nell'ambiente protetto, rendendo gli attacchi di tipo Pass-the-Hash può rendere completamente inefficace.

Windows Defender Credential Guard Usa:

- Sicurezza basata su virtualizzazione (obbligatorio). Altri requisiti:

    - CPU a 64 bit

    - Estensioni di virtualizzazione CPU, oltre a tabelle di tabelle codici estese

    - Hypervisor di Windows

- Avvio protetto (obbligatorio)

- TPM 2.0 discreto o firmware (preferito, fornisce un'associazione all'hardware)

È possibile usare Windows Defender Credential Guard per proteggere le identità con privilegi proteggendo le credenziali e i derivati delle credenziali in Windows Server 2016. Per altre informazioni sui requisiti di Windows Defender Credential Guard, vedere [Proteggi derivati delle credenziali di dominio con Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard).

#### <a name="windows-defender-remote-credential-guard"></a>Windows Defender Credential Guard remoto
Windows Defender Remote Credential Guard in Windows Server 2016 e Windows 10 Anniversary Update consente inoltre di proteggere le credenziali per gli utenti con le connessioni desktop remoto. In precedenza, gli utenti che usano Servizi Desktop remoto dovrà accedere al proprio computer locale e quindi essere necessario accedere nuovamente quando eseguono una connessione remota ai computer di destinazione. Il secondo account di accesso seguente passa le credenziali nel computer di destinazione, esponendoli a Pass-the-Hash o attacchi Pass-the-Ticket.

Con Remote Credential Guard di Windows Defender, Windows Server 2016 implementa single sign-on per le sessioni di Desktop remoto, eliminando la necessità di immettere nuovamente nome utente e password. Al contrario, sfrutta le credenziali che è già stata usata per accedere al computer locale. Per usare Credential Guard remoto di Windows Defender, il client Desktop remoto e il server deve soddisfare i requisiti seguenti:

- Deve essere aggiunto a un dominio di Active Directory e trovarsi nello stesso dominio o in un dominio con una relazione di trust.

- È necessario usare l'autenticazione Kerberos.

- È necessario eseguire almeno Windows 10 versione 1607 o Windows Server 2016.  

- L'app di Windows classico di Desktop remoto è necessaria. L'app Remote Desktop (Universal Windows Platform) non supporta Windows Defender Remote Credential Guard.

È possibile abilitare Credential Guard remoto di Windows Defender tramite un'impostazione del Registro di sistema nel server di Desktop remoto e criteri di gruppo o un parametro di connessione Desktop remoto nel client Desktop remoto. Per altre informazioni sull'abilitazione di Credential Guard remoto di Windows Defender, vedere [credenziali di protezione Desktop remoto con Credential Guard remoto di Windows Defender](https://docs.microsoft.com/windows/access-protection/remote-credential-guard). Come con Windows Defender Credential Guard, è possibile usare Credential Guard remoto di Windows Defender per proteggere le identità con privilegi in Windows Server 2016.

### <a name="secure-the-operating-system-to-run-your-apps-and-infrastructure"></a>Sicurezza del sistema operativo per eseguire le App e infrastruttura
Prevenire le minacce informatiche richiede anche la ricerca e bloccare il malware e attacchi che cercano di ottenere un controllo subverting le procedure operative standard dell'infrastruttura. Se gli utenti malintenzionati possono ottenere un sistema operativo o l'applicazione da eseguire in modo non predeterminati, non valida, probabilmente utilizzino tale sistema per eseguire azioni dannose. Windows Server 2016 fornisce livelli di protezione che impedire attacchi esterni che eseguono software dannoso o che sfruttano le vulnerabilità. Il sistema operativo impiega un ruolo attivo nella protezione dell'infrastruttura e applicazioni per gli amministratori per le attività indica che viene violato un sistema di avvisi.

#### <a name="windows-defender-device-guard"></a>Windows Defender Device Guard
Windows Server 2016 include Windows Defender Device Guard per assicurarsi che possa essere eseguito solo software considerato attendibile nel server. Utilizza la sicurezza basata sulla virtualizzazione, è possibile limitare quali file binari possono essere eseguite nel sistema in base ai criteri dell'organizzazione. Se diverso, i file binari specificati tenta di eseguire Windows Server 2016 la blocca e registra il tentativo non riuscito in modo che gli amministratori possono vedere che si è verificato una potenziale violazione. Violazione della notifica è una parte essenziale dei requisiti di conformità al GDPR.

Windows Defender Device Guard è integrato anche con PowerShell in modo che è possibile autorizzare gli script che è possono eseguire nel sistema. Nelle versioni precedenti di Windows Server, gli amministratori potrebbero ignorare l'imposizione dell'integrità codice semplicemente eliminando i criteri dal file di codice. Con Windows Server 2016, è possibile configurare un criterio che viene eseguito l'accesso da parte dell'organizzazione in modo che solo una persona con accesso al certificato che ha firmato il criterio è possibile modificare i criteri.

#### <a name="control-flow-guard"></a>Protezione del flusso di controllo 
Windows Server 2016 include anche la protezione integrata contro alcune classi di attacchi al danneggiamento della memoria. L'applicazione di patch dei server è importante, ma è sempre probabile che il malware può essere sviluppato in una vulnerabilità che non è ancora stata identificata. Alcuni dei metodi più comuni per sfruttare le vulnerabilità sono utili per fornire estremi o insoliti oppure elementi dati da un programma in esecuzione. Ad esempio, un utente malintenzionato può sfruttare una vulnerabilità di overflow del buffer, fornendo più input per un programma quella prevista e l'area riservata dal programma per contenere una risposta al sovraccarico. Ciò può danneggiare la memoria adiacente che potrebbe contenere un puntatore a funzione.

Quando il programma chiama tramite questa funzione, può quindi passare a una posizione non prevista specificata dall'autore dell'attacco. Questi attacchi sono noti anche come jump-oriented programmazione attacchi (JOP). Guard flusso di controllo impedisce attacchi JOP inserendo una stretta restrizioni sul quale codice dell'applicazione può essere eseguita: particolarmente indiretta chiamare le istruzioni. Aggiunge controlli di sicurezza leggero per identificare il set di funzioni nell'applicazione che sono destinazioni valide per le chiamate indirette. Quando viene eseguita un'applicazione, verifica che tali destinazioni di chiamata indiretta siano valide.

Se il controllo di Guard flusso di controllo non riesce in fase di esecuzione, Windows Server 2016 termina immediatamente il programma, interrompere eventuali exploit che tenta di chiamare indirettamente un indirizzo non valido. Guard flusso di controllo fornisce un ulteriore livello di protezione importante di Device Guard. Se un'applicazione elenco elementi consentiti è stata compromessa, sarebbe in grado di eseguire non controllato da Device Guard, perché la protezione del dispositivo screening vedrebbe che l'applicazione è stato firmato e viene considerato attendibile.

Ma perché Guard flusso di controllo può stabilire se l'applicazione è in esecuzione in un ordine non predeterminati, non valido, l'attacco avrà esito negativo, impedendo l'esecuzione dell'applicazione compromesso. Insieme, queste protezioni rendono molto difficile per gli utenti malintenzionati inserire malware nel software in esecuzione in Windows Server 2016.

Gli sviluppatori che compilano applicazioni in cui verranno gestiti i dati personali sono invitati a Abilita controllo (Guard flusso) nelle proprie applicazioni. Questa funzionalità è disponibile in Microsoft Visual Studio 2015 e viene eseguito in "Riconosce" versioni di Windows, ovvero le versioni x86 e x64 per Desktop e Server di Windows 10 e Windows 8.1 Update (KB3000850). Non è necessario abilitare Guard flusso di controllo per tutte le parti del codice, come una combinazione di Guard flusso di controllo abilitato e non-Guard flusso di controllo abilitato codice verrà eseguito correttamente. Ma non superati abilitare Guard flusso di controllo per tutto il codice può aprire lacune nella protezione. Inoltre, Guard flusso di controllo abilitato codice funziona correttamente su "Non compatibili con Guard flusso di controllo" versioni di Windows e pertanto completamente compatibile con loro.

#### <a name="windows-defender-antivirus"></a>Windows Defender Antivirus
Windows Server 2016 include il settore iniziali e Attiva funzionalità di rilevamento di Windows Defender per bloccare i malware conosciuti. Windows Defender Antivirus (AV) funziona insieme a Windows Defender Device Guard e Guard flusso di controllo per impedire l'installazione nei server di codice dannoso di alcun tipo. È attivata per impostazione predefinita, l'amministratore non è necessario intraprendere alcuna azione per poter iniziare a lavorare. Windows Defender AV è anche ottimizzato per supportare i vari ruoli server in Windows Server 2016. In passato, gli utenti malintenzionati usato shell, come PowerShell per avviare binario di codice dannoso. In Windows Server 2016, PowerShell è ora integrato con Windows Defender AV per eseguire l'analisi antimalware prima di avviare il codice.

Windows Defender AV è una soluzione antimalware predefinita che consente la gestione della protezione e antimalware per desktop, computer portatili e server. Windows Defender AV è stata migliorata in modo significativo poiché è stata introdotta in Windows 8. Windows Defender Antivirus in Windows Server utilizza un approccio ad approcci per migliorare antimalware:

- **Protezione distribuita dal cloud** consente di rilevare e bloccare il nuovo malware in pochi secondi, anche se il malware non è stato riconosciuto prima.

- **Contesto locale avanzato** migliora l'identificazione del malware. Windows Server informa Windows Defender AV non solo sul contenuto, ad esempio i file e i processi, ma anche il contenuto provenienza, in cui è stata archiviata e altro ancora. 

- **Sensori globali completi** garantire la protezione di Windows Defender AV correnti e considerare anche il malware più recente. Questo è possibile in due modi: raccogliendo i dati del contesto locale avanzato dagli endpoint e analizzando centralmente questi dati.

- **Correzione di manomissione** proteggere Windows Defender AV stesso dagli attacchi malware. Ad esempio, Windows Defender AV Usa i processi protetti, che impedisce che i processi non attendibili tenti di manomettere i componenti di Windows Defender AV, le chiavi del Registro di sistema e così via.

- **Funzionalità di livello Enterprise** offre ai professionisti IT gli strumenti e opzioni di configurazione necessarie per rendere Windows Defender AV una soluzione antimalware di livello aziendale.

#### <a name="enhanced-security-auditing"></a>Controllo della sicurezza avanzata 
Windows Server 2016 attivamente informano gli amministratori di potenziali tentativi di violazione con sicurezza avanzata di controllo che fornisce informazioni più dettagliate, che possono essere utilizzate per il rilevamento di attacchi più veloce e analisi per scopi legali. Registra gli eventi di Guard flusso di controllo, Windows Defender Device Guard e altre funzionalità di sicurezza in un'unica posizione, rendendo più semplice per gli amministratori di determinare quali sistemi possono essere a rischio.

Le nuove categorie di eventi includono:

- **Appartenenza al gruppo di controllo.** Consente di controllare le informazioni di appartenenza al gruppo nel token di accesso dell'utente. Gli eventi vengono generati quando appartenenze vengono enumerate o eseguire una query in un computer in cui è stata creata la sessione di accesso. 
 
- **Controllare l'attività di plug and Play.** Consente di controllare quando il servizio plug and play rileva un dispositivo esterno, che potrebbe contenere malware. Eventi Plug and Play utilizzabile per tenere traccia delle modifiche nell'hardware del sistema. Un elenco di ID fornitore dell'hardware è incluso nell'evento.

Windows Server 2016 si integra facilmente con sistemi di gestione (SIEM) agli eventi imprevisti di sicurezza, ad esempio Microsoft Operations Management Suite (OMS), che può incorporare le informazioni nei report di intelligence su potenziali violazioni della. La profondità delle informazioni fornite per il controllo avanzato consente ai team di sicurezza identificare e rispondere a potenziali violazioni della sicurezza in modo più rapido ed efficace.

### <a name="secure-virtualization"></a>Virtualizzazione sicura
Le aziende oggi virtualizzano tutto ciò che è possibile, da SQL Server a SharePoint per i controller di dominio Active Directory. Macchine virtuali (VM) semplificano semplicemente distribuire, gestire, il servizio, e automatizzare l'infrastruttura. Ma quando si parla di sicurezza, le infrastrutture di virtualizzazione compromesso sono diventati un nuovo vettore di attacco che è difficile per difendersi – fino ad ora. Dalla prospettiva del regolamento GDPR, è opportuno valutare la protezione di macchine virtuali, come si proteggono server fisici, incluso l'uso della tecnologia TPM VM.

Windows Server 2016 in sostanza, cambia come le aziende possono proteggere la virtualizzazione, includendo più tecnologie che consentono di creare macchine virtuali che verranno eseguiti solo nel proprio fabric; aiuta a impedire l'archiviazione, rete e dispositivi host su che vengono eseguite.

#### <a name="shielded-virtual-machines"></a>Macchine virtuali schermate
Le stesse operazioni che rendono così facile eseguire la migrazione delle macchine virtuali, il backup e replica, nonché renderli più facile da modificare e copiare. Una macchina virtuale è semplicemente un file, in modo che non sia protetto in rete, in archiviazione, nel backup o di altro tipo. Un altro problema è che gli amministratori dell'infrastruttura: se sono un amministratore di archiviazione o un amministratore di rete – abbiano accesso a tutte le macchine virtuali.

Un amministratore nell'infrastruttura di compromesso può causare facilmente compromessa dati tra le macchine virtuali. È necessario solo l'autore dell'attacco è usare le credenziali compromesse per copiare i file della macchina virtuale che desiderano in un'unità USB e illustrano all'esterno dell'organizzazione, in cui i file della macchina virtuale sono accessibile da qualsiasi altro sistema. Se una di queste VM rubate fosse un controller di dominio Active Directory, ad esempio, l'utente malintenzionato potrebbe facilmente visualizzare il contenuto e usare tecniche di attacco di forza bruta immediatamente disponibili per violare le password nel database di Active Directory e infine concedere loro l'accesso per tutti gli altri elementi all'interno dell'infrastruttura.

Windows Server 2016 introduce schermato macchine virtuali (VM schermate) per proteggersi da scenari, ad esempio quella appena descritta. Le macchine virtuali schermate includono un dispositivo TPM virtuale, che consente alle organizzazioni di applicare la crittografia BitLocker per le macchine virtuali e verificare che vengono eseguite solo su host attendibili per la protezione da compromesso di archiviazione, rete e gli amministratori di host. Le macchine virtuali schermate vengono create tramite macchine virtuali di generazione 2, che supportano il firmware Unified Extensible Firmware Interface (UEFI) e dispone di TPM virtuale.

#### <a name="host-guardian-service"></a>Servizio sorveglianza host
Insieme a macchine virtuali schermate, il servizio sorveglianza Host è un componente essenziale per la creazione di un'infrastruttura di virtualizzazione sicura. Il processo è di attestare l'integrità di un host Hyper-V prima di consentirne una VM schermata per l'avvio o eseguire la migrazione a quell'host. Mantiene le chiavi per le VM schermate e non rilascerà li fino a quando non è garantita l'integrità della sicurezza. Esistono due modi in cui è possibile richiedere che gli host Hyper-V attestare la il servizio sorveglianza Host.

Il primo e più sicuro, è attestazione hardware. Questa soluzione richiede che le macchine virtuali schermate in esecuzione negli host dotati di chip TPM 2.0 e UEFI 2.3.1. Questo hardware è necessaria per fornire l'avvio con misurazioni e le informazioni sull'integrità di kernel del sistema operativo richieste dal servizio sorveglianza Host per assicurarsi che l'host Hyper-V non è stato manomesso.

Le organizzazioni IT hanno l'alternativa di tramite attestazione amministratore, che può essere opportuno se hardware TPM 2.0 non è in uso nell'organizzazione. Questo modello l'attestazione è facile da distribuire in quanto gli host vengono semplicemente inseriti in un gruppo di sicurezza e il servizio sorveglianza Host è configurato per consentire le VM schermate per l'esecuzione negli host che sono membri del gruppo di sicurezza. Con questo metodo, non c'è alcuna misura complesso per garantire che il computer host non è stato alterato. Tuttavia, eliminare la possibilità di non crittografate le macchine virtuali walking out lo sportello USB unità o che la macchina virtuale verrà eseguito in un host non autorizzato. Questo avviene perché i file della macchina virtuale non verranno eseguito in qualsiasi computer diversi da quelli del gruppo designato. Se ancora non si dispone di hardware TPM 2.0, è possibile iniziare con attestazione amministratore e passare a attestazione hardware quando l'hardware viene aggiornato.

#### <a name="virtual-machine-trusted-platform-module"></a>Macchina virtuale di Trusted Platform Module
Windows Server 2016 supporta TPM per le macchine virtuali, che consente di supportare le tecnologie di sicurezza avanzate, ad esempio Crittografia unità BitLocker® nelle macchine virtuali. È possibile abilitare il supporto TPM in qualsiasi macchina virtuale di generazione 2 Hyper-V tramite Gestione di Hyper-V o il cmdlet Enable-VMTPM Windows PowerShell.

È possibile proteggere TPM virtuale (vTPM) usando le chiavi di crittografia locali archiviato nell'host o nel servizio sorveglianza Host. Pertanto, mentre il servizio sorveglianza Host richiede maggiore infrastruttura, nonché altre funzionalità di protezione.

#### <a name="distributed-network-firewall-using-software-defined-networking"></a>Firewall di rete distribuita tramite rete software-defined
Un modo per migliorare la protezione in ambienti virtualizzati è necessario segmentare la rete in modo da consentire alle macchine virtuali di comunicare solo con i sistemi specifici necessari per funzionare. Ad esempio, se l'applicazione non è necessaria per connettersi con Internet, è possibile partizionare viene disabilitato, eliminando tali sistemi come destinazioni da attacchi esterni. La rete software-defined (SDN) in Windows Server 2016 include un firewall di rete distribuita che consente di creare in modo dinamico i criteri di sicurezza che consente di proteggere le applicazioni da attacchi provenienti da all'interno o all'esterno di una rete. Questo firewall di rete distribuita aggiunge livelli per la sicurezza grazie alla possibilità di isolare le applicazioni nella rete. I criteri possono essere applicati in un punto qualsiasi nell'infrastruttura di rete virtuale, isolare il traffico delle VM a VM, il traffico di macchina virtuale dell'host o al traffico per macchine Virtuali a Internet dove sia necessario – per singoli sistemi che potrebbero essere stati compromessi o a livello di codice in più subnet. Funzionalità di rete definita dal software Windows Server 2016 consentono inoltre di eseguire il routing o eseguire il mirroring di traffico in ingresso verso Appliance virtuali non Microsoft. Ad esempio, si potrebbe scegliere di inviare tutto il traffico di posta elettronica attraverso un'appliance virtuale Barracuda per protezione posta indesiderata aggiuntiva. In questo modo è al livello facilmente la sicurezza sia in locale o nel cloud.

### <a name="other-gdpr-considerations-for-servers"></a>Altre considerazioni GDPR per i server
Gli standard GDPR è riportati i requisiti espliciti per la notifica di violazioni in cui si intende una violazione dei dati personali, "_una violazione della sicurezza iniziali per l'eliminazione accidentale o illegali, perdita, l'alterazione, divulgazione non autorizzate, o l'accesso a, personale dati trasmessi, memorizzati o elaborati in caso contrario._ "  Ovviamente, non è possibile iniziare per spostarsi in avanti per soddisfare i requisiti di notifica al GDPR rigorosi entro 72 ore, se non è possibile rilevare in primo luogo la violazione.

Come indicato nel Centro sicurezza di Windows white paper, [Post violazione: Gestione di minacce avanzate](http://wincom.blob.core.windows.net/documents/Post_Breach_Dealing_with_Advanced_Threats_Whitepaper.pdf)

> "_a differenza di violazione della pre, post violazione, si presuppone che una violazione si è già verificato, che agisce come un registratore di volo e criminalità scena responsabile dell'indagine (CSI). Post violazione offre ai team di sicurezza le informazioni e set di strumenti necessari per identificare, analizzare e rispondere agli attacchi che in caso contrario rimarrà non rilevati e di sotto di radar._ "

In questa sezione si esaminerà come Windows Server consente di soddisfare gli obblighi previsti notifica violazione GDPR. Verrà avviata a comprendere i dati sottostanti di minaccia a raccolte e analizzate per il vantaggio Microsoft e, tramite Windows Defender Advanced Threat Protection (ATP), tali dati possono essere è fondamentali.

#### <a name="insightful-security-diagnostic-data"></a>Dati di diagnostica sulla sicurezza dettagliati
Per quasi due decenni, Microsoft ha stato attivazione minacce in intelligence utile innalzano propria piattaforma e protezione dei clienti. Oggi, con gli enormi vantaggi dell'elaborazione offerti dal cloud, stiamo trovando nuovi modi per utilizzare i nostri motori di analisi avanzata controllati dall'Intelligence per le minacce al fine di proteggere i nostri clienti.

Applicando una combinazione di processi automatizzati e manuali, machine learning ed esperti umani, possiamo creare un Intelligence Security Graph che apprenda da se stesso e si evolva in tempo reale, riducendo il tempo collettivo per rilevare e rispondere a nuovi problemi tra i nostri prodotti.

![Graph di Security Intelligence di Microsoft](../media/GDPR-Windows-Server-Overview/gdpr-intelligent-security-graph.png)

L'ambito di intelligence sulle minacce di Microsoft si estende su, letteralmente miliardi di punti dati: 35 a miliardi di messaggi analizzati ogni mese, 1 miliardo di clienti aziendali e consumer segmenti l'accesso a servizi cloud di oltre 200 e 14 miliardi di autenticazioni eseguiti ogni giorno. Tutti questi dati sono riuniti automaticamente da Microsoft per creare Intelligent Security Graph che possono aiutarti a proteggere l'ingresso principale in modo dinamico per la sicurezza della connessione, rimanere produttivi e soddisfare i requisiti del GDPR.

#### <a name="detecting-attacks-and-forensic-investigation"></a>Rilevamento di attacchi e indagini forensi
Anche le migliori difese endpoint potrebbero essere violate alla fine, poiché gli attacchi cibernetici diventano più sofisticati e mirati. Due funzionalità è utilizzabile per facilitare il potenziale del rilevamento di violazione - Windows Defender Advanced Threat Protection (ATP) e Microsoft Advanced Threat Analitica (ATA).

Windows Defender Advanced Threat Protection (ATP) consente di rilevare, analizzare e rispondere agli attacchi avanzati e alle violazioni dei dati nelle reti. I tipi di violazione dei dati GDPR prevede che l'utente per la protezione da adottando le misure di sicurezza tecnica per garantire la riservatezza in corso, l'integrità e disponibilità dei dati personali e i sistemi di elaborazione.

I vantaggi principali di Windows Defender ATP comprendono quanto segue:

- **Rilevamento di non rilevabili.** Sensori compilati in dettaglio il kernel del sistema operativo, gli esperti di sicurezza di Windows e ottica univoco alle segnalazioni da oltre 1 miliardo macchine e segnali in tutti i servizi Microsoft.

- **Incorporato, non fissate in.** Senza agenti, con prestazioni elevate e un impatto minimo, basate sul cloud; facile gestione con alcuna distribuzione. 

- **Unica console per la sicurezza di Windows.** Esplorare 6 mesi della sequenza temporale-machine avanzata, unificare gli eventi di sicurezza di Windows Defender ATP, Windows Defender Antivirus e Windows Defender Device Guard.

- **Potenza di Microsoft graph.** Sfrutta l'Intelligence di Microsoft Security Graph per integrare il rilevamento e l'esplorazione con sottoscrizione di Office 365 ATP, per registrare nuovamente e rispondere agli attacchi.

Altre informazioni in [What’s new in the Windows Defender ATP Creators Update preview](https://blogs.microsoft.com/microsoftsecure/2017/03/13/whats-new-in-the-windows-defender-atp-creators-update-preview/).

ATA è un prodotto locale che aiuta a rilevare identità sia compromessa in un'organizzazione. ATA può acquisire e analizzare il traffico di rete per l'autenticazione, autorizzazione e la raccolta di protocolli (ad esempio Kerberos, DNS, RPC, NTLM e altri protocolli) di informazioni. ATA Usa questi dati per creare un profilo comportamentale sugli utenti e altre entità in una rete in modo che è possibile rilevare le anomalie e i modelli di attacco noti. Nella tabella seguente elenca i tipi di attacco, rilevati da ATA.


|Tipo di attacco |Descrizione |
|---------|---------|
|Attacchi dannosi |Questi attacchi vengono rilevati mediante la ricerca di attacchi da un elenco noto di tipi di attacco, tra cui:<ul><li>Pass-the-Ticket (PtT)</li><li>Pass-the-Hash (PtH)</li><li>Overpass-the-Hash</li><li>PAC contraffatta (MS14 068)</li><li>Golden Ticket</li><li>Repliche dannose</li><li>Esplorazione</li><li>Attacchi di forza bruta</li><li>Esecuzione remota</li></ul>Per un elenco completo di attacchi dannosi che possono essere rilevati e le relative descrizioni, vedere [cosa sospette rilevate da ATA?](https://docs.microsoft.com/advanced-threat-analytics/understand-explore/ata-threats).|
|Comportamento anomalo |Questi attacchi vengono rilevati tramite analisi comportamentali e usano machine learning per identificare attività sospette, tra cui:<ul><li>Accessi anomali</li><li>Minacce sconosciute</li><li>La condivisione di password</li><li>Spostamento laterale</li></ul>|
|I rischi e problemi di sicurezza |Questi attacchi vengono rilevati dall'esame di rete corrente e la configurazione di sistema, tra cui:<ul><li>Attendibilità è interrotta</li><li>Protocolli deboli</li><li>Vulnerabilità note dei protocolli</li></ul>|

È possibile usare ATA per aiutare a rilevare gli utenti malintenzionati provano a compromettere le identità con privilegi. Per altre informazioni sulla distribuzione di ATA, vedere gli argomenti di pianificazione, progettazione e distribuisci nel [documentazione di Advanced Threat Analitica](https://docs.microsoft.com/advanced-threat-analytics/).

## <a name="related-content-for-associated-windows-server-2016-solutions"></a>Contenuto correlato di associati soluzioni Windows Server 2016

- **Windows Defender Antivirus:** https://www.youtube.com/watch?v=P1aNEy09NaI e https://docs.microsoft.com/windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10

- **Windows Defender Advanced Threat Protection:** https://www.youtube.com/watch?v=qxeGa3pxIwg e https://docs.microsoft.com/windows/threat-protection/windows-defender-atp/configure-server-endpoints-windows-defender-advanced-threat-protection

- **Windows Defender Device Guard:** https://www.youtube.com/watch?v=F-pTkesjkhI e https://docs.microsoft.com/windows/device-security/device-guard/device-guard-deployment-guide

- **Windows Defender Credential Guard:** https://www.youtube.com/watch?v=F-pTkesjkhI e https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard

- **Guard flusso di controllo:** https://msdn.microsoft.com/en-us/library/windows/desktop/mt637065(v=vs.85).aspx

- **Sicurezza e Assurance:** https://docs.microsoft.com/windows-server/security/security-and-assurance

## <a name="disclaimer"></a>Dichiarazione di non responsabilità
Questo articolo è un commento sull'RGPD, su come Microsoft lo interpreta alla data di pubblicazione. Abbiamo speso molto tempo sull'RGPD e ci piace pensare che abbiamo riflettuto sulla finalità e il significato. Ma l'applicazione dell'RGPD è molto legata alla situazione e non tutti gli aspetti e le interpretazioni dell'RGPD sono ben noti.

Di conseguenza, questo articolo viene fornito solo a scopo informativo e non deve essere necessariamente visto come consulenza legale o per stabilire in che modo l'RGPD potrebbe applicarsi a te o alla tua organizzazione. Ti consigliamo di utilizzare un professionista legalmente qualificato per discutere dell'RGPD, di come si applica in modo specifico alla tua organizzazione e di come garantire la conformità.

MICROSOFT ESCLUDE OGNI GARANZIA, ESPRESSA, IMPLICITA O DI LEGGE NEI CONFRONTI DELLE INFORMAZIONI FORNITE NEL PRESENTE ARTICOLO. Questo articolo viene fornito "com'è". Le informazioni e le indicazioni riportate nel presente articolo, inclusi gli URL e altri riferimenti a siti Web Internet, sono soggette a modifica senza preavviso.

Il presente articolo non implica la concessione di alcun diritto di proprietà intellettuale in relazione ai prodotti Microsoft.  Questo articolo potrà essere duplicato e utilizzato esclusivamente per fini di riferimento interno.  

Data di pubblicazione: settembre 2017<br>
Versione 1.0<br>
© 2017 Microsoft. Tutti i diritti sono riservati.


