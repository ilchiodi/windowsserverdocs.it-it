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
ms.openlocfilehash: 21240cfa1f0fa1f8e8fc3d2acf83f5df80812816
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857462"
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

##

[Chiave sintassi della riga di comando](command-line-syntax-key.md) vedere anche