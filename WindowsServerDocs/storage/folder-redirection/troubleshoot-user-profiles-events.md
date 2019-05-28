---
title: Risolvere i problemi di profili utente con eventi
description: Come risolvere i problemi durante il caricamento e scaricamento dei profili utente mobili usando gli eventi e log di traccia.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: f30bfcd531731e3a0d14350536ddf418c50f3ea0
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2019
ms.locfileid: "65475944"
---
# <a name="troubleshoot-user-profiles-with-events"></a>Risolvere i problemi di profili utente con eventi

>Si applica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 e Windows Server (canale semestrale).

Questo argomento descrive come risolvere i problemi durante il caricamento e scaricamento dei profili utente usando gli eventi e log di traccia. Le sezioni seguenti descrivono come usare i tre registri eventi che registrano informazioni sul profilo utente.

## <a name="step-1-checking-events-in-the-application-log"></a>Passaggio 1: Verifica degli eventi nel registro applicazioni

Il primo passaggio nella risoluzione dei problemi di caricamento e scaricamento utente profili (inclusi i profili utente mobili) consiste nell'utilizzare il Visualizzatore eventi per esaminare gli eventi di avviso ed errore che i record nell'applicazione servizio profili utente di log.

Di seguito viene illustrato come visualizzare gli eventi di servizi del profilo utente nel registro applicazioni di:

1. Avviare il Visualizzatore eventi. A tale scopo, aprire **Pannello di controllo**, selezionare **sistema e sicurezza**, quindi il **strumenti di amministrazione** selezionare **controllare nei registri eventi**. Verrà visualizzata la finestra del Visualizzatore eventi.
2. Nell'albero della console, passare innanzitutto alla **registri di Windows**, quindi **applicazione**.
3. Nel riquadro azioni, selezionare **filtro registro corrente**. Verrà visualizzata la finestra di dialogo Filtro registro corrente.
4. Nel **origini eventi** , quindi selezionare la **servizio profili utente** casella di controllo e quindi selezionare **OK**.
5. Esaminare l'elenco di eventi, prestando particolare attenzione agli eventi di errore.
6. Quando si individua eventi degno di nota, selezionare il collegamento Guida in linea registro eventi per visualizzare informazioni aggiuntive e procedure di risoluzione dei problemi.
7. Per eseguire un'ulteriore risoluzione dei problemi, si noti la data e l'ora degli eventi degno di nota ed esaminare il registro operativo (come descritto nel passaggio 2) per visualizzare i dettagli sull'attività del servizio profili utente alla stessa ora degli eventi di errore o avviso.

>[!NOTE]
>È possibile ignorare tranquillamente User Profile Service evento 1530 "Windows ha rilevato che il file del Registro di sistema è ancora in uso da altre applicazioni o servizi."

## <a name="step-2-view-the-operational-log-for-the-user-profile-service"></a>Passaggio 2: Visualizzare il registro operativo per il servizio profili utente

Se è possibile risolvere il problema usando il registro applicazioni da solo, usare la procedura seguente per visualizzare gli eventi del servizio profili utente nel registro operativo. Questo log mostra alcune dei meccanismi interni del servizio e può essere utile individuare dove il caricamento del profilo o scaricare il processo che ha causato il problema.

Sia nel registro applicazioni di Windows e il registro operativo del servizio di profilo utente sono abilitate per impostazione predefinita in tutte le installazioni di Windows.

Di seguito viene illustrato come visualizzare il registro operativo per il servizio profili utente:

1. Nell'albero della console Visualizzatore eventi passare a **registri applicazioni e servizi**, quindi **Microsoft**, quindi **Windows**, quindi **User Profile Service**e quindi **Operational**.
2. Esaminare gli eventi verificatisi alla stessa ora degli eventi di errore o avviso che si è preso nota nel registro dell'applicazione.

## <a name="step-3-enable-and-view-analytic-and-debug-logs"></a>Passaggio 3: Abilitare e visualizzare analisi e i registri di debug

