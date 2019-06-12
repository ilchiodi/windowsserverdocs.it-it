---
title: Configurare i file di dump di memoria per l'installazione Server Core
description: Informazioni su come configurare i file di dump di memoria per un'installazione Server Core di Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: 235df6f681de51a12f82b9fad019dd2db45fd486
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435554"
---
# <a name="configure-memory-dump-files-for-server-core-installation"></a>Configurare i file di dump di memoria per l'installazione Server Core

>Si applica a: Windows Server (canale semestrale) e Windows Server 2016

Utilizzare la procedura seguente per configurare un dump di memoria per l'installazione Server Core. 

## <a name="step-1-disable-the-automatic-system-page-file-management"></a>Passaggio 1: Disabilitare la gestione di file pagina automatici del sistema

Il primo passaggio consiste nel configurare manualmente le opzioni di ripristino di sistema. Ciò è necessario per completare i passaggi rimanenti.

Eseguire il comando seguente: 

```
wmic computersystem set AutomaticManagedPagefile=False
```
 
## <a name="step-2-configure-the-destination-path-for-a-memory-dump"></a>Passaggio 2: Configurare il percorso di destinazione per un dump di memoria

Non è necessario avere il file di paging nella partizione in cui è installato il sistema operativo. Per inserire il file di paging in un'altra partizione, è necessario creare una nuova voce del Registro di sistema denominata **DedicatedDumpFile**. È possibile definire le dimensioni del file di paging utilizzando il **DumpFileSize** voce del Registro di sistema. Per creare le voci del Registro di sistema DedicatedDumpFile e DumpFileSize, seguire questa procedura: 

1. Al prompt dei comandi, eseguire la **regedit** comando per aprire l'Editor del Registro di sistema.
2. Individuare e fare clic sulla seguente sottochiave del Registro di sistema: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl
3. Fare clic su **Modifica > Nuovo > valore stringa**.
4. Denominare il nuovo valore **DedicatedDumpFile**, quindi premere INVIO.
5. Fare doppio clic su **DedicatedDumpFile**, quindi fare clic su **Modify**.
6. Nelle **dati valore** tipo  **\<Drive\>:\\\<Dedicateddumpfile.sys\>** , quindi fare clic su **OK**.

   >[!NOTE] 
   > Sostituire \<unità\> con un'unità con sufficiente disco spazio per il file di paging, quindi sostituire \<Dedicateddumpfile.dmp\> con il percorso completo del file dedicato.
 
7. Fare clic su **Modifica > Nuovo > valore DWORD**.
8. Tipo di **DumpFileSize**, quindi premere INVIO.
9. Fare doppio clic su **DumpFileSize**, quindi fare clic su **Modify**.
10. Nelle **Modifica valore DWORD**, in **Base**, fare clic su **Decimal**.
11. Nelle **dati valore**, digitare il valore appropriato e quindi fare clic su **OK**.
    >[!NOTE]
    > La dimensione del file di dump è espressa in megabyte (MB).
12. Chiudere l'Editor del Registro di sistema.

Dopo aver individuato la posizione della partizione del dump della memoria, configurare il percorso di destinazione per il file di paging. Per visualizzare il percorso di destinazione corrente per il file di paging, eseguire il comando seguente:

```
wmic RECOVEROS get DebugFilePath
```

La destinazione predefinita per **DebugFilePath** è systemroot%\memory.dmp. Per modificare il percorso di destinazione corrente, eseguire il comando seguente:

```
wmic RECOVEROS set DebugFilePath = <FilePath>
```

Impostare \<FilePath\> nel percorso di destinazione. Ad esempio, il comando seguente imposta il percorso di destinazione del dump della memoria per C:\WINDOWS\MEMORY. DMP: 

```
wmic RECOVEROS set DebugFilePath = C:\WINDOWS\MEMORY.DMP
```
 
## <a name="step-3-set-the-type-of-memory-dump"></a>Passaggio 3: Impostare il tipo di dump di memoria

Determinare il tipo di dump di memoria da configurare per il server. Per visualizzare il tipo di dump di memoria corrente, eseguire il comando seguente:

```
wmic RECOVEROS get DebugInfoType
```

Per modificare il tipo di dump di memoria corrente, eseguire il comando seguente: 

```
wmic RECOVEROS set DebugInfoType = <Value>
```

\<Valore\> può essere 0, 1, 2 o 3, come definita di seguito.

- 0: Disabilitare la rimozione di un dump di memoria.
- 1: Dump di memoria completo. Registra tutto il contenuto della memoria di sistema quando il computer si arresta in modo imprevisto. Un dump di memoria completa può contenere i dati dai processi che erano in esecuzione quando è stato recuperato il dump di memoria.
- 2: Immagine della memoria kernel (impostazione predefinita). Registra solo la memoria kernel. Ciò consente di velocizzare il processo di registrazione delle informazioni in un file di log quando il computer si arresta in modo imprevisto.
- 3: Dump di memoria di piccole dimensioni. Registra il set più piccolo di informazioni utili che possono aiutare a identificare il motivo dell'arresto imprevisto del computer.

## <a name="step-4-configure-the-server-to-restart-automatically-after-generating-a-memory-dump"></a>Passaggio 4: Configurare il server venga riavviato automaticamente dopo aver generato un dump di memoria

Per impostazione predefinita, il server viene riavviato automaticamente dopo la generazione di un dump di memoria. Per visualizzare la configurazione corrente, eseguire il comando seguente:

