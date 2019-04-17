---
title: Utilizzare Robocopy per preseed i file per la replica DFS
description: Come utilizzare Robocopy.exe per preseed i file per la replica DFS.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: a7a14b1a1e0f91002b201869e4c68187ffaf3f8f
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/22/2018
ms.locfileid: "2082287"
---
# <a name="use-robocopy-to-preseed-files-for-dfs-replication"></a>Utilizzare Robocopy per preseed i file per la replica DFS

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

In questo argomento viene illustrato come utilizzare lo strumento da riga di comando, **Robocopy.exe**per preseed file quando si configura la replica per la replica nel sistema DFS (Distributed File) (noto anche come che il servizio o DFS-R) in Windows Server. Per preseeding file prima di configurare la replica DFS, aggiungere un nuovo partner di replica o sostituire un server, è possibile accelerare la sincronizzazione iniziale e consentire la duplicazione del database di replica DFS in Windows Server 2012 R2. Il metodo Robocopy è uno dei seguenti metodi preseeding; per una panoramica, vedere [passaggio 1: preseed file per la replica DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495046(v%3dws.11)>).

L'utilità da riga di comando Robocopy (copia File affidabile) è incluso in Windows Server. L'utilità sono disponibili opzioni estese che includono Copia protezione, backup API supporto, funzionalità di tentativi e la registrazione. Versioni successive includono il supporto dei / o il multithread e riattivare memorizzato nella cache.

>[!IMPORTANT]
>Robocopy non vengono copiati esclusivamente i file bloccati. Se gli utenti tendono per bloccare il numero di file per i periodi di tempo nel file server, è consigliabile utilizzare un metodo preseeding diverso. Preseeding non richiede l'ideale tra gli elenchi di file nel server di origine e di destinazione, ma è più file che non esistono quando viene eseguita la sincronizzazione iniziale per la replica DFS, preseeding meno efficace. Per ridurre i conflitti di blocco, utilizzare Robocopy durante le ore non di punta per l'organizzazione. Esaminare i registri Robocopy sempre dopo preseeding per assicurarsi di avere compreso i file che sono stati ignorati a causa di blocchi esclusivi.

Per utilizzare Robocopy preseed file per la replica DFS, eseguire la procedura seguente:

