---
title: Creare nuovi articoli di Windows Server tramite GitHub e Visual Studio Code
description: Come creare nuovi articoli correlati a Windows Server, tramite GitHub e Visual Studio Code, come i dipendenti Microsoft.
author: eross-msft
ms.author: lizross
ms.date: 05/02/2019
ms.openlocfilehash: d75bf135266a4783ab2617977b344782cea679ef
ms.sourcegitcommit: 29ad32b9dea298a7fe81dcc33d2a42d383018e82
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/15/2019
ms.locfileid: "65624559"
---
# <a name="create-new-windows-server-articles-using-github-and-visual-studio-code"></a>Creare nuovi articoli di Windows Server tramite GitHub e Visual Studio Code

Esistono due posizioni distinte, in cui vengono conservati contenuti tecnici di Windows Server. Uno dei percorsi è pubblico (windowsserverdocs) mentre l'altra privata (windowsserverdocs-richiesta pull). Identità dell'utente determina la posizione a cui vuole:

- **Sono dipendenti Microsoft.** Qualità di dipendente Microsoft, sono disponibili opzioni, basate su ciò che si sta tentando di eseguire:

    - **Creare un nuovo articolo.** Per creare un nuovo articolo, è necessario creare e configurare gli strumenti e un account GitHub, fork e clonazione del repository windowsserverdocs-richiesta pull, impostare il ramo remoto, creare l'articolo e infine creare una nuova richiesta pull per l'approvazione e pubblicazione. Per queste istruzioni, continuare a leggere questo articolo.

    - **Apportare le modifiche estese a un articolo esistente.** Per apportare modifiche sostanziali a un articolo esistente, è possibile seguire le istruzioni riportate nel [modifica di un articolo di Windows Server esistente tramite GitHub e Visual Studio Code](edit-existing-using-github.md) articolo.

    - **Apportare piccole modifiche a un articolo esistente.** Per apportare piccole modifiche a un articolo esistente, è possibile seguire le istruzioni riportate nel [aggiornano gli articoli di Windows Server esistenti usando un web browser e GitHub](github-browser-updates.md) articolo.

- **Non sono dipendenti Microsoft.** Qualità di dipendente non Microsoft, è necessario contribuire al percorso pubblico. Per informazioni su come eseguire questa operazione, vedere la [aggiunta come contributo alla documentazione tecnica di Windows Server](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/CONTRIBUTING.md) articolo.

## <a name="prerequisites"></a>Prerequisiti

Prima di iniziare a usare nel repository, è necessario creare e configurare l'account GitHub, impostare una verifica a due fattori e installare e configurare tutti gli strumenti necessari. Se è stato già fatto, è possibile ignorare verso il basso per il [dividere la sezione repository](#fork-the-repository) di questo articolo.

