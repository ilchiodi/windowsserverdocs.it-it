---
title: reimpostare
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: afbdab44-199c-4e11-884f-e96804965c21
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0bd9b6735697cbcefdcf68dc3d4a53a6870a7a76
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850962"
---
# <a name="reset"></a>reimpostare



Reimposta DiskShadow.exe allo stato predefinito. **Reimpostare** è particolarmente utile per la separazione operazioni DiskShadow composte, ad esempio **creare**, **importare**, **backup**, o **ripristinare**.

## <a name="syntax"></a>Sintassi

```
reset
```

## <a name="remarks"></a>Note

-   Quando si utilizza il **reimpostare** comando, si perde lo stato di comandi, ad esempio **aggiungere**, **impostare**, **carico**, o **writer**. **Reimpostare** inoltre rilascia IVssBackupComponent interfacce e perde le copie shadow non permanente.

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)