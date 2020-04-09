---
title: Seleziona partizione
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 25f70083-b8f7-4a8e-9b34-4b3ffbe06670
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 97145d73cbbe1bdc9b27e545b047b78fe89e4984
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834804"
---
# <a name="select-partition"></a>Seleziona partizione

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Seleziona la partizione specificata e sposta lo stato attivo su di essa. Questo comando può essere utilizzato anche per visualizzare la partizione che attualmente ha lo stato attivo sul disco selezionato.  
  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
select partition=<n>  
```  
  
### <a name="parameters"></a>Parametri  
  
|   Parametro    |                                                                                    Descrizione                                                                                    |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| \=partizione <n> | Il numero di partizione che deve ricevere lo stato attivo. È possibile visualizzare i numeri per tutte le partizioni del disco attualmente selezionato in precedenza tramite il **elenco partizione** comando DiskPart. |
  
## <a name="remarks"></a>Note  
  
-   Prima di poter selezionare una partizione è innanzitutto necessario selezionare un disco utilizzando il **disco selezionare** comando.  
  
-   Se non viene specificato alcun numero di partizione, questo comando Visualizza la partizione che attualmente ha lo stato attivo nel disco selezionato.  
  
-   Se viene selezionato un volume con una partizione corrispondente, la partizione verrà selezionata automaticamente.  
  
-   Se viene selezionata una partizione con un volume corrispondente, il volume verrà selezionato automaticamente.  
  
## <a name="examples"></a><a name=BKMK_examples></a>Esempi  
Per spostare lo stato attivo per la partizione 3, digitare:  
  
```  
select partitition=3  
```  
  
Per visualizzare la partizione che attualmente ha lo stato attivo sul disco selezionato, digitare:  
  
```  
select partition  
```  
  
## <a name="additional-references"></a>Altre informazioni di riferimento  
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  

  

