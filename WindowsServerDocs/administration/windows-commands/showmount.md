---
title: showmount
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a6dd562e-e3bd-4ee6-be3b-6d29e29fd20e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 26acf24922e6ac53a5c902d65eb0f23bff0af93b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852352"
---
# <a name="showmount"></a>showmount

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile usare **showmount** per visualizzare le directory montate.  
  
## <a name="syntax"></a>Sintassi  
```
showmount {-e|-a|-d} <Server>  
```

## <a name="description"></a>Descrizione  
Il **showmount** comandi\-utilità della riga vengono visualizzate informazioni sui sistemi di file montati esportato dal Server per NFS nel computer specificato dal *Server*. Se *Server* non viene fornito **showmount** Visualizza le informazioni relative al computer in cui il **showmount** comando viene eseguito.  
  
È necessario specificare una delle opzioni seguenti:  
  
- **\-e** -consente di visualizzare tutti i sistemi esportati nel server di file.  
- **\-una** -consente di visualizzare tutte le Network File System \(NFS\) client e le directory nel server di ognuno è montato.  
- **\-1!d** -consente di visualizzare tutte le directory nel server in cui sono attualmente montate dal client NFS.  
  
## <a name="see-also"></a>Vedere anche  
[Servizi per riferimento ai comandi di Network File System](services-for-network-file-system-command-reference.md)  