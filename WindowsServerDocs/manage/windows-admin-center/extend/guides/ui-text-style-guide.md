---
title: Guida di stile per la progettazione e il testo dell'interfaccia utente di Windows Admin Center
description: Testo dell'interfaccia utente del centro di amministrazione di Windows e guida di stile di progettazione
ms.technology: manage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.date: 01/17/2020
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: ba3cdb8dbd81ee85b0679905444f35174b8138e0
ms.sourcegitcommit: 840d1d8851f68936db3934c80796fb8722d3c64a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519463"
---
# <a name="windows-admin-center-ui-text-and-design-style-guide"></a>Guida di stile per la progettazione e il testo dell'interfaccia utente di Windows Admin Center

>Si applica a: centro di amministrazione di Windows

In questo argomento viene descritto l'approccio generale per la scrittura del testo dell'interfaccia utente di Windows Admin Center, nonché alcune convenzioni specifiche e le operazioni da intraprendere.

Windows Admin Center e tutte le estensioni devono seguire [i principi della comunicazione di Microsoft](https://docs.microsoft.com/style-guide/brand-voice-above-all-simple-human) in modo che l'esperienza sia di facile utilizzo e comprensione. Questa Guida di stile si basa sui principi della comunicazione, nonché sulla [guida di stile di scrittura Microsoft](https://docs.microsoft.com/style-guide/welcome/), pertanto leggi le informazioni in entrambe le risorse in relazione ad esempio [all'accesso facilitato](https://docs.microsoft.com/style-guide/accessibility/accessibility-guidelines-requirements), [agli acronimi](https://docs.microsoft.com/style-guide/acronyms)e [alla scelta delle parole](https://docs.microsoft.com/style-guide/word-choice/) , come ad esempio [per favore](https://docs.microsoft.com/style-guide/a-z-word-list-term-collections/p/please) e [siamo spiacenti](https://docs.microsoft.com/style-guide/a-z-word-list-term-collections/s/sorry).

## <a name="buttons"></a>Pulsanti

- I pulsanti devono essere costituiti da una sola parola se possibile e in particolare se pensi di localizzare lo strumento. Due o tre sono OK, ma tentano di evitare più tempo. Se si dispone di quattro parole o più, sarebbe preferibile utilizzare un controllo di collegamento.
- Le etichette dei pulsanti devono essere concise, specifiche e di chiara interpretazione. Invece di un pulsante "Invia" generico, utilizza un verbo corrispondente all'azione dell'utente, ad esempio "Crea", "Elimina", "Aggiungi", "Formatta" e così via.
- Se un pulsante segue una domanda, l'etichetta deve corrispondere chiaramente alla domanda (in genere "Sì" o "No").

## <a name="capitalization"></a>Uso delle maiuscole

Per l'uso delle [maiuscole e minuscole](https://docs.microsoft.com/style-guide/capitalization) si segue lo stile di Microsoft, con la maiuscola a inizio frase praticamente per tutto.

| Elemento dell'interfaccia utente              |Uso delle maiuscole|Commenti|
|-------------------------|--------------|--------|
|Notifiche (ad esempio ANTEPRIMA) |Tutto maiuscolo      ||
|Tutti gli altri elementi          |Stile di frase|Tuttavia, vi sono alcune eccezioni in cui le proprietà degli oggetti di superficie di WMI o PowerShell sono fuori dal nostro controllo.|

## <a name="colons"></a>Due punti

Usare i due punti per inserire gli elenchi. Ad esempio:

    Choose one of the following:
    Cats
    Dogs
    Quokkas

Non usare i due punti nel testo dell'interfaccia utente quando un'etichetta si trova su una riga diversa rispetto a quella che etichetta o quando esiste una netta distinzione tra l'etichetta e la caratteristica.

Usare i due punti nel testo dell'interfaccia utente quando un'etichetta si trova sulla stessa riga del testo da essa etichetta ed è necessario evitare che i due elementi vengano eseguiti insieme.

## <a name="confirmation-messages"></a>Messaggi di conferma

Le finestre di dialogo di conferma sono utili quando la continuazione potrebbe avere risultati imprevisti, ad esempio la perdita di dati. Devono contenere analizzabili, informazioni utili con un risultato chiaro, in particolare per gli eventi che non possono essere annullati. 

- Verificare che sia necessaria una conferma. Se non sono presenti nuove informazioni da offrire (ad esempio, "si è certi?"), potrebbe non essere necessario un messaggio di conferma.  
- Verificare che il cliente desideri procedere con l'azione.
- Assicurarsi che l'istruzione principale (intestazione) e il testo esplicativo (corpo) non siano ridondanti.
- Nell'intestazione definire i possibili risultati come una domanda o un'istruzione su cosa accadrà successivamente. Ad esempio, "Cancella tutti i dati in questa unità? oppure "si sta per cancellare tutti i dati".
- Aggiungere i dettagli nel corpo. Se è presente una variabile, ad esempio il nome dell'elemento che si sta cambiando, includerla qui.
- Includere una semplice domanda (nell'intestazione o nel corpo) che incornicia una scelta chiara tra due pulsanti di azione.
- Per una scelta complessa, usare i pulsanti Sì/No, che favoriscono una lettura attenta. Per una scelta più semplice, utilizzare i pulsanti specifici dell'azione, ad esempio delete all o Cancel.

## <a name="first-run-experiences"></a>Esperienze di prima esecuzione

La prima volta che un utente visita una pagina, hai l'opportunità di aiutarlo a iniziare usare il tuo strumento. Questo aiuto può essere:

- Una stringa di testo in una pagina vuota con brevi istruzioni su come iniziare, ad esempio, "Seleziona Aggiungi per aggiungere un'app".
- Un collegamento del controllo che consente all'utente di iniziare, ad esempio "Aggiungi un'app per iniziare".
- Una piccola e breve animazione o un video che mostra all'utente come iniziare.

Di seguito sono riportati alcuni suggerimenti della guida di stile di Windows:

### <a name="1-be-helpful"></a>1. Sii utile

- Evita il linguaggio e lo stile marketing.
- Quando si esegue una demo o si suggerisce qualcosa, verificare che il risultato finale sia chiaro; la semplice visualizzazione del cliente come eseguire un'operazione non è efficace se non ne conosce il motivo.
- Non presentare suggerimenti se il cliente non ne ha bisogno.

### <a name="2-show-dont-tell"></a>2. Mostra, non dire

Mantieni il testo il più semplice possibile (pensa a piccole animazioni o video).

### <a name="3-dont-overwhelm"></a>3. Non esagerare

- Limita i popup e i suggerimenti a 4 per sessione di utilizzo combinati, comprese le notifiche di sistema e le notifiche della shell.
- Assicurati che la tempistica dei popup sia utile.
- Non impedire al cliente di eseguire alcuna operazione.
- Verifica che i popup possano essere chiusi facilmente.

### <a name="4-keep-it-contextual"></a>4. Mantieni il contesto

- I momenti di insegnamento sono più efficaci se presentati al momento giusto.
- Se crei tutorial o presentazioni, mantieni le informazioni concrete.
- Nessuna "indoratura" marketing, concentrati su suggerimenti e trucchi specifici.
- Fornire ai clienti un modo per tornare all'esercitazione in un secondo momento, se pertinente (spesso le persone non conservano le informazioni per la prima volta, ma le istruzioni di configurazione potrebbero essere rilevanti solo una volta).
- La messaggistica non causata da un problema è un momento naturale per l'apprendimento e/o il piacere, mantienila semplice e informativa.

### <a name="5-minimize-painful-setup"></a>5. Riduci al minimo le installazione complesse

Quando è necessario che il cliente esegua un'altra azione per provare il valore complessivo (accesso a un servizio online e così via), rendilo il più semplice possibile.

- La messaggistica deve essere breve e diretta.
- Evita che i clienti si allontanino. Se possibile, fornisci un mezzo per la connessione ovunque si trovino.
- Se puoi, offri l'opzione per farlo in un secondo momento, quindi ricorda loro di farlo in un secondo momento.
- Se li fai uscire dall'esperienza, fornisci un modo per tornare indietro rapidamente e facilmente.

## <a name="help-links"></a>Collegamenti alla guida

Di seguito sono riportati alcuni suggerimenti della guida di stile di Windows:

### <a name="when-should-we-provide-a-help-link"></a>Quando è necessario fornire un collegamento alla guida?

Quasi mai. Fornire un collegamento alla guida solo nei casi seguenti:

- Si tratta di una domanda ovvia e importante che i clienti hanno probabilmente a disposizione mentre si trovano nell'interfaccia utente la cui risposta sarà utile per l'attività dell'interfaccia utente. 
- Nell'interfaccia utente non è disponibile spazio sufficiente per fornire la quantità di informazioni necessarie per l'esito positivo degli utenti nell'attività dell'interfaccia utente. 

### <a name="where-should-help-links-appear"></a>Dove vengono visualizzati i collegamenti alla guida? 

- I collegamenti di testo dovrebbero apparire come vicini all'elemento dell'interfaccia utente che la guida viene indirizzata al più possibile. 
- Se è necessario fornire un collegamento di testo che si applica a un'intera schermata dell'interfaccia utente, posizionarlo nella parte inferiore sinistra della schermata. 
- Se si fornisce un collegamento tramite un pulsante? (?), la descrizione comando dovrebbe essere "Help".

### <a name="what-url-should-we-use"></a>Quale URL usare?

Non collegare mai direttamente a un indirizzo Web, bensì usare un servizio di reindirizzamento.

Gli sviluppatori Microsoft devono usare un FWLink tranne quando si tratta di un collegamento alla guida che gli utenti potrebbero dover digitare manualmente, nel qual caso usare un collegamento aka.ms (purché la destinazione dell'URL sia un sito Web che riconosce automaticamente le impostazioni locali del browser, ad esempio Docs.microsoft.com)

### <a name="text-guidelines"></a>Linee guida per il testo 

- Utilizzare frasi complete.
- Non includere la punteggiatura finale ad eccezione dei punti interrogativi. 
- Non è necessario usare lo stesso testo del titolo dell'attività; usare il testo che ha senso nel contesto dell'interfaccia utente, ma assicurarsi che esista una connessione logica tra i due. Ad esempio: 
- Collegamento alla guida: quali sono i rischi di consentire le eccezioni? 
- Titolo dell'argomento della guida: "consentire a un programma di comunicare tramite Windows Firewall"
- Essere più specifici possibile sul contenuto dell'argomento della guida. 
    - Nostro stile
        - In che modo Windows Firewall proteggere il computer?
        - Motivi per cui le evidenziazioni possono migliorare un'immagine
    - Non lo stile
        - Ulteriori informazioni su Windows Firewall
        - Altre informazioni sulla gestione dei colori
        - Scopri di più
- Usare l'intera frase per il testo del collegamento, non solo per le parole chiave. 
    - Nostro stile 
        - [Quali sono i rischi per consentire le eccezioni?]()
    - Non lo stile
        - Quali sono i [rischi per consentire le eccezioni]()? 

## <a name="error-messages"></a>Messaggi di errore

Ecco alcune indicazioni adattate dalla Guida di stile di Windows:

La scrittura di un buon messaggio è un equilibrio tra fornire una spiegazione sufficiente ma non essere troppo tecnica; tra essere casuali e personali ma non fastidiosi o offensivi.

### <a name="general-guidelines"></a>Linee guida generali

Usare un messaggio per ogni caso di errore.

#### <a name="headings"></a>Titoli

- Mantenerla concisa e spiegare in modo conciso il problema o la **soluzione ideale**. <br>Alcune superfici dell'interfaccia utente possono avere intestazioni che troncano invece di incapsulare quando sono troppo lunghe, quindi è necessario tenere sotto controllo.
- Usare la soluzione nell'intestazione se è un semplice passaggio.
- Verificare che l'intestazione sia correlata direttamente al pulsante nel caso in cui il lettore ignori il testo del corpo.
- Evitare di usare "si è verificato un problema" nelle intestazioni, a meno che non si disponga di un'altra scelta. Essere più specifici del problema.
- Evitare di usare le variabili, ad esempio i nomi di file, cartelle e app, nelle intestazioni. Inserirli nel corpo.

#### <a name="body"></a>Corpo

- Se l'intestazione spiega adeguatamente il problema o la soluzione, non è necessario il testo del corpo.
- Non ripetere il titolo nel messaggio con una formulazione leggermente diversa.
- Comunicazione chiara e concisa della soluzione.
- Concentrarsi innanzitutto sull'assegnazione dei fatti.
- Non incolpare gli utenti dell'errore.
- Se è presente un codice di errore associato all'errore e si ritiene che l'inclusione del codice di errore possa aiutare il cliente o il supporto Microsoft a cercare il problema, includerlo direttamente sotto il testo del corpo e scriverlo come segue:

    Codice errore: ####

    Se il cliente dispone di tutte le informazioni necessarie per risolvere l'errore senza il codice, non è necessario includerlo.

#### <a name="buttons"></a>Pulsanti

- Scrivere il testo del pulsante in modo che sia una risposta specifica all'istruzione principale. Se non è possibile, usare "close" per il testo del pulsante dismissable (invece di "OK" o "Done").
- Se è presente più di un pulsante, rendere il pulsante più a sinistra l'azione che l'utente è incoraggiato a eseguire. Rendere il pulsante più a destra l'azione più conservativa, ad esempio "Annulla".

#### <a name="help-links"></a>Collegamenti alla guida

Considerare solo i collegamenti della Guida per i messaggi di errore che non è possibile rendere specifici e interoperabili.

## <a name="null-state-text"></a>Testo di stato null

Di seguito sono riportate alcune informazioni dalla Guida di stile di Windows.

Lo stato null si verifica quando il contenuto o i dati del cliente sono assenti da un'app o da una funzionalità, quando non vengono restituiti risultati dopo una ricerca o quando le informazioni richieste non sono presenti in un modulo, ad esempio le informazioni di fatturazione per una transazione.

### <a name="guidelines"></a>Linee guida

- Se possibile, usare le situazioni di stato null come opportunità per informare gli utenti su come usare la funzionalità (ad esempio, come aggiungere musica, dove trovare immagini e così via).  
  - Se è presente un titolo nell'interfaccia utente, spiegare l'azione da intraprendere per "correggere" lo stato null (ad esempio, "aggiungere musica") 
  - Divertirsi con il testo. Questo spazio può essere utile perché probabilmente non verrà visualizzato più volte. 
  - Evitare che "si trovi da soli". Questo è triste ed è stato sovrautilizzato. 
  - Evitare domande come "non è stata connessa la stampante?" OK per usare una sola volta, ma questo formato tende a essere utilizzato in modo eccessivo e le domande inseriscono un carico/pressione aggiuntivo per il cliente. Può anche essere condiscendente. 
  - La varietà nel testo di stato null è un aspetto positivo. 

### <a name="examples"></a>Esempi

- "Aggiungi un utente come preferito, che verrà visualizzato qui".
- "Hai ottenuto tutti i risultati o le clip di gioco che sei particolarmente orgoglioso? Aggiungerli alla presentazione ".
- "Non ci sono ancora in un'entità. Inizia una volta! "
- "Quando un utente aggiunge un amico, li visualizzerà qui".
- "Quando si eseguono operazioni quali sbloccare i risultati, registrare clip di gioco e aggiungere amici, il tutto verrà visualizzato qui".
- "I tuoi amici preferiti verranno visualizzati qui, quindi potrai vedere quando sono online e a cosa sono."

## <a name="punctuation"></a>Segni di punteggiatura

- Nessuna punteggiatura finale (punti, punti interrogativi) per intestazioni o frasi incomplete. Fa eccezione la finestra di dialogo in cui nell'intestazione è presente una domanda
- Utilizza le linee guida della Guida di stile Microsoft per [i punti](https://docs.microsoft.com/style-guide/punctuation/periods) e [i punti interrogativi](https://docs.microsoft.com/style-guide/punctuation/question-marks).

## <a name="status-messages"></a>Messaggi di stato

Messaggi di stato sono costituiti da messaggi di tipo popup e notifiche.

|Tipo di stringa         | Note                               |
|------------        |-------------------------------------|
|Avviso popup               |Frase con punteggiatura finale: idealmente con una variabile oggetto in modo che gli utenti possano capire a quale oggetto si riferisce il messaggio nel caso in cui si siano allontanati dall'oggetto|
|Intestazione notifica (titolo) |Distinzione tra maiuscole e minuscole senza terminazione della punteggiatura (intestazione): idealmente con una variabile oggetto|
|Dettagli di notifica|Frasi complete, idealmente con un collegamento all'interfaccia utente che visualizza l'oggetto|

Ecco alcuni consigli dettagliati per i messaggi di notifica:

|Tipo di stringa         | Note                               |
|------------        |-------------------------------------|
|Avviato             |Ometti quando possibile: in genere puoi passare direttamente al messaggio che indica un'operazione in corso per ridurre il numero di distrazioni.|
|In progress         |Inizia con l'azione che stai eseguendo e finisci con l'ellissi per indicare che l'operazione è in corso. Ecco un esempio:<br> *Creazione del volume ' Customer data '...* <br><br>Quando sono presenti più variabili, usare il modello seguente: <br>*Eliminazione della macchina virtuale seguente: {0}; Host: {1}* |
|Operazioni riuscita             |Inizia con l'azione e termina con l'indicazione che è stata completata. Ecco un esempio:<br> *Creazione del volume ' Customer data ' completata.*|
|Operazione non riuscita             |Inizia con "Non è possibile" e termina con l'operazione che non è stata eseguita. Ecco un esempio:<br> *Non è stato possibile creare il volume ' Customer data '.*|

## <a name="tooltips"></a>Descrizioni comandi

Le descrizioni comando valide descrivono brevemente i controlli senza etichetta o forniscono informazioni aggiuntive per i controlli con etichetta, quando questa operazione è utile. Possono anche aiutare i clienti a esplorare l'interfaccia utente offrendo informazioni aggiuntive, non ridondanti, su etichette di controllo, icone, collegamenti e così via.

Le descrizioni comandi devono essere usate in qualsiasi momento. È possibile che si verifichi un'interruzione del cliente, quindi non includere una descrizione comando che ripete semplicemente un'etichetta o ne indica l'ovvio. Deve sempre aggiungere informazioni preziose.

|    Contesto                                 |    Come scrivere le descrizioni comandi    |
|    -----------------------                 |    -------------------------    |
|Quando un controllo o un elemento dell'interfaccia utente è senza etichetta...|Usare una semplice frase del sostantivo descrittivo. Ad esempio:<br> Evidenziazione della penna |
|Quando viene etichettato un elemento dell'interfaccia utente, ma lo scopo necessita di chiarimenti...|<ul><li>Descrivere brevemente le operazioni che è possibile eseguire con questo elemento dell'interfaccia utente. </li><li>Usare il formato verbo imperativo. Ad esempio, "trova testo in questo file" (non "trova il testo in questo file").</li><li>Non includere la punteggiatura finale a meno che non siano presenti più frasi complete.</li> </ul>|
|Quando un'etichetta di testo viene troncata o è probabile che venga troncata in alcune lingue...|<ul><li>Consente di specificare l'etichetta non troncata nella descrizione comando.</li><li>Facoltativo: in un'altra riga, fornire una descrizione chiarificata, ma solo se necessario.</li><li>Non specificare una descrizione comando se le informazioni non troncate vengono fornite altrove nella pagina o nel flusso.</li></ul>|
|Se è disponibile un tasto di scelta rapida...|<ul><li>Facoltativo: specificare il tasto di scelta rapida tra parentesi dopo l'etichetta o la frase descrittiva, ad esempio "stampa (CTRL + P)" o "trova testo in questo file (CTRL + F)"</li><li>È possibile aggiungere un tasto di scelta rapida utile a una descrizione comando chiarificata, ma evitare di aggiungere una descrizione comando solo per mostrare un tasto di scelta rapida. </li></ul>|