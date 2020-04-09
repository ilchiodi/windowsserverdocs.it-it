---
title: reset
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: afbdab44-199c-4e11-884f-e96804965c21
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5c27ddd93d06670a30f797bd58dd396a9e7ce70a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835784"
---
# <a name="reset"></a>reset



Reimposta DiskShadow.exe allo stato predefinito. **Reimpostare** è particolarmente utile per la separazione operazioni DiskShadow composte, ad esempio **creare**, **importare**, **backup**, o **ripristinare**.

## <a name="syntax"></a>Sintassi

```
reset
```

## <a name="remarks"></a>Note

-   Quando si utilizza il **reimpostare** comando, si perde lo stato di comandi, ad esempio **aggiungere**, **impostare**, **carico**, o **writer**. **Reset** rilascia anche le interfacce IVssBackupComponent e perde le copie shadow non persistenti.

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)