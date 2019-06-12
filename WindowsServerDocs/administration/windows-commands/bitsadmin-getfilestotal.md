---
title: bitsadmin getfilestotal
description: Argomento i comandi di Windows per **bitsadmin getfilestotal** -recupera il numero di file del processo specificato.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: b5f6d32b3410b182c510cf40b9def5370efafdc4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435127"
---
# <a name="bitsadmin-getfilestotal"></a>bitsadmin getfilestotal



Recupera il numero di file del processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetFilesTotal <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente recupera il numero di file inclusi nel processo denominato *myDownloadJob*.
```
C:\>bitsadmin /GetFilesTotal myDownloadJob
```

# #

[Chiave sintassi della riga di comando](command-line-syntax-key.md) vedere anche