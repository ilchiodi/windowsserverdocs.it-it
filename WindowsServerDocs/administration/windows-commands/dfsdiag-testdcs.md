---
title: Dfsdiag TestDCs
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 62956ae65d2311939ac0db6a4b86950f21dba407
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836602"
---
# <a name="dfsdiag-testdcs"></a>Dfsdiag TestDCs

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Controlla la configurazione dei controller di dominio eseguendo i test seguenti su ogni controller di dominio nel dominio specificato:  
  
-   verifica che il File System distribuito \(DFS\) Namespace servizio è in esecuzione e che il tipo di avvio sia impostato su automatico.  
  
-   Cerca il supporto del sito\-stimata dei riferimenti NETLOGON e SYSvol.  
  
-   verifica la coerenza dell'associazione del sito per il nome host e indirizzo IP.  
  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
dfsdiag /TestDCs [/Domain:<Domain name>]  
```  
  
### <a name="parameters"></a>Parametri  
  
|Parametro|Descrizione|  
|-------|--------|  
|\/Dominio:<Domain name>|Dominio a cui si desidera verificare.|  
  
## <a name="remarks"></a>Note  
\/Dominio è un parametro facoltativo. Il valore predefinito è il dominio locale appartenente a host locale.  
  
## <a name="BKMK_Examples"></a>Esempi  
Per verificare la configurazione dei controller di dominio nel dominio Contoso.com, digitare:  
  
```  
dfsdiag /TestDCs /Domain:Contoso.com  
```  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
  
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

