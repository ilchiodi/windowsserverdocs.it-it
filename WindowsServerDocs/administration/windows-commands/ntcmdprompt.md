---
title: ntcmdprompt
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0063bdbb-dc2b-41c4-99ee-b011603aaa86
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 25afab00ced3cb14771c18aa38c7fd8c98aecc0c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838044"
---
# <a name="ntcmdprompt"></a>ntcmdprompt

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esegue l'interprete dei comandi **Cmd.exe**, anziché **Command.com**, dopo l'esecuzione di un Terminate e rimanere residenti in Memoria o dopo l'avvio del prompt dei comandi dall'interno di un'applicazione MS-DOS.
## <a name="syntax"></a>Sintassi
```
ntcmdprompt
```
#### <a name="parameters"></a>Parametri

| Parametro |             Descrizione              |
|-----------|--------------------------------------|
|    /?     | Visualizza la guida al prompt dei comandi. |

## <a name="remarks"></a>Note
- Quando **Command.com** è in esecuzione, alcune funzionalità di **Cmd.exe**, ad esempio il **doskey** di visualizzazione della cronologia dei comandi, non sono disponibili. Se si preferisce eseguire il **Cmd.exe** interprete dei comandi dopo aver avviato una terminazione e rimanere residenti in Memoria o il prompt dei comandi da un'applicazione basata su MS-DOS, è possibile utilizzare il **ntcmdprompt** comando. Tuttavia, tenere presente che il Resident potrebbe non essere disponibili per l'uso quando si esegue **Cmd.exe**. È possibile includere il comando **ntcmdprompt** nel file **config. NT** o nel file di avvio personalizzato equivalente nel file PIF (Program Information file) di un'applicazione.
  ## <a name="examples"></a>Esempi
  Per includere **ntcmdprompt** nel file **config. NT** o nel file di avvio della configurazione specificato in PIF, digitare: **ntcmdprompt**
  ## <a name="additional-references"></a>Altre informazioni di riferimento
- - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

