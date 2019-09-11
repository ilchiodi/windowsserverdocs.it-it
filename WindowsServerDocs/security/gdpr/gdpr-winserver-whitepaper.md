---
title: Applicazione del Regolamento generale sulla protezione dei dati (RGPD) per Windows Server 2016
description: Questo articolo ti consente di comprendere che cosa è l'RGPD e di conoscere i prodotti che Microsoft fornisce per introdurti alla conformità.
ms.technology: techgroup-security
ms.topic: article
ms.date: 09/25/2017
ms.author: nirb
author: nirb-ms
ms.openlocfilehash: 506cd5cb44d93c9d7d221917505f76a2c5625baa
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870547"
---
# <a name="beginning-your-general-data-protection-regulation-gdpr-journey-for-windows-server"></a>Inizio del percorso di Regolamento generale sulla protezione dei dati (GDPR) per Windows Server 

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

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

- **Diritti di privacy personali avanzati.** Aumentare la protezione dei dati per i residenti dell'Unione europea assicurando che abbiano il diritto di accedere ai loro dati personali per correggere imprecisioni, cancellarli, rifiutare il loro trattamento e trasferirli.

- **Maggiore responsabilità per la protezione dei dati personali.** Rafforzata la responsabilità delle organizzazioni che trattano i dati personali, fornendo una maggiore chiarezza di responsabilità nel garantire la conformità.

- **Segnalazione di violazioni dei dati personali obbligatorie.** Le organizzazioni che controllano i dati personali sono obbligate a segnalare tempestivamente alle autorità di controllo violazioni dei dati personali che rappresentino un rischio per i diritti e le libertà delle persone e, laddove possibile, non oltre 72 ore dal momento in cui vengono a conoscenza della violazione.

Come è prevedibile, l'RGPD può avere un impatto significativo sull'azienda, obbligando di fatto ad aggiornare le informative sulla privacy, a implementare e rafforzare i controlli di protezione dei dati e le procedure di notifica delle violazioni, a implementare criteri di elevata trasparenza e a investire ulteriormente nell'IT e nella formazione. Microsoft Windows 10 può aiutarti in modo efficace ed efficiente a soddisfare alcuni di questi requisiti.

## <a name="personal-and-sensitive-data"></a>Dati personali e sensibili
Nell'ambito dell'impegno per la conformità all'RGPD dovrai comprendere come il regolamento definisce i dati personali e sensibili e come tali definizioni siano correlate ai dati conservati dalla tua organizzazione. In base a tale comprensione, sarà possibile individuare il punto in cui vengono creati, elaborati, gestiti e archiviati i dati.

L'RGPD considera come dati personali qualsiasi dato relativo a una persona naturale identificata o identificabile. Ciò può includere sia l'identificazione diretta (ad esempio, il nome legale) sia l'identificazione indiretta (ad esempio, dati specifici che identifichino chiaramente la persona cui fanno riferimento). L'RGPD chiarisce inoltre che il concetto di dati personali include gli identificatori online (ad esempio, gli indirizzi IP, gli ID del dispositivo mobile) e i dati sulla posizione.

GDPR introduce definizioni specifiche per i dati genetici (ad esempio, la sequenza genica di un singolo) e i dati biometrici. Dati genetici e dati biometrici insieme ad altre sottocategorie di dati personali (dati personali che rivelano origini razziali o etniche, opinioni politiche, credenze religiose o filosofiche o appartenenza a sindacato): dati relativi all'integrità o dati relativi a un la vita sessuale o l'orientamento sessuale della persona viene considerata come dati personali sensibili sotto la GDPR. Per i dati personali sensibili viene garantita la protezione avanzata e in genere è necessario il consenso esplicito di un utente in cui devono essere elaborati.

### <a name="examples-of-info-relating-to-an-identified-or-identifiable-natural-person-data-subject"></a>Esempi di dati relativi a una persona naturale identificata o identificabile (soggetto titolare di dati)
In questo elenco vengono forniti esempi di diversi tipi di dati che saranno regolati tramite l'RGPD. Non è un elenco esaustivo.

-   NOME

-   Numero di identificazione (ad esempio, CF)

-   Dati sulla posizione (ad esempio, indirizzo di casa)

