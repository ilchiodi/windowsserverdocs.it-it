---
title: cacls
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b5bdbaaa-4557-48b8-80df-e75ee0d2f27d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 289015ff580d7e2fba5ff911878028f5302cab8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838082"
---
# <a name="cacls"></a>cacls

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza o modifica gli elenchi di controllo di accesso discrezionale (DACL) nei file specificati.  
## <a name="syntax"></a>Sintassi  
```  
cacls <filename> [/t] [/m] [/l] [/s[:sddl]] [/e] [/c] [/g user:<perm>] [/r user [...]] [/p user:<perm> [...]] [/d user [...]]  
```  
### <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|\<filename\>|Obbligatorio. Consente di visualizzare gli ACL dei file specificati.|  
|/t|Modifica gli ACL dei file specificati nella directory corrente e tutte le sottodirectory.|  
|/m|le modifiche degli ACL dei volumi montati in una directory.|  
|/l|Utilizzare il collegamento simbolico stesso e di destinazione.|  
|/s:sddl|sostituisce gli ACL con quelle specificate nella stringa SDDL (non valido con **/e**, **/g**, **/r**, **p**, o **/d**).|  
|/e|modificare ACL invece di sostituirlo.|  
|/c|Continua in errori di accesso negato.|  
|/g user:\<perm\>|GRANT specificato diritti di accesso utente.<br /><br />Valori validi per l'autorizzazione:<br /><br />-n - Nessuno<br />-r - lettura<br />-w - scrittura<br />-c - modifica (scrittura)<br />controllo completo - f-|  
|utente /r [...]|Revocare i diritti di accesso dell'utente specificato (valido solo con **/e**).|  
|[/p user:\<perm\> [...]|Sostituire i diritti di accesso dell'utente specificato.<br /><br />Valori validi per l'autorizzazione:<br /><br />-n - Nessuno<br />-r - lettura<br />-w - scrittura<br />-c - modifica (scrittura)<br />controllo completo - f-|  
|[/d utente [...]|Negare l'accesso utente specificato.|  
|/?|Visualizza la guida al prompt dei comandi.|  
## <a name="remarks"></a>Note  
-   Questo comando è stato deprecato. Usare [icacls](icacls.md) invece.  
-   Usare la tabella seguente vengono descritti i risultati:  

    |Output|Voce di controllo di accesso (ACE) si applica a|  
    |-----|----------------------|  
    |OI|Oggetto eredita. Questa cartella e file.|  
    |CI|Eredità. Questa cartella e nelle sottocartelle.|  
    |IO|Solo eredità. La voce ACE non è applicabile per il file/directory corrente.|  
    |Nessun messaggio di output|Solo questa cartella.|  
    |(OI)(CI)|Questa cartella, le sottocartelle e file.|  
    |(OI)(CI)(IO)|Solo sottocartelle e file.|  
    |(CI)(IO)|Solo sottocartelle.|  
    |(OI)(IO)|Solo i file.|  

-   È possibile usare caratteri jolly (**?** e **\***) per specificare più file.  
-   È possibile specificare più di un utente.  

#### <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)   
-   [icacls](icacls.md)  
