---
title: tskill
description: Argomento di riferimento per tskill, che termina un processo in esecuzione in una sessione in un server host sessione Desktop remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 08986e6a-6900-4ece-85a1-8f73b14db1b3 Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 13bd18a84dccbbeee88c24b9b07208b3174bc558
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721244"
---
# <a name="tskill"></a>tskill

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Termina un processo in esecuzione in una sessione in un server host sessione Desktop remoto.


> [!NOTE]
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per informazioni sulle novità della versione più recente, vedere Novità di [Servizi Desktop remoto in Windows server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.

## <a name="syntax"></a>Sintassi
```
tskill {<ProcessID> | <ProcessName>} [/server:<ServerName>] [/id:<SessionID> | /a] [/v]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------|--------|
|\<> ProcessId|Specifica l'ID del processo che si desidera terminare.|
|\<ProcessName>|Specifica il nome del processo che si desidera terminare. Questo parametro può includere caratteri jolly.|
|/Server:\<nomeserver>|Specifica il server terminal che contiene il processo che si desidera terminare. Se **/Server** non è specificato, viene utilizzato il server Host sessione Desktop remoto corrente.|
|/ID:\<SessionID>|Termina il processo in esecuzione nella sessione specificata.|
|/a|Termina il processo in esecuzione in tutte le sessioni.|
|/v|Visualizza le informazioni sulle azioni eseguite.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni
- È possibile usare **tskill** per terminare solo i processi che appartengono all'utente, a meno che non si sia un amministratore. Gli amministratori hanno accesso completo a tutte le funzioni **tskill** e possono terminare i processi in esecuzione in altre sessioni utente.
- Quando tutti i processi in esecuzione in una sessione terminano, termina anche la sessione.
- Se si usano i parametri *ProcessName* e **/Server:**<em>ServerName</em> , è necessario specificare anche il parametro **/ID:**<em>SessionID</em> o **/a** .

## <a name="examples"></a>Esempi
- Per terminare il processo 6543, digitare:
  ```
  tskill 6543
  ```
- Per terminare Esplora processi in esecuzione nella sessione 5, digitare:
  ```
  tskill explorer /id:5
  ```
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
  - [Guida di](command-line-syntax-key.md)
  riferimento ai comandi servizi desktop remoto della chiave della sintassi della riga di comando[(Servizi terminal)](remote-desktop-services-terminal-services-command-reference.md)
