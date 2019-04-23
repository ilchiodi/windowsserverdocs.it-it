---
title: Dfsdiag TestReferral
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 877c60dc-e993-4bd5-87dd-e892e3f98a1a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cd1b87befa8a9cfda5ea27a4ce5a5105ea1a1009
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848022"
---
# <a name="dfsdiag-testreferral"></a>Dfsdiag TestReferral

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Controlla File System distribuito \(DFS\) riferimenti eseguendo i test seguenti:  
  
-   Quando si usa il parametro DFSpath senza argomenti, questo comando verifica che l'elenco di riferimento include tutti i domini trusted.  
  
-   Quando si specifica un dominio, il comando esegue un controllo di integrità dei controller di dominio \(dfsdiag \/testdcs\) e verifica le associazioni di sito e la cache del dominio dell'host locale.  
  
-   Quando si specifica un dominio e \\SYSvol o \\NETLOGON, oltre a eseguire lo stesso stato controlla come quando si specifica un dominio, il comando verifica che il tempo TTL \(durata (TTL)\) di riferimenti di SYSvol e NETLOGON corrisponde al valore predefinito di 900 secondi.  
  
-   Quando si specifica una radice dello spazio dei nomi, oltre a eseguire lo stesso stato controlla come quando si specifica un dominio, il comando esegue un controllo della configurazione DFS \(dfsdiag \/TestDFSConfig\) e un controllo di integrità dello spazio dei nomi \(dfsdiag \/TestDFSIntegrity\).  
  
-   Quando si specifica una cartella DFS \(link\), oltre a eseguire lo stesso stato controlla come quando si specifica una radice dello spazio dei nomi, il comando viene convalidata la configurazione del sito per le destinazioni cartella \(dfsdiag \/ testsites\) e convalida l'associazione del sito dell'host locale.  
  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
dfsdiag /TestReferral /DFSpath:<DFS path for getting referrals> [/Full]  
```  
  
### <a name="parameters"></a>Parametri  
  
|Parametro|Descrizione|  
|-------|--------|  
|\/DFSpath:<path for getting referrals>|Il percorso DFS può essere uno dei seguenti:<br /><br />-   \(vuoto\): Domini trusted di test.<br />-   \\\\Dominio: Riferimenti di controller di dominio.<br />-   \\\\Domain\\SYSvol: Riferimenti di SYSvol.<br />-   \\\\Dominio\\NETLOGON: Riferimenti di NETLOGON.<br />-   \\\\<Domain or server>\\<Namespace Root>: Riferimenti alla radice di Namespace.<br />-   \\\\<Domain or server>\\<Namespace root>\\<DFS folder>: Cartella DFS \(collegamento\) dei riferimenti.|  
|\/completo|Applicabile solo a riferimenti dominio radice. verifica la coerenza delle informazioni di associazione del sito tra il Registro di sistema e i servizi di dominio active directory \(Active Directory Domain Services\).|  
  
## <a name="BKMK_Examples"></a>Esempi  
Per TBD, digitare:  
  
```  
dfsdiag /TestReferral /DFSpath:\\Contoso.com\MyNamespace  
```  
  
Per TBD, digitare:  
  
```  
dfsdiag /TestReferral /DFSpath:  
```  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
  
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)  
  

