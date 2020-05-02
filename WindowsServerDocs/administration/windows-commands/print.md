---
title: print
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aa2325d5-a993-4ed3-b996-255165452db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8937253da730c2ab77ff03cdeb9f31d24e608eab
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722860"
---
# <a name="print"></a>print



Invia un file di testo a una stampante.



## <a name="syntax"></a>Sintassi

```
Print [/d:<PrinterName>] [<Drive>:][<Path>]<FileName>[ ...]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/d:\<printername>|Specifica la stampante in cui si desidera stampare il processo. Per stampare su una stampante connessa localmente, specificare la porta nel computer in cui è connessa la stampante.</br>-I valori validi per le porte parallele sono LPT1, LPT2 e LPT3.</br>-I valori validi per le porte seriali sono COM1, COM2, COM3 e COM4.</br>È inoltre possibile specificare una stampante di rete utilizzando il nome della coda\\\\(*ServerName*\*printerName *). Se non si specifica una stampante, per impostazione predefinita il processo di stampa viene inviato a LPT1.|
|\<> unità:|Specifica l'unità logica o fisica in cui si trova il file che si desidera stampare. Questo parametro non è obbligatorio se il file che si desidera stampare si trova nell'unità corrente.|
|\<> percorso|Specifica il percorso del file che si desidera stampare. Questo parametro non è obbligatorio se il file che si desidera stampare si trova nella directory corrente.|
|\<Nomefile> [...]|Obbligatorio. Specifica il file che si desidera stampare. È possibile includere più file in un unico comando.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

-   Un file può essere stampato in background se viene inviato a una stampante connessa a una porta seriale o parallela sul computer locale.
-   È possibile eseguire molte attività di configurazione dal prompt dei comandi usando il comando **mode** .

    Per ulteriori informazioni su, vedere la [modalità](mode.md) :  
    -   Configurazione di una stampante connessa a una porta parallela
    -   Configurazione di una stampante connessa a una porta seriale
    -   Visualizzazione dello stato di una stampante
    -   Preparazione di una stampante per il cambio di tabella codici

## <a name="examples"></a>Esempi

Per inviare il file report. txt nella directory corrente a una stampante connessa a LPT2 nel computer locale, digitare:
```
print /d:lpt2 report.txt
```
Per inviare il file report. txt nella directory c:\Accounting alla coda di stampa Printer1 sul \\ \\server CopyRoom, digitare:
```
print /d:\\copyroom\printer1 c:\accounting\report.txt 
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

[Informazioni di riferimento sui comandi di stampa](print-command-reference.md)

[Modalità](mode.md)