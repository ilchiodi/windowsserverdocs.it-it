---
title: ntcmdprompt
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0063bdbb-dc2b-41c4-99ee-b011603aaa86
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 583f56c294e66542a75efca09e97d57ae54a8cea
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436420"
---
# <a name="ntcmdprompt"></a>ntcmdprompt

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esegue l'interprete dei comandi **Cmd.exe**, anziché **Command.com**, dopo l'esecuzione di un Terminate e rimanere residenti in Memoria o dopo l'avvio del prompt dei comandi dall'interno di un'applicazione MS-DOS.
## <a name="syntax"></a>Sintassi
```
ntcmdprompt
```
### <a name="parameters"></a>Parametri

| Parametro |             Descrizione              |
|-----------|--------------------------------------|
|    /?     | Visualizza la guida al prompt dei comandi. |

## <a name="remarks"></a>Note
- Quando **Command.com** è in esecuzione, alcune funzionalità di **Cmd.exe**, ad esempio il **doskey** di visualizzazione della cronologia dei comandi, non sono disponibili. Se si preferisce eseguire il **Cmd.exe** interprete dei comandi dopo aver avviato una terminazione e rimanere residenti in Memoria o il prompt dei comandi da un'applicazione basata su MS-DOS, è possibile utilizzare il **ntcmdprompt** comando. Tuttavia, tenere presente che il Resident potrebbe non essere disponibili per l'uso quando si esegue **Cmd.exe**. È possibile includere il **ntcmdprompt** comando le **config** file o il file di avvio personalizzata equivalente in informazioni file Pif (program) un'applicazione.
  ## <a name="examples"></a>Esempi
  Per includere **ntcmdprompt** nel **config** file o il file di avvio di configurazione specificato nel file Pif, tipo: **ntcmdprompt**
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

