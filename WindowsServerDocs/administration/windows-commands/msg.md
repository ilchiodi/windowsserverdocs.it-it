---
title: msg
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9501cf3e-568e-4982-9987-8daecc6c17ff
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e44d754557f918c218e9e9d35149bffd983a0d54
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723860"
---
# <a name="msg"></a>msg

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Invia un messaggio a un utente in un server di host sessione Desktop remoto (host sessione Desktop remoto).

> [!NOTE]
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per informazioni sulle novità della versione più recente, vedere Novità di [Servizi Desktop remoto in Windows server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.

## <a name="syntax"></a>Sintassi
```
msg {<UserName> | <SessionName> | <SessionID>| @<FileName> | *} [/server:<ServerName>] [/time:<Seconds>] [/v] [/w] [<Message>]
```

### <a name="parameters"></a>Parametri

|      Parametro       |                                                                                                                               Descrizione                                                                                                                               |
|----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      <UserName>      |                                                                                                  Specifica il nome dell'utente a cui si desidera ricevere il messaggio.                                                                                                   |
|    <SessionName>     |                                                                                                 Specifica il nome della sessione a cui si desidera ricevere il messaggio.                                                                                                 |
|     <SessionID>      |                                                                                            Specifica l'ID numerico della sessione di cui si desidera ricevere un messaggio.                                                                                            |
|     @<FileName>      |                                                                         Identifica un file contenente un elenco di nomi utente, nomi di sessione e ID di sessione per i quali si desidera ricevere il messaggio.                                                                         |
|          \*          |                                                                                                           Invia il messaggio a tutti i nomi utente nel sistema.                                                                                                            |
| /server:<ServerName> |                                              Specifica il server Host sessione Desktop remoto la cui sessione o l'utente desidera ricevere il messaggio. Se non è specificato, **/Server** usa il server a cui si è attualmente connessi.                                              |
|   /ora<Seconds>    | Specifica la quantità di tempo durante la quale il messaggio inviato viene visualizzato sullo schermo dell'utente. Quando viene raggiunto il limite di tempo, il messaggio scompare. Se non è impostato alcun limite di tempo, il messaggio rimane sullo schermo dell'utente fino a quando l'utente non Visualizza il messaggio e fa clic su **OK**. |
|          /v          |                                                                                                         Visualizza le informazioni sulle azioni eseguite.                                                                                                         |
|          /W          |         Attende un riconoscimento da parte dell'utente che il messaggio è stato ricevuto. Usare questo parametro con **/ora:**<*seconds*> per evitare un possibile ritardo lungo se l'utente non risponde immediatamente. È inoltre utile utilizzare questo parametro con **/v** .          |
|      <Message>       |                  Specifica il testo del messaggio che si desidera inviare. Se non viene specificato alcun messaggio, verrà richiesto di immettere un messaggio. Per inviare un messaggio contenuto in un file, digitare il simbolo di minore di (<) seguito dal nome del file.                  |
|          /?          |                                                                                                                  Visualizza la guida al prompt dei comandi.                                                                                                                   |

## <a name="remarks"></a>Osservazioni
-   Se non si specifica un utente o una sessione, in **msg** viene visualizzato un messaggio di errore. Quando si specifica una sessione, è necessario che sia attiva.
-   Per inviare un messaggio, l'utente deve disporre dell'autorizzazione di accesso speciale Message.

## <a name="examples"></a>Esempi
-   Per inviare il messaggio di cui si ha diritto, è possibile incontrarlo alle 13.00 di oggi per tutte le sessioni per User1, digitare:
    ```
    msg User1 Let's meet at 1PM today
    ```
-   Per inviare lo stesso messaggio a Session modeM02, digitare:
    ```
    msg modem02 Let's meet at 1PM today
    ```
-   Per inviare il messaggio alla sessione 12, digitare:
    ```
    msg 12 Let's meet at 1PM today
    ```
-   Per inviare il messaggio a tutte le sessioni contenute nel file USERs, digitare:
    ```
    msg @userlist Let's meet at 1PM today
    ```
-   Per inviare il messaggio a tutti gli utenti connessi, digitare:
    ```
    msg * Let's meet at 1PM today
    ```
-   Per inviare il messaggio a tutti gli utenti, con un timeout di riconoscimento (ad esempio, 10 secondi), digitare:
    ```
    msg * /time:10 Let's meet at 1PM today
    ```

## <a name="additional-references"></a>Riferimenti aggiuntivi
-  - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-  [Informazioni di riferimento sui comandi di Servizi Desktop remoto (Servizi terminal)](remote-desktop-services-terminal-services-command-reference.md)
