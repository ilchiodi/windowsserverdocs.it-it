---
title: Aggiornare Windows Server 2016 a Windows Server 2019 | Microsoft Docs
description: Informazioni su come eseguire un aggiornamento sul posto per passare da Windows Server 2016 a Windows Server 2019.
ms.prod: windows server
ms.technology: server-general
ms.topic: upgrade
author: RobHindman
ms.author: robhind
ms.date: 09/16/2019
ms.openlocfilehash: 99133f2c582b180f240740fc2f39e99527bc0cf8
ms.sourcegitcommit: 27f0caf74e88781054250455c3c1adf06deb6234
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/19/2019
ms.locfileid: "71124821"
---
# <a name="upgrade-windows-server-2016-to-windows-server-2019"></a>Aggiornare Windows Server 2016 a Windows Server 2019

Se si desidera utilizzare lo stesso hardware e tutti i ruoli del server già configurati senza rendere flat il server, è consigliabile eseguire un aggiornamento sul posto. Un aggiornamento sul posto consente di passare da un sistema operativo precedente a uno più recente, mantenendo intatti le impostazioni, i ruoli del server e i dati. Questo articolo consente di passare da Windows Server 2016 a Windows Server 2019.

## <a name="before-you-begin-your-in-place-upgrade"></a>Prima di iniziare l'aggiornamento sul posto

Prima di iniziare l'aggiornamento di Windows Server, è consigliabile raccogliere alcune informazioni dai dispositivi, a scopo di diagnostica e risoluzione dei problemi. Poiché queste informazioni sono destinate all'uso solo se l'aggiornamento non riesce, è necessario assicurarsi di archiviare le informazioni in un punto in cui è possibile accedervi dal dispositivo.

### <a name="to-collect-your-info"></a>Per raccogliere le informazioni

1. Aprire un prompt dei comandi, passare `c:\Windows\system32`a, quindi digitare **systeminfo. exe**.

2. Copiare, incollare e archiviare le informazioni di sistema risultanti in un punto qualsiasi del dispositivo.

3. Digitare **ipconfig** al prompt dei comandi, quindi copiare e incollare le informazioni di configurazione risultanti nello stesso percorso precedente.

4. Aprire l'editor del registro di sistema, passare all'hive HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion, quindi copiare e incollare Windows Server **BuildLabEx** (Version) e **EditionID** (Edition) nello stesso percorso precedente.

Dopo aver raccolto tutte le informazioni relative a Windows Server, si consiglia vivamente di eseguire il backup del sistema operativo, delle applicazioni e delle macchine virtuali. È inoltre necessario **arrestare**, **eseguire la migrazione rapida**o **eseguire la migrazione in tempo reale** di tutte le macchine virtuali attualmente in esecuzione nel server. Non è possibile avere macchine virtuali in esecuzione durante l'aggiornamento sul posto.

## <a name="to-perform-the-upgrade"></a>Per eseguire l'aggiornamento

1. Verificare che il valore di **BuildLabEx** indichi che si sta eseguendo Windows Server 2016.

2. Individuare il supporto di installazione di Windows Server 2019, quindi selezionare **Setup. exe**.

    ![Esplora risorse che Visualizza il file Setup. exe](media/upgrade-2016-2019/setup-2019.png)

3. Selezionare **Sì** per avviare il processo di installazione.

    ![Controllo dell'account utente che richiede l'autorizzazione per avviare l'installazione](media/upgrade-2016-2019/start-setup-uac-box.png)

4. Per i dispositivi connessi a Internet, selezionare l'opzione **Scarica aggiornamenti, driver e funzionalità facoltative (scelta consigliata)** e quindi fare clic su **Avanti**.

    ![Schermata per scegliere di accedere online per ottenere gli aggiornamenti importanti di Windows](media/upgrade-2016-2019/online-updates-win-setup.png)

5. Il programma di installazione controlla la configurazione del dispositivo, è necessario attenderne il completamento, quindi selezionare **Avanti**.

6. A seconda del canale di distribuzione in cui sono stati ricevuti i supporti di Windows Server (Retail, Volume License, OEM, ODM e così via) e la licenza per il server, è possibile che venga richiesto di immettere un codice di licenza per continuare.

7. Selezionare l'edizione di Windows Server 2019 che si vuole installare e quindi fare clic su **Avanti**.

    ![Schermata per scegliere l'edizione di Windows Server 2016 da installare](media/upgrade-2016-2019/select-os-edition.png)

8. Selezionare **Accept (Accetto** ) per accettare le condizioni del contratto di licenza, in base al canale di distribuzione (ad esempio, retail, Volume License, OEM, ODM e così via).

    ![Schermata per accettare il contratto di licenza](media/upgrade-2016-2019/license-terms.png)

9. Selezionare **Mantieni file personali e app** per scegliere di eseguire un aggiornamento sul posto e quindi fare clic su **Avanti**.

    ![Schermata per scegliere il tipo di installazione](media/upgrade-2016-2019/choose-install-upgrade.png)

10. Al termine dell'analisi del dispositivo, verrà richiesto di procedere con l'aggiornamento selezionando **Installa**.

    ![Schermata che indica che si è pronti per avviare l'aggiornamento](media/upgrade-2016-2019/ready-to-install.png)

    Viene avviato l'aggiornamento sul posto, che mostra la schermata di **aggiornamento di Windows** con lo stato di avanzamento. Al termine dell'aggiornamento, il server viene riavviato.

    ![Schermata che mostra lo stato di avanzamento dell'aggiornamento](media/upgrade-2016-2019/upgrading-windows-with-progress.png)

## <a name="after-your-upgrade-is-done"></a>Al termine dell'aggiornamento

Al termine dell'aggiornamento, è necessario assicurarsi che l'aggiornamento a Windows Server 2019 abbia avuto esito positivo.

### <a name="to-make-sure-your-upgrade-was-successful"></a>Per assicurarsi che l'aggiornamento sia stato completato correttamente

1. Aprire l'editor del registro di sistema, passare all'hive HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion e visualizzare il **ProductName**. Dovrebbe essere visualizzata l'edizione di Windows Server 2019, ad esempio **Windows server 2019 datacenter**.

2. Verificare che tutte le applicazioni siano in esecuzione e che le connessioni client alle applicazioni abbiano esito positivo.

Se si ritiene che si sia verificato un errore durante l'aggiornamento, copiare e zippare la `C:\Windows\Panther`directory (in genere) e contattare il `%SystemRoot%\Panther` supporto tecnico Microsoft.

## <a name="related-articles"></a>Articoli correlati

- Per altri dettagli e informazioni su Windows Server 2019, vedere [Introduzione a Windows server 2019](https://docs.microsoft.com/windows-server/get-started-19/get-started-19).