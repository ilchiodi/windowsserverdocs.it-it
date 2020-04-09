---
title: TestDCs Dfsdiag
description: Windows Commands Topic for Dfsdiag TestDCs, che controlla la configurazione dei controller di dominio nel dominio specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: abb915ab-23eb-45d7-9a2e-b6b9a5756a70
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 092ce3710eb6d209f596683bd4ad054dadd11aa3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846319"
---
# <a name="dfsdiag-testdcs"></a>TestDCs Dfsdiag

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
|/Domain:`<domain_name>`|Dominio a cui si desidera verificare.|  
  
## <a name="remarks"></a>Note  

/Domain è un parametro facoltativo. Il valore predefinito è il dominio locale appartenente a host locale.  
  
## <a name="examples"></a><a name=BKMK_Examples></a>Esempi  
Per verificare la configurazione dei controller di dominio nel dominio Contoso.com, digitare:  
  
```  
dfsdiag /TestDCs /Domain:Contoso.com  
```  
  
## <a name="additional-references"></a>Altre informazioni di riferimento  
  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  
-   [Dfsdiag](dfsdiag.md)  
  

