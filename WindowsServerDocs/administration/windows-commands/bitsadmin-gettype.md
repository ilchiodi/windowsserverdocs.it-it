---
title: bitsadmin gettype
description: Windows Commands argomento per **BITSAdmin GetType**, che consente di recuperare il tipo di processo del processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bec16f04-3e95-4587-889e-3de6ad03c9c8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 66a1fc5b0478e1eec26557dc9a7f76d50abcb8b6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850444"
---
# <a name="bitsadmin-gettype"></a>bitsadmin gettype

Recupera il tipo di processo del processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /gettype <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |

## <a name="output"></a>Output

I valori di output includono:

| Type | Descrizione |
| --------------- | ----------- |
| Download | Il processo è un download. |
| Carica | Il processo è un caricamento. |
| Caricamento-risposta | Il processo è di caricamento-risposta. |
| Sconosciuto | Il tipo del processo è sconosciuto. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente viene recuperato il tipo di processo per il processo denominato *myDownloadJob*.

```
C:\>bitsadmin /gettype myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)