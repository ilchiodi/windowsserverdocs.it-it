---
title: Logman query
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 6acf6cf5240dd59357f4c788577190699a354744
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374424"
---
# <a name="logman-query"></a>Logman query

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

agente di raccolta dati query o proprietà dell'insieme agenti di raccolta dati.  

## <a name="syntax"></a>Sintassi  
```  
logman query [providers|"Data Collector Set name"] [options]  
```  
## <a name="parameters"></a>Parametri  

|     Parametro      |                                 Descrizione                                  |
|--------------------|------------------------------------------------------------------------------|
|         /?         |                       Vengono visualizzate sensibile al contesto della Guida.                       |
| -s <computer name> |            Eseguire il comando nel computer remoto specificato.             |
|  -config <value>   |           Specifica il file di impostazioni che contiene le opzioni di comando.            |
|    [-n] <name>     |                          Nome dell'oggetto di destinazione.                          |
|        -ets        | Inviare comandi alle sessioni di traccia eventi direttamente senza salvare o pianificare. |

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
