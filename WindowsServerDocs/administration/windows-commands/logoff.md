---
title: disconnessione
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 939f09cc-de8c-436c-a05d-aca5f2a06371
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1661a9dd6cc89ea05980fd9085aa8fa67b8fe2c0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840414"
---
# <a name="logoff"></a>disconnessione

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Disconnette un utente da una sessione in un server di host sessione Desktop remoto (host sessione Desktop remoto) ed elimina la sessione dal server.
per esempi relativi all'uso di questo comando, vedere [esempi](#BKMK_examples).

> [!NOTE]
> In Windows Server 2008 R2 Servizi terminal è stato rinomato Servizi Desktop remoto. Per informazioni sulle novità della versione più recente, vedere Novità di [Servizi Desktop remoto in Windows server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.

## <a name="syntax"></a>Sintassi
```
logoff [<SessionName> | <SessionID>] [/server:<ServerName>] [/v]
```
### <a name="parameters"></a>Parametri

|      Parametro       |                                                                             Descrizione                                                                              |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <SessionName>     |                                                                  Specifica il nome della sessione.                                                                  |
|     <SessionID>      |                                                 Specifica l'ID numerico che identifica la sessione sul server.                                                 |
| /server:<ServerName> | Specifica il server Host sessione Desktop remoto che contiene la sessione di cui si desidera disconnettere l'utente. Se non viene specificato, viene utilizzato il server in cui si è attualmente attivo. |
|          /v          |                                                       Visualizza le informazioni sulle azioni eseguite.                                                        |
|          /?          |                                                                 Visualizza la guida al prompt dei comandi.                                                                 |

## <a name="remarks"></a>Note
- È sempre possibile disconnettersi dalla sessione a cui si è attualmente connessi. È necessario, tuttavia, disporre dell'autorizzazione controllo completo per disconnettere gli utenti da altre sessioni.
- La disconnessione di un utente da una sessione senza preavviso può comportare la perdita di dati in sessione dell'utente. È necessario inviare un messaggio all'utente tramite il **msg** comando per avvertire l'utente prima di effettuare questa operazione.
- Se <*SessionID*> o <*sessionname*> non è specificato, la **disconnessione** disconnette l'utente dalla sessione corrente. Se si specifica <*SessionName*>, deve essere di quella attiva.
- Quando ci disconnette un utente, tutti i processi e la sessione viene eliminato dal server.
- Non è possibile disconnettere un utente dalla sessione della console.
  ## <a name="examples"></a><a name=BKMK_examples></a>Esempi
- Per disconnettere un utente dalla sessione corrente, digitare:
  ```
  logoff
  ```
- Per disconnettere un utente da una sessione utilizzando l'ID della sessione, ad esempio sessione 12, digitare:
  ```
  logoff 12
  ```
- Per disconnettere un utente da una sessione utilizzando il nome di sessione e di server, ad esempio sessione TERM04 su Server1, digitare:
  ```
  logoff TERM04 /server:Server1
  ```

## <a name="additional-references"></a>Altre informazioni di riferimento
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Informazioni di riferimento sui comandi di Servizi Desktop remoto (Servizi terminal)](remote-desktop-services-terminal-services-command-reference.md)
