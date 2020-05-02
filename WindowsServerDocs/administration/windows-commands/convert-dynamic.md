---
title: Converti dinamica
description: Argomento di riferimento per il comando Convert Dynamic, che converte un disco di base in un disco dinamico.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7b8fa4b1-850f-4e48-b05f-871c883ea33c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 05d507fb5a1f8ac3ca8d8899249a26dee496ed2a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720784"
---
# <a name="convert-dynamic"></a>Converti dinamica

Converte un disco di base in un disco dinamico. Per eseguire questa operazione, è necessario selezionare un disco di base. Usare il [comando Seleziona disco](select-disk.md) per selezionare un disco di base e spostare lo stato attivo a esso.

> [!NOTE]
> Per istruzioni sull'uso di questo comando, vedere [riportare un disco dinamico in un disco di base](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc755238(v=ws.11)).

## <a name="syntax"></a>Sintassi

```
convert dynamic [noerr]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| NOERR | Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore. |

#### <a name="remarks"></a>Osservazioni

- Tutte le partizioni esistenti sul disco di base diventano volumi semplici.

## <a name="examples"></a>Esempi

Per convertire un disco di base in un disco dinamico, digitare:

```
convert dynamic
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Convert (comando)](convert.md)
