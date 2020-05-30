---
title: logman start e logman stop
description: Argomento di riferimento per i comandi logman start e logman stop, che avvia un agente di raccolta dati e imposta la data e l'ora di inizio su manuale oppure interrompe un insieme agenti di raccolta dati e imposta l'ora di fine su manuale.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a40006a1-876e-474b-aaf1-f365c730deea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: db0111c4f58f0a28ad76affd3e4d8e24d4a927c2
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222911"
---
# <a name="logman-start-and-logman-stop"></a>logman start e logman stop

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il comando **logman start** avvia un agente di raccolta dati e imposta l'ora di inizio su manuale. Il comando **logman stop** arresta un insieme agenti di raccolta dati e imposta l'ora di fine su manuale.

## <a name="syntax"></a>Sintassi

```
logman start <[-n] <name>> [options]
logman stop <[-n] <name>> [options]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| -s`<computer name>` | Eseguire il comando nel computer remoto specificato. |
| -config`<value>` | Specifica il file di impostazioni che contiene le opzioni di comando. |
| [-n]`<name>` | Specifica il nome dell'oggetto di destinazione. |
| -ets | Invia direttamente i comandi alle sessioni di traccia eventi senza salvare o pianificare. |
| -come | Esegue l'operazione richiesta in modo asincrono. |
| -? | Vengono visualizzate sensibile al contesto della Guida. |

### <a name="examples"></a>Esempi

Per avviare l'agente di raccolta dati *perf_log*, nel computer remoto *server_1*, digitare:

```
logman start perf_log -s server_1
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [logman (comando)](logman.md)
