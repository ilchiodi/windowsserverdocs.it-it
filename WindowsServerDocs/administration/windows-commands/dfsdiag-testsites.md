---
title: TestSites Dfsdiag
description: Argomento di riferimento per Dfsdiag TestSites, che controlla la configurazione dei siti di servizi di dominio Active Directory (AD DS) verificando che i server che fungono da server dello spazio dei nomi o cartella (collegamento) abbiano le stesse associazioni di sito in tutti i controller di dominio.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 39a0d415-7eb7-4a26-861b-7ff00c45dcda
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 68048699a812beac94fa121d6801da5f42e5393b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719559"
---
# <a name="dfsdiag-testsites"></a>TestSites Dfsdiag

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Controlla la configurazione dei siti di servizi di dominio Active Directory (AD DS) verificando che i server che fungono da server dello spazio dei nomi o da destinazioni di cartelle (collegamento) dispongano delle stesse associazioni di sito in tutti i controller di dominio.

## <a name="syntax"></a>Sintassi  
  
```  
dfsdiag /TestSites </Machine:<server name>| /DFSpath:<namespace root or DFS folder> [/Recurse]> [/Full]  
```  
  
#### <a name="parameters"></a>Parametri  
  
|Parametro|Descrizione|  
|-------|--------|  
|\/Macchina<server name>|Il nome del server in cui si desidera verificare l'associazione del sito.|  
|\/DFSpath<namespace root or DFS folder>|La radice dello spazio dei nomi o la cartella file system distribuito (DFS) (collegamento) con le destinazioni per cui verificare l'associazione del sito.|  
|\/Recurse|Enumera e verifica le associazioni di sito per tutte le destinazioni cartella sotto la radice dello spazio dei nomi specificato.|  
|\/Completo|Verifica che servizi di dominio Active Directory e il registro di sistema del server contengano le stesse informazioni di associazione del sito.|  
  
## <a name="examples"></a>Esempi  
  
```  
dfsdiag /TestSites /Machine:MyServer  
```  
 
```  
dfsdiag /TestSites /DFSpath:\\Contoso.com\Namespace1\Folder1 /Full  
```  
  
```  
dfsdiag /TestSites /DFSpath:\\Contoso.com\Namespace2 /Recurse /Full  
```  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  
-   [Dfsdiag](dfsdiag.md)  
  

