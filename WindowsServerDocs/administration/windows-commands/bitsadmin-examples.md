---
title: esempi di Bitsadmin
description: Gli esempi seguenti illustrano come usare lo strumento Bitsadmin per eseguire le attività più comuni.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: c675f08752b3464f7ab1eddd4e9fddf3b16db5f4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381766"
---
# <a name="bitsadmin-examples"></a>esempi di Bitsadmin

Negli esempi seguenti viene illustrato come utilizzare lo strumento `bitsadmin` per eseguire le attività più comuni.

## <a name="transfer-a-file"></a>Trasferire un file

L'opzione **/Transfer** è un collegamento per l'esecuzione delle attività elencate di seguito. Questa opzione Crea il processo, aggiunge i file al processo, attiva il processo nella coda di trasferimento e completa il processo. BITSAdmin continua a visualizzare le informazioni sullo stato di avanzamento nella finestra di MS-DOS fino al completamento del trasferimento o si verifica un errore.

**Bitsadmin/Transfer myDownloadJob/download/Priority Normal `https://downloadsrv/10mb.zip c:\\10mb.zip`**

## <a name="create-a-download-job"></a>Creazione di un processo di download

Usare l'opzione **/create** per creare un processo di download denominato myDownloadJob.

**Bitsadmin/create myDownloadJob**

BITSAdmin restituisce un GUID che identifica in modo univoco il processo. Utilizzare il GUID o il nome del processo nelle chiamate successive. Il testo seguente è un esempio di output.

``` syntax
Created job {C775D194-090F-431F-B5FB-8334D00D1CB6}.
```

Usare quindi l'opzione **/AddFile** per aggiungere uno o più file al processo di download.

## <a name="add-files-to-the-download-job"></a>Aggiungere file al processo di download

Usare l'opzione **/AddFile** per aggiungere un file al processo. Ripetere questa chiamata per ogni file che si desidera aggiungere. Se più processi usano myDownloadJob come nome, è necessario sostituire myDownloadJob con il GUID del processo per identificare in modo univoco il processo.

**Bitsadmin/AddFile myDownloadJob https://downloadsrv/10mb.zip c: @no__t -210MB. zip**

Per attivare il processo nella coda di trasferimento, usare l'opzione **/Resume** .

## <a name="activate-the-download-job"></a>Attivare il processo di download

Quando si crea un nuovo processo, BITS sospende il processo. Per attivare il processo nella coda di trasferimento, usare l'opzione **/Resume** . Se più processi usano myDownloadJob come nome, è necessario sostituire myDownloadJob con il GUID del processo per identificare in modo univoco il processo.

**Bitsadmin/Resume myDownloadJob**

Per determinare lo stato di avanzamento del processo, usare l'opzione **/List**, **/info**o **/Monitor** .

## <a name="determine-the-progress-of-the-download-job"></a>Determinare lo stato di avanzamento del processo di download

Usare l'opzione **/info** per determinare lo stato di avanzamento di un processo. Se più processi usano myDownloadJob come nome, è necessario sostituire myDownloadJob con il GUID del processo per identificare in modo univoco il processo.

**Bitsadmin/info myDownloadJob/verbose**

L'opzione **/info** restituisce lo stato del processo e il numero di file e byte trasferiti. Quando lo stato viene trasferito, BITS ha trasferito correttamente tutti i file nel processo. L'argomento **/verbose** fornisce i dettagli completi del processo. Il testo seguente è un esempio di output.

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

Per ricevere informazioni per tutti i processi nella coda di trasferimento, usare l'opzione **/List** o **/Monitor** .

## <a name="completing-the-download-job"></a>Completamento del processo di download

Quando lo stato del processo viene trasferito, BITS ha trasferito correttamente tutti i file nel processo. Tuttavia, i file non sono disponibili finché non si usa l'opzione **/completo** . Se più processi usano myDownloadJob come nome, è necessario sostituire myDownloadJob con il GUID del processo per identificare in modo univoco il processo.

**Bitsadmin/completo myDownloadJob**

## <a name="monitoring-jobs-in-the-transfer-queue"></a>Monitoraggio dei processi nella coda di trasferimento

Utilizzare l'opzione **/List**, **/Monitor**o **/info** per monitorare i processi nella coda di trasferimento. L'opzione **/List** fornisce informazioni per tutti i processi nella coda.

**/List Bitsadmin**

L'opzione **/List** restituisce lo stato del processo e il numero di file e byte trasferiti per tutti i processi nella coda di trasferimento. Il testo seguente è un esempio di output.

``` syntax
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN

Listed 2 job(s).
```

Usare l'opzione **/Monitor** per monitorare tutti i processi nella coda. L'opzione **/Monitor** aggiorna i dati ogni 5 secondi. Per arrestare l'aggiornamento, premere CTRL + C.

**/monitor Bitsadmin**

L'opzione **/Monitor** restituisce lo stato del processo e il numero di file e byte trasferiti per tutti i processi nella coda di trasferimento. Il testo seguente è un esempio di output.

``` syntax
MONITORING BACKGROUND COPY MANAGER(5 second refresh)
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN
{0B138008-304B-4264-B021-FD04455588FF} job3 TRANSFERRED 1 / 1 100379370 / 100379370
```

## <a name="deleting-jobs-from-the-transfer-queue"></a>Eliminazione di processi dalla coda di trasferimento

Usare l'opzione **/Reset** per rimuovere tutti i processi dalla coda di trasferimento.

**/Reset Bitsadmin**

Il testo seguente è un esempio di output.

``` syntax
{DC61A20C-44AB-4768-B175-8000D02545B9} canceled.
{BB6E91F3-6EDA-4BB4-9E01-5C5CBB5411F8} canceled.
2 out of 2 jobs canceled.
```
