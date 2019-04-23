# <a name="rename-or-delete-an-article"></a>Rinominare o eliminare un articolo

Prima di rinominare o eliminare un articolo, è necessario seguire questa procedura per evitare o ridurre il numero di collegamenti interrotti. I clienti odiano collegamenti interrotti e non sono timidi su una voce off quando verranno aggiunti a uno. Si noti che, ridenominazione o eliminazione di un articolo, è l'ultimo passaggio in questo processo, non per la prima.


## <a name="step-1-manage-inbound-links"></a>Passaggio 1: Gestire i collegamenti in ingresso

Determinare se sono presenti tutti i collegamenti in ingresso non Microsoft per il contenuto, ad esempio blog, forum e altri contenuti sul web. Contattare i proprietari dei blog per i collegamenti seguenti, modificare e rimuovere o aggiornare i collegamenti dei post dei forum. Gli strumenti di analitica Web consentono di identificare i collegamenti in ingresso a traffico elevato che potrebbe essere necessario gestire in questo modo.

## <a name="step-2-remove-all-crosslinks-to-the-article-from-the-windowsserverdocs-pr-repository"></a>Passaggio 2: Rimuovere tutti i collegamenti incrociati all'articolo dal repository WindowsServerDocs-richiesta pull

1. Aggiorna locale ramo – eseguire `git pull upstream <branch>`, Internet Explorer per l'aggiornamento dal master, eseguire `git pull upstream master`

2.  Analizza la cartella WindowsServerDocs-richiesta pull per trovare articoli che hanno un collegamento all'articolo che si desidera rinominare o ritirare, quindi aggiornare gli articoli per rimuovere i collegamenti o sostituirli con collegamenti a un altro articolo. È possibile usare una ricerca e sostituire l'utilità per individuare collegamenti incrociati se è installato. In caso contrario, è possibile usare Windows PowerShell:

 a. Avviare Windows PowerShell.

 b. Al prompt di PowerShell, passare alla cartella WindowsServerDocs-richiesta pull:

 `cd WindowsServerDocs-pr\WindowsServerDocs-pr`

 c. Eseguire un comando per elencare tutti i file che contengono un collegamento all'articolo per rinominare o eliminare:

 `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name`

  Per inviare l'elenco di nomi di file in un file di testo (in questo caso, denominato psoutput.txt), eseguire:

  `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name | Out-File C:\Users\<your account>\psoutput.txt`

3. Aggiungere e salvare tutte le modifiche, eseguirne il push nel fork e aprire una richiesta pull. Per istruzioni, vedere [comandi Git per creare o aggiornare un articolo](git-steps-create-update-content.md).

## <a name="step-3-update-fwlinks"></a>Passaggio 3: Aggiornare fwlink

Controllare lo strumento FWLink qualsiasi fwlink che fanno riferimento all'articolo. Puntare qualsiasi fwlink al contenuto di sostituzione. Se non si usa l'alias che possiede il collegamento, creare un join. Se i proprietari non aggiorna il collegamento, aprire un ticket con Microsoft.com per visualizzare il collegamento stato modificato. Altre informazioni - [wiki interno](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Manage%20inbound%20links%20to%20retired%20topics.aspx).

## <a name="step-4-remove-crosslinks-to-the-article-from-table-of-contents"></a>Passaggio 4: Rimuovere i collegamenti incrociati all'articolo dal sommario

Funziona con la persona che gestisce il file ToC.md. Questo file consente di popolare il sommario a sinistra nella raccolta di documentazione tecnica. Se non si conosce chi contattare, Invia messaggio di posta elettronica wssc-pra@microsoft.com.

## <a name="step-5-add-redirects"></a>Passaggio 5: Aggiungere reindirizzamenti
Se si sta rinominando o l'eliminazione di un file, aggiungere un reindirizzamento in modo da non interrompono i collegamenti esistenti:

1. Lasciare il vecchio file nel percorso esistente con il nome del file esistente.
2. Sostituire il contenuto del file con questo frammento di metadati:
   ```
   ---
   redirect_url: <redirection-URL-or-file>
   ---
   ```
   \<Reindirizzamento URL-o-file > è l'URL completo di un percorso diverso o è il percorso + nome file da un altro argomento nello stesso repository OPS.

   Ad esempio - sarebbe l'intero file:

   ```
   ---
   redirect_url: ../../failover-clustering/whats-new-in-failover-clustering
   ---
   ```

3. Dopo la creazione di una richiesta pull per il reindirizzamento, fare clic su collegamenti nei commenti per la richiesta pull - se il reindirizzamento funzioni si dovrebbe essere l'argomento di destinazione.

## <a name="step-6-rename-or-delete-the-article"></a>Passaggio 6: Rinominare o eliminare l'articolo

Se non si usa un reindirizzamento, eseguire questo passaggio dopo aver completato i passaggi precedenti e vengono pubblicati tutti gli articoli interessati. Se si usa un reindirizzamento, ridenominazione o eliminazione dell'articolo potrebbe annullare l'operazione e causare un collegamento interrotto. Per rinominare un file, è sufficiente rinominarlo nel file system, quindi aggiungere, eseguire il commit e il push della modifica e quindi aprire una richiesta pull.
Per eliminare un file, è necessario sapere che non può essere usato per eliminare solo un file dal file system perché si tratta di un controllo del codice sorgente e deve conoscere è destinato a questo scopo. In caso contrario, i file eliminati verranno probabilmente visualizzato nuovamente.
Esistono due modi per eseguire questa operazione:

- File system e git: Eliminare il file dal file system. Quindi dallo strumento git, eseguire uno dei seguenti  ```Git add -A``` | ```Git add --all``` | ```Git add -u```
- Solo git:   Eseguire ```git rm foo.md```

    Altre informazioni [ http://stackoverflow.com/questions/2047465/how-can-i-delete-a-file-from-git-repo ](http://stackoverflow.com/questions/2047465/how-can-i-delete-a-file-from-git-repo) e [https://git-scm.com/docs/git-rm](https://git-scm.com/docs/git-rm) 

## <a name="step-7-find-and-fix-straggler-broken-links"></a>Passaggio 7: Individuare e correggere i collegamenti interrotti sospetto

Utilizzare lo strumento di controllo di qualità del contenuto per trovare i collegamenti interrotti che i passaggi precedenti non è stata catch, quindi rimuovere o risolvere i collegamenti.

## <a name="step-8-remove-cached-pages-from-search-engines"></a>Passaggio 8: Rimuovere le pagine memorizzate nella cache dai motori di ricerca

Passare a pagine web seguenti per rimuovere le pagine web memorizzate nella cache dai motori di ricerca: [Bing](https://www.bing.com/webmaster/tools/content-removal?rflid=1)
[Google](https://www.google.com/webmasters/tools/removals?pli=1)


### <a name="contributors-guide-links"></a>Collegamenti della Guida collaboratori

- [Indice di articoli con indicazioni](./contributor-guide-index.md)

