---
title: bootcfg query
description: 'Argomento dei comandi di Windows per **query bootcfg** : esegue una query e visualizza le voci della sezione [boot loader] e [Operating Systems] di boot. ini.'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: ae82357cfe178343872448c2ebd46c49a797b5a9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379908"
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
|    /s <computer>    |                                         Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale.                                          |
| /u <Domain>\\<User> | Esegue il comando con le autorizzazioni dell'account dell'utente specificato da <User>or <Domain> @ no__t-2 @ no__t-3. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso. |
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
- La parte delle voci di avvio dell'output della **query bootcfg** Visualizza i dettagli seguenti per ogni voce del sistema operativo nella sezione [Operating Systems] di boot. ini: ID della voce di avvio, nome descrittivo, percorso e opzioni di caricamento del sistema operativo.
  ## <a name="BKMK_examples"></a>Esempi
  Gli esempi seguenti illustrano come utilizzare il **bootcfg /query** comando:
  ```
  bootcfg /query
  bootcfg /query /s srvmain /u maindom\hiropln /p p@ssW23
  bootcfg /query /u hiropln /p p@ssW23
  ```
  #### <a name="additional-references"></a>Riferimenti aggiuntivi
  [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
