---
title: netcfg
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e2daaab7-12db-4e36-b70c-db8906d084f7 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8f8368aaff16592a55cc9def84d593cf323f28ee
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373287"
---
# <a name="netcfg"></a>netcfg

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Installa il Ambiente preinstallazione di Windows (WinPE), una versione leggera di Windows usata per distribuire le workstation.   
## <a name="syntax"></a>Sintassi  
```  
netcfg [/v] [/e] [/winpe] [/l ] /c /i  
```  
### <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|/v|Esegui in modalità dettagliata (dettagliata)|  
|/e|Usare le variabili di ambiente di manutenzione durante l'installazione e la disinstallazione|  
|/winpe|Installa TCP/IP, NetBIOS e il client Microsoft per la preinstallazione di Windows ambiente|  
|/l|Fornisce il percorso del INF|  
|/c|Fornisce la classe del componente da installare. protocollo, servizio o client|  
|/i|Fornisce l'ID componente|  
|/s|Fornisce il tipo di componenti da visualizzare<br /><br />\ta = Adapters, n = NET Components|  
|/?|Visualizza la guida al prompt dei comandi.|  
## <a name="BKMK_Examples"></a>Esempi  
Per installare l' *esempio* di protocollo usando c:\oemdir\example.inf:  
```  
netcfg /l c:\oemdir\example.inf /c p /i example  
```  
Per installare il servizio *MS_Server* :  
```  
netcfg /c s /i MS_Server  
```  
Per installare TCP/IP, NetBIOS e il client Microsoft per l'ambiente preinstallazione di Windows  
```  
netcfg /v /winpe  
```  
Per visualizzare se il componente *MS_IPX* è installato:  
```  
netcfg /q MS_IPX  
```  
Per disinstallare il componente *MS_IPX*:  
```  
netcfg /u MS_IPX  
```  
Per visualizzare tutti i componenti .NET installati:  
```  
netcfg /s n  
```  
Per visualizzare i percorsi di associazione contenenti *MS_TCPIP*:  
```  
netcfg /b ms_tcpip  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
