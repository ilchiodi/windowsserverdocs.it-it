---
title: dfsdiag TestDFSConfig
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 106aeeb9-ea79-4e6e-829c-eca06309bab2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: affb3c879275228fa0ec17f77ad77db6fc44d6ab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814772"
---
# <a name="dfsdiag-testdfsconfig"></a>dfsdiag TestDFSConfig

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Controlla la configurazione di un File System distribuito \(DFS\) dello spazio dei nomi, eseguendo le azioni seguenti:  
  
-   verifica che il servizio DFS Namespace è in esecuzione e che il tipo di avvio sia impostato su automatico in tutti i server dello spazio dei nomi.  
  
-   verifica che la configurazione del Registro di sistema DFS sia coerenza tra i server dello spazio dei nomi.  
  
-   Convalida le dipendenze seguenti nei server dello spazio dei nomi del cluster che eseguono Windows Server 2008 o versione successiva:  
  
    -   Dipendenza delle risorse di primo livello dello spazio dei nomi sulla stessa risorsa nome rete.  
  
    -   Dipendenza dalla risorsa nome risorsa indirizzo IP di rete.  
  
    -   Dipendenza delle risorse di primo livello dello spazio dei nomi sulla risorsa disco fisico.  
  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
dfsdiag /TestDFSConfig /DFSRoot:<namespace>  
```  
  
### <a name="parameters"></a>Parametri  
  
|Parametro|Descrizione|  
|-------|--------|  
|\/DFSRoot:<namespace>|Lo spazio dei nomi \(radice DFS\) per la diagnosi.|  
  
## <a name="BKMK_Examples"></a>Esempi  
Per TBD, digitare:  
  
```  
dfsdiag /TestDFSConfig /DFSRoot:\\Contoso.com\MyNamespace  
```  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
  
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

