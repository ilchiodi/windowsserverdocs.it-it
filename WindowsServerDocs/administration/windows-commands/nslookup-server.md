---
title: nslookup server
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: e24e55026d12a0d8afc5b6f1bef926ece9087bd0
ms.sourcegitcommit: 6423dfa9cecb3b06bdd563cae113c3e80a4ec330
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/18/2019
ms.locfileid: "71105016"
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
|   <DNSDomain>   | Richiesto. Specifica il nuovo dominio DNS per il server predefinito. |
| {Help &#124; ?} |     Viene visualizzato un breve riepilogo di **nslookup** sottocomandi.      |

## <a name="remarks"></a>Note
- Il comando **Server** usa il server predefinito corrente per cercare le informazioni sul dominio DNS specificato. Questo si differenzia dal comando **lserver** , che usa il server iniziale.
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
  [Chiave della sintassi della riga di comando](command-line-syntax-key.md)[nslookup lserver](nslookup-lserver.md) 
  
