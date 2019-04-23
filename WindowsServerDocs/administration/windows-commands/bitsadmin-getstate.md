---
title: bitsadmin getstate
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f7ed7529fda264efaceb6b4b36e36e728c211f3f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889622"
---
# <a name="bitsadmin-getstate"></a>bitsadmin getstate



Recupera lo stato del processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetState <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="remarks"></a>Note

Gli stati possibili sono:

|-----|-----| | IN CODA | Il processo è in attesa di esecuzione. | | LA CONNESSIONE | BITS è la connessione al server. | | TRASFERIMENTO | BITS è il trasferimento dei dati. | | SOSPESO | Il processo è stato sospeso. | | ERRORE | Si è verificato un errore irreversibile; il trasferimento non verrà ritentato. | | TRANSIENT_ERROR | Si è verificato un errore reversibile; i tentativi di trasferimento quando scade l'intervallo minimo tra tentativi. | | RICONOSCIUTO | Il processo è stato completato. | | ANNULLATO | Il processo è stato annullato. |

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente recupera lo stato del processo *myDownloadJob*.
```
C:\>bitsadmin /GetState myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)