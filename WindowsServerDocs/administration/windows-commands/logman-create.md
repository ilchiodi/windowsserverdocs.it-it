---
title: creare Logman
description: Argomento di riferimento per il comando logman create, che consente di creare un contatore, una traccia, un agente di raccolta dati di configurazione o un'API.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 972f0126-7bc4-4b14-9265-062864f3ffd4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c4a68be098f868cdd9cd48c1e7c68fc183fa1fab
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222958"
---
# <a name="logman-create"></a>creare Logman

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea un contatore, una traccia, un agente di raccolta dati di configurazione o un'API.

## <a name="syntax"></a>Sintassi

```
logman create <counter | trace | alert | cfg | api> <[-n] <name>> [options]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| [Logman creazione del contatore](logman-create-counter.md) | Crea un agente di raccolta dati del contatore. |
| [Logman creare traccia](logman-create-trace.md) | Crea un agente di raccolta dati di traccia. |
| [Logman crea avviso](logman-create-alert.md) | Crea un agente di raccolta dati di avviso. |
| [Logman creare cfg](logman-create-cfg.md) | Crea un agente di raccolta dati di configurazione. |
| [Logman creare api](logman-create-api.md) | Crea un agente di raccolta dati di traccia API. |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [logman (comando)](logman.md)