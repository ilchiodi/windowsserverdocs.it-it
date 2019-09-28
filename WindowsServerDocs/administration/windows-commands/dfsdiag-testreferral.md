---
title: TestReferral Dfsdiag
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: af22520d2c89f9d9f9d91ea6f43a33f3ff9c57f1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378365"
---
# <a name="dfsdiag-testreferral"></a>TestReferral Dfsdiag

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Controlla File System distribuito \(DFS\) riferimenti eseguendo i test seguenti:  
  
-   Quando si usa il parametro DFSpath senza argomenti, questo comando convalida che l'elenco di riferimenti includa tutti i domini trusted.  
  
-   Quando si specifica un dominio, il comando esegue un controllo di integrità dei controller di dominio \(dfsdiag \/testdcs @ no__t-2 e testa le associazioni del sito e la cache di dominio dell'host locale.  
  
-   Quando si specifica un dominio e \\SYSvol o \\NETLOGON, oltre a eseguire gli stessi controlli di integrità di quando si specifica un dominio, il comando verifica che la durata (TTL) \(TTL @ no__t-3 dei riferimenti SYSvol o NETLOGON corrisponda al valore predefinito 900 secondi.  
  
-   Quando si specifica una radice dello spazio dei nomi, oltre a eseguire gli stessi controlli di integrità di quando si specifica un dominio, il comando esegue un controllo della configurazione DFS \(dfsdiag \/TestDFSConfig @ no__t-2 e un controllo di integrità dello spazio dei nomi \(dfsdiag @no__ t-4TestDFSIntegrity @ no__t-5.  
  
-   Quando si specifica una cartella DFS \(link @ no__t-1, oltre a eseguire gli stessi controlli di integrità di quando si specifica una radice dello spazio dei nomi, il comando convalida la configurazione del sito per le destinazioni cartella \(dfsdiag \/testsites @ no__t-4 e convalida Associazione sito dell'host locale.  
  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
dfsdiag /TestReferral /DFSpath:<DFS path for getting referrals> [/Full]  
```  
  
### <a name="parameters"></a>Parametri  
  
|Parametro|Descrizione|  
|-------|--------|  
|\/DFSpath: <path for getting referrals>|Il percorso DFS può essere uno dei seguenti:<br /><br />-    @ no__t-1blank @ no__t-2: Verifica i domini trusted.<br />-    @ no__t-1 @ no__t-2Domain: Riferimenti del controller di dominio.<br />-    @ no__t-1 @ no__t-2Domain @ no__t-3SYSvol: Riferimenti SYSvol.<br />-    @ no__t-1 @ no__t-2Domain @ no__t-3NETLOGON: Riferimenti NETLOGON.<br />-    @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5: Riferimenti alla radice dello spazio dei nomi.<br />-    @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7: Cartella DFS \(link @ no__t-1 riferimenti.|  
|\/Full|Applicabile solo a riferimenti dominio radice. Verifica la coerenza delle informazioni di associazione del sito tra il registro di sistema e servizi di dominio Active Directory \(AD DS @ no__t-1.|  
  
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
  
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  

