---
title: Usare Robocopy per eseguire il Preseeding dei file per Replica DFS
description: Come usare Robocopy. exe per eseguire il Preseeding dei file per Replica DFS.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 4a0cad3c685c8609784c7096fe31d55294712c2e
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871985"
---
# <a name="use-robocopy-to-preseed-files-for-dfs-replication"></a>Usare Robocopy per eseguire il Preseeding dei file per Replica DFS

>Si applica a Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

In questo argomento viene illustrato come utilizzare lo strumento da riga di comando **Robocopy. exe**per eseguire il Preseeding dei file quando si configura la replica per la replica file System distribuito (DFS) (nota anche come DFSR o DFS-R) in Windows Server. Eseguendo il Preseeding dei file prima di configurare Replica DFS, aggiungere un nuovo partner di replica o sostituire un server è possibile velocizzare la sincronizzazione iniziale e abilitare la clonazione del database di Replica DFS in Windows Server 2012 R2. Il metodo Robocopy è uno dei diversi metodi di Preseeding; per una panoramica, vedere [passaggio 1: Preseeding dei file per replica DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495046(v%3dws.11)>).

L'utilità da riga di comando Robocopy (Solid file copy) è inclusa in Windows Server. L'utilità fornisce opzioni estese che includono la copia della sicurezza, il supporto delle API di backup, le funzionalità di ripetizione dei tentativi e la registrazione. Le versioni successive includono il multithreading e il supporto di I/O non memorizzato nel buffer.

>[!IMPORTANT]
>Robocopy non copia i file bloccati in modo esclusivo. Se gli utenti tendono a bloccare molti file per periodi prolungati nei file server, è consigliabile usare un metodo di Preseeding diverso. Il Preseeding non richiede una corrispondenza perfetta tra gli elenchi di file nel server di origine e di destinazione, ma più file che non esistono quando viene eseguita la sincronizzazione iniziale per Replica DFS, il Preseeding meno efficace è. Per ridurre al minimo i conflitti di blocco, utilizzare Robocopy durante le ore non di punta per l'organizzazione. Esaminare sempre i log Robocopy dopo il Preseeding per assicurarsi di comprendere quali file sono stati ignorati a causa di blocchi esclusivi.

Per usare Robocopy per il Preseeding dei file per Replica DFS, seguire questa procedura:

