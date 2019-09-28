---
title: TestSites Dfsdiag
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: af72da64dd20d4b37824355a494cb8f97f597b28
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378385"
---
# <a name="dfsdiag-testsites"></a>TestSites Dfsdiag

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Controlla la configurazione dei siti di servizi di dominio Active Directory \(AD DS @ no__t-1 verificando che i server che fungono da server dello spazio dei nomi o cartella \(Link @ no__t-3 abbiano le stesse associazioni di sito in tutti i controller di dominio.  
  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
dfsdiag /TestSites </Machine:<server name>| /DFSpath:<namespace root or DFS folder> [/Recurse]> [/Full]  
```  
  
### <a name="parameters"></a>Parametri  
  
|Parametro|Descrizione|  
|-------|--------|  
|\/Machine: <server name>|Il nome del server in cui si desidera verificare l'associazione del sito.|  
|\/DFSpath: <namespace root or DFS folder>|Il File System distribuito o dello spazio dei nomi radice \(DFS\) cartella \(collegamento\) con le destinazioni per il quale verificare l'associazione del sito.|  
|\/Recurse|Enumera e verifica le associazioni di sito per tutte le destinazioni cartella sotto la radice dello spazio dei nomi specificato.|  
|\/Full|Verifica che servizi di dominio Active Directory e il registro di sistema del server contengano le stesse informazioni di associazione del sito.|  
  
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
  
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  
-   [Dfsdiag](dfsdiag.md)  
  

