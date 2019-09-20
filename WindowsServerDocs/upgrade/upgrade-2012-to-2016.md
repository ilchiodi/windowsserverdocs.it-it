---
title: Aggiornare Windows Server 2012 a Windows Server 2016 | Microsoft Docs
description: Informazioni su come eseguire un aggiornamento sul posto per passare da Windows Server 2012 a Windows Server 2016.
ms.prod: windows server
ms.technology: server-general
ms.topic: upgrade
author: RobHindman
ms.author: robhind
ms.date: 09/16/2019
ms.openlocfilehash: 09c5a95e2ccd065f3ebbe551404064c39f803f2a
ms.sourcegitcommit: 27f0caf74e88781054250455c3c1adf06deb6234
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/19/2019
ms.locfileid: "71124721"
---
# <a name="upgrade-windows-server-2012-to-windows-server-2016"></a>Aggiornare Windows Server 2012 a Windows Server 2016

Se si desidera utilizzare lo stesso hardware e tutti i ruoli del server già configurati senza rendere flat il server, è consigliabile eseguire un aggiornamento sul posto. Un aggiornamento sul posto consente di passare da un sistema operativo precedente a uno più recente, mantenendo intatti le impostazioni, i ruoli del server e i dati. Questo articolo consente di passare da Windows Server 2012 a Windows Server 2016.

