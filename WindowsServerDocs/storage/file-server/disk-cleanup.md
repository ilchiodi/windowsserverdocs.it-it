---
title: Utilizzare Pulitura disco in Windows Server
description: Informazioni su come usare le opzioni della riga di comando per configurare lo strumento di pulitura disco (Cleanmgr.exe) per pulire automaticamente determinati file.
ms.prod: windows-server-threshold
ms.reviewer: cosmosdarwin
author: iangpgh
ms.author: jgerend
manager: daveba
ms.technology: storage-spaces
ms.date: 06/20/2019
ms.openlocfilehash: b479697366239144e5ca9d3486b84191eb51dc4d
ms.sourcegitcommit: 078304c4b92bb57eb85ba29634afc92cc028c644
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67301600"
---
# <a name="using-disk-cleanup-on-windows-server"></a>Utilizzare Pulitura disco in Windows Server

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Lo strumento di pulitura disco Cancella i file non necessari in un ambiente di Windows Server. Questo strumento è disponibile per impostazione predefinita in Windows Server 2019 e Windows Server 2016, ma potrebbe essere necessario eseguire alcuni passaggi manuali per abilitarlo in versioni precedenti di Windows Server.

Per avviare lo strumento di pulitura disco, eseguire il comando Cleanmgr.exe oppure selezionare **avviare**, selezionare **strumenti di amministrazione di Windows**, quindi selezionare **Pulitura disco**.

È anche possibile eseguire Pulitura disco usando il [cleanmgr comando Windows](../../administration/windows-commands/clean-mgr.md) e usare le opzioni della riga di comando per specificare che la pulitura disco pulizia determinati file.

## <a name="enable-disk-cleanup-on-an-earlier-version-of-windows-server-by-installing-the-desktop-experience"></a>Abilitare la pulitura disco in una versione precedente di Windows Server installando l'esperienza Desktop

Seguire questi passaggi per usare l'aggiunta guidata ruoli e funzionalità per installare esperienza Desktop in un server che esegue Windows Server 2012 R2 o versioni precedenti, che installa anche Pulitura disco.

1. Se Server Manager è già aperto, andare al passaggio successivo. Se Server Manager non è aperto, aprirlo in uno dei modi seguenti.

   - Nel desktop di Windows avviare Server Manager facendo clic su **Server Manager** nella barra delle applicazioni di Windows.

   - Passare a **avviare** e selezionare il riquadro di Server Manager.

1. Nel **Manage** menu, selezionare Aggiungi **ruoli e funzionalità**.

1. Nel **prima di iniziare** pagina, verificare che il server di destinazione e l'ambiente di rete siano preparati per la funzionalità che si desidera installare. Selezionare **Avanti**.

1. Nel **Seleziona tipo di installazione** pagina, selezionare **installazione basata su ruoli o basata su funzionalità** per installare tutte le funzionalità di parti in un singolo server. Selezionare **Avanti**.

1. Nella pagina **Selezione server di destinazione** scegliere un server dal pool di server oppure selezionare un disco rigido virtuale offline. Selezionare **Avanti**.

1. Nel **Selezione ruoli server** pagina, selezionare **successivo**.

1. Nel **Selezione funzionalità** pagina, selezionare **interfaccia utente e l'infrastruttura**e quindi selezionare **esperienza Desktop**.

1. Nelle **aggiungere le funzionalità necessarie per un'esperienza Desktop?** , selezionare **Aggiungi funzionalità**.

1. Procedere con l'installazione e quindi riavviare il sistema.

1. Verificare che il **Pulitura disco** viene visualizzato il pulsante di opzione nella finestra di dialogo delle proprietà.

   ![Finestra di dialogo di proprietà con l'opzione di pulitura disco del disco](media/diskpropswcleanup.png)

## <a name="manually-add-disk-cleanup-to-an-earlier-version-of-windows-server"></a>Aggiungere manualmente Pulitura disco a una versione precedente di Windows Server

Lo strumento di pulitura disco (cleanmgr.exe) non è presente in Windows Server 2012 R2 o versioni precedenti a meno che non si dispone di funzionalità esperienza Desktop installata.

Per usare cleanmgr.exe, installare esperienza Desktop, come descritto in precedenza oppure copiare due file che sono già presenti nel server, cleanmgr.exe e cleanmgr.exe.mui. Usare la tabella seguente per individuare i file per il sistema operativo.

| Sistema operativo  | Architecture  | Percorso file  |
| ----------------- | -------------- | --------------- |
| Windows Server 2008 R2 | 64 bit | C:\Windows\winsxs\amd64_microsoft-windows-cleanmgr_31bf3856ad364e35_6.1.7600.16385_none_c9392808773cd7da\cleanmgr.exe 
| Windows Server 2008 R2 | 64 bit | C:\Windows\winsxs\amd64_microsoft-windows-cleanmgr.resources_31bf3856ad364e35_6.1.7600.16385_en-us_b9cb6194b257cc63\cleanmgr.exe.mui |

Individuare cleanmgr.exe e spostare il file **%systemroot%\System32**.

Individuare cleanmgr.exe.mui e spostare i file **%systemroot%\System32\en-US**.

È ora possibile avviare lo strumento di pulizia disco eseguendo Cleanmgr.exe dal prompt dei comandi o facendo clic **avviare** e digitando **Cleanmgr** nella barra di ricerca.

Affinché il pulsante di pulitura disco vengono visualizzate nella finestra di dialogo proprietà del disco, è necessario anche installare la funzionalità esperienza Desktop.

## <a name="additional-references"></a>Altri riferimenti

[Liberare spazio nell'unità in Windows 10](https://support.microsoft.com/en-us/help/12425/windows-10-free-up-drive-space)

[cleanmgr](../../administration/windows-commands/clean-mgr.md)
