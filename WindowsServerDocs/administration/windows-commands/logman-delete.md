---
title: Logman delete
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8f3b2422-3dce-4fb4-adbb-8536b1d7da2b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8360a4955a5ebed3eb25fda77acf587c56fbf5d6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374431"
---
# <a name="logman-delete"></a>Logman delete

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Elimina un agente di raccolta dati esistente.  

## <a name="syntax"></a>Sintassi  
```  
logman delete <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Parametri  

|        Parametro        |                                                                               Descrizione                                                                               |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /?            |                                                                    Vengono visualizzate sensibile al contesto della Guida.                                                                     |
|   -s <computer name>    |                                                          Eseguire il comando nel computer remoto specificato.                                                          |
|     -config <value>     |                                                         Specifica il file di impostazioni che contiene le opzioni di comando.                                                         |
|       [-n] <name>       |                                                                   Nome dell'agente di raccolta di dati di destinazione.                                                                    |
|          -ets           |                                              Inviare comandi alle sessioni di traccia eventi direttamente senza salvare o pianificare.                                               |
| -u [-] < utente [password] > | Specifica l'utente di Esegui come. L'immissione di un \* per la password genera una richiesta per la password. La password non viene visualizzata quando si digita. |

## <a name="BKMK_examples"></a>Esempi  
Il comando seguente elimina perf_log l'agente di raccolta dati.  
```  
logman delete perf_log  
```  
#### <a name="additional-references"></a>Riferimenti aggiuntivi  
[logman](logman.md)  
