---
title: cmdkey
description: Windows Commands argomento per cmdkey, che consente di creare, elencare ed eliminare nomi utente e password archiviati e credenziali.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5fcd68ee-a14a-4b71-9300-c3f5c5d31e8e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cdb732bf95e30af012f78d1bad337d6d6d191268
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847594"
---
# <a name="cmdkey"></a>cmdkey

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea, elenca ed elimina nomi utente e password o credenziali archiviati.

## <a name="syntax"></a>Sintassi
```
cmdkey [{/add:<TargetName>|/generic:<TargetName>}] {/smartcard|/user:<UserName> [/pass:<Password>]} [/delete{:<TargetName>|/ras}] /list:<TargetName>
```
### <a name="parameters"></a>Parametri

|             Parametri             |                                                                                    Descrizione                                                                                     |
|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         /Add:<TargetName>          | aggiunge un nome utente e una password all'elenco.<p>Richiede il parametro di <TargetName> che identifica il nome del computer o del dominio a cui verrà associata questa voce. |
|       /problemi generici:<TargetName>        |   aggiunge credenziali generiche all'elenco.<p>Richiede il parametro di <TargetName> che identifica il nome del computer o del dominio a cui verrà associata questa voce.    |
|             /Smartcard             |                                                                    Recupera le credenziali da una smart card.                                                                     |
|          /User:<UserName>          |                                 Specifica il nome dell'utente o dell'account da archiviare con questa voce. Se il *nome utente* non viene specificato, verrà richiesto.                                  |
|          /Pass:<Password>          |                                       Specifica la password da archiviare con questa voce. Se la *password* non viene specificata, verrà richiesta.                                        |
| /Delete{:<TargetName> &#124; /RAS} |  Elimina un nome utente e una password dall'elenco. Se *TargetName* è specificato, la voce verrà eliminata. Se si specifica/RAS, la voce di accesso remoto archiviata verrà eliminata.   |
|         /list:<TargetName>         |                  Visualizza l'elenco dei nomi utente e delle credenziali archiviati. Se *TargetName* non è specificato, verranno elencati tutti i nomi utente e le credenziali archiviati.                   |
|                 /?                 |                                                                        Visualizza la guida al prompt dei comandi.                                                                        |

## <a name="remarks"></a>Note
- Se viene trovata più di una smart card nel sistema quando viene usata l'opzione della riga di comando/smartcard, **cmdkey** visualizzerà informazioni su tutte le smart card disponibili, quindi chiederà all'utente di specificare quale usare.
- Le password non verranno visualizzate dopo l'archiviazione.
  ## <a name="examples"></a><a name=BKMK_examples></a>Esempi
  Per visualizzare un elenco di tutti i nomi utente e le credenziali archiviati, digitare:
  ```
  cmdkey /list
  ```
  Per aggiungere un nome utente e una password per l'utente Zaffaronif per accedere al computer Server01 con la password Kleo, digitare:
  ```
  cmdkey /add:server01 /user:mikedan /pass:Kleo
  ```
  Per aggiungere un nome utente e una password per l'utente Zaffaronif per accedere al computer Server01 e richiedere la password ogni volta che viene eseguito l'accesso a Server01, digitare:
  ```
  cmdkey /add:server01 /user:mikedan
  ```
  Per eliminare le credenziali archiviate da accesso remoto, digitare:
  ```
  cmdkey /delete /ras
  ```
  Per eliminare le credenziali archiviate per Server01, digitare:
  ```
  cmdkey /delete:Server01
  ```
  ## <a name="additional-references"></a>Altre informazioni di riferimento
  - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
