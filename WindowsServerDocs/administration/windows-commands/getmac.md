---
title: getmac
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a749a348-7cd1-4336-9f33-bb42dd0e31e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b1266b7368f1b073e00735a8d3362c75305d7c0f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438275"
---
# <a name="getmac"></a>getmac

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Restituisce la media access control (MAC) indirizzo e l'elenco dei protocolli di rete associati con ogni indirizzo per tutte le schede di rete in ogni computer, localmente o attraverso una rete. 
## <a name="syntax"></a>Sintassi
```
getmac[.exe][/s <computer> [/u <Domain\<User> [/p <Password>]]][/fo {TABLE | list | CSV}][/nh][/v]
```
### <a name="parameters"></a>Parametri

|             Parametro              |                                                                                          Descrizione                                                                                          |
|------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /s <computer>            |                                      Specifica il nome o indirizzo IP di un computer remoto (non utilizzare le barre rovesciate). Il valore predefinito è il computer locale.                                       |
|        /u <Domain>\\<User>         | Esegue il comando con le autorizzazioni dell'account dell'utente specificato dall'utente o dominio\utente. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso. |
|           /p <Password>            |                                                     Specifica la password dell'account utente specificato nella **/u** parametro.                                                     |
| /FO {tabella &#124; elenco&#124; CSV} |                       Specifica il formato da utilizzare per l'output della query. I valori validi sono **tabella**, **elenco**, e **CSV**. Il formato predefinito per l'output è **TABELLA**.                        |
|                /NH                 |                                             Omette le intestazioni di colonna nell'output. Valido quando il **/fo** parametro è impostato su **TABELLA** o **CSV**.                                              |
|                 /v                 |                                                                    Specifica la visualizzazione di informazioni dettagliate.                                                                     |
|                 /?                 |                                                                                                                                                                                               |

## <a name="remarks"></a>Note
**GETMAC** può essere utile quando si desidera immettere l'indirizzo MAC in un analizzatore di rete o quando è necessario sapere quali protocolli sono attualmente in uso in ogni scheda di rete in un computer.
## <a name="BKMK_Examples"></a>Esempi
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
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
