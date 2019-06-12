---
title: Utilizzando il comando get-DriverPackageFile
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f01a2c67-7e9c-4aad-b625-383f5a1fca25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 264bdb6d51622e6323be00b44014b86cd9662e61
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440507"
---
# <a name="using-the-get-driverpackagefile-command"></a>Utilizzando il comando get-DriverPackageFile



Visualizza informazioni su un pacchetto driver, inclusi i driver e file in che esso contenuti.

## <a name="syntax"></a>Sintassi

```
WDSUTIL /Get-DriverPackageFile /InfFile:<Inf File path> [/Architecture:{x86 | ia64 | x64}] [/Show:{Drivers | Files | All}]
```

## <a name="parameters"></a>Parametri

|         Parametro         |                              Descrizione                               |
|---------------------------|------------------------------------------------------------------------|
| / InfFile:\<percorso del Inf File > | Specifica il nome di file e percorso completo del file di pacchetto di driver inf. |
|    [/Architecture:{x86    |                                  ia64                                  |
|     [/Show: {driver      |                                 File                                  |

## <a name="BKMK_examples"></a>Esempi

Per visualizzare informazioni su un file di driver, digitare:
```
WDSUTIL /Get-DriverPackageFile /InfFile:"C:\temp\1394.inf" /Architecture:x86
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)