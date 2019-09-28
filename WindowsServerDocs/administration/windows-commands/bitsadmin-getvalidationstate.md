---
title: getvalidationstate Bitsadmin
description: "Argomento comandi di Windows per **BITSAdmin getvalidationstate** -segnala lo stato di convalida del contenuto del file specificato all'interno del processo. "
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6ada3f1f-9967-4262-9d22-ed641e23f516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ca4269a596010258edd0479f5a7e9844bc9c98df
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381264"
---
# <a name="bitsadmin-getvalidationstate"></a>getvalidationstate Bitsadmin



Segnala lo stato di convalida del contenuto del file specificato all'interno del processo.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetValidationState <Job> <file index> 
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|Indice file|Inizia da 0|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente viene ottenuto lo stato di convalida del contenuto del file 2 nel processo denominato *il mio lavoro*.
```
C:\>bitsadmin /GetValidationState myJob 1
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)