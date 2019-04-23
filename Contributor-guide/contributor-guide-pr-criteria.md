# <a name="quality-criteria-for-pull-request-review"></a>Esaminare i criteri di qualità per la richiesta pull

Le richieste pull devono soddisfare criteri minimi venga accettata per l'integrazione nel repository. I revisori delle richieste pull a livello generale esaminare solo ciò che è nuovo o modificato, utilizzando il blocco e non bloccante qualità esaminare gli elementi elencati in questo articolo per valutare le modifiche. I revisori possono rivolgersi per ulteriori informazioni o per un richiedente risolvere i problemi prima di accettare una richiesta pull. Il richiedente è responsabile di rendere le modifiche.

>[!IMPORTANT] 
>Apre una richiesta pull Attiva controlli della qualità e la pubblicazione per il sito di staging interno. È responsabilità dell'utente per esaminare entrambi, mediante collegamenti nei commenti \(nella scheda conversazione della richiesta pull, fare clic su **Mostra tutte le verifiche** > **dettagli**\). Dopo che è stato fatto, indicare aggiungendo il **"ready-to-merge"** etichetta per la richiesta pull. \(Fare clic su **etichette** o l'icona dell'ingranaggio a destra del flusso di commento della richiesta pull.) Se non è possibile aggiungere l'etichetta \(provenire i collaboratori hanno segnalato questo), aggiungere "ready-to-merge' un commento alla richiesta pull e Invia messaggio di posta elettronica all'alias per i revisori, wssc-pra@microsoft.com.

## <a name="blocking-content-quality-items"></a>Gli elementi della qualità del contenuto di blocco

Gli aggiornamenti devono essere conformi con i criteri seguenti per eseguire il merge.

| Category | Revisione della qualità |
|----------|---------------------|
|Prerequisiti| La richiesta pull non ha alcun:<br>-gli avvisi o errori di convalida<br>-conflitti di unione<br>-regressioni ovvie del contenuto<br>-incorporati repository o eventuali file anomali e|
|Integrità del repository |Richiesta pull contiene meno di 100 file modificati a meno che non sta aggiornando un ramo di rilascio dal master o per eseguire un aggiornamento bulk simile, ad esempio una correzione dei metadati. (Davvero, una richiesta pull dovrebbe contenere molte meno, ma dopo 100 file modificati GitHub non visualizza le differenze).|
|Integrità del repository| Richiesta pull non ottenere merge dalla persona che ha creato la richiesta pull.|
|denominazione |I nomi di file per i nuovi file seguono il [convenzioni di denominazione di file](file-names-and-locations.md).|
|denominazione |Le nuove cartelle create nel repository seguono le [linee guida per la denominazione delle cartelle](file-names-and-locations.md#folder-names-in-the-repo).|
|Denominazione/SEO|Il titolo H1 contiene informazioni sufficienti per descrivere il contenuto dell'articolo, distinguerlo da altri articoli di eseguire il mapping a parole chiave dei clienti probabile. Ad esempio, un titolo H1 di "Panoramica" non soddisfa questo criterio.|
|Content|È un documento tecnico. Vedere le [mappa posizioni indicazioni](content-channel-guidance.md).|
|Content|Dispone di un paragrafo introduttivo e un corpo procedurale o concettuale del contenuto. Ha contenuto sufficiente per giustificarne l'esistenza come articolo separato e non è un piccolo frammento di informazioni.|
|Content| Ne consegue la formattazione standard e la progettazione del contenuto: elementi che devono essere elenchi numerati sono numerati, puntati gli elementi che devono essere elenchi non ordinati. Si tratta di base dell'usabilità.|
|Content| Non contiene grafica insolita, architettura o strutture né progettazione ovviamente non standard.|
|Funzionalità di sito/progettazione| L'articolo non fornisce un sommario creato manualmente all'interno del contenuto. L'articolo deve basarsi su H2s per il sommario della pagina.|
|Funzionalità di sito/progettazione| Se vengono utilizzati i titoli H2, sono presenti almeno due. Qualsiasi testine H3 sono precedute da un solo titolo H2. Usando un solo titolo H2 crea un sommario dell'articolo singolo elemento. Utilizzare un titoli H2 prima un titolo H3 per assicurare che un sommario viene creato.|
|Markdown| Articolo non contiene blocchi di codice HTML. HTML minori inline è consentito: ad esempio apice, pedice, caratteri speciali e altri formattazione secondarie che non supporta Markdown. Le tabelle HTML sono consentite solo se la tabella contiene puntato o elenchi numerati, ma che in genere indica che il contenuto potrebbe essere semplificato in modo che possono essere codificata in Markdown.|
|Markdown   |Gli elementi markdown personalizzati vengono usati nei casi appropriati. Ad esempio: Le note sono codificate tramite la! Estensione di nota, non come testo normale.|
|Immagini|Le immagini includono il testo alternativo. **Questo è un requisito di accessibilità che deve essere acquisito come parte dei criteri di rilascio.** [Linee guida qui](https://worldready.cloudapp.net/Styleguide/Read?id=2665&topicid=28349). |
|Immagini |Le immagini GIF animate non sono accettate nel repository.|
|Immagini | Le immagini hanno una risoluzione chiara, sono ortografici e non contenere nessuna informazioni private [vedere le linee guida immagine](https://github.com/Azure/azure-content/blob/master/contributor-guide/create-images-markdown.md). |
|Gestione temporanea| L'articolo è stato visualizzato in anteprima in gestione temporanea e non contiene eventuali errori di formattazione ovvi:<br>-Un elenco numerato o puntato visualizzato come paragrafo <br> -Codice in un blocco di codice visualizzati in parte nel blocco di codice e in parte al suo esterno <br>-Passaggi numerati con numerazione in modo non corretto a causa di problemi nei rientri|

## <a name="non-blocking-content-quality-items"></a>Non bloccanti qualità del contenuto

Per questi elementi, i revisori delle richieste pull forniscono commenti e suggerimenti e istruzioni per l'autore dovrà attenersi apportando correzioni per una richiesta pull successiva. Tuttavia, questi commenti e suggerimenti non blocca la decisione di unione. Gli autori sono tenuti entro 3 giorni lavorativi con correzioni.

| Category | Revisione della qualità |
|----------|---------------------|
|Content|Gli articoli devono includere una sezione "passaggi successivi" alla fine con 1-3 passaggi successivi pertinenti e interessanti. Deve essere incluso testo breve che aiuta i clienti comprendere perché i passaggi successivi sono rilevanti. (Solo nuovi articoli).
|Immagini|Le immagini usano lo stile di callout corretto e il colore e schermate usano lo stile di bordi e segnaposto corretto. [Vedere le linee guida immagine](https://github.com/Azure/azure-content/blob/master/contributor-guide/create-images-markdown.md).|
|Funzionalità di sito/progettazione|I titoli H2, visualizzati nel sommario della pagina, dovrebbero idealmente estendersi su non più di 2 righe. Titoli più lunghi rendere più difficile analizzare il sommario dell'articolo.|
|Convenzioni stilistiche|Tutti i titoli e le intestazioni sono maiuscola.|
|Process|Quando si elimina o Rinomina un articolo, assicurarsi di che seguire il processo. I revisori devono aggiungere il commento e il collegamento seguente in un commento della richiesta di pull:<br><br>*Verificare di che aver seguito il processo nella Guida per i collaboratori: [Modificare il nome di un articolo o eliminarlo](rename-or-retire.md).*|
