---
title: ntcmdprompt
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0063bdbb-dc2b-41c4-99ee-b011603aaa86
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b5b0dcbc0735f20b63cbf441f4014411acd3a48e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723470"
---
# <a name="ntcmdprompt"></a>ntcmdprompt

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esegue l'interprete dei comandi **Cmd.exe**, anziché **Command.com**, dopo l'esecuzione di un Terminate e rimanere residenti in Memoria o dopo l'avvio del prompt dei comandi dall'interno di un'applicazione MS-DOS.
## <a name="syntax"></a>Sintassi
```
ntcmdprompt
```
#### <a name="parameters"></a>Parametri

| Parametro |             Descrizione              |
|-----------|--------------------------------------|
|    /?     | Visualizza la guida al prompt dei comandi. |

## <a name="remarks"></a>Osservazioni
- Quando **Command.com** è in esecuzione, alcune funzionalità di **Cmd.exe**, ad esempio il **doskey** di visualizzazione della cronologia dei comandi, non sono disponibili. Se si preferisce eseguire il **Cmd.exe** interprete dei comandi dopo aver avviato una terminazione e rimanere residenti in Memoria o il prompt dei comandi da un'applicazione basata su MS-DOS, è possibile utilizzare il **ntcmdprompt** comando. Tuttavia, tenere presente che il Resident potrebbe non essere disponibili per l'uso quando si esegue **Cmd.exe**. È possibile includere il comando **ntcmdprompt** nel file **config. NT** o nel file di avvio personalizzato equivalente nel file PIF (Program Information file) di un'applicazione.
  ## <a name="examples"></a>Esempi
  Per includere **ntcmdprompt** nel file **config. NT** o nel file di avvio della configurazione specificato in PIF, digitare: **ntcmdprompt**
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
- - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

