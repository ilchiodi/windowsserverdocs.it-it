---
title: Risolvere i problemi relativi al messaggio di errore ID evento 50
description: Viene descritto come risolvere il messaggio di errore ID evento 50
author: Deland-Han
manager: dcscontentpm
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 7ce3551b60450a3720c9350b5c55f396368490c1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80815234"
---
# <a name="troubleshoot-the-event-id-50-error-message"></a>Risolvere i problemi relativi al messaggio di errore ID evento 50

##  <a name="symptoms"></a>Sintomi

Quando si stanno scrivendo informazioni sul disco fisico, è possibile che nel registro eventi di sistema vengano registrati i due messaggi di evento seguenti: 

```
Event ID: 50 
Event Type: Warning 
Event Source: Ftdisk 
Description: {Lost Delayed-Write Data} The system was attempting to transfer file data from buffers to \Device\HarddiskVolume4. The write operation failed, and only some of the data may have been written to the file.
Data: 
0000: 00 00 04 00 02 00 56 00 
0008: 00 00 00 00 32 00 04 80 
0010: 00 00 00 00 00 00 00 00 
0018: 00 00 00 00 00 00 00 00 
0020: 00 00 00 00 00 00 00 00 
0028: 11 00 00 80 
```

```
Event ID: 26 
Event Type: Information
Event Source: Application Popup
Description: Windows - Delayed Write Failed : Windows was unable to save all the data for the file \Device\HarddiskVolume4\Program Files\Microsoft SQL Server\MSSQL$INSTANCETWO\LOG\ERRORLOG. The data has been lost. This error may be caused by a failure of your computer hardware or network connection.

Please try to save this file elsewhere.
```

Questi messaggi di ID evento indicano esattamente la stessa cosa e vengono generati per gli stessi motivi. Ai fini di questo articolo, viene descritto solo il messaggio dell'ID evento 50.

> [!NOTE] 
> Il dispositivo e il percorso nella descrizione e i dati esadecimali specifici possono variare. 

##  <a name="more-information"></a>Altre informazioni

Un messaggio con ID evento 50 viene registrato se si verifica un errore generico quando Windows tenta di scrivere informazioni sul disco. Questo errore si verifica quando Windows tenta di eseguire il commit dei dati dal file system gestione cache (non dalla cache a livello hardware) al disco fisico. Questo comportamento fa parte della gestione della memoria di Windows. Se, ad esempio, un programma invia una richiesta di scrittura, la richiesta di scrittura viene memorizzata nella cache da Gestione cache e il programma viene informato che la scrittura è stata completata correttamente. In un momento successivo, gestione cache tenta di scrivere i dati nel disco fisico in modo lazy. Quando Gestione cache tenta di eseguire il commit dei dati su disco, si verifica un errore durante la scrittura dei dati e i dati vengono scaricati dalla cache ed eliminati. La memorizzazione nella cache write-back migliora le prestazioni del sistema, ma la perdita di dati e l'integrità del volume possono verificarsi a causa di errori di scrittura ritardata persi.

È importante ricordare che non tutte le I/O vengono memorizzate nel buffer di I/O dal gestore della cache. I programmi possono impostare un flag di FILE_FLAG_NO_BUFFERING che ignora gestione cache. Quando SQL esegue scritture critiche in un database, questo flag è impostato in modo da garantire che la transazione venga completata direttamente su disco. Ad esempio, le Scritture non critiche nei file di log eseguono l'I/O memorizzato nel buffer per migliorare le prestazioni complessive. Un messaggio con ID evento 50 non risulta mai dall'I/O non memorizzato nel buffer.

Sono disponibili diverse origini per un messaggio con ID evento 50. Ad esempio, un messaggio con ID evento 50 registrato da un'origine MRxSmb si verifica se si verifica un problema di connettività di rete con il redirector. Per evitare l'esecuzione di passaggi di risoluzione dei problemi non corretti, assicurarsi di esaminare il messaggio con ID evento 50 per confermare che si riferisce a un problema di I/O su disco e che questo articolo sia valido.

Un messaggio con ID evento 50 è simile a un ID evento 9 e a un messaggio con ID 11. Sebbene l'errore non sia così grave come l'errore indicato dall'ID evento 9 e un messaggio con ID 11, è possibile utilizzare le stesse tecniche di risoluzione dei problemi per un messaggio con ID evento 50 come per un evento con ID 9 e un messaggio di ID evento 11. Tuttavia, tenere presente che qualsiasi elemento nello stack può provocare Scritture con ritardo perduto, ad esempio driver di filtro e driver di mini-porta. 

