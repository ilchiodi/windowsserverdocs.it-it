---
title: irftp
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e15c60a7-546d-4e9f-9871-43aaa1b569d6 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d469270b744a9de881efd9b0cfa6f1105f70519a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848422"
---
# <a name="irftp"></a>irftp

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Invia i file su un collegamento a infrarossi.    
## <a name="syntax"></a>Sintassi  
```  
irftp [<Drive>:\] [[<path>] <FileName>] [/h][/s]  
```  

### <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|Unità:\|specifica l'unità che contiene i file che si desiderano inviare tramite un collegamento a infrarossi.|  
|[path]FileName|Specifica il percorso e nome del file o set di file che si desiderano inviare tramite un collegamento a infrarossi. Se si specifica un set di file, è necessario specificare il percorso completo per ogni file.|  
|/h|Specifica la modalità nascosta. Quando si utilizza tale modalità, i file vengono inviati senza visualizzare la finestra di dialogo Collegamento senza fili.|  
|/s|Apre la finestra di dialogo Collegamento senza fili, che è possibile selezionare il file o un set di file che si desiderano inviare senza utilizzare la riga di comando per specificare l'unità, percorso e i nomi di file.|  

## <a name="remarks"></a>Note  
-   Prima di utilizzare questo comando, verificare che i dispositivi che si desidera comunicare attraverso un collegamento a infrarossi dispongono di funzionalità a infrarossi e funzioni correttamente e che un collegamento a infrarossi viene stabilito tra i dispositivi.  
-   Utilizzato senza parametri o con **/s**, **irftp** apre il **collegamento senza fili** della finestra di dialogo in cui è possibile selezionare i file che si desiderano inviare senza utilizzare la riga di comando.  

## <a name="BKMK_Examples"></a>Esempi  
Inviare example. txt tramite il collegamento a infrarossi.  
```  
irftp c:\example.txt  
```  

## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)  
