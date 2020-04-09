---
title: showmount
description: Windows Commands argomento per showmount, che visualizza le directory montate.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a6dd562e-e3bd-4ee6-be3b-6d29e29fd20e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6fa61d47bb14cf21d93beec0a6e9257b9f66737b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834224"
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