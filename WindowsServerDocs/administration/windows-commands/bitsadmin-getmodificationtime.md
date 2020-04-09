---
title: bitsadmin getmodificationtime
description: Windows Commands Topic for **BITSAdmin getmodificationtime**, che recupera l'ultima volta in cui il processo è stato modificato o i dati sono stati trasferiti.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e543945e-92c4-491e-8c2d-344f8a3e342d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ace0f64b1fbe7ba72174bb3df2bd4dd65e929769
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850614"
---
# <a name="bitsadmin-getmodificationtime"></a>bitsadmin getmodificationtime

Recupera l'ultima volta in cui il processo è stato modificato o il trasferimento dei dati è riuscito.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getmodificationtime <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente viene recuperata l'ora dell'Ultima modifica per il processo denominato *myDownloadJob*.

```
C:\>bitsadmin /getmodificationtime myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)