---
title: Configurare i file di dump della memoria per l'installazione Server Core
description: Informazioni su come configurare i file di dump della memoria per un'installazione Server Core di Windows Server
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: 4f1baa52fc9f0ebfe8afae35d86b7a7238d56223
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383395"
---
# <a name="configure-memory-dump-files-for-server-core-installation"></a>Configurare i file di dump della memoria per l'installazione Server Core

>Si applica a: Windows Server 2019, Windows Server 2016 e Windows Server (canale semestrale)

Usare la procedura seguente per configurare un dump della memoria per l'installazione dei componenti di base del server. 

## <a name="step-1-disable-the-automatic-system-page-file-management"></a>Passaggio 1: disabilitare la gestione automatica dei file di paging di sistema

Il primo passaggio consiste nel configurare manualmente l'errore di sistema e le opzioni di ripristino. Questa operazione è necessaria per completare i passaggi rimanenti.

Eseguire il comando seguente: 

```
wmic computersystem set AutomaticManagedPagefile=False
```
 
## <a name="step-2-configure-the-destination-path-for-a-memory-dump"></a>Passaggio 2: configurare il percorso di destinazione per un dump della memoria

Non è necessario avere il file di paging nella partizione in cui è installato il sistema operativo. Per inserire il file di paging in un'altra partizione, è necessario creare una nuova voce del registro di sistema denominata **DedicatedDumpFile**. È possibile definire le dimensioni del file di paging usando la voce del registro di sistema **DumpFileSize** . Per creare le voci del registro di sistema DedicatedDumpFile e DumpFileSize, attenersi alla procedura seguente: 

1. Al prompt dei comandi, eseguire il comando **Regedit** per aprire l'editor del registro di sistema.
2. Individuare e quindi fare clic sulla sottochiave del registro di sistema seguente: HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\CrashControl
3. Fare clic su **modifica > nuovo valore stringa >** .
4. Assegnare al nuovo valore il nome **DedicatedDumpFile**, quindi premere INVIO.
5. Fare clic con il pulsante destro del mouse su **DedicatedDumpFile**, quindi scegliere **modifica**.
6. In tipo di **dati valore** **\<unità\>:\\\<Dedicateddumpfile. sys\>** , quindi fare clic su **OK**.

   >[!NOTE] 
   > Sostituire \<unità\> con un'unità con spazio su disco sufficiente per il file di paging e sostituire \<DedicatedDumpFile. dmp\> con il percorso completo del file dedicato.
 
7. Fare clic su **modifica > nuovo > valore DWORD**.
8. Digitare **DumpFileSize**, quindi premere INVIO.
9. Fare clic con il pulsante destro del mouse su **DumpFileSize**, quindi scegliere **modifica**.
10. In **Modifica valore DWORD**, in **base**, fare clic su **decimale**.
11. In **dati valore**Digitare il valore appropriato, quindi fare clic su **OK**.
    >[!NOTE]
    > Le dimensioni del file dump sono in megabyte (MB).
12. Uscire dall'editor del registro di sistema.

Dopo aver determinato il percorso della partizione del dump della memoria, configurare il percorso di destinazione per il file di paging. Per visualizzare il percorso di destinazione corrente per il file di paging, eseguire il comando seguente:

```
wmic RECOVEROS get DebugFilePath
```

La destinazione predefinita per **DebugFilePath** è%SystemRoot%\Memory.dmp. Per modificare il percorso di destinazione corrente, eseguire il comando seguente:

```
wmic RECOVEROS set DebugFilePath = <FilePath>
```

Impostare \<FilePath\> sul percorso di destinazione. Ad esempio, il comando seguente imposta il percorso di destinazione del dump della memoria su C:\WINDOWS\MEMORY. DMP 

```
wmic RECOVEROS set DebugFilePath = C:\WINDOWS\MEMORY.DMP
```
 
## <a name="step-3-set-the-type-of-memory-dump"></a>Passaggio 3: impostare il tipo di dump della memoria

Determinare il tipo di dump della memoria da configurare per il server. Per visualizzare il tipo di dump della memoria corrente, eseguire il comando seguente:

```
wmic RECOVEROS get DebugInfoType
```

Per modificare il tipo di dump della memoria corrente, eseguire il comando seguente: 

```
wmic RECOVEROS set DebugInfoType = <Value>
```

\<valore\> può essere 0, 1, 2 o 3, come definito di seguito.

- 0: Disabilita la rimozione di un dump di memoria.
- 1: dump di memoria completo. Registra tutto il contenuto della memoria di sistema quando il computer si arresta in modo imprevisto. Un dump completo della memoria può contenere dati di processi in esecuzione quando è stato raccolto il dump della memoria.
- 2: dump della memoria del kernel (impostazione predefinita). Registra solo la memoria kernel. In questo modo si accelera il processo di registrazione delle informazioni in un file di log quando il computer si arresta in modo imprevisto.
- 3: dump di memoria di piccole dimensioni. Registra il set più piccolo di informazioni utili che possono aiutare a identificare i motivi dell'arresto imprevisto del computer.

## <a name="step-4-configure-the-server-to-restart-automatically-after-generating-a-memory-dump"></a>Passaggio 4: configurare il server per il riavvio automatico dopo la generazione di un dump della memoria

Per impostazione predefinita, il server viene riavviato automaticamente dopo la generazione di un dump della memoria. Per visualizzare la configurazione corrente, eseguire il comando seguente:

```
wmic RECOVEROS get AutoReboot
```

