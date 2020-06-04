---
title: msg
description: Argomento di riferimento per il comando msg, che invia un messaggio a un utente su un server host sessione Desktop remoto
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9501cf3e-568e-4982-9987-8daecc6c17ff
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5eca19eee696e7a45cec2f16398055a7b06d00d6
ms.sourcegitcommit: 5e313a004663adb54c90962cfdad9ae889246151
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2020
ms.locfileid: "84354361"
---
# <a name="msg"></a>msg

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Invia un messaggio a un utente su un server host sessione Desktop remoto.

> [!NOTE]
> Per inviare un messaggio, è necessario disporre delle autorizzazioni di accesso speciale al messaggio.

## <a name="syntax"></a>Sintassi

```
msg {<username> | <sessionname> | <sessionID>| @<filename> | *} [/server:<servername>] [/time:<seconds>] [/v] [/w] [<message>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<username>` | Specifica il nome dell'utente a cui si desidera ricevere il messaggio. Se non si specifica un utente o una sessione, questo comando Visualizza un messaggio di errore. Quando si specifica una sessione, è necessario che sia attiva. |
| `<sessionname>` | Specifica il nome della sessione a cui si desidera ricevere il messaggio. Se non si specifica un utente o una sessione, questo comando Visualizza un messaggio di errore. Quando si specifica una sessione, è necessario che sia attiva. |
| `<sessionID>` | Specifica l'ID numerico della sessione di cui si desidera ricevere un messaggio. |
| `@<filename>` | Identifica un file contenente un elenco di nomi utente, nomi di sessione e ID di sessione per i quali si desidera ricevere il messaggio. |
| * | Invia il messaggio a tutti i nomi utente nel sistema. |
| /server:`<servername>` | Specifica il server host sessione Desktop remoto la cui sessione o l'utente desidera ricevere il messaggio. Se non è specificato, **/Server** usa il server a cui si è attualmente connessi. |
| /ora`<seconds>` | Specifica la quantità di tempo durante la quale il messaggio inviato viene visualizzato sullo schermo dell'utente. Quando viene raggiunto il limite di tempo, il messaggio scompare. Se non è impostato alcun limite di tempo, il messaggio rimane sullo schermo dell'utente fino a quando l'utente non Visualizza il messaggio e fa clic su **OK**. |
| /v | Visualizza le informazioni sulle azioni eseguite. |
| /W | Attende un riconoscimento da parte dell'utente che il messaggio è stato ricevuto. Utilizzare questo parametro con `/time:<*seconds*>` per evitare un possibile ritardo lungo se l'utente non risponde immediatamente. È inoltre utile utilizzare questo parametro con **/v** . |
| `<message>` | Specifica il testo del messaggio che si desidera inviare. Se non viene specificato alcun messaggio, verrà richiesto di immettere un messaggio. Per inviare un messaggio contenuto in un file, digitare il simbolo di minore di (<) seguito dal nome del file. |
| /? | Visualizza la guida al prompt dei comandi. |

### <a name="examples"></a>Esempio

Per inviare un messaggio intitolato, *ora è possibile incontrare le ore 13.00* per tutte le sessioni per *User1*, digitare:

```
msg User1 Let's meet at 1PM today
```

Per inviare lo stesso messaggio a Session *modeM02*, digitare:

```
msg modem02 Let's meet at 1PM today
```

Per inviare il messaggio a tutte le sessioni contenute nel file *Users*, digitare:

```
msg @userlist Let's meet at 1PM today
```

Per inviare il messaggio a tutti gli utenti connessi, digitare:

```
msg * Let's meet at 1PM today
```

Per inviare il messaggio a tutti gli utenti, con un timeout di riconoscimento (ad esempio, 10 secondi), digitare:

```
msg * /time:10 Let's meet at 1PM today
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
