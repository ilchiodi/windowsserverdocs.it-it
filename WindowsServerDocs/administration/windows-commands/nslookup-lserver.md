---
title: nslookup lserver
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aee5ea0b-bb17-4c14-bde7-2f7a91f2f22b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2f2f787915f2b941d6c098d44de1bb0e04dbd491
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436908"
---
# <a name="nslookup-lserver"></a>nslookup lserver

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cambia il server predefinito per il dominio del sistema DNS (Domain Name) specificato.
## <a name="syntax"></a>Sintassi
```
lserver <DNSDomain> 
```
## <a name="parameters"></a>Parametri

|    Parametro    |                      Descrizione                      |
|-----------------|-------------------------------------------------------|
|   <DNSDomain>   | Specifica il nuovo dominio DNS per il server predefinito.  |
| {help &#124; ?} | Viene visualizzato un breve riepilogo di **nslookup** sottocomandi. |

## <a name="remarks"></a>Note
- Il **lserver** comando Usa il primo server per cercare le informazioni sul dominio DNS specificato. È in contrasto con la **server** comando, che usa il server predefinito corrente.
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
  [Chiave sintassi della riga di comando](command-line-syntax-key.md)
  [server nslookup](nslookup-server.md)
