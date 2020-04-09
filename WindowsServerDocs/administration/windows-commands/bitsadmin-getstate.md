---
title: bitsadmin getstate
description: Windows Commands argomento per **BITSAdmin GetState**, che recupera lo stato del processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1252d6cf-14ca-44df-beb2-930ff011f297
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 43cd8c8e614cce65f55b16fc5395b1d37de0cf95
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850464"
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
| lavoro | Nome visualizzato o GUID del processo. |

## <a name="output"></a>Output

I valori di output includono:

| Stato | Descrizione |
| --------------- | ----------- |
| Accodato | Il processo è in attesa di esecuzione. |
| Connessione in corso | BITS è la connessione al server. |
| Trasferimento | BITS è il trasferimento dei dati. |
| Trasferiti | BITS ha trasferito correttamente tutti i file nel processo. |
| Suspended | Il processo è stato sospeso. |
| Errore | Si è verificato un errore irreversibile; il trasferimento non verrà ritentato. |
| Transient_Error | Si è verificato un errore reversibile; i tentativi di trasferimento quando scade l'intervallo minimo tra tentativi. |
| Riconosciuto | Il processo è stato completato. |
| Canceled | Il processo è stato annullato. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente recupera lo stato del processo *myDownloadJob*.

```
C:\>bitsadmin /getstate myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
