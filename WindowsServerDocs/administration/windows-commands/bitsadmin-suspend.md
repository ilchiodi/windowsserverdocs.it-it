---
title: bitsadmin suspend
description: 'Argomento dei comandi di Windows per **BITSAdmin Suspend** : sospende il processo specificato.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f9d42500-7bea-4aa8-a9f0-c22f6ed3e73b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7a3a484df2b50cdc8893512020b835f913793d2c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380375"
---
# <a name="bitsadmin-suspend"></a>bitsadmin suspend

> Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Sospende il processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /Suspend <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------|--------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="remarks"></a>Note

Per riavviare il processo, usare l'opzione [BITSAdmin Resume](bitsadmin-resume.md) .

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente viene sospeso il processo denominato *myDownloadJob*.

```
C:\>bitsadmin /Suspend myDownloadJob
```

#### <a name="additional-references"></a>Riferimenti aggiuntivi

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
