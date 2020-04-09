---
title: TestSites Dfsdiag
description: Windows Commands Topic for Dfsdiag TestSites, che controlla la configurazione dei siti di servizi di dominio Active Directory (AD DS) verificando che i server che fungono da server dello spazio dei nomi o cartella (collegamento) abbiano le stesse associazioni di sito in tutti i controller di dominio.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 39a0d415-7eb7-4a26-861b-7ff00c45dcda
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 80cc9095748dafb030b204130bfa2ccb61ec69ea
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846224"
---
# <a name="dfsdiag-testsites"></a>TestSites Dfsdiag

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Controlla la configurazione dei siti di servizi di dominio Active Directory (AD DS) verificando che i server che fungono da server dello spazio dei nomi o da destinazioni di cartelle (collegamento) dispongano delle stesse associazioni di sito in tutti i controller di dominio.

## <a name="syntax"></a>Sintassi  
  
```  
dfsdiag /TestSites </Machine:<server name>| /DFSpath:<namespace root or DFS folder> [/Recurse]> [/Full]  
```  
  
#### <a name="parameters"></a>Parametri  
  
|Parametro|Descrizione|  
|-------|--------|  
|Computer \/:<server name>|Il nome del server in cui si desidera verificare l'associazione del sito.|  
|\/DFSpath:<namespace root or DFS folder>|La radice dello spazio dei nomi o la cartella file system distribuito (DFS) (collegamento) con le destinazioni per cui verificare l'associazione del sito.|  
|recurse \/|Enumera e verifica le associazioni di sito per tutte le destinazioni cartella sotto la radice dello spazio dei nomi specificato.|  
|\/completo|Verifica che servizi di dominio Active Directory e il registro di sistema del server contengano le stesse informazioni di associazione del sito.|  
  
## <a name="examples"></a><a name=BKMK_Examples></a>Esempi  
  
```  
dfsdiag /TestSites /Machine:MyServer  
```  
 
```  
dfsdiag /TestSites /DFSpath:\\Contoso.com\Namespace1\Folder1 /Full  
```  
  
```  
dfsdiag /TestSites /DFSpath:\\Contoso.com\Namespace2 /Recurse /Full  
```  
  
## <a name="additional-references"></a>Altre informazioni di riferimento  
  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  
-   [Dfsdiag](dfsdiag.md)  
  

