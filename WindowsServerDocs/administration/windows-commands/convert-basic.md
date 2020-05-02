---
title: convert basic
description: Argomento di riferimento per il comando Convert Basic, che converte un disco dinamico vuoto in un disco di base.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 61329896-3b56-4959-8d58-45cbe18ba860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e44ecc9f5d18bbe426c63f8854e7c3347f418bb2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720787"
---
# <a name="convert-basic"></a>convert basic

Converte un disco dinamico vuoto in un disco di base. Per eseguire questa operazione, è necessario selezionare un disco dinamico. Usare il [comando Seleziona disco](select-disk.md) per selezionare un disco dinamico e spostare lo stato attivo a esso.

> [!IMPORTANT]
> Il disco deve essere vuoto per convertirlo in un disco di base. Eseguire il backup dei dati e quindi eliminare tutte le partizioni o i volumi prima di convertire il disco.

> [!NOTE]
> Per istruzioni sull'uso di questo comando, vedere [riportare un disco dinamico in un disco di base](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc755238(v=ws.11)).

## <a name="syntax"></a>Sintassi

```
convert basic [noerr]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| NOERR | Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore. |

## <a name="examples"></a>Esempi

Per convertire il disco dinamico selezionato in Basic, digitare:

```
convert basic
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Convert (comando)](convert.md)
