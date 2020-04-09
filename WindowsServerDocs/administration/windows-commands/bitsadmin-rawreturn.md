---
title: bitsadmin rawreturn
description: Windows Commands Topic for **BITSAdmin rawreturn**, che restituisce i dati appropriati per l'analisi.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bbe97130-26f6-4cdd-84f1-baf530ce38b7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e8cd84b6082b1fda8061ee7b324dd66991d3b206
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849884"
---
# <a name="bitsadmin-rawreturn"></a>bitsadmin rawreturn

Restituisce i dati appropriati per l'analisi. In genere, si usa questo comando insieme alle opzioni **/create** e **/Get*** per ricevere solo il valore. È necessario specificare questa opzione prima di altre opzioni.

## <a name="syntax"></a>Sintassi

```
bitsadmin /rawreturn
```

## <a name="remarks"></a>Note

- Rimuove i caratteri di nuova riga e la formattazione dall'output.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente recupera i dati non elaborati per lo stato del processo denominato *myDownloadJob*.

```
C:\>bitsadmin /rawreturn /getstate myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)