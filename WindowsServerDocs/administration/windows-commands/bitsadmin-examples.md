---
title: Esempi di bitsadmin
description: Esempi che illustrano come usare lo strumento Bitsadmin per eseguire le attività più comuni.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cb8f8374-ba6e-4a68-85a1-9a95b8215354
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/31/2018
ms.openlocfilehash: 1db9dd387d7b9cc39c582ce79e5163c83579b613
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83819631"
---
# <a name="bitsadmin-examples"></a>Esempi di bitsadmin

Negli esempi seguenti viene illustrato come utilizzare lo `bitsadmin` strumento per eseguire le attività più comuni.

## <a name="transfer-a-file"></a>Trasferire un file

Per creare un processo, aggiungere i file, attivare il processo nella coda di trasferimento e completare il processo:

`bitsadmin /transfer myDownloadJob /download /priority normal https://downloadsrv/10mb.zip c:\\10mb.zip`

BITSAdmin continua a visualizzare le informazioni sullo stato di avanzamento nella finestra di MS-DOS fino al completamento del trasferimento o si verifica un errore.

## <a name="create-a-download-job"></a>Creazione di un processo di download

Per creare un processo di download denominato *myDownloadJob*:

```
bitsadmin /create myDownloadJob
```

BITSAdmin restituisce un GUID che identifica in modo univoco il processo. Utilizzare il GUID o il nome del processo nelle chiamate successive. Il testo seguente è un esempio di output.

### <a name="sample-output"></a>Output di esempio

`created job {C775D194-090F-431F-B5FB-8334D00D1CB6}`

## <a name="add-files-to-the-download-job"></a>Aggiungere file al processo di download

Per aggiungere un file al processo:

```
bitsadmin /addfile myDownloadJob https://downloadsrv/10mb.zip c:\\10mb.zip
```

Ripetere questa chiamata per ogni file che si desidera aggiungere. Se più processi usano *myDownloadJob* come nome, è necessario usare il GUID del processo per identificarlo in modo univoco per il completamento.

## <a name="activate-the-download-job"></a>Attivare il processo di download

Dopo aver creato un nuovo processo, BITS sospende automaticamente il processo. Per attivare il processo nella coda di trasferimento:

```
bitsadmin /resume myDownloadJob
```

Se più processi usano *myDownloadJob* come nome, è necessario usare il GUID del processo per identificarlo in modo univoco per il completamento.

## <a name="determine-the-progress-of-the-download-job"></a>Determinare lo stato di avanzamento del processo di download

L'opzione **/info** restituisce lo stato del processo e il numero di file e byte trasferiti. Quando lo stato viene visualizzato come `TRANSFERRED` , significa che BITS ha trasferito correttamente tutti i file nel processo. È anche possibile aggiungere l'argomento **/verbose** per ottenere i dettagli completi del processo e **/List** o **/Monitor** per ottenere tutti i processi nella coda di trasferimento.

Per restituire lo stato del processo:

```
bitsadmin /info myDownloadJob /verbose
```

Se più processi usano *myDownloadJob* come nome, è necessario usare il GUID del processo per identificarlo in modo univoco per il completamento.

## <a name="complete-the-download-job"></a>Completare il processo di download

Per completare il processo dopo che lo stato è stato modificato in `TRANSFERRED` :

```
bitsadmin /complete myDownloadJob
```

È necessario eseguire l' `/complete` opzione prima che i file del processo diventino disponibili. Se più processi usano *myDownloadJob* come nome, è necessario usare il GUID del processo per identificarlo in modo univoco per il completamento.

## <a name="monitor-jobs-in-the-transfer-queue-using-the-list-switch"></a>Monitorare i processi nella coda di trasferimento usando l'opzione/list

Per restituire lo stato del processo e il numero di file e byte trasferiti per tutti i processi nella coda di trasferimento:

```
bitsadmin /list
```

### <a name="sample-output"></a>Output di esempio

```
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN

Listed 2 job(s).
```

## <a name="monitor-jobs-in-the-transfer-queue-using-the-monitor-switch"></a>Monitorare i processi nella coda di trasferimento usando l'opzione/Monitor

Per restituire lo stato del processo e il numero di file e byte trasferiti per tutti i processi nella coda di trasferimento, aggiornare i dati ogni 5 secondi:

```
bitsadmin /monitor
```

> [!NOTE]
> Per arrestare l'aggiornamento, premere CTRL + C.

### <a name="sample-output"></a>Output di esempio

```
MONITORING BACKGROUND COPY MANAGER(5 second refresh)
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN
{0B138008-304B-4264-B021-FD04455588FF} job3 TRANSFERRED 1 / 1 100379370 / 100379370
```

## <a name="monitor-jobs-in-the-transfer-queue-using-the-info-switch"></a>Monitorare i processi nella coda di trasferimento usando l'opzione/Info

Per restituire lo stato del processo e il numero di file e byte trasferiti:

```
bitsadmin /info
```

### <a name="sample-output"></a>Output di esempio

```
GUID: {482FCAF0-74BF-469B-8929-5CCD028C9499} DISPLAY: myDownloadJob
TYPE: DOWNLOAD STATE: TRANSIENT_ERROR OWNER: domain\user
PRIORITY: NORMAL FILES: 0 / 1 BYTES: 0 / UNKNOWN
CREATION TIME: 12/17/2002 1:21:17 PM MODIFICATION TIME: 12/17/2002 1:21:30 PM
COMPLETION TIME: UNKNOWN
NOTIFY INTERFACE: UNREGISTERED NOTIFICATION FLAGS: 3
RETRY DELAY: 600 NO PROGRESS TIMEOUT: 1209600 ERROR COUNT: 0
PROXY USAGE: PRECONFIG PROXY LIST: NULL PROXY BYPASS LIST: NULL
ERROR FILE:    https://downloadsrv/10mb.zip -> c:\10mb.zip
ERROR CODE:    0x80072ee7 - The server name or address could not be resolved
ERROR CONTEXT: 0x00000005 - The error occurred while the remote file was being
processed.
DESCRIPTION:
JOB FILES:
0 / UNKNOWN WORKING https://downloadsrv/10mb.zip -> c:\10mb.zip
NOTIFICATION COMMAND LINE: none
```

## <a name="delete-jobs-from-the-transfer-queue"></a>Elimina processi dalla coda di trasferimento

Per rimuovere tutti i processi dalla coda di trasferimento, usare l'opzione/Reset:

```
bitsadmin /reset
```

### <a name="sample-output"></a>Output di esempio

```
{DC61A20C-44AB-4768-B175-8000D02545B9} canceled.
{BB6E91F3-6EDA-4BB4-9E01-5C5CBB5411F8} canceled.
2 out of 2 jobs canceled.
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
