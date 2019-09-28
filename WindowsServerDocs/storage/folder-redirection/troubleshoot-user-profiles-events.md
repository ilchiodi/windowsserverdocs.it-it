---
title: Risolvere i problemi relativi ai profili utente con eventi
description: Come risolvere i problemi relativi al caricamento e allo scaricamento di profili utente mediante eventi e log di traccia.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 9e927a77627e786015a928d798aafee13a2cc34b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394376"
---
# <a name="troubleshoot-user-profiles-with-events"></a>Risolvere i problemi relativi ai profili utente con eventi

>Si applica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 e Windows Server (canale semestrale).

In questo argomento viene illustrato come risolvere i problemi relativi al caricamento e allo scaricamento di profili utente mediante eventi e log di traccia. Le sezioni seguenti descrivono come usare i tre registri eventi che registrano le informazioni sul profilo utente.

## <a name="step-1-checking-events-in-the-application-log"></a>Passaggio 1: Verifica degli eventi nel registro applicazioni

Il primo passaggio per la risoluzione dei problemi relativi al caricamento e allo scaricamento di profili utente (inclusi i profili utente mobili) consiste nell'utilizzare Visualizzatore eventi per esaminare gli eventi di avviso e di errore che i record del servizio profili utente sono presenti nel registro applicazioni.

Di seguito viene illustrato come visualizzare gli eventi dei servizi profili utente nel registro applicazioni:

1. Avviare Visualizzatore eventi. A tale scopo, aprire **il pannello di controllo**, selezionare **sistema e sicurezza**, quindi, nella sezione **strumenti di amministrazione** , selezionare **Visualizza registri eventi**. Viene visualizzata la finestra Visualizzatore eventi.
2. Nell'albero della console passare prima a **registri di Windows**, quindi **applicazione**.
3. Nel riquadro azioni selezionare **Filtro registro corrente**. Verrà visualizzata la finestra di dialogo Filtra log corrente.
4. Nella casella **origini eventi** selezionare la casella di controllo **servizio profili utente** e quindi fare clic su **OK**.
5. Esaminare l'elenco degli eventi, prestando particolare attenzione agli eventi di errore.
6. Quando si trovano eventi degni di nota, selezionare il collegamento alla Guida in linea registro eventi per visualizzare informazioni aggiuntive e procedure per la risoluzione dei problemi.
7. Per eseguire ulteriori operazioni di risoluzione dei problemi, annotare la data e l'ora degli eventi degni di nota ed esaminare il registro operativo, come descritto nel passaggio 2, per visualizzare i dettagli sulle operazioni eseguite dal servizio profili utente per l'ora degli eventi di errore o di avviso.

>[!NOTE]
>È possibile ignorare in modo sicuro l'evento 1530 "Windows ha rilevato che il file del registro di sistema è ancora in uso da altre applicazioni o servizi".

## <a name="step-2-view-the-operational-log-for-the-user-profile-service"></a>Passaggio 2: Visualizzare il registro operativo per il servizio profili utente

Se non è possibile risolvere il problema utilizzando solo il registro applicazioni, attenersi alla procedura seguente per visualizzare gli eventi del servizio profili utente nel registro operativo. Questo log mostra alcuni dei processi interni del servizio e consente di individuare il punto in cui si verifica il problema nel caricamento o nello scaricamento del profilo.

Sia il registro applicazioni di Windows sia il registro operativo del servizio profili utente sono abilitati per impostazione predefinita in tutte le installazioni di Windows.

Di seguito viene illustrato come visualizzare il log operativo per il servizio profili utente:

1. Nell'albero della console Visualizzatore eventi passare a **registri applicazioni e servizi**, quindi a **Microsoft**, **Windows**, **servizio profili utente**e quindi **operativo**.
2. Esaminare gli eventi che si sono verificati al momento dell'errore o degli eventi di avviso indicati nel registro applicazioni.

## <a name="step-3-enable-and-view-analytic-and-debug-logs"></a>Passaggio 3: Abilitare e visualizzare i log analitici e di debug