Per identificare il problema, è possibile usare i dati binari associati a qualsiasi errore "disco" associato (indicato da un ID evento 9, 11, messaggio di errore 51 o altri messaggi).

###  <a name="how-to-decode-the-data-section-of-an-event-id-50-event-message"></a>Come decodificare la sezione di dati di un messaggio di evento con ID evento 50 

Quando si decodifica la sezione dati nell'esempio di un messaggio con ID evento 50 incluso nella sezione "Summary", si noterà che il tentativo di eseguire un'operazione di scrittura non è riuscito perché il dispositivo è occupato e i dati sono stati persi. Questa sezione descrive come decodificare questo messaggio di ID evento 50. 

Nella tabella seguente viene descritto il significato di ogni offset del messaggio: 

|OffsetLengthValues|Length|Valori|
|-----------|------------|---------|
|0x00|2|Non utilizzate|
|0x02|2|Dimensioni dati dump = 0x0004|
|0x04|2|Numero di stringhe = 0x0002|
|0x06|2|Offset alle stringhe|
|0x08|2|Categoria evento|
|0x0C|4|Codice errore NTSTATUS = 0x80040032 = IO_LOST_DELAYED_WRITE|
|0x10|8|Non utilizzate|
|0x18|8|Non utilizzate|
|0x20|8|Non utilizzate|
|0x28|4|Codice di errore stato NT|

#### <a name="key-sections-to-decode"></a>Sezioni principali da decodificare

**Codice di errore**

Nell'esempio nella sezione "Summary", il codice di errore è elencato nella seconda riga. Questa riga inizia con "0008:" e include gli ultimi quattro byte in questa riga: 0008:00 00 00 00 32 00 04 80 in questo caso, il codice di errore è 0x80040032. Il codice seguente è il codice per l'errore 50 ed è lo stesso per tutti i messaggi con ID 50: IO_LOST_DELAYED_WRITEWARNINGNote quando si convertono i dati esadecimali nel messaggio ID evento nel codice di stato, tenere presente che i valori sono rappresentati nel formato little endian.

**Disco di destinazione**

È possibile identificare il disco in cui è stata tentata la scrittura usando il collegamento simbolico elencato nell'unità nella sezione "Description" del messaggio ID evento, ad esempio: \Device\HarddiskVolume4. Per ulteriori informazioni su come identificare l'unità, fare clic sul seguente numero di articolo per visualizzare l'articolo della Microsoft Knowledge Base: [159865](/EN-US/help/159865) come distinguere un dispositivo disco fisico da un messaggio di evento

**Il codice di stato finale**

Il codice di stato finale è l'informazione più importante in un messaggio con ID evento 50. Questo è il codice di errore che viene restituito quando è stata effettuata la richiesta di I/O ed è l'origine chiave delle informazioni. Nell'esempio nella sezione "Summary", il codice di stato finale è elencato in 0x28, la sesta riga, che inizia con "0028:" e include gli unici quattro ottetti in questa riga: 

```
0028: 11 00 00 80 
```

In questo caso, lo stato finale è uguale a 0x80000011. Questo codice di stato viene mappato a STATUS_DEVICE_BUSY e implica che il dispositivo è attualmente occupato.

>[!NOTE] 
> Quando si convertono i dati esadecimali nel messaggio ID evento 50 nel codice di stato, tenere presente che i valori sono rappresentati nel formato little endian. Poiché il codice di stato è l'unica informazione a cui si è interessati, potrebbe essere più semplice visualizzare i dati in formato parole invece che in byte. In tal caso, i byte saranno nel formato corretto e i dati potrebbero essere più facili da interpretare rapidamente.

A tale scopo, fare clic su **parole** nella finestra **Proprietà evento** . Nella visualizzazione parole dati l'esempio nella sezione "Sintomi" viene letto come segue: dati: 

```
() Bytes (.) 
Words 0000: 00040000 00560002 00000000 80040032 0010: 00000000 00000000 00000000 00000000 0020: 00000000 00000000 80000011
```

Per ottenere un elenco dei codici di stato di Windows NT, vedere NTSTATUS. H in Windows Software Developers Kit (SDK).