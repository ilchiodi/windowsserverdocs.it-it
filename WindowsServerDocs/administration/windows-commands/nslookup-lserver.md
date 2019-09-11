---
title: nslookup lserver
description: 'Argomento dei comandi di Windows per * * * *- '
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
ms.openlocfilehash: 30c5ba8b7fef9b09d854aca998948f7891d99a02
ms.sourcegitcommit: ee8e0b217be6f6b2532ee7265fb4be00c106e124
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70878125"
---
# <a name="nslookup-lserver"></a>nslookup lserver

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imposta il server predefinito sul dominio di Domain Name System (DNS) specificato.
## <a name="syntax"></a>Sintassi
```
lserver <DNSDomain> 
```
## <a name="parameters"></a>Parametri

|    Parametro    |                      Descrizione                      |
|-----------------|-------------------------------------------------------|
|   <DNSDomain>   | Specifica il nuovo dominio DNS per il server predefinito.  |
| {Help &#124; ?} | Viene visualizzato un breve riepilogo di **nslookup** sottocomandi. |

## <a name="remarks"></a>Note
- Il comando **lserver** usa il server iniziale per cercare le informazioni sul dominio DNS specificato. Questo si differenzia dal comando **Server** , che usa il server predefinito corrente.
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
  [Sintassi della riga di comando chiave](command-line-syntax-key.md)
  [nslookup server](nslookup-server.md)
