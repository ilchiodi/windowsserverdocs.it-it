---
title: progress
description: Argomento di riferimento per Progress, che visualizza lo stato di avanzamento durante l'esecuzione di un comando.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8ce5e77b-e13f-4ac3-948d-31770a6c7e25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3fcb06304df22208cef013d73a4ebf0af1991f88
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721426"
---
# <a name="progress"></a>progress

Visualizza lo stato di avanzamento durante l'esecuzione di un comando. È possibile utilizzare **/stato di avanzamento** con tutti gli altri comandi WDSUTIL eseguiti. Si noti che è necessario specificare **/Verbose** e **/stato di avanzamento** direttamente dopo **WDSUTIL**.

## <a name="syntax"></a>Sintassi

```
WDSUTIL /progress <commands>
```

## <a name="examples"></a>Esempi

Per inizializzare il server e visualizzare lo stato di avanzamento, digitare:
```
WDSUTIL /Verbose /Progress /Initialize-Server /Server:MyWDSServer /RemInst:C:\RemoteInstall
```