---
title: Risoluzione dei profili utente quando si verificano eventi
description: Risoluzione dei problemi di caricamento e scaricamento dei profili utente tramite gli eventi e log di traccia.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 6099dac7d77e37b761785b4f58b6106472e5ba1e
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/22/2018
ms.locfileid: "2082301"
---
# <a name="troubleshoot-user-profiles-with-events"></a>Risoluzione dei profili utente quando si verificano eventi

>Si applica a: 10 Windows, Windows 8, Windows 8.1, Windows Server 2012, Windows Server 2012 R2 e Windows Server 2016.

In questo argomento viene descritto come per la risoluzione dei problemi di caricamento e scaricamento dei profili utente tramite gli eventi e log di traccia. Nelle sezioni seguenti viene descritto come utilizzare i registri eventi tre che prendere nota delle informazioni dei profili utente.

## <a name="step-1-checking-events-in-the-application-log"></a>Passaggio 1: Controllo eventi nel registro applicazioni

Il primo passaggio per risolvere i problemi di caricare e scaricare utente profili (inclusi i profili utente comuni) sono possibile utilizzare il Visualizzatore eventi per esaminare tutti gli eventi di avviso ed errore che si collegano i record nell'applicazione del servizio profili utente.

Di seguito viene descritto come visualizzare gli eventi di servizi di profilo utente nel registro dell'applicazione:

1. Avviare il Visualizzatore eventi. A tale scopo, aprire il **Pannello di controllo**, selezionare **sistema e sicurezza**e quindi selezionare **i registri eventi di visualizzazione**nella sezione **Strumenti di amministrazione** . Viene visualizzata la finestra Visualizzatore eventi.
2. Nell'albero della console passare innanzitutto nei **Registri di Windows**, quindi **applicazione**.
3. Nel riquadro azioni, selezionare **Filtro registro corrente**. Verrà visualizzata la finestra di dialogo Filtro registro corrente.
4. Nella finestra di **origini eventi** , selezionare la casella di controllo **Del servizio profili utente** e quindi scegliere **OK**.
5. Esaminare l'elenco degli eventi, con particolare attenzione agli eventi di errore.
6. Quando si trovano gli eventi degne di nota, fare clic sul collegamento Guida registro eventi per visualizzare ulteriori informazioni e procedure sulla risoluzione dei problemi.
7. Per eseguire la risoluzione del problema, annotare la data e ora di eventi degne di nota ed esaminare il registro operativo (come descritto nel passaggio 2) per visualizzare i dettagli relativi alle operazioni eseguite il servizio profili utente al momento degli eventi di errore o avviso.

>[!NOTE]
>È possibile ignorare User Profile Service 1530 "Windows rilevati eventi che i file del Registro di sistema è ancora in uso da altre applicazioni o servizi."

## <a name="step-2-view-the-operational-log-for-the-user-profile-service"></a>Passaggio 2: Visualizzare il registro operativo per il servizio profili utente

Se è possibile risolvere il problema utilizzando il registro applicazioni solo, utilizzare la procedura seguente per visualizzare gli eventi di servizio profili utente nel registro operativo. Questo registro vengono illustrati alcuni dei componenti interni del servizio e contribuisce di individuare dove il carico di profilo o scaricare processo che si è verificato il problema.

Nel registro applicazioni di Windows sia il registro utente Profile Service operativi sono abilitate per impostazione predefinita in tutte le installazioni di Windows.

Di seguito viene descritto come visualizzare il registro operativo per il servizio profili utente:

1. Nel caso in cui il Visualizzatore albero della console, accedere a **registri applicazioni e servizi**, quindi **Microsoft**, quindi **Windows**, quindi **Servizio profili utente**e quindi **operativo**.
2. Esaminare gli eventi che si sono verificati al momento dell'errore o avviso gli eventi che si è preso nota nel registro dell'applicazione.

## <a name="step-3-enable-and-view-analytic-and-debug-logs"></a>Passaggio 3: Abilitare visualizzare analitico e del Registro di debug

