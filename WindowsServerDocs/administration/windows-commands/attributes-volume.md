---
title: volume di attributi
description: Argomento i comandi di Windows per **attributi volume** -Visualizza, imposta o Cancella gli attributi di un volume.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e40e8284-3d57-4de8-a46c-e4ade34a0d53
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 37af55ee2a041fbcf8068e0def72147732d3a687
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846582"
---
# <a name="attributes-volume"></a>volume di attributi

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza, imposta o Cancella gli attributi di un volume.  
  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
attributes volume [{set | clear}] [{hidden | readonly | nodefaultdriveletter | shadowcopy}] [noerr]  
```  
  
## <a name="parameters"></a>Parametri  
  
|Parametro|Descrizione|  
|-------|--------|  
|set|Imposta l'attributo specificato del volume con lo stato attivo.|  
|clear|Cancella l'attributo specificato del volume con lo stato attivo.|  
|readonly|Specifica che il volume viene letto\-solo.|  
|Nascosta|Specifica che il volume è nascosto.|  
|NODEFAULTDRIVELETTER sono|Specifica che il volume non riceve una lettera di unità per impostazione predefinita.|  
|SHADOWCOPY|Specifica che il volume è un volume copia shadow.|  
|NOERR|Solo per script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|  
  
## <a name="remarks"></a>Note  
  
-   Nel record di avvio principale base \(MBR\) dischi, il **nascosto**, **readonly**, e **NODEFAULTDRIVELETTER sono** parametri si applicano a tutti i volumi in il disco.  
  
-   Nella tabella di partizione GUID semplice \(gpt\) dischi e su dynamic dischi MBR e gpt, il **nascosto**, **readonly**, e **NODEFAULTDRIVELETTER sono** i parametri si applicano solo al volume selezionato.  
  
-   È necessario selezionare un volume per la **attributi volume** comando abbia esito positivo. Utilizzare il **Selezionare volume** comando per selezionare un volume e spostare lo stato attivo a esso.  
  
## <a name="BKMK_examples"></a>Esempi  
Per visualizzare gli attributi correnti nel volume selezionato, digitare:  
  
```  
attributes volume  
```  
  
Per impostare il volume selezionato come nascosto e lettura\-solo, digitare:  
  
```  
attributes volume set hidden readonly  
```  
  
Per rimuovere i nascosti e lettura\-solo gli attributi del volume selezionato, digitare:  
  
```  
attributes volume clear hidden readonly  
```  
  
#### <a name="additional-references"></a>Riferimenti aggiuntivi  
[Chiave sintassi della riga di comando](command-line-syntax-key.md)  
  

  

