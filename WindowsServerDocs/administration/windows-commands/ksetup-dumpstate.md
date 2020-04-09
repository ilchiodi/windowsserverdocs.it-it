---
title: 'che Ksetup: dumpstate'
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3ef2f7b8-97af-4f42-9542-cff324840637
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 46f827d26d867392db4cbef92cf5be496aee8d74
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841514"
---
# <a name="ksetupdumpstate"></a>che Ksetup: dumpstate



Visualizza lo stato corrente delle impostazioni dell'area di autenticazione per tutte le aree di autenticazione definiti nel computer. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
ksetup /dumpstate
```

#### <a name="parameters"></a>Parametri

None

## <a name="remarks"></a>Note

L'output di questo comando include l'area di autenticazione predefinito (il dominio che il computer è un membro di) e tutte le aree di autenticazione definiti in questo computer. Di seguito è incluso per ogni area di autenticazione:
-   Tutte le chiave centri di distribuzione (KDC) che sono associati a questa area di autenticazione
-   Tutti i **realm set** flag per l'area di autenticazione
-   Password KDC

Questo comando non Visualizza il nome di dominio specificato dal rilevamento DNS o dal comando **che Ksetup/Domain**.

Questo comando consente di visualizzare la password del computer che viene impostata tramite il comando **che ksetup /setcomputerpassword**.

**Che Ksetup** produce lo stesso output **che ksetup /dumpstate**.

## <a name="examples"></a><a name=BKMK_Examples></a>Esempi

Trovare la maggior parte delle configurazioni dell'area di autenticazione Kerberos in un computer:
```
ksetup /dumpstate
```

## <a name="additional-references"></a>Altre informazioni di riferimento

-   [Che Ksetup](ksetup.md)
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)