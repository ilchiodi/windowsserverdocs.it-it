---
title: nslookup server
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 608267f8-f7b4-412a-8dcd-e08b5ffc2085
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8de2d8a801d57a9197d5e8ec7d3b6adb2d00dd04
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838684"
---
# <a name="nslookup-server"></a>nslookup server

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imposta il server predefinito sul dominio di Domain Name System (DNS) specificato.
## <a name="syntax"></a>Sintassi
```
server <DNSDomain>
```
### <a name="parameters"></a>Parametri

|    Parametro    |                          Descrizione                           |
|-----------------|----------------------------------------------------------------|
|   <DNSDomain>   | Obbligatoria. Specifica il nuovo dominio DNS per il server predefinito. |
| {Help &#124; ?} |     Viene visualizzato un breve riepilogo di **nslookup** sottocomandi.      |

## <a name="remarks"></a>Note
- Il comando **Server** usa il server predefinito corrente per cercare le informazioni sul dominio DNS specificato. Questo si differenzia dal comando **lserver** , che usa il server iniziale.
  ## <a name="additional-references"></a>Altre informazioni di riferimento
  - [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
  [nslookup lserver](nslookup-lserver.md)
