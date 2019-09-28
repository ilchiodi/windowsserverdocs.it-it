---
title: cmdkey
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5fcd68ee-a14a-4b71-9300-c3f5c5d31e8e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dc2b12cb53eef930d05c1e291de5574a8ba94306
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379315"
---
# <a name="cmdkey"></a>cmdkey

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea, elenca ed elimina nomi utente e password o credenziali archiviati.

## <a name="syntax"></a>Sintassi
```
cmdkey [{/add:<TargetName>|/generic:<TargetName>}] {/smartcard|/user:<UserName> [/pass:<Password>]} [/delete{:<TargetName>|/ras}] /list:<TargetName>
```
## <a name="parameters"></a>Parametri

|             Parametri             |                                                                                    Descrizione                                                                                     |
|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         /Add: <TargetName>          | aggiunge un nome utente e una password all'elenco.<br /><br />Richiede il parametro di <TargetName> che identifica il computer o il nome di dominio a cui verrà associata questa voce. |
|       /problemi generici: <TargetName>        |   aggiunge credenziali generiche all'elenco.<br /><br />Richiede il parametro di <TargetName> che identifica il computer o il nome di dominio a cui verrà associata questa voce.    |
|             /Smartcard             |                                                                    Recupera le credenziali da una smart card.                                                                     |
|          /User: <UserName>          |                                 Specifica il nome dell'utente o dell'account da archiviare con questa voce. Se il *nome utente* non viene specificato, verrà richiesto.                                  |
|          /Pass: <Password>          |                                       Specifica la password da archiviare con questa voce. Se la *password* non viene specificata, verrà richiesta.                                        |
| /Delete{: <TargetName> &#124; /RAS} |  Elimina un nome utente e una password dall'elenco. Se *TargetName* è specificato, la voce verrà eliminata. Se si specifica/RAS, la voce di accesso remoto archiviata verrà eliminata.   |
|         /list: <TargetName>         |                  Visualizza l'elenco dei nomi utente e delle credenziali archiviati. Se *TargetName* non è specificato, verranno elencati tutti i nomi utente e le credenziali archiviati.                   |
|                 /?                 |                                                                        Visualizza la guida al prompt dei comandi.                                                                        |

## <a name="remarks"></a>Note
- Se viene trovata più di una smart card nel sistema quando viene usata l'opzione della riga di comando/smartcard, **cmdkey** visualizzerà informazioni su tutte le smart card disponibili, quindi chiederà all'utente di specificare quale usare.
- Le password non verranno visualizzate dopo l'archiviazione.
  ## <a name="BKMK_examples"></a>Esempi
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
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
  [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
