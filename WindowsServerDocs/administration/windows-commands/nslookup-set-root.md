---
title: nslookup set root
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8ad5393c-d4fd-4594-8187-576b1dcde60a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ea2c34bbf7c9323c948d57ac2a838c22aea1008e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838314"
---
# <a name="nslookup-set-root"></a>nslookup set root

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modifica il nome del server principale per le query.
## <a name="syntax"></a>Sintassi
```
set root=<RootServer>
```
### <a name="parameters"></a>Parametri

|    Parametro    |                                   Descrizione                                    |
|-----------------|----------------------------------------------------------------------------------|
|  <RootServer>   | Specifica il nuovo nome per il server radice. Il valore predefinito è ns.nic.ddn.mil. |
| {Help &#124; ?} |              Viene visualizzato un breve riepilogo di **nslookup** sottocomandi.               |

## <a name="remarks"></a>Note
- Il sottocomando **set root** influiscono sul sottocomando **radice** .
  ## <a name="additional-references"></a>Altre informazioni di riferimento
  - [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
  [radice nslookup](nslookup-root.md)
