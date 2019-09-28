---
title: Reimpostazione
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: a8903c300d12a019f8fb4aef6d367131a195d034
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371464"
---
# <a name="reset"></a>Reimpostazione



Reimposta DiskShadow.exe allo stato predefinito. **Reimpostare** è particolarmente utile per la separazione operazioni DiskShadow composte, ad esempio **creare**, **importare**, **backup**, o **ripristinare**.

## <a name="syntax"></a>Sintassi

```
reset
```

## <a name="remarks"></a>Note

-   Quando si utilizza il **reimpostare** comando, si perde lo stato di comandi, ad esempio **aggiungere**, **impostare**, **carico**, o **writer**. **Reset** rilascia anche le interfacce IVssBackupComponent e perde le copie shadow non persistenti.

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)