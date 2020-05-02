---
title: showmount
description: Argomento di riferimento per showmount, che consente di visualizzare le directory montate.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a6dd562e-e3bd-4ee6-be3b-6d29e29fd20e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 60057c154e7a646d14a0e57d5cf4cd72d0f90876
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721808"
---
# <a name="showmount"></a>showmount

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile usare **showmount** per visualizzare le directory montate.  
  
## <a name="syntax"></a>Sintassi  
```
showmount {-e|-a|-d} <Server>  
```

## <a name="description"></a>Descrizione  
L' **showmount** utilità della\-riga di comando showmount visualizza informazioni sui file system montati esportati da server per NFS nel computer specificato dal *server*. Se il *Server* non è specificato, **showmount** Visualizza informazioni sul computer in cui viene eseguito il comando **showmount** .  
  
È necessario specificare una delle opzioni seguenti:  
  
- e-Visualizza tutti i file system esportati nel server. ** \-**  
- a: Visualizza tutti i client NFS \(\) di file System di rete e le directory nel server in cui è installato ciascuno di essi. ** \-**  
- d: Visualizza tutte le directory nel server attualmente montate dai client NFS. ** \-**  
  
## <a name="see-also"></a>Vedi anche  
[Informazioni di riferimento sui comandi di Servizi per NFS](services-for-network-file-system-command-reference.md)  