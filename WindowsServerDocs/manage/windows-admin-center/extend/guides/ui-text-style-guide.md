---
title: Guida di stile per la progettazione e il testo dell'interfaccia utente di Windows Admin Center
description: Interfaccia utente di Windows Admin Center progettazione e testo della Guida di stile SDK
ms.technology: manage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.date: 10/17/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: be41267d6584002ebf87e5fe828a41575d305e1b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445919"
---
# <a name="windows-admin-center-ui-text-and-design-style-guide"></a>Guida di stile per la progettazione e il testo dell'interfaccia utente di Windows Admin Center

>Si applica a: Windows Admin Center

In questo argomento viene descritto l'approccio generale per la scrittura del testo dell'interfaccia utente di Windows Admin Center, nonché alcune convenzioni specifiche e le operazioni da intraprendere.

Windows Admin Center e tutte le estensioni devono seguire [i principi della comunicazione di Microsoft](https://docs.microsoft.com/style-guide/brand-voice-above-all-simple-human) in modo che l'esperienza sia di facile utilizzo e comprensione. Questa Guida di stile si basa sui principi della comunicazione, nonché sulla [guida di stile di scrittura Microsoft](https://docs.microsoft.com/style-guide/welcome/), pertanto leggi le informazioni in entrambe le risorse in relazione ad esempio [all'accesso facilitato](https://docs.microsoft.com/style-guide/accessibility/accessibility-guidelines-requirements), [agli acronimi](https://docs.microsoft.com/style-guide/acronyms)e [alla scelta delle parole](https://docs.microsoft.com/style-guide/word-choice/) , come ad esempio [per favore](https://docs.microsoft.com/style-guide/a-z-word-list-term-collections/p/please) e [siamo spiacenti](https://docs.microsoft.com/style-guide/a-z-word-list-term-collections/s/sorry).

## <a name="buttons"></a>Pulsanti

- I pulsanti devono essere costituiti da una sola parola se possibile e in particolare se pensi di localizzare lo strumento. Due o tre è OK, ma provare a evitare più. Se si dispone di quattro parole o più, sarebbe preferibile usare un controllo collegamento.
- Le etichette dei pulsanti devono essere concise, specifiche e di chiara interpretazione. Invece di un pulsante "Invia" generico, utilizza un verbo corrispondente all'azione dell'utente, ad esempio "Crea", "Elimina", "Aggiungi", "Formatta" e così via.
- Se un pulsante segue una domanda, l'etichetta deve corrispondere chiaramente alla domanda (in genere "Sì" o "No").

## <a name="capitalization"></a>Uso delle maiuscole

Per l'uso delle [maiuscole e minuscole](https://docs.microsoft.com/style-guide/capitalization) si segue lo stile di Microsoft, con la maiuscola a inizio frase praticamente per tutto.

| Elemento dell'interfaccia utente              |Uso delle maiuscole|Commenti|
|-------------------------|--------------|--------|
|Notifiche (ad esempio ANTEPRIMA) |Tutto maiuscolo      ||
|Tutti gli altri elementi          |Stile di frase|Tuttavia, vi sono alcune eccezioni in cui le proprietà degli oggetti di superficie di WMI o PowerShell sono fuori dal nostro controllo.|

## <a name="colons"></a>Due punti

Usare i due punti per introdurre gli elenchi. Ad esempio:

    Choose one of the following:
    Cats
    Dogs
    Quokkas

Non usare i due punti nel testo dell'interfaccia utente quando un'etichetta si trova in un'altra riga dalla cosa che le etichette o quando è presente una chiara distinzione tra l'etichetta e la cosa è l'assegnazione di etichette.

Utilizzare i due punti nel testo dell'interfaccia utente quando un'etichetta è sulla stessa riga del testo ed etichettati ed è necessario impedire che i due elementi sono in esecuzione contemporaneamente.

## <a name="confirmation-messages"></a>Messaggi di conferma

Le finestre di dialogo di conferma sono utili quando si continua potrà avere risultati imprevisti, ad esempio la perdita di dati. Devono contenere informazioni utili, possono essere analizzate con un risultato non crittografato, in particolare per gli eventi che non può essere annullata. 

- Assicurarsi che sia necessario un messaggio di conferma. Se è presente alcuna informazione di nuovo per offrire (ad esempio, "continuare?"), quindi un messaggio di conferma non sia necessario.  
- Verificare che il cliente vuole procedere con l'azione.
- Assicurarsi che l'istruzione principale (intestazione) e testo esplicativo (corpo) non sono ridondanti.
- Nell'intestazione, definiscono i possibili risultati come una domanda o una dichiarazione in merito a ciò che avverrà successivamente. Ad esempio, "cancellare tutti i dati in questa unità? o "Si sta per cancellare tutti i dati".
- Aggiungere i dettagli nel corpo. Se c'è una variabile, ad esempio il nome dell'articolo che si è su Modifica, includerlo.
- Include una semplice domanda (nell'intestazione o nel corpo) che delimita una scelta definita tra due pulsanti di azione.
- Per una scelta complessa, usare Sì/No pulsanti, incoraggiano la lettura di attenzione. Per una scelta più semplice, usare i pulsanti che sono specifici per l'azione, ad esempio elimina tutto o annullare.

## <a name="first-run-experiences"></a>Prima esecuzione esperienze

La prima volta che un utente visita una pagina, hai l'opportunità di aiutarlo a iniziare usare il tuo strumento. Questo aiuto può essere:

- Una stringa di testo in una pagina vuota con brevi istruzioni su come iniziare, ad esempio, "Seleziona Aggiungi per aggiungere un'app".
- Un collegamento del controllo che consente all'utente di iniziare, ad esempio "Aggiungi un'app per iniziare".
- Una piccola e breve animazione o un video che mostra all'utente come iniziare.

Di seguito sono riportati alcuni suggerimenti della guida di stile di Windows:

### <a name="1-be-helpful"></a>1. Sii utile

- Evita il linguaggio e lo stile marketing.
- Quando dimostri o suggerisci qualcosa, assicurati che il risultato finale sia chiaro, mostrare semplicemente al cliente come si fa qualcosa non è efficace se non sa perché lo sta facendo.
- Non mostrare suggerimenti se il cliente non ne ha bisogno.

### <a name="2-show-dont-tell"></a>2. Mostra, non raccontare

Mantieni il testo il più semplice possibile (pensa a piccole animazioni o video).

### <a name="3-dont-overwhelm"></a>3. Non sovraccaricare

- Limita i popup e i suggerimenti a 4 per sessione di utilizzo combinati, comprese le notifiche di sistema e le notifiche della shell.
- Assicurati che la tempistica dei popup sia utile.
- Non impedire al cliente di eseguire qualcosa.
- Verifica che i popup possano essere chiusi facilmente.

### <a name="4-keep-it-contextual"></a>4. Mantenerla contestuali

- I momenti di insegnamento sono più efficaci se presentati al momento giusto.
- Se crei tutorial o presentazioni, mantieni le informazioni concrete.
- Nessuna "indoratura" marketing, concentrati su suggerimenti e trucchi specifici.
- Fornisci ai clienti il modo per tornare al tutorial in un secondo momento, se pertinente (le persone spesso non memorizzano le informazioni la prima volta mentre le istruzioni di configurazione potrebbero essere importanti una sola volta).
- La messaggistica non causata da un problema è un momento naturale per l'apprendimento e/o il piacere, mantienila semplice e informativa.

### <a name="5-minimize-painful-setup"></a>5. Ridurre al minimo il programma di installazione più complesso

Quando è necessario che il cliente esegua un'altra azione per provare il valore complessivo (accesso a un servizio online e così via), rendilo il più semplice possibile.

- La messaggistica deve essere breve e diretta.
- Evita che i clienti si allontanino. Se possibile, fornisci un mezzo per la connessione ovunque si trovino.
- Se puoi, offri l'opzione per farlo in un secondo momento, quindi ricorda loro di farlo in un secondo momento.
- Se li fai uscire dall'esperienza, fornisci un modo per tornare indietro rapidamente e facilmente.

## <a name="help-links"></a>Collegamenti della Guida

Di seguito sono riportati alcuni suggerimenti della guida di stile di Windows:

### <a name="when-should-we-provide-a-help-link"></a>Quando si dovrebbe fornire un collegamento alla Guida?

Quasi mai. Fornire un collegamento alla Guida solo quando:

- È presente un problema ovvio e importante che i clienti possono avere quando sono fisicamente nell'interfaccia utente a cui la risposta aiutano gli amministratori hanno esito positivo nell'attività dell'interfaccia utente. 
- C'è spazio sufficiente nell'interfaccia utente per fornire la quantità di informazioni necessarie per gli utenti riescono all'attività dell'interfaccia utente. 

### <a name="where-should-help-links-appear"></a>Dove dovrebbe aiutare i collegamenti vengono visualizzati? 

- I collegamenti di testo dovrebbero essere visualizzato il più vicino di elemento dell'interfaccia utente che la Guida è indirizzata possibili. 
- Se è necessario fornire un collegamento di testo che si applica a un'intera schermata dell'interfaccia utente, inserirlo nella parte inferiore della schermata. 
- Se si fornisce un collegamento tramite un pulsante della Guida (?), la descrizione comando deve essere "Help".

### <a name="what-url-should-we-use"></a>Quale URL consigliabile usare?

Non collegare mai direttamente a un indirizzo web, usare invece un servizio di reindirizzamento.

Gli sviluppatori di Microsoft devono usare un FWLink tranne quando è un collegamento alla Guida che gli utenti potrebbero dover digitare manualmente, nel qual caso utilizzare un collegamento aka.ms (fino a quando la destinazione dell'URL è un sito Web che riconosce automaticamente le impostazioni locali del browser, ad esempio Docs.microsoft.com)

### <a name="text-guidelines"></a>Linee guida di testo 

- Usare frasi intere.
- Non includere la punteggiatura, ad eccezione di punti interrogativi finale. 
- Non è necessario usare lo stesso testo come il titolo dell'attività; usare il testo che ha senso nel contesto dell'interfaccia utente, ma occorre assicurarsi che vi sia una connessione logica tra i due. Ad esempio: 
- Collegamento alla Guida: Quali sono i rischi di autorizzazione delle eccezioni? 
- Titolo dell'argomento della Guida: "Consentendo a un programma di comunicare tramite Windows Firewall"
- Sia il più preciso possibile sul contenuto dell'argomento della Guida. 
    - Nostro stile di visualizzazione
        - Come Windows Firewall consente di proteggere il computer?
        - Il motivo per cui evidenziazioni possono migliorare un'immagine
    - Non nostro stile di visualizzazione
        - Altre informazioni su Windows Firewall
        - Altre informazioni sulla gestione dei colori
        - Scopri di più
- Usare l'intera frase per il testo del collegamento, non solo le parole chiave. 
    - Nostro stile di visualizzazione 
        - [Quali sono i rischi di autorizzazione delle eccezioni?]()
    - Non nostro stile di visualizzazione
        - Quali sono le [rischi per la concessione di eccezioni]()? 

## <a name="error-messages"></a>Messaggi di errore

Ecco alcune indicazioni adattato dalla Guida di stile Windows:

La scrittura di un messaggio valido è un equilibrio tra fornendo sufficiente spiegazione, ma non in corso eccessivamente technical; tra la occasionali e personalizzabile, ma non fastidioso o offensivi.

### <a name="general-guidelines"></a>Linee guida generali

Usare un messaggio per ogni caso di errore.

#### <a name="headings"></a>Intestazioni

- Essere concisi e spiegare in modo conciso che cos'è il problema oppure **idealmente operazioni da eseguire**. <br>Alcune aree dell'interfaccia utente potrebbero contenere le intestazioni che tronca invece di racchiudere quando sono troppo lunghi, quindi Preparatevi per questi.
- Se si tratta di un semplice passaggio, usare la soluzione nell'intestazione.
- Assicurarsi che l'intestazione è correlato direttamente al pulsante nel caso in cui il lettore ignora il corpo del testo.
- Evitare di usare "Si è verificato un problema" nelle intestazioni, a meno che non si ha altra scelta. Essere più specifici relativi al problema.
- Evitare di usare le variabili (ad esempio nomi di file, cartelle e app) nelle intestazioni. Inserirli nel corpo.

#### <a name="body"></a>Body

- Se l'intestazione sufficientemente spiega il problema o la soluzione, non è necessario il corpo del testo.
- Don't repeat il titolo nel messaggio con una formulazione leggermente diversa.
- Comunicare in modo chiaro e conciso che cos'è la soluzione.
- Concentrarsi sulle offrendo prima di tutto i fatti.
- Non segnala l'errore gli utenti per l'errore.
- Se è presente un codice di errore associato all'errore e se si ritiene che il cliente o il supporto tecnico Microsoft per condurre ricerche sul problema, può essere utile includere il codice di errore includerlo direttamente sotto il testo del corpo e scriverli come indicato di seguito:

    Codice di errore: # # #

    Se il cliente ha tutte le informazioni necessarie per risolvere l'errore senza il codice, non è necessario includerlo.

#### <a name="buttons"></a>Pulsanti

- Scrivere il testo del pulsante in modo che si tratti di una risposta specifica per l'istruzione principale. Se ciò non è possibile, usare "Chiudi" per il testo del pulsante licenziamento (invece di "Okay" o "Done").
- Se si dispone di più di un pulsante, verificare l'azione che l'utente è stimolato a richiedere il pulsante più a sinistra. Rendere il pulsante all'estrema destra l'azione più conservativo, ad esempio "Cancel".

#### <a name="help-links"></a>Collegamenti della Guida

Considerare solo i collegamenti della Guida per i messaggi di errore che non è possibile apportare specifici e pratici.

## <a name="null-state-text"></a>Testo stato null

Ecco alcuni argomenti della Guida dalla Guida di stile Windows.

Stato null si verifica quando i dati del cliente o il contenuto è assente dall'app o funzionalità, quando viene restituito alcun risultato dopo una ricerca o quando richiesto, mancano le informazioni da un form, ad esempio le informazioni per una transazione di fatturazione.

### <a name="guidelines"></a>Linee guida

- Se possibile, usare situazioni dello stato null come un'opportunità per informare gli utenti su come usare la funzionalità (ad esempio, come aggiungere musica, dove per cercare immagini e così via.)  
  - Se si dispone di un titolo nell'interfaccia utente, descrivere l'azione da eseguire "corretti" stato null (ad esempio, "aggiungere alcuni musica") 
  - Buon divertimento con il testo. Quest'area può essere un'opportunità per fornire apprezzate dagli poiché probabile che non verrà visualizzato più volte. 
  - Evitare di "ci sono." Si tratta sad ed è stato eccessivo. 
  - Evitare di domande come "Non sono connesse la stampante?" Ideale per usare una sola volta, ma questo formato tende a ottenere eccessivo e domande inserire onere comporti un utilizzo elevato del cliente. Può anche risultare condescending. 
  - Diversi nel testo di stato null sono un fatto positivo. 

### <a name="examples"></a>Esempi

- "Aggiungere un utente come preferito e sarà possibile visualizzarli qui."
- "Hai qualsiasi traguardi o gioco clip sei particolarmente orgoglioso? Aggiungerli per la presentazione."
- Del "Nessuno della ancora un'entità. Avviare un computer."
- "Quando un utente si aggiunge come friend, sarà possibile visualizzarli qui."
- "Quando resto come raggiungere gli obiettivi, registrare clip giochi e per aggiungere amici, si noterà tutto qui."
- "I tuoi amici preferiti verranno visualizzate qui, in modo da visualizzare quando si è online e che cosa sono in."

## <a name="punctuation"></a>Segni di punteggiatura

- Nessuna punteggiatura finale (punti, punti interrogativi) per intestazioni o frasi incomplete. Fa eccezione la finestra di dialogo in cui nell'intestazione è presente una domanda
- Utilizza le linee guida della Guida di stile Microsoft per [i punti](https://docs.microsoft.com/style-guide/punctuation/periods) e [i punti interrogativi](https://docs.microsoft.com/style-guide/punctuation/question-marks).

## <a name="status-messages"></a>Messaggi di stato

Messaggi di stato sono costituiti da messaggi di tipo popup e notifiche.

|Tipo di stringa         | Note                               |
|------------        |-------------------------------------|
|Avviso popup               |Frase con punteggiatura finale: idealmente con una variabile oggetto in modo che gli utenti possano capire a quale oggetto si riferisce il messaggio nel caso in cui si siano allontanati dall'oggetto|
|Intestazione di notifica|Frase senza punteggiatura finale (titolo): idealmente con una variabile oggetto|
|Dettagli di notifica|Frasi complete, idealmente con un collegamento all'interfaccia utente che visualizza l'oggetto|

Ecco alcuni consigli dettagliati per i messaggi di notifica:

|Tipo di stringa         | Note                               |
|------------        |-------------------------------------|
|Avviato             |Ometti quando possibile: in genere puoi passare direttamente al messaggio che indica un'operazione in corso per ridurre il numero di distrazioni.|
|In progress         |Inizia con l'azione che stai eseguendo e finisci con l'ellissi per indicare che l'operazione è in corso. Di seguito è riportato un esempio:<br> *Creazione del volume "Dati dei clienti"...*|
|Riuscito             |Inizia con l'azione e termina con l'indicazione che è stata completata. Di seguito è riportato un esempio:<br> *È stato creato il volume "Dati dei clienti".*|
|Operazione non riuscita             |Inizia con "Non è possibile" e termina con l'operazione che non è stata eseguita. Di seguito è riportato un esempio:<br> *Non è stato possibile creare il volume "Dati dei clienti".*|

## <a name="tooltips"></a>Descrizioni comandi

Le descrizioni comandi buona brevemente descrivono senza etichetta controlli o forniscono alcune informazioni aggiuntive per i controlli con etichettati, quando ciò è utile. Sono inoltre può aiutare i clienti passare l'interfaccia utente, offrendo aggiuntive, ovvero non ridondante, informazioni sulle etichette del controllo, icone, collegamenti, e così via.

Le descrizioni comandi devono essere utilizzati con moderazione o non ereditarli affatto. Può trattarsi di una cliente, in modo da non includere una descrizione comando che indica l'ovvio o si ripete un'etichetta semplicemente un'interruzione. Sempre consigliabile aggiungere informazioni preziose.

|    Contesto                                 |    Come scrivere le descrizioni comandi    |
|    -----------------------                 |    -------------------------    |
|Quando un controllo o elemento dell'interfaccia utente è senza etichetta...|Usare un semplice e descrittivo sintagma nominale. Ad esempio:<br> L'evidenziazione della penna |
|Quando un elemento dell'interfaccia utente con l'etichetta, ma il suo scopo è necessario un chiarimento...|<ul><li>Descrivere brevemente operazioni possibili con questo elemento dell'interfaccia utente. </li><li>Usare il formato verbo imperativa. Ad esempio, "Trova testo in questo file" (non "trova il testo nel file").</li><li>Non includere la punteggiatura finale, a meno che non sono presenti più frasi complete.</li> </ul>|
|Quando un'etichetta di testo è troncata o probabilità per il troncamento in alcune lingue...|<ul><li>Specificare l'etichetta non troncato nella descrizione comando.</li><li>Facoltativo: In un'altra riga, fornire una descrizione e chiarire, ma solo se necessario.</li><li>Non fornire una descrizione comando se le informazioni non troncata viene fornita in un' posizione nella pagina o nel flusso.</li></ul>|
|Se un tasto di scelta rapida è disponibile...|<ul><li>Facoltativo: Fornire il tasto di scelta rapida in parentesi che seguono l'etichetta o la frase descrittiva, ad esempio "Print (Ctrl + P)" o "Trova testo in questo file (Ctrl + F)"</li><li>È anche possibile aggiungere una scelta rapida da tastiera utili a una descrizione comando e chiarire, ma evitare di aggiungere una descrizione comandi solo per mostrare i tasti di scelta rapida. </li></ul>|