---
title: bootcfg query
description: Argomento di riferimento per il comando di query bootcfg, che esegue query e visualizza le voci della sezione del caricatore di avvio e del sistema operativo da boot. ini.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a4cacfd1-10a6-4a11-b0c5-f8abde72bfc8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 44c5ce60f55618d16e80d1f1efde3af1cbf6c609
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82709336"
---
# <a name="bootcfg-query"></a>bootcfg query

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esegue query e Visualizza [caricatore di avvio] e le voci da Boot. ini di sezione [operating systems].

## <a name="syntax"></a>Sintassi

```
bootcfg /query [/s <computer> [/u <domain>\<user> /p <password>]]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `/s <computer>` | Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale. |
| `/u <domain>\<user>`  | Esegue il comando con le autorizzazioni dell'account dell'utente specificato da `<user>` o `<domain>\<user>`. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso. |
| `/p <password>` | Specifica la password dell'account utente specificato nella **/u** parametro. |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="sample-output"></a>Output di esempio

Output di esempio per il comando **bootcfg/query** :
  
```
Boot Loader Settings
----------
timeout: 30
default: multi(0)disk(0)rdisk(0)partition(1)\WINDOWS
Boot Entries
------
Boot entry ID:   1
Friendly Name:
path: multi(0)disk(0)rdisk(0)partition(1)\WINDOWS
OS Load Options: /fastdetect /debug /debugport=com1:
```

- L'area **impostazioni del caricatore di avvio** Mostra ogni voce nella sezione [boot loader] di boot. ini.

- L'area **voci di avvio** Mostra ulteriori dettagli per ogni voce del sistema operativo nella sezione [Operating Systems] del boot. ini

## <a name="examples"></a>Esempi

Per usare il comando **bootcfg/query** :

```
bootcfg /query
bootcfg /query /s srvmain /u maindom\hiropln /p p@ssW23
bootcfg /query /u hiropln /p p@ssW23
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando bootcfg](bootcfg.md)
