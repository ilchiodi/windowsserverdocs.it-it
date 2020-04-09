---
title: bootcfg debug
description: Argomento dei comandi di Windows per bootcfg debug, che aggiunge o modifica le impostazioni di debug per una voce del sistema operativo specificata.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 28afa5fb-a236-46e2-b1a4-a3c43a49c437
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: de225e1bd0f8406a0b28e5af28fd29bceb8e713c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848634"
---
# <a name="bootcfg-debug"></a>bootcfg debug

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Aggiunge o modifica le impostazioni di debug per una voce del sistema operativo specificato.

## <a name="syntax"></a>Sintassi
```
bootcfg /debug {ON | OFF | edit}[/s <computer> [/u <Domain>\<User> /p <Password>]] [/port {COM1 | COM2 | COM3 | COM4}] [/baud {9600 | 19200 | 38400 | 57600 | 115200}] [/id <OSEntryLineNum>]
```
### <a name="parameters"></a>Parametri

|                           Parametro                           |                                                                                                                                                                                                                    Descrizione                                                                                                                                                                                                                    |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                  {ON &#124; OFF&#124; edit}                   | Specifica il valore per il debug.<p>**VIA** -Abilita il supporto del debug remoto aggiungendo l'opzione /debug specificato <OSEntryLineNum>.<p>**DISATTIVARE** -Disabilita il supporto del debug remoto rimuovendo l'opzione /debug da specificato <OSEntryLineNum>.<p>**modifica** : consente di modificare le impostazioni della velocità della porta e della velocità in baud cambiando i valori associati all'opzione/debug per il <OSEntryLineNum>specificato. |
|                         /s <computer>                         |                                                                                                                                                                Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale.                                                                                                                                                                 |
|                      /u <Domain>\\<User>                      |                                                                                                                       Esegue il comando con le autorizzazioni dell'account dell'utente specificato da <User> o <Domain>\\<User>. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso.                                                                                                                        |
|                         /p <Password>                         |                                                                                                                                                                               Specifica la password dell'account utente specificato nella **/u** parametro.                                                                                                                                                                               |
|       porta {COM1 & #124; COM2 & #124; COM3 & #124; COM4}        |                                                                                                                                                                Specifica la porta COM da utilizzare per il debug. Non utilizzare il **porta/** parametro se il debug è stato disabilitato.                                                                                                                                                                |
| /baud {9600 & #124; 19200 & #124; 38400 & #124; 57600 & #124; 115200} |                                                                                                                                                               Specifica la velocità in baud da utilizzare per il debug. Non utilizzare il **/baud** parametro se il debug è stato disabilitato.                                                                                                                                                                |
|                     <OSEntryLineNum>/ID                      |                                                                                                               Specifica il numero di riga voce del sistema operativo in della sezione [operating systems] del file Boot. ini alla quale aggiungere le opzioni di debug. La prima riga dopo la sezione [operating systems] sezione di intestazione è 1.                                                                                                                |
|                              /?                               |                                                                                                                                                                                                       Visualizza la guida al prompt dei comandi.                                                                                                                                                                                                        |

##### <a name="remarks"></a>Note
- Se è richiesto il debug della porta 1394, usare [bootcfg dbg1394](bootcfg-dbg1394.md).
  ## <a name="examples"></a><a name=BKMK_examples></a>Esempi
  Gli esempi seguenti illustrano come utilizzare il **bootcfg /debug**comando:
  ```
  bootcfg /debug on /port com1 /id 2 
  bootcfg /debug edit /port com2 /baud 19200 /id 2 
  bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /debug off /id 2
  ```
  ## <a name="additional-references"></a>Altre informazioni di riferimento
  - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
