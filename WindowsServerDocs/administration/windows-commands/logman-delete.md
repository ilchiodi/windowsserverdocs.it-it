---
title: Logman delete
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8f3b2422-3dce-4fb4-adbb-8536b1d7da2b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e0ab38eab988770de4fbcef8af2c7be6a6137b16
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840754"
---
# <a name="logman-delete"></a>Logman delete

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Elimina un agente di raccolta dati esistente.  

## <a name="syntax"></a>Sintassi  
```  
logman delete <[-n] <name>> [options]  
```  
### <a name="parameters"></a>Parametri  

|        Parametro        |                                                                               Descrizione                                                                               |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /?            |                                                                    Vengono visualizzate sensibile al contesto della Guida.                                                                     |
|   -s <computer name>    |                                                          Eseguire il comando nel computer remoto specificato.                                                          |
|     -config <value>     |                                                         Specifica il file di impostazioni che contiene le opzioni di comando.                                                         |
|       [-n] <name>       |                                                                   Nome dell'agente di raccolta di dati di destinazione.                                                                    |
|          -ets           |                                              Inviare comandi alle sessioni di traccia eventi direttamente senza salvare o pianificare.                                               |
| -u [-] < utente [password] > | Specifica l'utente di Esegui come. L'immissione di un \* per la password genera una richiesta per la password. La password non viene visualizzata quando si digita. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi  
Il comando seguente elimina perf_log l'agente di raccolta dati.  
```  
logman delete perf_log  
```  
## <a name="additional-references"></a>Altre informazioni di riferimento  
[logman](logman.md)  
