---
title: Uso di Pulizia disco in Windows Server
description: Informazioni su come usare le opzioni della riga di comando per configurare lo strumento Pulizia disco (Cleanmgr.exe) per pulire automaticamente determinati file.
ms.prod: windows-server
ms.reviewer: cosmosdarwin
author: iangpgh
ms.author: jgerend
manager: daveba
ms.technology: storage-spaces
ms.date: 06/20/2019
ms.openlocfilehash: bb93ec15fd138ee65797c9d27413552c3a1759a6
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "75949676"
---
# <a name="using-disk-cleanup-on-windows-server"></a>Uso di Pulizia disco in Windows Server

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Lo strumento Pulizia disco cancella i file non necessari in un ambiente Windows Server. Questo strumento è disponibile per impostazione predefinita in Windows Server 2019 e Windows Server 2016, ma potresti dover eseguire alcuni passaggi manuali per abilitarlo nelle versioni precedenti di Windows Server.

Per avviare lo strumento Pulizia disco, esegui il comando Cleanmgr.exe oppure seleziona **Avvio**, **Strumenti di amministrazione Windows** e quindi seleziona **Pulizia disco**.

Puoi eseguire Pulizia disco anche tramite il [comando cleanmgr di Windows](../../administration/windows-commands/cleanmgr.md) e usare le opzioni della riga di comando per specificare che Pulizia disco elimini determinati file.

## <a name="enable-disk-cleanup-on-an-earlier-version-of-windows-server-by-installing-the-desktop-experience"></a>Abilitare Pulizia disco in versioni precedenti di Windows Server installando Esperienza desktop

Segui questi passaggi per usare la procedura guidata Aggiungi ruoli e funzionalità per installare Esperienza desktop in un server che esegue Windows Server 2012 R2 o versioni precedenti e installare anche Pulizia disco.

1. Se Server Manager è già aperto, andare al passaggio successivo. Se Server Manager non è aperto, aprirlo in uno dei modi seguenti.

   - Nel desktop di Windows avviare Server Manager facendo clic su **Server Manager** nella barra delle applicazioni di Windows.

   - Passa a **Start** e seleziona il riquadro Server Manager.

1. Scegli **Aggiungi ruoli e funzionalità** dal menu **Gestisci**.

1. Nella pagina **Prima di iniziare** verifica che il server di destinazione e l'ambiente di rete siano preparati per la funzionalità che vuoi installare. Selezionare **Avanti**.

1. Nella pagina **Selezione tipo di installazione** seleziona **Installazione basata su ruoli o basata su funzionalità** per installare tutte le funzionalità in un solo server. Selezionare **Avanti**.

1. Nella pagina **Selezione server di destinazione** scegliere un server dal pool di server oppure selezionare un disco rigido virtuale offline. Selezionare **Avanti**.

1. Nella pagina **Selezione ruoli server** seleziona **Avanti**.

1. Nella pagina **Selezione funzionalità** seleziona **User Interface and Infrastructure** (Infrastruttura e interfaccia utente) e quindi **Esperienza desktop**.

1. In **Add features that are required for Desktop Experience?** (Aggiungere le funzionalità necessarie per Esperienza desktop?) seleziona **Aggiungi funzionalità**.

1. Procedi con l'installazione e quindi riavvia il sistema.

1. Verifica che nella finestra di dialogo Proprietà venga visualizzato il pulsante **Pulizia disco**.

   ![Finestra di dialogo Proprietà del disco con l'opzione Pulizia disco](media/diskpropswcleanup.png)

## <a name="manually-add-disk-cleanup-to-an-earlier-version-of-windows-server"></a>Aggiungere manualmente Pulizia disco a una versione precedente di Windows Server

Lo strumento Pulizia disco (cleanmgr.exe) non è presente in Windows Server 2012 R2 o versioni precedenti, a meno che non sia stata installata la funzionalità Esperienza desktop.

Per usare cleanmgr.exe, installa Esperienza desktop come descritto in precedenza oppure copia due file già presenti nel server, cleanmgr.exe e cleanmgr.exe.mui. Per individuare i file per il sistema operativo in uso, fai riferimento alla tabella seguente.

| Sistema operativo  | Architecture  | Percorso file  |
| ----------------- | -------------- | --------------- |
| Windows Server 2008 R2 | 64 bit | C:\Windows\winsxs\amd64_microsoft-windows-cleanmgr_31bf3856ad364e35_6.1.7600.16385_none_c9392808773cd7da\cleanmgr.exe 
| Windows Server 2008 R2 | 64 bit | C:\Windows\winsxs\amd64_microsoft-windows-cleanmgr.resources_31bf3856ad364e35_6.1.7600.16385_en-us_b9cb6194b257cc63\cleanmgr.exe.mui |

Individua cleanmgr.exe e sposta il file in **%systemroot%\System32**.

Individua cleanmgr.exe.mui e sposta il file in **%systemroot%\System32\en-US**.

Puoi ora avviare lo strumento Pulizia disco eseguendo Cleanmgr.exe dal prompt dei comandi oppure facendo clic su **Start** e digitando **Cleanmgr** nella barra di ricerca.

Per visualizzare il pulsante Pulizia disco nella finestra di dialogo Proprietà di un disco, devi installare anche la funzionalità Esperienza desktop.

## <a name="additional-references"></a>Altri riferimenti

[Liberare spazio sull'unità in Windows 10](https://support.microsoft.com/help/12425/windows-10-free-up-drive-space)

[cleanmgr](../../administration/windows-commands/cleanmgr.md)