Se sono necessarie più dettagliatamente che fornisce il registro operativo, è possibile abilitare analitico e debug dei log nel computer interessato. Questo livello di registrazione sia molto più dettagliato e deve essere disabilitato, ad eccezione di quando si tenta di risolvere un problema.

Di seguito viene illustrato come abilitare e visualizzare analisi e i registri di debug:

1. Nel **azioni** riquadro del Visualizzatore eventi, selezionare **View**e quindi selezionare **Visualizza registri analitici e Debug**.
2. Passare a **registri applicazioni e servizi**, quindi **Microsoft**, quindi **Windows**, quindi **User Profile Service**, quindi  **Diagnostica**.
3. Selezionare **Attiva registro** e quindi selezionare **Yes**. In questo modo il log di diagnostica, che verrà avviata la registrazione.
4. Se sono necessarie informazioni ancora più dettagliate, vedere [passaggio 4: Creazione e la decodifica di una traccia](#step-4-creating-and-decoding-a-trace) per altre informazioni su come creare un log di traccia.
5. Dopo averli completati alla risoluzione del problema, passare al **diagnostica** log, selezionare **Disattiva registro**, selezionare **visualizzazione** e quindi deselezionare la **Mostra Registri di Debug e analitico** casella di controllo per nascondere analitiche e la registrazione debug.

## <a name="step-4-creating-and-decoding-a-trace"></a>Passaggio 4: Creazione e la decodifica di una traccia

Se non è possibile risolvere il problema usando gli eventi, è possibile creare un log di traccia, un file ETL, durante la riproduzione del problema e poi decodificarlo e uso dei simboli pubblici da Microsoft symbol server. I log di traccia forniscono informazioni molto specifiche su ciò che il servizio profili utente, infatti e possono aiutarti a individuare dove si è verificato l'errore.

La strategia migliore quando si usa la traccia ETL consiste nell'acquisire prima del log più piccolo possibile. Dopo che il log viene decodificato, cercare il log degli errori.

Di seguito viene illustrato come creare e la decodifica di una traccia per il servizio profili utente:

1. Accedere al computer in cui l'utente si è verificati problemi, usando un account membro del gruppo Administrators locale.
2. Al prompt dei comandi con privilegi elevati digitare i comandi seguenti, dove *\<percorso\>* è il percorso in una cartella locale creato in precedenza, ad esempio c:\\log:
        
    ```PowerShell
    logman create trace -n RUP -o <Path>\RUP.etl -ets
    logman update RUP -p {eb7428f5-ab1f-4322-a4cc-1f1a9b2c5e98} 0x7FFFFFFF 0x7 -ets
    ```
3. Dalla schermata Start, selezionare il nome utente e quindi selezionare **cambia account**, prestando attenzione a non esegue la disconnessione l'amministratore. Se si usa Desktop remoto, chiudere la sessione di amministrazione per stabilire la sessione utente.
4. Riprodurre il problema. La procedura per riprodurre il problema è in genere per accedere come l'utente verifica il problema, disconnettere l'utente, o entrambi.
5. Dopo la riproduzione del problema, accedere come amministratore locale nuovamente.
6. Al prompt dei comandi con privilegi elevati eseguire il comando seguente per salvare il log in un file con estensione ETL:
  
    ```PowerShell
    logman stop -n RUP -ets
    ```
7. Digitare il comando seguente per esportare il file con estensione ETL in un file leggibile dall'utente nella directory corrente (probabilmente la cartella home o % WINDIR %\\cartella System32):
    
    ```PowerShell
    Tracerpt <path>\RUP.etl
    ```
8. Aprire il **Summary** file e **dumpfile** file (è possibile aprirli in Microsoft Excel per visualizzare più facilmente i dettagli completi del log). Cercare gli eventi che includono **esito negativo** oppure **non è stato possibile**; è possibile ignorare le righe che includono il **sconosciuto** nome dell'evento.

## <a name="more-information"></a>Altre informazioni

* [Distribuire profili utente mobili](deploy-roaming-user-profiles.md)