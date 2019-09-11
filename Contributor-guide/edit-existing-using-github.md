---
title: Modificare un articolo di Windows Server esistente usando GitHub e Visual Studio Code
description: Come modificare gli articoli esistenti relativi a Windows Server, usando GitHub e Visual Studio Code, come dipendenti Microsoft.
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: d681d2fc2b69898e841932a95738b89515ffb51a
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865074"
---
# <a name="edit-an-existing-windows-server-article-using-github-and-visual-studio-code"></a>Modificare un articolo di Windows Server esistente usando GitHub e Visual Studio Code

Sono disponibili due posizioni separate in cui si mantiene contenuto tecnico di Windows Server. Uno dei percorsi è Public (windowsserverdocs) mentre l'altro è privato (windowsserverdocs-PR). Determinare il percorso a cui si contribuisce:

- **Sono un dipendente Microsoft.** In qualità di dipendente Microsoft, sono disponibili opzioni basate su ciò che si sta tentando di eseguire:

    - **Creare un nuovo articolo.** Per creare un nuovo articolo, è necessario creare e configurare l'account GitHub e gli strumenti, creare un fork e clonare il repository windowsserverdocs-PR, configurare il ramo remoto, creare l'articolo e infine creare una nuova richiesta pull per l'approvazione e la pubblicazione. Per queste istruzioni, è possibile seguire le istruzioni riportate nell'articolo [creare nuovi articoli su Windows Server usando GitHub e Visual Studio Code](create-new-using-github.md) .

    - **Apportare modifiche di grandi dimensioni a un articolo esistente.** Per apportare modifiche sostanziali a un articolo esistente, è possibile seguire le istruzioni riportate in questo articolo.

    - **Apportare modifiche minime a un articolo esistente.** Per apportare modifiche minime a un articolo esistente, è possibile seguire le istruzioni riportate nell'articolo [aggiornare gli articoli di Windows Server esistenti usando un Web browser e GitHub](github-browser-updates.md) .

- **Non sono un dipendente Microsoft.** In qualità di dipendente non Microsoft, è necessario contribuire al percorso pubblico. Per informazioni su come eseguire questa operazione, vedere l'articolo relativo alla [documentazione tecnica di Windows Server](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/CONTRIBUTING.md) .

## <a name="switch-your-repo-and-create-a-new-branch"></a>Cambiare il repository e creare un nuovo ramo

Per modificare un articolo esistente, seguire questa procedura.

### <a name="create-a-new-branch-and-locate-the-file-you-want-to-update"></a>Creare un nuovo ramo e individuare il file che si desidera aggiornare

Prima di iniziare a lavorare sul contenuto, è necessario innanzitutto passare al repository windowsserverdocs-PR e individuare l'articolo che si vuole aggiornare.

#### <a name="to-create-a-new-branch-in-git-bash"></a>Per creare un nuovo ramo in git bash

- Aprire git bash e digitare i comandi (uno alla volta):

    ```markdown
    cd windowsserverdocs-pr

    git checkout –B <name_of_your_new_branch>

    git push origin <name_of_your_new_branch>
    ```

    >[!Note]
    >Si consiglia vivamente di assegnare un nome al ramo in modo chiaro e univoco per ritrovarlo in un secondo momento.

    Al termine del comando, il nuovo ramo sarà pronto per la modifica del file.

#### <a name="to-locate-your-article-and-make-your-edits"></a>Per individuare l'articolo e apportare le modifiche

1. Aprire Visual Studio Code e passare a **file**, selezionare **Apri cartella**, quindi passare al percorso GitHub della cartella che contiene l'articolo da modificare.

2. Nel riquadro di **esplorazione** selezionare il file.

3. Aggiornare le informazioni nella pagina e quindi selezionare **file** > **Salva**.

### <a name="preview-your-text"></a>Visualizzare in anteprima il testo

Dopo aver aggiornato il testo, è necessario visualizzare in anteprima le modifiche per assicurarsi che vengano visualizzate correttamente.

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

Dopo aver completato gli aggiornamenti, è necessario ottenere l'approvazione dal writer (attendere un po' di tempo) per la pubblicazione.

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