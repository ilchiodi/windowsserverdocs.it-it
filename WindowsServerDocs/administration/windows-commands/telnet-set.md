---
title: set Telnet
description: Argomento di riferimento per Telnet set, che imposta le opzioni.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 67316b5f-9c6f-43e3-86d5-dcff9ae2ac3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 477c2ee259301af26870339a2c329c2c3502963b
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222680"
---
# <a name="telnet-set"></a>Telnet: impostare

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imposta le opzioni.

## <a name="syntax"></a>Sintassi
```
set [bsasdel] [crlf] [delasbs] [escape <Char>] [localecho] [logfile <FileName>] [logging] [mode {console | stream}] [ntlm] [term {ansi | vt100 | vt52 | vtnt}] [?]
```
#### <a name="parameters"></a>Parametri

|                    Parametro                     |                                                                                                                                              Descrizione                                                                                                                                              |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                     bsasdel                      |                                                                                                                                 Invia **BACKSPACE** come **Delete**.                                                                                                                                  |
|                       CRLF                       |                                                                                                        Invia CR & LF (0x0D, 0x 0A) quando viene premuto il tasto **invio** . Noto come nuova modalità riga.                                                                                                        |
|                     delasbs                      |                                                                                                                                 Invia **Delete** come **BACKSPACE**.                                                                                                                                  |
|                fuga<Character>                | Imposta il carattere di escape utilizzato per immettere il prompt del client Telnet. Il carattere di escape può essere un singolo carattere o una combinazione del tasto **CTRL** più un carattere. Per impostare una combinazione di tasti di controllo, tenere premuto il tasto **CTRL** mentre si digita il carattere che si desidera assegnare. |
|                    LOCALECHO                     |                                                                                                                                         Attiva l'eco locale.                                                                                                                                          |
|                logfile<FileName>                |                                                                                               Registra la sessione Telnet corrente nel file locale. La registrazione viene avviata automaticamente quando si imposta questa opzione.                                                                                               |
|                     registrazione                      |                                                                                                                  Attiva la registrazione. Se non è impostato alcun file di log, viene visualizzato un messaggio di errore.                                                                                                                   |
|           modalità {console &#124; schermata}           |                                                                                                                                       Imposta la modalità operativa.                                                                                                                                        |
|                       ntlm                       |                                                                                                                                     Attiva l'autenticazione NTLM.                                                                                                                                     |
| termine {ANSI &#124; VT100 &#124; VT52 &#124; VTNT} |                                                                                                                                        Imposta il tipo di terminale.                                                                                                                                        |
|                        ?                         |                                                                                                                                    Visualizza la guida per questo comando.                                                                                                                                    |

## <a name="remarks"></a>Commenti
1. È possibile usare il comando **Annulla** per disattivare un'opzione precedentemente impostata.
2. Nelle versioni non in lingua inglese di Telnet, il **codice** <option> è disponibile. **Codice** <option> imposta il codice corrente impostato su un'opzione, che può essere uno dei seguenti: **Shift JIS**, **Japanese EUC**, **JIS kanji**, **JIS kanji (78)**, **Dec kanji**, **NEC kanji**. È necessario impostare lo stesso set di codice nel computer remoto.
   ## <a name="examples"></a>Esempi
   Impostare il file di log e iniziare la registrazione nel file locale tnlog. txt
   ```
   set logfile tnlog.txt
   ```
   ## <a name="additional-references"></a>Riferimenti aggiuntivi
3. - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
