---
title: bootcfg raw
description: 'Argomento dei comandi di Windows per **bootcfg RAW** : aggiunge le opzioni di caricamento del sistema operativo specificate come stringa a una voce del sistema operativo nella sezione **[Operating Systems]** del file Boot. ini.'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: fb5c052f85f54656c54a9e534f867d287407d2d4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379905"
---
# <a name="bootcfg-raw"></a>bootcfg raw

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

aggiunge le opzioni di caricamento del sistema operativo specificate come stringa a una voce del sistema operativo nella sezione **[Operating Systems]** del file Boot. ini.

## <a name="syntax"></a>Sintassi
```
bootcfg /raw [/s <computer> [/u <Domain>\<User> /p <Password>]] <OSLoadOptionsString> [/id <OSEntryLineNum>] [/a]
```
## <a name="parameters"></a>Parametri

|         Nome          |                                                                                                            Definizione                                                                                                             |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /s <computer>     |                                                        Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale.                                                         |
| /u <Domain> \\ @ no__t-2  |               Esegue il comando con le autorizzazioni dell'account dell'utente specificato da <User> o <Domain> @ no__t-2 @ no__t-3. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso.                |
|     /p <Password>     |                                                                       Specifica la password dell'account utente specificato nella **/u** parametro.                                                                       |
| <OSLoadOptionsString> | Specifica le opzioni di caricamento del sistema operativo da aggiungere alla voce del sistema operativo. Queste opzioni carico sostituiscono le opzioni di carico esistente associate alla voce del sistema operativo. Nessuna convalida di <OSLoadOptions> viene eseguita. |
| /ID <OSEntryLineNum>  |                       Specifica il numero di riga voce del sistema operativo in della sezione [operating systems] del file Boot. ini da aggiornare. La prima riga dopo la sezione [operating systems] sezione di intestazione è 1.                       |
|          /a           |                                                       Specifica che le opzioni di sistema operativo da aggiungere devono essere aggiunto per le opzioni del sistema operativo esistente.                                                        |
|          /?           |                                                                                               Visualizza la guida al prompt dei comandi.                                                                                                |

##### <a name="remarks"></a>Note
- **bootcfg RAW** viene usato per aggiungere testo alla fine di una voce del sistema operativo, sovrascrivendo eventuali opzioni di immissione esistenti del sistema operativo. Il testo deve contenere opzioni di caricamento del sistema operativo valido, ad esempio **/debug**, **/fastdetect**, **/nodebug**, **/baudrate**, **/crashdebug**, e **/sos**. Ad esempio, il seguente comando aggiunge " **/debug /fastdetect**" alla fine della prima voce del sistema operativo, sostituendo le opzioni di voce del sistema operativo precedente:
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
