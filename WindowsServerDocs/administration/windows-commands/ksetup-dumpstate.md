---
title: 'che Ksetup: dumpstate'
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3ef2f7b8-97af-4f42-9542-cff324840637
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 625d05b2fea9ae58681648c64e309aa8b2a201ed
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375004"
---
# <a name="ksetupdumpstate"></a>che Ksetup: dumpstate



Visualizza lo stato corrente delle impostazioni dell'area di autenticazione per tutte le aree di autenticazione definiti nel computer. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
ksetup /dumpstate
```

### <a name="parameters"></a>Parametri

Nessuno

## <a name="remarks"></a>Note

L'output di questo comando include l'area di autenticazione predefinito (il dominio che il computer è un membro di) e tutte le aree di autenticazione definiti in questo computer. Di seguito è incluso per ogni area di autenticazione:
-   Tutte le chiave centri di distribuzione (KDC) che sono associati a questa area di autenticazione
-   Tutti i **realm set** flag per l'area di autenticazione
-   Password KDC

Questo comando non Visualizza il nome di dominio specificato dal rilevamento DNS o dal comando **che Ksetup/Domain**.

Questo comando consente di visualizzare la password del computer che viene impostata tramite il comando **che ksetup /setcomputerpassword**.

**Che Ksetup** produce lo stesso output **che ksetup /dumpstate**.

## <a name="BKMK_Examples"></a>Esempi

Trovare la maggior parte delle configurazioni dell'area di autenticazione Kerberos in un computer:
```
ksetup /dumpstate
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Che Ksetup](ksetup.md)
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)