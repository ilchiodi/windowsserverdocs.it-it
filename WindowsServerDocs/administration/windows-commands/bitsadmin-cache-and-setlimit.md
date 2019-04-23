---
title: setlimit e cache bitsadmin
description: Argomento i comandi di Windows per **bitsadmin cache e setlimit** -imposta il limite delle dimensioni della cache.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 46578835-d5ce-423b-be4d-62ddb9e1908d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7d0b72c5ec6c779fa4ce3fa038352836cd9456ac
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852592"
---
# <a name="bitsadmin-cache-and-setlimit"></a>setlimit e cache bitsadmin



Imposta il limite delle dimensioni della cache.

## <a name="syntax"></a>Sintassi

```
bitsadmin /Cache /SetLimit Percent
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Percentuale|Il limite della cache definito come una percentuale dello spazio totale su disco rigido...|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente limita la dimensione della cache al 50%.
```
C:\>bitsadmin /Cache /SetLimit 50 
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)