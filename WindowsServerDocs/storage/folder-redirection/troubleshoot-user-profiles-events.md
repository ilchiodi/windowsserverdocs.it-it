---
title: Risolvere i problemi relativi ai profili utente con eventi
description: Come risolvere i problemi relativi al caricamento e allo scaricamento dei profili utente tramite eventi e log di traccia.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 9e927a77627e786015a928d798aafee13a2cc34b
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "71394376"
---
# <a name="troubleshoot-user-profiles-with-events"></a>Risolvere i problemi relativi ai profili utente con eventi

>Si applica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 e Windows Server (Canale semestrale).

Questo argomento illustra come risolvere i problemi relativi al caricamento e allo scaricamento dei profili utente tramite eventi e log di traccia. Le sezioni seguenti descrivono come usare i tre registri eventi in cui vengono registrate le informazioni sui profili utente.

## <a name="step-1-checking-events-in-the-application-log"></a>Passaggio 1: Verifica degli eventi nel registro applicazioni

Il primo passaggio per la risoluzione dei problemi relativi al caricamento e allo scaricamento dei profili utente (inclusi i profili utente mobili) consiste nell'usare il Visualizzatore eventi per esaminare gli eventuali eventi di avviso e di errore che il servizio profili utente registra nel registro applicazioni.

Di seguito viene illustrato come visualizzare gli eventi del servizio profili utente nel registro applicazioni:

1. Avvia il Visualizzatore eventi. A tale scopo, apri il **Pannello di controllo**, seleziona **Sistema e sicurezza** e quindi nella sezione **Strumenti di amministrazione** seleziona **Visualizza i registri eventi**. Viene visualizzata la finestra Visualizzatore eventi.
2. Nell'albero della console passa prima di tutto a **Registri di Windows** e quindi ad **Applicazione**.
3. Nel riquadro Azioni seleziona **Filtro registro corrente**. Viene visualizzata la finestra di dialogo Filtro registro corrente.
4. Nella casella **Origini eventi** seleziona la casella di controllo **Servizio profili utente** e quindi scegli **OK**.
5. Esamina l'elenco degli eventi, prestando particolare attenzione agli eventi di errore.
6. Quando trovi eventi degni di nota, seleziona il collegamento Guida registro eventi per visualizzare altre informazioni e procedure per la risoluzione dei problemi.
7. Per eseguire altre attività di risoluzione dei problemi, annota la data e l'ora degli eventi degni di nota e quindi esamina il log operativo (come descritto nel passaggio 2) per visualizzare i dettagli sulle operazioni eseguite dal servizio profili utente approssimativamente all'ora degli eventi di errore o di avviso.

>[!NOTE]
>Puoi ignorare tranquillamente l'evento 1530 del servizio profili utente, ovvero "Il file del Registro di sistema è ancora utilizzato da altri servizi o applicazioni".

## <a name="step-2-view-the-operational-log-for-the-user-profile-service"></a>Passaggio 2: Visualizzare il log operativo per il servizio profili utente

Se non riesci a risolvere il problema usando solo il registro applicazioni, esegui la procedura seguente per visualizzare gli eventi del servizio profili utente nel log operativo. Questo log mostra alcuni dei processi interni del servizio e può essere utile per individuare il punto in cui si verifica il problema nel processo di caricamento o scaricamento del profilo.

Il registro applicazioni di Windows e il log operativo del servizio profili utente sono entrambi abilitati per impostazione predefinita in tutte le installazioni Windows.

Di seguito viene illustrato come visualizzare il log operativo per il servizio profili utente:

1. Nell'albero della console del Visualizzatore eventi passa a **Registri applicazioni e servizi**, **Microsoft**, **Windows**, **Servizio profili utente** e quindi **Operativo**.
2. Esamina gli eventi che si sono verificati interno all'orario degli eventi di errore o di avviso notati nel registro applicazioni.

## <a name="step-3-enable-and-view-analytic-and-debug-logs"></a>Passaggio 3: Abilitare e visualizzare il registro analitico e quello di debug

