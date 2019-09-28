---
title: TestDCs Dfsdiag
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: abb915ab-23eb-45d7-9a2e-b6b9a5756a70
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a193e68b6f015b1535a98e20b52deb2a4a14034c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378445"
---
# <a name="dfsdiag-testdcs"></a>TestDCs Dfsdiag

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Controlla la configurazione dei controller di dominio eseguendo i test seguenti su ogni controller di dominio nel dominio specificato:  
  
-   Verifica che il servizio dello spazio dei nomi file system distribuito \(DFS @ no__t-1 sia in esecuzione e che il tipo di avvio sia impostato su automatico.  
  
-   Verifica il supporto dei riferimenti di Site @ no__t-0costed per NETLOGON e SYSvol.  
  
-   Verifica la coerenza dell'associazione del sito in base al nome host e all'indirizzo IP.  
  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
dfsdiag /TestDCs [/Domain:<Domain name>]  
```  
  
### <a name="parameters"></a>Parametri  
  
|Parametro|Descrizione|  
|-------|--------|  
|\/Domain: <Domain name>|Dominio a cui si desidera verificare.|  
  
## <a name="remarks"></a>Note  
\/Domain è un parametro facoltativo. Il valore predefinito è il dominio locale appartenente a host locale.  
  
## <a name="BKMK_Examples"></a>Esempi  
Per verificare la configurazione dei controller di dominio nel dominio Contoso.com, digitare:  
  
```  
dfsdiag /TestDCs /Domain:Contoso.com  
```  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
  
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  
-   [Dfsdiag](dfsdiag.md)  
  

