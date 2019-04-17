---
title: Guida di stile per la progettazione e il testo dell'interfaccia utente di Windows Admin Center
description: Interfaccia utente di Windows Admin Center progettazione e il testo Guida di stile SDK
ms.technology: manage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.date: 10/17/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ab5bee55975b803a77db0b6cdb179b76590e1d83
ms.sourcegitcommit: 56cccb09a35b2d3eef9056e2406c3a1762d28682
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2018
ms.locfileid: "4739981"
---
# Guida di stile per la progettazione e il testo dell'interfaccia utente di Windows Admin Center

>Si applica a: Windows Admin Center

In questo argomento viene descritto l'approccio generale per la scrittura del testo dell'interfaccia utente di Windows Admin Center, nonché alcune convenzioni specifiche e le operazioni da intraprendere.

Windows Admin Center e tutte le estensioni devono seguire [i principi della comunicazione di Microsoft](https://docs.microsoft.com/style-guide/brand-voice-above-all-simple-human) in modo che l'esperienza sia di facile utilizzo e comprensione. Questa Guida di stile si basa sui principi della comunicazione, nonché sulla [guida di stile di scrittura Microsoft](https://docs.microsoft.com/style-guide/welcome/), pertanto leggi le informazioni in entrambe le risorse in relazione ad esempio [all'accesso facilitato](https://docs.microsoft.com/style-guide/accessibility/accessibility-guidelines-requirements), [agli acronimi](https://docs.microsoft.com/style-guide/acronyms)e [alla scelta delle parole](https://docs.microsoft.com/style-guide/word-choice/) , come ad esempio [per favore](https://docs.microsoft.com/style-guide/a-z-word-list-term-collections/p/please) e [siamo spiacenti](https://docs.microsoft.com/style-guide/a-z-word-list-term-collections/s/sorry).

## Pulsanti

- I pulsanti devono essere costituiti da una sola parola se possibile e in particolare se pensi di localizzare lo strumento. Due o tre è OK ma cerca di evitare più. Se disponi di quattro parole o più lunghi, sarebbe preferibile usare un controllo collegamento.
- Le etichette dei pulsanti devono essere concise, specifiche e di chiara interpretazione. Invece di un pulsante "Invia" generico, utilizza un verbo corrispondente all'azione dell'utente, ad esempio "Crea", "Elimina", "Aggiungi", "Formatta" e così via.
- Se un pulsante segue una domanda, l'etichetta deve corrispondere chiaramente alla domanda (in genere "Sì" o "No").

## Uso delle maiuscole

Per l'uso delle [maiuscole e minuscole](https://docs.microsoft.com/style-guide/capitalization) si segue lo stile di Microsoft, con la maiuscola a inizio frase praticamente per tutto.

| Elemento dell'interfaccia utente              |Uso delle maiuscole|Commenti|
|-------------------------|--------------|--------|
|Notifiche (ad esempio ANTEPRIMA) |Tutto maiuscolo      ||
|Tutti gli altri elementi          |Stile di frase|Tuttavia, vi sono alcune eccezioni in cui le proprietà degli oggetti di superficie di WMI o PowerShell sono fuori dal nostro controllo.|

## Due punti

Utilizzare i due punti di introdurre gli elenchi. Ad esempio:

    Choose one of the following:
    Cats
    Dogs
    Quokkas

Non usare i due punti in testo dell'interfaccia utente quando l'etichetta è in un diverso di riga dalla cosa le etichette o quando è presente una chiara distinzione tra l'etichetta e la cosa è etichettando.

Utilizzare i due punti in testo dell'interfaccia utente quando l'etichetta è la stessa riga del testo etichette e hai bisogno impedire che i due elementi eseguiti insieme.

## Messaggi di conferma

Le finestre di dialogo di conferma sono utili quando continuare potrebbe avere risultati imprevisti, ad esempio la perdita di dati. Devono contenere informazioni utili, acquisibile con un risultato chiaro, in particolare per gli eventi che non può essere annullata. 

- Assicurati che sia necessario un messaggio di conferma. Se non esiste alcun nuove info di rendere disponibile (ad esempio, "sei che?"), quindi un messaggio di conferma potrebbe non essere necessario.  
- Verifica che il cliente vuole procedere con l'azione.
- Assicurati che all'istruzione principale (intestazione) e testo descrittivo (corpo) non sono ridondanti.
- Nell'intestazione, definire i risultati possibili come una domanda o un'istruzione su cosa accadrà successivamente. Ad esempio, "cancellare tutti i dati di questa unità? oppure "Si sta per cancellare tutti i dati".
- Aggiungere i dettagli del corpo. Se non esiste una variabile, ad esempio il nome dell'elemento che sei sulla modifica, includerlo qui.
- Includere una domanda semplice (nell'intestazione o nel corpo della) che fotogrammi chiaro scegliere tra due pulsanti di azione.
- Per una scelta complessa, Usa Sì/No pulsanti, che incoraggiano lettura attenzione. Per una scelta più semplice, Usa pulsanti specifici per l'azione, ad esempio eliminare tutti o annullare.

## Esperienze di prima esecuzione

La prima volta che un utente visita una pagina, hai l'opportunità di aiutarlo a iniziare usare il tuo strumento. Questo aiuto può essere:

- Una stringa di testo in una pagina vuota con brevi istruzioni su come iniziare, ad esempio, "Seleziona Aggiungi per aggiungere un'app".
- Un collegamento del controllo che consente all'utente di iniziare, ad esempio "Aggiungi un'app per iniziare".
- Una piccola e breve animazione o un video che mostra all'utente come iniziare.

Di seguito sono riportati alcuni suggerimenti della guida di stile di Windows:

### 1. Sii utile

- Evita il linguaggio e lo stile marketing.
- Quando dimostri o suggerisci qualcosa, assicurati che il risultato finale sia chiaro, mostrare semplicemente al cliente come si fa qualcosa non è efficace se non sa perché lo sta facendo.
- Non mostrare suggerimenti se il cliente non ne ha bisogno.

### 2. Mostra, non dire

Mantieni il testo il più semplice possibile (pensa a piccole animazioni o video).

### 3. Non esagerare

- Limita i popup e i suggerimenti a 4 per sessione di utilizzo combinati, comprese le notifiche di sistema e le notifiche della shell.
- Assicurati che la tempistica dei popup sia utile.
- Non impedire al cliente di eseguire qualcosa.
- Verifica che i popup possano essere chiusi facilmente.

### 4. Mantieni il contesto

- I momenti di insegnamento sono più efficaci se presentati al momento giusto.
- Se crei tutorial o presentazioni, mantieni le informazioni concrete.
- Nessuna "indoratura" marketing, concentrati su suggerimenti e trucchi specifici.
- Fornisci ai clienti il modo per tornare al tutorial in un secondo momento, se pertinente (le persone spesso non memorizzano le informazioni la prima volta mentre le istruzioni di configurazione potrebbero essere importanti una sola volta).
- La messaggistica non causata da un problema è un momento naturale per l'apprendimento e/o il piacere, mantienila semplice e informativa.

### 5. Riduci al minimo le installazione complesse

Quando è necessario che il cliente esegua un'altra azione per provare il valore complessivo (accesso a un servizio online e così via), rendilo il più semplice possibile.

- La messaggistica deve essere breve e diretta.
- Evita che i clienti si allontanino. Se possibile, fornisci un mezzo per la connessione ovunque si trovino.
- Se puoi, offri l'opzione per farlo in un secondo momento, quindi ricorda loro di farlo in un secondo momento.
- Se li fai uscire dall'esperienza, fornisci un modo per tornare indietro rapidamente e facilmente.

## Collegamenti della Guida

Di seguito sono riportati alcuni suggerimenti della guida di stile di Windows:

### Quando è necessario forniamo un collegamento alla Guida?

Quasi mai. Fornire un collegamento alla Guida solo quando:

- Esiste una domanda ovvia e importante che i clienti potrebbero avere mentre si trovano nell'interfaccia utente la risposta a cui verrà consentire loro l'esito positivo nell'attività dell'interfaccia utente. 
- Non c'è spazio sufficiente nell'interfaccia utente per fornire la quantità di informazioni necessarie per gli utenti hanno esito positivo nell'attività dell'interfaccia utente. 

### In cui deve aiutare i collegamenti vengono visualizzati? 

- Collegamenti testo dovrebbero essere visualizzato più vicino l'elemento dell'interfaccia utente che la Guida è destinata a minori possibile. 
- Se è necessario fornire un collegamento di testo che si applica a un intero schermo dell'interfaccia utente, posizionalo nella parte inferiore sinistra dello schermo. 
- Se Fornisci un collegamento tramite un pulsante della Guida (?), la descrizione comando deve essere "Guida".

### Quale URL è consigliabile utilizzare?

Mai collegare direttamente a un indirizzo web, Usa invece un servizio di reindirizzamento.

Gli sviluppatori di Microsoft devono usare un FWLink tranne quando un collegamento alla Guida che gli utenti potrebbero essere necessario digitare manualmente, nel qual caso utilizzare un collegamento aka.ms/Project-Rome (purché la destinazione dell'URL è un sito Web che riconosce automaticamente le impostazioni locali del browser, ad esempio Docs.microsoft.com)

### Linee guida per il testo 

- Usare frasi intere.
- Non includere, ad eccezione dei punti interrogativi punteggiatura finale. 
- Non è necessario usare lo stesso testo come il titolo di attività. usare il testo appropriato nel contesto dell'interfaccia utente, ma assicurati che esiste una connessione logica tra i due. Ad esempio: 
- Collegamento alla Guida: che cosa sono i rischi di consentire le eccezioni? 
- Titolo dell'argomento: "Che consente di un programma di comunicare attraverso Windows Firewall"
- Essere il più specifici possibile sul contenuto dell'argomento della Guida. 
    - Il nostro stile
        - Come Windows Firewall consente di proteggere il mio computer?
        - Perché le luci possono migliorare un'immagine
    - Non nostro stile
        - Altre informazioni su Windows Firewall
        - Altre informazioni sulla gestione dei colori
        - Altre informazioni
- Usa l'intera frase per il testo del collegamento, non solo le parole chiave. 
    - Il nostro stile 
        - [Quali sono i rischi di consentire le eccezioni?]()
    - Non nostro stile
        - Quali sono i [rischi di consentire le eccezioni]()? 

## Messaggi di errore

Ecco alcune indicazioni utili adattato dalla Guida di stile di Windows:

Scrittura di un buon messaggio è un equilibrio tra fornendo sufficiente spiegazione ma che non è eccessivamente tecnico; tra stato casuale e personalizzabile, ma non fastidiosa o offensive.

### Linee guida generali

Usa un unico messaggio per ogni caso di errore.

#### Intestazioni

- Mantenerla breve e spiegare conciso qual è il problema o **idealmente operazione da eseguire**. <br>Alcune aree dell'interfaccia utente possono contenere le intestazioni che troncare invece di ritorno a capo quando stanno troppo tempo, pertanto sempre aggiornato per questi.
- Usa la soluzione nell'intestazione se si tratta di un semplice passaggio.
- Assicurati che il titolo sia direttamente correlato al pulsante nel caso in cui il visualizzatore ignora il testo del corpo.
- Evita di usare "Si è verificato un problema" nelle intestazioni, a meno che non è non disponibile alcuna altra scelta. Essere più specifiche sul problema.
- Evita di usare le variabili (ad esempio i nomi di file, cartelle e app) nelle intestazioni. Inserirli nel corpo.

#### Corpo

- Se l'intestazione sufficientemente spiega il problema o la soluzione, non è necessario il corpo del testo.
- Evita di ripetere il titolo del messaggio parafrasandolo.
- Comunicare in modo chiaro e conciso che cos'è la soluzione.
- Concentrati sulla dando fatti prima di tutto.
- Non incolpi agli utenti per l'errore.
- Se non esiste un codice di errore associato all'errore e se si ritiene che incluso il codice di errore potrebbe essere utile il cliente o il supporto tecnico Microsoft per la ricerca del problema, includerla direttamente sotto il testo del corpo e Scrivi come indicato di seguito:

    Codice di errore: # # #

    Se il cliente ha tutte le informazioni necessarie per risolvere l'errore senza il codice, non dovrai includerlo.

#### Pulsanti

- Scrivere testo del pulsante in modo che sia una risposta specifica all'istruzione principale. Se non è possibile, Usa "Chiudi" per il testo del pulsante annullamento delle condizioni (anziché "Accettabile" o "Eseguita").
- Se hai più di un pulsante, Rendi il pulsante all'estrema sinistra l'azione che è consigliabile richiedere all'utente. Rendere pulsante all'estrema destra l'azione più conservativo, ad esempio "Annulla".

#### Collegamenti della Guida

Considera solo collegamenti della Guida per i messaggi di errore che non è possibile apportare specifici e operativi.

## Testo di stato null

Ecco alcune informazioni dalla Guida di stile di Windows.

Si verifica uno stato null quando i dati dei clienti o il contenuto è assente da un'app o funzionalità, quando non vengono restituiti dopo una ricerca oppure quando ritenuto necessario informazioni risulta mancante da un modulo, ad esempio informazioni per una transazione di fatturazione.

### Linee guida

 - Se possibile, usare situazioni stato null come un'opportunità per formare gli utenti su come usare la funzionalità (come, ad esempio aggiungere musica, in cui cercare immagini e così via).  
- Se hai un titolo nell'interfaccia utente, illustrano l'azione da intraprendere per "correzione" lo stato null (ad esempio, "Aggiungi alcune musica") 
- Divertiti con il testo. Questo spazio può essere una buona opportunità per fornire e divertimento poiché che probabilmente non sarà visibile più volte. 
- Evitare "È tristi qui." Si tratta di sei e ha stato costituire. 
- Evitare, ad esempio "Non hai collegato la stampante?" OK confermare di utilizzare una sola volta, ma questo formato tende a ottenere costituire e alle domande aggiuntive onere/pressione del cliente. Anche possibile sentirti condescending. 
- Gamma nel testo di stato null è un'ottima soluzione. 

### Esempi

- "Aggiungi un utente come preferito, e verrà visualizzato qui."
- "OK qualsiasi gli obiettivi o orgogliosi particolarmente dei clip di gioco? Aggiungerli al tuo showcase."
- Del "Nessun in una parte ancora. Avviare un computer!"
- "Quando un utente si aggiunge come un amico, vedrai qui."
- "Quando vari come sbloccare gli obiettivi, registrare clip di gioco e aggiungere gli amici, vedrai lo tutti qui."
- "Amici preferiti verranno visualizzato qui, in modo da visualizzare quando che siano in linea e cosa loro fino a."

## Segni di punteggiatura

- Nessuna punteggiatura finale (punti, punti interrogativi) per intestazioni o frasi incomplete. Fa eccezione la finestra di dialogo in cui nell'intestazione è presente una domanda
- Utilizza le linee guida della Guida di stile Microsoft per [i punti](https://docs.microsoft.com/style-guide/punctuation/periods) e [i punti interrogativi](https://docs.microsoft.com/style-guide/punctuation/question-marks).

## Messaggi di stato

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
|In corso         |Inizia con l'azione che stai eseguendo e finisci con l'ellissi per indicare che l'operazione è in corso. Ecco un esempio:<br> *Creazione del volume "dati dei clienti" in corso...*|
|Operazioni riuscita             |Inizia con l'azione e termina con l'indicazione che è stata completata. Ecco un esempio:<br> *Creazione del volume "dati dei clienti" completata.*|
|Errore             |Inizia con "Non è possibile" e termina con l'operazione che non è stata eseguita. Ecco un esempio:<br> *Non è possibile creare il volume "dati dei clienti".*|

## Descrizioni comandi

Le descrizioni comandi buona brevemente descrivono controlli senza etichetta o forniscono un po' di informazioni aggiuntive per i controlli con etichettati, quando ciò è utile. Possono inoltre possono aiutare i clienti esplorare l'interfaccia utente offrendo aggiuntive, ovvero non ridondante, ovvero informazioni le etichette di controllo, le icone, collegamenti e così via.

Le descrizioni comandi devono essere utilizzate con moderazione o non affatto. Possono essere un'interruzione per il cliente, in modo che non includono una descrizione comando che semplicemente ripete un'etichetta o stati l'ovvia. Dovrebbe sempre aggiungere informazioni importanti.

|    Contesto                                 |    Come scrivere le descrizioni comandi    |
|    -----------------------                 |    -------------------------    |
|Quando un controllo o un elemento dell'interfaccia utente è senza etichetta...|Usa una frase sostantivo semplici e descrittivo. Ad esempio:<br> Evidenziatore |
|Quando un elemento dell'interfaccia utente con etichetta, ma il suo scopo chiarimento...|<ul><li>Descrivi brevemente il modo operazioni possibili con questo elemento dell'interfaccia utente. </li><li>Utilizzare il modulo imperativo verbo. Ad esempio, "trovare il testo in questo file" (non "trova il testo nel file").</li><li>Non includere punteggiatura finale, a meno che non sono presenti più frasi complete.</li> </ul>|
|Quando un'etichetta di testo viene troncato o probabilmente troncare in alcune lingue...|<ul><li>Fornire l'etichetta untruncated nella descrizione comando.</li><li>Facoltativo: In un'altra riga, fornisci una descrizione comprensibili, ma solo se necessario.</li><li>Non fornisci una descrizione comando se le informazioni untruncated viene fornita altrove nella pagina o nel flusso.</li></ul>|
|Se è disponibile un tasto di scelta rapida...|<ul><li>Facoltativo: Fornire i tasti di scelta rapida tra parentesi dopo l'etichetta o frase descrittiva, ad esempio "Stampa (Ctrl + P)" o "Ricerca di testo in questo file (Ctrl + F)"</li><li>È possibile aggiungere una descrizione comando comprensibili utile tasti di scelta rapida, ma evita di aggiungere una descrizione comando solo per mostrare un tasto di scelta rapida. </li></ul>|