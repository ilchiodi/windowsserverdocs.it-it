---
title: prompt_1 FTP
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 930df39b-45c4-4e0b-bfe2-1d1963be817a vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 08c7f9c14f4168bb5d3aa874711669eede8d0d87
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376203"
---
# <a name="ftp-prompt_1"></a>FTP: prompt_1

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di attivare e disattivare la modalità di **richiesta** .   
## <a name="syntax"></a>Sintassi  
```  
prompt  
```  
### <a name="parameters"></a>Parametri  
nessuno  
## <a name="remarks"></a>Note  
- Per impostazione predefinita, il **prompt** è on.  
- richieste **FTP** durante i trasferimenti di più file per consentire di recuperare o archiviare i file in modo selettivo.  **Mget** e **mput** trasferiscono tutti i file se la **richiesta** è disattivata.  
  ## <a name="BKMK_Examples"></a>Esempi  
  Attiva o disattiva la modalità di richiesta.  
  ```  
  prompt  
  ```  
  ## <a name="additional-references"></a>Riferimenti aggiuntivi  
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
