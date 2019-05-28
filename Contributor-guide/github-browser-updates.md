---
title: Modifica articoli esistenti di Windows Server tramite un web browser e GitHub
description: Come apportare modifiche rapide alla documentazione di Windows Server esistente usando un web browser e GitHub, come i dipendenti Microsoft.
author: eross-msft
ms.author: lizross
ms.date: 05/02/2019
ms.openlocfilehash: d4574de7774a43092815a3d154020559c50fdcf9
ms.sourcegitcommit: 29ad32b9dea298a7fe81dcc33d2a42d383018e82
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/15/2019
ms.locfileid: "65624595"
---
# <a name="update-existing-windows-server-articles-using-a-web-browser-and-github"></a>Aggiornare gli articoli di Windows Server esistenti tramite un web browser e GitHub

Esistono due posizioni distinte, in cui vengono conservati contenuti tecnici di Windows Server. Uno dei percorsi è pubblico (windowsserverdocs) mentre l'altra privata (windowsserverdocs-richiesta pull). Identità dell'utente determina la posizione a cui vuole:

- **Non sono dipendenti Microsoft.** Qualità di dipendente non Microsoft, è necessario contribuire al percorso pubblico. Per informazioni su come eseguire questa operazione, vedere la [Contributing.md](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/CONTRIBUTING.md) file.

- **Sono dipendenti Microsoft.** Qualità di dipendente Microsoft, sono disponibili opzioni, basate su ciò che si sta tentando di eseguire:

    - **Creare un nuovo articolo.** Per creare un nuovo articolo, è necessario creare e configurare gli strumenti e un account GitHub, fork e clonazione del repository windowsserverdocs-richiesta pull, impostare il ramo remoto, creare l'articolo e infine creare una nuova richiesta pull per l'approvazione e pubblicazione. Per queste istruzioni, vedere la [creare nuovi articoli di Windows Server tramite GitHub e Visual Studio Code](create-new-using-github.md) articolo.

    - **Apportare le modifiche estese a un articolo esistente.** Per apportare modifiche sostanziali a un articolo esistente, è possibile seguire le istruzioni riportate nel [modifica di un articolo di Windows Server esistente tramite GitHub e Visual Studio Code](edit-existing-using-github.md) articolo.

    - **Apportare piccole modifiche a un articolo esistente.** Per apportare piccole modifiche a un articolo esistente, è possibile seguire le istruzioni riportate in questo articolo.

    > [!IMPORTANT]
    > Tutti i repository che pubblicano contenuto in docs.microsoft.com adottano il [Microsoft codice di comportamento Open Source](https://opensource.microsoft.com/codeofconduct/) o nella [codice del comportamento di .NET Foundation](https://dotnetfoundation.org/code-of-conduct). Per altre informazioni, vedere la [codice di domande frequenti sul comportamento](https://opensource.microsoft.com/codeofconduct/faq/). Oppure contattare [ opencode@microsoft.com ](mailto:opencode@microsoft.com), o [ conduct@dotnetfoundation.org ](mailto:conduct@dotnetfoundation.org) con eventuali domande o commenti.
    >
    > Le correzioni secondarie o i chiarimenti inviati per la documentazione ed esempi di codice nei repository pubblici vengono analizzati i [condizioni per l'utilizzo di docs.microsoft.com](https://docs.microsoft.com/legal/termsofuse). Modifiche nuove o significative generano un commento nella richiesta pull, in cui viene richiesto di inviare un contratto di licenza online contributi (CLA) se non si è dipendenti Microsoft. È necessario completare il modulo online prima che si venga esaminata o accettata la richiesta pull.

## <a name="quick-edits-to-existing-articles-using-github-and-a-web-browser"></a>Modifiche rapide agli articoli esistenti usando un web browser e GitHub

Le modifiche rapide semplificano il processo di segnalazione e correzione di piccoli errori e omissioni nei documenti. Nonostante tutti gli sforzi, piccoli errori grammaticali e ortografici _scopo_ arrivino nei documenti pubblicati.

1. Vai a https://github.com/MicrosoftDocs/windowsserverdocs-pr/tree/master/WindowsServerDocs.

2. Passare all'articolo da modificare e quindi selezionare il **modificare il file** pulsante.

   ![Questo pulsante file modifica](media/github-browser-updates/edit-this-file.png)

3. Modificare l'argomento, quindi scorrere fino alla fine, vengono brevemente descritte le modifiche e quindi selezionare **eseguire il Commit delle modifiche**.

    ![Eseguire il commit delle modifiche con informazioni sul titolo](media/github-browser-updates/commit-changes.png)

## <a name="submit-the-pull-request"></a>Inviare la richiesta pull

Dopo aver creato la richiesta pull, è necessario inviarlo per l'approvazione e pubblicazione.

### <a name="to-submit-your-pull-request"></a>Per inviare la richiesta pull

1. Nel **aprire una richiesta pull** pagina, aggiornare il messaggio di commit per renderla più appropriata per una richiesta pull. Ad esempio: Correggere l'errore di digitazione nel primo paragrafo.

2. Assicurarsi che solo i commit e si prevede di includere i file sono inclusi. Controllare anche che passa la richiesta pull per il ramo corretto nel repository upstream, dei due schemi (in genere) o un ramo di rilascio (in alcuni casi).

3. Nel **revisori** area del riquadro destro, selezionare l'icona a forma di ingranaggio e quindi immettere _windowsservercontent_. Sarà un membro dell'alias in occasione per esaminare le modifiche e uno di tipo merge il pull request o aggiungere commenti sugli aspetti da modificare prima dell'unione.

4. Selezionare **Crea richiesta pull**. La nuova richiesta pull è collegata al ramo di lavoro nel fork. Fino a quando non viene unita alla richiesta pull, i nuovi commit che si esegue il push nel fork per il ramo di lavoro stesso vengono inclusi automaticamente in della richiesta pull. Una nuova etichetta viene aggiunta alla richiesta pull con la dicitura **do-not-merge**. Ciò significa semplicemente che il contenuto è ancora in corso e non dovrà essere riveduto o il push nel sito attivo.

5. Quando si è pronti per un utente dall'alias per esaminare il contenuto, è necessario aggiungere il testo **#sign-off** ai commenti. Questo commento:

    - Aggiorna l'etichetta per la richiesta pull dal **do-not-merge** al **ready-to-merge**.

    - Consente l'alias e i writer che sei pronto per ricevere i contenuti esaminato.

    - Consente gli amministratori sanno che dopo l'approvazione, il contenuto è pronto go live.