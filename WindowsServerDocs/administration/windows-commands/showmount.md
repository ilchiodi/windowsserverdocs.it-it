---
title: showmount
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: e1d197072db93130de880b5ec52d1875720b1d26
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383906"
---
# <a name="showmount"></a>showmount

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile usare **showmount** per visualizzare le directory montate.  
  
## <a name="syntax"></a>Sintassi  
```
showmount {-e|-a|-d} <Server>  
```

## <a name="description"></a>Descrizione  
Il comando **showmount**\-riga Utility Visualizza informazioni sui file system montati esportati da server per NFS nel computer specificato dal *Server*. Se il *Server* non è specificato, **showmount** Visualizza informazioni sul computer in cui viene eseguito il comando **showmount** .  
  
È necessario specificare una delle opzioni seguenti:  
  
- **\-e** -Visualizza tutti i file system esportati nel server.  
- **\-a** -Visualizza tutti i client \(NFS\) di file System di rete e le directory nel server in cui è installato.  
- **\-d** -Visualizza tutte le directory nel server attualmente montate dai client NFS.  
  
## <a name="see-also"></a>Vedi anche  
[Informazioni di riferimento sui comandi di Servizi per NFS](services-for-network-file-system-command-reference.md)  