---
title: prnqctl
description: Stampare una pagina di test, sospendere o riprendere una stampante.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8df9dfa7-984c-4276-bb7d-e7675e7c399e jpjofre
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 377b448eb74f0a27228d8502ce149fb04869895b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722804"
---
# <a name="prnqctl"></a>prnqctl

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Stampa una pagina di test, sospende o riprende una stampante e cancella una coda di stampa.  

## <a name="syntax"></a>Sintassi  
```  
cscript Prnqctl {-z | -m | -e | -x | -?} [-s <ServerName>]   
[-p <printerName>] [-u <UserName>] [-w <Password>]  
```  
### <a name="parameters"></a>Parametri  

|Parametro|Descrizione|  
|-------|--------|  
|-Z|sospende la stampa sulla stampante specificata con il parametro **-p** .|  
|-M|Riprende la stampa sulla stampante specificata con il parametro **-p** .|  
|-E|stampa una pagina di test sulla stampante specificata con il parametro **-p** .|  
|-X|Annulla tutti i processi di stampa della stampante specificati con il parametro **-p** .|  
|-s \<nomeserver>|Specifica il nome del computer remoto che ospita la stampante che si desidera gestire. Se non si specifica un computer, viene usato il computer locale.|  
|-p \<printername>|Specifica il nome della stampante che si desidera gestire. Obbligatorio.|  
|-u \<nomeutente>-w \<password>|Specifica un account con le autorizzazioni per la connessione al computer che ospita la stampante che si desidera gestire. Tutti i membri del gruppo Administrators locale del computer di destinazione dispongono di queste autorizzazioni, ma è possibile concedere anche le autorizzazioni ad altri utenti. Se non si specifica un account, è necessario effettuare l'accesso con un account con le autorizzazioni necessarie per il funzionamento del comando.|  
|/?|Visualizza la guida al prompt dei comandi.|  

## <a name="remarks"></a>Osservazioni  
- Il comando **prnqctl** è uno script Visual Basic che si trova nella directory\\ <language> %windir%\system32\. printing_Admin_Scripts. Per usare questo comando, al prompt dei comandi digitare **cscript** seguito dal percorso completo del file prnqctl o passare alla cartella appropriata. Ad esempio:  
  ```  
  cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnqctl  
  ```  
- Se le informazioni fornite contengono spazi, racchiudere il testo tra virgolette (ad esempio, `"computer Name"`).  

## <a name="examples"></a><a name="BKMK_examples"></a>Esempi  
Per stampare una pagina di test sulla stampante stampa laserprinter1 condivisa dal computer \\\Server1, digitare:  
```  
cscript Prnqctl -e -s Server1 -p Laserprinter1  
```  
Per sospendere la stampa sulla stampante stampa laserprinter1 nel computer locale, digitare:  
```  
cscript Prnqctl -z -p Laserprinter1  
```  
Per annullare tutti i processi di stampa sulla stampante stampa laserprinter1 nel computer locale, digitare:  
```  
cscript Prnqctl -x -p Laserprinter1  
```  

## <a name="additional-references"></a>Riferimenti aggiuntivi  
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
[Riferimento al comando stampa](print-command-reference.md)  