Se è necessario un maggior numero di dettagli rispetto a quello fornito dal log operativo, è possibile abilitare i registri analitici e di debug nel computer interessato. Questo livello di registrazione è molto più dettagliato e deve essere disabilitato tranne quando si tenta di risolvere un problema.

Di seguito viene illustrato come abilitare e visualizzare i log analitici e di debug:

1. Nel riquadro **azioni** di Visualizzatore eventi selezionare **Visualizza, quindi selezionare Mostra** **log analitici e di debug**.
2. Passare a **registri applicazioni e servizi**, quindi **Microsoft**, **Windows**, **servizio profili utente**e quindi **diagnostica**.
3. Selezionare **Abilita log** , quindi selezionare **Sì**. In questo modo viene abilitato il log di diagnostica, che avvia la registrazione.
4. Se sono necessarie informazioni ancora più dettagliate, vedere [Step 4: Creazione e decodifica di una traccia @ no__t-0 per ulteriori informazioni sulla creazione di un log di traccia.
5. Al termine della risoluzione del problema, passare al log di **diagnostica** , selezionare **Disabilita log**, selezionare **Visualizza** , quindi deselezionare la casella di controllo Mostra **log analitici e debug** per nascondere la registrazione analitica e di debug.

## <a name="step-4-creating-and-decoding-a-trace"></a>Passaggio 4: Creazione e decodifica di una traccia

Se non è possibile risolvere il problema utilizzando gli eventi, è possibile creare un log di traccia (un file ETL) durante la riproduzione del problema e quindi decodificarlo utilizzando i simboli pubblici del server dei simboli Microsoft. I log di traccia forniscono informazioni molto specifiche sulle operazioni eseguite dal servizio profili utente e consentono di individuare il punto in cui si è verificato l'errore.

La strategia migliore quando si usa la traccia ETL è acquisire prima di tutto il log più piccolo possibile. Una volta decodificato il log, cercare nel registro gli errori.

Di seguito viene illustrato come creare e decodificare una traccia per il servizio profili utente:

1. Accedere al computer in cui si sono verificati i problemi dell'utente, utilizzando un account membro del gruppo Administrators locale.
2. Da un prompt dei comandi con privilegi elevati, immettere i comandi seguenti, dove *\<Path @ no__t-2* è il percorso di una cartella locale creata in precedenza, ad esempio C: \\logs:
        
    ```PowerShell
    logman create trace -n RUP -o <Path>\RUP.etl -ets
    logman update RUP -p {eb7428f5-ab1f-4322-a4cc-1f1a9b2c5e98} 0x7FFFFFFF 0x7 -ets
    ```
3. Dalla schermata Start selezionare il nome utente e quindi fare clic su **Cambia account**, facendo attenzione a non disconnettere l'amministratore. Se si utilizza Desktop remoto, chiudere la sessione di amministrazione per stabilire la sessione utente.
4. Riprodurre il problema. La procedura per riprodurre il problema è in genere eseguire l'accesso quando l'utente riscontra il problema, disconnettere l'utente o entrambi.
5. Dopo aver riprodotto il problema, accedere di nuovo come amministratore locale.
6. Da un prompt dei comandi con privilegi elevati, eseguire il comando seguente per salvare il log in un file ETL:
  
    ```PowerShell
    logman stop -n RUP -ets
    ```
7. Digitare il comando seguente per esportare il file ETL in un file leggibile nella directory corrente (probabilmente la cartella Home o la cartella% WINDIR% \\System32):
    
    ```PowerShell
    Tracerpt <path>\RUP.etl
    ```
8. Aprire il file **Summary. txt** e il file **dumpfile. XML** (è possibile aprirli in Microsoft Excel per visualizzare più facilmente i dettagli completi del log). Cerca gli eventi che includono **Fail** o **failed**; è possibile ignorare in modo sicuro le righe che includono il nome di evento **sconosciuto** .

## <a name="more-information"></a>Altre informazioni

* [Distribuire profili utente mobili](deploy-roaming-user-profiles.md)