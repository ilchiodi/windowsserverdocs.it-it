---
title: del
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 346eede2-2085-44f5-9936-6877b5d5a833
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6e569443a56646862c7a2c9fbd2c599cede941a1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378698"
---
# <a name="del"></a>del



Elimina uno o più file. Questo comando è analogo a come il **erase** comando.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
del [/p] [/f] [/s] [/q] [/a[:]<Attributes>] <Names>
erase [/p] [/f] [/s] [/q] [/a[:]<Attributes>] <Names>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Names >|Specifica un elenco di uno o più file o directory. Per eliminare più file, è possibile utilizzare caratteri jolly. Se viene specificata una directory, verranno eliminati tutti i file all'interno della directory.|
|/ p|Richiede una conferma prima di eliminare il file specificato.|
|/f|Eliminazione di forza dei file di sola lettura.|
|/s|Elimina i file dalla directory corrente e tutte le sottodirectory specificati. Visualizza i nomi dei file di come vengono eliminati.|
|/q|Specifica la modalità non interattiva. Non viene chiesto di confermare l'eliminazione.|
|/a [:] \<Attributes >|Elimina i file in base ai seguenti attributi di file:</br>**r** i file di sola lettura</br>**h** file nascosti</br>**i** non contenuti i file indicizzati</br>**s** i file di sistema</br>**un** pronto per l'archiviazione dei file</br>**l** Reparse Point</br>-Prefisso vale a dire 'not'|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

> [!CAUTION]
> Se si utilizza **CANC** per eliminare un file dal disco, non è possibile recuperarlo.
> -   Se si usa **/p**, **Canc** Visualizza il nome di un file e invia il messaggio seguente:

    `FileName, Delete (Y/N)?`

    To confirm the deletion, press Y. To cancel the deletion and display the next file name (that is, if you specified a group of files), press N. To stop the **del** command, press CTRL+C.
- Se si disabilita le estensioni dei comandi, **/s** vengono visualizzati i nomi di tutti i file che non sono stati trovati anziché visualizzare i nomi dei file da eliminare (ovvero, il comportamento viene invertito).
- Se si specifica una cartella in *nomi*, vengono eliminati tutti i file nella cartella. Ad esempio, il seguente comando Elimina tutti i file nella cartella \Work:  
  ```
  del \work
  ```  
- È possibile utilizzare caratteri jolly ( **&#42;** e **?** ) per eliminare più di un file alla volta. Tuttavia, per evitare di eliminare accidentalmente dei file, è necessario utilizzare caratteri jolly con cautela con il **CANC** comando. Ad esempio, se si digita il comando seguente:  
  ```
  del *.*
  ```  
  Il **Canc** comando Visualizza il messaggio seguente:

  `Are you sure (Y/N)?`

  Per eliminare tutti i file nella directory corrente, premere Y e quindi premere INVIO. Per annullare l'eliminazione, premere N, quindi premere INVIO.

> [!NOTE]
> Prima di utilizzare caratteri jolly con il **CANC** comando, utilizzare gli stessi caratteri jolly con il **dir** comando per elencare tutti i file che verranno eliminati.
> -   Il **CANC** comando con parametri diversi, è disponibile dalla Console di ripristino.

## <a name="BKMK_examples"></a>Esempi

Per eliminare tutti i file in una cartella denominata Test sull'unità C, digitare uno dei seguenti:
```
del c:\test
del c:\test\*.*
```
Per eliminare tutti i file con estensione. bat dalla directory corrente, digitare:
```
del *.bat
```
Per eliminare tutti i file di sola lettura nella directory corrente, digitare:
```
del /a:r *.*
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
