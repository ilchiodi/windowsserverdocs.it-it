---
title: lpq
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bb6abcc4-310a-4fa4-927b-4084b62ca02e vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 79d9f19f70840c8e40d602ba7ce634d4a6dbb73b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866762"
---
# <a name="lpq"></a>lpq

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza lo stato di una coda di stampa in un computer che esegue Daemon LPD (Line printer).  
  
## <a name="syntax"></a>Sintassi  
```  
lpq -S <ServerName> -P <printerName> [-l]  
```  
## <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|-S <ServerName>|Specifica, tramite nome o indirizzo IP, il computer o dispositivo che ospita la coda di stampa LPD con cui si desidera visualizzare lo stato di condivisione della stampante. Obbligatorio.|  
|-P <printerName>|Specifica (nome) della stampante per la coda di stampa con uno stato che si desidera visualizzare. Obbligatorio.|  
|-l|Specifica che si desidera visualizzare i dettagli relativi allo stato della coda di stampa.|  
|/?|Visualizza la guida al prompt dei comandi.|  
## <a name="remarks"></a>Note  
Il **-S** e **-P** i parametri devono essere digitati in lettere maiuscole e minuscole.  
## <a name="BKMK_examples"></a>Esempi  
In questo esempio viene illustrato come visualizzare lo stato della coda di stampa Laserprinter1 in un host LPD 10.0.0.45:  
```  
lpq -S 10.0.0.45 -P Laserprinter1  
```  
#### <a name="additional-references"></a>Riferimenti aggiuntivi  
[Chiave sintassi della riga di comando](command-line-syntax-key.md)  
[Riferimenti ai comandi di stampa](print-command-reference.md)  
