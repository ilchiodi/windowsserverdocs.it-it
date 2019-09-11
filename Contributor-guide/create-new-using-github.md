---
title: Creare nuovi articoli su Windows Server usando GitHub e Visual Studio Code
description: Come creare nuovi articoli correlati a Windows Server usando GitHub e Visual Studio Code come dipendenti Microsoft.
author: eross-msft
ms.author: lizross
ms.date: 05/02/2019
ms.openlocfilehash: 3f09c36c1e3960728ff016f5801deb854e3d3c96
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865073"
---
# <a name="create-new-windows-server-articles-using-github-and-visual-studio-code"></a>Creare nuovi articoli su Windows Server usando GitHub e Visual Studio Code

Sono disponibili due posizioni separate in cui si mantiene contenuto tecnico di Windows Server. Uno dei percorsi è Public (windowsserverdocs) mentre l'altro è privato (windowsserverdocs-PR). Determinare il percorso a cui si contribuisce:

- **Sono un dipendente Microsoft.** In qualità di dipendente Microsoft, sono disponibili opzioni basate su ciò che si sta tentando di eseguire:

    - **Creare un nuovo articolo.** Per creare un nuovo articolo, è necessario creare e configurare l'account GitHub e gli strumenti, creare un fork e clonare il repository windowsserverdocs-PR, configurare il ramo remoto, creare l'articolo e infine creare una nuova richiesta pull per l'approvazione e la pubblicazione. Per queste istruzioni, continuare a leggere questo articolo.

    - **Apportare modifiche di grandi dimensioni a un articolo esistente.** Per apportare modifiche sostanziali a un articolo esistente, è possibile seguire le istruzioni nell'articolo [modificare un articolo di Windows Server esistente usando GitHub e Visual Studio Code](edit-existing-using-github.md) .

    - **Apportare modifiche minime a un articolo esistente.** Per apportare modifiche minime a un articolo esistente, è possibile seguire le istruzioni riportate nell'articolo [aggiornare gli articoli di Windows Server esistenti usando un Web browser e GitHub](github-browser-updates.md) .

- **Non sono un dipendente Microsoft.** In qualità di dipendente non Microsoft, è necessario contribuire al percorso pubblico. Per informazioni su come eseguire questa operazione, vedere l'articolo relativo alla [documentazione tecnica di Windows Server](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/CONTRIBUTING.md) .

## <a name="prerequisites"></a>Prerequisiti

Prima di poter iniziare a usare il repository, è necessario creare e configurare l'account GitHub, configurare la verifica a due fattori e installare e configurare tutti gli strumenti necessari. Se questa operazione è già stata eseguita, è possibile passare alla [sezione fork the repository](#fork-the-repository) di questo articolo.

