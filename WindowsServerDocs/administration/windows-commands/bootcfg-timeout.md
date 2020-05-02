---
title: bootcfg timeout
description: Argomento di riferimento per il comando bootcfg timeout, che consente di modificare il valore di timeout del sistema operativo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aa858eac-2bb7-4a27-a9bc-3e4a6eb8b2c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e10ccab69ca58a6cef260546e965f90eecfc8beb
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82708939"
---
# <a name="bootcfg-timeout"></a>bootcfg timeout

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modifica il valore di timeout del sistema operativo.

## <a name="syntax"></a>Sintassi

```
bootcfg /timeout <timeoutvalue> [/s <computer> [/u <domain>\<user> /p <password>]]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `/timeout <timeoutvalue>` | Specifica il valore di timeout nella sezione [boot loader]. Il `<timeoutvalue>` è il numero di secondi, l'utente deve selezionare un sistema operativo dalla schermata del caricatore di avvio prima di NTLDR carica il valore predefinito. L'intervallo valido per `<timeoutvalue>` è 0-999. Se il valore è 0, NTLDR avvia immediatamente il sistema operativo predefinito senza visualizzare la schermata del caricatore di avvio. |
| `/s <computer>` | Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale. |
| `/u <domain>\<user>`  | Esegue il comando con le autorizzazioni dell'account dell'utente specificato da `<user>` o `<domain>\<user>`. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso. |
| `/p <password>` | Specifica la password dell'account utente specificato nella **/u** parametro. |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="examples"></a>Esempi

Per usare il comando **bootcfg/timeout** :

```
bootcfg /timeout 30
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /timeout 50
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando bootcfg](bootcfg.md)
