---
title: bitsadmin suspend
description: Argomento di riferimento per il comando Bitsadmin Suspend, che sospende il processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f9d42500-7bea-4aa8-a9f0-c22f6ed3e73b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8117cf9f4286994847e53dca8065da6821d47c5d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720455"
---
# <a name="bitsadmin-suspend"></a>bitsadmin suspend

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Sospende il processo specificato. Se il processo è stato sospeso per errore, è possibile usare l'opzione [BITSAdmin Resume](bitsadmin-resume.md) per riavviare il processo.

## <a name="syntax"></a>Sintassi

```
bitsadmin /suspend <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ---------- |
| processo | Nome visualizzato o GUID del processo. |

## <a name="example"></a>Esempio

Per sospendere il processo denominato *myDownloadJob*:


```
bitsadmin /suspend myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin Resume](bitsadmin-resume.md)

- [comando Bitsadmin](bitsadmin.md)
