---
title: irftp
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e15c60a7-546d-4e9f-9871-43aaa1b569d6 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 28cf722a5e630cb05b0348ebf2d4f582217b5497
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841984"
---
# <a name="irftp"></a>irftp

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Invia i file su un collegamento a infrarossi.    
## <a name="syntax"></a>Sintassi  
```  
irftp [<Drive>:\] [[<path>] <FileName>] [/h][/s]  
```  

#### <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|Unità:\|specifica l'unità che contiene i file che si desidera inviare tramite un collegamento a infrarossi.|  
|percorso FileName|Specifica il percorso e nome del file o set di file che si desiderano inviare tramite un collegamento a infrarossi. Se si specifica un set di file, è necessario specificare il percorso completo per ogni file.|  
|/h|Specifica la modalità nascosta. Quando si utilizza tale modalità, i file vengono inviati senza visualizzare la finestra di dialogo Collegamento senza fili.|  
|/s|Apre la finestra di dialogo Collegamento senza fili, che è possibile selezionare il file o un set di file che si desiderano inviare senza utilizzare la riga di comando per specificare l'unità, percorso e i nomi di file.|  

## <a name="remarks"></a>Note  
-   Prima di utilizzare questo comando, verificare che i dispositivi che si desidera comunicare attraverso un collegamento a infrarossi dispongono di funzionalità a infrarossi e funzioni correttamente e che un collegamento a infrarossi viene stabilito tra i dispositivi.  
-   Utilizzato senza parametri o con **/s**, **irftp** apre il **collegamento senza fili** della finestra di dialogo in cui è possibile selezionare i file che si desiderano inviare senza utilizzare la riga di comando.  

## <a name="examples"></a><a name=BKMK_Examples></a>Esempi  
Inviare example. txt tramite il collegamento a infrarossi.  
```  
irftp c:\example.txt  
```  

## <a name="additional-references"></a>Altre informazioni di riferimento  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
