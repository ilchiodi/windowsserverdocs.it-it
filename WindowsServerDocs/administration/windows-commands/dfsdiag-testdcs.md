---
title: TestDCs Dfsdiag
description: Argomento di riferimento per Dfsdiag TestDCs, che controlla la configurazione dei controller di dominio nel dominio specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: abb915ab-23eb-45d7-9a2e-b6b9a5756a70
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6ac7fe1a7bae6a7b3dab9004b6212b7d93774ade
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719599"
---
# <a name="dfsdiag-testdcs"></a>TestDCs Dfsdiag

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Controlla la configurazione dei controller di dominio eseguendo i test seguenti su ogni controller di dominio nel dominio specificato:  
  
-   Verifica che il servizio spazio dei nomi file system distribuito (DFS) sia in esecuzione e che il tipo di avvio sia impostato su automatico.  
  
-   Verifica il supporto dei riferimenti per il sito per NETLOGON e SYSvol.  
  
-   Verifica la coerenza dell'associazione del sito per il nome host e indirizzo IP.

## <a name="syntax"></a>Sintassi  
  
```  
dfsdiag /TestDCs [/Domain:<Domain name>]  
```  
  
#### <a name="parameters"></a>Parametri  
  
|Parametro|Descrizione|  
|-------|--------|  
|Dominio`<domain_name>`|Dominio a cui si desidera verificare.|  
  
## <a name="remarks"></a>Osservazioni  

/Domain è un parametro facoltativo. Il valore predefinito è il dominio locale appartenente a host locale.  
  
## <a name="examples"></a>Esempi  
Per verificare la configurazione dei controller di dominio nel dominio Contoso.com, digitare:  
  
```  
dfsdiag /TestDCs /Domain:Contoso.com  
```  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  
-   [Dfsdiag](dfsdiag.md)  
  

