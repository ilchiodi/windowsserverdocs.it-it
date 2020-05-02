---
title: DriveInfo BdeHdCfg
description: Argomento di riferimento per il comando BdeHdCfg driveinfo, che consente di visualizzare la lettera di unità, le dimensioni totali, lo spazio disponibile massimo e le caratteristiche della partizione.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f2d065e7-eced-4509-a1a0-ee2521a7f02e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b18b4c3e128cd17353d369b418a049d0208cb654
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718682"
---
# <a name="bdehdcfg-driveinfo"></a>BdeHdCfg: DriveInfo

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di visualizzare la lettera di unità, le dimensioni totali, lo spazio disponibile massimo e le caratteristiche della partizione. Vengono elencate solo le partizioni valide. Lo spazio non allocato non viene elencato se sono già presenti quattro partizioni primarie o estese.

>[!NOTE]
> Questo comando è esclusivamente informativo e non apporta alcuna modifica all'unità.

## <a name="syntax"></a>Sintassi

```
bdehdcfg -driveinfo <drive_letter>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| <drive_letter> | Specifica una lettera di unità seguita da due punti. |

## <a name="example"></a>Esempio

Per visualizzare le informazioni sull'unità C::

```
bdehdcfg  driveinfo C:
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
