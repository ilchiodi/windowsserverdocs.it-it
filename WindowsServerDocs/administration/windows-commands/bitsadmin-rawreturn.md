---
title: bitsadmin rawreturn
description: Argomento comandi di Windows per **BITSAdmin rawreturn** -restituisce i dati appropriati per l'analisi.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bbe97130-26f6-4cdd-84f1-baf530ce38b7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 86d769de460538acda696194348980de5752d6d8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380882"
---
# <a name="bitsadmin-rawreturn"></a>bitsadmin rawreturn

Restituisce i dati appropriati per l'analisi.

## <a name="syntax"></a>Sintassi

```
bitsadmin /RawReturn
```

## <a name="remarks"></a>Note

Rimuove i caratteri di nuova riga e la formattazione dall'output.

In genere, si usa questo comando insieme alle opzioni **create** e **Get @ no__t-2*** per ricevere solo il valore. È necessario specificare questa opzione prima di altre opzioni.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente recupera i dati non elaborati per lo stato del processo denominato *myDownloadJob*.
```
C:\>bitsadmin /RawReturn /GetState myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)