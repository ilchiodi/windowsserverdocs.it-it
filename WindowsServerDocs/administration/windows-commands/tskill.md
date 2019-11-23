---
title: tskill
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 697363c91837ff675a14099fd212f4f0753b739b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392340"
---
# <a name="tskill"></a>tskill

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Termina un processo in esecuzione in una sessione in un server Host sessione Desktop remoto (host sessione Desktop remoto).
per esempi relativi all'uso di questo comando, vedere [esempi](#BKMK_examples).

> [!NOTE]
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per informazioni sulle novità della versione più recente, vedere Novità di [Servizi Desktop remoto in Windows server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.

## <a name="syntax"></a>Sintassi
```
tskill {<ProcessID> | <ProcessName>} [/server:<ServerName>] [/id:<SessionID> | /a] [/v]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------|--------|
|> ProcessId \<|Specifica l'ID del processo che si desidera terminare.|
|\<ProcessName >|Specifica il nome del processo che si desidera terminare. Questo parametro può includere caratteri jolly.|
|/Server:\<ServerName >|Specifica il server terminal che contiene il processo che si desidera terminare. Se **/Server** non è specificato, viene utilizzato il server Host sessione Desktop remoto corrente.|
|/ID:\<SessionID >|Termina il processo in esecuzione nella sessione specificata.|
|/a|Termina il processo in esecuzione in tutte le sessioni.|
|/v|Visualizza le informazioni sulle azioni eseguite.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni
- È possibile usare **tskill** per terminare solo i processi che appartengono all'utente, a meno che non si sia un amministratore. Gli amministratori hanno accesso completo a tutte le funzioni **tskill** e possono terminare i processi in esecuzione in altre sessioni utente.
- Quando tutti i processi in esecuzione in una sessione terminano, termina anche la sessione.
- Se si usano i parametri *ProcessName* e **/Server:** <em>ServerName</em> , è necessario specificare anche il parametro **/ID:** <em>SessionID</em> o **/a** .

## <a name="BKMK_examples"></a>Esempi
- Per terminare il processo 6543, digitare:
  ```
  tskill 6543
  ```
- Per terminare il processo "Explorer" in esecuzione nella sessione 5, digitare:
  ```
  tskill explorer /id:5
  ```
  #### <a name="additional-references"></a>Altri riferimenti
  [Chiave di sintassi della riga di comando](command-line-syntax-key.md)
  [riferimento ai comandi di &#40;Servizi Desktop remoto Servizi terminal&#41; ](remote-desktop-services-terminal-services-command-reference.md)
