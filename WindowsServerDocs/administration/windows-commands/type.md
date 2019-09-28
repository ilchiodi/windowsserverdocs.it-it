---
title: type
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c44fe905-a865-4c97-8cc5-fb95fec7d4d5
author: coreyp-at-msft
ms.author: coreyp
manager: dansimp
ms.openlocfilehash: 37f66d54983c002d5d09db5cb255d01635a534de
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392324"
---
# <a name="type"></a>type


Nella shell dei comandi di Windows, **digitare** è un comando incorporato che Visualizza il contenuto di un file di testo. Usare il comando **Type** per visualizzare un file di testo senza modificarlo.


In PowerShell **digitare** un alias predefinito per il cmdlet **[Get-Content](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/get-content)** , che visualizza anche il contenuto di un file, ma con una sintassi diversa.


Per esempi relativi all'uso di questo comando all'interno della shell dei comandi di Windows (cmd. exe), vedere [esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
type [<Drive>:][<Path>]<FileName>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[\<Drive >:] [\<Path >] \<FileName >|Specifica il percorso e il nome del file o dei file che si desidera visualizzare. Separare più nomi di file con uno spazio.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Se *filename* contiene spazi, racchiuderlo tra virgolette (ad esempio, "nome file contenente Spaces. txt").
-   Se si visualizza un file binario o un file creato da un programma, è possibile visualizzare caratteri strani sullo schermo, inclusi i caratteri AvanzMod e i simboli della sequenza di escape. Questi caratteri rappresentano i codici di controllo usati nel file binario. In generale, evitare di usare il comando **Type** per visualizzare i file binari.

## <a name="BKMK_examples"></a>Esempi

Per visualizzare il contenuto di un file denominato Festiv. Mar, digitare:
```
type holiday.mar 
```
Per visualizzare il contenuto di un file lungo denominato Holiday. Mar una schermata alla volta, digitare:
```
type holiday.mar | more 
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
