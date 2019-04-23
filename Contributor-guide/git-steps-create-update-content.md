<properties pageTitle="Comandi GIT per la creazione di un nuovo articolo o l'aggiornamento di un articolo esistente" description="Passaggi per creare e aggiornare un articolo in WindowsServerDocs-pr." metaKeywords="" services="" solutions="" documentationCenter="" authors="Kathy Davies" videoId="" scriptId="" manager="dongill" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="08/24/16" ms.author="kathydav" />

# <a name="git-commands-to-create-or-update-an-article"></a>Comandi GIT per creare o aggiornare un articolo

>! NOTA: Questi comandi partono dal presupposto di aver configurato Github per specificare il repository predefinito in cui si esegue il pull dal file. In Github è terminologia, in cui si esegue il pull dal file la *upstream*. In cui si esegue il push di file è il *origin*. A seconda come nostro repository e del flusso di lavoro sono stati progettati, l'upstream deve essere impostato su repository, dell'organizzazione Microsoft - https://github.com/Microsoft/WindowsServerDocs-pr e l'origine deve essere il fork di questo repository, il proprio account Github. Ad esempio, è di Kathy https://github.com/KBDAzure/WindowsServerDocs-pr 

>Per controllare le impostazioni, digitare ```git config -l```. Esaminare gli URL per assicurarsi che fanno riferimento al percorso desiderato.

## <a name="add-or-update-an-article"></a>Aggiungere o aggiornare un articolo

Di seguito viene illustrato come creare un branch locale, salvare le modifiche e quindi eseguirne il push nel fork remoto.

1. Avviare Git Bash (o lo strumento da riga di comando da utilizzare per Git).

2. Modificare WindowsServerDocs-richiesta pull:

        cd WindowsServerDocs-pr

3. È consigliabile mantenere nell'unità locale Master ramo e allo schema remoto creare un ramo nel fork in sincronia con Master del repository ramo. Ciò consente di risparmiare una grande quantità di frustrazione e perso tempo - si è più probabile individuare tempestivamente i problemi e semplificare le cose in uno stato di attivo valido, noto. Eseguire:

        git checkout master
        git pull upstream master
        git push origin master

4. È ora possibile creare un ramo per eseguire il giornaliero o il risultato finale in base al funziona. Il modo migliore per creare un ramo di lavoro locale da un ramo di rilascio è da usare per eseguire:

        git checkout upstream/upstream-branch-name -b your-local-branch-name

   Verrà creato il ramo locale direttamente dal ramo upstream e ti aiuta a evitare l'unione dei file non corretti nel nuovo ramo locale. Ad esempio, per creare un ramo di lavoro basato sul ramo a livello generale la soglia, è possibile eseguire un comando simile al seguente:
      
        git checkout upstream/master -b working-11-18

   Se si lavora sul contenuto che deve essere eseguito il merge nel ramo che non è         

5. Aggiungere il ramo di lavoro locale nel fork:

        git push origin <working branch>

6. Creare un nuovo articolo o apportare modifiche a un articolo esistente. Windows Explorer consente di aprire i file markdown e il markdown editor per creare e modificare i file. Per informazioni di formattazione di base, vedere questo [articolo](https://help.github.com/articles/getting-started-with-writing-and-formatting-on-github/) in Github.

7. Aggiungere i metadati necessari e informazioni sulla versione, in base alla [controllo delle versioni dei metadati e prodotto](metadata-OSversioning-and-trademarks.md).

8. Controllare lo stato, quindi aggiungere e salvare le modifiche:

        git status

   Esaminare i risultati per assicurarsi che i file elencati sono quelli che è stato modificato. Se tutti i file accurati, eseguire questo comando per aggiungere tutti i file:

        git add .
        git commit –m "<comment>"

   Per aggiungere solo i file specifici (ad esempio, se ```git status``` Elenca i file che non si vuole inviare), è necessario invece eseguire:

        git add <file path>
        git commit –m "<comment>"

   >[!IMPORTANT]
   >Il comando ```git add .``` aggiunge tutte le modifiche in sospeso segnalate da ```git status```. Ciò significa che, se ```git status``` Mostra gli aggiornamenti non viene tenuta tracciati che non si desidera aggiungere, usare ```git add <file path>``` invece.  

9. Aggiornare il ramo di lavoro locali con modifiche dall'upstream:

        git pull upstream master

10. Il push delle modifiche al fork su GitHub:

        git push origin <working branch>

## <a name="submit-your-changes"></a>Inviare le modifiche

Quando si è pronti per inviare il contenuto per la gestione temporanea, convalida e/o la pubblicazione, usare la UI GitHub per creare una richiesta pull. 

Quando si apre una richiesta pull (PR), si attiva passi del test, compila il progetto e pubblica il sito di staging interno. È possibile aprire una richiesta pull che non si è pronti a sono uniti poiché si tratta di come ottenere un superamento test e verificare gli aggiornamenti nel sito di gestione temporanea. Dettagli di compilazione e i collegamenti di preproduzione vengono registrati come commenti della richiesta pull. 

È responsabilità dell'utente per le operazioni seguenti **prima di richiedere affinché le modifiche sottoposte a merge**:
  - Esaminare i dettagli per assicurarsi che non contenga errori di compilazione. 
  - Esaminare gli aggiornamenti nel sito di gestione temporanea.

Dopo che è stato fatto, indica che in uno dei modi:
- Aggiunta l'etichetta "ready-to-merge" per la richiesta pull. \(Fare clic su **etichette** o sull'icona a forma di ingranaggio a destra del flusso di commento della richiesta pull.)
- Aggiunta di ready-to-merge come commento e inviare posta elettronica all'alias WSSC Pull revisori: wssc pra

Il revisore delle richieste pull controlla le modifiche e accetta la richiesta pull a meno che non sono presenti problemi o domande. Commenti e suggerimenti o richieste per risolvere i problemi vengono aggiunti come commenti. Rivedere [criteri di qualità per pull richiesta revisione](contributor-guide-pr-criteria.md) per informazioni su quello previsto.

## <a name="publishing"></a>Pubblicazione

- Gli articoli pubblicati in circa 10:00 e alle ore 15:00 costa pacifica, dal lunedì al venerdì. Tenere presente che un revisore di richieste pull richiede tempo per leggere e accettare le modifiche prima che possa unite. Le modifiche devono essere unite per essere prelevato il successivo ciclo di pubblicazione pianificata. Se è necessario un articolo pubblicato per un ciclo di pubblicazione specifica, lasciare un revisore di richieste pull sapere in anticipo. Quando viene accettata la richiesta pull, le modifiche vengono unite nel repository e verranno incluso nella pubblicazione ora successiva esecuzione. Possono volerci fino a 30 minuti per gli articoli per la visualizzazione online dopo la pubblicazione. 
