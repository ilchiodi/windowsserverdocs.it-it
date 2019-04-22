---
title: Selezionare una partizione
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: c7d5675aa6c33ddbe1e5e873e1a7cf7a2e8f8017
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824962"
---
# <a name="select-partition"></a>Selezionare una partizione

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Seleziona la partizione specificata e sposta lo stato attivo a esso. Questo comando può essere utilizzato anche per visualizzare la partizione che attualmente ha lo stato attivo sul disco selezionato.  
  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
select partition=<n>  
```  
  
## <a name="parameters"></a>Parametri  
  
|Parametro|Descrizione|  
|-------|--------|  
|partition\=<n>|Il numero di partizione che deve ricevere lo stato attivo. È possibile visualizzare i numeri per tutte le partizioni del disco attualmente selezionato in precedenza tramite il **elenco partizione** comando DiskPart.|  
  
## <a name="remarks"></a>Note  
  
-   Prima di poter selezionare una partizione è innanzitutto necessario selezionare un disco utilizzando il **disco selezionare** comando.  
  
-   Se non viene specificato alcun numero di partizione, questo comando Visualizza la partizione che attualmente ha lo stato attivo sul disco selezionato.  
  
-   Se si seleziona un volume con una partizione corrispondente, la partizione verrà selezionata automaticamente.  
  
-   Se è selezionata una partizione con un volume corrispondente, il volume verrà selezionato automaticamente.  
  
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
[Chiave sintassi della riga di comando](command-line-syntax-key.md)  
  

  

