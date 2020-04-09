---
title: nfsstat
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: da7a9768-44bd-404b-97ee-c388d00dc395
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94eb389fdd694c08dcd1d77325f145a81e59ea0f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838884"
---
# <a name="nfsstat"></a>nfsstat



È possibile utilizzare **nfsstat** per visualizzare o reimpostare i conteggi delle chiamate effettuate a server per NFS.

## <a name="syntax"></a>Sintassi

```
nfsstat [-z]
```

## <a name="description"></a>Descrizione

Se utilizzata senza l'opzione **-z** , l'utilità da riga di comando **nfsstat** Visualizza il numero di chiamate NFS V2, NFS v3 e Mount V3 effettuate al server poiché i contatori sono stati impostati su 0, quando il servizio è stato avviato o quando i contatori sono stati reimpostati tramite **nfsstat-z**.