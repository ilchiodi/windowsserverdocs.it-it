---
title: set di Telnet
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 67316b5f-9c6f-43e3-86d5-dcff9ae2ac3e vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b68ce0ee87d80b25cf13db5bebc6c407a9fe091f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441025"
---
# <a name="telnet-set"></a>telnet: set

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imposta le opzioni.   
## <a name="syntax"></a>Sintassi  
```  
set [bsasdel] [crlf] [delasbs] [escape <Char>] [localecho] [logfile <FileName>] [logging] [mode {console | stream}] [ntlm] [term {ansi | vt100 | vt52 | vtnt}] [?]  
```  
### <a name="parameters"></a>Parametri  

|                    Parametro                     |                                                                                                                                              Descrizione                                                                                                                                              |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                     bsasdel                      |                                                                                                                                 Invia **Backspace** come una **eliminare**.                                                                                                                                  |
|                       crlf                       |                                                                                                        Invia CR e LF (0x0D, 0 x 0A) quando il **invio** viene premuto. Nota come modalità di nuova riga.                                                                                                        |
|                     delasbs                      |                                                                                                                                 Invia **eliminare** come una **Backspace**.                                                                                                                                  |
|                escape <Character>                | Imposta il carattere di escape utilizzato per accedere al prompt di client telnet. Il carattere di escape può essere un singolo carattere oppure può essere una combinazione dei **CTRL** principali oltre a un carattere. Per impostare una combinazione di tasti di controllo, tenere premuto il **CTRL** mentre si digita il carattere che si vuole assegnare. |
|                    eco locale                     |                                                                                                                                         Attiva echo locale.                                                                                                                                          |
|                logfile <FileName>                |                                                                                               Registra la sessione telnet corrente del file locale. Registrazione inizia automaticamente quando si imposta questa opzione.                                                                                               |
|                     logging                      |                                                                                                                  Attiva la registrazione. Se non è impostato alcun file di log, viene visualizzato un messaggio di errore.                                                                                                                   |
|           modalità {console di &#124; schermata}           |                                                                                                                                       Imposta la modalità operativa.                                                                                                                                        |
|                       ntlm                       |                                                                                                                                     Attiva l'autenticazione NTLM.                                                                                                                                     |
| term {ansi &#124; vt100 &#124; vt52 &#124; vtnt} |                                                                                                                                        Imposta il tipo di terminale.                                                                                                                                        |
|                        ?                         |                                                                                                                                    Visualizza la Guida per questo comando.                                                                                                                                    |

## <a name="remarks"></a>Note  
1. È possibile usare la **unset** comando per disattivare un'opzione che è stata impostata in precedenza.  
2. Nelle versioni localizzate di telnet, il **set di codici** <option> è disponibile. **Set di codici** <option> imposta il codice corrente impostato su un'opzione, che può essere una qualsiasi delle operazioni seguenti: **shift JIS**, **EUC giapponese**, **Kanji JIS**, **Kanji JIS (78)** , **Kanji DEC**, **NEC Kanji**. È consigliabile impostare lo stesso codice impostata nel computer remoto.  
   ## <a name="BKMK_Examples"></a>Esempi  
   Impostare il file di log e avviare la registrazione per il file locale tnlog.txt  
   ```  
   set logfile tnlog.txt  
   ```  
   ## <a name="additional-references"></a>Riferimenti aggiuntivi  
3. [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
