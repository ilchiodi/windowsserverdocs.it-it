---
title: bootcfg raw
description: Argomento i comandi di Windows per **bootcfg raw** -aggiunge le opzioni di caricamento del sistema operativo specificate sotto forma di stringa a una voce del sistema operativo le **[i sistemi operativi]** sezione del file Boot. ini.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e3458749-b0a0-460f-a022-3ff199a71f27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5334ff8a1c5d15343b4a48814b52012c641016a4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434691"
---
# <a name="bootcfg-raw"></a>bootcfg raw

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Aggiunge le opzioni di caricamento del sistema operativo specificate sotto forma di stringa a una voce del sistema operativo il **[i sistemi operativi]** sezione del file Boot. ini.

## <a name="syntax"></a>Sintassi
```
bootcfg /raw [/s <computer> [/u <Domain>\<User> /p <Password>]] <OSLoadOptionsString> [/id <OSEntryLineNum>] [/a]
```
## <a name="parameters"></a>Parametri

|         Nome          |                                                                                                            Definizione                                                                                                             |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /s <computer>     |                                                        Specifica il nome o indirizzo IP di un computer remoto (non utilizzare le barre rovesciate). Il valore predefinito è il computer locale.                                                         |
| /u <Domain> \\<User>  |               Esegue il comando con le autorizzazioni dell'account dell'utente specificato da <User> oppure <Domain> \\ <User>. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso.                |
|     /p <Password>     |                                                                       Specifica la password dell'account utente specificato nella **/u** parametro.                                                                       |
| <OSLoadOptionsString> | Specifica le opzioni di caricamento del sistema operativo da aggiungere alla voce del sistema operativo. Queste opzioni carico sostituiscono le opzioni di carico esistente associate alla voce del sistema operativo. Nessuna convalida di <OSLoadOptions> viene eseguita. |
| /id <OSEntryLineNum>  |                       Specifica il numero di riga voce del sistema operativo in della sezione [operating systems] del file Boot. ini da aggiornare. La prima riga dopo la sezione [operating systems] sezione di intestazione è 1.                       |
|          /a           |                                                       Specifica che le opzioni di sistema operativo da aggiungere devono essere aggiunto per le opzioni del sistema operativo esistente.                                                        |
|          /?           |                                                                                               Visualizza la guida al prompt dei comandi.                                                                                                |

##### <a name="remarks"></a>Note
- **bootcfg raw** viene usato per aggiungere testo alla fine di una voce del sistema operativo, sovrascrivendo eventuali opzioni di voce del sistema operativo esistente. Il testo deve contenere opzioni di caricamento del sistema operativo valido, ad esempio **/debug**, **/fastdetect**, **/nodebug**, **/baudrate**, **/crashdebug**, e **/sos**. Ad esempio, il seguente comando aggiunge " **/debug /fastdetect**" alla fine della prima voce del sistema operativo, sostituendo le opzioni di voce del sistema operativo precedente:
  ```
  bootcfg /raw "/debug /fastdetect" /id 1
  ```
  ## <a name="BKMK_examples"></a>Esempi
  Gli esempi seguenti illustrano come utilizzare il **bootcfg / raw** comando:
  ```
  bootcfg /raw "/debug /sos" /id 2
  bootcfg /raw /s srvmain /u maindom\hiropln /p p@ssW23 "/crashdebug " /id 2
  ```
  #### <a name="additional-references"></a>Riferimenti aggiuntivi
  [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
