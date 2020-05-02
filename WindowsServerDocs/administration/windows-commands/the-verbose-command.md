---
title: verbose
description: Argomento di riferimento per Verbose, che Visualizza l'output dettagliato per un comando specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fcf30ae7-818f-4e7e-a083-a1812682032b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3563673d1f80167e469d98a664a6f96ca49815a1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721367"
---
# <a name="verbose"></a>verbose

Visualizza output dettagliato per il comando specificato. È possibile utilizzare **/Verbose** con tutti gli altri comandi WDSUTIL eseguiti. Si noti che è necessario specificare **/Verbose** e **/stato di avanzamento** direttamente dopo **WDSUTIL**.

## <a name="syntax"></a>Sintassi

```
WDSUTIL /verbose <commands>
```

## <a name="examples"></a>Esempi

Per eliminare computer approvati dal database di aggiunta automatica e visualizzare output dettagliato, digitare:
```
WDSUTIL /Verbose /progress /Delete-AutoAddDevices /Server:MyWDSServer /DeviceType:ApprovedDevices
```