---
title: Usare Robocopy per eseguito il preseeding file per la replica DFS
description: Come usare Robocopy.exe a eseguito il preseeding file per la replica DFS.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: a7a14b1a1e0f91002b201869e4c68187ffaf3f8f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865082"
---
# <a name="use-robocopy-to-preseed-files-for-dfs-replication"></a>Usare Robocopy per eseguito il preseeding file per la replica DFS

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

In questo argomento illustra come usare lo strumento da riga di comando **Robocopy.exe**per eseguito il preseeding file quando si configura la replica per la replica del File System distribuito (DFS) (noto anche come DFSR o DFS-R) in Windows Server. Da preseeding file prima di impostare la replica DFS, aggiungere un nuovo partner di replica, o sostituire un server, è possibile velocizzare la sincronizzazione iniziale e abilitare la clonazione del database della replica DFS in Windows Server 2012 R2. Il metodo di Robocopy è uno dei diversi metodi preseeding; per una panoramica, vedere [passaggio 1: eseguito il preseeding file per la replica DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495046(v%3dws.11)>).

L'utilità della riga di comando Robocopy (copia File affidabile) è incluso in Windows Server. L'utilità fornisce una vasta scelta di opzioni che include la copia di sicurezza, backup API supporto, le funzionalità di ripetizione dei tentativi e la registrazione. Nelle versioni successive includono il supporto dei / o il multithreading e non memorizzato nel buffer.

>[!IMPORTANT]
>Robocopy non vengono copiate in modo esclusivo i file bloccati. Se gli utenti tendono a bloccare molti file per lunghi periodi nei file server, è consigliabile usare un metodo preseeding diverso. Preseeding non richiede una perfetta corrispondenza tra gli elenchi di file nel server di origine e destinazione, ma è i più file che non esistano durante la sincronizzazione iniziale viene eseguita per replica DFS, il preseeding meno efficaci. Per ridurre al minimo i conflitti di blocco, usare Robocopy durante le ore non di punta per l'organizzazione. Esaminare i log di Robocopy sempre dopo il preseeding per assicurarsi di comprendere quali file sono stati ignorati a causa di blocchi esclusivi.

Usare Robocopy per eseguito il preseeding file per la replica DFS, seguire questa procedura:

