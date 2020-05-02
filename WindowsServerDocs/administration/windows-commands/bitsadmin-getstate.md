---
title: bitsadmin getstate
description: Argomento di riferimento per il comando Bitsadmin GetState, che recupera lo stato del processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1252d6cf-14ca-44df-beb2-930ff011f297
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ab014c96c6d5d62232243d704d41d33cfcfc50f0
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717532"
---
# <a name="bitsadmin-getstate"></a>bitsadmin getstate

Recupera lo stato del processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getstate <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |

#### <a name="output"></a>Output

I valori di output restituiti possono essere:

| State | Descrizione |
| --------------- | ----------- |
| Queued | Il processo è in attesa di esecuzione. |
| Connecting | BITS è la connessione al server. |
| Transferring | BITS è il trasferimento dei dati. |
| Trasferiti | BITS ha trasferito correttamente tutti i file nel processo. |
| Suspended | Il processo è stato sospeso. |
| Errore | Si è verificato un errore irreversibile; il trasferimento non verrà ritentato. |
| Transient_Error | Si è verificato un errore reversibile; i tentativi di trasferimento quando scade l'intervallo minimo tra tentativi. |
| Confermato | Il processo è stato completato. |
| Cancellati | Il processo è stato annullato. |

## <a name="examples"></a>Esempi

Per recuperare lo stato per il processo denominato *myDownloadJob*:

```
bitsadmin /getstate myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
