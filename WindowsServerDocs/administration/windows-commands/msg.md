---
title: msg
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9501cf3e-568e-4982-9987-8daecc6c17ff
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 68a393b57a255915b93759b4b26286ce4d838019
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437218"
---
# <a name="msg"></a>msg

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Invia un messaggio a un utente in un server Host sessione Desktop remoto (Host sessione rd).
Per esempi di come usare questo comando, vedere [esempi](#BKMK_examples).
> [!NOTE]
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per scoprire quali sono le novità nella versione più recente, vedere [novità in Servizi Desktop remoto in Windows Server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.

## <a name="syntax"></a>Sintassi
```
msg {<UserName> | <SessionName> | <SessionID>| @<FileName> | *} [/server:<ServerName>] [/time:<Seconds>] [/v] [/w] [<Message>]
```

## <a name="parameters"></a>Parametri

|      Parametro       |                                                                                                                               Descrizione                                                                                                                               |
|----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      <UserName>      |                                                                                                  Specifica il nome dell'utente che si desidera ricevere il messaggio.                                                                                                   |
|    <SessionName>     |                                                                                                 Specifica il nome della sessione che si desidera ricevere il messaggio.                                                                                                 |
|     <SessionID>      |                                                                                            Specifica l'ID numerico della sessione il cui utente che si desidera ricevere un messaggio.                                                                                            |
|     @<FileName>      |                                                                         Identifica un file contenente un elenco di nomi utente, i nomi delle sessioni e degli ID di sessione che si desidera ricevere il messaggio.                                                                         |
|          \*          |                                                                                                           Invia il messaggio a tutti i nomi utente nel sistema.                                                                                                            |
| /server:<ServerName> |                                              Specifica il server Host sessione Desktop remoto, la cui sessione o un utente che si desidera ricevere il messaggio. Se non viene specificato, **/server** Usa il server a cui si è attualmente connessi.                                              |
|   /time:<Seconds>    | Specifica la quantità di tempo in cui viene visualizzato il messaggio che è stato inviato sullo schermo dell'utente. Una volta raggiunto il limite di tempo, il messaggio scompare. Se non è impostato alcun limite di tempo, il messaggio rimane nella schermata dell'utente fino a quando l'utente vede il messaggio e fa clic su **OK**. |
|          /v          |                                                                                                         Consente di visualizzare informazioni sulle azioni eseguibili in esecuzione.                                                                                                         |
|          /w          |         Attende un riconoscimento da parte dell'utente che è stato ricevuto il messaggio. Usare questo parametro con **/ora:** <*secondi*> per evitare un lungo ritardo possibili se l'utente non risponde immediatamente. Utilizzo di questo parametro con **/v** è inoltre utile.          |
|      <Message>       |                  Specifica il testo del messaggio da inviare. Se non viene specificato alcun messaggio, verrà chiesto di immettere un messaggio. Per inviare un messaggio in cui è contenuto in un file, digitare il simbolo di (<) seguito dal nome del file maggiore.                  |
|          /?          |                                                                                                                  Visualizza la guida al prompt dei comandi.                                                                                                                   |

## <a name="remarks"></a>Note
-   Se non si specifica un utente o una sessione, **msg** viene visualizzato un messaggio di errore. Quando si specifica una sessione, deve essere di quella attiva.
-   L'utente deve avere autorizzazioni di accesso speciali di messaggi per inviare un messaggio.

## <a name="BKMK_examples"></a>Esempi
-   Per inviare che il messaggio intitolata "Vediamoci Vediamoci oggi" a tutte le sessioni per User1, digitare:
    ```
    msg User1 Let's meet at 1PM today
    ```
-   Per inviare lo stesso messaggio a modeM02 sessione, digitare:
    ```
    msg modem02 Let's meet at 1PM today
    ```
-   Per inviare il messaggio alla sessione 12, digitare:
    ```
    msg 12 Let's meet at 1PM today
    ```
-   Per inviare il messaggio a tutte le sessioni contenute nel file USERlist, digitare:
    ```
    msg @userlist Let's meet at 1PM today
    ```
-   Per inviare il messaggio a tutti gli utenti connessi al computer, digitare:
    ```
    msg * Let's meet at 1PM today
    ```
-   Per inviare il messaggio a tutti gli utenti con un timeout di conferma (ad esempio, 10 secondi), digitare:
    ```
    msg * /time:10 Let's meet at 1PM today
    ```

#### <a name="additional-references"></a>Riferimenti aggiuntivi
-  [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-  [Servizi Desktop remoto &#40;servizi Terminal&#41; Guida comandi](remote-desktop-services-terminal-services-command-reference.md)