1. [Scaricare e installare l'ultima versione di Robocopy.](#step-1:-download-and-install-the-latest-version-of-robocopy)
2. [Stabilizzare i file che verranno replicati.](#step-2:-stabilize-files-that-will-be-replicated)
3. [Copiare i file replicati nel server di destinazione.](#step-3:-copy-the-replicated-files-to-the-destination-server)

## <a name="prerequisites"></a>Prerequisiti

Poiché preseeding non implicano direttamente la replica DFS, è sufficiente soddisfare i requisiti per l'esecuzione di una copia del file con Robocopy.

- È necessario un account membro del gruppo Administrators locale nel server di origine e destinazione.

- Installare la versione più recente di Robocopy nel server che userà per copiare i file, ovvero il server di origine o il server di destinazione. è necessario installare la versione più recente per la versione del sistema operativo. Per istruzioni, vedere [passaggio 2: Stabilizzare i file che verranno replicati](#step-2:-stabilize-files-that-will-be-replicated). A meno che non si sono preseeding file da un server che esegue Windows Server 2003 R2, è possibile eseguire Robocopy nel server di origine o destinazione. Il server di destinazione, in genere con la versione più recente del sistema operativo, consente di accedere alla versione più recente di Robocopy.

- Assicurarsi che sia disponibile nell'unità di destinazione spazio di archiviazione sufficiente. Non creare una cartella nel percorso che si intende copiare: Robocopy è necessario creare la cartella radice.
    
    >[!NOTE]
    >Quando si decide quanto spazio allocare per i file preseeded, prendere in considerazione l'aumento dei dati prevista nel tempo e l'archiviazione requisiti per la replica DFS. Per la pianificazione della Guida, vedere [modificare le dimensioni della Quota della cartella di gestione temporanea e dei conflitti e cartella eliminata](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754229(v=ws.11)) nelle [gestione di replica DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754771(v=ws.11)>).

- Nel server di origine, se lo si desidera installare Process Monitor o Process Explorer, che è possibile usare per controllare per le applicazioni che bloccano i file. Per informazioni sul download, vedere [Process Monitor](https://docs.microsoft.com/sysinternals/downloads/procmon) e [Process Explorer](https://docs.microsoft.com/sysinternals/downloads/process-explorer).

## <a name="step-1-download-and-install-the-latest-version-of-robocopy"></a>Passaggio 1: Scaricare e installare l'ultima versione di Robocopy

Prima di usare Robocopy per eseguito il preseeding file, è necessario scaricare e installare la versione più recente di **Robocopy.exe**. Ciò garantisce che la replica DFS non ignorare i file a causa di problemi all'interno di versioni di spedizione di Robocopy.

L'origine per l'ultima versione di Robocopy compatibile dipende dalla versione di Windows Server in cui è in esecuzione nel server. Per informazioni su come scaricare l'hotfix con la versione più recente di Robocopy per Windows Server 2008 R2 o Windows Server 2008, vedere [elenco di hotfix attualmente disponibili per le tecnologie del File System distribuito (DFS) in Windows Server 2008 e Windows Server 2008 R2](https://support.microsoft.com/help/968429/list-of-currently-available-hotfixes-for-distributed-file-system-dfs-t).

In alternativa, è possibile individuare e installare l'hotfix più recente per un sistema operativo attenendosi alla procedura seguente.

### <a name="locate-and-install-the-latest-version-of-robocopy-for-a-specific-version-of-windows-server"></a>Individuare e installare l'ultima versione di Robocopy per una versione specifica di Windows Server

1. In un web browser, aprire [ https://support.microsoft.com ](https://support.microsoft.com/).

2. In **supporto per la ricerca**, immettere la seguente stringa, sostituendo `<operating system version>` con il sistema operativo appropriato, quindi premere INVIO:
    
    ```robocopy.exe kbqfe "<operating system version>"```
    
    Ad esempio, immettere **robocopy.exe barra "Windows Server 2008 R2"**.

3. Individuare e scaricare l'hotfix con il numero di ID più alto (vale a dire, la versione più recente).

4. Installare l'hotfix sul server.

## <a name="step-2-stabilize-files-that-will-be-replicated"></a>Passaggio 2: Stabilizzare i file che verranno replicati

Dopo aver installato l'ultima versione di Robocopy sul server, è preferibile che i file bloccati di copiare il blocco utilizzando i metodi descritti nella tabella seguente. La maggior parte delle applicazioni non bloccano in modo esclusivo i file. Tuttavia, durante le normali operazioni, una piccola percentuale di file potrebbe essere bloccata nei file server.

|Origine del blocco|Spiegazione|Misura di prevenzione|
|---|---|---|
|Gli utenti in remoto aprire i file nelle condivisioni.|I dipendenti connettono a un server di file standard e modificare documenti, dei contenuti multimediali o altri file. Talvolta definito come la home directory tradizionali o carichi di lavoro dei dati condivisa.|Eseguire solo operazioni di Robocopy durante l'attività meno intensa, quello non lavorativo. Ciò riduce al minimo il numero di file che Robocopy deve ignorare durante il preseeding.<br><br>Si consiglia di impostare temporaneamente l'accesso in lettura per le condivisioni file che verranno replicate usando Windows PowerShell **Concedi SmbShareAccess** e **Close SmbSession** cmdlet. Se si imposta le autorizzazioni per un gruppo comune, ad esempio Everyone o Authenticated Users di lettura, gli utenti standard potrebbero essere meno propenso aprire i file con blocchi esclusivi (se le proprie applicazioni di rilevare l'accesso in sola lettura quando vengono aperti i file).<br><br>È possibile anche impostare una regola del firewall temporaneo per la porta SMB 445 in ingresso al server per bloccare l'accesso ai file oppure usare il **blocco SmbShareAccess** cmdlet. Tuttavia, entrambi questi metodi sono estremamente problematica per le operazioni dell'utente.|
|Le applicazioni aprono i file locali.|I carichi di lavoro dell'applicazione in esecuzione in un file server a volte bloccare i file.|Disabilitare o disinstallare le applicazioni che bloccano i file temporaneamente. È possibile utilizzare Process Explorer o monitoraggio di processo per determinare quali applicazioni bloccano i file. Per scaricare Process Monitor o Process Explorer, visitare il [Process Monitor](https://docs.microsoft.com/sysinternals/downloads/procmon) e [Process Explorer](https://docs.microsoft.com/sysinternals/downloads/process-explorer) pagine.|

## <a name="step-3-copy-the-replicated-files-to-the-destination-server"></a>Passaggio 3: Copiare i file replicati nel server di destinazione

Dopo che ridurre al minimo i blocchi sui file che verranno replicati, è possibile eseguito il preseeding i file dal server di origine al server di destinazione.

>[!NOTE]
>È possibile eseguire Robocopy nel computer di origine o il computer di destinazione. La procedura seguente viene descritto come eseguire Robocopy nel server di destinazione, che in genere è in esecuzione un sistema operativo più recente, per sfruttare i vantaggi di eventuali funzionalità aggiuntive di Robocopy che può fornire il sistema operativo più recente.

### <a name="preseed-the-replicated-files-onto-the-destination-server-with-robocopy"></a>Eseguire il preseeding dei file replicati nel server di destinazione con Robocopy

1. Accedere al server di destinazione con un account membro del gruppo Administrators locale nel server di origine e destinazione.

2. Apri una finestra del prompt dei comandi con privilegi elevati.

3. Per eseguito il preseeding i file dall'origine al server di destinazione, eseguire il comando seguente, sostituendo la propria origine, destinazione e i percorsi di file di log per i valori tra parentesi quadre:
    
    ```PowerShell
    robocopy "<source replicated folder path>" "<destination replicated folder path>" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:<log file path> /v
    ```
    
    Questo comando Copia tutto il contenuto della cartella di origine alla cartella di destinazione, con i parametri seguenti:
    
    |Parametro|Descrizione|
    |---|---|
    |"\<origine replicati percorso della cartella\>"|Specifica la cartella di origine a eseguito il preseeding nel server di destinazione.|
    |"\<destinazione replicata percorso della cartella\>"|Specifica il percorso della cartella in cui verrà archiviati i file preseeded.<br><br>La cartella di destinazione non deve esistere già nel server di destinazione. Per ottenere l'hash dei file corrispondenti, Robocopy deve creare la cartella radice quando lo preseeds i file.|
    |/e|Copia le sottodirectory e i file, nonché le sottodirectory vuote.|
    |/ b|Copia i file in modalità Backup.|
    |/ copyal|Copia tutte le informazioni di file, inclusi dati, attributi, timestamp, l'elenco di controllo di accesso (ACL) alle NTFS, informazioni sul proprietario e le informazioni di controllo.|
    |/r:6|Esegue l'operazione di sei tentativi quando si verifica un errore.|
    |/w:5|Attende 5 secondi tra i tentativi.|
    |MT:64|Copia i 64 file contemporaneamente.|
    |/xd DfsrPrivate|Consente di escludere la cartella DfsrPrivate.|
    |/Tee|Scrive l'output di stato alla finestra della console, nonché nel file di log.|
    |log/\<percorso file registro >|Specifica il file di log da scrivere. Sovrascrive il contenuto del file esistente. (Per aggiungere le voci del file di log esistente, usare `/log+ <log file path>`.)|
    |/v|Produce un output dettagliato che include i file ignorati.|
    
    Ad esempio, il comando seguente esegue la replica i file della cartella di origine replicati, e:\\RF01, nell'unità di dati D nel server di destinazione:
    
    ```PowerShell
    robocopy.exe "\\srv01\e$\rf01" "d:\rf01" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:c:\temp\preseedsrv02.log
    ```
    
    >[!NOTE]
    >È consigliabile usare i parametri descritti in precedenza quando si usa Robocopy per eseguito il preseeding file per la replica DFS. Tuttavia, è possibile modificare alcuni dei valori o aggiungere altri parametri. Ad esempio, si scopre tramite test di avere la capacità di impostare un valore più alto (Conteggio thread) per il */MT* parametro. Inoltre, se è necessario replicare principalmente i file di dimensioni maggiori, sarai in grado di aumentare le prestazioni di copia mediante l'aggiunta di **/j** opzione per i/o non memorizzato nel buffer. Per altre informazioni sui parametri di Robocopy, vedere la [Robocopy](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy) riferimento della riga di comando.

    >[!WARNING]
    >Per evitare potenziali perdite di dati quando si usa Robocopy per eseguito il preseeding file per la replica DFS, non apportare le modifiche seguenti ai parametri consigliati:
    >- Non usare la */mir* parametri (che rispecchia una struttura di directory) o il */mov* parametro (che sposta i file, quindi li elimina dall'origine).
    >-  Non rimuovere il **/e**, **/b**, e **/copyall** opzioni.

4. Dopo aver completato la copia, esaminare il registro per errori o di file ignorati. Usare Robocopy per copiare tutti i file ignorati singolarmente anziché ricopiando l'intero set di file. Se i file sono stati ignorati a causa di blocchi esclusivi, provare a copiare singoli file con Robocopy in un secondo momento oppure accettare che tali file richiede la replica attraverso la rete da replica DFS durante la sincronizzazione iniziale.

## <a name="next-step"></a>Passaggio successivo

Dopo aver completato la copia iniziale e usare Robocopy per risolvere i problemi con tutti i file ignorati possibile, si userà il **Get-DfsrFileHash** cmdlet di Windows PowerShell o il **Dfsrdiag** comando convalidare i file preseeded confrontando gli hash dei file nei server di origine e destinazione. Per istruzioni dettagliate, vedere [passaggio 2: Convalidare i file Preseeded per replica DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495042(v%3dws.11)>).