---
title: getmac
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a749a348-7cd1-4336-9f33-bb42dd0e31e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6b593bf61bb08d2c1c7868b1bbb175ed64a8bcf7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842614"
---
# <a name="getmac"></a>getmac

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Restituisce la media access control (MAC) indirizzo e l'elenco dei protocolli di rete associati con ogni indirizzo per tutte le schede di rete in ogni computer, localmente o attraverso una rete. 
## <a name="syntax"></a>Sintassi
```
getmac[.exe][/s <computer> [/u <Domain\<User> [/p <Password>]]][/fo {TABLE | list | CSV}][/nh][/v]
```
#### <a name="parameters"></a>Parametri

|             Parametro              |                                                                                          Descrizione                                                                                          |
|------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /s <computer>            |                                      Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale.                                       |
|        /u <Domain>\\<User>         | Esegue il comando con le autorizzazioni dell'account dell'utente specificato dall'utente o dominio\utente. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso. |
|           /p <Password>            |                                                     Specifica la password dell'account utente specificato nella **/u** parametro.                                                     |
| /FO {elenco &#124; &#124; tabella CSV} |                       Specifica il formato da utilizzare per l'output della query. I valori validi sono **Table**, **List**e **CSV**. Il formato predefinito per l'output è **TABELLA**.                        |
|                /NH                 |                                             Omette le intestazioni di colonna nell'output. Valido quando il **/fo** parametro è impostato su **TABELLA** o **CSV**.                                              |
|                 /v                 |                                                                    Specifica la visualizzazione di informazioni dettagliate.                                                                     |
|                 /?                 |                                                                                                                                                                                               |

## <a name="remarks"></a>Note
**getmac** può essere utile quando si vuole immettere l'indirizzo Mac in un analizzatore di rete o quando è necessario conoscere i protocolli attualmente in uso in ogni scheda di rete in un computer.
## <a name="examples"></a><a name=BKMK_Examples></a>Esempi
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
## <a name="additional-references"></a>Altre informazioni di riferimento
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
