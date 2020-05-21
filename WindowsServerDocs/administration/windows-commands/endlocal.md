---
title: endlocal
description: Argomento di riferimento per il comando endlocal, che termina la localizzazione delle modifiche dell'ambiente in un file batch, e ripristina le variabili di ambiente nei rispettivi valori prima dell'esecuzione del comando setlocale corrispondente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 765fae3c-0c0a-4639-99a4-cf613489b949
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 229914ddbfa7361738cad79903630be9e749c795
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436886"
---
# <a name="endlocal"></a>endlocal

Termina la localizzazione delle modifiche all'ambiente in un file batch e ripristinare i valori delle variabili di ambiente prima della corrispondente **setlocal** comando è stato eseguito.

## <a name="syntax"></a>Sintassi

```
endlocal
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Osservazioni

- Il **endlocal** comando non ha alcun effetto all'esterno di un file batch o script.

- È implicita **endlocal** comando alla fine di un file batch.

- Se sono abilitate le estensioni dei comandi (estensioni di comando sono abilitate per impostazione predefinita), il **endlocal** comando Ripristina lo stato delle estensioni (vale a dire, abilitato o disabilitato) in vigore prima della corrispondente **setlocal** comando è stato eseguito.

> [!NOTE]
> Per ulteriori informazioni sull'abilitazione e la disabilitazione delle estensioni del comando, vedere il [comando cmd](cmd.md).

### <a name="examples"></a>Esempi

È possibile localizzare le variabili di ambiente in un file batch. Ad esempio, il programma seguente avvia il programma batch *superapp* in rete, indirizza l'output a un file e visualizza il file nel blocco note:

```
@echo off
setlocal
path=g:\programs\superapp;%path%
call superapp>c:\superapp.out
endlocal
start notepad c:\superapp.out
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
