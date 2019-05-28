# <a name="contributing-to-windows-server-technical-documentation"></a>Aggiunta come contributo alla documentazione tecnica di Windows Server

Grazie per l'interesse dimostrato nella documentazione tecnica di Windows Server. Siamo lieti di ricevere feedback, modifiche e aggiunte alla nostra documentazione. Esistono due posizioni distinte, in cui vengono conservati contenuti tecnici di Windows Server. Uno dei percorsi è pubblico (windowsserverdocs) mentre l'altra privata (windowsserverdocs-richiesta pull). Identità dell'utente determina la posizione a cui vuole:

- **Non sono dipendenti Microsoft.** Qualità di dipendente non Microsoft, è necessario contribuire al percorso pubblico. Per informazioni su come eseguire questa operazione, continuare a leggere questo articolo.

- **Sono dipendenti Microsoft.** Qualità di dipendente Microsoft, sono disponibili opzioni, basate su ciò che si sta tentando di eseguire:

    - **Creare un nuovo articolo.** Per creare un nuovo articolo, è necessario creare e configurare gli strumenti e un account GitHub, fork e clonazione del repository windowsserverdocs-richiesta pull, impostare il ramo remoto, creare l'articolo e infine creare una nuova richiesta pull per l'approvazione e pubblicazione. Per queste istruzioni, vedere la [creare nuovi articoli di Windows Server tramite GitHub e Visual Studio Code](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/Contributor-guide/create-new-using-github.md) articolo.

    - **Apportare le modifiche estese a un articolo esistente.** Per apportare modifiche sostanziali a un articolo esistente, è possibile seguire le istruzioni riportate nel [modifica di un articolo di Windows Server esistente tramite GitHub e Visual Studio Code](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/Contributor-guide/edit-existing-using-github.md) articolo.

    - **Apportare piccole modifiche a un articolo esistente.** Per apportare piccole modifiche a un articolo esistente, è possibile seguire le istruzioni riportate nel [aggiornano gli articoli di Windows Server esistenti usando un web browser e GitHub](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/Contributor-guide/github-browser-updates.md) articolo.

## <a name="sign-a-cla"></a>Firma un contratto di licenza

Tutti i collaboratori ***non*** i dipendenti Microsoft devono [firmare un Microsoft Licensing Agreement contributi](https://cla.microsoft.com/) prima di modificare qualsiasi repository Microsoft. Se si è già stato modificato nei repository Microsoft in passato, Complimenti!
È stato già completato questo passaggio.

## <a name="editing-topics"></a>Modifica di argomenti

Abbiamo provato a facilitare la modifica di un file esistente, pubblico più semplice possibile.

### <a name="to-edit-a-topic"></a>Per modificare un argomento

1. Passare alla pagina https://docs.microsoft.com/windows-server che si desidera aggiornare e quindi selezionare **modifica**.

    ![Web di GitHub, che mostra il collegamento di modifica](media/contribute-link.png)

2. Accedi a (o l'iscrizione) un account GitHub.

    È necessario un account GitHub per visualizzare la pagina che consente di modificare un argomento.

3. Selezionare il **a forma di matita** icona (nella casella rossa) per modificare il contenuto.

    ![Web di GitHub, che mostra l'icona a forma di matita nella casella rossa](media/pencil-icon.png)

4. Usando il linguaggio Markdown, apportare le modifiche all'argomento. Per informazioni sulla modifica del contenuto con Markdown, vedere:

    - **Se è collegato all'organizzazione di Microsoft in GitHub:** [Guida del collaboratore di Windows Server](https://github.com/MicrosoftDocs/windowsserverdocs-pr/tree/master/Contributor-guide)

    - **Se si è esterna a Microsoft:** [Mastering Markdown](https://guides.github.com/features/mastering-markdown/)

5. Apportare le modifiche suggerite e quindi selezionare **Anteprima modifiche** per assicurarsi che vengono rilevati problemi.

    ![Web di GitHub, che mostra la scheda Anteprima modifiche](media/preview-changes.png)

6. Dopo aver completato l'argomento di modifica, scorrere fino alla fine della pagina, digitare un nome descrittivo per il fork e quindi fare clic su **Proponi modifica file** per creare il fork nell'account GitHub personale.

    ![Web di GitHub, che mostra il pulsante di Propose file change](media/propose-file-change.png)

    Il **confronto tra modifiche** verrà visualizzata la schermata per vedere quali sono le modifiche tra il fork e il contenuto originale.

7. Nel **confronto tra modifiche** dello schermo, si noterà se si verificano problemi con il file di cui si sta archiviando.

    Se non esistono problemi, noterete il messaggio **in grado di eseguire il merge**.

    ![Web di GitHub, che illustra il confronto viene modificato sullo schermo](media/compare-changes.png)

8. Selezionare **Crea richiesta pull**.

    Richieste pull consentono ad altri utenti informare delle modifiche è stato eseguito il push in un ramo in un repository in GitHub. Dopo l'apertura di una richiesta pull, è possibile discutere e rivedere le modifiche potenziali con i collaboratori e aggiungere follow-up commit prima che le modifiche vengono unite nel ramo di base. Per altre informazioni, vedere [sulle richieste pull](https://help.github.com/articles/about-pull-requests)

9. Immettere un titolo e una descrizione per fornire il contesto appropriato sulle novità nella richiesta al responsabile dell'approvazione.

10. Scorrere fino alla parte inferiore della pagina, assicurarsi che solo i file modificati sono in questa richiesta pull. In caso contrario, si potrebbe sovrascrivere le modifiche da altri utenti.

11. Selezionare **Crea richiesta pull** nuovamente nell'inviare effettivamente alla richiesta pull.

    La richiesta pull viene inviata al writer dell'argomento e vengono verificate le modifiche. Se viene accettata la richiesta, vengono pubblicati gli aggiornamenti.

## <a name="resources"></a>Risorse

- È possibile usare un editor di testo e per modificare Markdown. È consigliabile [Visual Studio Code](https://code.visualstudio.com/), un editor leggero open source gratuito da Microsoft.