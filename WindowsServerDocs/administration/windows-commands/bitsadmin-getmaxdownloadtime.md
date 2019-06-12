---
title: getmaxdownloadtime Bitsadmin
description: Argomento i comandi di Windows per **bitsadmin getmaxdownloadtime** -recupera il timeout del download in secondi.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cdce64f6-7125-489d-be3c-4af1dfc8c46a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d067d6a0821d9af4784c02c6a332e8eddd2352c0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434940"
---
# <a name="bitsadmin-getmaxdownloadtime"></a>getmaxdownloadtime Bitsadmin

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera il timeout del download in secondi.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetMaxDownloadtime <Job> 
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------|--------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="remarks"></a>Note

-   N\/A

## <a name="BKMK_examples"></a>Esempi
L'esempio seguente ottiene tempi di download massimo per il processo denominato *myDownloadJob* in pochi secondi.

```
C:\>bitsadmin /GetMaxDownloadtime myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)


