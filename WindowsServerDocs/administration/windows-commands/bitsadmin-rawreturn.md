---
title: bitsadmin rawreturn
description: Argomento di riferimento per il comando Bitsadmin rawreturn, che restituisce i dati appropriati per l'analisi.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bbe97130-26f6-4cdd-84f1-baf530ce38b7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: af465bb9f51ab6f43980c43bf2be1f5158429a82
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717090"
---
# <a name="bitsadmin-rawreturn"></a>bitsadmin rawreturn

Restituisce i dati appropriati per l'analisi. In genere, si usa questo comando insieme alle opzioni **/create** e **/Get*** per ricevere solo il valore. È necessario specificare questa opzione prima di altre opzioni.

> [!NOTE]
> Questo comando rimuove i caratteri di nuova riga e la formattazione dall'output.

## <a name="syntax"></a>Sintassi

```
bitsadmin /rawreturn
```

## <a name="examples"></a>Esempi

Per recuperare i dati non elaborati per lo stato del processo denominato *myDownloadJob*:

```
bitsadmin /rawreturn /getstate myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
