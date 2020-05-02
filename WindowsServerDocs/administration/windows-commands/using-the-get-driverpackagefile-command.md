---
title: Get-DriverPackageFile
description: Argomento di riferimento per Get-DriverPackageFile, che consente di visualizzare informazioni su un pacchetto driver, inclusi i driver e i file in esso contenuti.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f01a2c67-7e9c-4aad-b625-383f5a1fca25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 71fc38e31471a1deb9d6be29b04d3cd911be1bd6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719926"
---
# <a name="get-driverpackagefile"></a>Get-DriverPackageFile

Visualizza informazioni su un pacchetto driver, inclusi i driver e file in che esso contenuti.

## <a name="syntax"></a>Sintassi

```
WDSUTIL /Get-DriverPackageFile /InfFile:<Inf File path> [/Architecture:{x86 | ia64 | x64}] [/Show:{Drivers | Files | All}]
```

### <a name="parameters"></a>Parametri

|         Parametro         |                              Descrizione                               |
|---------------------------|------------------------------------------------------------------------|
| /InfFile:\<percorso File INF> | Specifica il percorso completo e il nome file del file con estensione inf del pacchetto driver. |
|    [/Architettura: {x86    |                                  ia64                                  |
|     [/Show: {Drivers      |                                 File                                  |

## <a name="examples"></a>Esempi

Per visualizzare le informazioni relative a un file di driver, digitare:
```
WDSUTIL /Get-DriverPackageFile /InfFile:C:\temp\1394.inf /Architecture:x86
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)