---
title: select volume
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5d70d776-80ad-4f20-8288-a7997fb1df28
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a3885d5d40e35e975ebe1cc28ddc26d4c1e78e24
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721995"
---
# <a name="select-volume"></a>select volume

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Seleziona il volume specificato e sposta lo stato attivo a esso. Questo comando può essere utilizzato anche per visualizzare il volume che attualmente ha lo stato attivo sul disco selezionato.  
  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
select volume={<n>|<d>}  
```  
  
### <a name="parameters"></a>Parametri  
  
| Parametro |                                                                               Descrizione                                                                                |
|-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <n>    | Il numero del volume per ricevere lo stato attivo. È possibile visualizzare i numeri per tutti i volumi sul disco attualmente selezionato in precedenza tramite il **elenco volume** comando DiskPart. |
|    <d>    |                                                 Il montaggio o lettera punto percorso dell'unità del volume per ricevere lo stato attivo.                                                 |
  
## <a name="remarks"></a>Osservazioni  
  
-   Se non viene specificato alcun volume, questo comando Visualizza il volume che attualmente ha lo stato attivo nel disco selezionato.  
  
-   Su un disco di base, la selezione di un volume offre inoltre lo stato attivo alla partizione corrispondente.  
  
-   Se viene selezionato un volume con una partizione corrispondente, la partizione verrà selezionata automaticamente.  
  
-   Se viene selezionata una partizione con un volume corrispondente, il volume verrà selezionato automaticamente.  
  
## <a name="examples"></a>Esempi  
Per spostare lo stato attivo per il volume 2, digitare:  
  
```  
select volume=2  
```  
  
Per spostare lo stato attivo per l'unità C, digitare:  
  
```  
select volume=c  
```  
  
Per spostare lo stato attivo sul volume montato in una cartella denominata mountpath, digitare:  
  
```  
select volume=c:\mountpath  
```  
  
Per visualizzare il volume che attualmente ha lo stato attivo sul disco selezionato, digitare:  
  
```  
select volume  
```  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  

  

