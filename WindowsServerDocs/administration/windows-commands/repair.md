---
title: ripristinare
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9f84f661-f3cd-48c8-bf08-87819cf626fe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 46b98938394c10e31d4999ff0e060e10f7da9bdc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835934"
---
# <a name="repair"></a>ripristinare

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ripristina il volume RAID\-5 con lo stato attivo sostituendo l'area disco non riuscita con il disco dinamico specificato.  
  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
repair disk=<n> [align=<n>] [noerr]  
```  
  
### <a name="parameters"></a>Parametri  
  
| Parametro  |                                                                                             Descrizione                                                                                              |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| \=disco <n>  |                                                                 Specifica il disco dinamico che sostituirà la regione del disco non riuscita.                                                                 |
| Allinea\=<n> |          Consente di allineare tutti gli extent volume o una partizione per il limite di allineamento più vicino. *n* è il numero di kilobyte \(KB\) dall'inizio del disco per il limite di allineamento più vicino.           |
|   NOERR    | solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore. |
  
## <a name="remarks"></a>Note  
  
-   Il disco dinamico specificato deve disporre di spazio libero maggiore o uguale alla dimensione totale dell'area del disco RAID\-volume 5.  
  
-   Un volume in un sistema RAID\-5 matrice deve essere selezionato per eseguire questa operazione. Utilizzare il **Selezionare volume** comando per selezionare un volume e spostare lo stato attivo a esso.  
  
## <a name="examples"></a><a name=BKMK_examples></a>Esempi  
Per sostituire il volume con lo stato attivo sostituendola con un disco dinamico 4, digitare:  
  
```  
repair disk=4  
```  
  
## <a name="additional-references"></a>Altre informazioni di riferimento  
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  

  

