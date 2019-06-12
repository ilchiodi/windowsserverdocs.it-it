---
title: setvalidationstate Bitsadmin
description: Argomento i comandi di Windows per **bitsadmin setvalidationstate** -imposta lo stato di convalida del contenuto del file specificato all'interno del processo.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e8fc8e8c-171c-4681-8057-6986b018e576
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a832e8f3d21681f67a4486df33c387e5a8456718
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434875"
---
# <a name="bitsadmin-setvalidationstate"></a>setvalidationstate Bitsadmin



Imposta lo stato di convalida del contenuto del file specificato all'interno del processo.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetValidationState <Job> <file index> <true|false> 
```

## <a name="parameters"></a>Parametri

| Parametro  |          Descrizione           |
|------------|--------------------------------|
|    Job     | Nome visualizzato o il GUID del processo |
| Indice di file |         Inizia da 0          |
|    True    |             False              |

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente imposta lo stato di convalida del contenuto del file 2 su TRUE per il processo denominato *myJob*.
```
C:\>bitsadmin /SetValidationState myJob 2 TRUE 
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)