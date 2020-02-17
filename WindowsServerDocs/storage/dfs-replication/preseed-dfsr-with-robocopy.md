---
title: Usare Robocopy per eseguire il preseeding dei file per Replica DFS
description: Come usare Robocopy.exe per eseguire il preseeding dei file per Replica DFS.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: ea5cd954dde6d4fa8fcaa7874f75cb9588115ab1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402124"
---
# <a name="use-robocopy-to-preseed-files-for-dfs-replication"></a>Usare Robocopy per eseguire il preseeding dei file per Replica DFS

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Questo argomento descrive come usare lo strumento da riga di comando **Robocopy.exe** per eseguire il preseeding dei file quando viene configurata la replica per Replica DFS (nota anche come DFSR o DFS-R) in Windows Server. Eseguendo il preseeding dei file prima di configurare Replica DFS, aggiungere un nuovo partner di replica o sostituire un server, puoi velocizzare la sincronizzazione iniziale e abilitare la clonazione del database di Replica DFS in Windows Server 2012 R2. Il metodo Robocopy è uno dei diversi metodi di preseeding disponibili. Per una panoramica, vedi [Passaggio 1: Eseguire il preseeding dei file per Replica DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495046(v%3dws.11)>).

L'utilità della riga di comando Robocopy (Copia di file efficace) è inclusa in Windows Server e offre una vasta gamma di opzioni, tra cui la copia delle informazioni di sicurezza, il supporto delle API di backup, la registrazione e la possibilità di ripetere l'operazione. Le versioni più recenti includono il supporto di I/O senza buffer e multithreading.

>[!IMPORTANT]
>Robocopy non copia i file bloccati in modo esclusivo. Se gli utenti tendono a bloccare molti file per periodi prolungati nei file server, considera la possibilità di usare un metodo di preseeding diverso. Il preseeding non richiede una corrispondenza perfetta tra gli elenchi di file nel server di origine e in quello di destinazione, ma maggiore è il numero dei file inesistenti durante l'esecuzione della sincronizzazione iniziale per Replica DFS, minore è l'efficacia del preseeding. Per ridurre al minimo i conflitti di blocco, usa Robocopy durante le ore non di punta dell'organizzazione. Esamina sempre i registri di Robocopy dopo il preseeding per essere certo di comprendere quali file sono stati ignorati a causa di blocchi esclusivi.

Per usare Robocopy per il preseeding dei file per Replica DFS, è necessario eseguire queste operazioni:

