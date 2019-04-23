---
title: importare
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4b9d2751-7637-4738-83b0-8c578eb28f27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 379d5923a9355db2965b56c27cedd207b1b63006
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885992"
---
# <a name="import"></a>importare



Importa un gruppo di dischi esterni nel gruppo di dischi del computer locale.

## <a name="syntax"></a>Sintassi

```
import [noerr]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|NOERR|Solo per script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|

## <a name="remarks"></a>Note

-   Il comando import Importa ogni disco presente nello stesso gruppo di dischi con lo stato attivo.
-   Per eseguire questa operazione, è necessario selezionare un disco dinamico in un gruppo di dischi esterni. Utilizzare il **disco selezionare** comando per selezionare un disco e spostare lo stato attivo a esso.

## <a name="BKMK_examples"></a>Esempi

Per importare tutti i dischi che sono nello stesso gruppo di disco come disco con lo stato attivo al gruppo del disco del computer locale, digitare:
```
import
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)