Se il valore di **autoriavvio** è true, il server verrà riavviato automaticamente dopo la generazione di un dump della memoria. Non è necessaria alcuna configurazione ed è possibile procedere al passaggio successivo.

Se il valore di **autoriavvio** è false, il server non verrà riavviato automaticamente. Eseguire il comando seguente per modificare il valore:

```
wmic RECOVEROS set AutoReboot = true
```
 
## <a name="step-5-configure-the-server-to-overwrite-the-existing-memory-dump-file"></a>Passaggio 5: configurare il server per sovrascrivere il file di dump della memoria esistente

Per impostazione predefinita, il Server sovrascrive il file di dump della memoria esistente quando ne viene creato uno nuovo. Per determinare se i file di dump della memoria esistenti sono già configurati per la sovrascrittura, eseguire il comando seguente:

```
wmic RECOVEROS get OverwriteExistingDebugFile
```

Se il valore è 1, il server sovrascriverà il file di dump della memoria esistente. Non è necessaria alcuna configurazione ed è possibile procedere al passaggio successivo.

Se il valore è 0, il server non sovrascriverà il file di dump della memoria esistente. Eseguire il comando seguente per modificare il valore: 

```
wmic RECOVEROS set OverwriteExistingDebugFile = 1
```
 
## <a name="step-6-set-an-administrative-alert"></a>Passaggio 6: impostare un avviso amministrativo

Determinare se un avviso amministrativo è appropriato e impostare **SendAdminAlert** di conseguenza. Per visualizzare il valore corrente di SendAdminAlert, eseguire il comando seguente:

```
wmic RECOVEROS get SendAdminAlert
```

I valori possibili per SendAdminAlert sono TRUE o FALSE. Per modificare il valore di SendAdminAlert esistente in true, eseguire il comando seguente: 

```
wmic RECOVEROS set SendAdminAlert = true
```
 
## <a name="step-7-set-the-memory-dumps-page-file-size"></a>Passaggio 7: impostare le dimensioni del file di paging del dump di memoria

Per verificare le impostazioni del file di paging corrente, eseguire uno dei comandi seguenti:

   ```
   wmic.exe pagefile
   ```

   oppure

   ```
   wmic.exe pagefile list /format:list
   ```

Ad esempio, eseguire il comando seguente per configurare le dimensioni iniziali e massime del file di paging:

```
wmic pagefileset where name="c:\\pagefile.sys" set InitialSize=1000,MaximumSize=5000
```

## <a name="step-8-configure-the-server-to-generate-a-manual-memory-dump"></a>Passaggio 8: configurare il server per generare un dump di memoria manuale

È possibile generare manualmente un dump della memoria utilizzando una tastiera PS/2. Questa funzionalità è disabilitata per impostazione predefinita e non è disponibile per le tastiere USB (Universal Serial Bus).

Per abilitare i dump manuali della memoria utilizzando una tastiera PS/2, eseguire il comando seguente:

```
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\i8042prt\Parameters /v CrashOnCtrlScroll /t REG_DWORD /d 1 /f
```

Per determinare se la funzionalità è stata abilitata correttamente, eseguire il comando seguente:

```
Reg query HKEY_LOCAL_MACHINE \ SYSTEM \ CurrentControlSet \ Services \ i8042prt \ Parameters / v CrashOnCtrlScroll
```

Per rendere effettive le modifiche, è necessario riavviare il server. È possibile riavviare il server eseguendo il comando seguente:

```
Shutdown / r / t 0
```

È possibile generare dump di memoria manuali con una tastiera PS/2 connessa al server tenendo premuto il tasto CTRL mentre si preme il tasto BLOC SCORR due volte. Questo consente di verificare il bug del computer con il codice di errore 0xE2. Dopo aver riavviato il server, viene visualizzato un nuovo file di dump nel percorso di destinazione definito nel passaggio 2.

## <a name="step-9-verify-that-memory-dump-files-are-being-created-correctly"></a>Passaggio 9: verificare che i file di dump della memoria siano stati creati correttamente

È possibile utilizzare Dumpchk. exe utlity per verificare che i file di dump della memoria vengano creati correttamente. L'utilità Dumpchk. exe non viene installata con l'opzione di installazione dei componenti di base del server, pertanto sarà necessario eseguirla da un server con esperienza desktop o da Windows 10. Inoltre, è necessario installare gli strumenti di debug per i prodotti Windows.  

L'utilità Dumpchk. exe consente di trasferire il file di dump della memoria dall'installazione dei componenti di base del server di Windows Server 2008 all'altro computer utilizzando il supporto di propria scelta.

> [!WARNING]
> I file di paging possono avere dimensioni molto elevate, quindi considerare attentamente il metodo di trasferimento e le risorse richieste dal metodo.
 

Altri riferimenti

Per informazioni generali sull'uso dei file di dump della memoria, vedere [Panoramica delle opzioni del file di dump della memoria per Windows](https://support.microsoft.com/help/254649/overview-of-memory-dump-file-options-for-windows).

Per altre informazioni sui file di dump dedicati, vedere [come usare il valore del registro di sistema DedicatedDeumpFile per superare i limiti di spazio nell'unità di sistema durante l'acquisizione di un'immagine della memoria di sistema](https://blogs.msdn.microsoft.com/ntdebugging/2010/04/02/how-to-use-the-dedicateddumpfile-registry-value-to-overcome-space-limitations-on-the-system-drive-when-capturing-a-system-memory-dump/).



