---
title: getmac
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a749a348-7cd1-4336-9f33-bb42dd0e31e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1fa37f558b454388b8451629ef96584ab8da0c3d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724973"
---
# <a name="getmac"></a>getmac

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Restituisce la media access control (MAC) indirizzo e l'elenco dei protocolli di rete associati con ogni indirizzo per tutte le schede di rete in ogni computer, localmente o attraverso una rete. 
## <a name="syntax"></a>Sintassi
```
getmac[.exe][/s <computer> [/u <Domain\<User> [/p <Password>]]][/fo {TABLE | list | CSV}][/nh][/v]
```
#### <a name="parameters"></a>Parametri

|             Parametro              |                                                                                          Descrizione                                                                                          |
|------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /s<computer>            |                                      Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale.                                       |
|        /u<Domain>\\<User>         | Esegue il comando con le autorizzazioni dell'account dell'utente specificato dall'utente o dominio\utente. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso. |
|           /p<Password>            |                                                     Specifica la password dell'account utente specificato nella **/u** parametro.                                                     |
| /fo {TABLE &#124; list&#124; CSV} |                       Specifica il formato da utilizzare per l'output della query. I valori validi sono **Table**, **List**e **CSV**. Il formato predefinito per l'output è **TABELLA**.                        |
|                /NH                 |                                             Omette le intestazioni di colonna nell'output. Valido quando il parametro **/fo** è impostato su **Table** o **CSV**.                                              |
|                 /v                 |                                                                    Specifica la visualizzazione di informazioni dettagliate.                                                                     |
|                 /?                 |                                                                                                                                                                                               |

## <a name="remarks"></a>Osservazioni
**getmac** può essere utile quando si vuole immettere l'indirizzo Mac in un analizzatore di rete o quando è necessario conoscere i protocolli attualmente in uso in ogni scheda di rete in un computer.
## <a name="examples"></a>Esempi
Gli esempi seguenti illustrano come utilizzare il **getmac** comando:
```
getmac /fo table /nh /v
```
```
getmac /s srvmain
```
```
getmac /s srvmain /u maindom\hiropln
```
```
getmac /s srvmain /u maindom\hiropln /p p@ssW23
```
```
getmac /s srvmain /u maindom\hiropln /p p@ssW23 /fo list /v
```
```
getmac /s srvmain /u maindom\hiropln /p p@ssW23 /fo table /nh
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
