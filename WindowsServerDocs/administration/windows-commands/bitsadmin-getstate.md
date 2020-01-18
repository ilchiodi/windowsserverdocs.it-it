---
title: bitsadmin getstate
description: Argomento dei comandi di Windows per Bitsadmin GetState
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1252d6cf-14ca-44df-beb2-930ff011f297
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2cff790c8787b1514e8523a4583184d6f6a59efc
ms.sourcegitcommit: 51e0b575ef43cd16b2dab2db31c1d416e66eebe8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/17/2020
ms.locfileid: "76259106"
---
# <a name="bitsadmin-getstate"></a>bitsadmin getstate


Recupera lo stato del processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetState <Job>
```

## <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
|    Job    | Nome visualizzato o il GUID del processo |

## <a name="remarks"></a>Osservazioni

Gli stati possibili sono:

|      Stato      | Descrizione |
| --------------- | ----------- |
| IN CODA          | Il processo è in attesa di esecuzione. |
| CONNESSIONE      | BITS è la connessione al server. |
| IL TRASFERIMENTO    | BITS è il trasferimento dei dati. |
| TRASFERITI     | BITS ha trasferito correttamente tutti i file nel processo. |
| SOSPESO       | Il processo è stato sospeso. |
| ERRORE           | Si è verificato un errore irreversibile; il trasferimento non verrà ritentato. |
| TRANSIENT_ERROR | Si è verificato un errore reversibile; i tentativi di trasferimento quando scade l'intervallo minimo tra tentativi. |
| RICONOSCIUTO    | Il processo è stato completato. |
| ANNULLATO        | Il processo è stato annullato. |

## <a name="BKMK_examples"></a>Esempi:

Nell'esempio seguente recupera lo stato del processo *myDownloadJob*.

```
C:\>bitsadmin /GetState myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
