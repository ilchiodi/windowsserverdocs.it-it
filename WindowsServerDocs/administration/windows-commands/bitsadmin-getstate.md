---
title: bitsadmin getstate
description: 'Argomento dei comandi di Windows per * * * *- '
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
ms.openlocfilehash: 55be37a6b1b44b81ed9002e5e3b9eb1fd46bd0dc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381228"
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

|-----|-----| | In coda | Il processo è in attesa di esecuzione. | | CONNESSIONE | BITS sta contattando il server. | | TRASFERIMENTO | BITS sta trasferendo i dati. | | SOSPESO | Il processo è stato sospeso. | | ERRORE | Si è verificato un errore irreversibile. il trasferimento non verrà ripetuto. | | TRANSIENT_ERROR | Si è verificato un errore reversibile. tentativi di trasferimento quando scade il ritardo minimo tra tentativi. | | ACCETTATO | Il processo è stato completato. | | Annullato | Il processo è stato annullato. |

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente recupera lo stato del processo *myDownloadJob*.
```
C:\>bitsadmin /GetState myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)