---
title: bootcfg debug
description: 'Argomento i comandi di Windows per **debug bootcfg** : aggiunge o modifica le impostazioni di debug per una voce del sistema operativo specificato.'
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 28afa5fb-a236-46e2-b1a4-a3c43a49c437
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 44a1145384a62d30f055cb48fd7ed6adccd2c69b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434818"
---
# <a name="bootcfg-debug"></a>bootcfg debug

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Aggiunge o modifica le impostazioni di debug per una voce del sistema operativo specificato.

## <a name="syntax"></a>Sintassi
```
bootcfg /debug {ON | OFF | edit}[/s <computer> [/u <Domain>\<User> /p <Password>]] [/port {COM1 | COM2 | COM3 | COM4}] [/baud {9600 | 19200 | 38400 | 57600 | 115200}] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Parametri

|                           Parametro                           |                                                                                                                                                                                                                    Descrizione                                                                                                                                                                                                                    |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                  {ON &#124; OFF&#124; edit}                   | Specifica il valore per il debug.<br /><br />**VIA** -Abilita il supporto del debug remoto aggiungendo l'opzione /debug specificato <OSEntryLineNum>.<br /><br />**DISATTIVARE** -Disabilita il supporto del debug remoto rimuovendo l'opzione /debug da specificato <OSEntryLineNum>.<br /><br />**modificare** -consente di apportare modifiche alla porta e trasmissione delle impostazioni di frequenza modificando i valori associati per l'oggetto specificato con l'opzione /debug <OSEntryLineNum>. |
|                         /s <computer>                         |                                                                                                                                                                Specifica il nome o indirizzo IP di un computer remoto (non utilizzare le barre rovesciate). Il valore predefinito è il computer locale.                                                                                                                                                                 |
|                      /u <Domain>\\<User>                      |                                                                                                                       Esegue il comando con le autorizzazioni dell'account dell'utente specificato da <User> oppure <Domain> \\ <User>. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso.                                                                                                                        |
|                         /p <Password>                         |                                                                                                                                                                               Specifica la password dell'account utente specificato nella **/u** parametro.                                                                                                                                                                               |
|       porta {COM1 &#124; COM2 &#124; COM3 &#124; COM4}        |                                                                                                                                                                Specifica la porta COM da utilizzare per il debug. Non utilizzare il **porta/** parametro se il debug è stato disabilitato.                                                                                                                                                                |
| /baud {9600 &#124; 19200 &#124; 38400 &#124; 57600 &#124; 115200} |                                                                                                                                                               Specifica la velocità in baud da utilizzare per il debug. Non utilizzare il **/baud** parametro se il debug è stato disabilitato.                                                                                                                                                                |
|                     /id <OSEntryLineNum>                      |                                                                                                               Specifica il numero di riga voce del sistema operativo in della sezione [operating systems] del file Boot. ini alla quale aggiungere le opzioni di debug. La prima riga dopo la sezione [operating systems] sezione di intestazione è 1.                                                                                                                |
|                              /?                               |                                                                                                                                                                                                       Visualizza la guida al prompt dei comandi.                                                                                                                                                                                                        |

##### <a name="remarks"></a>Note
- Se è richiesto il debug porta 1394, utilizzare [dbg1394 bootcfg](bootcfg-dbg1394.md).
  ## <a name="BKMK_examples"></a>Esempi
  Gli esempi seguenti illustrano come utilizzare il **bootcfg /debug**comando:
  ```
  bootcfg /debug on /port com1 /id 2 
  bootcfg /debug edit /port com2 /baud 19200 /id 2 
  bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /debug off /id 2
  ```
  #### <a name="additional-references"></a>Riferimenti aggiuntivi
  [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
