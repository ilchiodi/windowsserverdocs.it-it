---
title: Modifica di un articolo di Windows Server esistente tramite GitHub e Visual Studio Code
description: Come modificare un articoli esistenti basate su Windows Server, tramite GitHub e Visual Studio Code, come i dipendenti Microsoft.
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: d2d95cc28089ceb74bf9690f6bd78611e7d9a27a
ms.sourcegitcommit: 29ad32b9dea298a7fe81dcc33d2a42d383018e82
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/15/2019
ms.locfileid: "65624582"
---
# <a name="edit-an-existing-windows-server-article-using-github-and-visual-studio-code"></a>Modifica di un articolo di Windows Server esistente tramite GitHub e Visual Studio Code

Esistono due posizioni distinte, in cui vengono conservati contenuti tecnici di Windows Server. Uno dei percorsi è pubblico (windowsserverdocs) mentre l'altra privata (windowsserverdocs-richiesta pull). Identità dell'utente determina la posizione a cui vuole:

- **Sono dipendenti Microsoft.** Qualità di dipendente Microsoft, sono disponibili opzioni, basate su ciò che si sta tentando di eseguire:

    - **Creare un nuovo articolo.** Per creare un nuovo articolo, è necessario creare e configurare gli strumenti e un account GitHub, fork e clonazione del repository windowsserverdocs-richiesta pull, impostare il ramo remoto, creare l'articolo e infine creare una nuova richiesta pull per l'approvazione e pubblicazione. Per queste istruzioni, è possibile seguire le istruzioni riportate nel [creare nuovi articoli di Windows Server tramite GitHub e Visual Studio Code](create-new-using-github.md) articolo.

    - **Apportare le modifiche estese a un articolo esistente.** Per apportare modifiche sostanziali a un articolo esistente, è possibile seguire le istruzioni riportate in questo articolo.

    - **Apportare piccole modifiche a un articolo esistente.** Per apportare piccole modifiche a un articolo esistente, è possibile seguire le istruzioni riportate nel [aggiornano gli articoli di Windows Server esistenti usando un web browser e GitHub](github-browser-updates.md) articolo.

- **Non sono dipendenti Microsoft.** Qualità di dipendente non Microsoft, è necessario contribuire al percorso pubblico. Per informazioni su come eseguire questa operazione, vedere la [aggiunta come contributo alla documentazione tecnica di Windows Server](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/CONTRIBUTING.md) articolo.

## <a name="switch-your-repo-and-create-a-new-branch"></a>Passare al repository e creare un nuovo ramo

Seguire questi passaggi per modificare un articolo esistente.

### <a name="create-a-new-branch-and-locate-the-file-you-want-to-update"></a>Creare un nuovo ramo e individuare il file da aggiornare

Prima di iniziare a lavorare sul tuo contenuto, è necessario innanzitutto modificare al repository windowsserverdocs-pr e quindi individuare l'articolo che si desidera aggiornare.

#### <a name="to-create-a-new-branch-in-git-bash"></a>Per creare un nuovo ramo nel Git Bash

- Aprire Git Bash e digitare i comandi (una alla volta):

    ```markdown
    cd windowsserverdocs-pr

    git checkout –B <name_of_your_new_branch>

    git push origin <name_of_your_new_branch>
    ```

    >[!Note]
    >Si consiglia di denominazione il ramo di un elemento ovvio e univoco in modo che è possibile trovarlo in un secondo momento.

    Al termine, i comandi che verrà di nuovo ramo e pronto per modificare il file.

#### <a name="to-locate-your-article-and-make-your-edits"></a>Per individuare un articolo e apportare le modifiche

1. Aprire Visual Studio Code e passare a **File**, selezionare **Apri cartella**e quindi passare al percorso di GitHub della cartella che contiene l'articolo da modificare.

2. Dal **Explorer** riquadro, selezionare il file.

3. Aggiornare le informazioni nella pagina e quindi selezionare **File** > **salvare**.

### <a name="preview-your-text"></a>Anteprima testo

Dopo aver aggiornato il testo, è necessario visualizzare in anteprima le modifiche per assicurarsi che vengano visualizzati correttamente.

#### <a name="to-preview-your-text"></a>Per visualizzare l'anteprima del testo

