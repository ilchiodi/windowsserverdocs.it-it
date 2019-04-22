---
title: compact
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 429b3752-df0a-43a4-a210-df2f3ad03c3b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 068111b293a3eb3987b14744a1bfcf2fde26bced
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826032"
---
# <a name="compact"></a>compact



Visualizza o modifica della compressione di file o directory partizioni NTFS. Se utilizzata senza parametri, **compact** Visualizza lo stato di compressione della directory corrente e i file in essa contenuti.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
compact [/c | /u] [/s[:<Dir>]] [/a] [/i] [/f] [/q] [<FileName>[...]]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/c|Comprime la directory specificata o un file.|
|/u|Decomprime la directory specificata o un file.|
|/s[:\<Dir>]|Si applica il **compact** comando a tutte le sottodirectory della directory specificata (o della directory corrente se non è specificato).|
|/a|Vengono visualizzati nascosti o i file di sistema.|
|/i|Ignora gli errori.|
|/f|Impone la compressione o decompressione del file o directory specificata. **/f** viene usato nel caso di un file che è stata compressa in parte durante l'operazione è stata interrotta da un arresto anomalo del sistema. Per forzare il file deve essere compresso nel suo complesso, usare il **/c** e **/f** parametri e specificare il file parzialmente compresso.|
|/q|Segnala solo le informazioni più importanti.|
|\<FileName>|Specifica il file o directory. È possibile usare più nomi di file e il **&#42;** e **?** caratteri jolly.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Il **compact** comando è la versione della riga di comando della funzionalità di compressione del file system NTFS. Lo stato della compressione di una directory indica se i file vengono compressi automaticamente quando vengono aggiunti alla directory. L'impostazione dello stato di compressione di una directory non cambia necessariamente lo stato della compressione dei file che si trovano già nella directory.
-   Non è possibile usare **compact** lettura, scrittura o montare i volumi che sono state compresse con DriveSpace o DoubleSpace.
-   Non è possibile usare **compact** per comprimere i file allocation table (FAT) o partizioni FAT32.

## <a name="BKMK_examples"></a>Esempi

Per impostare lo stato della compressione della directory corrente, le sottodirectory e i file esistenti, digitare:
```
compact /c /s 
```
Per impostare lo stato della compressione dei file e le sottodirectory della directory corrente, senza modificare lo stato della compressione della directory corrente stesso, digitare:
```
compact /c /s *.*
```
Per comprimere un volume, dalla directory radice del volume, digitare:
```
compact /c /i /s:\
```

> [!NOTE]
> In questo esempio imposta lo stato della compressione di tutte le directory (compresa la directory radice del volume) comprime tutti i file nel volume. Il **/i** parametro impedisce i messaggi di errore di interrompere il processo di compressione.

Per comprimere tutti i file con estensione bmp nella cartella \Tmp e tutte le sottodirectory della \Tmp, senza modificare l'attributo di compressione delle directory, digitare:
```
compact /c /s:\tmp *.bmp
```
Per forzare la compressione completata del file, parzialmente ZEBRA compresso causa durante un arresto anomalo del sistema, digitare:
```
compact /c /f zebra.bmp
```
Per rimuovere l'attributo di compressione dalla directory C:\Tmp, senza modificare lo stato della compressione di tutti i file in tale directory, digitare:
```
compact /u c:\tmp
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)