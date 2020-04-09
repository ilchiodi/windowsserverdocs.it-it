---
title: cache Bitsadmin e limite
description: Argomento dei comandi di Windows per la **cache Bitsadmin e**il valore di limite, che imposta il limite delle dimensioni della cache.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 46578835-d5ce-423b-be4d-62ddb9e1908d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 746ee0b69da8f5bd22fec2ccbd432126cc25d94d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850874"
---
# <a name="bitsadmin-cache-and-setlimit"></a>cache Bitsadmin e limite

Imposta il limite delle dimensioni della cache.

## <a name="syntax"></a>Sintassi

```
bitsadmin /cache /setlimit percent
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| percent | Limite della cache definito come percentuale dello spazio totale su disco rigido. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente la dimensione della cache viene limitata al 50%.

```
C:\>bitsadmin /cache /setlimit 50
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)