1. In Visual Studio Code, selezionare una delle **Preview** pulsanti nell'angolo in alto a destra.

    ![icona pulsante Anteprima](media/create-new-using-github/preview-button-full-page.png): Passa a un'intera pagina anteprima del contenuto.

    ![icona pulsante Anteprima](media/create-new-using-github/preview-button-side-by-side.png): Verrà visualizzata la pagina di anteprima accanto alla pagina di lavoro, side-by-side.

2. Assicurarsi che l'articolo viene descritto come si prevede di eseguire la ricerca.

    Dopo aver verificato che sembra corretto, è possibile eseguire il commit delle modifiche e creare una richiesta pull per la pubblicazione.

### <a name="commit-your-changes"></a>Eseguire il commit delle modifiche

Dopo essersi assicurati che il testo appare corretto, è possibile eseguire il commit delle modifiche alla versione locale del repository.

#### <a name="to-commit-your-changes"></a>Per eseguire il commit delle modifiche

- Aprire Git Bash e digitare i comandi (una alla volta, la rimozione dei tag facoltativi):

    ```markdown
    OPTIONAL: git status

    git add .

    git commit -m “public comment about what change is”

    OPTIONAL: git pull upstream master

    git push origin <name_of_your_new_branch>

    ```

    Comando facoltativo di git status Mostra quali file sono stati modificati come parte di questo commit. Facoltativo git pull pull master a monte verso il basso le modifiche del contenuto più recente dal ramo master MicrosoftDocs, sincronizzare il contenuto locale con il principale contenuto master. In questo modo descrive tutti gli eventuali conflitti potenziali anticipo in modo che è possibile correggerli prima di procedere con la fase di richiesta pull.

### <a name="submit-a-pull-request-for-review-and-publication"></a>Inviare una richiesta pull per la verifica e pubblicazione

Dopo aver completato gli aggiornamenti, è necessario ottenere l'approvazione dal writer (consentire tempo per l'oggetto) per la pubblicazione.

#### <a name="to-submit-your-pull-request"></a>Per inviare la richiesta pull

1. Passare a https://github.com/MicrosoftDocs/windowsserverdocs-pr e selezionare il **richieste Pull** scheda.

2. Nel **revisori** area del riquadro destro, selezionare l'icona a forma di ingranaggio e quindi immettere il _windowsservercontent_ alias per la revisione.

    Un membro del _windowsservercontent_ alias verrà esaminare le modifiche o aggiungere commenti sugli aspetti che devono essere modificate prima che l'unione può verificarsi.

3. Tipo di **#sign-off** nei commenti in modo che i revisori sappiano si desidera consegnare per revisione e la pubblicazione. Il **#sign-off** commento:

    - Aggiorna l'etichetta per la richiesta pull dal **do-not-merge** al **ready-to-merge**.

    - Consente l'alias e i writer che sei pronto per ricevere i contenuti esaminato.

    - Consente gli amministratori sanno che dopo l'approvazione, il contenuto è pronto go live.

    >[!Important]
    >Dopo aver aggiunto il commento #sign-off, un membro del team windowsservercontent esaminerà il testo ed eseguirne il push nel master, in modo che verrà inviata con la successiva pianificata di pubblicazione per live (10:30 e giorni della settimana 3.30 pm).
    >
    >Se non si aggiungono #sign-off come commento finale alla richiesta pull, il contenuto rimarrà nella coda senza esserne eseguito il push al Master e infine TTL.

## <a name="related-information"></a>Informazioni correlate

Per altre informazioni su GitHub e il linguaggio markdown, vedere:

### <a name="git-concepts"></a>Concetti GIT

- [Introduzione a Handbook GitHub Guide-Git](https://guides.github.com/introduction/git-handbook/)

- [Progetti di duplicazione Guide per GitHub](https://guides.github.com/activities/forking/)

- [Il flusso GitHub comprensione del Guide per GitHub](https://guides.github.com/introduction/flow/)

- [Informazioni su Git Branching](https://learngitbranching.js.org/ (ideale per gli strumenti di apprendimento visual!))

### <a name="markdown"></a>Markdown

- [Linee guida interne markdown](https://review.docs.microsoft.com/help/contribute/markdown-reference?branch=master)

- [Esercitazione di GitHub esterni,](https://www.markdowntutorial.com/)