1. [Scaricare e installare la versione più recente di Robocopy.](#step-1:-download-and-install-the-latest-version-of-robocopy)
2. [Stabilizzarsi i file che verranno replicati.](#step-2:-stabilize-files-that-will-be-replicated)
3. [Copiare i file replicati nel server di destinazione.](#step-3:-copy-the-replicated-files-to-the-destination-server)

## <a name="prerequisites"></a>Prerequisiti

Poiché preseeding non prevede direttamente replica DFS, è sufficiente soddisfare i requisiti per l'esecuzione di una copia del file con Robocopy.

- È necessario un account membro del gruppo Administrators locale nel server di origine e di destinazione.

- Installare la versione più recente di Robocopy nel server che verrà utilizzato per copiare i file, ovvero il server di origine o il server di destinazione. è necessario installare la versione più recente per la versione del sistema operativo. Per ulteriori informazioni, vedere [passaggio 2: stabilizzarsi i file che verranno replicati](#step-2:-stabilize-files-that-will-be-replicated). Se non si preseeding file da un server che esegue Windows Server 2003 R2, è possibile eseguire Robocopy nel server di origine o di destinazione. Il server di destinazione, che in genere presenta la versione più recente del sistema operativo, consente di accedere alla versione più recente di Robocopy.

- Verificare che lo spazio di archiviazione sufficiente sia disponibile in unità di destinazione. Non creare una cartella nel percorso che si desidera copiare: Robocopy necessario creare la cartella radice.
    
    >[!NOTE]
    >Quando si decide la quantità di spazio massima per i file preseeded, prendere in considerazione la crescita prevista dati nel tempo e requisiti di archiviazione per la replica DFS. Per la pianificazione della Guida in linea, vedere [modificare le dimensioni della Quota della cartella di gestione temporanea e conflitti e cartella eliminata](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754229(v=ws.11)) in [Gestione replica DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754771(v=ws.11)>).

- Nel server di origine, installare facoltativamente Process Monitor o Process Explorer, è possibile utilizzare per controllare per le applicazioni che causano il blocco file. Per informazioni sul download, vedere [Processo di monitoraggio](https://docs.microsoft.com/sysinternals/downloads/procmon) e [Process Explorer](https://docs.microsoft.com/sysinternals/downloads/process-explorer).

## <a name="step-1-download-and-install-the-latest-version-of-robocopy"></a>Passaggio 1: Scaricare e installare la versione più recente di Robocopy

Prima di utilizzare Robocopy per preseed file, è necessario scaricare e installare la versione più recente di **Robocopy.exe**. In questo modo, che replica DFS non ignorare il file a causa di problemi all'interno di versioni di spedizione di Robocopy.

L'origine per l'ultima versione Robocopy compatibile dipende dalla versione di Windows Server in cui è in esecuzione nel server. Per informazioni su come scaricare l'hotfix con la versione più recente di Robocopy per Windows Server 2008 R2 o Windows Server 2008, vedere [elenco degli aggiornamenti rapidi attualmente disponibili per le tecnologie di sistema DFS (Distributed File) in Windows Server 2008 e Windows Server 2008 R2](https://support.microsoft.com/help/968429/list-of-currently-available-hotfixes-for-distributed-file-system-dfs-t).

In alternativa, è possibile individuare e installare l'hotfix più recente per un sistema operativo eseguendo la procedura seguente.

### <a name="locate-and-install-the-latest-version-of-robocopy-for-a-specific-version-of-windows-server"></a>Individuare e installare la versione più recente di Robocopy per una versione specifica di Windows Server

1. In un web browser, aprire [https://support.microsoft.com](https://support.microsoft.com/).

2. Nel **Supporto di ricerca**, immettere le informazioni seguenti stringa, sostituendo `<operating system version>` con il sistema operativo appropriato, quindi premere INVIO:
    
    ```robocopy.exe kbqfe "<operating system version>"```
    
    Ad esempio, immettere **robocopy.exe barra "Windows Server 2008 R2"**.

3. Individuare e scaricare l'hotfix con il numero massimo di ID (vale a dire la versione più recente).

4. Installare l'hotfix nel server.

## <a name="step-2-stabilize-files-that-will-be-replicated"></a>Passaggio 2: Stabilizzarsi i file che verranno replicati

Dopo aver installato la versione più recente di Robocopy nel server, è preferibile che i file bloccati dalla copia blocco tramite i metodi descritti nella tabella seguente. La maggior parte delle applicazioni non bloccano esclusivamente i file. Tuttavia, durante le normali operazioni, una piccola parte dei file potrebbe essere bloccata nei file server.

|Origine del blocco|Spiegazione|Mitigazione|
|---|---|---|
|Gli utenti in modalità remota aprire i file in condivisioni.|I dipendenti connettano a un server di file standard e modificare documenti, dei contenuti multimediali o altri file. A volte indicato come la home directory o carichi di lavoro di dati condivisi tradizionale.|Eseguire le operazioni di Robocopy durante solo, non-orari di ufficio. Si riduce al minimo il numero di file Robocopy deve ignorare durante preseeding.<br><br>È consigliabile impostare temporaneamente accesso in sola lettura in condivisioni di file che verranno replicate utilizzando i cmdlet di Windows PowerShell **Grant SmbShareAccess** e **SmbSession Chiudi** . Se si impostano le autorizzazioni per un gruppo comune, ad esempio tutti gli utenti o gli utenti autenticati di lettura, gli utenti standard potrebbero essere meno probabilità di aprire i file con blocchi esclusivi (se le proprie applicazioni rilevare l'accesso in sola lettura quando i file vengono aperti).<br><br>È anche possibile impostare una regola del firewall temporaneo per la porta SMB 445 in ingresso per il server per bloccare l'accesso ai file o utilizzare il cmdlet **SmbShareAccess blocco** . Tuttavia, entrambi questi metodi sono molto eccesso per le operazioni utente.|
|Applicazioni di aprono file locali.|Carichi di lavoro dell'applicazione in esecuzione in un file server talvolta bloccare i file.|Disattivare temporaneamente o disinstallare le applicazioni che causano il blocco file. È possibile utilizzare Process Monitor o Process Explorer per determinare quali applicazioni sono blocco dei file. Per scaricare Process Monitor o Process Explorer, visitare pagine [Process Monitor](https://docs.microsoft.com/sysinternals/downloads/procmon) e [Process Explorer](https://docs.microsoft.com/sysinternals/downloads/process-explorer) .|

## <a name="step-3-copy-the-replicated-files-to-the-destination-server"></a>Passaggio 3: Copiare i file replicati nel server di destinazione

Dopo che ridurre al minimo i blocchi per i file che verranno replicati, è possibile preseed i file dal server di origine per il server di destinazione.

>[!NOTE]
>È possibile eseguire Robocopy nel computer di origine o nel computer di destinazione. La procedura seguente descrive Robocopy in esecuzione nel server di destinazione, che in genere è in esecuzione un sistema operativo più recente, per poter usufruire delle eventuali funzionalità aggiuntive di Robocopy contenenti il sistema operativo più recente.

### <a name="preseed-the-replicated-files-onto-the-destination-server-with-robocopy"></a>Preseed file replicati nel server di destinazione con Robocopy

1. Accedere al server di destinazione con un account membro del gruppo Administrators locale nel server di origine e di destinazione.

2. Apri una finestra del prompt dei comandi con privilegi elevati.

3. Per preseed i file dall'origine di server di destinazione, eseguire il comando seguente, utilizzando il proprio origine, destinazione e percorsi dei file di registro per i valori racchiuso tra parentesi quadre:
    
    ```PowerShell
    robocopy "<source replicated folder path>" "<destination replicated folder path>" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:<log file path> /v
    ```
    
    Questo comando consente di copiare tutto il contenuto della cartella di origine per la cartella di destinazione con i parametri seguenti:
    
    |Parametro|Descrizione|
    |---|---|
    |"< cartella replicata path\ di origine > \"|Specifica la cartella di origine a preseed nel server di destinazione.|
    |"\ < destinazione replicato path\ cartella >"|Specifica il percorso della cartella in cui verrà archiviati i file preseeded.<br><br>La cartella di destinazione non deve già esistere nel server di destinazione. Per ottenere gli hash file corrispondenti, Robocopy necessario creare la cartella principale quando preseeds i file.|
    |. exe /e|Copia le sottodirectory e i file, nonché le sottodirectory vuote.|
    |/b|Copia i file in modalità di Backup.|
    |/ copyal|Copia tutte le informazioni sui file, inclusi dati, gli attributi, indicatori di data / ora, elenco di controllo NTFS accesso (ACL), informazioni relative al proprietario e le informazioni di controllo.|
    |/r:6|Ritenta l'operazione di sei ore quando si verifica un errore.|
    |/w:5|Attende 5 secondi tra tentativi.|
    |MT:64|Copia 64 file contemporaneamente.|
    |/XD DfsrPrivate|Esclude la cartella DfsrPrivate.|
    |/Tee|Stato output viene scritto nella finestra della console, nonché per il file di registro.|
    |/log \ < percorso del file di registro >|Specifica il file di registro da scrivere. Sovrascrive il contenuto del file esistente. (Per aggiungere le voci del file di registro esistente, utilizzare `/log+ <log file path>`.)|
    |/v|Produce un output dettagliato che include file ignorati.|
    
    Ad esempio, il comando seguente consente di replicare file dalla cartella di origine replicato, E:\\RF01, per unità D dati nel server di destinazione:
    
    ```PowerShell
    robocopy.exe "\\srv01\e$\rf01" "d:\rf01" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:c:\temp\preseedsrv02.log
    ```
    
    >[!NOTE]
    >È consigliabile utilizzare i parametri descritti in precedenza quando si utilizza Robocopy per preseed i file per la replica DFS. Tuttavia, è possibile modificare alcuni dei rispettivi valori o aggiungere ulteriori parametri. Ad esempio, può scoprire tramite verifica che la capacità per impostare un valore più alto (numero di thread) per il parametro */MT* sia. Inoltre, se saranno principalmente replicare i file di dimensioni maggiori, è possibile migliorare le prestazioni di copia aggiungendo l'opzione **/j** dei / o senza buffer. Per ulteriori informazioni sui parametri Robocopy, vedere [Robocopy](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy) Command-Line reference.

    >[!WARNING]
    >Per evitare potenziale perdita di dati quando si utilizza Robocopy per preseed i file per la replica DFS, non effettuare le seguenti modifiche ai parametri consigliati:
    >- Non utilizzare il parametro */mir* (che riflette una struttura di directory) o */mov* (che consente di spostare i file e quindi li elimina dall'origine).
    >-  Non rimuovere le opzioni **. exe /e**, **/ b**e **/copyall** .

4. Una volta completata la copia, esaminare il Registro di eventuali errori o file ignorati. Utilizzare Robocopy per copiare i file singolarmente anziché l'intero insieme di file arrestati ignorati. Se i file sono stati ignorati a causa di blocchi esclusivi, provare a copiare singoli file con Robocopy successivamente o accetta che tali file richiederà la replica over cablato da replica DFS durante la sincronizzazione iniziale.

## <a name="next-step"></a>Passaggio successivo

Dopo aver completato la copia iniziale e utilizzare Robocopy per risolvere i problemi associati a tutti i file ignorati possibile, utilizzare il cmdlet **Get-DfsrFileHash** in Windows PowerShell o il comando **Dfsrdiag** per convalidare i file preseeded confrontando hash di file nel server di origine e di destinazione. Per istruzioni dettagliate, vedere [passaggio 2: convalidare Preseeded file per la replica DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495042(v%3dws.11)>).