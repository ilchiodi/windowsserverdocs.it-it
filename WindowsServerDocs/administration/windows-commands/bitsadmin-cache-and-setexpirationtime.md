---
title: Bitsadmin cache e setexpirationtime
description: Argomento dei comandi di Windows per la **cache Bitsadmin e setexpirationtime**, che imposta la scadenza della cache.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 00ea6e4e-b707-4b31-88dd-b61a78565c8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bf283a0a8b94fd55c591609e3dcd1d127a2be81a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850884"
---
>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="bitsadmin-cache-and-setexpirationtime"></a>Bitsadmin cache e setexpirationtime

Imposta l'ora di scadenza della cache.

## <a name="syntax"></a>Sintassi

```
bitsadmin /cache /setexpirationtime secs
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| secondi | Numero di secondi fino alla scadenza della cache. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

L'esempio seguente scade la cache in 60 secondi.

```
C:\>bitsadmin /cache / setexpirationtime 60
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
