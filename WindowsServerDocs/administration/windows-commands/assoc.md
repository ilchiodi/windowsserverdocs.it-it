---
title: assoc
description: Argomento i comandi di Windows per **assoc** -Visualizza o modifica delle associazioni di estensione nome file.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 237bedda-b24c-4fec-a39c-9b7eacf96417
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d893167081b66c81366b59613c52182a4ddba370
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816642"
---
# <a name="assoc"></a>assoc



Visualizza o modifica delle associazioni di estensione nome file. Se utilizzata senza parametri, **assoc** Visualizza un elenco delle associazioni corrente delle estensioni dei file.

> [!NOTE]
> Questo comando è supportato solo all'interno di CMD. File EXE e non è disponibile da PowerShell.
>

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
assoc [<.ext>[=[<FileType>]]]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|<.ext>|Specifica l'estensione del nome file.|
|\<FileType>|Specifica il tipo di file da associare l'estensione del nome file specificato.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Per rimuovere l'associazione del tipo di file per un'estensione di file, aggiungere uno spazio vuoto dopo il segno di uguale, premere la barra spaziatrice.
-   Per visualizzare i tipi di file corrente con le stringhe di comando di apertura definite, usare il **ftype** comando.
-   Per reindirizzare l'output del **assoc** in un file di testo, utilizzare il **>** operatore di reindirizzamento.

## <a name="BKMK_examples"></a>Esempi

Per visualizzare l'associazione di tipo file corrente per l'estensione txt, digitare:
```
assoc .txt
```
Per rimuovere l'associazione del tipo di file per l'estensione bak nome file, digitare:
```
assoc .bak= 
```

> [!NOTE]
> Assicurarsi di aggiungere uno spazio dopo il segno di uguale.

Per visualizzare l'output di **assoc** una schermata alla volta, tipo:
```
assoc | more
```
Per inviare l'output di **assoc** a assoc.txt il file, digitare:
```
assoc>assoc.txt
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)
