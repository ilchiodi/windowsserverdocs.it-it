---
title: ksetup:dumpstate
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 8e5e8f20188fc27cc08dfd37c5fdbd811925f476
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863122"
---
# <a name="ksetupdumpstate"></a>ksetup:dumpstate



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
-   La password KDC

Questo comando consente di visualizzare il nome di dominio specificato dal rilevamento automatico DNS o tramite il comando **che ksetup /domain**.

Questo comando consente di visualizzare la password del computer che viene impostata tramite il comando **che ksetup /setcomputerpassword**.

**Che Ksetup** produce lo stesso output **che ksetup /dumpstate**.

## <a name="BKMK_Examples"></a>Esempi

Trovare la maggior parte delle configurazioni dell'area di autenticazione Kerberos in un computer:
```
ksetup /dumpstate
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Ksetup](ksetup.md)
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)