1. [Creare un account e un profilo GitHub](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#create-a-github-account-and-set-up-your-profile)

2. [Collegare il proprio account all'account Microsoft e alle organizzazioni Microsoft e MicrosoftDocs](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#link-your-github-and-microsoft-accounts)

3. [Attivare la verifica a due fattori](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#enable-two-factor-authentication-and-create-an-access-token)

4. [Autorizzare il sistema di compilazione ad accedere all'account GitHub](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#authorize-the-ops-build-system-to-access-your-github-account)

5. [Installa Visual Studio Code](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-visual-studio-code)

6. [Installare GitHub e i relativi strumenti](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-git-client-tools)

7. [Installare docs Authoring Pack](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-the-docs-authoring-pack)

## <a name="set-up-your-own-version-of-the-repo"></a>Configurare la propria versione del repository

Dopo aver creato e configurato gli strumenti e l'account GitHub, è possibile creare una versione personale del repository. Questa è la posizione in cui si creeranno i rami e si apporteranno tutte le modifiche.

### <a name="fork-the-repository"></a>Dividere il repository

È necessaria una copia locale dei file di origine, in modo da poter creare richieste pull dal fork al repository di produzione.

#### <a name="to-fork-the-repository"></a>Per creare un fork del repository

1. Accedere al proprio account GitHub e andare a https://github.com/microsoftdocs/windowsserverdocs-pr.

2. Selezionare **fork**.

    ![Pulsante fork evidenziato nella pagina](media/create-new-using-github/fork-button.png)

3. Selezionare il proprio account GitHub come percorso della divisione.

    ![Pulsante fork evidenziato nella pagina](media/create-new-using-github/fork-location.png)

### <a name="clone-the-repository"></a>Clonare il repository

È necessario clonare il repository per ottenere una copia locale del repository nel dispositivo locale.

#### <a name="to-clone-the-repository"></a>Per clonare il repository

1. Passare a https://github.com/settings/developers, quindi selezionare **token di accesso personali** dal riquadro sinistro.

2. Selezionare **genera nuovo token**, assegnare un nome significativo e univoco al token, selezionare tutti gli ambiti disponibili e quindi selezionare **genera token**.

3. Copiare il token e inserirlo in un luogo sicuro. Questa operazione sarà necessaria per il resto del processo e dopo aver lasciato la pagina, non sarà possibile tornare a questa pagina.

4. Aprire un comando git bash e passare alla directory in cui si vuole archiviare il repository. Si consiglia di usare `C:\users\<your_name>\GitHub`,.

5. Digitare i comandi seguenti usando le informazioni specifiche, una alla volta, per clonare il repository e configurare i rami remoti:

    ```markdown

    git clone https://<your_github_username>:<your_personal_access_token>@github.com/<your_github_username>/windowsserverdocs-pr.git

    cd windowsserverdocs-pr

    git remote add upstream https://<your_github_username>:<your_personal_access_token>@github.com/MicrosoftDocs/windowsserverdocs-pr.git

    git fetch upstream master
    ```

6. Eseguire questo comando per verificare che il computer remoto sia configurato correttamente:

    `git remote -v`

7. Verrà visualizzato un output simile al seguente:

    ```markdown
    $ git remote -v

    origin https://github.com/<your_github_username>/windowsserverdocs-pr.git (fetch)
    origin https://github.com/<your_github_username>/windowsserverdocs-pr.git (push)
    upstream https://github.com/MicrosoftDocs/windowsserverdocs-pr.git (fetch)
    upstream https://github.com/MicrosoftDocs/windowsserverdocs-pr.git (push)
    ```

    Se l'output remoto non è simile al seguente, è possibile riprovare eseguendo `git remote remove upstream`prima.

## <a name="create-a-branch-and-a-new-article"></a>Creare un ramo e un nuovo articolo

Per creare un articolo, seguire questa procedura.

### <a name="create-a-new-branch-and-a-new-file"></a>Creare un nuovo ramo e un nuovo file

Prima di iniziare a lavorare sul contenuto, è necessario creare un nuovo ramo nel repository locale.

#### <a name="to-create-a-new-branch-in-git-bash"></a>Per creare un nuovo ramo in git bash

- Aprire git bash e digitare i comandi (uno alla volta):

    ```markdown
    cd windowsserverdocs-pr

    git checkout –B <name_of_your_new_branch>

    git push origin <name_of_your_new_branch>
    ```

    >[!Note]
    >Si consiglia vivamente di assegnare un nome al ramo in modo chiaro e univoco per ritrovarlo in un secondo momento.

    Al termine del comando, il nuovo ramo sarà pronto per la creazione del nuovo file. È sufficiente passare al repository windowsserverdocs-PR una volta per ogni istanza di git bash. Se si chiude git bash, sarà necessario modificare di nuovo le directory dopo averlo aperto.

#### <a name="to-create-a-new-file-in-your-branch"></a>Per creare un nuovo file nel Branch

1. Aprire Esplora risorse, passare alla directory di GitHub e creare un nuovo file di testo nella struttura di cartelle. Il nome del file deve essere in caratteri minuscoli e separati da trattini. Ad esempio, _What-is-Windows-Server.MD_.

     È inoltre necessario modificare l'estensione del file da txt a. MD. Nel messaggio di errore visualizzato selezionare **Sì** per salvare il file con la nuova estensione di file.

2. Aprire Visual Studio Code e passare a **file**, selezionare **Apri cartella**, quindi passare al percorso GitHub del file creato nel passaggio 1.

3. Nel riquadro di **esplorazione** selezionare il nuovo file.

4. Aggiungere il testo alla pagina. Se si sta creando un nuovo articolo, assicurarsi [di aggiungere i tag di metadati necessari all'articolo relativo a Windows Server](metadata-requirements-for-articles.md).

5. Selezionare **file** > **Salva**.

### <a name="preview-your-text"></a>Visualizzare in anteprima il testo

Dopo aver aggiunto il testo al nuovo file, è necessario visualizzare in anteprima le modifiche per assicurarsi che vengano visualizzate correttamente.

#### <a name="to-preview-your-text"></a>Per visualizzare in anteprima il testo

1. In Visual Studio Code selezionare uno dei pulsanti **Anteprima** nell'angolo superiore destro.

    ![icona pulsante Anteprima](media/create-new-using-github/preview-button-full-page.png): Passa a un'anteprima a pagina intera del contenuto.

    ![icona pulsante Anteprima](media/create-new-using-github/preview-button-side-by-side.png): Apre la pagina di anteprima accanto alla pagina di lavoro affiancata.

2. Assicurarsi che l'articolo abbia un aspetto simile a quello previsto.

    Dopo aver verificato che l'aspetto è corretto, è possibile eseguire il commit delle modifiche e creare una richiesta pull per la pubblicazione.

### <a name="commit-your-changes"></a>Eseguire il commit delle modifiche

Dopo aver verificato che il testo sia corretto, è possibile eseguire il commit delle modifiche apportate alla versione locale del repository.

#### <a name="to-commit-your-changes"></a>Per eseguire il commit delle modifiche

- Aprire git bash e digitare i comandi (uno alla volta, rimuovendo i tag FACOLTATIVi):

    ```markdown
    OPTIONAL: git status

    git add .

    git commit -m “public comment about what change is”

    OPTIONAL: git pull upstream master

    git push origin <name_of_your_new_branch>

    ```

    Il comando facoltativo stato git Mostra i file che sono stati modificati come parte del commit. Il Master facoltativo pull a Monte git estrae le ultime modifiche apportate al contenuto dal ramo master di MicrosoftDocs, sincronizzando il contenuto locale con il contenuto master primario. Ciò consente di mostrare in anticipo eventuali conflitti di merge, in modo da poterli correggere prima di passare alla fase di pull.

### <a name="submit-a-pull-request-for-review-and-publication"></a>Inviare una richiesta pull per la revisione e la pubblicazione

Dopo aver completato l'articolo, è necessario ottenere l'approvazione dal writer (attendere un po' di tempo) per la pubblicazione.

#### <a name="to-submit-your-pull-request"></a>Per inviare la richiesta pull

1. Passare a https://github.com/MicrosoftDocs/windowsserverdocs-pr e selezionare la scheda **richieste pull** .

2. Nell'area **revisori** del riquadro destro selezionare l'icona a forma di ingranaggio, quindi immettere l'alias _windowsservercontent_ per la revisione.

    Un membro dell'alias _windowsservercontent_ esamina le modifiche o aggiunge commenti sugli elementi che devono essere modificati prima che l'Unione possa verificarsi.

3. Digitare **#sign-off** nei commenti in modo che i revisori sappiano che si sta passando per la revisione e la pubblicazione. Commento **#sign** :

    - Aggiorna l'etichetta per la richiesta pull da **do-not-merge** a **Ready-to-merge**.

    - Consente all'alias e ai writer di tenere presente che si è pronti per rivedere il contenuto.

    - Consente agli amministratori di tenere presente che, dopo l'approvazione, il contenuto è pronto per l'uso.

    >[!Important]
    >Dopo aver aggiunto il commento #sign, un membro del team di windowsservercontent esaminerà il testo e lo invierà al database master, in modo che venga escluso con le successive pubblicazioni pianificate in tempo reale (10.00 e 3: pm giorni feriali).
    >
    >Se non si aggiunge #sign come commento finale alla richiesta pull, il contenuto rimarrà nella coda senza che venga effettuato il push al master e, infine, a vivere.

## <a name="related-information"></a>Informazioni correlate

Per ulteriori informazioni su GitHub e sul linguaggio Markdown, vedere:

### <a name="git-concepts"></a>Concetti di git

- [Guide su GitHub-Introduzione a git Manual](https://guides.github.com/introduction/git-handbook/)

- [Guide su GitHub-progetti di fork](https://guides.github.com/activities/forking/)

- [Guide di GitHub-informazioni sul flusso di GitHub](https://guides.github.com/introduction/flow/)

- [Informazioni sulla diramazione git] (https://learngitbranching.js.org/ (Ideale per gli Learner visivi.))

### <a name="markdown"></a>Markdown

- [Linee guida Markdown interne](https://review.docs.microsoft.com/help/contribute/markdown-reference?branch=master)

- [Esterno, esercitazione su GitHub](https://www.markdowntutorial.com/)
