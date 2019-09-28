---
title: Bitsadmin cache e DeleteUrl
description: Windows Commands Topic for **BITSAdmin cache and DeleteUrl** -Elimina tutte le voci della cache per l'URL specificato.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 869d3bc0f011cc82aaea9b7468667964051e1c00
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382063"
---
# <a name="bitsadmin-cache-and-deleteurl"></a>Bitsadmin cache e DeleteUrl



Elimina tutte le voci della cache per l'URL specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /DeleteURL url
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|url|Uniform Resource Locator che identifica un file remoto.|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente vengono eliminate tutte le voci della cache per https://www.microsoft.com/en/us/default.aspx
```
C:\>bitsadmin /DeleteURL https://www.microsoft.com/en/us/default.aspx 
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)