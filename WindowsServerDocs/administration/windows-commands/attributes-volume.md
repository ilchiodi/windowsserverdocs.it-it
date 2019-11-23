---
title: volume attributi
description: Windows Commands Topic for **Attributes volume** -Visualizza, imposta o cancella gli attributi di un volume.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 225a10307123763d1a024fcc08fbae536fd0b5df
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382579"
---
# <a name="attributes-volume"></a>volume attributi

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza, imposta o cancella gli attributi di un volume.  
  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
attributes volume [{set | clear}] [{hidden | readonly | nodefaultdriveletter | shadowcopy}] [noerr]  
```  
  
## <a name="parameters"></a>Parametri  
  
|Parametro|Descrizione|  
|-------|--------|  
|set|Imposta l'attributo specificato del volume con lo stato attivo.|  
|clear|Cancella l'attributo specificato del volume con lo stato attivo.|  
|readonly|Specifica che il volume viene letto solo\-.|  
|nascosto|Specifica che il volume è nascosto.|  
|NODEFAULTDRIVELETTER|Specifica che il volume non riceve una lettera di unità per impostazione predefinita.|  
|ShadowCopy|Specifica che il volume è un volume di copia shadow.|  
|NOERR|solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|  
  
## <a name="remarks"></a>Osservazioni  
  
-   Nel record di avvio principale di base \(i dischi\) MBR, i parametri **Hidden**, **ReadOnly**e **nodefaultdriveletter** si applicano a tutti i volumi sul disco.  
  
-   Nella tabella di partizione GUID di base \(dischi\) GPT e nei dischi MBR e GPT dinamici i parametri **Hidden**, **ReadOnly**e **nodefaultdriveletter** si applicano solo al volume selezionato.  
  
-   È necessario selezionare un volume affinché il comando **volume attributi** abbia esito positivo. Utilizzare il **Selezionare volume** comando per selezionare un volume e spostare lo stato attivo a esso.  
  
## <a name="BKMK_examples"></a>Esempi  
Per visualizzare gli attributi correnti sul volume selezionato, digitare:  
  
```  
attributes volume  
```  
  
Per impostare il volume selezionato come nascosto e leggere\-solo, digitare:  
  
```  
attributes volume set hidden readonly  
```  
  
Per rimuovere gli attributi Hidden e Read\-solo nel volume selezionato, digitare:  
  
```  
attributes volume clear hidden readonly  
```  
  
#### <a name="additional-references"></a>riferimenti aggiuntivi  
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  

  

