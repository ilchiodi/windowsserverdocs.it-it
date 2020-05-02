---
title: Logman start | arrestare
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a40006a1-876e-474b-aaf1-f365c730deea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 68f570d99d4b3eaa818c9fbdcce76c42d1cb12d4
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724340"
---
# <a name="logman-start--stop"></a>Logman start | arrestare

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

avviare un agente di raccolta dati e impostare l'ora di inizio su manuale oppure arrestare un insieme agenti di raccolta dati e impostare l'ora di fine su manuale.  

## <a name="syntax"></a>Sintassi  
```  
logman start <[-n] <name>> [options]  
logman stop <[-n] <name>> [options]  
```  
### <a name="parameters"></a>Parametri  

|     Parametro      |                                 Descrizione                                  |
|--------------------|------------------------------------------------------------------------------|
|         -?         |                       Vengono visualizzate sensibile al contesto della Guida.                       |
| -s<computer name> |            Eseguire il comando nel computer remoto specificato.             |
|  -config <value>   |           Specifica il file di impostazioni che contiene le opzioni di comando.            |
|    [-n]<name>     |                          Nome dell'oggetto di destinazione.                          |
|        -ets        | Inviare comandi alle sessioni di traccia eventi direttamente senza salvare o pianificare. |
|        -come         |               Eseguire l'operazione richiesta in modo asincrono.                |

## <a name="examples"></a>Esempi  
Il comando seguente avvia perf_log l'agente di raccolta dati in server_1 il computer remoto.  
```  
logman start perf_log -s server_1  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
[logman](logman.md)  