Per eseguire l'aggiornamento a Windows Server 2019, usare prima questo argomento per eseguire l'aggiornamento a Windows Server 2016, quindi [eseguire l'aggiornamento da Windows server 2016 a Windows server 2019](upgrade-2016-to-2019.md).

## <a name="before-you-begin-your-in-place-upgrade"></a>Prima di iniziare l'aggiornamento sul posto

Prima di iniziare l'aggiornamento di Windows Server, è consigliabile raccogliere alcune informazioni dai dispositivi, a scopo di diagnostica e risoluzione dei problemi. Poiché queste informazioni sono destinate all'uso solo se l'aggiornamento non riesce, è necessario assicurarsi di archiviare le informazioni in un punto in cui è possibile accedervi dal dispositivo.

### <a name="to-collect-your-info"></a>Per raccogliere le informazioni

1. Aprire un prompt dei comandi, passare `c:\Windows\system32`a, quindi digitare **systeminfo. exe**.

2. Copiare, incollare e archiviare le informazioni di sistema risultanti in un punto qualsiasi del dispositivo.

3. Digitare **ipconfig** al prompt dei comandi, quindi copiare e incollare le informazioni di configurazione risultanti nello stesso percorso precedente.

4. Aprire l'editor del registro di sistema, passare all'hive HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion, quindi copiare e incollare Windows Server **BuildLabEx** (Version) e **EditionID** (Edition) nello stesso percorso precedente.

Dopo aver raccolto tutte le informazioni relative a Windows Server, si consiglia vivamente di eseguire il backup del sistema operativo, delle applicazioni e delle macchine virtuali. È inoltre necessario **arrestare**, **eseguire la migrazione rapida**o **eseguire la migrazione in tempo reale** di tutte le macchine virtuali attualmente in esecuzione nel server. Non è possibile avere macchine virtuali in esecuzione durante l'aggiornamento sul posto.

## <a name="to-perform-the-upgrade"></a>Per eseguire l'aggiornamento

1. Verificare che il valore di **BuildLabEx** indichi che si sta eseguendo Windows Server 2012.

2. Individuare il supporto di installazione di Windows Server 2016, quindi selezionare **Setup. exe**.

    ![Esplora risorse che Visualizza il file Setup. exe](media/upgrade-2012-2016/setup-2016.png)

3. Selezionare **Sì** per avviare il processo di installazione.

    ![Controllo dell'account utente che richiede l'autorizzazione per avviare l'installazione](media/upgrade-2012-2016/start-setup-uac-box.png)

4. Nella schermata Windows Server 2016 selezionare **Installa ora**.

5. Per i dispositivi connessi a Internet, selezionare **Scarica e installa aggiornamenti (scelta consigliata)** .

    ![Schermata per scegliere di accedere online per ottenere gli aggiornamenti importanti di Windows](media/upgrade-2012-2016/imp-updates-win-setup.png)

6. Il programma di installazione controlla la configurazione del dispositivo, è necessario attenderne il completamento, quindi selezionare **Avanti**.

7. A seconda del canale di distribuzione in cui sono stati ricevuti i supporti di Windows Server (Retail, Volume License, OEM, ODM e così via) e la licenza per il server, è possibile che venga richiesto di immettere un codice di licenza per continuare.

    ![Schermata in cui è possibile immettere il codice Product Key](media/upgrade-2012-2016/enter-product-key.png)

8. Selezionare l'edizione di Windows Server 2016 che si vuole installare e quindi fare clic su **Avanti**.

    ![Schermata per scegliere l'edizione di Windows Server 2016 da installare](media/upgrade-2012-2016/select-os-edition.png)

9. Selezionare **Accept (Accetto** ) per accettare le condizioni del contratto di licenza, in base al canale di distribuzione (ad esempio, retail, Volume License, OEM, ODM e così via).

    ![Schermata per accettare il contratto di licenza](media/upgrade-2012-2016/license-terms.png)

10. Selezionare **Mantieni file personali e app** per scegliere di eseguire un aggiornamento sul posto e quindi fare clic su **Avanti**.

    ![Schermata per scegliere il tipo di installazione](media/upgrade-2012-2016/choose-install-upgrade.png)

11. Se viene visualizzata una pagina che indica che l'aggiornamento non è consigliato, è possibile ignorarlo e selezionare **Confirm (conferma**). È stata messa a posto per richiedere installazioni pulite, ma non è necessario.

    ![Schermata che mostra il pulsante conferma per assicurarsi di voler eseguire l'aggiornamento](media/upgrade-2012-2016/confirm-upgrade-process.png)

12. Il programma di installazione consentirà di rimuovere Microsoft Endpoint Protection utilizzando **Installazione applicazioni**.

    Questa funzionalità non è compatibile con Windows 10.

13. Al termine dell'analisi del dispositivo, verrà richiesto di procedere con l'aggiornamento selezionando **Installa**.

    ![Schermata che indica che si è pronti per avviare l'aggiornamento](media/upgrade-2012-2016/ready-to-install.png)

    Viene avviato l'aggiornamento sul posto, che mostra la schermata di **aggiornamento di Windows** con lo stato di avanzamento. Al termine dell'aggiornamento, il server viene riavviato.

    ![Schermata che mostra lo stato di avanzamento dell'aggiornamento](media/upgrade-2012-2016/upgrading-windows-with-progress.png)

## <a name="after-your-upgrade-is-done"></a>Al termine dell'aggiornamento

Al termine dell'aggiornamento, è necessario assicurarsi che l'aggiornamento a Windows Server 2016 abbia avuto esito positivo.

### <a name="to-make-sure-your-upgrade-was-successful"></a>Per assicurarsi che l'aggiornamento sia stato completato correttamente

1. Aprire l'editor del registro di sistema, passare all'hive HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion e visualizzare il **ProductName**. Dovrebbe essere visualizzata l'edizione di Windows Server 2016, ad esempio **Windows server 2016 datacenter**.

2. Verificare che tutte le applicazioni siano in esecuzione e che le connessioni client alle applicazioni abbiano esito positivo.

Se si ritiene che si sia verificato un errore durante l'aggiornamento, copiare e zippare la `C:\Windows\Panther`directory (in genere) e contattare il `%SystemRoot%\Panther` supporto tecnico Microsoft.

## <a name="next-steps"></a>Passaggi successivi

È possibile eseguire un altro aggiornamento per passare da Windows Server 2016 a Windows Server 2019. Per istruzioni dettagliate, vedere [Upgrade Windows server 2016 to Windows server 2019](upgrade-2016-to-2019.md).

## <a name="related-articles"></a>Articoli correlati

- Per altri dettagli e informazioni su Windows Server 2016, vedere [Introduzione a Windows server 2016](https://docs.microsoft.com/windows-server/get-started/server-basics).