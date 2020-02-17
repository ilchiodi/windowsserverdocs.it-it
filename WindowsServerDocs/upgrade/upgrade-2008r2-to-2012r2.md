---
title: Eseguire l'aggiornamento da Windows Server 2008 R2 a Windows Server 2012 R2 | Microsoft Docs
description: Informazioni su come eseguire un aggiornamento sul posto per passare da Windows Server 2008 R2 a Windows Server 2012 R2.
ms.prod: windows server
ms.technology: server-general
ms.topic: upgrade
author: RobHindman
ms.author: robhind
ms.date: 09/16/2019
ms.openlocfilehash: d5051239f7269eb4b6361187121ac960e06f6d9e
ms.sourcegitcommit: 27f0caf74e88781054250455c3c1adf06deb6234
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/19/2019
ms.locfileid: "71125081"
---
# <a name="upgrade-windows-server-2008-r2-to-windows-server-2012-r2"></a>Eseguire l'aggiornamento da Windows Server 2008 R2 a Windows Server 2012 R2

Se vuoi mantenere lo stesso hardware e tutti i ruoli del server che hai già configurato senza rendere flat il server, dovrai eseguire un aggiornamento sul posto. Con un aggiornamento sul posto passerai da un sistema operativo precedente a una versione più recente, mantenendo impostazioni, ruoli del server e dati. Questo articolo descrive come passare da Windows Server 2008 R2 a Windows Server 2012 R2.

Per eseguire l'aggiornamento a Windows Server 2019, usa prima questo argomento per eseguire l'aggiornamento a Windows Server 2012 R2 e quindi [aggiornare da Windows Server 2012 R2 a Windows Server 2019](upgrade-2012r2-to-2019.md).

## <a name="before-you-begin-your-in-place-upgrade"></a>Prima di iniziare l'aggiornamento sul posto

Prima di iniziare l'aggiornamento di Windows Server, consigliamo di raccogliere alcune informazioni dai dispositivi, a scopo di diagnostica e risoluzione dei problemi. Poiché queste informazioni sono utili solo se l'aggiornamento ha esito negativo, devi assicurarti di archiviarle in un punto accessibile dal dispositivo.

### <a name="to-collect-your-info"></a>Per raccogliere le informazioni

1. Apri un prompt dei comandi, passa a `c:\Windows\system32` e quindi digita **systeminfo.exe**.

2. Copia, incolla e archivia le informazioni di sistema risultanti in un punto qualsiasi del dispositivo.

3. Digita **ipconfig /all** al prompt dei comandi e quindi copia e incolla le informazioni di configurazione risultanti nello stesso percorso indicato in precedenza.

4. Apri l'editor del Registro di sistema, passa all'hive HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion e quindi copia e incolla Windows Server **BuildLabEx** (versione) e **EditionID** (edizione) nello stesso percorso indicato in precedenza.

Dopo aver raccolto tutte le informazioni relative a Windows Server, consigliamo vivamente di eseguire il backup del sistema operativo, delle app e delle macchine virtuali. Devi anche **arrestare** ed eseguire una **migrazione rapida** o **in tempo reale** di eventuali macchine virtuali attualmente in esecuzione nel server. Durante l'aggiornamento sul posto le macchine virtuali non devono essere in esecuzione.

## <a name="to-perform-the-upgrade"></a>Per eseguire l'aggiornamento

1. Verifica che il valore **BuildLabEx** confermi l'esecuzione di Windows Server 2008 R2.

2. Individua il supporto di installazione di Windows Server 2012 R2 e quindi seleziona **setup.exe**.

    ![Esplora risorse che visualizza il file setup.exe](media/upgrade-2008r2-2012r2/setup-2012r2.png)

