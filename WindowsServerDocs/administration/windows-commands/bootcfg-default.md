---
title: bootcfg default
description: Argomento di riferimento per il comando predefinito bootcfg, che specifica la voce del sistema operativo da designare come impostazione predefinita.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e21824d7-8278-41d7-a2c5-ce09803d513a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 43a1c2b4bcbb4062a24d4c72f5856898afecd349
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82709694"
---
# <a name="bootcfg-default"></a>bootcfg default

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Specifica la voce del sistema operativo per specificare che il valore predefinito.

## <a name="syntax"></a>Sintassi

```
bootcfg /default [/s <computer> [/u <domain>\<user> /p <password>]] [/id <osentrylinenum>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `/s <computer>` | Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale. |
| `/u <domain>\<user>`  | Esegue il comando con le autorizzazioni dell'account dell'utente specificato da `<user>` o `<domain>\<user>`. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso. |
| `/p <password>` | Specifica la password dell'account utente specificato nella **/u** parametro. |
| `/id <osentrylinenum>` | Specifica il numero di riga voce del sistema operativo in della sezione [operating systems] del file Boot. ini alla quale aggiungere opzioni di caricamento del sistema operativo. La prima riga dopo la sezione [operating systems] sezione di intestazione è 1. |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="examples"></a>Esempi

Per usare il comando **bootcfg/valore predefinito** :

```
bootcfg /default /id 2
bootcfg /default /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando bootcfg](bootcfg.md)
