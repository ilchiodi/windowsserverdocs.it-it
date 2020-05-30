---
title: Logman query
description: Argomento di riferimento per il comando Logman query, che esegue query sull'agente di raccolta dati o sulle proprietà dell'insieme agenti di raccolta dati.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1116a0f0-5415-4369-a045-12f79f8f66de
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f37b78023fd12c1d58234cdecdb69af8a1c6002d
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222812"
---
# <a name="logman-query"></a>Logman query

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Query sull'agente di raccolta dati o sulle proprietà dell'insieme agenti di raccolta dati.

## <a name="syntax"></a>Sintassi

```
logman query [providers|Data Collector Set name] [options]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| -s`<computer name>` | Eseguire il comando nel computer remoto specificato. |
| -config`<value>` | Specifica il file di impostazioni che contiene le opzioni di comando. |
| [-n]`<name>` | Nome dell'oggetto di destinazione. |
| -ets | Invia comandi direttamente alle sessioni di traccia eventi senza salvare o pianificare. |
| /? | Vengono visualizzate sensibile al contesto della Guida. |

### <a name="examples"></a>Esempi

Per elencare tutti gli insiemi agenti di raccolta dati configurati nel sistema di destinazione, digitare:

```
logman query
```

Per elencare gli agenti di raccolta dati contenuti nell'insieme agenti di raccolta dati denominato *perf_log*, digitare:

```
logman query perf_log
```

Per elencare tutti i provider disponibili di agenti di raccolta dati nel sistema di destinazione, digitare:

```
logman query providers
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [logman (comando)](logman.md)
