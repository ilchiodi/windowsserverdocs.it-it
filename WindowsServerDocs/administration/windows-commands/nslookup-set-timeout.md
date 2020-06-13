---
title: nslookup set timeout
description: Argomento di riferimento per il comando nslookup set timeout, che consente di modificare il numero iniziale di secondi di attesa di una risposta a una richiesta di ricerca.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 07afdaf4-ffec-496f-a188-4e91cf1a28f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0d8fd0d96226e193ba723cc0a726ddf5362a538c
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721386"
---
# <a name="nslookup-set-timeout"></a>nslookup set timeout

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modifica il numero iniziale di secondi di attesa di una risposta a una richiesta di ricerca. Se una risposta non viene ricevuta entro la quantità di tempo specificata, il periodo di timeout viene raddoppiato e la richiesta viene inviata nuovamente. Usare il comando [nslookup set Retry](nslookup-set-retry.md) per determinare il numero di tentativi di invio della richiesta.

## <a name="syntax"></a>Sintassi

```
set timeout=<number>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| ---------- | ---------- |
| `<number>` | Specifica il numero di secondi di attesa di una risposta. Il numero predefinito di secondi di attesa è **5**. |
| /? | Visualizza la guida al prompt dei comandi. |
| /help | Visualizza la guida al prompt dei comandi. |

### <a name="examples"></a>Esempio

Per impostare il timeout per ottenere una risposta a 2 secondi:

```
set timeout=2
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [nslookup set retry](nslookup-set-retry.md)
