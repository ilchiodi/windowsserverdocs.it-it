---
title: bootcfg dbg1394
description: Argomento di riferimento per il comando bootcfg dbg1394, che configura il debug della porta 1394 per una voce del sistema operativo specificata
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 35724697-90dd-4dbe-85b0-337fbd369dcc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 16230c52657fd5c9c14972726ed2465401995223
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82709698"
---
# <a name="bootcfg-dbg1394"></a>bootcfg dbg1394

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di configurare il debug 1394 porta per una voce del sistema operativo specificato.

## <a name="syntax"></a>Sintassi

```
bootcfg /dbg1394 {on | off}[/s <computer> [/u <domain>\<user> /p <password>]] [/ch <channel>] /id <osentrylinenum>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `{on | off}` | Specifica il valore per il debug della porta 1394, tra cui:<ul><li>**in.** Abilita il supporto del debug remoto aggiungendo l'opzione/dbg1394 all'oggetto `<osentrylinenum>`specificato.</li><li>**off.** Disabilita il supporto del debug remoto rimuovendo l'opzione/dbg1394 dall'oggetto specificato <osentrylinenum>.</li></ul> |
| `/s <computer>` | Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale. |
| `/u <domain>\<user>`  | Esegue il comando con le autorizzazioni dell'account dell'utente specificato da `<user>` o `<domain>\<user>`. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso. |
| `/p <password>` | Specifica la password dell'account utente specificato nella **/u** parametro. |
| `/ch <channel>` | Specifica il canale da utilizzare per il debug. I valori validi includono numeri interi compresi tra 1 e 64. Non usare questo parametro se il debug della porta 1394 è disabilitato. |
| `/id <osentrylinenum>` | Specifica il numero di riga voce del sistema operativo in della sezione [operating systems] del file Boot. ini alla quale aggiungere opzioni di caricamento del sistema operativo. La prima riga dopo la sezione [operating systems] sezione di intestazione è 1. |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="examples"></a>Esempi

Per usare il comando **bootcfg/dbg1394**:

```
bootcfg /dbg1394 /id 2
bootcfg /dbg1394 on /ch 1 /id 3
bootcfg /dbg1394 edit /ch 8 /id 2
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /dbg1394 off /id 2
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando bootcfg](bootcfg.md)
