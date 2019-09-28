---
title: nslookup server
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 608267f8-f7b4-412a-8dcd-e08b5ffc2085
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ce609f62f2d87024e1d75b43869b3b867e946e2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373063"
---
# <a name="nslookup-server"></a>nslookup server

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imposta il server predefinito sul dominio di Domain Name System (DNS) specificato.
## <a name="syntax"></a>Sintassi
```
server <DNSDomain>
```
## <a name="parameters"></a>Parametri

|    Parametro    |                          Descrizione                           |
|-----------------|----------------------------------------------------------------|
|   <DNSDomain>   | Obbligatorio. Specifica il nuovo dominio DNS per il server predefinito. |
| {Help &#124; ?} |     Viene visualizzato un breve riepilogo di **nslookup** sottocomandi.      |

## <a name="remarks"></a>Note
- Il comando **Server** usa il server predefinito corrente per cercare le informazioni sul dominio DNS specificato. Questo si differenzia dal comando **lserver** , che usa il server iniziale.
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
  [Chiave della sintassi della riga di comando](command-line-syntax-key.md)[nslookup lserver](nslookup-lserver.md) 
  
