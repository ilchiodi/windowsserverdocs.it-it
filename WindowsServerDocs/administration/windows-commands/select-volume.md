---
title: select volume
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5d70d776-80ad-4f20-8288-a7997fb1df28
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b0ebf9896621268c384ea8129d32c985028054d9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890732"
---
# <a name="select-volume"></a>select volume

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di selezionare il volume specificato e sposta lo stato attivo a esso. Questo comando può essere utilizzato anche per visualizzare il volume che attualmente ha lo stato attivo sul disco selezionato.  
  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
select volume={<n>|<d>}  
```  
  
## <a name="parameters"></a>Parametri  
  
|Parametro|Descrizione|  
|-------|--------|  
|<n>|Il numero del volume per ricevere lo stato attivo. È possibile visualizzare i numeri per tutti i volumi sul disco attualmente selezionato in precedenza tramite il **elenco volume** comando DiskPart.|  
|<d>|Il montaggio o lettera punto percorso dell'unità del volume per ricevere lo stato attivo.|  
  
## <a name="remarks"></a>Note  
  
-   Se si specifica alcun volume, questo comando Visualizza il volume che attualmente ha lo stato attivo sul disco selezionato.  
  
-   Su un disco di base, la selezione di un volume offre inoltre lo stato attivo alla partizione corrispondente.  
  
-   Se si seleziona un volume con una partizione corrispondente, la partizione verrà selezionata automaticamente.  
  
-   Se è selezionata una partizione con un volume corrispondente, il volume verrà selezionato automaticamente.  
  
## <a name="BKMK_examples"></a>Esempi  
Per spostare lo stato attivo per il volume 2, digitare:  
  
```  
select volume=2  
```  
  
Per spostare lo stato attivo per l'unità C, digitare:  
  
```  
select volume=c  
```  
  
Per spostare lo stato attivo per il volume montato in una cartella denominata "mountpath", digitare:  
  
```  
select volume=c:\mountpath  
```  
  
Per visualizzare il volume che attualmente ha lo stato attivo sul disco selezionato, digitare:  
  
```  
select volume  
```  
  
#### <a name="additional-references"></a>Riferimenti aggiuntivi  
[Chiave sintassi della riga di comando](command-line-syntax-key.md)  
  

  

