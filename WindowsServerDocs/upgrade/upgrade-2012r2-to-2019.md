---
title: Eseguire l'aggiornamento da Windows Server 2012 R2 a Windows Server 2019 | Microsoft Docs
description: Informazioni su come eseguire un aggiornamento sul posto per passare da Windows Server 2012 R2 a Windows Server 2019.
ms.prod: windows-server
ms.technology: server-general
ms.topic: upgrade
author: RobHindman
ms.author: robhind
ms.date: 09/16/2019
ms.openlocfilehash: 02d6dd21b798346245f209174902b6f4fdf24ff8
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80853344"
---
# <a name="upgrade-windows-server-2012-r2-to-windows-server-2019"></a>Eseguire l'aggiornamento da Windows Server 2012 R2 a Windows Server 2019

Se vuoi mantenere lo stesso hardware e tutti i ruoli del server che hai già configurato senza rendere flat il server, dovrai eseguire un aggiornamento sul posto. Con un aggiornamento sul posto passerai da un sistema operativo precedente a una versione più recente, mantenendo impostazioni, ruoli del server e dati. Questo articolo descrive come passare da Windows Server 2012 R2 a Windows Server 2019.

## <a name="before-you-begin-your-in-place-upgrade"></a>Prima di iniziare l'aggiornamento sul posto

Prima di iniziare l'aggiornamento di Windows Server, è consigliabile raccogliere alcune informazioni dai dispositivi, ai fini della diagnostica e della risoluzione dei problemi. Poiché queste informazioni sono utili solo se l'aggiornamento ha esito negativo, devi assicurarti di conservarle in un punto accessibile al di fuori del dispositivo.

### <a name="to-collect-your-info"></a>Per raccogliere le informazioni

1. Apri un prompt dei comandi, passa a `c:\Windows\system32` e quindi digita **systeminfo.exe**.

2. Copia, incolla e conserva le informazioni di sistema risultanti in un punto qualsiasi al di fuori del dispositivo.

3. Al prompt dei comandi digita **ipconfig /all** e quindi copia e incolla le informazioni di configurazione risultanti nello stesso percorso indicato in precedenza.

4. Apri l'editor del Registro di sistema, passa all'hive HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion e quindi copia e incolla i valori **BuildLabEx** (versione) ed **EditionID** (edizione) di Windows Server nello stesso percorso indicato in precedenza.

Dopo aver raccolto tutte le informazioni relative a Windows Server, è consigliabile eseguire il backup del sistema operativo, delle app e delle macchine virtuali. Devi anche **arrestare il sistema** ed eseguire una **migrazione rapida** o una **migrazione in tempo reale** delle eventuali macchine virtuali attualmente in esecuzione nel server. Durante l'aggiornamento sul posto le macchine virtuali non devono essere in esecuzione.

## <a name="to-perform-the-upgrade"></a>Per eseguire l'aggiornamento

1. Verifica che il valore **BuildLabEx** confermi l'esecuzione di Windows Server 2012 R2.

2. Individua il supporto di installazione di Windows Server 2019 e quindi seleziona **setup.exe**.

    ![Esplora risorse che visualizza il file setup.exe](media/upgrade-2012r2-2019/setup-2019.png)

3. Scegli **Sì** per avviare il processo di installazione.

    ![Controllo dell'account utente che richiede l'autorizzazione per avviare l'installazione](media/upgrade-2012r2-2019/start-setup-uac-box.png)

4. Per i dispositivi connessi a Internet, seleziona l'opzione **Download updates, drivers and optional features (recommended)** (Scarica aggiornamenti, driver e funzionalità facoltative - Scelta consigliata) e quindi scegli **Avanti**.

    ![Schermata per scegliere di accedere a Internet per scaricare aggiornamenti Windows importanti](media/upgrade-2012r2-2019/online-updates-win-setup.png)

5. Il programma di installazione controlla la configurazione del dispositivo. Attendi il completamento del controllo e quindi scegli **Avanti**.

6. A seconda del canale di distribuzione da cui hai ricevuto i supporti di Windows Server (vendita al dettaglio, contratto multilicenza, OEM, ODM e così via) e della licenza per il server, è possibile che venga richiesto di immettere un codice di licenza per continuare.

7. Seleziona l'edizione di Windows Server 2019 che vuoi installare e quindi scegli **Avanti**.

    ![Schermata per scegliere l'edizione di Windows Server 2012 R2 da installare](media/upgrade-2012r2-2019/select-os-edition.png)

8. Seleziona **Accetto** per accettare le condizioni del contratto di licenza, in base al canale di distribuzione (ad esempio, vendita al dettaglio, contratto multilicenza, OEM, ODM e così via).

    ![Schermata per accettare il contratto di licenza](media/upgrade-2012r2-2019/license-terms.png)

9. Il programma di installazione indicherà di rimuovere Microsoft Endpoint Protection tramite **Installazione applicazioni**.

    Questa funzionalità non è compatibile con Windows 2019.

10. Seleziona **Mantieni i file personali e le app** per scegliere di eseguire un aggiornamento sul posto e quindi fai clic su **Avanti**.

    ![Schermata per scegliere il tipo di installazione](media/upgrade-2012r2-2019/choose-install-upgrade.png)

11. Al termine dell'analisi del dispositivo, il programma di installazione richiederà di procedere con l'aggiornamento scegliendo **Installa**.

    ![Schermata che indica che sei pronto per avviare l'aggiornamento](media/upgrade-2012r2-2019/ready-to-install.png)

    L'aggiornamento sul posto inizia e mostra la schermata **Aggiornamento di Windows in corso** con lo stato di avanzamento. Al termine dell'aggiornamento, il server viene riavviato.

    ![Schermata che mostra l'avanzamento dell'aggiornamento](media/upgrade-2012r2-2019/upgrading-windows-with-progress.png)

## <a name="after-your-upgrade-is-done"></a>Al termine dell'aggiornamento

Al termine dell'aggiornamento, devi assicurarti che l'aggiornamento a Windows Server 2019 abbia avuto esito positivo.

### <a name="to-make-sure-your-upgrade-was-successful"></a>Per assicurarti che l'aggiornamento sia stato completato

1. Apri l'editor del Registro di sistema, passa all'hive HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion e visualizza il **NomeProdotto**. Dovresti visualizzare l'edizione di Windows Server 2019, ad esempio **Windows Server 2019 Datacenter**.

2. Verifica che tutte le applicazioni siano in esecuzione e che le connessioni client alle applicazioni funzionino correttamente.

Se pensi che si sia verificato un errore durante l'aggiornamento, copia e comprimi la directory `%SystemRoot%\Panther` (in genere `C:\Windows\Panther`) e contatta il Supporto tecnico Microsoft.

## <a name="related-articles"></a>Articoli correlati

- Per altri dettagli e informazioni su Windows Server 2019, vedere [Introduzione a Windows Server 2019](https://docs.microsoft.com/windows-server/get-started-19/get-started-19).