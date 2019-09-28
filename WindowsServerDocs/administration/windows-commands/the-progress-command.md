---
title: Il comando lo stato di avanzamento
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8ce5e77b-e13f-4ac3-948d-31770a6c7e25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 841d9103354e3162489492ba7dd97e726b37d37d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370185"
---
# <a name="the-progress-command"></a>Il comando lo stato di avanzamento



Visualizza lo stato di avanzamento durante l'esecuzione di un comando. È possibile utilizzare **/stato di avanzamento** con tutti gli altri comandi WDSUTIL eseguiti. Si noti che è necessario specificare **/Verbose** e **/stato di avanzamento** direttamente dopo **WDSUTIL**.

## <a name="syntax"></a>Sintassi

```
WDSUTIL /progress <commands>
```

## <a name="examples"></a>Esempi

Per inizializzare il server e visualizzare lo stato di avanzamento, digitare:
```
WDSUTIL /Verbose /Progress /Initialize-Server /Server:MyWDSServer /RemInst:"C:\RemoteInstall"
```