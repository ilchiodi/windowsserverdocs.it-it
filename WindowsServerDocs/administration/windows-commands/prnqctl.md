---
title: prnqctl
description: Stampare una pagina di test, sospendere o riprendere una stampante.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8df9dfa7-984c-4276-bb7d-e7675e7c399e jpjofre
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 189b344dc0c4f587ba7a6382c481304242e22c74
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372037"
---
# <a name="prnqctl"></a>prnqctl

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Stampa una pagina di test, sospende o riprende una stampante e cancella una coda di stampa.  

## <a name="syntax"></a>Sintassi  
```  
cscript Prnqctl {-z | -m | -e | -x | -?} [-s <ServerName>]   
[-p <printerName>] [-u <UserName>] [-w <Password>]  
```  
## <a name="parameters"></a>Parametri  

|Parametro|Descrizione|  
|-------|--------|  
|-z|sospende la stampa sulla stampante specificata con il parametro **-p** .|  
|-m|Riprende la stampa sulla stampante specificata con il parametro **-p** .|  
|-e|stampa una pagina di test sulla stampante specificata con il parametro **-p** .|  
|-x|Annulla tutti i processi di stampa della stampante specificati con il parametro **-p** .|  
|-s \<nomeserver >|Specifica il nome del computer remoto che ospita la stampante che si desidera gestire. Se non si specifica un computer, viene usato il computer locale.|  
|-p \<PrinterName >|Specifica il nome della stampante che si desidera gestire. Obbligatorio.|  
|-u \<nome utente >-w \<password >|Specifica un account con le autorizzazioni per la connessione al computer che ospita la stampante che si desidera gestire. Tutti i membri del gruppo Administrators locale del computer di destinazione dispongono di queste autorizzazioni, ma è possibile concedere anche le autorizzazioni ad altri utenti. Se non si specifica un account, è necessario effettuare l'accesso con un account con le autorizzazioni necessarie per il funzionamento del comando.|  
|/?|Visualizza la guida al prompt dei comandi.|  

## <a name="remarks"></a>Osservazioni  
- Il comando **prnqctl** è uno script Visual Basic che si trova nella directory%windir%\system32\. printing_Admin_Scripts\\<language>. Per usare questo comando, al prompt dei comandi digitare **cscript** seguito dal percorso completo del file prnqctl o passare alla cartella appropriata. Ad esempio:  
  ```  
  cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnqctl  
  ```  
- Se le informazioni fornite contengono spazi, racchiudere il testo tra virgolette, ad esempio `"computer Name"`.  

## <a name="BKMK_examples"></a>Esempi  
Per stampare una pagina di prova sulla stampante stampa laserprinter1 condivisa dal computer \\\Server1, digitare:  
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

#### <a name="additional-references"></a>riferimenti aggiuntivi  
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
[Riferimento al comando stampa](print-command-reference.md)  
