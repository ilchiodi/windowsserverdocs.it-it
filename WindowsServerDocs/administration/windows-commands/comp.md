---
title: comp
description: Argomento di riferimento per il comando comp, che confronta il contenuto di due file o set di file byte per byte.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 40319d23-704d-4da1-be93-8259547275d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c2939ee2166d961cae8ae0699c130e91117dd8a6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82711471"
---
# <a name="comp"></a>comp

Confronta il contenuto di due file o gruppi di file byte per byte. Questi file possono essere archiviati nella stessa unità o in unità diverse e nella stessa directory o in directory diverse. Quando questo comando Confronta i file, ne Visualizza il percorso e i nomi file. Se utilizzata senza parametri, **comp** viene richiesto di inserire i file da confrontare.

## <a name="syntax"></a>Sintassi

```
comp [<data1>] [<data2>] [/d] [/a] [/l] [/n=<number>] [/c]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<data1>` | Specifica il percorso e nome del primo file o set di file che si desidera confrontare. È possibile utilizzare caratteri jolly (**&#42;** e **?**) per specificare più file. |
| `<data2>` | Specifica il percorso e nome del secondo file o set di file che si desidera confrontare. È possibile utilizzare caratteri jolly (**&#42;** e **?**) per specificare più file. |
| /d | Visualizza le differenze in formato decimale. (Il formato predefinito è esadecimale). |
| /a | Visualizza le differenze come caratteri. |
| /l | Visualizza il numero della riga in cui viene riscontrata una differenza, invece di visualizzare l'offset di byte. |
| /n =`<number>` | Confronta solo il numero di righe che vengono specificate per ogni file, anche se i file sono di dimensioni diverse. |
| /C | Esegue un confronto senza tale distinzione. |
| / [offline] | Elabora i file con il set di attributi non in linea. |
| /? | Visualizza la Guida al prompt dei comandi. |

## <a name="remarks"></a>Osservazioni

- Durante il confronto, **comp** Visualizza i messaggi che identificano le posizioni delle informazioni non uguali tra i file. Ogni messaggio indica che l'indirizzo di memoria offset dei byte differenti e il contenuto dei byte (in notazione esadecimale, a meno che il **/a** o **/d** viene specificato il parametro della riga di comando). I messaggi vengono visualizzati nel formato seguente:

    ```
    Compare error at OFFSET xxxxxxxx
    file1 = xx
    file2 = xx
    ```

    Dopo dieci confronti diversi, **comp** interrompe il confronto dei file e visualizza il messaggio seguente:

    `10 Mismatches - ending compare`

- Se si omettono i componenti necessari di *Data1* o *data2*oppure se si omette interamente *data2* , questo comando richiederà di specificare le informazioni mancanti.

- Se *Data1* contiene solo una lettera di unità o un nome di directory senza nome file, questo comando Confronta tutti i file nella directory specificata con il file specificato in *Data1*.

- Se *data2* contiene solo una lettera di unità o un nome di directory, il nome file predefinito per *data2* diventa lo stesso nome di *Data1*.

- Se il comando **comp** non riesce a trovare i file specificati, verrà richiesto di specificare un messaggio che indica se si desidera confrontare file aggiuntivi.

- I file che si confrontano possono avere lo stesso nome file, purché si trovino in directory diverse o in unità diverse. È possibile utilizzare caratteri jolly (**&#42;** e **?**) per specificare i nomi dei file.

- È necessario specificare **/n** per confrontare file di dimensioni diverse. Se le dimensioni dei file sono diverse e **/n** non è specificato, viene visualizzato il messaggio seguente:

    ```
    Files are different sizes
    Compare more files (Y/N)?
    ```

    Per confrontare comunque questi file, premere **N** per arrestare il comando. Eseguire quindi di nuovo il comando **comp** , usando l'opzione **/n** per confrontare solo la prima parte di ogni file.

- Se si usano caratteri jolly (**&#42;** e **?**) per specificare più file, **comp** trova il primo file che corrisponde a *Data1* e lo confronta con il file corrispondente in *data2*, se esistente. Il comando **comp** segnala i risultati del confronto per ogni file corrispondente a *Data1*. Al termine, **comp** Visualizza il messaggio seguente:

    `Compare more files (Y/N)?`

    Per confrontare più file, premere **Y**. Il comando **comp** richiede i percorsi e i nomi dei nuovi file. Per arrestare i confronti, premere **N**. Quando si preme **Y**, vengono richieste le opzioni della riga di comando da usare. Se non si specifica alcuna opzione della riga di comando, **comp** usa quelli specificati in precedenza.

## <a name="examples"></a>Esempi

Per confrontare il contenuto della directory *c:\Rapporti* con la directory `\\sales\backup\april`di backup, digitare:

```
comp c:\reports \\sales\backup\april
```

Per confrontare le prime dieci righe dei file di testo nella directory *\Invoice* e visualizzare il risultato in formato decimale, digitare:

```
comp \invoice\*.txt \invoice\backup\*.txt /n=10 /d
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)