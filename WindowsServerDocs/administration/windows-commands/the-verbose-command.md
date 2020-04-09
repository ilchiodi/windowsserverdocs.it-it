---
title: verbose
description: Windows Commands Topic for Verbose, che Visualizza l'output dettagliato per un comando specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fcf30ae7-818f-4e7e-a083-a1812682032b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f67a4fe99a5051e8844e6386d87911946a808c1e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832844"
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