3. Seleziona **Sì** per avviare il processo di installazione.

    ![Controllo dell'account utente che richiede l'autorizzazione per avviare l'installazione](media/upgrade-2008r2-2012r2/start-setup-uac-box.png)

4. Nella schermata Windows Server 2012 R2 seleziona **Installa ora**.

5. Per i dispositivi connessi a Internet, seleziona **Accedi a Internet per scaricare i più recenti aggiornamenti per l'installazione (scelta consigliata)** .

    ![Schermata per scegliere di accedere a Internet per scaricare aggiornamenti Windows importanti](media/upgrade-2008r2-2012r2/imp-updates-win-setup.png)

6. Seleziona l'edizione di Windows Server 2012 R2 che vuoi installare e quindi seleziona **Avanti**.

    ![Schermata per scegliere l'edizione di Windows Server 2012 R2 da installare](media/upgrade-2008r2-2012r2/select-os-edition.png)

7. Seleziona **Accetto le condizioni di licenza** per accettare le condizioni del contratto di licenza, in base al canale di distribuzione (ad esempio, vendita al dettaglio, contratto multilicenza, OEM, ODM e così via) e quindi seleziona **Avanti**.

    ![Schermata per accettare il contratto di licenza](media/upgrade-2008r2-2012r2/license-terms.png)

8. Seleziona **Aggiorna: installa Windows e mantieni file, impostazioni e applicazioni** per scegliere di eseguire un aggiornamento sul posto.

    ![Schermata per scegliere il tipo di installazione](media/upgrade-2008r2-2012r2/choose-install-upgrade.png)

9. Il programma di installazione ricorda di verificare che le app siano compatibili con Windows Server 2012 R2, usando le informazioni contenute nell'articolo [Installazione e aggiornamento di Windows Server](https://docs.microsoft.com/windows-server/get-started/installation-and-upgrade), e quindi seleziona **Avanti**.

    ![Schermata che ricorda di verificare la compatibilità delle app](media/upgrade-2008r2-2012r2/compatibility-report.png)

10. Se visualizzi una pagina che indica che l'aggiornamento non è consigliato, puoi ignorarla e selezionare **Conferma**. È stata inserita per richiedere installazioni pulite, ma non è necessaria.

    ![Schermata che mostra l'avanzamento dell'aggiornamento](media/upgrade-2008r2-2012r2/upgrading-windows-with-progress.png)

    L'aggiornamento sul posto inizia e mostra la schermata **Sto aggiornando Windows** con lo stato di avanzamento. Al termine dell'aggiornamento, il server viene riavviato.

## <a name="after-your-upgrade-is-done"></a>Al termine dell'aggiornamento

Al termine dell'aggiornamento, devi assicurarti che l'aggiornamento a Windows Server 2012 R2 abbia avuto esito positivo.

### <a name="to-make-sure-your-upgrade-was-successful"></a>Per assicurarti che l'aggiornamento sia stato completato

1. Apri l'editor del Registro di sistema, passa all'hive HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion e visualizza il **NomeProdotto**. Dovresti visualizzare l'edizione di Windows Server 2012 R2, ad esempio **Windows Server 2012 R2 Datacenter**.

2. Verifica che tutte le applicazioni siano in esecuzione e che le connessioni client alle applicazioni funzionino correttamente.

Se pensi che si sia verificato un errore durante l'aggiornamento, copia e comprimi la directory `%SystemRoot%\Panther` (in genere `C:\Windows\Panther`) e contatta il Supporto tecnico Microsoft.

## <a name="next-steps"></a>Passaggi successivi

Puoi eseguire uno o più aggiornamenti per passare da Windows Server 2012 R2 a Windows Server 2019. Per istruzioni dettagliate, vedi [Eseguire l'aggiornamento da Windows Server 2012 R2 a Windows Server 2019](upgrade-2012r2-to-2019.md).

## <a name="related-articles"></a>Articoli correlati

- Per altri dettagli e informazioni su Windows Server 2012 R2, vedi [Windows Server 2012 R2 e Windows Server 2012](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh801901(v=ws.11)).
