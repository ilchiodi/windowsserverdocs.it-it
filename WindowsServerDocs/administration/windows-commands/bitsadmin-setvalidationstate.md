---
title: setvalidationstate Bitsadmin
description: Argomento dei comandi di Windows per **BITSAdmin setvalidationstate** -imposta lo stato di convalida del contenuto del file specificato all'interno del processo.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 37d7fa3a8a91abf1e7b6ac5a51b6cebd78984a91
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380403"
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
| Indice file |         Inizia da 0          |
|    True    |             False              |

## <a name="BKMK_examples"></a>Esempi

L'esempio seguente imposta lo stato di convalida del contenuto del file 2 su TRUE per il processo denominato *il mio lavoro*.
```
C:\>bitsadmin /SetValidationState myJob 2 TRUE 
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)