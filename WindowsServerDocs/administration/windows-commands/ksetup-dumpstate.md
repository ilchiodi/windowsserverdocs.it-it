---
title: 'che Ksetup: dumpstate'
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3ef2f7b8-97af-4f42-9542-cff324840637
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 27a7e3154b9dfa663b88b04857ea7650995613c6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724647"
---
# <a name="ksetupdumpstate"></a>che Ksetup: dumpstate



Visualizza lo stato corrente delle impostazioni dell'area di autenticazione per tutte le aree di autenticazione definiti nel computer.

## <a name="syntax"></a>Sintassi

```
ksetup /dumpstate
```

#### <a name="parameters"></a>Parametri

nessuno

## <a name="remarks"></a>Osservazioni

L'output di questo comando include l'area di autenticazione predefinito (il dominio che il computer è un membro di) e tutte le aree di autenticazione definiti in questo computer. Di seguito è incluso per ogni area di autenticazione:
-   Tutte le chiave centri di distribuzione (KDC) che sono associati a questa area di autenticazione
-   Tutti i **realm set** flag per l'area di autenticazione
-   Password KDC

Questo comando non Visualizza il nome di dominio specificato dal rilevamento DNS o dal comando **che Ksetup/Domain**.

Questo comando consente di visualizzare la password del computer che viene impostata tramite il comando **che ksetup /setcomputerpassword**.

**Che Ksetup** produce lo stesso output **che ksetup /dumpstate**.

## <a name="examples"></a>Esempi

Trovare la maggior parte delle configurazioni dell'area di autenticazione Kerberos in un computer:
```
ksetup /dumpstate
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

-   [Che Ksetup](ksetup.md)
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)