```
wmic RECOVEROS get AutoReboot
```

Se il valore per **AutoReboot** è TRUE, il server verrà riavviato automaticamente dopo aver generato un dump di memoria. È necessaria alcuna configurazione ed è possibile procedere al passaggio successivo.

Se il valore per **AutoReboot** è FALSE, il server non verrà riavviato automaticamente. Eseguire il comando seguente per modificare il valore:

```
wmic RECOVEROS set AutoReboot = true
```
 
## <a name="step-5-configure-the-server-to-overwrite-the-existing-memory-dump-file"></a>Passaggio 5: Configurare il server per sovrascrivere il file di dump della memoria esistente

Per impostazione predefinita, il server sovrascrive il file di dump della memoria esistente quando ne viene creata una nuova. Per determinare se i file di dump di memoria esistenti sono già configurati verranno sovrascritti, eseguire il comando seguente:

```
wmic RECOVEROS get OverwriteExistingDebugFile
```

Se il valore è 1, il server sovrascriverà il file di dump della memoria esistente. È necessaria alcuna configurazione, ed è possibile procedere al passaggio successivo.

Se il valore è 0, il server non sovrascrive il file di dump della memoria esistente. Eseguire il comando seguente per modificare il valore: 

```
wmic RECOVEROS set OverwriteExistingDebugFile = 1
```
 
## <a name="step-6-set-an-administrative-alert"></a>Passaggio 6: Impostare un avviso amministrativo

Determinare se un avviso amministrativo è appropriato e impostare **SendAdminAlert** conseguenza. Per visualizzare il valore corrente di SendAdminAlert, eseguire il comando seguente:

```
wmic RECOVEROS get SendAdminAlert
```

I valori possibili per SendAdminAlert sono TRUE o FALSE. Per modificare il valore SendAdminAlert esistente su true, eseguire il comando seguente: 

```
wmic RECOVEROS set SendAdminAlert = true
```
 
## <a name="step-7-set-the-memory-dumps-page-file-size"></a>Passaggio 7: Impostare dimensioni file di paging del dump della memoria

Per controllare le impostazioni del file di pagina corrente, eseguire uno dei seguenti comandi:

   ```
   wmic.exe pagefile
   ```

   oppure

   ```
   wmic.exe pagefile list /format:list
   ```

Ad esempio, eseguire il comando seguente per configurare le dimensioni massime e iniziali del file di paging:

```
wmic pagefileset where name="c:\\pagefile.sys" set InitialSize=1000,MaximumSize=5000
```

## <a name="step-8-configure-the-server-to-generate-a-manual-memory-dump"></a>Passaggio 8: Configurare il server per generare un dump di memoria manuale

È possibile generare un dump di memoria manualmente mediante una tastiera PS/2. Questa funzionalità è disabilitata per impostazione predefinita e non è disponibile per le tastiere Universal Serial Bus (USB).

Per abilitare la memoria manuale esegue il dump con una tastiera PS/2, eseguire il comando seguente:

```
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\i8042prt\Parameters /v CrashOnCtrlScroll /t REG_DWORD /d 1 /f
```

Per determinare se la funzionalità è stata abilitata correttamente, eseguire il comando seguente:

```
Reg query HKEY_LOCAL_MACHINE \ SYSTEM \ CurrentControlSet \ Services \ i8042prt \ Parameters / v CrashOnCtrlScroll
```

È necessario riavviare il server rendere effettive le modifiche. È possibile riavviare il server eseguendo il comando seguente:

```
Shutdown / r / t 0
```

È possibile generare i dump di memoria manuale con una tastiera PS/2 che è connesso al server, tenendo premuto il tasto CTRL destro mentre si preme il tasto BLOC SCORR due volte. In questo modo il computer verifica con codice di errore 0xE2 di bug. Dopo aver riavviato il server, viene visualizzato un nuovo file di dump nel percorso di destinazione che è stato stabilito nel passaggio 2.

## <a name="step-9-verify-that-memory-dump-files-are-being-created-correctly"></a>Passaggio 9: Verificare che dump della memoria vengono creati i file in modo corretto

È possibile usare il sistema dumpchk.exe per verificare che i file di dump di memoria siano stati creati correttamente. L'utilità dumpchk.exe non è installato con l'opzione di installazione Server Core, pertanto sarà necessario per l'esecuzione da un server con esperienza Desktop o Windows 10. Inoltre, gli strumenti di debug per i prodotti Windows devono essere installati.  

L'utilità dumpchk.exe consente di trasferire il file di dump di memoria dall'installazione Server Core di Windows Server 2008 a un altro computer usando il supporto di propria scelta.

> [!WARNING]
> I file di paging possono essere molto grandi, quindi è consigliabile prendere in considerazione il trasferimento metodo e le risorse che richiede di metodo.
 

Altri riferimenti

Per informazioni generali sull'uso di file di dump di memoria, vedere [opzioni di panoramica del file di dump della memoria per Windows](https://support.microsoft.com/help/254649/overview-of-memory-dump-file-options-for-windows).

Per altre informazioni sui file di dump dedicata, vedere [come usare il valore del Registro di sistema DedicatedDeumpFile per superare le limitazioni di spazio nell'unità di sistema durante l'acquisizione di un dump della memoria di sistema](https://blogs.msdn.microsoft.com/ntdebugging/2010/04/02/how-to-use-the-dedicateddumpfile-registry-value-to-overcome-space-limitations-on-the-system-drive-when-capturing-a-system-memory-dump/).



