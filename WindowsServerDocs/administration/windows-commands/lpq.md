---
title: lpq
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bb6abcc4-310a-4fa4-927b-4084b62ca02e vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 051b1983fcc0fddd7b69e561c0a27a120f78d998
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840394"
---
# <a name="lpq"></a>lpq

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di visualizzare lo stato di una coda di stampa in un computer che esegue LPD (Line Printer Daemon).  

## <a name="syntax"></a>Sintassi  
```  
lpq -S <ServerName> -P <printerName> [-l]  
```  
### <a name="parameters"></a>Parametri  

|    Parametro     |                                                                        Descrizione                                                                        |
|------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| -S <ServerName>  | Specifica, tramite nome o indirizzo IP, il computer o dispositivo che ospita la coda di stampa LPD con cui si desidera visualizzare lo stato di condivisione della stampante. Obbligatoria. |
| -P <printerName> |                           Specifica (nome) della stampante per la coda di stampa con uno stato che si desidera visualizzare. Obbligatoria.                           |
|        -l        |                                      Specifica che si desidera visualizzare i dettagli relativi allo stato della coda di stampa.                                      |
|        /?        |                                                           Visualizza la guida al prompt dei comandi.                                                            |

## <a name="remarks"></a>Note  
Il **-S** e **-P** i parametri devono essere digitati in lettere maiuscole e minuscole.  
## <a name="examples"></a><a name=BKMK_examples></a>Esempi  
Questo esempio Mostra come visualizzare lo stato della coda della stampante stampa laserprinter1 in un host LPD in 10.0.0.45:  
```  
lpq -S 10.0.0.45 -P Laserprinter1  
```  
## <a name="additional-references"></a>Altre informazioni di riferimento  
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
[Riferimento al comando stampa](print-command-reference.md)  
