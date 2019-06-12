---
title: nslookup set root
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 38cd5a2e9878a8e43393befc5cbd4fc47c65ec53
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436599"
---
# <a name="nslookup-set-root"></a>nslookup set root

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifica il nome del server principale usato per le query.
## <a name="syntax"></a>Sintassi
```
set root=<RootServer>
```
## <a name="parameters"></a>Parametri

|    Parametro    |                                   Descrizione                                    |
|-----------------|----------------------------------------------------------------------------------|
|  <RootServer>   | Specifica il nuovo nome per il server principale. Il valore predefinito è DDN.mil. |
| {help &#124; ?} |              Viene visualizzato un breve riepilogo di **nslookup** sottocomandi.               |

## <a name="remarks"></a>Note
- Il **radice del set** sottocomando interessa le **radice** sottocomando.
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
  [Chiave sintassi della riga di comando](command-line-syntax-key.md)
  [nslookup radice](nslookup-root.md)
