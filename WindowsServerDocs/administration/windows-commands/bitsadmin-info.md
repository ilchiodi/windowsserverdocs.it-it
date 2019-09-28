---
title: bitsadmin info
description: Nell'argomento comandi di Windows per **vengono visualizzate le informazioni di riepilogo sul processo specificato.** -Bitsadmin informazioni
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c306677-0d64-41c0-8276-5bba7750cecb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3b6710d73860315fcd13670669871cd310ffb41c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381083"
---
# <a name="bitsadmin-info"></a>bitsadmin info



Visualizza le informazioni di riepilogo sul processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /Info <Job> [/verbose]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="remarks"></a>Note

Usare il parametro/Verbose per fornire informazioni dettagliate sul processo.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente vengono recuperate le informazioni sul processo denominato *myDownloadJob*.
```
C:\>bitsadmin /Info myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)