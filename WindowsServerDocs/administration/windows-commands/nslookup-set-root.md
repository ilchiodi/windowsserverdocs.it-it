---
title: nslookup set root
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8ad5393c-d4fd-4594-8187-576b1dcde60a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 08cf41ec9b6ac30699013112216a538dcf625fd5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372842"
---
# <a name="nslookup-set-root"></a>nslookup set root

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modifica il nome del server radice utilizzato per le query.
## <a name="syntax"></a>Sintassi
```
set root=<RootServer>
```
## <a name="parameters"></a>Parametri

|    Parametro    |                                   Descrizione                                    |
|-----------------|----------------------------------------------------------------------------------|
|  <RootServer>   | Specifica il nuovo nome per il server radice. Il valore predefinito è ns.nic.ddn.mil. |
| {Help &#124; ?} |              Viene visualizzato un breve riepilogo di **nslookup** sottocomandi.               |

## <a name="remarks"></a>Note
- Il sottocomando **set root** influiscono sul sottocomando **radice** .
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
  [Sintassi della riga di comando chiave](command-line-syntax-key.md)
  [nslookup radice](nslookup-root.md)
