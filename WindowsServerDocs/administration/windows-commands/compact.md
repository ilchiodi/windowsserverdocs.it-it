---
title: compatto
description: Windows Commands (comandi di Windows) per Compact, che consente di visualizzare o modificare la compressione di file o directory in partizioni NTFS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 429b3752-df0a-43a4-a210-df2f3ad03c3b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e9e6a3ba71ecc0c8e264ac4af8dc1da42d23fdc2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847354"
---
# <a name="compact"></a>compatto

Consente di visualizzare o modificare la compressione di file o directory in partizioni NTFS. Se utilizzata senza parametri, **Compact** Visualizza lo stato di compressione della directory corrente e dei file in essa contenuti.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
compact [/c | /u] [/s[:<Dir>]] [/a] [/i] [/f] [/q] [<FileName>[...]]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/c|Comprime la directory o il file specificato.|
|/u|Decomprime la directory o il file specificato.|
|/s [:\<dir >]|Applica il comando **Compact** a tutte le sottodirectory della directory specificata (o della directory corrente se non ne viene specificato nessuno).|
|/a|Visualizza i file nascosti o di sistema.|
|/i|Ignora gli errori.|
|/f|Forza la compressione o la decompressione del file o della directory specificata. **/f** viene usato nel caso di un file che è stato parzialmente compresso quando l'operazione è stata interrotta da un arresto anomalo del sistema. Per forzare la compressione del file nel suo complesso, usare i parametri **/c** e **/f** e specificare il file parzialmente compresso.|
|/q|Segnala solo le informazioni più importanti.|
|\<FileName >|Specifica il file o la directory. È possibile usare più nomi file e **&#42;** e **?** caratteri jolly.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Il comando **Compact** è la versione da riga di comando della funzionalità di compressione file system NTFS. Lo stato di compressione di una directory indica se i file vengono compressi automaticamente al momento dell'aggiunta alla directory. L'impostazione dello stato di compressione di una directory non comporta necessariamente la modifica dello stato di compressione dei file già presenti nella directory.
-   Non è possibile usare **Compact** per leggere, scrivere o montare volumi compressi con DriveSpace o DoubleSpace.
-   Non è possibile usare **Compact** per comprimere le partizioni FAT (file allocation table) o FAT32.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per impostare lo stato di compressione della directory corrente, delle relative sottodirectory e dei file esistenti, digitare:
```
compact /c /s 
```
Per impostare lo stato di compressione dei file e delle sottodirectory nella directory corrente, senza modificare lo stato di compressione della directory corrente, digitare:
```
compact /c /s *.*
```
Per comprimere un volume, dalla directory radice del volume, digitare:
```
compact /c /i /s:\
```

> [!NOTE]
> Questo esempio imposta lo stato di compressione di tutte le directory (inclusa la directory radice nel volume) e comprime tutti i file nel volume. Il parametro **/i** impedisce che i messaggi di errore interrompano il processo di compressione.

Per comprimere tutti i file con estensione bmp nella directory \Tmp e in tutte le sottodirectory di \Tmp, senza modificare l'attributo compresso delle directory, digitare:
```
compact /c /s:\tmp *.bmp
```
Per forzare la compressione completa del file Zebra. bmp, che è stato parzialmente compresso durante un arresto anomalo del sistema, digitare:
```
compact /c /f zebra.bmp
```
Per rimuovere l'attributo compresso dalla directory C:\Tmp, senza modificare lo stato di compressione di tutti i file nella directory, digitare:
```
compact /u c:\tmp
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)