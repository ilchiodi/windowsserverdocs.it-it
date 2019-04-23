---
title: prnqctl
description: Stampare una pagina di test, sospendere o riprendere una stampante.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 26af9527b7b16b42fd9d389f3409143dfc3e9aa9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858082"
---
# <a name="prnqctl"></a>prnqctl

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di stampare una pagina di test, sospende o riprende una stampante e cancella la coda di stampa.  
  
## <a name="syntax"></a>Sintassi  
```  
cscript Prnqctl {-z | -m | -e | -x | -?} [-s <ServerName>]   
[-p <printerName>] [-u <UserName>] [-w <Password>]  
```  
## <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|-z|Interrompe la stampa su stampante specificata con il **-p** parametro.|  
|-m|Riprende la stampa su stampante specificata con il **-p** parametro.|  
|-e|Consente di stampare una pagina di prova nella stampante specificata con il **-p** parametro.|  
|-x|Annulla tutti i processi di stampa nella stampante specificata con il **-p** parametro.|  
|-s \<nomeserver >|Specifica il nome del computer remoto che ospita la stampante che si desidera gestire. Se non si specifica un computer, viene utilizzato il computer locale.|  
|-p \<nomestampante >|Specifica il nome della stampante che si desidera gestire. Obbligatorio.|  
|-u \<UserName > -w \<Password >|Specifica un account con autorizzazioni sufficienti per connettersi al computer che ospita la stampante che si desidera gestire. Tutti i membri del gruppo Administrators locale del computer di destinazione dispongono di queste autorizzazioni, ma è anche possibile concedere le autorizzazioni ad altri utenti. Se non si specifica un account, è necessario essere connessi con un account con queste autorizzazioni per il comando lavorare.|  
|/?|Visualizza la guida al prompt dei comandi.|  

## <a name="remarks"></a>Note  
-   Il **prnqctl** comando è uno script Visual Basic che si trova nel %WINdir%\System32\printing_Admin_Scripts\\ <language> directory. Per usare questo comando, un prompt dei comandi, digitare **cscript** aggiungendo il percorso completo del file prnqctl, o cambiare le directory nella cartella appropriata. Ad esempio:   
    ```  
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnqctl  
    ```  
-   Se le informazioni fornite contengono spazi, utilizzare le virgolette intorno al testo (ad esempio, `"computer Name"`).  

## <a name="BKMK_examples"></a>Esempi  
Per stampare una pagina test nella stampante Laserprinter1 condivisa dal \\\Server1 computer, digitare:  
```  
cscript Prnqctl -e -s Server1 -p Laserprinter1  
```  
Per sospendere la stampa su stampante Laserprinter1 nel computer locale, digitare:  
```  
cscript Prnqctl -z -p Laserprinter1  
```  
Per annullare tutti i processi di stampa nella stampante Laserprinter1 nel computer locale, digitare:  
```  
cscript Prnqctl -x -p Laserprinter1  
```  

#### <a name="additional-references"></a>Riferimenti aggiuntivi  
[Chiave sintassi della riga di comando](command-line-syntax-key.md)  
[Riferimenti ai comandi di stampa](print-command-reference.md)  
