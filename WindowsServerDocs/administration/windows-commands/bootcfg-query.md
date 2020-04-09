---
title: bootcfg query
description: Argomento dei comandi di Windows per la query bootcfg, che esegue query e visualizza le voci della sezione del caricatore di avvio e del sistema operativo da boot. ini.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a4cacfd1-10a6-4a11-b0c5-f8abde72bfc8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1ac80c802b1d30dcf7221f94f761233c6b6fc6b6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848524"
---
# <a name="bootcfg-query"></a>bootcfg query

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esegue query e Visualizza [caricatore di avvio] e le voci da Boot. ini di sezione [operating systems].

## <a name="syntax"></a>Sintassi
```
bootcfg /query [/s <computer> [/u <Domain>\<User> /p <Password>]]
```
### <a name="parameters"></a>Parametri

|        Termine         |                                                                                             Definizione                                                                                              |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>    |                                         Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale.                                          |
| /u <Domain>\\<User> | Esegue il comando con le autorizzazioni dell'account dell'utente specificato da <User>o <Domain>\\<User>. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso. |
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
  Friendly Name:   
  path:            multi(0)disk(0)rdisk(0)partition(1)\WINDOWS
  OS Load Options: /fastdetect /debug /debugport=com1:
  ```
- La parte di impostazioni del caricatore di avvio di **query bootcfg** output viene visualizzato ogni voce nella sezione [boot loader] del file Boot. ini.
- La parte delle voci di avvio dell'output della **query bootcfg** Visualizza i dettagli seguenti per ogni voce del sistema operativo nella sezione [Operating Systems] di boot. ini: ID della voce di avvio, nome descrittivo, percorso e opzioni di caricamento del sistema operativo.
  ## <a name="examples"></a><a name=BKMK_examples></a>Esempi
  Gli esempi seguenti illustrano come utilizzare il **bootcfg /query** comando:
  ```
  bootcfg /query
  bootcfg /query /s srvmain /u maindom\hiropln /p p@ssW23
  bootcfg /query /u hiropln /p p@ssW23
  ```
  ## <a name="additional-references"></a>Altre informazioni di riferimento
  - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
