---
title: DeleteUrl e cache bitsadmin
description: Argomento i comandi di Windows per **bitsadmin cache e deleteurl** -Elimina tutte le voci della cache per l'URL specificato.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e108b76b-fae9-4c16-bf4c-d74c9f025953
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a831c49e1461761cb7466b46e7a5ad8e037f4ec9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816652"
---
# <a name="bitsadmin-cache-and-deleteurl"></a>DeleteUrl e cache bitsadmin



Elimina tutte le voci della cache per l'URL specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /DeleteURL url
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|url|L'URL che identifica un file remoto.|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente vengono eliminate tutte le voci della cache per https://www.microsoft.com/en/us/default.aspx
```
C:\>bitsadmin /DeleteURL https://www.microsoft.com/en/us/default.aspx 
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)