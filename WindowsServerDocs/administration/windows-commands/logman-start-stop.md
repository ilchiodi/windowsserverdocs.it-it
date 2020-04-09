---
title: Logman start | arrestare
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a40006a1-876e-474b-aaf1-f365c730deea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2bd81a33779aa58e7528d0173a7a4b49489de8f9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840624"
---
# <a name="logman-start--stop"></a>Logman start | arrestare

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
| -s <computer name> |            Eseguire il comando nel computer remoto specificato.             |
|  -config <value>   |           Specifica il file di impostazioni che contiene le opzioni di comando.            |
|    [-n] <name>     |                          Nome dell'oggetto di destinazione.                          |
|        -ets        | Inviare comandi alle sessioni di traccia eventi direttamente senza salvare o pianificare. |
|        -come         |               Eseguire l'operazione richiesta in modo asincrono.                |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi  
Il comando seguente avvia perf_log l'agente di raccolta dati in server_1 il computer remoto.  
```  
logman start perf_log -s server_1  
```  
## <a name="additional-references"></a>Altre informazioni di riferimento  
[logman](logman.md)  
