---
title: chkntfs
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 93eca810-8699-4716-8e9d-aecd54f704be
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 876c0e0c254216ac217aea7d165d5f4e3a7da9b4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861992"
---
# <a name="chkntfs"></a>chkntfs



Visualizza o modifica automatica del disco di controllo quando il computer viene avviato. Se utilizzata senza le opzioni, **chkntfs** consente di visualizzare il file system del volume specificato. Se il controllo automatico dei file è pianificato per l'esecuzione, **chkntfs** Visualizza se il volume specificato è stato modificato o verrà selezionata la volta successiva che il computer viene avviato.

> [!NOTE]
> Per eseguire **chkntfs**, è necessario essere un membro del gruppo Administrators.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
chkntfs <Volume> [...]
chkntfs [/d]
chkntfs [/t[:<Time>]]
chkntfs [/x <Volume> [...]]
chkntfs [/c <Volume> [...]]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Volume > [...]|Specifica uno o più volumi per controllare quando il computer viene avviato. I volumi validi includono lettere di unità (seguite da due punti), punti o i nomi dei volumi di montaggio.|
|/d|Ripristina tutti **chkntfs** predefinite, eccetto il conto alla rovescia per la verifica automatica dei file. Per impostazione predefinita, tutti i volumi vengono controllati quando il computer viene avviato, e **chkdsk** viene eseguito su quelli che vengono modificati.|
|/t [:\<tempo >]|Modifica il conto alla rovescia initiation Autochk.exe alla quantità di tempo espresso in secondi. Se non si immette una volta **/t** Visualizza l'ora corrente del conto alla rovescia.|
|/x \<volume > [...]|Specifica uno o più volumi da escludere dalla verifica quando il computer viene avviato, anche se il volume è contrassegnato in modo che richiedano **chkdsk**.|
|/c \<volume > [...]|Pianifica l'uno o più volumi da controllare quando il computer viene avviato e viene eseguito **chkdsk** su quelle che sono dirty.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="BKMK_examples"></a>Esempi

Per visualizzare il tipo di file system per l'unità C, digitare:
```
chkntfs c:
```
L'output seguente indica un file system NTFS:
```
The type of the file system is NTFS.
```

> [!NOTE]
> Se il controllo automatico dei file è pianificato per l'esecuzione, verrà visualizzato l'output aggiuntivi, che indica se l'unità è stato modificato o è stata manualmente pianificata sia selezionata la volta successiva che il computer viene avviato.

Per visualizzare il conto alla rovescia Autochk.exe avvio, digitare:
```
chkntfs /t
```
Ad esempio, se il conto alla rovescia viene impostato su 10 secondi, viene visualizzato l'output seguente:
```
The AUTOCHK initiation countdown time is set to 10 second(s).
```
Per modificare il conto alla rovescia initiation Autochk.exe su 30 secondi, digitare:
```
chkntfs /t:30
```

> [!NOTE]
> Sebbene sia possibile impostare il conto alla rovescia Autochk.exe inizializzazione su zero, tale operazione così sarà dall'annullamento di una verifica automatica dei file potrebbe richiedere molto tempo.

Il **/x** opzione della riga di comando non è cumulabile. Se si digita più di una volta, la voce più recente sostituisce la voce precedente. Per escludere più volumi da sottoposto al controllo, è necessario elencare ognuno di essi in un unico comando. Ad esempio, per escludere i volumi sia D ed E, digitare:
```
chkntfs /x d: e:
```
Il **/c** opzione della riga di comando è cumulativa. Se si digita **/c** più volte, ogni voce rimane. Per assicurarsi che sia selezionato solo un determinato volume, reimpostare le impostazioni predefinite per cancellare tutti i comandi precedenti, escludere tutti i volumi di archiviazione e quindi pianificare controllo sul volume automatico dei file.

Ad esempio, per pianificare il volume D ma non i volumi C o E controllo automatico dei file, digitare i comandi seguenti nell'ordine indicato:
```
chkntfs /d
chkntfs /x c: d: e:
chkntfs /c d:
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)