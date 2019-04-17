---
title: Configurare i file dump di memoria per l'installazione Server Core
description: Informazioni su come configurare i file dump di memoria per l'installazione Server Core di Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: bd22378ec7ce5a1ff4e39546246e6e85ca859c45
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/01/2018
ms.locfileid: "1448089"
---
# <a name="configure-memory-dump-files-for-server-core-installation"></a>Configurare i file dump di memoria per l'installazione Server Core

>Si applica a: Windows Server 2016 (Canale semestrale) e Windows Server 2016

Utilizzare la procedura seguente per configurare un'immagine della memoria per l'installazione Server Core. 

## <a name="step-1-disable-the-automatic-system-page-file-management"></a>Passaggio 1: Disabilitare la gestione dei file di pagina automatico del sistema

Il primo passaggio consiste nel configurare manualmente le opzioni di ripristino e degli errori di sistema. Questa operazione è necessaria per completare i passaggi rimanenti.

Eseguire il comando seguente: 

```
wmic computersystem set AutomaticManagedPagefile=False
```
 
## <a name="step-2-configure-the-destination-path-for-a-memory-dump"></a>Passaggio 2: Configurare il percorso di destinazione per un'immagine della memoria

Non è necessario che il file di pagina nella partizione in cui è installato il sistema operativo. Per inserire i file di paging in un'altra partizione, è necessario creare una nuova voce del Registro di sistema denominata **DedicatedDumpFile**. È possibile definire le dimensioni del file di paging utilizzando la voce del Registro di sistema **DumpFileSize** . Per creare le voci del Registro di sistema DedicatedDumpFile e DumpFileSize, procedere come segue: 

1. Al prompt dei comandi eseguire il comando **regedit** per aprire l'Editor del Registro di sistema.
2. Individuare e selezionare la sottochiave del Registro di sistema seguente: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl.
3. Fare clic su **Modifica > Nuovo > valore stringa**.
4. Denominare il nuovo valore **DedicatedDumpFile**e quindi premere INVIO.
5. Pulsante destro del mouse **DedicatedDumpFile**e quindi fare clic su **Modifica**.
6. In tipo di **dati del valore** **\ < Drive\ >: \ \ \ < Dedicateddumpfile.sys\ >** e quindi fare clic su **OK**.

   >[!NOTE] 
   > Sostituire \ < Drive\ > con un'unità in cui è sufficiente su disco la spaziatura per il file di paging e sostituire \ < Dedicateddumpfile.dmp\ > con il percorso completo del file dedicato.
 
7. Fare clic su **Modifica > Nuovo > valore DWORD**.
8. Digitare **DumpFileSize**e quindi premere INVIO.
9. Pulsante destro del mouse **DumpFileSize**e quindi fare clic su **Modifica**.
10. In **Modifica valore DWORD**, in **Base**, fare clic su **decimale**.
11. **Dati valore**, digitare il valore appropriato e quindi fare clic su **OK**.
   >[!NOTE]
   > Le dimensioni del file dump sono in megabyte (MB).
12. Chiudere l'Editor del Registro di sistema.

Dopo aver identificato la posizione della partizione dell'immagine della memoria, configurare il percorso di destinazione per il file di paging. Per visualizzare il percorso di destinazione corrente per il file di paging, eseguire il comando seguente:

```
wmic RECOVEROS get DebugFilePath
```

La destinazione predefinita dei **DebugFilePath** è systemroot%\memory.dmp. Per modificare il percorso di destinazione corrente, eseguire il comando seguente:

```
wmic RECOVEROS set DebugFilePath = <FilePath>
```

Impostare \ < FilePath\ > nel percorso di destinazione. Ad esempio, il comando seguente imposta il percorso di destinazione dump di memoria per C:\WINDOWS\MEMORY. DMP: 

```
wmic RECOVEROS set DebugFilePath = C:\WINDOWS\MEMORY.DMP
```
 
## <a name="step-3-set-the-type-of-memory-dump"></a>Passaggio 3: Impostare il tipo di immagine della memoria

Determinare il tipo di immagine della memoria per configurare per il server. Per visualizzare il tipo di immagine della memoria corrente, eseguire il comando seguente:

```
wmic RECOVEROS get DebugInfoType
```

Per modificare il tipo di immagine della memoria corrente, eseguire il comando seguente: 

```
wmic RECOVEROS set DebugInfoType = <Value>
```

\ < Value\ > può essere 0, 1, 2 o 3, come definito di seguito.

- 0: disattiva la rimozione di un'immagine della memoria.
- 1: dump completo della memoria. Registra tutto il contenuto della memoria di sistema quando il computer si arresta in modo imprevisto. Un'immagine della memoria completa può contenere dati di processi in esecuzione quando l'immagine della memoria vengono raccolti.
- 2: immagine della memoria kernel (impostazione predefinita). Registra solo la memoria kernel. Ciò consente di velocizzare il processo di registrazione delle informazioni in un file di registro durante l'arresto imprevisto del computer.
- 3: memoria ridotta. Consente di registrare l'insieme minimo di informazioni utili per identificare il motivo per cui arresto anomalo del computer.

## <a name="step-4-configure-the-server-to-restart-automatically-after-generating-a-memory-dump"></a>Passaggio 4: Configurare il server per riavviare automaticamente dopo la generazione di un'immagine della memoria

Per impostazione predefinita, il server si riavvia automaticamente dopo la generazione di un'immagine della memoria. Per visualizzare la configurazione corrente, eseguire il comando seguente:

