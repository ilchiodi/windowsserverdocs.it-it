---
title: logman
description: Argomento di riferimento per il comando Logman, che consente di creare e gestire i registri di sessione di traccia eventi e di prestazioni e supporta molte funzioni di performance monitor dalla riga di comando.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 574a5203-5b3b-4759-a678-f26d00dde447
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 44b5e134440d71eed61ca8e03739abcc962df1f9
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820551"
---
# <a name="logman"></a>logman

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea e gestisce i registri di sessione di traccia eventi e prestazioni e supporta numerose funzioni di monitoraggio delle prestazioni dalla riga di comando.

## <a name="syntax"></a>Sintassi

```
logman [create | query | start | stop | delete| update | import | export | /?] [options]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| [logman create](logman-create.md) | Crea un contatore, una traccia, un agente di raccolta dati di configurazione o un'API. |
| [logman query](logman-query.md) | Esegue una query sulle proprietà dell'agente di raccolta dati. |
| [logman start &#124; stop](logman-start-stop.md) | Avvia o interrompe la raccolta dei dati. |
| [logman delete](logman-delete.md) | Elimina un agente di raccolta dati esistente. |
| [logman update](logman-update.md) | Aggiorna le proprietà di un agente di raccolta dati esistente. |
| [logman import &#124; export](logman-import-export.md) | Importa un insieme agenti di raccolta dati da un file XML o esporta un insieme agenti di raccolta dati in un file XML. |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)