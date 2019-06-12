---
title: bitsadmin rawreturn
description: Argomento i comandi di Windows per **bitsadmin rawreturn** -restituisce i dati appropriati per l'analisi.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4e12c8e621021d35ac618b4592515fe38c36be0e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434895"
---
# <a name="bitsadmin-rawreturn"></a>bitsadmin rawreturn

Restituisce i dati appropriati per l'analisi.

## <a name="syntax"></a>Sintassi

```
bitsadmin /RawReturn
```

## <a name="remarks"></a>Note

Caratteri di nuova riga strisce e la formattazione dell'output.

In genere, si utilizza questo comando in combinazione con il **Create** e **ottenere\\** * commutatori per ricevere solo il valore. È necessario specificare questa opzione prima di altre opzioni.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente recupera i dati non elaborati per lo stato del processo denominato *myDownloadJob*.
```
C:\>bitsadmin /RawReturn /GetState myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)