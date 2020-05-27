---
title: goto
description: Argomento di riferimento per il comando goto, che indirizza cmd. exe a una riga con etichetta in un programma batch.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e0de1458-1f78-48ff-a746-c285a945a510
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1eb1b6b275887de535614fa5df4adabe33406a31
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83818971"
---
# <a name="goto"></a>goto

Indirizza cmd.exe a una riga con etichetta in un file batch. All'interno di un programma batch, questo comando indirizza l'elaborazione del comando a una riga identificata da un'etichetta. Quando viene trovato l'etichetta, l'elaborazione continua a partire da quelli che iniziano nella riga successiva.

## <a name="syntax"></a>Sintassi

```
goto <label>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<label>` | Specifica una stringa di testo che viene utilizzata come etichetta nel programma batch. |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Osservazioni

-  Se sono abilitate le estensioni dei comandi (impostazione predefinita) e si usa il comando **goto** con un'etichetta di destinazione **: EOF**, si trasferisce il controllo alla fine del file di script batch corrente e si chiude il file di script batch senza definire un'etichetta. Quando si usa questo comando con l'etichetta **: EOF** , è necessario inserire i due punti prima dell'etichetta. Ad esempio: `goto:EOF`.

- È possibile usare gli spazi nel parametro *Label* , ma non è possibile includere altri separatori, ad esempio punti e virgola (;) o segni di uguale (=)).

- Il valore dell' *etichetta* specificato deve corrispondere a un'etichetta nel programma batch. L'etichetta all'interno del programma batch deve iniziare con due punti (:). Se una riga inizia con i due punti, viene considerata come un'etichetta e tutti i comandi della riga vengono ignorati. Se il programma batch non contiene l'etichetta specificata nel parametro *Label* , il programma batch viene arrestato e viene visualizzato il messaggio seguente: `Label not found` .

- È possibile utilizzare **goto** con altri comandi per eseguire operazioni condizionali. Per ulteriori informazioni sull'utilizzo di **goto** per le operazioni condizionali, vedere il [comando if](if.md).

## <a name="examples"></a>Esempi

Il programma batch seguente formatta un disco nell'unità come disco di sistema. Se l'operazione ha esito positivo, il **goto** comando indirizza l'elaborazione di **: fine** etichetta:

```
echo off
format a: /s
if not errorlevel 1 goto end
echo An error occurred during formatting.
:end
echo End of batch program.
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando cmd](cmd.md)

- [comando if](if.md)