1. [Scaricare e installare la versione più recente di Robocopy.](#step-1-download-and-install-the-latest-version-of-robocopy)
2. [Stabilizzare i file che verranno replicati.](#step-2-stabilize-files-that-will-be-replicated)
3. [Copiare i file replicati nel server di destinazione.](#step-3-copy-the-replicated-files-to-the-destination-server)

## <a name="prerequisites"></a>Prerequisiti

Poiché il preseeding non interessa direttamente Replica DFS, devi soddisfare solo i requisiti per l'esecuzione di una copia file con Robocopy.

- Devi disporre di un account membro del gruppo Administrators locale sia nel server di origine che in quello di destinazione.

- Installa la versione più recente di Robocopy nel server che userai per copiare i file, ovvero il server di origine o il server di destinazione. Dovrai installare la versione più recente per la versione del sistema operativo. Per le istruzioni, vedi [Passaggio 2: Stabilizzare i file che verranno replicati](#step-2-stabilize-files-that-will-be-replicated). Tranne nei casi in cui intendi eseguire il preseeding dei file da un server che esegue Windows Server 2003 R2, puoi eseguire Robocopy nel server di origine o di destinazione. Il server di destinazione, che in genere dispone della versione più recente del sistema operativo, consente di accedere alla versione più recente di Robocopy.

- Verifica che nell'unità di destinazione sia disponibile spazio di archiviazione sufficiente. Non creare una cartella nel percorso in cui vuoi copiare i file; Robocopy deve creare la cartella radice.
    
    >[!NOTE]
    >Quando decidi la quantità di spazio da allocare per i file di cui è stato eseguito il preseeding, considera l'espansione dei dati prevista nel tempo e i requisiti di archiviazione per Replica DFS. Per informazioni utili per la pianificazione, vedi [Modificare le dimensioni della quota della cartella di gestione temporanea e della cartella dei file eliminati e con conflitti](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754229(v=ws.11)) in [Gestione di Replica DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754771(v=ws.11)>).

- Nel server di origine installa facoltativamente Monitoraggio processi o Esplora processi, che puoi usare per cercare le applicazioni che bloccano i file. Per informazioni sul download, vedi [Monitoraggio processi](https://docs.microsoft.com/sysinternals/downloads/procmon) ed [Esplora processi](https://docs.microsoft.com/sysinternals/downloads/process-explorer).

## <a name="step-1-download-and-install-the-latest-version-of-robocopy"></a>Passaggio 1: Scaricare e installare la versione più recente di Robocopy

Prima di usare Robocopy per il preseeding dei file, devi scaricare e installare la versione più recente di **Robocopy.exe**. Ciò garantisce che Replica DFS non ignori i file a causa di problemi nelle versioni di Robocopy.

Il codice sorgente per la versione compatibile più recente di Robocopy dipende dalla versione di Windows Server in esecuzione nel server. Per informazioni sul download dell'hotfix con la versione più recente di Robocopy per Windows Server 2008 R2 o Windows Server 2008, vedi [Elenco degli aggiornamenti rapidi attualmente disponibili per le tecnologie di File System distribuito (DFS) in Windows Server 2008 e Windows Server 2008 R2](https://support.microsoft.com/help/968429/list-of-currently-available-hotfixes-for-distributed-file-system-dfs-t).

In alternativa, puoi individuare e installare l'hotfix più recente per un sistema operativo seguendo questa procedura.

### <a name="locate-and-install-the-latest-version-of-robocopy-for-a-specific-version-of-windows-server"></a>Individuare e installare la versione più recente di Robocopy per una versione specifica di Windows Server

1. In un Web browser apri [https://support.microsoft.com](https://support.microsoft.com/).

2. In **Cerca supporto** immetti la stringa seguente, sostituendo `<operating system version>` con il sistema operativo appropriato, quindi premi INVIO:
    
    ```robocopy.exe kbqfe "<operating system version>"```
    
    Ad esempio, immetti **robocopy.exe kbqfe "Windows Server 2008 R2"** .

3. Individua e scarica l'hotfix con il numero ID più elevato, ovvero la versione più recente.

4. Installa l'hotfix nel server.

## <a name="step-2-stabilize-files-that-will-be-replicated"></a>Passaggio 2: Stabilizzare i file che verranno replicati

Dopo aver installato la versione più recente di Robocopy nel server, devi impedire che i file bloccati blocchino la copia usando i metodi descritti nella tabella seguente. La maggior parte delle applicazioni non blocca i file in modo esclusivo. Tuttavia, durante le normali operazioni, potrebbe risultare bloccata una piccola percentuale di file nei file server.

|Origine del blocco|Spiegazione|Misura di prevenzione|
|---|---|---|
|Gli utenti aprono in remoto i file nelle condivisioni.|I dipendenti si connettono a un file server standard e modificano documenti, contenuto multimediale o altri file (indicati a volte come carichi di lavoro di dati condivisi o home directory tradizionale).|Esegui le operazioni di Robocopy solo durante gli orari non lavorativi e non di punta. Questo consente di ridurre al minimo il numero di file che Robocopy deve ignorare durante il preseeding.<br><br>Prendi in considerazione la possibilità di impostare temporaneamente l'accesso in sola lettura nelle condivisioni file che verranno replicate tramite i cmdlet **Grant-SmbShareAccess** e **Close-SmbSession** di Windows PowerShell. Se imposti sulla lettura le autorizzazioni per un gruppo comune come Everyone o Authenticated Users, è meno probabile che gli utenti standard aprano i file con blocchi esclusivi (se le loro applicazioni rilevano l'accesso in sola lettura all'apertura dei file).<br><br>Puoi anche considerare l'eventualità di impostare una regola del firewall temporanea per la porta SMB 445 in ingresso verso tale server per bloccare l'accesso ai file oppure usa il cmdlet **Block-SmbShareAccess**. Tuttavia, entrambi questi metodi causano interruzioni significative nell'attività degli utenti.|
|Le applicazioni aprono i file in locale.|I carichi di lavoro delle applicazioni in esecuzione in un file server a volte bloccano i file.|Disabilita o disinstalla temporaneamente le applicazioni che bloccano i file. Per individuare queste applicazioni, puoi usare Monitoraggio processi o Esplora processi. Per scaricare questi strumenti, visita le pagine relative a [Monitoraggio processi](https://docs.microsoft.com/sysinternals/downloads/procmon) ed [Esplora processi](https://docs.microsoft.com/sysinternals/downloads/process-explorer).|

## <a name="step-3-copy-the-replicated-files-to-the-destination-server"></a>Passaggio 3: Copiare i file replicati nel server di destinazione

Dopo aver ridotto al minimo i blocchi sui file che verranno replicati, puoi eseguire il preseeding dei file dal server di origine al server di destinazione.

>[!NOTE]
>Puoi eseguire Robocopy nel computer di origine oppure di destinazione. La procedura riportata di seguito descrive l'esecuzione di Robocopy nel server di destinazione, che in genere esegue un sistema operativo più recente, per sfruttare le eventuali funzionalità aggiuntive di Robocopy che potrebbero essere fornite dal sistema operativo più recente.

### <a name="preseed-the-replicated-files-onto-the-destination-server-with-robocopy"></a>Eseguire il preseeding dei file replicati nel server di destinazione con Robocopy

1. Accedi al server di destinazione con un account membro del gruppo Administrators locale sia nel server di origine che in quello di destinazione.

2. Aprire un prompt dei comandi con privilegi elevati.

3. Per eseguire il preseeding dei file dal server di origine al server di destinazione, esegui il comando seguente, sostituendo i valori tra parentesi con i tuoi percorsi di origine, di destinazione e dei file di log:
    
    ```PowerShell
    robocopy "<source replicated folder path>" "<destination replicated folder path>" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:<log file path> /v
    ```
    
    Questo comando copia tutto il contenuto della cartella di origine nella cartella di destinazione, con i parametri seguenti:
    
    |Parametro|Description|
    |---|---|
    |"\<source replicated folder path\>"|Specifica la cartella di origine di cui eseguire il preeseeding nel server di destinazione.|
    |"\<destination replicated folder path\>"|Specifica il percorso della cartella in cui verranno archiviati i file di cui è stato eseguito il preseeding.<br><br>La cartella di destinazione non deve essere già presente nel server di destinazione. Per ottenere hash file corrispondenti, Robocopy deve creare la cartella radice quando esegue il preseeding dei file.|
    |/e|Copia le sottodirectory e i relativi file, nonché le sottodirectory vuote.|
    |/ b|Copia i file in modalità di backup.|
    |/copyall|Copia tutte le informazioni dei file, inclusi i dati, gli attributi, i timestamp, l'elenco di controllo di accesso NTFS, le informazioni sul proprietario e le informazioni di controllo.|
    |/r:6|Ripete l'operazione sei volte quando si verifica un errore.|
    |/w:5|Attende 5 secondi tra un tentativo e l'altro.|
    |MT:64|Copia 64 file contemporaneamente.|
    |/xd DfsrPrivate|Esclude la cartella DfsrPrivate.|
    |/tee|Scrive l'output dello stato nella finestra della console, oltre che nel file di registro.|
    |/log \<percorso file registro>|Specifica il file di registro per la scrittura. Sovrascrive il contenuto esistente del file. Per aggiungere le voci al file di registro esistente, usa `/log+ <log file path>`.|
    |/v|Produce output dettagliato che include i file ignorati.|
    
    Ad esempio, il comando seguente replica i file dalla cartella replicata di origine, E:\\RF01, all'unità dati D nel server di destinazione:
    
    ```PowerShell
    robocopy.exe "\\srv01\e$\rf01" "d:\rf01" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:c:\temp\preseedsrv02.log
    ```
    
    >[!NOTE]
    >È consigliabile usare i parametri descritti in precedenza quando usi Robocopy per eseguire il preseeding dei file per Replica DFS. Tuttavia, puoi modificare alcuni valori o aggiungere altri parametri. Ad esempio, a seguito del testing potresti scoprire di poter impostare un valore più alto (conteggio dei thread) per il parametro */MT*. Inoltre, se eseguirai principalmente la replica di file di dimensioni maggiori, potresti riuscire a migliorare le prestazioni di copia aggiungendo l'opzione **/j** per l'I/O senza buffer. Per altre informazioni sui parametri di Robocopy, vedi le informazioni di riferimento sulla riga di comando di [Robocopy](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy).

    >[!WARNING]
    >Per evitare potenziali perdite di dati quando usi Robocopy per eseguire il preseeding dei file per Replica DFS, non apportare le modifiche seguenti ai parametri consigliati:
    >- Non usare il parametro */mir* (che genera una copia speculare di un albero di directory) o il parametro */mov* (che sposta i file e quindi li elimina dall'origine).
    >-  Non rimuovere le opzioni **/e**, **/b** e **/copyall**.

4. Al termine della copia, esamina il registro per individuare eventuali errori o file ignorati. Usa Robocopy per copiare singolarmente i file ignorati invece di copiare di nuovo l'intero set di file. Se i file sono stati ignorati a causa di blocchi esclusivi, prova a copiare i singoli file con Robocopy in un secondo momento oppure accetta che tali file debbano essere sottoposti a replica in rete tramite Replica DFS durante la sincronizzazione iniziale.

## <a name="next-step"></a>Passaggio successivo

Dopo aver completato la copia iniziale e aver usato Robocopy per risolvere i problemi relativi al maggior numero possibile di file ignorati, userai il cmdlet **Get-DfsrFileHash** in Windows PowerShell o il comando **Dfsrdiag** per convalidare i file di cui è stato eseguito il preseeding confrontando gli hash file nel server di origine e in quello di destinazione. Per istruzioni dettagliate, vedi [Passaggio 2: Convalidare i file sottoposti a preseeding per Replica DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495042(v%3dws.11)>).
