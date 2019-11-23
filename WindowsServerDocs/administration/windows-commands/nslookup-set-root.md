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
ms.openlocfilehash: 5a1737275bf6321525bbba56cd4d6a77ef973423
ms.sourcegitcommit: 9a6a692a7b2a93f52bb9e2de549753e81d758d28
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/18/2019
ms.locfileid: "72591020"
---
# <a name="nslookup-set-root"></a>nslookup set root

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modifica il nome del server principale per le query.
## <a name="syntax"></a>Sintassi
```
set root=<RootServer>
```
## <a name="parameters"></a>Parametri

|    Parametro    |                                   Descrizione                                    |
|-----------------|----------------------------------------------------------------------------------|
|  <RootServer>   | Specifica il nuovo nome per il server radice. Il valore predefinito è ns.nic.ddn.mil. |
| {Help &#124; ?} |              Viene visualizzato un breve riepilogo di **nslookup** sottocomandi.               |

## <a name="remarks"></a>Osservazioni
- Il sottocomando **set root** influiscono sul sottocomando **radice** .
  ## <a name="additional-references"></a>riferimenti aggiuntivi
  [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
  [radice nslookup](nslookup-root.md)
