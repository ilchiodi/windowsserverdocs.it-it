---
title: bitsadmin getfilestotal
description: 'Windows Commands Topic for **BITSAdmin getfilestotal** : Recupera il numero di file nel processo specificato.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c5de113e-f29c-4cd3-9392-0e300018d516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 27cf04e8745aeab5cd1f2ce379c8506be642fea2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381610"
---
# <a name="bitsadmin-getfilestotal"></a>bitsadmin getfilestotal



Recupera il numero di file nel processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetFilesTotal <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente viene recuperato il numero di file inclusi nel processo denominato *myDownloadJob*.
```
C:\>bitsadmin /GetFilesTotal myDownloadJob
```

# #

[Chiave sintassi della riga di comando](command-line-syntax-key.md) Vedere anche