1. [Creare un account GitHub e un profilo](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#create-a-github-account-and-set-up-your-profile)

2. [Collegare l'account al proprio account Microsoft e per le organizzazioni Microsoft e documentazione Microsoft](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#link-your-github-and-microsoft-accounts)

3. [Attivare la verifica a due fattori](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#enable-two-factor-authentication-and-create-an-access-token)

4. [Autorizzare il sistema di compilazione per accedere all'account GitHub](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#authorize-the-ops-build-system-to-access-your-github-account)

5. [Installare Visual Studio Code](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-visual-studio-code)

6. [Installare GitHub e dei relativi strumenti](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-git-client-tools)

7. [Installare Docs Authoring Pack](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-the-docs-authoring-pack)

## <a name="set-up-your-own-version-of-the-repo"></a>Configurare la propria versione del repository

Dopo aver creato e configurare l'account GitHub e gli strumenti, è possibile creare una versione personale del repository. Si tratta in cui verrà creato i rami e apportare tutte le modifiche.

### <a name="fork-the-repository"></a>Dividere il repository

È necessaria una copia locale dei file di origine, pertanto è possibile creare richieste pull dal fork al repository di produzione.

#### <a name="to-fork-the-repository"></a>Per creare la biforcazione del repository

1. Accedere al proprio account GitHub e passare a https://github.com/microsoftdocs/windowsserverdocs-pr.

2. Selezionare **Fork**.

    ![Pulsante di divisione evidenziato nella pagina](media/create-new-using-github/fork-button.png)

3. Selezionare il proprio account GitHub come percorso di fork.

    ![Pulsante di divisione evidenziato nella pagina](media/create-new-using-github/fork-location.png)

### <a name="clone-the-repository"></a>Clonare il repository

È necessario clonare get repository una copia locale del repository al dispositivo locale.

#### <a name="to-clone-the-repository"></a>Per clonare il repository

1. Passare a https://github.com/settings/developers, quindi selezionare **token di accesso personali** nel riquadro sinistro.

2. Selezionare **Genera nuovo token**, assegnare il token di un nome univoco e significativo, selezionare tutti gli ambiti disponibili e quindi selezionare **genera token**.

3. Copiare il token e inserirlo in un luogo sicuro. Sarà necessaria per il resto del processo e dopo si esce dalla pagina, sarà possibile tornare a esso.

4. Aprire un comando di Git Bash e passare alla directory in cui si desidera archiviare il repository. È consigliabile usare, `C:\users\<your_name>\GitHub`.

5. Digitare i comandi seguenti utilizzando le informazioni specifiche, uno alla volta, clonare il repository e impostare i rami remoti:

    ```markdown

    git clone https://<your_github_username>:<your_personal_access_token>@github.com/<your_github_username>/windowsserverdocs-pr.git

    cd windowsserverdocs-pr

    git remote add upstream https://<your_github_username>:<your_personal_access_token>@github.com/MicrosoftDocs/windowsserverdocs-pr.git

    git fetch upstream master
    ```

6. Eseguire questo comando per verificare che l'elemento remoto sia configurato correttamente:

    `git remote -v`

7. Dovrebbe essere simile a questo output:

    ```markdown
    $ git remote -v

    origin https://github.com/<your_github_username>/windowsserverdocs-pr.git (fetch)
    origin https://github.com/<your_github_username>/windowsserverdocs-pr.git (push)
    upstream https://github.com/MicrosoftDocs/windowsserverdocs-pr.git (fetch)
    upstream https://github.com/MicrosoftDocs/windowsserverdocs-pr.git (push)
    ```

    Se l'output remoto non è simile a ciò, è possibile ripetere l'operazione eseguendo prima `git remote remove upstream`.

## <a name="create-a-branch-and-a-new-article"></a>Creare un ramo e un nuovo articolo

Seguire questi passaggi per creare un articolo.

### <a name="create-a-new-branch-and-a-new-file"></a>Creare un nuovo ramo e un nuovo file

Prima di iniziare a lavorare sul tuo contenuto, è necessario creare un nuovo ramo nel repository locale.

#### <a name="to-create-a-new-branch-in-git-bash"></a>Per creare un nuovo ramo nel Git Bash

- Aprire Git Bash e digitare i comandi (una alla volta):

    ```markdown
    cd windowsserverdocs-pr

    git checkout –B <name_of_your_new_branch>

    git push origin <name_of_your_new_branch>
    ```

    >[!Note]
    >Si consiglia di denominazione il ramo di un elemento ovvio e univoco in modo che è possibile trovarlo in un secondo momento.

    Al termine, i comandi che verrà di nuovo ramo e pronti per creare il nuovo file. È sufficiente modificare nel repository windowsserverdocs-richiesta pull di una volta per ogni istanza di Git Bash. Se si chiude Git Bash, è necessario passare nuovamente alla directory dopo l'apertura.

#### <a name="to-create-a-new-file-in-your-branch"></a>Per creare un nuovo file nel ramo

1. Aprire Windows Explorer, andare alla directory di GitHub e creare un nuovo file di testo nella struttura di cartelle. Tutte le lettere minuscole e sono separati da trattini, deve essere il nome del file. Ad esempio, _what-viene-windows-server.md_.

     È necessario modificare anche l'estensione di file dal file con estensione txt a. md. Nel messaggio di errore che viene visualizzato, selezionare **Sì** per salvare il file con la nuova estensione di file.

2. Aprire Visual Studio Code e passare a **File**, selezionare **Apri cartella**e quindi passare al percorso di GitHub del file è stato creato nel passaggio 1.

3. Dal **Explorer** riquadro, selezionare il nuovo file.

4. Aggiungere il testo della pagina e quindi selezionare **File** > **salvare**.

### <a name="preview-your-text"></a>Anteprima testo

Dopo aver aggiunto il testo per il nuovo file, è necessario visualizzare in anteprima le modifiche per assicurarsi che vengano visualizzati correttamente.

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

Dopo aver completato l'articolo, è necessario ottenere l'approvazione dal writer (consentire tempo per l'oggetto) per la pubblicazione.

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
