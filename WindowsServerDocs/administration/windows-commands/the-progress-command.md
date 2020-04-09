---
title: corso
description: Windows Commands Topic for progress, che visualizza lo stato di avanzamento durante l'esecuzione di un comando.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8ce5e77b-e13f-4ac3-948d-31770a6c7e25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9957174203df8f2f5c02bf3ab8a4f3406701a8da
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833124"
---
# <a name="progress"></a>corso

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