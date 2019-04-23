---
title: Dfsdiag TestDFSIntegrity
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 173ee832-26e1-4ec8-a23a-38a7d6229ac3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9a79e034f7c60be89266eb29dcd69e8f73b2aafe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837092"
---
# <a name="dfsdiag-testdfsintegrity"></a>Dfsdiag TestDFSIntegrity

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Controlla l'integrità del File System distribuito \(DFS\) dello spazio dei nomi eseguendo i test seguenti:  
  
-   Verifica che le incoerenze tra i controller di dominio o danneggiamento dei metadati DFS.  
  
-   Viene convalidata la configurazione di accesso\-basato su enumerazione per assicurarsi che sia coerenza tra i metadati DFS e la condivisione del server dello spazio dei nomi.  
  
-   Rileva le cartelle DFS sovrapposte \(collegamenti\), cartelle duplicate e le cartelle con destinazioni cartella sovrapposti.  
  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
dfsdiag /TestDFSIntegrity /DFSRoot:<DFS root path> [/Recurse] [/Full]  
```  
  
### <a name="parameters"></a>Parametri  
  
|Parametro|Descrizione|  
|-------|--------|  
|\/DFSRoot:<DFS root path>|Lo spazio dei nomi DFS per la diagnosi.|  
|\/Recurse|Esegue il test incluso che lo spazio dei nomi interlinks.|  
|\/completo|verifica la coerenza della condivisione e configurazione lato client e gli ACL NTFS in tutte le destinazioni cartella. Verifica inoltre che la proprietà online è impostata.|  
  
## <a name="BKMK_Examples"></a>Esempi  
Per TBD, digitare:  
  
```  
dfsdiag /TestDFSIntegrity /DFSRoot:\\Contoso.com\MyNamespace /Recurse /Full  
```  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
  
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

