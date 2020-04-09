---
title: type
description: Windows Commands argomento per Type, che Visualizza il contenuto di un file di testo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c44fe905-a865-4c97-8cc5-fb95fec7d4d5
author: coreyp-at-msft
ms.author: coreyp
manager: dansimp
ms.openlocfilehash: 3163601d118df315edcae540917313703f677d52
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832414"
---
# <a name="type"></a>type

Nella shell dei comandi di Windows, **digitare** è un comando incorporato che Visualizza il contenuto di un file di testo. Usare il comando **Type** per visualizzare un file di testo senza modificarlo.

In PowerShell **digitare** un alias predefinito per il cmdlet **[Get-Content](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/get-content)** , che visualizza anche il contenuto di un file, ma con una sintassi diversa.

Per esempi relativi all'uso di questo comando all'interno della shell dei comandi di Windows (cmd. exe), vedere [esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
type [<Drive>:][<Path>]<FileName>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[\<unità >:] [\<percorso >]\<FileName >|Specifica il percorso e il nome del file o dei file che si desidera visualizzare. Separare più nomi di file con uno spazio.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Se *filename* contiene spazi, racchiuderlo tra virgolette (ad esempio, il nome file contenente Spaces. txt).
-   Se si visualizza un file binario o un file creato da un programma, è possibile visualizzare caratteri strani sullo schermo, inclusi i caratteri AvanzMod e i simboli della sequenza di escape. Questi caratteri rappresentano i codici di controllo usati nel file binario. In generale, evitare di usare il comando **Type** per visualizzare i file binari.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per visualizzare il contenuto di un file denominato Festiv. Mar, digitare:
```
type holiday.mar 
```
Per visualizzare il contenuto di un file lungo denominato Holiday. Mar una schermata alla volta, digitare:
```
type holiday.mar | more 
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
