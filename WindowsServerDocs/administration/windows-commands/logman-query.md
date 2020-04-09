---
title: Logman query
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1116a0f0-5415-4369-a045-12f79f8f66de
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e7b7cc202266a568108c7cbf0eac89260721014a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840664"
---
# <a name="logman-query"></a>Logman query

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

agente di raccolta dati query o proprietà dell'insieme agenti di raccolta dati.  

## <a name="syntax"></a>Sintassi  
```  
logman query [providers|Data Collector Set name] [options]  
```  
### <a name="parameters"></a>Parametri  

|     Parametro      |                                 Descrizione                                  |
|--------------------|------------------------------------------------------------------------------|
|         /?         |                       Vengono visualizzate sensibile al contesto della Guida.                       |
| -s <computer name> |            Eseguire il comando nel computer remoto specificato.             |
|  -config <value>   |           Specifica il file di impostazioni che contiene le opzioni di comando.            |
|    [-n] <name>     |                          Nome dell'oggetto di destinazione.                          |
|        -ets        | Inviare comandi alle sessioni di traccia eventi direttamente senza salvare o pianificare. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi  
Il seguente comando Elenca tutti i set di agenti di raccolta dati configurati nel sistema di destinazione.  
```  
logman query  
```  
Il seguente comando Elenca agenti di raccolta dati contenuti in denominata perf_log insieme di raccolta dati.  
```  
logman query perf_log  
```  
Il seguente comando Elenca tutti i provider disponibili agenti di raccolta dati nel sistema di destinazione.  
```  
logman query providers  
```  
## <a name="additional-references"></a>Altre informazioni di riferimento  
[logman](logman.md)  
