---
title: DriveInfo BdeHdCfg
description: 'Windows Commands Topic for * * BdeHdCfg: DriveInfo * *-Visualizza la lettera di unità, le dimensioni totali, lo spazio disponibile massimo e le caratteristiche della partizione.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f2d065e7-eced-4509-a1a0-ee2521a7f02e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b0f4541bfd71fb7639d18e6e548559ed02918815
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382273"
---
# <a name="bdehdcfg-driveinfo"></a>BdeHdCfg: DriveInfo

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di visualizzare la lettera di unità, le dimensioni totali, lo spazio disponibile massimo e le caratteristiche della partizione. Vengono elencate solo le partizioni valide. Lo spazio non allocato non viene elencato se sono già presenti quattro partizioni primarie o estese. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).
## <a name="syntax"></a>Sintassi
```
bdehdcfg -driveinfo <DriveLetter>
```
### <a name="parameters"></a>Parametri

|   Parametro   |                  Descrizione                  |
|---------------|-----------------------------------------------|
| <DriveLetter> | Specifica una lettera di unità seguita da due punti. |

## <a name="remarks"></a>Note
Il comando è esclusivamente informativo e non apporta alcuna modifica all'unità.
## <a name="BKMK_Examples"></a>Esempio
Nell'esempio seguente vengono visualizzate le informazioni sull'unità C.
```
bdehdcfg  driveinfo C:
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [bdehdcfg](bdehdcfg.md)
