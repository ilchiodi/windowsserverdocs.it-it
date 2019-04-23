---
title: mget FTP
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6c85ae96-ec51-48a9-a227-7f02c7332c69 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1160ec742dde318141da720bd35b7d60ab805bb1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888422"
---
# <a name="ftp-mget"></a>ftp: mget

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Tipo di trasferimento copia i file remoti nel computer locale usando il file corrente.   
## <a name="syntax"></a>Sintassi  
```  
mget <remoteFile>[ ]  
```  
### <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|<remoteFile>|Specifica i file remoti da copiare nel computer locale.|  
## <a name="BKMK_Examples"></a>Esempi  
copiare i file remoti **a.exe** e **b.exe** nel computer locale usando il tipo di trasferimento di file corrente.  
```  
mget a.exe b.exe  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [ftp: ascii](ftp-ascii.md)  
-   [ftp: binary](ftp-binary.md)  
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)  
