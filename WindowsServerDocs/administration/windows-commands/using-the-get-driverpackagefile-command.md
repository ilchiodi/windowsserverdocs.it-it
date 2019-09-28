---
title: Utilizzando il comando get-DriverPackageFile
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 21bbe17e56177da5cd2c1bf83c712d256cc794c8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363154"
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
| /InfFile: percorso file \<Inf > | Specifica il percorso completo e il nome file del file con estensione inf del pacchetto driver. |
|    [/Architettura: {x86    |                                  ia64                                  |
|     [/Show: {Drivers      |                                 File                                  |

## <a name="BKMK_examples"></a>Esempi

Per visualizzare le informazioni relative a un file di driver, digitare:
```
WDSUTIL /Get-DriverPackageFile /InfFile:"C:\temp\1394.inf" /Architecture:x86
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)