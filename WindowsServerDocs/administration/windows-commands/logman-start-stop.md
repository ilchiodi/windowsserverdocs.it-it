---
title: Logman start | arrestare
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a40006a1-876e-474b-aaf1-f365c730deea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 47d5bda20c79ac069c8c51e51535de49b5ca380d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821892"
---
# <a name="logman-start--stop"></a>Logman start | arrestare

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

avviare un agente di raccolta dati e impostare il momento di avvio su manuale o arrestare un agente di raccolta dati impostare e l'ora di fine su manuale.  
  
## <a name="syntax"></a>Sintassi  
```  
logman start <[-n] <name>> [options]  
logman stop <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|-?|Vengono visualizzate sensibile al contesto della Guida.|  
|-s <computer name>|Eseguire il comando nel computer remoto specificato.|  
|-config <value>|Specifica il file di impostazioni che contiene le opzioni di comando.|  
|[-n] <name>|Nome dell'oggetto di destinazione.|  
|-ets|Inviare comandi alle sessioni di traccia eventi direttamente senza salvare o pianificare.|  
|-come|Eseguire l'operazione richiesta in modo asincrono.|  
## <a name="BKMK_examples"></a>Esempi  
Il comando seguente avvia perf_log l'agente di raccolta dati in server_1 il computer remoto.  
```  
logman start perf_log -s server_1  
```  
#### <a name="additional-references"></a>Riferimenti aggiuntivi  
[logman](logman.md)  
