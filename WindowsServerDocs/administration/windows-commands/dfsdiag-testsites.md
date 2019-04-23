---
title: Dfsdiag TestSites
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 39a0d415-7eb7-4a26-861b-7ff00c45dcda
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6486c79cfa58bb262fd3161ad0801e84185b6629
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873972"
---
# <a name="dfsdiag-testsites"></a>Dfsdiag TestSites

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Controlla la configurazione dei servizi di dominio active directory \(AD DS\) siti verificando che i server che fungono da server dello spazio dei nomi o la cartella \(collegamento\) destinazioni hanno le stesse associazioni di sito nel dominio tutti controller.  
  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
dfsdiag /TestSites </Machine:<server name>| /DFSpath:<namespace root or DFS folder> [/Recurse]> [/Full]  
```  
  
### <a name="parameters"></a>Parametri  
  
|Parametro|Descrizione|  
|-------|--------|  
|\/Computer:<server name>|Il nome del server in cui si desidera verificare l'associazione del sito.|  
|\/DFSpath:<namespace root or DFS folder>|Il File System distribuito o dello spazio dei nomi radice \(DFS\) cartella \(collegamento\) con le destinazioni per il quale verificare l'associazione del sito.|  
|\/Recurse|Enumera e verifica le associazioni di sito per tutte le destinazioni cartella sotto la radice dello spazio dei nomi specificato.|  
|\/completo|verifica che Active Directory Domain Services e il Registro di sistema del server contengono le stesse informazioni di associazione di sito.|  
  
## <a name="BKMK_Examples"></a>Esempi  
Per TBD, digitare:  
  
```  
dfsdiag /TestSites /Machine:MyServer  
```  
  
Per TBD, digitare:  
  
```  
dfsdiag /TestSites /DFSpath:\\Contoso.com\Namespace1\Folder1 /Full  
```  
  
Per TBD, digitare:  
  
```  
dfsdiag /TestSites /DFSpath:\\Contoso.com\Namespace2 /Recurse /Full  
```  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
  
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

