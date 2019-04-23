# <a name="tips-and-gotchas"></a>Elementi particolari e suggerimenti

Se si ha familiarità con Git, può essere frustrante imparare a lavorare in modo efficace. Le informazioni vengono raccolte qui in modo risulta più semplice trovare più se è incluso all'interno di altre informazioni in questa Guida.

## <a name="basics"></a>Nozioni fondamentali:

Si supponga di comandi nella Guida del collaboratore, questo è stato configurato Github per specificare il repository predefinito in cui si esegue il pull dal file. In Github è terminologia, in cui si esegue il pull dal file la *upstream*. In cui si esegue il push di file è il *origin*. 

Perché sul modo in cui sono progettati i repository e del flusso di lavoro, l'upstream deve essere impostato su repository, dell'organizzazione Microsoft - https://github.com/Microsoft/WindowsServerDocs-pr e l'origine deve essere il fork di questo repository, ovvero il proprio account Github. Ad esempio, https://github.com/MY-GITHUB-ALIAS/WindowsServerDocs-pr 

>Per controllare le impostazioni, digitare ```git config -l```. Esaminare gli URL per assicurarsi che fanno riferimento al percorso desiderato.

Alcuni suggerimenti per ramo di base:

-  Funziona nei rami, non nella copia locale del database Master. Questo rende più semplice risolvere i problemi nei rami e tornare a un punto valido noto.

-  Basare i rami locali su un ramo nel repository condiviso, non nel fork remoto. Quindi, occorre effettuare il push dei rami locali nel fork remoto. Dal fork remoto, si verrà richiesta di dispongano degli aggiornamenti nel fork unite nel repository condiviso. I comandi in questo articolo mostrano come eseguire questa operazione.

-  Quando si crea un branch locale, il comando indica a Git quali branch che si desidera basare il nuovo ramo. Si tratta quando e come si specificherà master o un ramo di un'attività cardine o una funzionalità. Ad esempio, questo comando crea un nuovo ramo da un ramo upstream e quindi estrae il nuovo ramo in modo che si è pronti per operare al suo interno:

        git checkout upstream/upstream-branch-name -b your-local-branch-name

Per informazioni su markdown, iniziare con questo [articolo](https://help.github.com/articles/getting-started-with-writing-and-formatting-on-github/) in Github. Per le tabelle, vedere questo [uno](https://help.github.com/articles/organizing-information-with-tables/).

## <a name="gotchas"></a>Elementi particolari

### <a name="deleting-files"></a>L'eliminazione di file
Non può essere usato per eliminare solo un file dal file system. Usare i comandi Git per informare il sistema è destinato a tale scopo, in caso contrario, i file eliminati diventerà probabilmente verranno visualizzati i file di zombie. E mai a un'ora valida. Dopo tutto, si tratta di un controllo del codice sorgente e se l'utente non segnala che si vuole eliminare questi file, modo suggerito aggiunge nuovamente automaticamente. Grazie, Git. Per istruzioni, vedere [Rinomina o si ritira un articolo](rename-r-retire.md).

### <a name="merge-conflicts"></a>Conflitti di unione

Se si verifica un conflitto di merge mentre si lavora in locale, è necessario correggerla oppure eseguire il backup esterno. La maggior parte dei casi è consigliabile correggere, a meno che non si trovano i file. Tenere presente se non si interviene, potrebbe essere bloccato da push delle modifiche all'origine, pertanto è necessario eseguire su un valore.

**Come correggere** -linee guida di Azure dispone di altre informazioni dettagliate e istruzioni per risolvere il conflitto. **Una richiesta pull con conflitti di merge non deve mai essere accettata**. Assicurarsi di sostituire il nome del nostro repository e branch in tutti gli esempi, quando si esaminano le [istruzioni](https://microsoft.sharepoint.com/teams/azurecontentguidance/wiki/Pages/Resolve%20a%20local%20merge%20conflict%20yourself.aspx). 

**Come eseguire il backup** , ovvero se i conflitti sono nei file non si è proprietari o non è presente un motivo non è possibile correggerli al momento, come eseguire il backup. Questa opzione Reimposta il set di file locale come si trovavano prima che si è verificato il conflitto di merge. Esegui questo comando:

```git merge --abort```

Se è stato ancora di staging e commit eseguito il lavoro locale, è possibile farlo ora. Tuttavia, se eseguire il push delle modifiche e si apre una richiesta pull, il conflitto verrà probabilmente visualizzato nuovamente se fosse in file con le modifiche. È possibile risolvere il problema o richiedere assistenza da altri utenti a risolvere il problema, prima che la richiesta può essere accettata. Per questo motivo, è consigliabile usare solo il metodo per annullare il blocco se sono presenti attività urgenti, ad esempio un aggiornamento critico.

