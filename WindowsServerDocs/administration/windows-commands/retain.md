---
title: mantenere
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: eeab0aef-2ba5-441a-a10d-bbef6f0d7e3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 891192c0d6b1687cf6bffea0f57d1853e023b776
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835764"
---
# <a name="retain"></a>mantenere



Prepara un volume semplice dinamico esistente da utilizzare come un avvio o il volume di sistema.

## <a name="syntax"></a>Sintassi

```
retain
```

## <a name="remarks"></a>Note

-   Su un disco dinamico MBR (record) di avvio principale, questo comando crea una voce di partizione nel record di avvio principale.
-   In un disco GPT (tabella) di partizione GUID, questo comando crea una voce di partizione della tabella di partizione GUID.

## <a name="additional-references"></a>Altre informazioni di riferimento