1. [Scaricare e installare la versione più recente di Robocopy.](#step-1-download-and-install-the-latest-version-of-robocopy)
2. [Stabilizzare i file che verranno replicati.](#step-2-stabilize-files-that-will-be-replicated)
3. [Copiare i file replicati nel server di destinazione.](#step-3-copy-the-replicated-files-to-the-destination-server)

## <a name="prerequisites"></a>Prerequisiti

Poiché il Preseeding non implica direttamente Replica DFS, è necessario soddisfare solo i requisiti per l'esecuzione di una copia di file con Robocopy.

- È necessario un account membro del gruppo Administrators locale nel server di origine e in quello di destinazione.

- Installare la versione più recente di Robocopy nel server che si utilizzerà per copiare i file, ovvero il server di origine o il server di destinazione. sarà necessario installare la versione più recente per la versione del sistema operativo. Per istruzioni, vedere [passaggio 2: Stabilizzare i file che verranno replicati](#step-2-stabilize-files-that-will-be-replicated). A meno che non si esegua il Preseeding dei file da un server che esegue Windows Server 2003 R2, è possibile eseguire Robocopy nel server di origine o di destinazione. Il server di destinazione, che in genere presenta la versione più recente del sistema operativo, consente di accedere alla versione più recente di Robocopy.

- Verificare che nell'unità di destinazione sia disponibile spazio di archiviazione sufficiente. Non creare una cartella nel percorso in cui si intende eseguire la copia: Robocopy deve creare la cartella radice.
    
    >[!NOTE]
    >Quando si decide la quantità di spazio da allocare per i file di cui è stato effettuato il seeding, considerare la crescita dei dati prevista nel tempo e nei requisiti di archiviazione per Replica DFS. Per informazioni sulla pianificazione, vedere [modificare le dimensioni della quota della cartella di gestione temporanea e della cartella dei file eliminati e con conflitti](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754229(v=ws.11)) nella [gestione di replica DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754771(v=ws.11)>).

- Nel server di origine, facoltativamente, installare Process Monitor o Process Explorer, che è possibile utilizzare per verificare la presenza di applicazioni che bloccano i file. Per informazioni sul download, vedere [Process Monitor](https://docs.microsoft.com/sysinternals/downloads/procmon) ed [Esplora processi](https://docs.microsoft.com/sysinternals/downloads/process-explorer).

## <a name="step-1-download-and-install-the-latest-version-of-robocopy"></a>Passaggio 1: Scaricare e installare la versione più recente di Robocopy

Prima di usare Robocopy per il Preseeding dei file, è necessario scaricare e installare la versione più recente di **Robocopy. exe**. Ciò garantisce che Replica DFS non ignori i file a causa di problemi nelle versioni di spedizione di Robocopy.

Il codice sorgente per la versione più recente di Robocopy dipende dalla versione di Windows Server in esecuzione nel server. Per informazioni sul download dell'hotfix con la versione più recente di Robocopy per Windows Server 2008 R2 o Windows Server 2008, vedere l' [elenco degli hotfix attualmente disponibili per le tecnologie file System distribuito (DFS) in Windows server 2008 e in Windows Server 2008 R2](https://support.microsoft.com/help/968429/list-of-currently-available-hotfixes-for-distributed-file-system-dfs-t).

In alternativa, è possibile individuare e installare l'hotfix più recente per un sistema operativo seguendo questa procedura.

### <a name="locate-and-install-the-latest-version-of-robocopy-for-a-specific-version-of-windows-server"></a>Individuare e installare la versione più recente di Robocopy per una versione specifica di Windows Server

1. In un Web browser aprire [https://support.microsoft.com](https://support.microsoft.com/).

2. In **supporto ricerca**immettere la stringa seguente, sostituendo `<operating system version>` con il sistema operativo appropriato, quindi premere il tasto invio:
    
    ```robocopy.exe kbqfe "<operating system version>"```
    
    Ad esempio, immettere **Robocopy. exe kbqfe "Windows Server 2008 R2"** .

3. Individuare e scaricare l'hotfix con il numero ID più elevato, ovvero la versione più recente.

4. Installare l'hotfix nel server.

## <a name="step-2-stabilize-files-that-will-be-replicated"></a>Passaggio 2: Stabilizzare i file che verranno replicati

Dopo aver installato la versione più recente di Robocopy nel server, è necessario impedire che i file bloccati blocchino la copia usando i metodi descritti nella tabella seguente. La maggior parte delle applicazioni non blocca esclusivamente i file. Tuttavia, durante le normali operazioni, potrebbe essere bloccata una piccola percentuale di file nei file server.

|Origine del blocco|Spiegazione|Misura di prevenzione|
|---|---|---|
|Gli utenti aprono in modalità remota i file nelle condivisioni.|I dipendenti si connettono a una file server standard e modificano documenti, contenuto multimediale o altri file. Talvolta definito come la cartella principale tradizionale o i carichi di lavoro di dati condivisi.|Eseguire solo le operazioni di Robocopy durante gli orari di minore attività e non lavorative. Questo consente di ridurre al minimo il numero di file che Robocopy deve ignorare durante il Preseeding.<br><br>Prendere in considerazione l'impostazione temporanea dell'accesso in sola lettura nelle condivisioni file che verranno replicate usando i cmdlet **Grant-SmbShareAccess** e **Close-SmbSession** di Windows PowerShell. Se si impostano le autorizzazioni per un gruppo comune, ad esempio Everyone o Authenticated Users, è meno probabile che gli utenti standard aprano i file con blocchi esclusivi (se le applicazioni rilevano l'accesso in sola lettura quando i file vengono aperti).<br><br>È anche possibile impostare una regola del firewall temporanea per la porta SMB 445 in ingresso a tale server per bloccare l'accesso ai file o usare il cmdlet **Block-SmbShareAccess** . Tuttavia, entrambi questi metodi sono molto problematici per le operazioni dell'utente.|
|Le applicazioni aprono i file locali.|I carichi di lavoro delle applicazioni in esecuzione in un file server talvolta bloccano i file.|Disabilitare o disinstallare temporaneamente le applicazioni che bloccano i file. È possibile utilizzare Process Monitor o Process Explorer per individuare le applicazioni che bloccano i file. Per scaricare Process Monitor o Process Explorer, visitare le pagine [Process Monitor](https://docs.microsoft.com/sysinternals/downloads/procmon) ed [Process Explorer](https://docs.microsoft.com/sysinternals/downloads/process-explorer) .|

## <a name="step-3-copy-the-replicated-files-to-the-destination-server"></a>Passaggio 3: Copiare i file replicati nel server di destinazione

Dopo aver ridotto al minimo i blocchi sui file che verranno replicati, è possibile eseguire il Preseeding dei file dal server di origine al server di destinazione.

>[!NOTE]
>È possibile eseguire Robocopy sia nel computer di origine che nel computer di destinazione. Nella procedura riportata di seguito viene descritto l'esecuzione di Robocopy nel server di destinazione, che in genere esegue un sistema operativo più recente, per sfruttare tutte le funzionalità di Robocopy aggiuntive che potrebbero essere fornite dal sistema operativo più recente.

### <a name="preseed-the-replicated-files-onto-the-destination-server-with-robocopy"></a>Eseguire il Preseeding dei file replicati nel server di destinazione con Robocopy

1. Accedere al server di destinazione con un account membro del gruppo Administrators locale nel server di origine e in quello di destinazione.

2. Apri una finestra del prompt dei comandi con privilegi elevati.

3. Per eseguire il Preseeding dei file dal server di origine al server di destinazione, eseguire il comando seguente, sostituendo i percorsi di origine, di destinazione e dei file di log per i valori tra parentesi:
    
    ```PowerShell
    robocopy "<source replicated folder path>" "<destination replicated folder path>" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:<log file path> /v
    ```
    
    Questo comando copia tutto il contenuto della cartella di origine nella cartella di destinazione, con i parametri seguenti:
    
    |Parametro|Descrizione|
    |---|---|
    |"\<percorso\>cartella replicata di origine"|Specifica la cartella di origine da preinizializzare nel server di destinazione.|
    |"\<percorso\>cartella replicata di destinazione"|Consente di specificare il percorso della cartella in cui archiviare i file di cui è stato utilizzato il seeding.<br><br>La cartella di destinazione non deve esistere già nel server di destinazione. Per ottenere gli hash di file corrispondenti, Robocopy deve creare la cartella radice quando esegue il Preseeding dei file.|
    |/e|Copia le sottodirectory e i relativi file, nonché le sottodirectory vuote.|
    |/ b|Copia i file in modalità di backup.|
    |/copyall|Copia tutte le informazioni sui file, inclusi i dati, gli attributi, i timestamp, l'elenco di controllo di accesso (ACL) NTFS, le informazioni sul proprietario e le informazioni di controllo.|
    |/r: 6|Ripete l'operazione sei volte quando si verifica un errore.|
    |/w: 5|Attende 5 secondi tra i tentativi.|
    |MT: 64|Copia i file 64 simultaneamente.|
    |DfsrPrivate/xD|Esclude la cartella DfsrPrivate.|
    |/tee|Scrive l'output dello stato nella finestra della console, oltre che nel file di log.|
    |> \<percorso file di log/log|Specifica il file di log da scrivere. Sovrascrive il contenuto esistente del file. Per aggiungere le voci al file di log esistente, utilizzare `/log+ <log file path>`.|
    |/v|Produce output dettagliato che include i file ignorati.|
    
    Il comando seguente, ad esempio, consente di replicare i file dalla cartella replicata di\\origine, E: RF01, all'unità dati D nel server di destinazione:
    
    ```PowerShell
    robocopy.exe "\\srv01\e$\rf01" "d:\rf01" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:c:\temp\preseedsrv02.log
    ```
    
    >[!NOTE]
    >Si consiglia di usare i parametri descritti in precedenza quando si usa Robocopy per eseguire il Preseeding dei file per Replica DFS. Tuttavia, è possibile modificare alcuni dei valori o aggiungere altri parametri. Ad esempio, è possibile verificare che sia possibile impostare un valore superiore (conteggio dei thread) per il parametro */mt* . Inoltre, se si replicano principalmente file di dimensioni maggiori, potrebbe essere possibile aumentare le prestazioni di copia aggiungendo l'opzione **/j** per i/O senza buffer. Per ulteriori informazioni sui parametri di Robocopy, vedere la Guida di riferimento alla riga di comando [Robocopy](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy) .

    >[!WARNING]
    >Per evitare potenziali perdite di dati quando si usa Robocopy per eseguire il Preseeding dei file per Replica DFS, non apportare le modifiche seguenti ai parametri consigliati:
    >- Non usare il parametro */Mir* (che rispecchia un albero di directory) o il parametro */MOV* (che sposta i file, quindi li elimina dall'origine).
    >-  Non rimuovere le opzioni **/e**, **/b**e **/CopyAll** .

4. Al termine della copia, esaminare il log per individuare eventuali errori o file ignorati. Usare Robocopy per copiare i file ignorati singolarmente anziché ricopiare l'intero set di file. Se i file sono stati ignorati a causa di blocchi esclusivi, provare a copiare i singoli file con Robocopy in un secondo momento o accettare che i file necessiteranno della replica in transito per Replica DFS durante la sincronizzazione iniziale.

## <a name="next-step"></a>Passaggio successivo

Dopo aver completato la copia iniziale e aver usato Robocopy per risolvere i problemi con il numero di file ignorati possibile, usare il cmdlet **Get-DfsrFileHash** in Windows PowerShell o il comando **Dfsrdiag** per convalidare i file di cui è stato effettuato il seeding tramite il confronto hash di file nel server di origine e in quello di destinazione. Per istruzioni dettagliate, vedere [Step 2: Convalidare i file sottoposte a seeding per Replica DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495042(v%3dws.11)>).
