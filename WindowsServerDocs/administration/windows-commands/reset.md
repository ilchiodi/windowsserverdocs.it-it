---
title: reset
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: afbdab44-199c-4e11-884f-e96804965c21
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0701324ad1ee94cc645c7519d81fef7357b6a34a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722351"
---
# <a name="reset"></a>reset



Reimposta DiskShadow.exe allo stato predefinito. **Reimpostare** è particolarmente utile per la separazione operazioni DiskShadow composte, ad esempio **creare**, **importare**, **backup**, o **ripristinare**.

## <a name="syntax"></a>Sintassi

```
reset
```

## <a name="remarks"></a>Osservazioni

-   Quando si utilizza il **reimpostare** comando, si perde lo stato di comandi, ad esempio **aggiungere**, **impostare**, **carico**, o **writer**. **Reset** rilascia anche le interfacce IVssBackupComponent e perde le copie shadow non persistenti.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)