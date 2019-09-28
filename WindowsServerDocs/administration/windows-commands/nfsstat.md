---
title: nfsstat
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da7a9768-44bd-404b-97ee-c388d00dc395
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4f9db119596b5602f18acfa10af6aa1b7cbbc9b2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373183"
---
# <a name="nfsstat"></a>nfsstat



È possibile utilizzare **nfsstat** per visualizzare o reimpostare i conteggi delle chiamate effettuate a server per NFS.

## <a name="syntax"></a>Sintassi

```
nfsstat [-z]
```

## <a name="description"></a>Descrizione

Se utilizzata senza l'opzione **-z** , l'utilità da riga di comando **nfsstat** Visualizza il numero di chiamate NFS V2, NFS v3 e Mount V3 effettuate al server poiché i contatori sono stati impostati su 0, quando il servizio è stato avviato o quando i contatori sono stati reimpostati utilizzando  **nfsstat-z**.