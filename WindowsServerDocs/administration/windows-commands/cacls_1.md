---
title: cacls
description: Windows Commands Topic for cacls, che Visualizza o modifica gli elenchi di controllo di accesso discrezionale (DACL) sui file specificati.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b5bdbaaa-4557-48b8-80df-e75ee0d2f27d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8cc6300563880a587b6544ec8cb61e9010ad6c2c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848244"
---
# <a name="cacls"></a>cacls

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza o modifica gli elenchi di controllo di accesso discrezionale (DACL) sui file specificati.  

## <a name="syntax"></a>Sintassi  
```  
cacls <filename> [/t] [/m] [/l] [/s[:sddl]] [/e] [/c] [/g user:<perm>] [/r user [...]] [/p user:<perm> [...]] [/d user [...]]  
```  
#### <a name="parameters"></a>Parametri  

|        Parametro        |                                                                                            Descrizione                                                                                             |
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      \<filename\>       |                                                                            Obbligatoria. Visualizza gli ACL dei file specificati.                                                                             |
|           /t            |                                                          modifica gli ACL dei file specificati nella directory corrente e in tutte le sottodirectory.                                                          |
|           /m            |                                                                          modifica gli ACL dei volumi montati in una directory.                                                                           |
|           /l            |                                                                        Lavorare sul collegamento simbolico rispetto alla destinazione.                                                                         |
|         /s: SDDL         |                                       sostituisce gli ACL con quelli specificati nella stringa SDDL (non valido con **/e**, **/g**, **/r**, **/p**o **/d**).                                        |
|           /e            |                                                                                 modificare l'ACL anziché sostituirlo.                                                                                  |
|           /c            |                                                                                 Continua in presenza di errori di accesso negato.                                                                                  |
|    utente/g:\<\> Perm     |   Concedere i diritti di accesso utente specificati.<p>Valori validi per l'autorizzazione:<p>-n-none<br />-r-lettura<br />-w-Write<br />-c-modifica (scrittura)<br />-f-controllo completo   |
|      /r (utente) [...]      |                                                                  Revoca i diritti di accesso dell'utente specificato (valido solo con **/e**).                                                                   |
| [/p utente:\<Perm\> [...] | sostituisce i diritti di accesso dell'utente specificato.<p>Valori validi per l'autorizzazione:<p>-n-none<br />-r-lettura<br />-w-Write<br />-c-modifica (scrittura)<br />-f-controllo completo |
|     [/d utente [...]      |                                                                                    Negare l'accesso utente specificato.                                                                                     |
|           /?            |                                                                                Visualizza la guida al prompt dei comandi.                                                                                |

## <a name="remarks"></a>Note  
- Questo comando è stato deprecato. Usare invece [icacls](icacls.md) .  
- Usare la tabella seguente per interpretare i risultati:  


  |      Output       |                La voce di controllo di accesso (ACE) si applica a                |
  |-------------------|---------------------------------------------------------------------|
  |        OI         |               Oggetto che eredita. Questa cartella e i file.                |
  |        CI         |           Il contenitore eredita. Questa cartella e le relative sottocartelle.            |
  |        I/O         | Solo ereditarietà. La voce ACE non si applica al file o alla directory corrente. |
  | Nessun messaggio di output |                          Solo questa cartella.                          |
  |     OI Ci      |                 Questa cartella, le sottocartelle e i file.                 |
  |   OI Ci Io    |                     Solo sottocartelle e file.                      |
  |     Ci Io      |                          Solo sottocartelle.                           |
  |     OI Io      |                             Solo file.                             |


- È possibile utilizzare caratteri jolly ( **?** e **\\\*** ) per specificare più file.  
- È possibile specificare più di un utente.  

## <a name="additional-references"></a>Altre informazioni di riferimento  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)   
-   [icacls](icacls.md)  
