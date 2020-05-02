---
title: nslookup server
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 608267f8-f7b4-412a-8dcd-e08b5ffc2085
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c9a3f4c7bdbcf8122bb532fafe83b400400d8d6b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723679"
---
# <a name="nslookup-server"></a>nslookup server

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imposta il server predefinito sul dominio di Domain Name System (DNS) specificato.
## <a name="syntax"></a>Sintassi
```
server <DNSDomain>
```
### <a name="parameters"></a>Parametri

|    Parametro    |                          Descrizione                           |
|-----------------|----------------------------------------------------------------|
|   <DNSDomain>   | Obbligatorio. Specifica il nuovo dominio DNS per il server predefinito. |
| {Help &#124;?} |     Viene visualizzato un breve riepilogo di **nslookup** sottocomandi.      |

## <a name="remarks"></a>Osservazioni
- Il comando **Server** usa il server predefinito corrente per cercare le informazioni sul dominio DNS specificato. Questo si differenzia dal comando **lserver** , che usa il server iniziale.
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
  - [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
  [nslookup lserver](nslookup-lserver.md)
