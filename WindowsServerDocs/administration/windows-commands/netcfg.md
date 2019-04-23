---
title: netcfg
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: aed535f843da6d735526ea97c07f94564dc00dc6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871322"
---
# <a name="netcfg"></a>netcfg

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Installa l'ambiente di preinstallazione di Windows (WinPE), una versione leggera di Windows usato per distribuire le workstation.   
## <a name="syntax"></a>Sintassi  
```  
netcfg [/v] [/e] [/winpe] [/l ] /c /i  
```  
### <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|/v|Esecuzione in modalità dettagliata (dettagliata)|  
|/e|Usare variabili di ambiente per la manutenzione durante l'installazione e disinstallazione|  
|/winpe|Consente di installare il Client di Microsoft, NetBIOS e TCP/IP per Ambiente preinstallazione di Windows|  
|/l|Fornisce la posizione del INF|  
|/c|Fornisce la classe del componente da installare; protocollo, servizio o client|  
|/i|Fornisce l'ID componente|  
|/s|Fornisce il tipo di componenti per mostrare<br /><br />\tA adapter, n = = netti componenti|  
|/?|Visualizza la guida al prompt dei comandi.|  
## <a name="BKMK_Examples"></a>Esempi  
Per installare il protocollo *esempio* usando c:\oemdir\example.inf:  
```  
netcfg /l c:\oemdir\example.inf /c p /i example  
```  
Per installare il *MS_Server* servizio:  
```  
netcfg /c s /i MS_Server  
```  
Per installare TCP/IP, NetBIOS e il Client di Microsoft per l'ambiente preinstallazione di Windows  
```  
netcfg /v /winpe  
```  
Per visualizzare se componente *MS_IPX* è installato:  
```  
netcfg /q MS_IPX  
```  
Per disinstallare il componente *MS_IPX*:  
```  
netcfg /u MS_IPX  
```  
Per visualizzare tutti i componenti netti installati:  
```  
netcfg /s n  
```  
A presentazioni associazione i percorsi contenenti *MS_TCPIP*:  
```  
netcfg /b ms_tcpip  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)  
