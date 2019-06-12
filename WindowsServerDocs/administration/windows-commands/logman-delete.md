---
title: Logman delete
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: e396d79dc936f56a69fac9469c020348640f1094
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437785"
---
# <a name="logman-delete"></a>Logman delete

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

eliminare una raccolta di dati esistente.  

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
| -u [-] < utente [password] > | Specifica l'utente di Esegui come. Immettere un \* per la password produce una richiesta per la password. La password non viene visualizzata quando si digita. |

## <a name="BKMK_examples"></a>Esempi  
Il comando seguente elimina perf_log l'agente di raccolta dati.  
```  
logman delete perf_log  
```  
#### <a name="additional-references"></a>Riferimenti aggiuntivi  
[logman](logman.md)  
