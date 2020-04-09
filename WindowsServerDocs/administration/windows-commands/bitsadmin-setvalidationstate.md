---
title: setvalidationstate Bitsadmin
description: Windows Commands Topic for Bitsadmin setvalidationstate, che imposta lo stato di convalida del contenuto del file specificato all'interno del processo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e8fc8e8c-171c-4681-8057-6986b018e576
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: de6480596b55b3a483076297f32ce52a975915db
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849114"
---
# <a name="bitsadmin-setvalidationstate"></a>setvalidationstate Bitsadmin

Imposta lo stato di convalida del contenuto del file specificato all'interno del processo.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetValidationState <Job> <file index> <true|false> 
```

### <a name="parameters"></a>Parametri

| Parametro  |          Descrizione           |
|------------|--------------------------------|
|    Job     | Nome visualizzato o il GUID del processo |
| Indice file |         Inizia da 0          |
|    True    |             False              |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

L'esempio seguente imposta lo stato di convalida del contenuto del file 2 su TRUE per il processo denominato *il mio lavoro*.
```
C:\>bitsadmin /SetValidationState myJob 2 TRUE 
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)