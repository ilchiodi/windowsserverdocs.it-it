---
title: bdehdcfg driveinfo
description: 'Argomento i comandi di Windows per * * bdehdcfg: driveinfo * * - visualizzare la lettera di unità, le dimensioni totali, lo spazio disponibile massimo e le caratteristiche della partizione.'
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4aa041c27b1797e7d00476212887a7dc6dbc1880
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889062"
---
# <a name="bdehdcfg-driveinfo"></a>bdehdcfg: driveinfo

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di visualizzare la lettera di unità, le dimensioni totali, lo spazio disponibile massimo e le caratteristiche della partizione. Vengono elencate solo le partizioni valide. Lo spazio non allocato non viene elencato se sono già presenti quattro partizioni primarie o estese. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).
## <a name="syntax"></a>Sintassi
```
bdehdcfg -driveinfo <DriveLetter>
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|<DriveLetter>|Specifica una lettera di unità seguita da due punti.|
## <a name="remarks"></a>Note
Il comando è solo informativo ed non effettua alcuna modifica all'unità.
## <a name="BKMK_Examples"></a>Esempio
Nell'esempio seguente verrà visualizzate le informazioni di unità per l'unità C.
```
bdehdcfg  driveinfo C:
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
-   [bdehdcfg](bdehdcfg.md)