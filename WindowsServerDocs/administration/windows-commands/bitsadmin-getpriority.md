---
title: Bitsadmin GetPriority
description: 'Argomento dei comandi di Windows per **BITSAdmin GetPriority** : Recupera la priorità del processo specificato.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 0b8914f27c690aa9bb9cbf30430b3edf55f2eb92
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381426"
---
# <a name="bitsadmin-getpriority"></a>Bitsadmin GetPriority

Recupera la priorità del processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetPriority <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="remarks"></a>Note

La priorità è in **primo piano**, **alta**, **normale**, **bassa**o **sconosciuta**.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente viene recuperata la priorità per il processo denominato *myDownloadJob*.
```
C:\>bitsadmin /GetPriority myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
