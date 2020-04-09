---
title: clip
description: Windows Commands argomento for clip, che reindirizza l'output del comando dalla riga di comando agli Appunti di Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 85322d85-3376-4806-845b-93ac77fe27bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0d997154a382cf39aa2b877d7a2b84f4ff34157d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847644"
---
# <a name="clip"></a>clip

Reindirizza l'output del comando dalla riga di comando agli Appunti di Windows. È quindi possibile incollare l'output di testo in altri programmi.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
<Command> | clip
clip < <FileName>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Comando \<>|Specifica un comando il cui output si desidera inviare negli Appunti di Windows.|
|\<FileName >|Specifica un file il cui contenuto si desidera inviare negli Appunti di Windows.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

È possibile utilizzare il **clip** comando per copiare i dati direttamente in un'applicazione in grado di ricevere il testo dagli Appunti.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

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

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)