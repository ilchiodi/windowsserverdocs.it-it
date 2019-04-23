<properties title="" pageTitle="I nomi di file e i percorsi per gli articoli tecnici di Windows Server 2016" description="Illustra la struttura di file per gli articoli e le convenzioni di denominazione da seguire quando si crea un nuovo articolo." metaKeywords="" services="" solutions="" documentationCenter="" authors="Kathy-Davies" videoId="" scriptId="" manager="required" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="03/14/2016" ms.author="jimpark; tysonn" />

# <a name="file-names-and-locations-for-windows-server-technical-articles"></a>I nomi di file e i percorsi per gli articoli tecnici di Windows Server

Conoscenza e in base alle regole consente di ottenere la richiesta pull accettata più velocemente.

+ [regole]
+ [Pattern]
+ [Approvazione dei nomi file]

## <a name="rules"></a>Regole

- Tutti i file devono essere markdown e usare l'estensione di file con estensione MD.
- Mantenere i nomi dei file da 3 a 5 parole, se possibile: 10 parole è veramente troppo lungo per la leggibilità e il motore di ricerca.
- Usare lettere minuscole e solo lettere, numeri e trattini.
- Senza spazi o caratteri di punteggiatura. Usare un trattino per separare le parole e i numeri nel nome del file.
- Usare la versione abbreviata di verbi di azione, non il '-ing' modulo: `deploy-nano-server` non `Deploying-Nano-Server`
- Non inserire parole brevi, ad esempio un oggetto e,, o.
- Parole di controllo ortografico in caso contrario. evitare gli acronimi non necessari o non approvati nei nomi dei file
- I nomi di file devono essere univoci - anziché `overview.md` usare `storage-spaces-overview.md`

Gli acronimi e initialisms nei nomi dei file - linee guida specifiche:

- Segui le istruzioni di Microsoft esistenti per le abbreviazioni nome accettabile
- Le abbreviazioni standard del settore sono accettabili in base alle esigenze nei nomi dei file.

## <a name="pattern"></a>Pattern

Esaminare l'elenco di articoli nel repository per avere un'idea dei nomi esistenti. Ecco il modello generale:

 **component-topic-title.md**
 
Ad esempio, `storage-spaces-direct-overview.md`

## <a name="file-name-approval"></a>Approvazione dei nomi file

Quando si invia un nuovo file, i revisori delle richieste pull verificare il nome e forniscono commenti e suggerimenti tramite il flusso di commenti richiesta pull se sono necessarie modifiche. Il nome del file deve essere corretto prima che venga accettata la richiesta pull. I collaboratori possono facilmente il push dell'aggiornamento della richiesta pull in sospeso.

## <a name="folders-in-the-repo"></a>Cartelle nel repository

Usare la struttura di cartelle esistente. Non crea cartelle senza concedere l'approvazione da un amministratore di repository. Comunicare con essi se si ritiene che sia necessario una nuova cartella.

Il repository GitHub deve avere un semplice e relativamente flat la struttura di cartelle: \<area >\\\<tecnologia >: per l'esempio storage\data-la deduplicazione. In questo modo offre i vantaggi seguenti:
 - È molto semplice.
 - È più da vicino al funzionamento di Azure.com
 - Semplifica inoltre per i writer/PMs non trovare rapidamente i relativi argomenti: è necessario esaminare altre cartelle cercando annidata in cui una determinata tecnologia intorno.
 - Conserva l'URL breve, che risulta appropriato per SEO ed esperienza utente, i clienti in alcuni casi esaminare l'URL per i suggerimenti.

## <a name="changing-case-in-file-names"></a>Modificare maiuscole e minuscole nei nomi dei file

Sistemi operativi Windows viene operata la distinzione. Se è necessario modificare un nome di file per correggere le maiuscole e minuscole, è preferibile apportare una modifica al contenuto del file, a meno che non ci si trova un Linux o Mac. Ad esempio: 

  biztalk-administration-and-Development-Task-List-in-BizTalk-Services --> biztalk-services-administration-and-development-task-list

Usare il `git mv` comando per rinominare un file:
```
  git mv <WindowsServerDocs/tech-area/subarea/current-file-name.md> <WindowsServerDocs/tech-area/subarea/new-file-name.md>
```

### <a name="contributors-guide-links"></a>Collegamenti della Guida collaboratori

- [Indice di articoli con indicazioni](./contributor-guide-index.md)


<!--Anchors-->
[regole]: #rules
[Pattern]: #pattern
[Approvazione dei nomi file]: #file-name-approval
