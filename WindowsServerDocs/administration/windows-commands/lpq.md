---
title: lpq
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bb6abcc4-310a-4fa4-927b-4084b62ca02e vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f2d7a013ad9481780873cd57be4fa15732fc6196
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724251"
---
# <a name="lpq"></a>lpq

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di visualizzare lo stato di una coda di stampa in un computer che esegue LPD (Line Printer Daemon).  

## <a name="syntax"></a>Sintassi  
```  
lpq -S <ServerName> -P <printerName> [-l]  
```  
### <a name="parameters"></a>Parametri  

|    Parametro     |                                                                        Descrizione                                                                        |
|------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| -S<ServerName>  | Specifica, tramite nome o indirizzo IP, il computer o dispositivo che ospita la coda di stampa LPD con cui si desidera visualizzare lo stato di condivisione della stampante. Obbligatorio. |
| -P<printerName> |                           Specifica (nome) della stampante per la coda di stampa con uno stato che si desidera visualizzare. Obbligatorio.                           |
|        -l        |                                      Specifica che si desidera visualizzare i dettagli relativi allo stato della coda di stampa.                                      |
|        /?        |                                                           Visualizza la guida al prompt dei comandi.                                                            |

## <a name="remarks"></a>Osservazioni  
Il **-S** e **-P** i parametri devono essere digitati in lettere maiuscole e minuscole.  
## <a name="examples"></a>Esempi  
Questo esempio Mostra come visualizzare lo stato della coda della stampante stampa laserprinter1 in un host LPD in 10.0.0.45:  
```  
lpq -S 10.0.0.45 -P Laserprinter1  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
[Riferimento al comando stampa](print-command-reference.md)  
