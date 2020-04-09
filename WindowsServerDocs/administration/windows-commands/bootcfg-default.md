---
title: bootcfg default
description: Windows Commands Topic for bootcfg default, che specifica la voce del sistema operativo da designare come impostazione predefinita.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e21824d7-8278-41d7-a2c5-ce09803d513a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 517cf444a5517b3d612266b57b428e47ac60d4ef
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848565"
---
# <a name="bootcfg-default"></a>bootcfg default

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Specifica la voce del sistema operativo per specificare che il valore predefinito.

## <a name="syntax"></a>Sintassi
```
bootcfg /default [/s <computer> [/u <Domain>\<User> /p <Password>]] [/id <OSEntryLineNum>]
```
### <a name="parameters"></a>Parametri

|      Parametro       |                                                                                             Descrizione                                                                                              |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                          Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale.                                          |
| /u <Domain>\\<User>  | Esegue il comando con le autorizzazioni dell'account dell'utente specificato da <User> o <Domain>\\<User>. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso. |
|    /p <Password>     |                                                        Specifica la password dell'account utente specificato nella **/u** parametro.                                                         |
| <OSEntryLineNum>/ID | Specifica il numero di riga voce del sistema operativo in della sezione [operating systems] del file Boot. ini da impostare come predefinita. La prima riga dopo la sezione [operating systems] sezione di intestazione è 1.  |
|          /?          |                                                                                 Visualizza la guida al prompt dei comandi.                                                                                 |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi
Gli esempi seguenti illustrano come utilizzare il **bootcfg /default**comando:
```
bootcfg /default /id 2
bootcfg /default /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```
## <a name="additional-references"></a>Altre informazioni di riferimento
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
