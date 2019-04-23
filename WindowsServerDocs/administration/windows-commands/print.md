---
title: print
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aa2325d5-a993-4ed3-b996-255165452db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d85fc5b2cd5f5ba09ebdf4756a5adb60c1759f2a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831552"
---
# <a name="print"></a>print



Invia un file di testo a una stampante.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
Print [/d:<PrinterName>] [<Drive>:][<Path>]<FileName>[ ...]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/d:\<PrinterName>|Specifica la stampante che si desidera eseguire il processo di stampa. Per stampare su una stampante connessa in locale, specificare la porta nel computer in cui è connesso alla stampante.</br>-I valori validi per le porte parallele sono LPT1, LPT2 e LPT3.</br>-I valori validi per le porte seriali sono COM1, COM2, COM3 e COM4.</br>È anche possibile specificare una stampante di rete utilizzando il nome della coda (\\\\*ServerName*\*nomestampante *). Se non si specifica una stampante, il processo di stampa viene inviato alla porta LPT1 per impostazione predefinita.|
|\<Drive>:|Specifica l'unità logica o fisica in cui si trova il file che si desidera stampare. Questo parametro non è obbligatorio se il file desiderato si trova nell'unità attuale.|
|\<Path>|Specifica il percorso del file che si desidera stampare. Questo parametro non è obbligatorio se il file desiderato si trova nella directory corrente.|
|\<Nome file > [...]|Obbligatorio. Specifica il file che si desidera stampare. È possibile includere più file in un unico comando.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Un file è possibile stampare in background se viene inviato a una stampante collegata a una porta seriale o parallela nel computer locale.
-   È possibile eseguire molte attività di configurazione dal prompt dei comandi usando il **modalità** comando.

    Visualizzare [modalità](mode.md) per altre informazioni:  
    -   Configurazione di una stampante collegata a una porta parallela
    -   Configurazione di una stampante collegata a una porta seriale
    -   Visualizzazione dello stato di una stampante
    -   Preparazione di una stampante per il cambio di pagina di codice

## <a name="BKMK_examples"></a>Esempi

Per inviare il file di report. txt nella directory corrente a una stampante collegata alla porta LPT2 nel computer locale, digitare:
```
print /d:lpt2 report.txt
```
Per inviare il file di report. txt nella directory c:\Accounting Benché "printer1" coda di stampa \\ \\copisteria server, digitare:
```
print /d:\\copyroom\printer1 c:\accounting\report.txt 
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)

[Riferimenti ai comandi di stampa](print-command-reference.md)

[Modalità](mode.md)