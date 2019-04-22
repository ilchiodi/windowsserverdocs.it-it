---
title: sxstrace
description: Informazioni su come diagnosticare i problemi side-by-side.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fcd26eeb-fbd9-4a86-b6a9-dfa5e9c6e4fc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 396d06bf079c0cfa8ba4864f71333eec39f7b255
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814102"
---
# <a name="sxstrace"></a>sxstrace

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Individua i problemi side-by-side.    

## <a name="syntax"></a>Sintassi  
```  
sxstrace [{[trace /logfile:<FileName> [/nostop]|[parse /logfile:<FileName> /outfile:<ParsedFile>  [/filter:<AppName>]}]  
```  

### <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|traccia|Abilita l'analisi per sxs (side-by-side)|  
|/logfile|Specifica il file di registro non elaborati.|  
|\<FileName>|Salva log di traccia delle *FileName*.|  
|/nostop|Non specifica nessuna richiesta per arrestare la traccia.|  
|analisi|Converte il file di traccia non elaborati.|  
|al file di output|Specifica il nome del file di output.|  
|\<ParsedFile>|Specifica il nome del file analizzato.|  
|/Filter|Consente il filtraggio dell'output.|  
|\<NomeApp >|Specifica il nome dell'applicazione.|  
|stoptrace|Arrestare la traccia se non è stato arrestato prima.|  
|/?|Visualizza la guida al prompt dei comandi.|  

## <a name="BKMK_Examples"></a>Esempi  
Abilitare la traccia e salvare il file di traccia per **sxstrace.etl**:  
```  
sxstrace trace /logfile:sxstrace.etl  
```  
Convertire il file di traccia non elaborati in un formato leggibile e salvare i risultati in **sxstrace.txt**:  
```  
sxstrace parse /logfile:sxstrace.etl /outfile:sxstrace.txt  
```  

## <a name="additional-references"></a>Altri riferimenti  
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)  
  
