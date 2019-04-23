---
title: Analizzatore diagnostico della Guida di AD FS
description: Questo documento descrive analizzatore diagnostica consentono di AD FS e come è possibile eseguire basic controlla con diagnostica di AD FS modulo di PowerShell.
author: billmath
ms.author: billmath
manager: mtillman
ms.reviewer: anandyadavMSFT
ms.date: 02/13/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a5b5f895a0575094f8f1af950bde82e1d56325b2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829122"
---
# <a name="ad-fs-help-diagnostics-analyzer"></a>Analizzatore diagnostico della Guida di AD FS

AD FS offre numerose impostazioni che supportano l'ampia gamma di funzionalità da essa fornite per lo sviluppo dell'applicazione e l'autenticazione. Durante la risoluzione dei problemi, è consigliabile assicurarsi che tutte le impostazioni di ADFS siano configurate correttamente. Esegue una verifica manuale di queste impostazioni possono talvolta richiedere molto tempo. AD FS della Guida diagnostica Analyzer aiuta a eseguire i controlli di base usando il modulo ADFSToolbox PowerShell. Dopo aver eseguito i controlli, Guida di ADFS offre i [analizzatore diagnostica](https://aka.ms/adfsdiagnosticsanalyzer) che consentono di visualizzare i risultati facilmente e offrono procedure di correzione.

Completare l'operazione accetta 3 semplici passaggi:

1. Configurare il modulo ADFSToolbox nel server AD FS primario o nel server WAP
2. Eseguire la diagnostica e caricare il file di Guida di ADFS
3. Visualizzare analisi diagnostica e risolvere eventuali problemi

Passare a [analizzatore diagnostica consentono di AD FS (https://aka.ms/adfsdiagnosticsanalyzer) ](https://aka.ms/adfsdiagnosticsanalyzer) per avviare la risoluzione dei problemi.

![Strumento Analizzatore di diagnostica AD FS nella Guida di ADFS](media/ad-fs-diagonostics-analyzer/home.png)

## <a name="step-1-setup-the-adfstoolbox-module-on-ad-fs-server"></a>Passaggio 1: Programma di installazione del modulo ADFSToolbox nel server AD FS

Per eseguire la [analizzatore diagnostica](https://aka.ms/adfsdiagnosticsanalyzer), è necessario installare il modulo ADFSToolbox PowerShell. Se il server AD FS disponga della connettività a internet, è possibile installare il modulo ADFSToolbox direttamente da PowerShell gallery. In caso di nessuna connettività a internet, clonare il repository di GitHub per l'installazione manuale. 

![Analizzatore di diagnostica di AD FS - programma di installazione](media/ad-fs-diagonostics-analyzer/step1.png)

### <a name="setup-using-powershell-gallery"></a>Installazione mediante PowerShell gallery

Se il server AD FS abbia la connettività internet, è consigliabile installare il modulo ADFSToolbox direttamente da PowerShell gallery usando i comandi di PowerShell indicati di seguito.
 
   ```powershell 
    Install-Module -Name ADFSToolbox -force
    Import-Module ADFSToolbox -force
   ```
### <a name="setup-manually-by-cloning-the-repository"></a>Configurare manualmente tramite la clonazione del repository

Il modulo ADFSToolbox può essere installato manualmente da GitHub direttamente. Seguire le istruzioni seguenti per la clonazione del repository e l'installazione del modulo ADFSToolbox nei server AD FS.

1. Scaricare il [repository](https://github.com/Microsoft/adfsToolbox/archive/master.zip)
2. Decomprimere il file scaricato e copiare la cartella adfsToolbox-master in % SYSTEMDRIVE %\\Program Files\\WindowsPowerShell\\moduli\\.
3. Importare il modulo di PowerShell. In una finestra di PowerShell con privilegi elevata, eseguire le operazioni seguenti:
 
   ```powershell 
    Import-Module ADFSToolbox -Force
   ```

## <a name="step-2-execute-the-diagnostics-and-upload-the-file-to-ad-fs-help"></a>Passaggio 2: Eseguire la diagnostica e caricare il file di Guida di ADFS

Un singolo comando utilizzabile per eseguire facilmente i test diagnostici tra tutti i server AD FS nella farm. Il modulo PowerShell userà le sessioni remote di Powershell per eseguire i test di diagnostica in server diversi nella farm.

```powershell
    Export-AdfsDiagnosticsFile [-adfsServers <list of servers>]
```

In una farm AD FS 2016 in Server, il comando leggerà l'elenco dei server AD FS dalla configurazione di AD FS. I test diagnostici sono quindi eseguito un tentativo di ogni server nell'elenco. Se l'elenco dei server AD FS non è disponibile (esempio 2012R2), quindi i test vengono eseguiti sul computer locale. Per specificare un elenco di server su cui verranno eseguiti i test, usare il **adfsServers** argomento per fornire un elenco di server. Di seguito viene fornito un esempio

```powershell
    Export-AdfsDiagnosticsFile -adfsServers @("sts1.contoso.com", "sts2.contoso.com", "sts3.contoso.com")
```

Il risultato è un file JSON che viene creato nella stessa directory in cui è stato eseguito il comando. Il nome del file è ADFSDiagnosticsFile\<timestamp\>. Nome del file di esempio è ADFSDiagnosticsFile 20180716124030.

Nel passaggio 2 nella [ https://aka.ms/adfsdiagnosticsanalyzer ](https://aka.ms/adfsdiagnosticsanalyzer) consente di selezionare il file dei risultati per caricare il browser file.

![Strumento Analizzatore di AD FS diagnostica - execute e carica i risultati](media/ad-fs-diagonostics-analyzer/step2.png)

Fare clic su **caricare** per completare il caricamento e procedere al passaggio successivo.

## <a name="step-3-view-diagnostics-analysis-and-resolve-any-issues"></a>Passaggio 3: Visualizzare analisi diagnostica e risolvere eventuali problemi

Esistono quattro sezioni dei risultati di test:

1. Non riuscita: In questa sezione contiene l'elenco di test non riusciti. Ogni risultato è costituito da:
2. Avviso: In questa sezione contiene un elenco di test che hanno comportato un messaggio di avviso. Questi problemi comporterà una non eventuali problemi per l'autenticazione su una scala più ampia, ma da risolvere appena possibile.
3. Non applicabile: In questa sezione contiene l'elenco dei test che non sono stati eseguiti perché non sono stati applicabili per il server su cui è stato in esecuzione il comando.
4. Passato: In questa sezione contiene l'elenco di test passati e non dispongono di alcun intervento dell'utente.

Ogni risultato del test viene visualizzato con informazioni dettagliate che descrivono il test e la risoluzione di questa procedura:

1. Nome test: Nome del test che è stata eseguita
2. Dettagli: Descrizione dell'operazione complessiva che è stata eseguita durante il test
3. Passaggi per la risoluzione del problema: I passaggi consigliati per risolvere il problema evidenziato dal test

![Strumento Analizzatore di AD FS diagnostica - elenco dei risultati dei test](media/ad-fs-diagonostics-analyzer/step3a.png)

![Strumento di analyzer di AD FS diagnostica - risoluzione di errori](media/ad-fs-diagonostics-analyzer/step3b.png)

## <a name="next"></a>Avanti

- [Usare le guide troublehshooting Guida di ADFS](https://aka.ms/adfshelp/troubleshooting )
- [Risoluzione dei problemi di AD FS](ad-fs-tshoot-overview.md)

 
