---
title: chkntfs
description: Windows Commands Topic for chkntfs, che consente di visualizzare o modificare il controllo automatico del disco all'avvio del computer.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 93eca810-8699-4716-8e9d-aecd54f704be
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bb04022964b3c315c1003a9746f6551fc281dba3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847774"
---
# <a name="chkntfs"></a>chkntfs

Visualizza o modifica il controllo del disco automatico all'avvio del computer. Se utilizzata senza opzioni, **chkntfs** visualizza la file System del volume specificato. Se è pianificata l'esecuzione del controllo automatico dei file, **chkntfs** Visualizza se il volume specificato è modificato o è pianificato per essere controllato al successivo avvio del computer.

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

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<volume > [...]|Specifica uno o più volumi da controllare all'avvio del computer. I volumi validi includono le lettere di unità (seguite da due punti), i punti di montaggio o i nomi dei volumi.|
|/d|Ripristina tutte le impostazioni predefinite di **chkntfs** , eccetto l'ora del conto alla rovescia per il controllo automatico dei file. Per impostazione predefinita, tutti i volumi vengono controllati all'avvio del computer e **chkdsk** viene eseguito su quelli sporchi.|
|/t [:\<ora >]|Modifica l'ora del conto alla rovescia di avvio Autochk. exe per la quantità di tempo specificata in secondi. Se non si immette un'ora, **/t** Visualizza l'ora del conto alla rovescia corrente.|
|/x \<volume > [...]|Specifica uno o più volumi da escludere dal controllo quando il computer viene avviato, anche se il volume è contrassegnato per la richiesta di **chkdsk**.|
|/c \<volume > [...]|Pianifica uno o più volumi da controllare all'avvio del computer ed esegue **chkdsk** su quelli che sono sporchi.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per visualizzare il tipo di file system per l'unità C, digitare:
```
chkntfs c:
```
L'output seguente indica un file system NTFS:
```
The type of the file system is NTFS.
```

> [!NOTE]
> Se è pianificata l'esecuzione del controllo automatico dei file, verrà visualizzato un output aggiuntivo che indica se l'unità è sporca o è stata pianificata manualmente per essere controllata al successivo avvio del computer.

Per visualizzare l'ora del conto alla rovescia di avvio Autochk. exe, digitare:
```
chkntfs /t
```
Se, ad esempio, l'ora del conto alla rovescia è impostata su 10 secondi, viene visualizzato l'output seguente:
```
The AUTOCHK initiation countdown time is set to 10 second(s).
```
Per modificare il tempo del conto alla rovescia di avvio di Autochk. exe in 30 secondi, digitare:
```
chkntfs /t:30
```

> [!NOTE]
> Sebbene sia possibile impostare il tempo del conto alla rovescia per l'avvio di Autochk. exe su zero, in questo modo si impedisce l'annullamento di un controllo automatico del file che richiede molto tempo.

L'opzione della riga di comando **/x** non è cumulativa. Se si digita più volte, la voce più recente sostituisce quella precedente. Per escludere più volumi da controllare, è necessario elencarli tutti in un unico comando. Ad esempio, per escludere sia i volumi D che E, digitare:
```
chkntfs /x d: e:
```
L'opzione della riga di comando **/c** è cumulativa. Se si digita **/c** più di una volta, ogni voce rimane. Per assicurarsi che sia selezionato solo un determinato volume, reimpostare le impostazioni predefinite per cancellare tutti i comandi precedenti, escludere tutti i volumi da controllare e quindi pianificare il controllo automatico dei file nel volume desiderato.

Ad esempio, per pianificare il controllo automatico dei file sul volume D ma non sui volumi C o E, digitare i comandi seguenti nell'ordine indicato:
```
chkntfs /d
chkntfs /x c: d: e:
chkntfs /c d:
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)