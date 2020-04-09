---
title: prompt_1 FTP
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 930df39b-45c4-4e0b-bfe2-1d1963be817a vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9d16f70226e91e2e845480be8d83481fd76af173
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843154"
---
# <a name="ftp-prompt_1"></a>FTP: prompt_1

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di attivare e disattivare la modalità di **richiesta** .   
## <a name="syntax"></a>Sintassi  
```  
prompt  
```  
#### <a name="parameters"></a>Parametri  
nessuno  
## <a name="remarks"></a>Note  
- Per impostazione predefinita, il **prompt** è on.  
- richieste **FTP** durante i trasferimenti di più file per consentire di recuperare o archiviare i file in modo selettivo.  **Mget** e **mput** trasferiscono tutti i file se la **richiesta** è disattivata.  
  ## <a name="examples"></a><a name=BKMK_Examples></a>Esempi  
  Attiva o disattiva la modalità di richiesta.  
  ```  
  prompt  
  ```  
  ## <a name="additional-references"></a>Altre informazioni di riferimento  
- - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
