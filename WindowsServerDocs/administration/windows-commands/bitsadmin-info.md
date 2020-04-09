---
title: bitsadmin info
description: Windows Commands argomento for **BITSAdmin info**, che visualizza le informazioni di riepilogo sul processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5c306677-0d64-41c0-8276-5bba7750cecb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b9d8b840e39620db01c44001d9cb7d593be2be5d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850344"
---
# <a name="bitsadmin-info"></a>bitsadmin info

Visualizza le informazioni di riepilogo sul processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /info <job> [/verbose]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |
| /verbose | Facoltativa. Fornisce informazioni dettagliate su ogni processo. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente vengono recuperate le informazioni sul processo denominato *myDownloadJob*.

```
C:\>bitsadmin /info myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)