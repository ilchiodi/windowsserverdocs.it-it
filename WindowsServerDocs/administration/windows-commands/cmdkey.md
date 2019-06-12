---
title: cmdkey
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 5c06a04fa6473bc30c3b354f049a55775d2308a0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434308"
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
|         /add:<TargetName>          | Aggiunge un nome utente e una password per l'elenco.<br /><br />Richiede che il parametro di <TargetName> che identifica il nome di dominio o computer che verrà associato questo movimento. |
|       /generic:<TargetName>        |   Aggiunge le credenziali generiche all'elenco.<br /><br />Richiede che il parametro di <TargetName> che identifica il nome di dominio o computer che verrà associato questo movimento.    |
|             /smartcard             |                                                                    Recupera le credenziali da una smart card.                                                                     |
|          /User:<UserName>          |                                 Specifica l'utente o il nome dell'account per l'archiviazione con questo movimento. Se *UserName* viene omesso, si verrà richiesto.                                  |
|          /pass:<Password>          |                                       Specifica la password per l'archiviazione con questo movimento. Se *Password* viene omesso, si verrà richiesto.                                        |
| /delete{:<TargetName> &#124; /ras} |  Elimina un nome utente e password dall'elenco. Se *TargetName* viene specificato, che verrà eliminata la voce. Se viene specificato /ras, verrà eliminata la voce di accesso remoto archiviati.   |
|         /List:<TargetName>         |                  Visualizza l'elenco di nomi utente archiviato e le credenziali. Se *TargetName* non i nomi utente specificato, tutte le stored e le credenziali verranno elencate.                   |
|                 /?                 |                                                                        Visualizza la guida al prompt dei comandi.                                                                        |

## <a name="remarks"></a>Note
- Se viene trovata più di una smart card nel sistema quando viene utilizzata l'opzione della riga di comando di /smartcard, **cmdkey** visualizzerà informazioni su tutte le smart card disponibile e quindi verrà richiesto all'utente di specificare l'immagine da usare.
- Le password non verranno visualizzate dopo che sono archiviati.
  ## <a name="BKMK_examples"></a>Esempi
  Per visualizzare un elenco di tutti i nomi utente e le credenziali archiviate, digitare:
  ```
  cmdkey /list
  ```
  Per aggiungere un nome utente e password per consentire all'utente bianchi accesso computer Server01 con la password Kleo, digitare:
  ```
  cmdkey /add:server01 /user:mikedan /pass:Kleo
  ```
  Per aggiungere un nome utente e password per consentire all'utente bianchi per accedere a computer Server01 e Richiedi la password ogni volta che si accede a Server01, digitare:
  ```
  cmdkey /add:server01 /user:mikedan
  ```
  Per eliminare le credenziali di accesso remoto è archiviato, digitare:
  ```
  cmdkey /delete /ras
  ```
  Per eliminare le credenziali archiviate per Server01, digitare:
  ```
  cmdkey /delete:Server01
  ```
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
  [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
