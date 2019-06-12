---
title: Repair
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9f84f661-f3cd-48c8-bf08-87819cf626fe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1e4b9cde10e11558aaa95edda94921144dac1f86
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441798"
---
# <a name="repair"></a>Repair

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ripristina il RAID\-5 volume attivo sostituendo la regione del disco non riuscito con il disco dinamico specificato.  
  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
repair disk=<n> [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parametri  
  
| Parametro  |                                                                                             Descrizione                                                                                              |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| disk\=<n>  |                                                                 Specifica il disco dinamico che sostituirà la regione del disco non riuscita.                                                                 |
| align\=<n> |          Consente di allineare tutti gli extent volume o una partizione per il limite di allineamento più vicino. *n* è il numero di kilobyte \(KB\) dall'inizio del disco per il limite di allineamento più vicino.           |
|   NOERR    | Solo per script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore. |
  
## <a name="remarks"></a>Note  
  
-   Il disco dinamico specificato deve disporre di spazio libero maggiore o uguale alla dimensione totale dell'area del disco RAID\-volume 5.  
  
-   Un volume in un sistema RAID\-5 matrice deve essere selezionato per eseguire questa operazione. Utilizzare il **Selezionare volume** comando per selezionare un volume e spostare lo stato attivo a esso.  
  
## <a name="BKMK_examples"></a>Esempi  
Per sostituire il volume con lo stato attivo sostituendola con un disco dinamico 4, digitare:  
  
```  
repair disk=4  
```  
  
#### <a name="additional-references"></a>Riferimenti aggiuntivi  
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  

  

