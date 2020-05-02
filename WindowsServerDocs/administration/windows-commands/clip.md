---
title: clip
description: Argomento di riferimento per il comando di ritaglio, che reindirizza l'output del comando dalla riga di comando agli Appunti di Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 85322d85-3376-4806-845b-93ac77fe27bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 61c905e3dcce52f3a3d35adeac55fc5df574f664
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82712787"
---
# <a name="clip"></a>clip

Reindirizza l'output del comando dalla riga di comando agli Appunti di Windows. È possibile utilizzare questo comando per copiare i dati direttamente in qualsiasi applicazione in grado di ricevere testo dagli Appunti. È anche possibile incollare questo output di testo in altri programmi.

## <a name="syntax"></a>Sintassi

```
<command> | clip
clip < <filename>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<command>` | Specifica un comando il cui output si desidera inviare agli Appunti di Windows. |
| `<filename>` | Specifica un file il cui contenuto si desidera inviare agli Appunti di Windows. |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="examples"></a>Esempi

Per copiare la directory corrente listato negli Appunti di Windows, digitare:

```
dir | clip
```

Per copiare l'output di un programma denominato *Generic. awk* negli Appunti di Windows, digitare:

```
awk -f generic.awk input.txt | clip
```

Per copiare il contenuto di un file denominato *Readme. txt* negli Appunti di Windows, digitare:

```
clip < readme.txt
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)