Se hai necessità di altri dettagli oltre a quelli forniti dal log operativo, puoi abilitare il registro analitico e quello di debug nel computer interessato. Questo livello di registrazione è molto più dettagliato e deve essere disabilitato, tranne quando tenti di risolvere un problema.

Di seguito viene illustrato come abilitare e visualizzare il registro analitico e quello di debug:

1. Nel riquadro **Azioni** del Visualizzatore eventi seleziona **Visualizza** e quindi **Visualizza registri analitici e di debug**.
2. Passa a **Registri applicazioni e servizi**, **Microsoft**, **Windows**, **Servizio profili utente** e quindi **Diagnostica**.
3. Seleziona **Attiva registro** e quindi scegli **Sì**. In questo modo viene abilitato il log di diagnostica, che avvia la registrazione.
4. Se hai necessità di informazioni ancora più dettagliate, vedi [Passaggio 4: Creazione e decodifica di una traccia](#step-4-creating-and-decoding-a-trace) per avere altre informazioni su come creare un log di traccia.
5. Al termine della risoluzione del problema, passa al log di **Diagnostica**, seleziona **Disattiva registro**, seleziona **Visualizza** e quindi deseleziona la casella di controllo **Visualizza registri analitici e di debug** per nascondere la registrazione analitica e di debug.

## <a name="step-4-creating-and-decoding-a-trace"></a>Passaggio 4: Creazione e decodifica di una traccia

Se non riesci a risolvere il problema usando gli eventi, puoi creare un log di traccia (un file ETL) durante la riproduzione del problema e quindi decodificarlo usando i simboli pubblici del server dei simboli Microsoft. I log di traccia forniscono informazioni molto specifiche sulle operazioni eseguite dal servizio profili utente e consentono di individuare il punto in cui si è verificato l'errore.

Quando viene usata la traccia ETL, la strategia migliore consiste prima di tutto nell'acquisire il log delle minori dimensioni possibili. Una volta decodificato il log, cerca gli errori in esso riportati.

Di seguito viene illustrato come creare e decodificare una traccia per il servizio profili utente:

1. Accedi al computer in cui l'utente riscontra problemi, usando un account membro del gruppo Administrators locale.
2. A un prompt dei comandi con privilegi elevati immetti i comandi seguenti, dove *\<Path\>* è il percorso di una cartella locale creata in precedenza, ad esempio C:\\logs:
        
    ```PowerShell
    logman create trace -n RUP -o <Path>\RUP.etl -ets
    logman update RUP -p {eb7428f5-ab1f-4322-a4cc-1f1a9b2c5e98} 0x7FFFFFFF 0x7 -ets
    ```
3. Nella schermata Start seleziona il nome utente e quindi scegli **Cambia account**, facendo attenzione a non disconnettere l'amministratore. Se usi Desktop remoto, chiudi la sessione di amministrazione per stabilire la sessione utente.
4. Riproduci il problema. La procedura per riprodurre il problema in genere consiste nell'eseguire l'accesso come l'utente che riscontra il malfunzionamento, nel disconnettere l'utente o nell'eseguire entrambe le operazioni.
5. Dopo aver riprodotto il problema, accedi di nuovo come amministratore locale.
6. A un prompt dei comandi con privilegi elevati esegui il comando seguente per salvare il log in un file ETL:
  
    ```PowerShell
    logman stop -n RUP -ets
    ```
7. Digita il comando seguente per esportare il file ETL in un file leggibile nella directory corrente (probabilmente la home directory o la cartella %WINDIR%\\System32):
    
    ```PowerShell
    Tracerpt <path>\RUP.etl
    ```
8. Apri i file **Summary.txt** e **Dumpfile.xml** (puoi aprirli in Microsoft Excel per visualizzare più facilmente i dettagli completi del log). Cerca gli eventi che includono **fail** o **failed**. Puoi ignorare tranquillamente le righe che contengono il nome evento **Unknown**.

## <a name="more-information"></a>Altre informazioni

* [Distribuire profili utente mobili](deploy-roaming-user-profiles.md)