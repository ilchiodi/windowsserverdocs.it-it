---
title: assoc
description: Argomento dei comandi di Windows per **Assoc** -Visualizza o modifica le associazioni dell'estensione di file.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: e4a6fd700cbe66897a24f01f66387e76e07b568b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382674"
---
# <a name="assoc"></a>assoc



Visualizza o modifica le associazioni di estensione del nome file. Se usato senza parametri, **Assoc** Visualizza un elenco di tutte le associazioni di estensione del nome di file corrente.

> [!NOTE]
> Questo comando è supportato solo all'interno di CMD. EXE e non è disponibile da PowerShell.
>

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
assoc [<.ext>[=[<FileType>]]]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|<. ext >|Specifica l'estensione del nome file.|
|\<FileType >|Specifica il tipo di file da associare all'estensione del nome file specificata.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Per rimuovere l'associazione del tipo di file per un'estensione di file, aggiungere uno spazio vuoto dopo il segno di uguale premendo la barra SPAZIAtrice.
-   Per visualizzare i tipi di file correnti con stringhe di comando aperte definite, utilizzare il comando **ftype** .
-   Per reindirizzare l'output di **Assoc** a un file di testo, usare l'operatore di Reindirizzamento **>** .

## <a name="BKMK_examples"></a>Esempi

Per visualizzare l'associazione del tipo di file corrente per l'estensione txt del nome file, digitare:
```
assoc .txt
```
Per rimuovere l'associazione del tipo di file per l'estensione bak del nome file, digitare:
```
assoc .bak= 
```

> [!NOTE]
> Assicurarsi di aggiungere uno spazio dopo il segno di uguale.

Per visualizzare l'output di **Assoc** una schermata alla volta, digitare:
```
assoc | more
```
Per inviare l'output di **Assoc** al file Assoc. txt, digitare:
```
assoc>assoc.txt
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
