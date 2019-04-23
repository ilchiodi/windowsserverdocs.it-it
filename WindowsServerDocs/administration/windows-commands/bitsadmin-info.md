---
title: bitsadmin info
description: Argomento i comandi di Windows per **Visualizza informazioni di riepilogo sul processo specificato.** -info bitsadmin
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c306677-0d64-41c0-8276-5bba7750cecb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ee96c69e311600a53f04b1b883983718adf0f69
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851522"
---
# <a name="bitsadmin-info"></a>bitsadmin info



Visualizza le informazioni di riepilogo sul processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /Info <Job> [/verbose]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="remarks"></a>Note

Usare il / verbose parametro per fornire informazioni dettagliate sul processo.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente recupera informazioni sul processo denominato *myDownloadJob*.
```
C:\>bitsadmin /Info myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)