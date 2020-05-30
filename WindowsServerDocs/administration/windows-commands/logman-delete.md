---
title: Logman delete
description: Argomento di riferimento per il comando logman Delete, che consente di eliminare un agente di raccolta dati esistente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8f3b2422-3dce-4fb4-adbb-8536b1d7da2b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 18739f3d7ab5af38dbe369e45aca8393ba3a3152
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222950"
---
# <a name="logman-delete"></a>Logman delete

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Elimina un agente di raccolta dati esistente.

## <a name="syntax"></a>Sintassi

```
logman delete <[-n] <name>> [options]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| -s`<computer name>` | Esegue il comando nel computer remoto specificato. |
| -config`<value>` | Specifica il file di impostazioni che contiene le opzioni di comando. |
| [-n]`<name>` | Nome dell'oggetto di destinazione. |
| -ets | Invia comandi direttamente alle sessioni di traccia eventi senza salvare o pianificare. |
| -[-] u`<user [password]>` | Specifica l'utente di Esegui come. L'immissione di un oggetto \* per la password genera una richiesta per la password. La password non viene visualizzata quando la si digita al prompt della password. |
| /? | Vengono visualizzate sensibile al contesto della Guida. |

### <a name="examples"></a>Esempi

Per eliminare l'agente di raccolta dati *perf_log*, digitare:

```
logman delete perf_log
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [logman (comando)](logman.md)
