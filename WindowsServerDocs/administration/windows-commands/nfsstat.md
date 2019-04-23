---
title: nfsstat
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9db8b903d4c3681b2b3bae3424f8af83696ae2c7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853292"
---
# <a name="nfsstat"></a>nfsstat



È possibile usare **nfsstat** per visualizzare o reimpostare i conteggi delle chiamate effettuate a Server per NFS.

## <a name="syntax"></a>Sintassi

```
nfsstat [-z]
```

## <a name="description"></a>Descrizione

Quando viene utilizzata senza le **- z** opzione, il **nfsstat** utilità della riga di comando Visualizza il numero di NFS V2, NFS V3 e montaggio V3 chiamate al server perché i contatori sono stati impostati su 0, quando il servizio avvio o quando sono stati reimpostati i contatori usando **nfsstat - z**.