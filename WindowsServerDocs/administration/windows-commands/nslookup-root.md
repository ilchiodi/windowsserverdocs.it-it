---
title: nslookup root
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9c29edc3-ec49-43f2-bc49-86bf0612d816
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bdfbe40443cf8f2fec2f81608bb93603cd74937f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723696"
---
# <a name="nslookup-root"></a>nslookup root

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

imposta il server predefinito sul server per la radice dello spazio dei nomi di dominio di Domain Name System (DNS).
## <a name="syntax"></a>Sintassi
```
root 
```
### <a name="parameters"></a>Parametri

|    Parametro    |                      Descrizione                      |
|-----------------|-------------------------------------------------------|
| {Help &#124;?} | Viene visualizzato un breve riepilogo di **nslookup** sottocomandi. |

## <a name="remarks"></a>Osservazioni
- Attualmente, viene usato il server dei nomi ns.nic.ddn.mil. Questo comando è un sinonimo di lserver ns.nic.ddn.mil. È possibile modificare il nome del server radice con il comando **Imposta radice** .
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
  - [Sintassi della riga di comando chiave](command-line-syntax-key.md)
  [nslookup set root](nslookup-set-root.md)
