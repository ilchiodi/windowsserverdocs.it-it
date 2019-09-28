---
title: Seleziona partizione
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 25f70083-b8f7-4a8e-9b34-4b3ffbe06670
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9a186e2678fde64396a8b4b57a2d14e4b0b7bf26
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371067"
---
# <a name="select-partition"></a>Seleziona partizione

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Seleziona la partizione specificata e sposta lo stato attivo su di essa. Questo comando può essere utilizzato anche per visualizzare la partizione che attualmente ha lo stato attivo sul disco selezionato.  
  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
select partition=<n>  
```  
  
## <a name="parameters"></a>Parametri  
  
|   Parametro    |                                                                                    Descrizione                                                                                    |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Partition @ no__t-0 @ no__t-1 | Il numero di partizione che deve ricevere lo stato attivo. È possibile visualizzare i numeri per tutte le partizioni del disco attualmente selezionato in precedenza tramite il **elenco partizione** comando DiskPart. |
  
## <a name="remarks"></a>Note  
  
-   Prima di poter selezionare una partizione è innanzitutto necessario selezionare un disco utilizzando il **disco selezionare** comando.  
  
-   Se non viene specificato alcun numero di partizione, questo comando Visualizza la partizione che attualmente ha lo stato attivo nel disco selezionato.  
  
-   Se viene selezionato un volume con una partizione corrispondente, la partizione verrà selezionata automaticamente.  
  
-   Se viene selezionata una partizione con un volume corrispondente, il volume verrà selezionato automaticamente.  
  
## <a name="BKMK_examples"></a>Esempi  
Per spostare lo stato attivo per la partizione 3, digitare:  
  
```  
select partitition=3  
```  
  
Per visualizzare la partizione che attualmente ha lo stato attivo sul disco selezionato, digitare:  
  
```  
select partition  
```  
  
#### <a name="additional-references"></a>Riferimenti aggiuntivi  
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  

  

