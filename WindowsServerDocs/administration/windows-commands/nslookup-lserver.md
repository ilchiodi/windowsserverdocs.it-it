---
title: nslookup lserver
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aee5ea0b-bb17-4c14-bde7-2f7a91f2f22b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b2054c0fd427b41e7d6076258b29ab78d0fb7892
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723670"
---
# <a name="nslookup-lserver"></a>nslookup lserver

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imposta il server predefinito sul dominio di Domain Name System (DNS) specificato.
## <a name="syntax"></a>Sintassi
```
lserver <DNSDomain> 
```
### <a name="parameters"></a>Parametri

|    Parametro    |                      Descrizione                      |
|-----------------|-------------------------------------------------------|
|   <DNSDomain>   | Specifica il nuovo dominio DNS per il server predefinito.  |
| {Help &#124;?} | Viene visualizzato un breve riepilogo di **nslookup** sottocomandi. |

## <a name="remarks"></a>Osservazioni
- Il comando **lserver** usa il server iniziale per cercare le informazioni sul dominio DNS specificato. Questo si differenzia dal comando **Server** , che usa il server predefinito corrente.
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
  - [Sintassi della riga di comando chiave](command-line-syntax-key.md)
  [nslookup server](nslookup-server.md)
