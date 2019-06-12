---
title: tskill
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 08986e6a-6900-4ece-85a1-8f73b14db1b3 Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b582334d7b79b2badbb86818be1093b6a5f55080
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440811"
---
# <a name="tskill"></a>tskill

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Termina un processo in esecuzione in una sessione in un server Host sessione Desktop remoto (Host sessione rd).
Per esempi di come usare questo comando, vedere [esempi](#BKMK_examples).

> [!NOTE]
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per scoprire quali sono le novità nella versione più recente, vedere [novità in Servizi Desktop remoto in Windows Server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.

## <a name="syntax"></a>Sintassi
```
tskill {<ProcessID> | <ProcessName>} [/server:<ServerName>] [/id:<SessionID> | /a] [/v]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------|--------|
|\<ProcessID>|Specifica l'ID del processo che si desidera terminare.|
|\<ProcessName>|Specifica il nome del processo che si desidera terminare. Questo parametro può includere caratteri jolly.|
|/server:\<ServerName>|Specifica il server terminal che contiene il processo che si desidera terminare. Se **/server** viene omesso, viene utilizzato il server Host sessione Desktop remoto corrente.|
|/id:\<SessionID>|Termina il processo di cui è in esecuzione nella sessione specificata.|
|/a|Termina il processo di cui è in esecuzione in tutte le sessioni.|
|/v|Consente di visualizzare informazioni sulle azioni eseguibili in esecuzione.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note
- È possibile usare **tskill** per terminare solo i processi che appartengono all'utente, a meno che non si è un amministratore. Gli amministratori hanno accesso completo a tutti **tskill** le funzioni e autorizzati a terminare processi in esecuzione in altre sessioni utente.
- Quando terminano, tutti i processi in esecuzione in una sessione termina anche la sessione.
- Se si usa il *ProcessName* e il **/server:** <em>nomeserver</em> parametri, è necessario specificare anche il **/id:**  <em>SessionID</em> o il **/a** parametro.

## <a name="BKMK_examples"></a>Esempi
- Per terminare il processo 6543, digitare:
  ```
  tskill 6543
  ```
- Per terminare il processo "explorer" in esecuzione nella sessione 5, digitare:
  ```
  tskill explorer /id:5
  ```
  #### <a name="additional-references"></a>Altri riferimenti
  [Chiave sintassi della riga di comando](command-line-syntax-key.md)
  [Servizi Desktop remoto &#40;servizi Terminal&#41; Guida comandi](remote-desktop-services-terminal-services-command-reference.md)
