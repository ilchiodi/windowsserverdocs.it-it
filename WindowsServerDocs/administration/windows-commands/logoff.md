---
title: disconnessione
description: Argomento di riferimento per il comando di disconnessione, che consente di disconnettere un utente da una sessione in un server host sessione Desktop remoto ed eliminare la sessione.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 939f09cc-de8c-436c-a05d-aca5f2a06371
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 636591843ce878bc85c5cccf6faece6652e25424
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222731"
---
# <a name="logoff"></a>disconnessione

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Disconnette un utente da una sessione in un server host sessione Desktop remoto ed elimina la sessione.

## <a name="syntax"></a>Sintassi
```
logoff [<sessionname> | <sessionID>] [/server:<servername>] [/v]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<sessionname>` | Specifica il nome della sessione. Deve trattarsi di una sessione attiva.|
| `<sessionID>` | Specifica l'ID numerico che identifica la sessione sul server. |
| /server:`<servername>` | Specifica il server host sessione Desktop remoto che contiene la sessione di cui si desidera disconnettere l'utente. Se non viene specificato, viene utilizzato il server in cui si è attualmente attivo. |
| /v | Visualizza le informazioni sulle azioni eseguite. |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Commenti

- È sempre possibile disconnettersi dalla sessione a cui si è attualmente connessi. È necessario, tuttavia, disporre dell'autorizzazione **controllo completo** per disconnettere gli utenti da altre sessioni.

- La disconnessione di un utente da una sessione senza preavviso può comportare la perdita di dati in sessione dell'utente. È necessario inviare un messaggio all'utente tramite il **msg** comando per avvertire l'utente prima di effettuare questa operazione.

- Se `<sessionID>` o `<sessionname>` non è specificato, la **disconnessione** disconnette l'utente dalla sessione corrente.

- Dopo la disconnessione di un utente, tutti i processi terminano e la sessione viene eliminata dal server.

- Non è possibile disconnettere un utente dalla sessione della console.

### <a name="examples"></a>Esempi

Per disconnettere un utente dalla sessione corrente, digitare:

```
logoff
```

Per disconnettere un utente da una sessione usando l'ID della sessione, ad esempio *Session 12*, digitare:

```
logoff 12
```

Per disconnettere un utente da una sessione usando il nome della sessione e del server, ad esempio Session *TERM04* su *Server1*, digitare:

```
logoff TERM04 /server:Server1
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Informazioni di riferimento sui comandi di Servizi Desktop remoto (Servizi terminal)](remote-desktop-services-terminal-services-command-reference.md)
