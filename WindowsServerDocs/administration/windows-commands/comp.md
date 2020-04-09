---
title: comp
description: Windows Commands argomento for comp, che confronta il contenuto di due file o set di file byte per byte.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 40319d23-704d-4da1-be93-8259547275d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2f61743b55f38cfdebb17506368609895f48b4f6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847454"
---
# <a name="comp"></a>comp

Confronta il contenuto di due file o gruppi di file byte per byte. Se utilizzata senza parametri, **comp** viene richiesto di inserire i file da confrontare.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
comp [<Data1>] [<Data2>] [/d] [/a] [/l] [/n=<Number>] [/c]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Data1 >|Specifica il percorso e nome del primo file o set di file che si desidera confrontare. È possibile utilizzare caratteri jolly ( **&#42;** e **?** ) per specificare più file.|
|\<data2 >|Specifica il percorso e nome del secondo file o set di file che si desidera confrontare. È possibile utilizzare caratteri jolly ( **&#42;** e **?** ) per specificare più file.|
|/d|Visualizza le differenze in formato decimale. (Il formato predefinito è esadecimale).|
|/a|Visualizza le differenze come caratteri.|
|/l|Visualizza il numero della riga in cui viene riscontrata una differenza, invece di visualizzare l'offset di byte.|
|/n = numero\<>|Confronta solo il numero di righe che vengono specificate per ogni file, anche se i file sono di dimensioni diverse.|
|/c|Esegue un confronto senza tale distinzione.|
|/ [offline]|Elabora i file con il set di attributi non in linea.|
|/?|Visualizza la Guida dal prompt dei comandi.|

## <a name="remarks"></a>Note

-   Come il comando **comp** identifica le informazioni non corrispondenti

    Durante il confronto, **comp** Visualizza i messaggi che identificano le posizioni delle informazioni non uguali tra i file. Ogni messaggio indica che l'indirizzo di memoria offset dei byte differenti e il contenuto dei byte (in notazione esadecimale, a meno che il **/a** o **/d** viene specificato il parametro della riga di comando). I messaggi vengono visualizzati nel formato seguente:

    `Compare error at OFFSET xxxxxxxx`

    `file1 = xx`

    `file2 = xx`

    Dopo dieci confronti diversi, **comp** interrompe il confronto dei file e visualizza il messaggio seguente:

    `10 Mismatches - ending compare`
-   Gestione di casi speciali per *Data1* e *Data2*  
    -   Se si omettono i componenti necessari di entrambi *Data1* o *Data2* o se si omette *Data2*, **comp** richiede informazioni mancanti.
    -   Se *Data1* contiene solo una lettera di unità o un nome di directory con alcun nome di file, **comp** Confronta tutti i file nella directory specificata nel file specificato *Data1*.
    -   Se *Data2* contiene solo una lettera di unità o un nome di directory, il nome file predefinito per *Data2* è uguale a quello in *Data1*.
    -   Se **comp** Impossibile trovare i file viene specificato, verrà visualizzato un messaggio per determinare se si desidera confrontare più file.
-   Confronto di file in posizioni diverse

    **Comp** può confrontare i file nella stessa unità o in unità diverse e nella stessa directory o in directory diverse. Quando **comp** Confronta i file, Visualizza i nomi di file e i percorsi.
-   Confronto di file con gli stessi nomi

    I file che si confrontano possono avere lo stesso nome file, purché si trovino in directory diverse o in unità diverse. Se non si specifica un nome di file per *Data2*, il nome file predefinito per *Data2* corrisponde al nome del file in *Data1*. È possibile utilizzare caratteri jolly ( **&#42;** e **?** ) per specificare i nomi dei file.
-   Confronto di file di dimensioni diverse

    È necessario specificare **/n** per confrontare file di dimensioni diverse. Se le dimensioni dei file sono diverse e **/n** non è specificata, **comp** Visualizza il messaggio seguente:

    `Files are different sizes`

    `Compare more files (Y/N)?`

    Per confrontare questi file, premere N per arrestare il comando **comp** . Quindi, eseguire di nuovo il **comp** comando con il **/n** possibile confrontare solo la prima parte di ogni file.
-   Confronto dei file in sequenza

    Se si usano caratteri jolly ( **&#42;** e **?** ) per specificare più file, **comp** trova il primo file che corrisponde a *Data1* e lo confronta con il file corrispondente in *data2*, se esistente. Il **comp** i risultati del confronto per ogni file corrisponde *Data1*. Al termine, **comp** Visualizza il messaggio seguente:

    `Compare more files (Y/N)?`

    Per confrontare più file, premere Y. Il comando **comp** richiede i percorsi e i nomi dei nuovi file. Per interrompere l'operazione, premere N. Quando si preme Y, **comp** richiesto per le opzioni della riga di comando da utilizzare. Se non si specificano le opzioni della riga di comando, **comp** utilizza quelli specificati in precedenza.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per confrontare il contenuto della directory C:\Rapporti con la directory di backup \\\\Sales\Backup\April, digitare:
```
comp c:\reports \\sales\backup\april
```
Per confrontare le prime dieci righe dei file di testo nella directory \Invoice e visualizzare il risultato in formato decimale, digitare:
```
comp \invoice\*.txt \invoice\backup\*.txt /n=10 /d
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)