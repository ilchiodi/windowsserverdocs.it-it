---
title: Get-DriverPackageFile
description: Windows Commands argomento per Get-DriverPackageFile, che consente di visualizzare informazioni su un pacchetto driver, inclusi i driver e i file in esso contenuti.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f01a2c67-7e9c-4aad-b625-383f5a1fca25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d485a24479aa857270968a1bff7bd55a014347a3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831034"
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
| /InfFile:\<percorso file inf > | Specifica il percorso completo e il nome file del file con estensione inf del pacchetto driver. |
|    [/Architettura: {x86    |                                  ia64                                  |
|     [/Show: {Drivers      |                                 Files                                  |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per visualizzare le informazioni relative a un file di driver, digitare:
```
WDSUTIL /Get-DriverPackageFile /InfFile:C:\temp\1394.inf /Architecture:x86
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)