-   Identificatore online (ad esempio, indirizzo di posta elettronica, nomi visualizzati, l'indirizzo IP, ID del dispositivo)

-   Pseudonimi (ad esempio, l'utilizzo di un codice per identificare le persone)

-   Dati genetici (ad esempio, campioni biologici di una persona)

-   Dati biometrici (ad esempio, le impronte digitali, il riconoscimento facciale)

## <a name="getting-started-on-the-journey-towards-gdpr-compliance"></a>Guida introduttiva sul viaggio verso la conformità RGPD
Una volta specificato quanto sia necessario per diventare compatibili con l'RGPD, ti consigliamo di non aspettare che la compatibilità venga imposta. È opportuno che tu esamini ora le procedure relative alla privacy e alla gestione dei dati. Ti consigliamo di iniziare il viaggio verso la conformità all'RGPD concentrandoti su quattro fasi principali:

-   **Individuare.** Identifica i dati personali in tuo possesso e dove si trovano. 

-   **Gestire.** Disciplina la modalità di trattamento e accessibilità dei dati personali.

-   **Proteggere.** Stabilisci controlli di sicurezza per impedire, rilevare vulnerabilità e violazioni dei dati e rispondere ad esse.  

-   **Relazione.** Agisci sulle richieste di dati, segnala violazioni dei dati e conserva la documentazione richiesta.

    ![Grafico sull'interazione delle 4 fasi RGPD chiave](../media/GDPR-Windows-Server-Overview/gdpr-steps-diagram.png)

Per ognuna delle fasi abbiamo descritto gli strumenti di esempio, le risorse e le funzionalità in varie soluzioni Microsoft, che possono essere utilizzate per aiutarti a soddisfare i requisiti di quella fase. Anche se questo articolo non è una guida completa, sono stati inclusi collegamenti per ottenere ulteriori dettagli e altre informazioni sono disponibili nella [sezione GDPR del Centro protezione Microsoft](https://www.microsoft.com/en-us/trustcenter/privacy/gdpr).

## <a name="windows-server-security-and-privacy"></a>Sicurezza e privacy di Windows Server
GDPR richiede di implementare misure di sicurezza tecniche e organizzative appropriate per proteggere i dati e i sistemi di elaborazione personali. Nel contesto di GDPR, gli ambienti di server fisici e virtuali stanno elaborando potenzialmente dati personali e sensibili. L'elaborazione può indicare qualsiasi operazione o set di operazioni, ad esempio la raccolta, l'archiviazione e il recupero dei dati.

La possibilità di soddisfare questo requisito e di implementare misure di sicurezza tecnica appropriate deve riflettere le minacce che si affrontano nell'ambiente IT sempre più ostile. Il panorama delle minacce per la sicurezza odierno è una delle minacce aggressive e tenacie. Negli anni precedenti gli autori degli attacchi dannosi erano principalmente alla ricerca del riconoscimento da parte della community degli attacchi o del brivido di una disconnessione temporanea del sistema. Da allora, i motivi dell'autore dell'attacco si sono spostati verso il denaro, inclusi i dispositivi e l'host dei dati fino a quando il proprietario non paga il riscatto richiesto.

Gli attacchi moderni si concentrano sempre più sul furto della proprietà intellettuale su larga scala, sulla riduzione delle prestazioni del sistema di destinazione che può comportare perdite finanziarie e ora anche sul terrorismo informatico che minaccia la sicurezza delle persone, delle aziende e degli interessi nazionali a livello mondiale. Questi attacchi sono in genere progettati da persone altamente specializzate ed esperti in sicurezza, alcune delle quali sono alle dipendente di stati-nazione con grandi budget e risorse umane apparentemente illimitate. Minacce come queste richiedono un approccio all'altezza della sfida.

Non solo queste minacce sono un rischio per la possibilità di mantenere il controllo dei propri dati personali o sensibili, ma sono anche un rischio serio per le aziende in generale. Prendere in considerazione i dati recenti di McKinsey, Ponemon Institute, Verizon e Microsoft:

- Il costo medio del tipo di violazione dei dati previsto dall'RGPD è di 3,5 milioni di dollari.

- Il 63% di tali violazioni riguarda password deboli o rubate, che l'RGPD prevede sia tu a risolvere.

- Più di 300.000 nuovi esempi di malware vengono creati e distribuiti ogni giorno per rendere ancora più difficile la tua attività di protezione dei dati.

Come osservato nei recenti attacchi ransomware, una volta chiamato la peste nera di Internet, gli utenti malintenzionati hanno obiettivi più grandi che possono permettersi di pagare di più, con conseguenze potenzialmente catastrofiche. Il GDPR include sanzioni che rendono i sistemi, inclusi i desktop e i portatili, che contengono effettivamente obiettivi avanzati per i dati personali e sensibili.

Due principi chiave hanno guidato e continuano a guidare lo sviluppo di Windows:

- **Sicurezza.** I dati archiviati dal software e dai servizi per conto dei clienti devono essere protetti da danni e usati o modificati solo in modi appropriati. I modelli di sicurezza dovrebbero essere facili da comprendere per gli sviluppatori e compilarli nelle proprie applicazioni.

- **Privacy.** Gli utenti devono avere il controllo della modalità di utilizzo dei dati. I criteri per l'uso delle informazioni devono essere chiari per l'utente. Gli utenti devono avere il controllo di quando e se ricevono informazioni per sfruttare al meglio il tempo. È consigliabile che gli utenti specifichino l'uso appropriato delle proprie informazioni, incluso il controllo dell'uso del messaggio di posta elettronica inviato.

Microsoft è rimasta salda a fronte di questi principi, come recentemente indicato dal CEO di Microsoft, di Nadella, 

> "_Poiché il mondo continua a cambiare e i requisiti aziendali si evolvono, alcune sono coerenti: la richiesta di sicurezza e privacy da parte del cliente"._

Quando si lavora per la conformità con la GDPR, è importante comprendere il ruolo dei server fisici e virtuali per la creazione, l'accesso, l'elaborazione, l'archiviazione e la gestione dei dati che possono essere qualificati come dati personali e potenzialmente sensibili in GDPR. Windows Server offre funzionalità che consentono di soddisfare i requisiti di GDPR per implementare misure di sicurezza tecniche e organizzative appropriate per proteggere i dati personali.

Il comportamento di sicurezza di Windows Server 2016 non è un Bolt-on; si tratta di un principio di architettura. E può essere più comprensibile in quattro entità:

- **Proteggere.** Costante interesse e innovazione sulle misure preventive; blocca gli attacchi noti e il malware noto.

- **Rilevare.** Strumenti di monitoraggio completi che consentono di individuare le anomalie e rispondere agli attacchi più velocemente.

- **Rispondere.** Tecnologie di risposta e ripristino leader, oltre a un'esperienza di consulenza approfondita.

- **Isolare.** Isolare i componenti del sistema operativo e i segreti dati, limitare i privilegi di amministratore e misurare rigorosamente l'integrità dell'host.

Con Windows Server, la capacità di proteggere, rilevare e difendere i tipi di attacchi che possono causare violazioni dei dati è notevolmente migliorata. Dati i rigidi requisiti nell'RGPD riguardanti la notifica delle violazioni, un'ottima difesa dei sistemi desktop e portatili riduce i rischi che dovrai affrontare e che potrebbero comportare costose analisi e notifiche delle violazioni.

Nella sezione seguente viene illustrato in che modo Windows Server fornisce le funzionalità che si adattano al quadrato nella fase "protezione" del percorso di conformità di GDPR. Queste funzionalità rientrano in tre scenari di protezione:

- **Proteggere le credenziali e limitare i privilegi di amministratore.** Windows Server 2016 consente di implementare queste modifiche, per evitare che il sistema venga usato come punto di avvio per ulteriori intrusioni.

- **Proteggere il sistema operativo per eseguire le app e l'infrastruttura.** Windows Server 2016 offre livelli di protezione che consentono di impedire agli utenti malintenzionati esterni di eseguire software dannoso o sfruttare vulnerabilità.

- **Virtualizzazione sicura.** Windows Server 2016 consente la virtualizzazione sicura, usando macchine virtuali schermate e l'infrastruttura sorvegliata. Questo consente di crittografare ed eseguire le macchine virtuali in host attendibili nell'infrastruttura, proteggendo in modo più efficace gli attacchi dannosi.

Queste funzionalità, descritte in dettaglio più avanti con i riferimenti a specifici requisiti di GDPR, sono basate sulla protezione avanzata dei dispositivi, che consente di mantenere l'integrità e la sicurezza del sistema operativo e dei dati.

Il provisioning delle chiavi all'interno della GDPR è la protezione dei dati in base alla progettazione e, per impostazione predefinita, e la possibilità di soddisfare tale provisioning sono le funzionalità di Windows 10, come la crittografia del dispositivo BitLocker. BitLocker utilizza la tecnologia Trusted Platform Module (TPM), che fornisce funzioni correlate alla sicurezza basate su hardware. Questo chip del processore di crittografia include più meccanismi di sicurezza fisica per renderla resistente alle manomissioni e il software dannoso non è in grado di manomettere le funzioni di sicurezza del TPM.

Il chip include più meccanismi di sicurezza fisica in grado di proteggerlo da manomissioni; il software dannoso non sarà pertanto in grado di manomettere le funzioni di sicurezza del TPM. Alcuni dei principali vantaggi dell'uso della tecnologia TPM sono elencati di seguito:

-   Generare, archiviare e limitare l'uso di chiavi di crittografia.

-   Usare la tecnologia TPM per l'autenticazione del dispositivo della piattaforma usando la chiave RSA univoca del TPM, che viene masterizzata in se stessa.

-   Garantire l'integrità della piattaforma mediante l'analisi e l'archiviazione delle misure di sicurezza.

La protezione aggiuntiva del dispositivo avanzato, importante per l'esecuzione senza violazioni di dati, include l'avvio sicuro di Windows che consente di garantire l'integrità del sistema verificando che il malware non sia in grado di avviarsi prima delle difese di sistema.

## <a name="windows-server-supporting-your-gdpr-compliance-journey"></a>Windows Server: Supporto del percorso di conformità di GDPR
Le funzionalità principali di Windows Server consentono di implementare in modo efficiente ed efficace i meccanismi di sicurezza e privacy richiesti da GDPR per la conformità. Sebbene l'utilizzo di queste funzionalità non garantisca la conformità, l'utente supporterà le proprie attività.

Il sistema operativo server si trova a un livello strategico nell'infrastruttura di un'organizzazione, offrendo nuove opportunità per creare livelli di protezione da attacchi che potrebbero sottrarre dati e interrompere l'attività aziendale. Gli aspetti chiave di GDPR, ad esempio la privacy per progettazione, protezione dei dati e controllo di accesso, devono essere risolti all'interno dell'infrastruttura IT a livello di server.

Per contribuire alla protezione dell'identità, del sistema operativo e dei livelli di virtualizzazione, Windows Server 2016 contribuisce a bloccare i vettori di attacco comuni usati per ottenere accesso illecito ai sistemi: credenziali rubate, malware e un'infrastruttura di virtualizzazione compromessa. Oltre a ridurre i rischi aziendali, i componenti di sicurezza incorporati in Windows Server 2016 aiutano a soddisfare i requisiti di conformità per le normative chiave governative e di sicurezza del settore. 

Queste protezioni di identità, sistema operativo e virtualizzazione ti permettono di proteggere meglio il tuo Data Center che esegue Windows Server come macchina virtuale in qualsiasi cloud e di limitare la capacità degli utenti malintenzionati di compromettere le credenziali, avviare malware e rimanere non rilevati nella tua rete. Analogamente, quando viene distribuito come host Hyper-V, Windows Server 2016 offre una garanzia di sicurezza per gli ambienti di virtualizzazione tramite macchine virtuali schermate e funzionalità firewall distribuite. Con Windows Server 2016, il sistema operativo server diventa un partecipante attivo alla sicurezza del Data Center.

### <a name="protect-your-credentials-and-limit-administrator-privileges"></a>Proteggi le credenziali e limita i privilegi di amministratore 
Controllare l'accesso ai dati personali e i sistemi che elaborano i dati, è un'area con GDPR che presenta requisiti specifici, incluso l'accesso da parte degli amministratori. Le identità con privilegi sono tutti gli account con privilegi elevati, ad esempio gli account utente che sono membri dei gruppi Domain Administrators, Enterprise Administrators, Local Administrators o even Power Users. Tali identità possono includere anche account a cui sono stati concessi direttamente i privilegi, ad esempio l'esecuzione di backup, l'arresto del sistema o altri diritti elencati nel nodo assegnazione diritti utente della console Criteri di sicurezza locale.

Come principio generale di controllo degli accessi e in linea con la GDPR, è necessario proteggere le identità con privilegi da compromessi da potenziali utenti malintenzionati. Prima di tutto, è importante comprendere il modo in cui le identità vengono compromesse. è quindi possibile pianificare di impedire agli utenti malintenzionati di accedere a queste identità con privilegi.

#### <a name="how-do-privileged-identities-get-compromised"></a>In che modo le identità con privilegi vengono compromesse?
Le identità con privilegi possono essere compromesse quando le organizzazioni non dispongono di linee guida per proteggerle. Di seguito vengono riportati alcuni esempi:

- **Più privilegi di quelli necessari.** Uno dei problemi più comuni è che gli utenti hanno più privilegi rispetto a quelli necessari per eseguire la funzione di lavoro. Ad esempio, un utente che gestisce DNS potrebbe essere un amministratore di AD. In genere, questa operazione viene eseguita per evitare la necessità di configurare livelli di amministrazione diversi. Tuttavia, se un account di questo tipo è compromesso, l'autore dell'attacco dispone automaticamente di privilegi elevati.

- **Accesso continuo con privilegi elevati.** Un altro problema comune è che gli utenti con privilegi elevati possono utilizzarlo per un tempo illimitato. Questo è molto comune con i professionisti IT che effettuano l'accesso a un computer desktop usando un account con privilegi, rimaneno connessi e usano l'account con privilegi per esplorare il Web e usare la posta elettronica (funzioni tipiche del processo di lavoro IT). La durata illimitata degli account con privilegi rende l'account più vulnerabile agli attacchi e aumenta le probabilità che l'account venga compromesso.

- **Ricerca di ingegneria sociale.** La maggior parte delle minacce per le credenziali viene avviata tramite la ricerca di un'organizzazione e poi gestita attraverso la progettazione sociale. Ad esempio, un utente malintenzionato può eseguire un attacco di phishing tramite posta elettronica per compromettere gli account legittimi, ma non necessariamente quelli con privilegi elevati, che hanno accesso alla rete di un'organizzazione. L'autore dell'attacco USA quindi questi account validi per eseguire ricerche aggiuntive sulla rete e per identificare gli account con privilegi che possono eseguire attività amministrative. 

- **Utilizzare gli account con privilegi elevati.** Anche con un normale account utente non con privilegi elevati in rete, gli utenti malintenzionati possono accedere agli account con autorizzazioni elevate. Uno dei metodi più comuni per eseguire questa operazione consiste nell'usare gli attacchi pass-the-hash o pass-the-token. Per ulteriori informazioni sulle tecniche pass-the-hash e altri furti di credenziali, vedere le risorse nella [pagina pass-the-hash (PtH)](https://technet.microsoft.com/dn785092.aspx).

Esistono naturalmente altri metodi che gli utenti malintenzionati possono usare per identificare e compromettere le identità con privilegi (con nuovi metodi creati ogni giorno). È quindi importante stabilire le procedure per consentire agli utenti di accedere con account con privilegi minimi per ridurre la capacità degli utenti malintenzionati di ottenere l'accesso alle identità con privilegi. Le sezioni seguenti descrivono le funzionalità in cui Windows Server può attenuare questi rischi.

#### <a name="just-in-time-admin-jit-and-just-enough-admin-jea"></a>Amministrazione JIT (just-in-Time) e amministrazione sufficiente (JEA)
Sebbene la protezione contro gli attacchi pass-the-hash o pass-the-ticket sia importante, le credenziali di amministratore possono comunque essere rubate in altro modo, tra cui ingegneria sociale, dipendenti con stato scontento e forza bruta. Quindi, oltre a isolare il più possibile le credenziali, è necessario anche un modo per limitare la portata dei privilegi a livello di amministratore nel caso in cui vengano compromessi.

Attualmente, troppi account amministratore hanno privilegi eccessivi, anche se hanno solo un'area di responsabilità. Ad esempio, un amministratore DNS, che richiede un set molto limitato di privilegi per la gestione dei server DNS, viene spesso concesso privilegi a livello di amministratore di dominio. Inoltre, dal momento che queste credenziali vengono concesse per l'perpetuità, non esiste alcun limite per quanto tempo possono essere utilizzate.

Ogni account con privilegi a livello di amministratore di dominio non necessari aumenta l'esposizione a utenti malintenzionati che cercano di compromettere le credenziali. Per ridurre al minimo la superficie di attacco, è necessario fornire solo il set specifico di diritti necessari a un amministratore per eseguire il processo e solo per la finestra di tempo necessaria per il completamento.

Utilizzando l'amministrazione sufficiente e l'amministrazione JIT, gli amministratori possono richiedere i privilegi specifici necessari per la finestra di tempo esatta necessaria. Per un amministratore DNS, ad esempio, l'uso di PowerShell per abilitare just enough Administration consente di creare un set limitato di comandi disponibili per la gestione DNS.

Se l'amministratore DNS deve eseguire un aggiornamento a uno dei server, dovrà richiedere l'accesso per gestire il DNS con Microsoft Identity Manager 2016. Il flusso di lavoro della richiesta può includere un processo di approvazione, ad esempio l'autenticazione a due fattori, che può chiamare il telefono cellulare dell'amministratore per confermare la propria identità prima di concedere i privilegi richiesti. Una volta concessa, questi privilegi DNS forniscono l'accesso al ruolo di PowerShell per DNS per un intervallo di tempo specifico.

Si supponga questo scenario se le credenziali dell'amministratore DNS sono state rubate. Innanzitutto, poiché le credenziali non dispongono di privilegi di amministratore collegati, l'autore dell'attacco non sarebbe in grado di accedere al server DNS, o a qualsiasi altro sistema, per apportare modifiche. Se l'autore dell'attacco ha tentato di richiedere i privilegi per il server DNS, l'autenticazione a due fattori richiede di confermare la propria identità. Poiché non è probabile che l'utente malintenzionato abbia il telefono cellulare dell'amministratore DNS, l'autenticazione avrà esito negativo. In questo modo l'utente malintenzionato viene bloccato dal sistema e avvisa l'organizzazione IT che le credenziali potrebbero essere compromesse.

Molte organizzazioni usano inoltre la soluzione gratuita per la [password dell'amministratore locale](http://aka.ms/laps) come meccanismo di amministrazione JIT semplice ma potente per i sistemi server e client. La funzionalità di giri consente di gestire le password degli account locali dei computer aggiunti a un dominio. Le password vengono archiviate in Active Directory (AD) e protette da e l'elenco di controllo di accesso (ACL), in modo che solo gli utenti idonei possano leggerlo o richiederne la reimpostazione.

Come indicato nella [Guida alla mitigazione del furto delle credenziali di Windows](https://www.microsoft.com/en-us/download/confirmation.aspx?id=54095), 

> "_gli strumenti e le tecniche che i criminali usano per eseguire il furto e il riutilizzo delle credenziali migliorano, gli utenti malintenzionati possono trovare più facilmente gli obiettivi. Il furto di credenziali spesso si basa su procedure operative o sull'esposizione delle credenziali utente, pertanto le mitigazioni efficaci richiedono un approccio olistico che indirizzi persone, processi e tecnologia. Inoltre, questi attacchi si basano sull'utente malintenzionato che ruba le credenziali dopo aver comprometteto un sistema per espandere o rendere permanente l'accesso, in modo che le organizzazioni debbano avere violazioni rapide implementando strategie che impediscono agli utenti malintenzionati di muoversi liberamente e non rilevati in rete compromessa._ "

Una considerazione importante per Windows Server è stata la mitigazione del furto delle credenziali, in particolare le credenziali derivate. Credential Guard offre una protezione significativamente migliorata rispetto al furto e al riutilizzo delle credenziali derivate mediante l'implementazione di una significativa modifica all'architettura di Windows progettata per eliminare gli attacchi di isolamento basati su hardware anziché semplicemente tentare di difendersi da essi.

Quando si usa Windows Defender Credential Guard, NTLM e le credenziali derivate da Kerberos sono protette tramite la sicurezza basata sulla virtualizzazione, le tecniche e gli strumenti di attacco per il furto di credenziali usati in molti attacchi mirati sono bloccati. Il malware eseguito nel sistema operativo, anche se con privilegi amministrativi, non può estrarre segreti protetti tramite la sicurezza basata su virtualizzazione. Anche se Windows Defender Credential Guard è una potente mitigazione, è probabile che gli attacchi permanenti alle minacce cambino a nuove tecniche di attacco ed è necessario anche incorporare Device Guard, come descritto di seguito, insieme ad altre strategie e architetture di sicurezza.

#### <a name="windows-defender-credential-guard"></a>Windows Defender Credential Guard
Windows Defender Credential Guard usa la sicurezza basata sulla virtualizzazione per isolare le informazioni sulle credenziali, impedendo l'intercettazione degli hash delle password o dei ticket Kerberos. Usa un processo LSA (Local Security Authority) completamente nuovo, che non è accessibile al resto del sistema operativo. Tutti i file binari usati dal LSA isolato vengono firmati con i certificati che vengono convalidati prima di avviarli nell'ambiente protetto, rendendo gli attacchi di tipo pass-the-hash completamente inefficaci.

Windows Defender Credential Guard USA:

- Sicurezza basata sulla virtualizzazione (obbligatoria). Richiesto anche:

    - CPU a 64 bit

    - Estensioni di virtualizzazione CPU, oltre a tabelle estese della pagina

    - Hypervisor di Windows

- Avvio protetto (obbligatorio)

- TPM 2.0 discreto o firmware (preferito, fornisce un'associazione all'hardware)

È possibile usare Windows Defender Credential Guard per proteggere le identità con privilegi proteggendo le credenziali e i derivati delle credenziali in Windows Server 2016. Per altre informazioni sui requisiti di Windows Defender Credential Guard, vedere [proteggere le credenziali di dominio derivate con Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard).

#### <a name="windows-defender-remote-credential-guard"></a>Windows Defender Remote Credential Guard
Windows Defender Remote Credential Guard in Windows Server 2016 e l'aggiornamento dell'anniversario di Windows 10 consentono inoltre di proteggere le credenziali per gli utenti con connessioni Desktop remoto. In precedenza, tutti gli utenti che utilizzavano Servizi Desktop remoto avrebbero dovuto accedere al computer locale, quindi dovranno eseguire di nuovo l'accesso quando eseguivano una connessione remota al computer di destinazione. Il secondo account di accesso passa le credenziali al computer di destinazione, mostrandogli gli attacchi pass-the-hash o pass-the-ticket.

Con Windows Defender Remote Credential Guard, Windows Server 2016 implementa Single Sign-On per le sessioni di Desktop remoto, eliminando la necessità di immettere nuovamente il nome utente e la password. Si avvale invece delle credenziali già utilizzate per accedere al computer locale. Per usare Windows Defender Remote Credential Guard, il client e il server Desktop remoto devono soddisfare i requisiti seguenti:

- Deve essere aggiunto a un dominio di Active Directory e trovarsi nello stesso dominio o in un dominio con una relazione di trust.

- È necessario utilizzare l'autenticazione Kerberos.

- Deve eseguire almeno Windows 10 versione 1607 o Windows Server 2016.  

- È richiesta l'app di Windows classica Desktop remoto. L'app piattaforma UWP (Universal Windows Platform) Desktop remoto non supporta Windows Defender Remote Credential Guard.

È possibile abilitare Windows Defender Remote Credential Guard usando un'impostazione del registro di sistema nel server Desktop remoto e Criteri di gruppo o un parametro Connessione Desktop remoto nel client Desktop remoto. Per altre informazioni sull'abilitazione di Windows Defender Remote Credential Guard, vedere [proteggere le credenziali desktop remoto con Windows Defender Remote Credential Guard](https://docs.microsoft.com/windows/access-protection/remote-credential-guard). Come con Windows Defender Credential Guard, è possibile usare Windows Defender Remote Credential Guard per proteggere le identità con privilegi in Windows Server 2016.

### <a name="secure-the-operating-system-to-run-your-apps-and-infrastructure"></a>Proteggere il sistema operativo per eseguire le app e l'infrastruttura
Prevenire le minacce informatiche richiede anche l'individuazione e il blocco di malware e attacchi che cercano di ottenere il controllo mediante la subverting delle procedure operative standard dell'infrastruttura. Se gli utenti malintenzionati possono ottenere un sistema operativo o un'applicazione da eseguire in un modo non predeterminato, è probabile che utilizzino tale sistema per intraprendere azioni dannose. Windows Server 2016 offre livelli di protezione che impediscono agli utenti malintenzionati esterni di eseguire software dannoso o sfruttare vulnerabilità. Il sistema operativo assume un ruolo attivo nella protezione dell'infrastruttura e delle applicazioni, avvisando gli amministratori dell'attività che indica che un sistema è stato violato.

#### <a name="windows-defender-device-guard"></a>Windows Defender Device Guard
Windows Server 2016 include Windows Defender Device Guard per garantire che solo il software attendibile possa essere eseguito sul server. Utilizzando la sicurezza basata sulla virtualizzazione, è possibile limitare i file binari che possono essere eseguiti nel sistema in base ai criteri dell'organizzazione. Se il tentativo di esecuzione è diverso da quello dei file binari specificati, Windows Server 2016 lo blocca e registra il tentativo non riuscito, in modo che gli amministratori possano verificare la presenza di una potenziale violazione. La notifica di violazione è una parte essenziale dei requisiti per la conformità a GDPR.

Windows Defender Device Guard è integrato anche con PowerShell, in modo che sia possibile autorizzare gli script che possono essere eseguiti nel sistema. Nelle versioni precedenti di Windows Server, gli amministratori potevano ignorare l'imposizione dell'integrità del codice eliminando semplicemente i criteri dal file di codice. Con Windows Server 2016, è possibile configurare un criterio firmato dall'organizzazione in modo che solo una persona con accesso al certificato che ha firmato i criteri possa modificare i criteri.

#### <a name="control-flow-guard"></a>Protezione del flusso di controllo 
Windows Server 2016 include anche la protezione incorporata da alcune classi di attacchi di danneggiamento della memoria. L'applicazione di patch ai server è importante, ma c'è sempre la possibilità di sviluppare malware per una vulnerabilità non ancora identificata. Alcuni dei metodi più comuni per sfruttare queste vulnerabilità sono la fornitura di dati insoliti o estremi a un programma in esecuzione. Un utente malintenzionato può, ad esempio, sfruttare una vulnerabilità di overflow del buffer fornendo un maggior numero di input a un programma rispetto al previsto e sovraccaricando l'area riservata dal programma per mantenere una risposta. Questo può danneggiare la memoria adiacente che potrebbe avere un puntatore a funzione.

Quando il programma chiama tramite questa funzione, può passare a una posizione non desiderata specificata dall'autore dell'attacco. Questi attacchi sono noti anche come attacchi JOP (Jump-Oriented Programming). La protezione del flusso di controllo impedisce gli attacchi JOP inserendo restrizioni restrittive sul codice dell'applicazione che può essere eseguito, soprattutto istruzioni per le chiamate indirette. Aggiunge controlli di sicurezza leggeri per identificare il set di funzioni nell'applicazione che sono destinazioni valide per le chiamate indirette. Quando un'applicazione viene eseguita, verifica che tali destinazioni di chiamata indirette siano valide.

Se il controllo di Guard flusso di controllo non riesce in fase di esecuzione, Windows Server 2016 termina immediatamente il programma, interrompendo eventuali exploit che tentano di chiamare indirettamente un indirizzo non valido. La protezione del flusso di controllo fornisce un importante livello aggiuntivo di protezione a Device Guard. Se un'applicazione in elenco bianco è stata compromessa, sarà possibile eseguirla deselezionata da Device Guard, perché la selezione di Device Guard potrebbe vedere che l'applicazione è stata firmata e considerata attendibile.

Tuttavia, poiché la protezione del flusso di controllo è in grado di stabilire se l'applicazione è in esecuzione in un ordine non predeterminato e non valido, l'attacco potrebbe non riuscire a impedire l'esecuzione dell'applicazione compromessa. Insieme, queste protezioni rendono molto difficile per gli utenti malintenzionati inserire malware nel software in esecuzione in Windows Server 2016.

Gli sviluppatori che compilano applicazioni in cui verranno gestiti i dati personali sono invitati ad abilitare la protezione del flusso di controllo (CFG) nelle proprie applicazioni. Questa funzionalità è disponibile in Microsoft Visual Studio 2015 e viene eseguita su versioni di Windows compatibili con CFG, ovvero le versioni x86 e x64 per desktop e server di Windows 10 e Windows 8.1 Update (KB3000850). Non è necessario abilitare la funzionalità CFG per ogni parte del codice, perché una combinazione di CFG abilitata e il codice non abilitato per la CFG verrà eseguita correttamente. Tuttavia, la mancata abilitazione di CFG per tutto il codice può aprire gap nella protezione. Inoltre, il codice abilitato per la funzionalità CFG funziona correttamente nelle versioni di Windows "non compatibili con CFG" ed è quindi completamente compatibile con loro.

#### <a name="windows-defender-antivirus"></a>Windows Defender Antivirus
Windows Server 2016 include le funzionalità di rilevamento attive e leader di settore di Windows Defender per bloccare malware noto. Windows Defender Antivirus (AV) funziona insieme a Windows Defender Device Guard e controllo del flusso di controllo per impedire l'installazione di codice dannoso di qualsiasi tipo nei server. Per impostazione predefinita, l'amministratore non deve intraprendere alcuna azione per avviare il lavoro. Windows Defender AV è inoltre ottimizzato per supportare i vari ruoli del server in Windows Server 2016. In passato, gli utenti malintenzionati usavano shell come PowerShell per avviare codice binario dannoso. In Windows Server 2016 PowerShell è ora integrato con Windows Defender AV per eseguire la ricerca di malware prima di avviare il codice.

Windows Defender AV è una soluzione antimalware incorporata che fornisce la gestione della sicurezza e antimalware per desktop, computer portatili e server. Windows Defender AV è stato notevolmente migliorato poiché è stato introdotto in Windows 8. Windows Defender antivirus in Windows Server usa un approccio a più livelli per migliorare la protezione antimalware:

- **Protezione distribuita dal cloud** consente di rilevare e bloccare il nuovo malware in pochi secondi, anche se il malware non è stato riconosciuto prima.

- **Contesto locale avanzato** migliora l'identificazione del malware. Windows Server informa Windows Defender AV non solo sul contenuto, ad esempio file e processi, ma anche da dove proviene il contenuto, dove è stato archiviato e altro ancora. 

- I **sensori globali estesi** consentono di tenere Windows Defender AV corrente e consapevole anche del malware più recente. Questo è possibile in due modi: raccogliendo i dati del contesto locale avanzato dagli endpoint e analizzando centralmente questi dati.

- La **correzione delle manomissioni** aiuta a sorvegliare Windows Defender AV stesso contro gli attacchi malware. Ad esempio, Windows Defender AV usa processi protetti che impediscono ai processi non attendibili di manomettere i componenti di Windows Defender AV, le chiavi del registro di sistema e così via.

- Le **funzionalità a livello aziendale** offrono ai professionisti IT gli strumenti e le opzioni di configurazione necessari per rendere Windows Defender AV una soluzione antimalware di livello aziendale.

#### <a name="enhanced-security-auditing"></a>Controllo di sicurezza avanzato 
Windows Server 2016 avvisa attivamente gli amministratori di potenziali tentativi di violazione con controllo di sicurezza avanzato che fornisce informazioni più dettagliate, che possono essere usate per il rilevamento di attacchi più veloci e l'analisi forense. Registra gli eventi da Guard flusso di controllo, Windows Defender Device Guard e altre funzionalità di sicurezza in un'unica posizione, rendendo più semplice per gli amministratori determinare quali sistemi potrebbero essere a rischio.

Le nuove categorie di eventi includono:

- **Controllare l'appartenenza al gruppo.** Consente di controllare le informazioni sull'appartenenza a gruppi nel token di accesso di un utente. Gli eventi vengono generati quando le appartenenze a gruppi vengono enumerate o sottoposte a query nel computer in cui è stata creata la sessione di accesso. 
 
- **Attività PnP di controllo.** Consente di controllare quando plug and Play rileva un dispositivo esterno, che potrebbe contenere malware. Gli eventi PnP possono essere utilizzati per tenere traccia delle modifiche apportate all'hardware del sistema. Un elenco di ID fornitore hardware è incluso nell'evento.

Windows Server 2016 si integra facilmente con i sistemi di gestione degli eventi imprevisti della sicurezza (SIEM), ad esempio Microsoft Operations Management Suite (OMS), che può incorporare le informazioni nei report di intelligence sulle potenziali violazioni. La profondità delle informazioni fornite dal controllo avanzato consente ai team di sicurezza di identificare e rispondere alle potenziali violazioni in modo più rapido ed efficace.

### <a name="secure-virtualization"></a>Virtualizzazione sicura
Le aziende oggi virtualizzano tutti gli elementi che possono, da SQL Server a SharePoint ai controller Dominio di Active Directory. Le macchine virtuali (VM) semplificano la distribuzione, la gestione, il servizio e l'automazione dell'infrastruttura. Tuttavia, quando si tratta di una sicurezza, le infrastrutture di virtualizzazione compromesse sono diventate un nuovo vettore di attacco difficile da difendere fino a questo momento. Dal punto di vista di GDPR, è necessario considerare la protezione delle macchine virtuali come si proteggono i server fisici, incluso l'uso della tecnologia TPM della macchina virtuale.

Windows Server 2016 modifica sostanzialmente il modo in cui le aziende possono proteggere la virtualizzazione, includendo più tecnologie che consentono di creare macchine virtuali che vengono eseguite solo nell'infrastruttura. supporto per la protezione dai dispositivi di archiviazione, rete e host in cui vengono eseguiti.

#### <a name="shielded-virtual-machines"></a>Macchine virtuali schermate
Gli stessi elementi che semplificano la migrazione, il backup e la replica delle macchine virtuali rendono più semplice la modifica e la copia. Una macchina virtuale è semplicemente un file, pertanto non è protetta sulla rete, nella risorsa di archiviazione, nei backup o altrove. Un altro problema consiste nel fatto che gli amministratori dell'infrastruttura, ovvero un amministratore di archiviazione o un amministratore di rete, possono accedere a tutte le macchine virtuali.

Un amministratore compromesso nell'infrastruttura può facilmente generare dati compromessi tra le macchine virtuali. È necessario che tutti gli utenti malintenzionati usino le credenziali compromesse per copiare tutti i file di macchina virtuale desiderati in un'unità USB e uscire dall'organizzazione, in cui è possibile accedere ai file delle macchine virtuali da qualsiasi altro sistema. Se una di queste macchine virtuali rubate era un controller di dominio Active Directory, ad esempio, l'autore dell'attacco potrebbe visualizzare facilmente il contenuto e utilizzare tecniche di forza bruta prontamente disponibili per craccare le password nel database Active Directory, fornendo loro l'accesso a tutti gli altri elementi all'interno dell'infrastruttura.

Windows Server 2016 introduce macchine virtuali schermate (VM schermate) per facilitare la protezione da scenari come quello appena descritto. Le macchine virtuali schermate includono un dispositivo TPM virtuale, che consente alle organizzazioni di applicare la crittografia BitLocker alle macchine virtuali e di assicurarsi che vengano eseguite solo su host attendibili per facilitare la protezione da amministratori di archiviazione, rete e host compromessi. Le macchine virtuali schermate vengono create usando macchine virtuali di seconda generazione, che supportano il firmware Unified Extensible Firmware Interface (UEFI) e hanno un TPM virtuale.

#### <a name="host-guardian-service"></a>Servizio sorveglianza host
Insieme alle macchine virtuali schermate, il servizio sorveglianza host è un componente essenziale per la creazione di un'infrastruttura di virtualizzazione sicura. Il suo compito consiste nell'attestare l'integrità di un host Hyper-V prima di consentire l'avvio di una macchina virtuale schermata o la migrazione a tale host. Include le chiavi per le macchine virtuali schermate e non le rilascia fino a quando non viene garantita l'integrità della sicurezza. Esistono due modi per richiedere agli host Hyper-V di attestare il servizio sorveglianza host.

Il primo e il più sicuro è l'attestazione di hardware attendibile. Questa soluzione richiede che le macchine virtuali schermate siano in esecuzione in host con chip TPM 2,0 e UEFI 2.3.1. Questo hardware è necessario per fornire le informazioni sull'integrità del kernel del sistema operativo e di avvio misurate richieste dal servizio sorveglianza host per assicurarsi che l'host Hyper-V non sia stato alterato.

Le organizzazioni IT hanno l'alternativa di usare l'attestazione attendibile dell'amministratore, che può essere utile se l'hardware TPM 2,0 non è in uso nell'organizzazione. Questo modello di attestazione è facile da distribuire perché gli host vengono semplicemente inseriti in un gruppo di sicurezza e il servizio sorveglianza host è configurato in modo da consentire l'esecuzione delle macchine virtuali schermate sugli host che sono membri del gruppo di sicurezza. Con questo metodo non esiste una misurazione complessa per garantire che il computer host non sia stato alterato. Tuttavia, si elimina la possibilità che le macchine virtuali non crittografate si esportino sulle unità USB o che la macchina virtuale venga eseguita in un host non autorizzato. Ciò è dovuto al fatto che i file della macchina virtuale non vengono eseguiti in un computer diverso da quelli del gruppo designato. Se non si dispone ancora di hardware TPM 2,0, è possibile iniziare con l'attestazione attendibile per l'amministratore e passare all'attestazione hardware attendibile quando l'hardware viene aggiornato.

#### <a name="virtual-machine-trusted-platform-module"></a>Trusted Platform Module della macchina virtuale
Windows Server 2016 supporta TPM per le macchine virtuali, che consente di supportare tecnologie di sicurezza avanzate, ad esempio BitLocker® crittografia unità in macchine virtuali. È possibile abilitare il supporto TPM in una macchina virtuale Hyper-V di seconda generazione usando la console di gestione di Hyper-V o il cmdlet Enable-VMTPM di Windows PowerShell.

È possibile proteggere il TPM virtuale (vTPM) utilizzando le chiavi di crittografia locali archiviate nell'host o archiviate nel servizio sorveglianza host. Quindi, anche se il servizio sorveglianza host richiede una maggiore infrastruttura, garantisce una maggiore protezione.

#### <a name="distributed-network-firewall-using-software-defined-networking"></a>Firewall di rete distribuita con rete definita dal software
Un modo per migliorare la protezione negli ambienti virtualizzati consiste nel segmentare la rete in modo da consentire alle macchine virtuali di comunicare solo con i sistemi specifici necessari per funzionare. Se, ad esempio, l'applicazione non deve connettersi a Internet, è possibile partizionarla, eliminando tali sistemi come destinazioni da utenti malintenzionati esterni. SDN (Software-Defined Networking) in Windows Server 2016 include un firewall di rete distribuito che consente di creare in modo dinamico i criteri di sicurezza che consentono di proteggere le applicazioni da attacchi provenienti dall'interno o dall'esterno di una rete. Questo firewall di rete distribuita aggiunge livelli alla sicurezza consentendo di isolare le applicazioni nella rete. I criteri possono essere applicati ovunque nell'infrastruttura di rete virtuale, isolando il traffico da macchina virtuale a macchina virtuale, il traffico da macchina virtuale a host o il traffico da macchina virtuale a Internet laddove necessario, per i singoli sistemi che potrebbero essere stati compromessi o a livello di codice tra più subnet. Le funzionalità di rete definite dal software di Windows Server 2016 consentono inoltre di instradare o eseguire il mirroring del traffico in ingresso a appliance virtuali non Microsoft. Ad esempio, è possibile scegliere di inviare tutto il traffico di posta elettronica attraverso un'appliance virtuale Barracuda per la protezione aggiuntiva del filtro della posta indesiderata. Questo consente di eseguire facilmente il livello di sicurezza aggiuntiva in locale o nel cloud.

### <a name="other-gdpr-considerations-for-servers"></a>Altre considerazioni su GDPR per i server
Il GDPR include i requisiti espliciti per la notifica di violazione in cui la violazione dei dati personali implica una violazione_della sicurezza che causa la distruzione accidentale o illegale, la perdita, la modifica, la divulgazione non autorizzata o l'accesso ai dati personali trasmesso, archiviato o altrimenti elaborato "._  Ovviamente, non è possibile iniziare a progredire per soddisfare i severi requisiti di notifica di GDPR entro 72 ore se non è possibile rilevare la violazione in primo luogo.

Come indicato nel centro sicurezza Windows white paper, [post-violazione: Gestione di minacce avanzate](http://wincom.blob.core.windows.net/documents/Post_Breach_Dealing_with_Advanced_Threats_Whitepaper.pdf)

> "_A differenza della pre-violazione, la post-violazione presuppone che si sia già verificata una violazione, che funge da registratore di volo e investigatore della scena del crimine (CSI). Il post-violazione fornisce ai team di sicurezza le informazioni e il set di strumenti necessari per identificare, analizzare e rispondere agli attacchi che altrimenti non saranno rilevati e al di sotto del radar._ "

In questa sezione verrà esaminato il modo in cui Windows Server può aiutare a soddisfare gli obblighi di notifica della violazione della GDPR. Questa operazione inizia con la comprensione dei dati delle minacce sottostanti disponibili per Microsoft, raccolti e analizzati per i vantaggi e come, tramite Windows Defender Advanced Threat Protection (ATP), i dati possono essere fondamentali.

#### <a name="insightful-security-diagnostic-data"></a>Dati di diagnostica sulla sicurezza dettagliati
Per quasi due decenni, Microsoft ha trasformato le minacce in informazioni utili che possono contribuire a fortificare la piattaforma e proteggere i clienti. Oggi, con gli enormi vantaggi dell'elaborazione offerti dal cloud, stiamo trovando nuovi modi per utilizzare i nostri motori di analisi avanzata controllati dall'Intelligence per le minacce al fine di proteggere i nostri clienti.

Applicando una combinazione di processi automatizzati e manuali, machine learning ed esperti umani, possiamo creare un Intelligence Security Graph che apprenda da se stesso e si evolva in tempo reale, riducendo il tempo collettivo per rilevare e rispondere a nuovi problemi tra i nostri prodotti.

![Microsoft Intelligence Security Graph](../media/GDPR-Windows-Server-Overview/gdpr-intelligent-security-graph.png)

L'ambito dell'Intelligence per le minacce di Microsoft si estende letteralmente a miliardi di punti dati: 35 miliardi messaggi analizzati mensilmente, 1 miliardo clienti in segmenti aziendali e consumer che accedono a 200 servizi cloud e 14 miliardi di autenticazione eseguiti ogni giorno. Tutti questi dati vengono riuniti per conto di Microsoft per creare i Intelligent Security Graph che consentono di proteggere la porta anteriore in modo dinamico per restare protetti, rimanere produttivi e soddisfare i requisiti di GDPR.

#### <a name="detecting-attacks-and-forensic-investigation"></a>Rilevamento di attacchi e indagini forensi
Anche le migliori difese endpoint potrebbero essere violate alla fine, poiché gli attacchi cibernetici diventano più sofisticati e mirati. È possibile usare due funzionalità per facilitare il rilevamento delle violazioni, ovvero Windows Defender Advanced Threat Protection (ATP) e Microsoft Advanced Threat Analytics (ATA).

Windows Defender Advanced Threat Protection (ATP) consente di rilevare, analizzare e rispondere agli attacchi avanzati e alle violazioni dei dati nelle reti. I tipi di violazione dei dati che GDPR prevede di proteggersi da misure di sicurezza tecniche per garantire la riservatezza, l'integrità e la disponibilità di dati e sistemi di elaborazione personali.

I vantaggi principali di Windows Defender ATP sono i seguenti:

- **Rilevamento dell'oggetto non rilevabile.** Sensori integrati nel kernel del sistema operativo, esperti di sicurezza di Windows e ottica univoca da oltre 1 miliardo computer e segnali in tutti i servizi Microsoft.

- **Incorporato, non bloccato.** Senza agente, con prestazioni elevate e un minimo effetto, basato sul cloud; facile gestione senza distribuzione. 

- **Riquadro singolo del vetro per la sicurezza di Windows.** Scopri 6 mesi di Rich, sequenza temporale del computer, unificazione degli eventi di sicurezza di Windows Defender ATP, Windows Defender Antivirus e Windows Defender Device Guard.

- **Potenza di Microsoft Graph.** Sfrutta il grafo Microsoft Intelligence Security per integrare il rilevamento e l'esplorazione con la sottoscrizione di Office 365 ATP, per tenere traccia degli attacchi e rispondere agli attacchi.

Per altre informazioni, vedere Novità [di Windows Defender ATP Creators Update Preview](https://blogs.microsoft.com/microsoftsecure/2017/03/13/whats-new-in-the-windows-defender-atp-creators-update-preview/).

ATA è un prodotto locale che consente di rilevare la compromissione delle identità in un'organizzazione. ATA è in grado di acquisire e analizzare il traffico di rete per i protocolli di autenticazione, autorizzazione e raccolta di informazioni, ad esempio Kerberos, DNS, RPC, NTLM e altri protocolli. ATA usa questi dati per creare un profilo comportamentale sugli utenti e altre entità in una rete, in modo che possa rilevare le anomalie e i modelli di attacco noti. La tabella seguente elenca i tipi di attacco rilevati da ATA.


|Tipo di attacco |Descrizione |
|---------|---------|
|Attacchi dannosi |Questi attacchi vengono rilevati cercando gli attacchi da un elenco noto di tipi di attacco, tra cui:<ul><li>Pass-the-ticket (PtT)</li><li>Pass-the-hash (PtH)</li><li>Overpass-the-hash</li><li>PAC falsificata (MS14-068)</li><li>Golden Ticket</li><li>Repliche dannose</li><li>Esplorazione</li><li>Forza bruta</li><li>Esecuzione remota</li></ul>Per un elenco completo di attacchi dannosi che possono essere rilevati e la loro descrizione, vedere [quali attività sospette possono essere rilevate da ATA?](https://docs.microsoft.com/advanced-threat-analytics/understand-explore/ata-threats).|
|Comportamento anomalo |Questi attacchi vengono rilevati tramite l'analisi comportamentale e usano Machine Learning per identificare le attività discutibili, tra cui:<ul><li>Accessi anomali</li><li>Minacce sconosciute</li><li>Condivisione password</li><li>Spostamento laterale</li></ul>|
|Problemi di sicurezza e rischi |Questi attacchi vengono rilevati osservando la configurazione di rete e di sistema corrente, tra cui:<ul><li>Attendibilità interruppe</li><li>Protocolli vulnerabili</li><li>Vulnerabilità del protocollo note</li></ul>|

È possibile usare ATA per aiutare a rilevare gli utenti malintenzionati che tentano di compromettere le identità con privilegi. Per altre informazioni sulla distribuzione di ATA, vedere gli argomenti relativi a piani, progettazione e distribuzione nella [documentazione di Advanced Threat Analytics](https://docs.microsoft.com/advanced-threat-analytics/).

## <a name="related-content-for-associated-windows-server-2016-solutions"></a>Contenuto correlato per le soluzioni Windows Server 2016 associate

- **Windows Defender Antivirus:** https://www.youtube.com/watch?v=P1aNEy09NaI e https://docs.microsoft.com/windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10

- **Windows Defender Advanced Threat Protection:** https://www.youtube.com/watch?v=qxeGa3pxIwg e https://docs.microsoft.com/windows/threat-protection/windows-defender-atp/configure-server-endpoints-windows-defender-advanced-threat-protection

- **Windows Defender Device Guard:** https://www.youtube.com/watch?v=F-pTkesjkhI e https://docs.microsoft.com/windows/device-security/device-guard/device-guard-deployment-guide

- **Windows Defender Credential Guard:** https://www.youtube.com/watch?v=F-pTkesjkhI e https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard

- **Guard flusso di controllo:** https://msdn.microsoft.com/library/windows/desktop/mt637065(v=vs.85).aspx

- **Sicurezza e garanzia:** https://docs.microsoft.com/windows-server/security/security-and-assurance

## <a name="disclaimer"></a>Dichiarazione di non responsabilità
Questo articolo è un commento sull'RGPD, su come Microsoft lo interpreta alla data di pubblicazione. Abbiamo dedicato molto tempo con GDPR e come pensiamo che siamo stati attenti al suo scopo e al suo significato. Ma l'applicazione dell'RGPD è molto legata alla situazione e non tutti gli aspetti e le interpretazioni dell'RGPD sono ben noti.

Di conseguenza, questo articolo viene fornito solo a scopo informativo e non deve essere necessariamente visto come consulenza legale o per stabilire in che modo l'RGPD potrebbe applicarsi a te o alla tua organizzazione. Ti consigliamo di utilizzare un professionista legalmente qualificato per discutere dell'RGPD, di come si applica in modo specifico alla tua organizzazione e di come garantire la conformità.

MICROSOFT ESCLUDE OGNI GARANZIA, ESPRESSA, IMPLICITA O DI LEGGE NEI CONFRONTI DELLE INFORMAZIONI FORNITE NEL PRESENTE ARTICOLO. Questo articolo viene fornito "com'è". Le informazioni e le indicazioni riportate nel presente articolo, inclusi gli URL e altri riferimenti a siti Web Internet, sono soggette a modifica senza preavviso.

Il presente articolo non implica la concessione di alcun diritto di proprietà intellettuale in relazione ai prodotti Microsoft.  Questo articolo potrà essere duplicato e utilizzato esclusivamente per fini di riferimento interno.  

Data di pubblicazione: settembre 2017<br>
Versione 1.0<br>
© 2017 Microsoft. Tutti i diritti sono riservati.


