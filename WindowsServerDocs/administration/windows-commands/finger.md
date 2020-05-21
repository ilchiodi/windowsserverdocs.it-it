---
title: finger
description: Argomento di riferimento per il comando finger, che visualizza le informazioni sugli utenti in un computer remoto specificato che esegue il servizio Finger o daemon.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 907ea637-5c6c-4752-84c2-46bbf2a68a33
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a3403e10a1777bc117659eb052958d3a20668557
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83437236"
---
# <a name="finger"></a>finger

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza informazioni sugli utenti in un computer remoto specificato (in genere un computer che esegue UNIX) che esegue il servizio Finger o daemon. Il computer remoto specifica il formato e l'output della visualizzazione di informazioni dell'utente. Se utilizzato senza parametri, **dito** Visualizza la Guida.

> [!IMPORTANT]
> Questo comando è disponibile solo se è installato il protocollo Internet Protocol (TCP/IP) come componente nelle proprietà di una scheda di rete in connessioni di rete.

## <a name="syntax"></a>Sintassi

```
finger [-l] [<user>] [@<host>] [...]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| -l | Visualizza le informazioni utente in formato lungo. |
| `<user>` | Specifica l'utente sul quale si desiderano informazioni. Se si omette il parametro *User* , questo comando Visualizza le informazioni su tutti gli utenti nel computer specificato. |
| `@<host>` | Specifica il computer remoto che esegue il servizio Finger in cui si stanno cercando le informazioni sull'utente. È possibile specificare un indirizzo IP o il nome del computer. |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Osservazioni

- È necessario anteporre **dito** parametri con un trattino (-) anziché una barra (/).

- `user@host`È possibile specificare più parametri.

### <a name="examples"></a>Esempi

Per visualizzare informazioni per *User1* nel computer *Users.Microsoft.com*, digitare:

```
finger user1@users.microsoft.com
```

Per visualizzare le informazioni relative a *tutti gli utenti* nel computer *Users.Microsoft.com*, digitare:

```
finger @users.microsoft.com
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
