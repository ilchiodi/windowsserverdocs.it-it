---
title: import
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4b9d2751-7637-4738-83b0-8c578eb28f27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 569d986c57ae8b3d7253c050146ac0583c7c92df
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724840"
---
# <a name="import"></a>import



Importa un gruppo di dischi esterni nel gruppo di dischi del computer locale.

## <a name="syntax"></a>Sintassi

```
import [noerr]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|NOERR|Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|

## <a name="remarks"></a>Osservazioni

-   Il comando import Importa ogni disco presente nello stesso gruppo di dischi con lo stato attivo.
-   Per eseguire questa operazione, è necessario selezionare un disco dinamico in un gruppo di dischi esterni. Utilizzare il **disco selezionare** comando per selezionare un disco e spostare lo stato attivo a esso.

## <a name="examples"></a>Esempi

Per importare tutti i dischi che sono nello stesso gruppo di disco come disco con lo stato attivo al gruppo del disco del computer locale, digitare:
```
import
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

