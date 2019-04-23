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
ms.openlocfilehash: 65804f99095d0c0a56537b1d155ac26e768f61a7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827662"
---
# <a name="ntcmdprompt"></a>ntcmdprompt

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esegue l'interprete dei comandi **Cmd.exe**, anziché **Command.com**, dopo l'esecuzione di un Terminate e rimanere residenti in Memoria o dopo l'avvio del prompt dei comandi dall'interno di un'applicazione MS-DOS.
## <a name="syntax"></a>Sintassi
```
ntcmdprompt
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/?|Visualizza la guida al prompt dei comandi.|
## <a name="remarks"></a>Note
-   Quando **Command.com** è in esecuzione, alcune funzionalità di **Cmd.exe**, ad esempio il **doskey** di visualizzazione della cronologia dei comandi, non sono disponibili. Se si preferisce eseguire il **Cmd.exe** interprete dei comandi dopo aver avviato una terminazione e rimanere residenti in Memoria o il prompt dei comandi da un'applicazione basata su MS-DOS, è possibile utilizzare il **ntcmdprompt** comando. Tuttavia, tenere presente che il Resident potrebbe non essere disponibili per l'uso quando si esegue **Cmd.exe**. È possibile includere il **ntcmdprompt** comando le **config** file o il file di avvio personalizzata equivalente in informazioni file Pif (program) un'applicazione.
## <a name="examples"></a>Esempi
Per includere **ntcmdprompt** nel **config** file o il file di avvio di configurazione specificato nel file Pif, tipo: **ntcmdprompt**
## <a name="additional-references"></a>Riferimenti aggiuntivi
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)

