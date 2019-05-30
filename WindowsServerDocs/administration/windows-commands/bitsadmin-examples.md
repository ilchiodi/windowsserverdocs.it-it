---
title: esempi di Bitsadmin
description: Negli esempi seguenti illustrano come usare lo strumento bitsadmin per eseguire le attività più comuni.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cb8f8374-ba6e-4a68-85a1-9a95b8215354
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/31/2018
ms.openlocfilehash: a98e1a876c972b0f146ff37aff0a77399b684e99
ms.sourcegitcommit: 8eea7aadbe94f5d4635c4ffedc6a831558733cc0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/29/2019
ms.locfileid: "66308555"
---
# <a name="bitsadmin-examples"></a>esempi di Bitsadmin

Gli esempi seguenti illustrano come usare il `bitsadmin` dello strumento per eseguire le attività più comuni.

## <a name="transfer-a-file"></a>Trasferire un file

Il **/trasferimento** switch è un collegamento per eseguire le attività elencate di seguito. Questa opzione crea il processo, aggiunge i file al processo, viene attivato il processo nella coda di trasferimento e completa il processo. BITSAdmin continua a visualizzare le informazioni sullo stato nella finestra di MS-DOS fino a quando non viene completato il trasferimento o si verifica un errore.

**normale /priority /download myDownloadJob /transfer Bitsadmin `https://downloadsrv/10mb.zip c:\\10mb.zip`**

## <a name="create-a-download-job"></a>Creare un processo di download

Usare il **/create** switch per creare un processo di download denominato myDownloadJob.

**Bitsadmin / creare myDownloadJob**

BITSAdmin restituisce un GUID che identifica in modo univoco il processo. Usare il nome di processo o GUID nelle chiamate successive. Il testo seguente è riportato un output.

``` syntax
Created job {C775D194-090F-431F-B5FB-8334D00D1CB6}.
```

Successivamente, usare il **/addfile** commutatore di aggiungere uno o più file per il processo di download.

## <a name="add-files-to-the-download-job"></a>Aggiungere file al processo di download

Usare la **/addfile** commutatore di aggiungere un file al processo. Ripetere questa chiamata per ogni file da aggiungere. Se più processi usano myDownloadJob per il proprio nome, è necessario sostituire myDownloadJob con il GUID del processo per identificare in modo univoco il processo.

**bitsadmin /addfile myDownloadJob https://downloadsrv/10mb.zip c:\\10mb.zip**

Per attivare il processo nella coda di trasferimento, usare il **/ripresa** passare.

## <a name="activate-the-download-job"></a>Attivare il processo di download

Quando si crea un nuovo processo, BITS sospende il processo. Per attivare il processo nella coda di trasferimento, usare il **/ripresa** passare. Se più processi usano myDownloadJob per il proprio nome, è necessario sostituire myDownloadJob con il GUID del processo per identificare in modo univoco il processo.

**myDownloadJob /resume Bitsadmin**

Per determinare lo stato di avanzamento del processo, usare il **/List**, **/info**, o **/monitorare** passare.

## <a name="determine-the-progress-of-the-download-job"></a>Determinare lo stato di avanzamento del processo di download

Usare la **/info** commutatore per determinare lo stato di avanzamento di un processo. Se più processi usano myDownloadJob per il proprio nome, è necessario sostituire myDownloadJob con il GUID del processo per identificare in modo univoco il processo.

**Bitsadmin /info myDownloadJob / verbose**

Il **/info** switch restituisce lo stato del processo e il numero di file e i byte trasferiti. Quando lo stato viene TRASFERITO, BITS avrà trasferito tutti i file del processo. Il **/Verbose** argomento è contenute informazioni dettagliate del processo. Il testo seguente è riportato un output.

``` syntax
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

Per ricevere le informazioni per tutti i processi nella coda di trasferimento, utilizzare il **/List** o **/monitorare** passare.

## <a name="completing-the-download-job"></a>Il completamento del processo di download

Quando lo stato del processo viene TRASFERITO, BITS avrà trasferito tutti i file del processo. Tuttavia, i file non sono disponibili finché non si usa la **/ completo** passare. Se più processi usano myDownloadJob per il proprio nome, è necessario sostituire myDownloadJob con il GUID del processo per identificare in modo univoco il processo.

**Bitsadmin / completare myDownloadJob**

## <a name="monitoring-jobs-in-the-transfer-queue"></a>Monitoraggio dei processi nella coda di trasferimento

Usare il **/List**, **/il monitoraggio**, o **/info** switch per monitorare i processi nella coda di trasferimento. Il **/List** commutatore fornisce informazioni per tutti i processi nella coda.

**bitsadmin /list**

Il **/List** switch restituisce lo stato del processo e il numero di file e i byte trasferiti per tutti i processi nella coda di trasferimento. Il testo seguente è riportato un output.

``` syntax
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN

Listed 2 job(s).
```

Usare la **/monitorare** switch per monitorare tutti i processi nella coda. Il **/monitorare** commutatore vengono aggiornati i dati ogni 5 secondi. Per arrestare l'aggiornamento, immettere CTRL + C.

**/monitor Bitsadmin**

Il **/monitorare** switch restituisce lo stato del processo e il numero di file e i byte trasferiti per tutti i processi nella coda di trasferimento. Il testo seguente è riportato un output.

``` syntax
MONITORING BACKGROUND COPY MANAGER(5 second refresh)
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN
{0B138008-304B-4264-B021-FD04455588FF} job3 TRANSFERRED 1 / 1 100379370 / 100379370
```

## <a name="deleting-jobs-from-the-transfer-queue"></a>Eliminazione dei processi dalla coda di trasferimento

Usare la **/Reimposta** switch per rimuovere tutti i processi dalla coda di trasferimento.

**bitsadmin /reset**

Il testo seguente è riportato un output.

``` syntax
{DC61A20C-44AB-4768-B175-8000D02545B9} canceled.
{BB6E91F3-6EDA-4BB4-9E01-5C5CBB5411F8} canceled.
2 out of 2 jobs canceled.
```
