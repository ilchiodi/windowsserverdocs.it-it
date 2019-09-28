---
title: Uso della pulizia del disco in Windows Server
description: Informazioni su come usare le opzioni della riga di comando per configurare lo strumento di pulizia dei dischi (cleanmgr. exe) per pulire automaticamente determinati file.
ms.prod: windows-server
ms.reviewer: cosmosdarwin
author: iangpgh
ms.author: jgerend
manager: daveba
ms.technology: storage-spaces
ms.date: 06/20/2019
ms.openlocfilehash: 2de3452a3528122beb26f403fb0c73d7ff13efd7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402117"
---
# <a name="using-disk-cleanup-on-windows-server"></a>Uso della pulizia del disco in Windows Server

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Lo strumento Pulitura disco cancella i file non necessari in un ambiente Windows Server. Questo strumento è disponibile per impostazione predefinita in Windows Server 2019 e Windows Server 2016, ma potrebbe essere necessario eseguire alcuni passaggi manuali per abilitarlo nelle versioni precedenti di Windows Server.

Per avviare lo strumento Pulitura disco, eseguire il comando cleanmgr. exe oppure selezionare **Start**, strumenti di **amministrazione di Windows**e quindi **Pulitura disco**.

È anche possibile eseguire la pulitura del disco usando il [comando di Windows cleanmgr](../../administration/windows-commands/cleanmgr.md) e usare le opzioni della riga di comando per specificare che pulitura disco pulisce determinati file.

## <a name="enable-disk-cleanup-on-an-earlier-version-of-windows-server-by-installing-the-desktop-experience"></a>Abilitare la pulizia dei dischi in una versione precedente di Windows Server installando l'esperienza desktop

Seguire questa procedura per usare l'aggiunta guidata ruoli e funzionalità per installare l'esperienza desktop in un server che esegue Windows Server 2012 R2 o versioni precedenti, che installa anche la pulizia del disco.

1. Se Server Manager è già aperto, andare al passaggio successivo. Se Server Manager non è aperto, aprirlo in uno dei modi seguenti.

   - Nel desktop di Windows avviare Server Manager facendo clic su **Server Manager** nella barra delle applicazioni di Windows.

   - Passare a **Start** e selezionare il riquadro Server Manager.

1. Scegliere Aggiungi **ruoli e funzionalità**dal menu **Gestisci** .

1. Nella pagina **prima di iniziare** verificare che il server di destinazione e l'ambiente di rete siano preparati per la funzionalità che si desidera installare. Selezionare **Avanti**.

1. Nella pagina **Selezione tipo di installazione** selezionare installazione basata su **ruoli o basata su funzionalità** per installare tutte le funzionalità di part in un unico server. Selezionare **Avanti**.

1. Nella pagina **Selezione server di destinazione** scegliere un server dal pool di server oppure selezionare un disco rigido virtuale offline. Selezionare **Avanti**.

1. Nella pagina **Selezione ruoli server** selezionare **Avanti**.

1. Nella pagina **Selezione funzionalità** selezionare **interfaccia utente e infrastruttura**, quindi selezionare **esperienza desktop**.

1. In **aggiungere le funzionalità necessarie per esperienza desktop**, selezionare **Aggiungi funzionalità**.

1. Procedere con l'installazione, quindi riavviare il sistema.

1. Verificare che il pulsante di opzione **Pulitura disco** sia visualizzato nella finestra di dialogo Proprietà.

   ![Finestra di dialogo Proprietà disco con opzione pulitura disco](media/diskpropswcleanup.png)

## <a name="manually-add-disk-cleanup-to-an-earlier-version-of-windows-server"></a>Aggiungere manualmente pulitura disco a una versione precedente di Windows Server

Lo strumento pulizia dischi (cleanmgr. exe) non è presente in Windows Server 2012 R2 o versioni precedenti, a meno che non sia stata installata la funzionalità esperienza desktop.

Per utilizzare cleanmgr. exe, installare l'esperienza desktop come descritto in precedenza oppure copiare due file già presenti nel server, cleanmgr. exe e cleanmgr. exe. mui. Usare la tabella seguente per individuare i file per il sistema operativo.

| Sistema operativo  | Architettura  | Percorso file  |
| ----------------- | -------------- | --------------- |
| Windows Server 2008 R2 | 64 bit | C:\Windows\winsxs\amd64_microsoft-windows-cleanmgr_31bf3856ad364e35_6.1.7600.16385_none_c9392808773cd7da\cleanmgr.exe 
| Windows Server 2008 R2 | 64 bit | C:\Windows\winsxs\amd64_microsoft-windows-cleanmgr.resources_31bf3856ad364e35_6.1.7600.16385_en-us_b9cb6194b257cc63\cleanmgr.exe.mui |

Individuare cleanmgr. exe e spostare il file in **%SystemRoot%\System32.** .

Individuare cleanmgr. exe. mui e spostare i file in **%systemroot%\System32\en-US**.

È ora possibile avviare lo strumento Pulitura disco eseguendo cleanmgr. exe dal prompt dei comandi oppure facendo clic su **Start** e digitando **cleanmgr** nella barra di ricerca.

Per visualizzare il pulsante Pulizia disco nella finestra di dialogo delle proprietà di un disco, sarà necessario installare anche la funzionalità esperienza desktop.

## <a name="additional-references"></a>Altri riferimenti

[Liberare spazio su disco in Windows 10](https://support.microsoft.com/en-us/help/12425/windows-10-free-up-drive-space)

[cleanmgr](../../administration/windows-commands/cleanmgr.md)
