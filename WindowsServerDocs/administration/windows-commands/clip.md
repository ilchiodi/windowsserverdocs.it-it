---
title: clip
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 85322d85-3376-4806-845b-93ac77fe27bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 82186869782c47f41930d46b4c33a710e6addedf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379334"
---
# <a name="clip"></a>clip



Reindirizza l'output del comando dalla riga di comando negli Appunti di Windows. È quindi possibile incollare l'output di testo in altri programmi.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
<Command> | clip
clip < <FileName>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Command >|Specifica un comando il cui output si desidera inviare negli Appunti di Windows.|
|\<> FileName|Specifica un file il cui contenuto si desidera inviare negli Appunti di Windows.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

È possibile utilizzare il **clip** comando per copiare i dati direttamente in un'applicazione in grado di ricevere il testo dagli Appunti.

## <a name="BKMK_examples"></a>Esempi

Per copiare la directory corrente listato negli Appunti di Windows, digitare:
```
dir | clip
```
Per copiare l'output di un programma denominato Generic. awk negli Appunti di Windows, digitare:
```
awk -f generic.awk input.txt | clip
```
Per copiare il contenuto di un file denominato Readme. txt negli Appunti di Windows, digitare:
```
clip < readme.txt
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)