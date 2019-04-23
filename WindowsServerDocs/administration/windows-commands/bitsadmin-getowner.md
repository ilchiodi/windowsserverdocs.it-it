---
title: bitsadmin getowner
description: Argomento i comandi di Windows per **bitsadmin getowner** -recupera il proprietario del processo specificato.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5203f84c-a879-4f31-ae3e-7ea74bd63ca5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1381bc1268b2b81e2bde18d0d8e17bd760345e0f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886712"
---
# <a name="bitsadmin-getowner"></a>bitsadmin getowner

Visualizza il nome visualizzato o il GUID del proprietario del processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetOwner <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="BKMK_examples"></a>Esempi

L'esempio seguente mostra il proprietario per il processo denominato *myDownloadJob*.
```
C:\>bitsadmin /GetOwner myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)