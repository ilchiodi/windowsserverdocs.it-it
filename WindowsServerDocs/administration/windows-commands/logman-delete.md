---
title: Logman delete
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8f3b2422-3dce-4fb4-adbb-8536b1d7da2b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8b30fd6eb7915d3d0296988a98968dcde58bdbc2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724369"
---
# <a name="logman-delete"></a>Logman delete

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Elimina un agente di raccolta dati esistente.  

## <a name="syntax"></a>Sintassi  
```  
logman delete <[-n] <name>> [options]  
```  
### <a name="parameters"></a>Parametri  

|        Parametro        |                                                                               Descrizione                                                                               |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /?            |                                                                    Vengono visualizzate sensibile al contesto della Guida.                                                                     |
|   -s<computer name>    |                                                          Eseguire il comando nel computer remoto specificato.                                                          |
|     -config <value>     |                                                         Specifica il file di impostazioni che contiene le opzioni di comando.                                                         |
|       [-n]<name>       |                                                                   Nome dell'agente di raccolta di dati di destinazione.                                                                    |
|          -ets           |                                              Inviare comandi alle sessioni di traccia eventi direttamente senza salvare o pianificare.                                               |
| -u [-] < utente [password] > | Specifica l'utente di Esegui come. L'immissione \* di un oggetto per la password genera una richiesta per la password. La password non viene visualizzata quando si digita. |

## <a name="examples"></a>Esempi  
Il comando seguente elimina perf_log l'agente di raccolta dati.  
```  
logman delete perf_log  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
[logman](logman.md)  
