---
title: chkntfs
description: Argomento di riferimento per il comando chkntfs, che consente di visualizzare o modificare il controllo del disco automatico all'avvio del computer.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 93eca810-8699-4716-8e9d-aecd54f704be
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 69b8e21a7b43538b6296666d813f2b33daa8045f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82713652"
---
# <a name="chkntfs"></a>chkntfs

Visualizza o modifica il controllo del disco automatico all'avvio del computer. Se utilizzata senza opzioni, **chkntfs** visualizza la file System del volume specificato. Se è pianificata l'esecuzione del controllo automatico dei file, **chkntfs** Visualizza se il volume specificato è modificato o è pianificato per essere controllato al successivo avvio del computer.

> [!NOTE]
> Per eseguire **chkntfs**, è necessario essere un membro del gruppo Administrators.

## <a name="syntax"></a>Sintassi

```
chkntfs <volume> [...]
chkntfs [/d]
chkntfs [/t[:<time>]]
chkntfs [/x <volume> [...]]
chkntfs [/c <volume> [...]]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<volume>` [...] | Specifica uno o più volumi da controllare all'avvio del computer. I volumi validi includono le lettere di unità (seguite da due punti), i punti di montaggio o i nomi dei volumi. |
| /d | Ripristina tutte le impostazioni predefinite di **chkntfs** , eccetto l'ora del conto alla rovescia per il controllo automatico dei file. Per impostazione predefinita, tutti i volumi vengono controllati all'avvio del computer e **chkdsk** viene eseguito su quelli sporchi. |
| /t [`:<time>`] | Modifica l'ora del conto alla rovescia di avvio Autochk. exe per la quantità di tempo specificata in secondi. Se non si immette un'ora, **/t** Visualizza l'ora del conto alla rovescia corrente. |
| /x `<volume>` [...] | Specifica uno o più volumi da escludere dal controllo quando il computer viene avviato, anche se il volume è contrassegnato per la richiesta di **chkdsk**. |
| /c `<volume>` [...] | Pianifica uno o più volumi da controllare all'avvio del computer ed esegue **chkdsk** su quelli che sono sporchi. |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="examples"></a>Esempi

Per visualizzare il tipo di file system per l'unità C, digitare:

```
chkntfs c:
```

> [!NOTE]
> Se è pianificata l'esecuzione del controllo automatico dei file, verrà visualizzato un output aggiuntivo che indica se l'unità è sporca o è stata pianificata manualmente per essere controllata al successivo avvio del computer.

Per visualizzare l'ora del conto alla rovescia di avvio Autochk. exe, digitare:

```
chkntfs /t
```

Per modificare il tempo del conto alla rovescia di avvio di Autochk. exe in 30 secondi, digitare:

```
chkntfs /t:30
```

> [!NOTE]
> Sebbene sia possibile impostare il tempo del conto alla rovescia per l'avvio di Autochk. exe su zero, in questo modo si impedisce l'annullamento di un controllo automatico del file che richiede molto tempo.

Per escludere più volumi da controllare, è necessario elencarli tutti in un unico comando. Ad esempio, per escludere sia i volumi D che E, digitare:

```
chkntfs /x d: e:
```

> [!IMPORTANT]
> L'opzione della riga di comando **/x** non è cumulativa. Se si digita più volte, la voce più recente sostituisce quella precedente.

Per pianificare il controllo automatico dei file nel volume D, ma non nei volumi C o E, digitare i comandi seguenti nell'ordine indicato:

```
chkntfs /d
chkntfs /x c: d: e:
chkntfs /c d:
```

> [!IMPORTANT]
> L'opzione della riga di comando **/c** è cumulativa. Se si digita **/c** più di una volta, ogni voce rimane. Per assicurarsi che sia selezionato solo un determinato volume, reimpostare le impostazioni predefinite per cancellare tutti i comandi precedenti, escludere tutti i volumi da controllare e quindi pianificare il controllo automatico dei file nel volume desiderato.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
