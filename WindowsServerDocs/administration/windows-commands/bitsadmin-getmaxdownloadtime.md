---
title: getmaxdownloadtime Bitsadmin
description: 'Argomento dei comandi di Windows per **BITSAdmin getmaxdownloadtime** : Recupera il timeout di download in secondi.'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 39a19f86e97c1a525b5beb0c5f3b23dff349cb19
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381580"
---
# <a name="bitsadmin-getmaxdownloadtime"></a>getmaxdownloadtime Bitsadmin

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera il timeout di download in secondi.

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
Nell'esempio seguente viene ottenuto il tempo massimo di download per il processo denominato *myDownloadJob* in secondi.

```
C:\>bitsadmin /GetMaxDownloadtime myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)


