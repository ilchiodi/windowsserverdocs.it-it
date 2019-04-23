---
title: Logman query
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1116a0f0-5415-4369-a045-12f79f8f66de
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 79e23e15d85e7ab50d651baf7a556cc76c64fc26
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876662"
---
# <a name="logman-query"></a>Logman query

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

raccolta di dati di query o dell'agente di raccolta dati impostare le proprietà.  
  
## <a name="syntax"></a>Sintassi  
```  
logman query [providers|"Data Collector Set name"] [options]  
```  
## <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|/?|Vengono visualizzate sensibile al contesto della Guida.|  
|-s <computer name>|Eseguire il comando nel computer remoto specificato.|  
|-config <value>|Specifica il file di impostazioni che contiene le opzioni di comando.|  
|[-n] <name>|Nome dell'oggetto di destinazione.|  
|-ets|Inviare comandi alle sessioni di traccia eventi direttamente senza salvare o pianificare.|  
## <a name="BKMK_examples"></a>Esempi  
Il seguente comando Elenca tutti i set di agenti di raccolta dati configurati nel sistema di destinazione.  
```  
logman query  
```  
Il seguente comando Elenca agenti di raccolta dati contenuti in denominata perf_log insieme di raccolta dati.  
```  
logman query "perf_log"  
```  
Il seguente comando Elenca tutti i provider disponibili agenti di raccolta dati nel sistema di destinazione.  
```  
logman query providers  
```  
#### <a name="additional-references"></a>Riferimenti aggiuntivi  
[logman](logman.md)  
