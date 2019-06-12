---
title: bootcfg dbg1394
description: Argomento i comandi di Windows per **dbg1394 bootcfg** -debug porta 1394 Configura per una voce del sistema operativo specifico
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 35724697-90dd-4dbe-85b0-337fbd369dcc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 85a554e25d1553ea4cd9415bb180df4751966926
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434840"
---
# <a name="bootcfg-dbg1394"></a>bootcfg dbg1394

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di configurare il debug 1394 porta per una voce del sistema operativo specificato.

## <a name="syntax"></a>Sintassi
```
bootcfg /dbg1394 {ON | OFF}[/s <computer> [/u <Domain>\<User> /p <Password>]] [/ch <Channel>] /id <OSEntryLineNum>
```
## <a name="parameters"></a>Parametri

|      Parametro       |                                                                                                                                           Descrizione                                                                                                                                            |
|----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   {ON &#124; OFF}    | Specifica il valore per il debug della porta 1394.<br /><br />-   **VIA** -Abilita il supporto del debug remoto aggiungendo l'opzione/dbg1394 specificato <OSEntryLineNum>.<br />-   **DISATTIVARE** -disabilita il supporto del debug remoto rimuovendo l'opzione/dbg1394 specificato <OSEntryLineNum>. |
|    /s <computer>     |                                                                                        Specifica il nome o indirizzo IP di un computer remoto (non utilizzare le barre rovesciate). Il valore predefinito è il computer locale.                                                                                        |
| /u <Domain>\\<User>  |                                               Esegue il comando con le autorizzazioni dell'account dell'utente specificato da <User> oppure <Domain> \\ <User>. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso.                                               |
|    /p <Password>     |                                                                                                      Specifica la password dell'account utente specificato nella **/u** parametro.                                                                                                       |
|     /CH canale      |                                                           Specifica il canale da utilizzare per il debug. I valori validi sono numeri interi compresi tra 1 e 64. Non usare la **/ch** <Channel> parametro se si sta disabilitando il debug porta 1394.                                                           |
| /id <OSEntryLineNum> |                                  Specifica il numero di riga voce del sistema operativo in della sezione [operating systems] del file Boot. ini alla quale aggiungere le opzioni di debug della porta 1394. La prima riga dopo la sezione [operating systems] sezione di intestazione è 1.                                  |
|          /?          |                                                                                                                               Visualizza la guida al prompt dei comandi.                                                                                                                               |

## <a name="BKMK_examples"></a>Esempi
Gli esempi seguenti illustrano come utilizzare il **bootcfg /dbg1394**comando:
```
bootcfg /dbg1394 /id 2 
bootcfg /dbg1394 on /ch 1 /id 3 
bootcfg /dbg1394 edit /ch 8 /id 2 
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /dbg1394 off /id 2
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
