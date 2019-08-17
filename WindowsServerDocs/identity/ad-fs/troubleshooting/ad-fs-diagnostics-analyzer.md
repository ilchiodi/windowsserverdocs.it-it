---
title: Analizzatore diagnostico della Guida di AD FS
description: Questo documento descrive AD FS Help Diagnostics Analyzer e come può eseguire i controlli di base usando AD FS modulo di PowerShell di diagnostica.
author: billmath
ms.author: billmath
manager: mtillman
ms.reviewer: anandyadavMSFT
ms.date: 03/29/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6b6b7563aaa1f3c7d706cfdd172faf18417623e5
ms.sourcegitcommit: 0467b8e69de66e3184a42440dd55cccca584ba95
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2019
ms.locfileid: "69546608"
---
# <a name="ad-fs-help-diagnostics-analyzer"></a>Analizzatore diagnostico della Guida di AD FS

AD FS dispone di numerose impostazioni che supportano la vasta gamma di funzionalità fornite per l'autenticazione e lo sviluppo di applicazioni. Durante la risoluzione dei problemi, è consigliabile assicurarsi che tutte le impostazioni di AD FS siano configurate correttamente. Una verifica manuale di queste impostazioni può talvolta richiedere molto tempo. AD FS Help Diagnostics Analyzer può essere utile per eseguire i controlli di base usando il modulo ADFSToolbox di PowerShell. Dopo aver eseguito i controlli, AD FS la guida fornisce l'analizzatore di [diagnostica](https://aka.ms/adfsdiagnosticsanalyzer) che consente di visualizzare facilmente i risultati e di offrire procedure correttive.

L'operazione completa richiede tre semplici passaggi:

1. Configurare il modulo ADFSToolbox nel server AD FS primario o nel server WAP
2. Eseguire la diagnostica e caricare il file nella Guida AD FS
3. Visualizza analisi diagnostica e risolvi eventuali problemi

Passare a [ad FS Help Diagnostics Analyzer https://aka.ms/adfsdiagnosticsanalyzer) (](https://aka.ms/adfsdiagnosticsanalyzer) per avviare la risoluzione dei problemi.

![Strumento AD FS Diagnostics Analyzer nella Guida di AD FS](media/ad-fs-diagonostics-analyzer/home.png)

## <a name="step-1-setup-the-adfstoolbox-module-on-ad-fs-server"></a>Passaggio 1: Configurare il modulo ADFSToolbox nel server AD FS

Per eseguire l' [analizzatore di diagnostica](https://aka.ms/adfsdiagnosticsanalyzer), è necessario installare il modulo ADFSToolbox di PowerShell. Se il server AD FS dispone di connettività a Internet, è possibile installare il modulo ADFSToolbox direttamente da PowerShell Gallery. In caso di assenza di connettività a Internet, è possibile installarla manualmente. 

[!WARNING]
Se si usa AD FS 2,1 o versione precedente, è necessario installare la versione 1.0.13 di ADFSToolbox. ADFSToolbox non supporta più AD FS 2,1 o versioni precedenti nelle versioni più recenti.

![Analizzatore diagnostica AD FS-installazione](media/ad-fs-diagonostics-analyzer/step1_v2.png)

### <a name="setup-using-powershell-gallery"></a>Installazione tramite PowerShell Gallery

Se il server AD FS ha la connettività Internet, è consigliabile installare il modulo ADFSToolbox direttamente da PowerShell Gallery usando i comandi di PowerShell indicati di seguito.

   ```powershell
    Install-Module -Name ADFSToolbox -force
    Import-Module ADFSToolbox -force
   ```

### <a name="setup-manually"></a>Installazione manuale

Il modulo ADFSToolbox deve essere copiato manualmente nei server AD FS o WAP. Seguire le istruzioni riportate di seguito.

1. Avviare una finestra di PowerShell con privilegi elevati in un computer dotato di accesso a Internet.
2. Installare il modulo della casella degli strumenti AD FS.

    ```powershell
    Install-Module -Name ADFSToolbox -Force
    ```
3. Copiare la cartella ADFSToolbox che `%SYSTEMDRIVE%\Program Files\WindowsPowerShell\Modules\` si trova nel computer locale nello stesso percorso nel computer ad FS o WAP.

4. Avviare una finestra di PowerShell con privilegi elevati nel computer AD FS ed eseguire il cmdlet seguente per importare il modulo.

    ```powershell
    Import-Module ADFSToolbox -Force
    ```

## <a name="step-2-execute-the-diagnostics-cmdlet"></a>Passaggio 2: Eseguire il cmdlet Diagnostics

![Strumento Analizzatore diagnostica AD FS-eseguire e caricare i risultati](media/ad-fs-diagonostics-analyzer/step2_v2.png)

È possibile utilizzare un singolo comando per eseguire agevolmente i test di diagnostica in tutti i server AD FS della farm. Il modulo di PowerShell userà le sessioni remote di PowerShell per eseguire i test di diagnostica in diversi server della farm.

```powershell
    Export-AdfsDiagnosticsFile [-ServerNames <list of servers>]
```

In un server 2016 o versione successiva AD FS Farm, il comando leggerà l'elenco dei server AD FS dalla configurazione AD FS. I test di diagnostica vengono quindi tentati per ogni server nell'elenco. Se l'elenco dei server AD FS non è disponibile (ad esempio 2012R2), i test vengono eseguiti sul computer locale. Per specificare un elenco di server in cui devono essere eseguiti i test, utilizzare l'argomento **serverNames** per fornire un elenco di server. Di seguito viene fornito un esempio

```powershell
    Export-AdfsDiagnosticsFile -ServerNames @("adfs1.contoso.com", "adfs2.contoso.com")
```

Il risultato è un file JSON creato nella stessa directory in cui è stato eseguito il comando. Il nome del file è AdfsDiagnosticsFile-\<timestamp.\> Un nome file di esempio è AdfsDiagnosticsFile-07312019-184201. JSON.

## <a name="step-3-upload-the-diagnostics-file"></a>Passaggio 3: Caricare il file di diagnostica

Nel passaggio 3 [https://aka.ms/adfsdiagnosticsanalyzer](https://aka.ms/adfsdiagnosticsanalyzer) in usare il browser file per selezionare il file di risultati da caricare.

Per completare il caricamento, fare clic su **carica** .

Eseguendo l'accesso con un account Microsoft, i risultati della diagnostica possono essere salvati per un punto di visualizzazione successivo e possono essere inviati al supporto tecnico Microsoft. Se in qualsiasi momento si apre un caso di supporto, Microsoft sarà in grado di visualizzare i risultati dell'analizzatore di diagnostica e di risolvere il problema più rapidamente.

![Strumento Analizzatore diagnostica AD FS-accedere](media/ad-fs-diagonostics-analyzer/sign_in_step.png)

## <a name="step-4-view-diagnostics-analysis-and-resolve-any-issues"></a>Passaggio 4: Visualizza analisi diagnostica e risolvi eventuali problemi

Sono disponibili cinque sezioni dei risultati del test:

1. Non riuscita: Questa sezione contiene un elenco di test non riusciti. Ogni risultato è costituito da:
2. Avviso: Questa sezione contiene un elenco di test che hanno generato un avviso. Questi problemi non provocheranno probabilmente problemi di autenticazione su una scala più ampia, ma devono essere risolti al più presto.
3. Passato Questa sezione contiene l'elenco dei test che sono stati superati e non hanno elementi di azione per l'utente.
4. Non eseguito: Questa sezione contiene l'elenco dei test che non è stato possibile eseguire a causa di informazioni mancanti.
5. Non applicabile: Questa sezione contiene l'elenco dei test non eseguiti perché non sono applicabili per il server specifico in cui è stato eseguito il comando.

![Strumento Analizzatore ad FS Diagnostics: elenco](media/ad-fs-diagonostics-analyzer/step3a_v3.png) dei risultati del test. ogni risultato del test viene visualizzato con i dettagli che descrivono il test e la risoluzione dei passaggi:

1. Nome test: Nome del test eseguito
2. Descrizione: Descrizione del test.
3. Dettagli: Descrizione dell'operazione complessiva eseguita durante il test
4. Passaggi per la risoluzione del problema: Procedura consigliata per risolvere il problema evidenziato dal test

![Strumento Analizzatore diagnostica AD FS-risoluzione degli errori](media/ad-fs-diagonostics-analyzer/step3b_v3.png)

## <a name="next"></a>Avanti

- [Usare AD FS guida troublehshooting](https://aka.ms/adfshelp/troubleshooting )
- [Risoluzione dei problemi relativi ad AD FS](ad-fs-tshoot-overview.md)
