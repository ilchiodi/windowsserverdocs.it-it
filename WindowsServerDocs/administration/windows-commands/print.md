---
title: print
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: ada0657e2f17754e55e97e6488aac99fb0025afb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372148"
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
|/d: \<PrinterName >|Specifica la stampante in cui si desidera stampare il processo. Per stampare su una stampante connessa localmente, specificare la porta nel computer in cui è connessa la stampante.</br>-I valori validi per le porte parallele sono LPT1, LPT2 e LPT3.</br>-I valori validi per le porte seriali sono COM1, COM2, COM3 e COM4.</br>È anche possibile specificare una stampante di rete usando il nome della coda (\\ @ no__t-1*ServerName*\*PrinterName *). Se non si specifica una stampante, per impostazione predefinita il processo di stampa viene inviato a LPT1.|
|> \<Drive:|Specifica l'unità logica o fisica in cui si trova il file che si desidera stampare. Questo parametro non è obbligatorio se il file che si desidera stampare si trova nell'unità corrente.|
|\<Path >|Specifica il percorso del file che si desidera stampare. Questo parametro non è obbligatorio se il file che si desidera stampare si trova nella directory corrente.|
|\<FileName > [...]|Obbligatorio. Specifica il file che si desidera stampare. È possibile includere più file in un unico comando.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Un file può essere stampato in background se viene inviato a una stampante connessa a una porta seriale o parallela sul computer locale.
-   È possibile eseguire molte attività di configurazione dal prompt dei comandi usando il comando **mode** .

    Per ulteriori informazioni su, vedere la [modalità](mode.md) :  
    -   Configurazione di una stampante connessa a una porta parallela
    -   Configurazione di una stampante connessa a una porta seriale
    -   Visualizzazione dello stato di una stampante
    -   Preparazione di una stampante per il cambio di tabella codici

## <a name="BKMK_examples"></a>Esempi

Per inviare il file report. txt nella directory corrente a una stampante connessa a LPT2 nel computer locale, digitare:
```
print /d:lpt2 report.txt
```
Per inviare il file report. txt nella directory c:\Accounting alla coda di stampa Printer1 nel server \\ @ no__t-1CopyRoom, digitare:
```
print /d:\\copyroom\printer1 c:\accounting\report.txt 
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

[Informazioni di riferimento sui comandi di stampa](print-command-reference.md)

[Modalità](mode.md)