---
title: Creare un'attività di gestione dei file personalizzata
description: Questo articolo descrive come creare un'attività di gestione di file personalizzata e attività personalizzate.
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 894c4e3c0b9fa0fde7b749e6effce531c3f999bb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403179"
---
# <a name="create-a-custom-file-management-task"></a>Creare un'attività di gestione dei file personalizzata

> Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

La scadenza non è sempre un'azione ideale da eseguire sui file. Le attività di gestione dei file consentono di eseguire anche comandi personalizzati.

> [!Note]
> Questa procedura presuppone una conoscenza delle attività di gestione dei file e pertanto descrive solo la scheda **Azione** in cui vengono configurate le impostazioni personalizzate.

## <a name="to-create-a-custom-task"></a>Per creare un'attività personalizzata

1.  Fare clic sul nodo **Attività di gestione file**.

2.  Fare clic con il pulsante destro del mouse su **Attività di gestione file**, quindi fare clic su **Crea attività di gestione file** (oppure fare clic su **Crea attività di gestione file** nel riquadro **Azioni**). Verrà visualizzata la finestra di dialogo **Crea attività di gestione file**.

3.  Nella scheda **Azione** immettere le informazioni seguenti:

    -   **Tipo**. Selezionare **Personalizza** nel menu a discesa.
    -   **Eseguibile**. Digitare o selezionare un comando da eseguire quando l'attività di gestione file elabora i file. Questo eseguibile deve essere impostato come scrivibile solo dagli amministratori e dal sistema. Se altri utenti hanno accesso in scrittura al file eseguibile, non verrà eseguito correttamente.
    -   **Impostazioni per i comandi**. Per configurare gli argomenti passati al file eseguibile quando un processo di gestione file elabora i file, modificare la casella di testo **Argomenti**. Per inserire altre variabili nel testo, posizionare il cursore nella posizione della casella di testo in cui si desidera inserire la variabile, selezionare la variabile che si desidera inserire e quindi fare clic su **Inserisci variabile**. Il testo tra parentesi quadre inserisce informazioni sulle variabili che può ricevere l'eseguibile. Ad esempio, il percorso del file di origine \[\] variabile inserisce il nome del file che deve essere elaborato dall'eseguibile. Facoltativamente, fare clic sul pulsante **Directory di lavoro** per specificare il percorso dell'eseguibile personalizzato.
    -   **Sicurezza comando**. Configurare le impostazioni di sicurezza per il file eseguibile. Per impostazione predefinita, il comando viene eseguito come servizio locale, ovvero l'account più restrittivo disponibile.

4.  Fai clic su **OK**.

## <a name="see-also"></a>Vedi anche

-   [Gestione delle classificazioni](classification-management.md)
-   [Attività di gestione file](file-management-tasks.md)