Se si richiede più dettagliatamente che fornisce il registro operativo, è possibile abilitare analitico e debug dei log del computer interessato. Questo livello di registrazione è molto più dettagliato e deve essere disabilitato, ad eccezione quando si tenta di risolvere un problema.

Di seguito viene descritto come abilitare visualizzare analitico e del Registro di debug:

1. Nel riquadro **Azioni** del Visualizzatore eventi, selezionare **Visualizza**e quindi selezionare **Mostra analitico registri e Debug**.
2. Accedere a **registri applicazioni e servizi**, quindi **Microsoft**, quindi **Windows**, quindi **servizio profili utente**e quindi **diagnostica**.
3. Selezionare **Attiva registrazione** e quindi su **Sì**. In questo modo il registro diagnostico che verrà avviata la registrazione.
4. Se si richiede anche informazioni dettagliate, vedere [passaggio 4: creazione e decodifica una traccia](#step-4:-creating-and-decoding-a-trace) per ulteriori informazioni su come creare un registro di traccia.
5. Dopo aver terminato la risoluzione dei problemi il problema, passare al Registro di **diagnostica** , selezionare **Disattiva registro**, scegliere **Visualizza** e quindi deselezionare la casella di controllo **Mostra analitico registri e Debug** per nascondere analitico e registrazione.

## <a name="step-4-creating-and-decoding-a-trace"></a>Passaggio 4: Creazione e decodifica una traccia

Se è possibile risolvere il problema utilizzando gli eventi, è possibile creare un registro di traccia (un file ETL) durante la riproduzione del problema e decodificare utilizzando i simboli pubblici dai server di simboli Microsoft. Log di traccia forniscono informazioni molto specifiche sugli elementi di servizio profili utente attività e può essere utile individuare dove si è verificato l'errore.

La strategia migliore quando si utilizza l'analisi ETL viene per prima cosa acquisire il registro più piccolo possibile. Dopo che il registro viene decodificato, eseguire una ricerca di errori nel registro.

Di seguito viene illustrato come creare e decodificare una traccia per il servizio profili utente:

1. Accedere al computer in cui l'utente è stati riscontrati problemi, utilizzando un account membro del gruppo Administrators locale.
2. Da un prompt dei comandi con privilegi elevati immettere i comandi seguenti, dove *\ < Path\ >* è il percorso di una cartella locale precedentemente creato, ad esempio C:\\logs:
        
    ```PowerShell
    logman create trace -n RUP -o <Path>\RUP.etl -ets
    logman update RUP -p {eb7428f5-ab1f-4322-a4cc-1f1a9b2c5e98} 0x7FFFFFFF 0x7 -ets
    ```
3. Nella schermata Start, selezionare il nome utente e quindi selezionare **account Switch**, prestando attenzione a non disconnettere l'amministratore. Se si utilizza Desktop remoto, chiudere la sessione di amministratore per stabilire la sessione dell'utente.
4. Riprodurre il problema. La procedura di riprodurre il problema è in genere per accedere come utente si verifica il problema, effettuare l'accesso utente disattivato, o entrambi.
5. Dopo la riproduzione del problema, come amministratore locale eseguire nuovamente l'accesso.
6. Eseguire il seguente comando per salvare il registro in un file ETL da un prompt dei comandi con privilegi elevati:
  
    ```PowerShell
    logman stop -n RUP -ets
    ```
7. Digitare il comando seguente per esportare il file ETL in un file leggibile nella directory corrente (probabilmente cartella principale o nella cartella %WINDIR%\\System32):
    
    ```PowerShell
    Tracerpt <path>\RUP.etl
    ```
8. Aprire il file **Summary. txt** e file **dumpfile** (si possono aprire in Microsoft Excel per visualizzare più agevolmente i dettagli completi del log). Cercare gli eventi che includono **esito negativo** o **non è riuscita**; è possibile ignorare le righe che includono il nome dell'evento **sconosciuto** .

## <a name="more-information"></a>Ulteriori informazioni

* [Distribuzione di profili utente](deploy-roaming-user-profiles.md)