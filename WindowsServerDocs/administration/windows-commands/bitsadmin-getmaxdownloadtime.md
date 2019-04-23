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
ms.openlocfilehash: 868f7ac58a69c067681bf0597524fbaa5a25984f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842422"
---
#<a name="bitsadmin-getmaxdownloadtime"></a>getmaxdownloadtime Bitsadmin

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
[Chiave sintassi della riga di comando](command-line-syntax-key.md)