```
wmic RECOVEROS get AutoReboot
```

Se il valore di **AutoReboot** è impostata su TRUE, il server si riavvierà automaticamente dopo la generazione di un'immagine della memoria. È necessaria alcuna configurazione ed è possibile procedere al passaggio successivo.

Se il valore di **AutoReboot** è FALSE, il server non si riavvierà automaticamente. Eseguire il comando seguente per modificare il valore:

```
wmic RECOVEROS set AutoReboot = true
```
 
## <a name="step-5-configure-the-server-to-overwrite-the-existing-memory-dump-file"></a>Passaggio 5: Configurare il server per sovrascrivere il file di immagine della memoria esistente

Per impostazione predefinita, il server sovrascrive il file dump di memoria esistente, quando viene creata una nuova. Per determinare se i file dump di memoria esistente già configurati per essere sovrascritto, eseguire il comando seguente:

```
wmic RECOVEROS get OverwriteExistingDebugFile
```

Se il valore è 1, il server sovrascriverà il file di immagine di memoria esistente. È necessaria alcuna configurazione ed è possibile procedere al passaggio successivo.

Se il valore è 0, il server non sovrascriva il file di immagine della memoria esistente. Eseguire il comando seguente per modificare il valore: 

```
wmic RECOVEROS set OverwriteExistingDebugFile = 1
```
 
## <a name="step-6-set-an-administrative-alert"></a>Passaggio 6: Impostare un avviso amministrativo

Determinare se un avviso amministrativo è appropriato e impostare **SendAdminAlert** in modo appropriato. Per visualizzare il valore corrente di SendAdminAlert, eseguire il comando seguente:

```
wmic RECOVEROS get SendAdminAlert
```

I valori possibili per SendAdminAlert sono TRUE o FALSE. Per modificare il valore SendAdminAlert esistente su true, eseguire il comando seguente: 

```
wmic RECOVEROS set SendAdminAlert = true
```
 
## <a name="step-7-set-the-memory-dumps-page-file-size"></a>Passaggio 7: Installazione del file di paging dell'immagine della memoria

Per verificare le impostazioni del file di pagina corrente, eseguire uno dei seguenti comandi:

   ```
   wmic.exe pagefile
   ```

   oppure

   ```
   wmic.exe pagefile list /format:list
   ```

Ad esempio, eseguire il comando seguente per configurare le dimensioni massime e iniziali del file di pagina:

```
wmic pagefileset where name="c:\\pagefile.sys" set InitialSize=1000,MaximumSize=5000
```

## <a name="step-8-configure-the-server-to-generate-a-manual-memory-dump"></a>Passaggio 8: Configurare il server per generare un'immagine della memoria manuale

È possibile generare un'immagine della memoria manualmente utilizzando una tastiera PS/2. Questa funzionalità è disabilitata per impostazione predefinita e non è disponibile per tastiere Bus USB (Universal Serial).

Per attivare la memoria manuale dump mediante una tastiera PS/2, eseguita il comando seguente:

```
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\i8042prt\Parameters /v CrashOnCtrlScroll /t REG_DWORD /d 1 /f
```

Per determinare se la caratteristica è stata abilitata correttamente, eseguire il comando seguente:

```
Reg query HKEY_LOCAL_MACHINE \ SYSTEM \ CurrentControlSet \ Services \ i8042prt \ Parameters / v CrashOnCtrlScroll
```

È necessario riavviare il server rendere effettive le modifiche. È possibile riavviare il server eseguendo il comando seguente:

```
Shutdown / r / t 0
```

È possibile generare immagini della memoria manuale con una tastiera PS/2 connesso al server tenendo premuto il tasto CTRL destra tenendo premuto il tasto BLOC SCORR due volte. In questo modo, il computer controllo con il codice di errore 0xE2. Dopo il riavvio del server, viene visualizzato un nuovo file dump nel percorso di destinazione stabilita nel passaggio 2.

## <a name="step-9-verify-that-memory-dump-files-are-being-created-correctly"></a>Passaggio 9: Verificare che immagine della memoria creare correttamente i file

È possibile utilizzare il sistema dumpchk.exe per verificare che i file dump memoria viene creati correttamente. L'utilità dumpchk.exe non è installato con l'opzione di installazione Server Core, in modo che è necessario eseguirlo da un server con l'esperienza Desktop o da Windows 10. È inoltre necessario installare gli strumenti di debug per i prodotti di Windows.  

L'utilità dumpchk.exe consente di trasferire i file di immagine della memoria da installazione Server Core di Windows Server 2008 a un altro computer utilizzando il supporto di propria scelta.

> [!WARNING]
> File di paging possono essere molto grandi, attentamente prendere in considerazione il trasferimento del metodo e le risorse che richiede di metodo.
 

Ulteriori riferimenti

Per informazioni generali sull'utilizzo dei file di immagine della memoria, vedere [Overview of opzioni del file immagine della memoria per Windows](https://support.microsoft.com/help/254649/overview-of-memory-dump-file-options-for-windows).

Per ulteriori informazioni sui file dump dedicati, vedere [come utilizzare il valore del Registro di sistema DedicatedDeumpFile per superare i limiti di spazio su unità di sistema durante l'acquisizione di un'immagine della memoria di sistema](https://blogs.msdn.microsoft.com/ntdebugging/2010/04/02/how-to-use-the-dedicateddumpfile-registry-value-to-overcome-space-limitations-on-the-system-drive-when-capturing-a-system-memory-dump/).



