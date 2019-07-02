---
title: bootcfg default
description: Argomento i comandi di Windows per **bootcfg predefinito** -specifica la voce del sistema operativo per specificare che il valore predefinito.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e21824d7-8278-41d7-a2c5-ce09803d513a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 63aaa7a044634d29c61f3085b1f0c015f4e64444
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434837"
---
# <a name="bootcfg-default"></a>bootcfg default

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Specifica la voce del sistema operativo per specificare che il valore predefinito.

## <a name="syntax"></a>Sintassi
```
bootcfg /default [/s <computer> [/u <Domain>\<User> /p <Password>]] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Parametri

|      Parametro       |                                                                                             Descrizione                                                                                              |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                          Specifica il nome o indirizzo IP di un computer remoto (non utilizzare le barre rovesciate). Il valore predefinito è il computer locale.                                          |
| /u <Domain>\\<User>  | Esegue il comando con le autorizzazioni dell'account dell'utente specificato da <User> oppure <Domain> \\ <User>. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso. |
|    /p <Password>     |                                                        Specifica la password dell'account utente specificato nella **/u** parametro.                                                         |
| /id <OSEntryLineNum> | Specifica il numero di riga voce del sistema operativo in della sezione [operating systems] del file Boot. ini da impostare come predefinita. La prima riga dopo la sezione [operating systems] sezione di intestazione è 1.  |
|          /?          |                                                                                 Visualizza la guida al prompt dei comandi.                                                                                 |

## <a name="BKMK_examples"></a>Esempi
Gli esempi seguenti illustrano come utilizzare il **bootcfg /default**comando:
```
bootcfg /default /id 2
bootcfg /default /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)