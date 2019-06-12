---
title: bootcfg query
description: Argomento i comandi di Windows per **query bootcfg** -query e Visualizza [caricatore di avvio] e [i sistemi operativi] sezione voci da Boot. ini.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4cacfd1-10a6-4a11-b0c5-f8abde72bfc8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e79acc100a9ec9955f2692a3c6ee812d0310b687
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434738"
---
# <a name="bootcfg-query"></a>bootcfg query

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esegue query e Visualizza [caricatore di avvio] e le voci da Boot. ini di sezione [operating systems].

## <a name="syntax"></a>Sintassi
```
bootcfg /query [/s <computer> [/u <Domain>\<User> /p <Password>]]
```
## <a name="parameters"></a>Parametri

|        Nome         |                                                                                             Definizione                                                                                              |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>    |                                         Specifica il nome o indirizzo IP di un computer remoto (non utilizzare le barre rovesciate). Il valore predefinito è il computer locale.                                          |
| /u <Domain>\\<User> | Esegue il comando con le autorizzazioni dell'account dell'utente specificato da <User>oppure <Domain> \\ <User>. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso. |
|    /p <Password>    |                                                        Specifica la password dell'account utente specificato nella **/u** parametro.                                                        |
|         /?          |                                                                                Visualizza la guida al prompt dei comandi.                                                                                 |

##### <a name="remarks"></a>Note
- Di seguito è riportato un esempio di **bootcfg /query** output:
  ```
  Boot Loader Settings
  ----------
  timeout: 30
  default: multi(0)disk(0)rdisk(0)partition(1)\WINDOWS
  Boot Entries
  ------
  Boot entry ID:   1
  Friendly Name:   ""
  path:            multi(0)disk(0)rdisk(0)partition(1)\WINDOWS
  OS Load Options: /fastdetect /debug /debugport=com1:
  ```
- La parte di impostazioni del caricatore di avvio di **query bootcfg** output viene visualizzato ogni voce nella sezione [boot loader] del file Boot. ini.
- La parte di voci di avvio di **query bootcfg** output visualizza i dettagli seguenti per ogni voce del sistema operativo nella sezione [operating systems] del file Boot. ini: ID della voce di avvio, nome descrittivo, percorso e le opzioni di caricamento del sistema operativo.
  ## <a name="BKMK_examples"></a>Esempi
  Gli esempi seguenti illustrano come utilizzare il **bootcfg /query** comando:
  ```
  bootcfg /query
  bootcfg /query /s srvmain /u maindom\hiropln /p p@ssW23
  bootcfg /query /u hiropln /p p@ssW23
  ```
  #### <a name="additional-references"></a>Riferimenti aggiuntivi
  [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
