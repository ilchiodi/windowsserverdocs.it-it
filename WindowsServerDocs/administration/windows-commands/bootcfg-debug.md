---
title: bootcfg debug
description: Argomento di riferimento per il comando bootcfg debug, che consente di aggiungere o modificare le impostazioni di debug per una voce del sistema operativo specificata.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 28afa5fb-a236-46e2-b1a4-a3c43a49c437
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c8059aaefd1b23b3e74f4c27ba96e322c44b5cb6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82709713"
---
# <a name="bootcfg-debug"></a>bootcfg debug

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Aggiunge o modifica le impostazioni di debug per una voce del sistema operativo specificato.

>[!NOTE]
> Se si sta tentando di eseguire il debug della porta 1394, usare invece il comando [bootcfg dbg1394](bootcfg-dbg1394.md) .

## <a name="syntax"></a>Sintassi

```
bootcfg /debug {on | off | edit}[/s <computer> [/u <domain>\<user> /p <password>]] [/port {COM1 | COM2 | COM3 | COM4}] [/baud {9600 | 19200 | 38400 | 57600 | 115200}] [/id <osentrylinenum>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `{on | off | edit}` | Specifica il valore per il debug della porta, tra cui:<ul><li>**in.** Abilita il supporto del debug remoto aggiungendo l'opzione/debug all'oggetto `<osentrylinenum>`specificato.</li><li>**off.** Disabilita il supporto del debug remoto rimuovendo l'opzione/debug dall'oggetto specificato <osentrylinenum>.</li><li>**modifica.** Consente di modificare le impostazioni della velocità della porta e della velocità in baud cambiando i valori associati all'opzione <osentrylinenum>/debug per l'oggetto specificato.</li></ul> |
| `/s <computer>` | Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale. |
| `/u <domain>\<user>`  | Esegue il comando con le autorizzazioni dell'account dell'utente specificato da `<user>` o `<domain>\<user>`. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso. |
| `/p <password>` | Specifica la password dell'account utente specificato nella **/u** parametro. |
| `/port {COM1 | COM2 | COM3 | COM4}` |  Specifica la porta COM da utilizzare per il debug. Non usare questo parametro se il debug è disabilitato. |
| `/baud {9600 | 19200 | 38400 | 57600 | 115200}` | Specifica la velocità in baud da utilizzare per il debug. Non usare questo parametro se il debug è disabilitato. |
| `/id <osentrylinenum>` | Specifica il numero di riga voce del sistema operativo in della sezione [operating systems] del file Boot. ini alla quale aggiungere opzioni di caricamento del sistema operativo. La prima riga dopo la sezione [operating systems] sezione di intestazione è 1. |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="examples"></a>Esempi

Per utilizzare il comando **bootcfg/debug** :

```
bootcfg /debug on /port com1 /id 2
bootcfg /debug edit /port com2 /baud 19200 /id 2
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /debug off /id 2
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando bootcfg](bootcfg.md)
