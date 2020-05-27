---
title: Importa DiskPart
description: Argomento di riferimento per il comando Import, che importa un gruppo di dischi esterno nel gruppo di dischi del computer locale.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4b9d2751-7637-4738-83b0-8c578eb28f27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6912aa9698d484501cad5f3cdfb5b19955bb4931
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83818451"
---
# <a name="import-diskpart"></a>importazione (Diskpart)

Importa un gruppo di dischi esterni nel gruppo di dischi del computer locale. Questo comando importa ogni disco che si trova nello stesso gruppo del disco con lo stato attivo.

> IMPORTANTE Prima di poter usare questo comando, è necessario usare il [comando Seleziona disco](select-disk.md) per selezionare un disco dinamico in un gruppo di dischi esterni e spostare lo stato attivo a esso.

## <a name="syntax"></a>Sintassi

```
import [noerr]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| NOERR | Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore. |

### <a name="examples"></a>Esempi

Per importare tutti i dischi che sono nello stesso gruppo di disco come disco con lo stato attivo al gruppo del disco del computer locale, digitare:

```
import
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Diskpart](diskpart.md)
