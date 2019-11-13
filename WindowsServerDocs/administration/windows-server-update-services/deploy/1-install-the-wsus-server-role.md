---
title: 'Passaggio 1: installare il ruolo Server WSUS'
description: 'Argomento di Windows Server Update Service (WSUS): viene descritto come installare il ruolo del server utilizzando Server Manager'
ms.prod: windows-server
ms.reviewer: na
ms.technology: manage-wsus
ms.topic: article
ms.assetid: fabc8619-350e-403b-96f8-116424931300
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 22554a9669c30cc827c509824f187fbaaedb1272
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361697"
---
# <a name="step-1-install-the-wsus-server-role"></a>Passaggio 1: Installare il ruolo del server WSUS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il passaggio successivo della procedura di distribuzione del server WSUS prevede l'installazione del ruolo server WSUS. Nella sezione seguente viene illustrato come installare il ruolo server WSUS tramite Server Manager.

> [!IMPORTANT]
> Questa procedura di installazione descrive solo come installare WSUS utilizzando il Database interno di Windows. Per informazioni sulle procedure per installare WSUS usando Microsoft SQL Server, vedere [questo articolo](https://social.technet.microsoft.com/wiki/contents/articles/10020.installing-wsus-server-role-on-windows-server-2012-with-microsoft-sql-database.aspx).

### <a name="to-install-the-wsus-server-role"></a>Per installare il ruolo server WSUS

1.  Accedere al server in cui si intende installare il ruolo server WSUS con un account membro del gruppo Administrators locale.

2.  In **Server Manager**fare clic su **Gestisci**e quindi su **Aggiungi ruoli e funzionalità**.

3.  Nella pagina **Prima di iniziare**, fare clic su **Avanti**.

4.  Nella pagina **Selezione tipo di installazione** verificare che sia selezionata l'opzione **installazione basata su ruoli o basata su funzionalità** e quindi fare clic su **Avanti**.

5.  Nella pagina **Selezione server di destinazione** scegliere la posizione in cui si trova il server (da un pool di server o da un disco rigido virtuale). Dopo aver selezionato il percorso, scegliere il server in cui si vuole installare il ruolo server WSUS e fare clic su **Avanti**.

6.  Nella pagina **Selezione ruoli server** selezionare **Windows Server Update Services**.  Viene visualizzato **Aggiungere le funzionalità necessarie per Windows Server Update Services**. Fare clic su **Aggiungi funzionalità necessarie**e quindi su **Avanti**.

7.  Nella pagina **Selezione funzionalità**. mantenere le selezioni predefinite e quindi fare clic su **Avanti**.

    > [!IMPORTANT]
    > WSUS richiede solo la configurazione del ruolo Server Web predefinito. Se viene richiesta un'altra configurazione del ruolo Server Web durante la configurazione di WSUS, accettare i valori predefiniti e continuare con la configurazione di WSUS.

8.  Nella pagina **Windows Server Update Services** fare clic su **Avanti**.

9. Nella pagina **Selezione servizi ruolo** lasciare le selezioni predefinite e quindi fare clic su **Avanti**.

    > [!TIP]
    > È necessario selezionare un tipo di database. Se le opzioni relative al database sono tutte deselezionate, le attività post-installazione avranno esito negativo.

10. Nella pagina **Selezione percorso del contenuto** digitare un percorso valido per l'archiviazione degli aggiornamenti. Ad esempio, è possibile creare una cartella denominata database_WSUS alla radice dell'unità K appositamente per questo scopo e digitare **k:\database_WSUS** come percorso valido.

11. Fare clic su **Avanti**. Viene visualizzata la pagina **Ruolo Server Web (IIS)** . Esaminare le informazioni e quindi fare clic su **Avanti**. In **selezionare i servizi ruolo da installare per server Web (IIS)** , mantenere le impostazioni predefinite e quindi fare clic su **Avanti**.

12. Nella pagina **Conferma selezioni per l'installazione** rivedere i messaggi, quindi fare clic su **Installa**. Viene avviata l'installazione guidata di WSUS. Questa operazione può richiedere diversi minuti.

13. Al termine dell'installazione di WSUS, nella finestra di riepilogo nella pagina **Stato installazione** fare clic su **Avvia attività successive a installazione**. Il testo cambia e richiede quanto segue: **Attendere la configurazione del server**. Al termine dell'attività, il testo diventa: **Configurazione riuscita**. Fare clic su **Chiudi**.

14. In **Server Manager**verificare se viene visualizzata una notifica che indica che è necessario riavviare il server. Il riavvio può essere necessario a seconda del ruolo server installato. Se il riavvio è necessario, assicurarsi di riavviare il server per completare l'installazione.

> [!IMPORTANT]
> A questo punto è terminato il processo di installazione, tuttavia per WSUS per il funzionamento è necessario procedere al [passaggio 2: configurare WSUS](2-configure-wsus.md).

